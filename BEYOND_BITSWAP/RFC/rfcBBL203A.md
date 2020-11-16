#  RFC|BB|L2-03A: Use of compression and adjustable block size
* Status: `Prototype`
* Implementation here: https://github.com/adlrocha/go-bitswap/tree/feature/rfcBBL203A
* Compression in libp2p: https://github.com/adlrocha/go-libp2p-compression-examples

## Abstract
This RFC proposes the exploration of using compression in block transmission. These techniques go from:
Block by block standard compression (e.g. gzip)
Whole transfer compression (e.g. when responding to a graphsync query, send all the blocks compressed)
Custom coding tables for sequences of bytes that appeared often (e.g. generate an Huffman table for all the protobuf headings so that these are compressed by default, like hpack does for http)

Additionally, to optimize the use of these schemes, a system of adjustable block sizes and coding strategies in transmission could be devised (e.g. dynamic Huffman tables).


<!-- Full description here: https://docs.google.com/document/d/1zjJCZel8zJzgK3XuHK0YZlNffEHThq7tUOssGgRTryY/edit#heading=h.6qnrq913vou6 -->


## Shortcomings
Blocks in IPFS are exchanged without the use of compression, this is a huge opportunity loss to minimize the bandwidth footprint and latency of transferring a file. For context, even minimal web assets are transmitted compressed through HTTP to increase website loading performance, most of them are below 256KiB, which is IPFS default block size. We expect to see several gains in transmission times.

## Description
Current implementation of file-sharing protocols may benefit from the use of on-the-fly compression to optimize the use of bandwidth and optimize the transmission of content.
Even more, when using the “Graphsynced” approach in the discovery of content, where we request peers for the level of fulfillment of an IPLD selector, we can request all the blocks for the IPLD selector to be compressed in the same package and forwarded to the requestor.

Some of the compression approaches to be explored in this RFC are:
* Block by block standard compression (e.g. gzip): Every block (and optionally every single Bitswap message) is compressed. Get inspiration from web compression.
* Whole transfer compression: All the blocks requested by a peer in a Wantlist or a graphsync IPLD selector are compressed in the same package.
* Custom coding tables for sequences of bytes that appeared often (e.g. generate a Huffman table for all the protobuf headings so that these are compressed by default, like hpack does for http).
* Use of “compressed caches” so that when a specific content has been identified as “regularly exchanged”, instead of having to compress it again it can be retrieved from the cache. This scheme may not be trivial.
* Use of different compression algorithms.
* Use of different block sizes before compression.

## Implementation plan
-  [x] Perform a simple test to evaluate the benefits of "on-the-fly" compression on blocks (to determine if IPFS could benefit from directly exchanging compressed messages and blocks). Evaluate different compression algorithms used in the web.

    -  [x] Evaluate the compression of full Bitswap messages (`bs.compressionStrategy = "full"`): To achieve this we add a compression flag in [Bitswap messages](https://github.com/adlrocha/go-bitswap/blob/master/message/message.go) to be able to identify when messages are compressed. Afterwards, if compression is enabled we need to [compress the message](https://github.com/adlrocha/go-bitswap/blob/d151875a94048c3db59de52b9cb99d0246d74613/network/ipfs_impl.go#L240) before sending it. Compressed messages are identified in the [newMessageFromProto](https://github.com/adlrocha/go-bitswap/blob/d151875a94048c3db59de52b9cb99d0246d74613/message/message.go#L199) of receiving peers, they are uncompressed and processed seamlessly by Bitswap. In order to open the door to the use different compression algorithms and different full-message compression strategies a compressionType has been added to [message.proto](https://github.com/adlrocha/go-bitswap/blob/master/message/pb/message.proto).

    - [x] Evaluate the compression of blocks only (`bs.compressionStrategy = "blocks"`): We compress each block before adding it to the protobuf message in [ToProtoV1](https://github.com/adlrocha/go-bitswap/blob/d151875a94048c3db59de52b9cb99d0246d74613/message/message.go#L583) function of message.go, and then uncompress them in [newMessageFromProto](https://github.com/adlrocha/go-bitswap/blob/d151875a94048c3db59de52b9cb99d0246d74613/message/message.go#L199). For the compression of blocks, the only thing that is changed for the transmission of the block is the RawData, the CID is kept without change so the block is conveniently identified.

    -  [x] Use [GZip](https://golang.org/pkg/compress/gzip/) as the default compression algorithm (`engine.compressor = "Gzip"`).

    - [x] Instead of compressing fields of the protobuf message, evaluate the compression of the full stream in the [bitswap network](https://github.com/adlrocha/go-bitswap/blob/d151875a94048c3db59de52b9cb99d0246d74613/network/ipfs_impl.go).
        * We may choose to use a multicodec to signal that a stream is compressed and evaluate the fact that instead of using a prefix to signal the size of sent messages, in order to be able to leverage streams, use multicodec and `KeepReading` and `EndOfStream` signals in protocol streams so there is no need to know the size of the compressed message beforehand.

    -  [ ] Evaluate other compression algorithms (Brotli and gzip are the best alternative, but in case we want to test with other algorithms):

        - [ ] [ZDAG](https://github.com/mikeal/zdag) Blocks

        <!-- - [ ] [Bzip2 compression](https://golang.org/pkg/compress/bzip2/) 

        - [ ]  [XZ compression](https://github.com/ulikunitz/xzhttps://github.com/ulikunitz/xz) -->

        - [ ]  Brotli compression (No Golang implementation, [incomplete implementation](https://github.com/dsnet/compress))

    - [ ]  Compare computational footprint of above implementations

- [ ]  Design and evaluate a simple scheme to "gather" WANT requests and create fully compiled/network coded responses to the requestor. This involves several tasks

    -  [ ] A way of matching several blocks with a request (graphsync, wantlist).

    - [ ]  Perform the compression operation. This may be computationally expensive and would need some time, and depending on the data, when the prepared response is ready to send, the requestor may have already received all the blocks conforming his desired content.

-  [ ] Evaluate how the use of different block sizes and compression may benefit performance.

-  [ ] Include a "compression parameter" in exchange requests to signal peers that you want blocks to be transmitted using a specific compression technique.

-  [ ] From the preliminary tests performed after the above implementations, evaluate if the protocol could benefit from the use of Huffman tables and compressed caches.

## Implementation details

* Block compression: Files within Bitswap are exchanged in the form of blocks. Files are composed of several blocks organized in a DAG structure (with each block having a size limit of 256KB). In this compression approach, we compress blocks before including them in a message and transmitting them to the network.
* Full message compression: In this compression strategy instead of only compressing blocks we compress every single message before sending it. It is the equivalent of compressing header+body in HTTP.
* Stream compression: It uses compression at a stream level, so every byte that enters a stream from the node to other peers is compressed (i.e. using a compressed writer).

* To drive the compression idea even further, we prototyped a `Compression` transport into libp2p (between the `Muxer` and the `Security` layer) so that every stream running over a libp2p node can potentially benefit from the use of compression. This is a non-breaking change as the `transport-upgrader` has also been updated to enable compression negotiation (so eventually anyone can come with their own compression and embed it into libp2p seamlessly). Some repos to get started with compression in libp2p:
     - Compression example: https://github.com/adlrocha/go-libp2p-compression-examples
     - Gzip compressor: https://github.com/adlrocha/go-libp2p-gzip
     - Testbed to test compression over IPFS: https://github.com/adlrocha/beyond-bitswap/tree/feat/compression
     
[See a discussion on the results here](https://github.com/protocol/ResNetLab/issues/5).


# Impact
A reduction of latency due to compressed transmissions. Potential increase in computational overhead.

## Evaluation Plan
-   [The IPFS File Transfer benchmarks.](https://docs.google.com/document/d/1LYs3WDCwpkrBdfrnB_LE0xsxdMCIhXdCchIkbzZc8OE/edit#heading=h.nxkc23tlbqhl)

-   See the computational footprint of different compression strategies and algorithms.

-   Compare the data sent and received using compression and with baseline Bitswap.

## Prior Work

This RFC takes inspiration from:
* [Dropbox’s work on compression](https://dropbox.tech/infrastructure/-broccoli--syncing-faster-by-syncing-less)
* [HPACK](https://blog.cloudflare.com/hpack-the-silent-killer-feature-of-http-2/)
* [HTTP Compression](https://developer.mozilla.org/en-US/docs/Web/HTTP/Compression)
* [Choose best compression algorithm assisted by AI](https://vks.ai/2019-12-05-shrynk-using-machine-learning-to-learn-how-to-compress)
* [Thorough benchmark of different GZip modes](https://www.rootusers.com/gzip-vs-bzip2-vs-xz-performance-comparison/)


## Results
The results for the implementation of this RFC were reported here: https://research.protocol.ai/blog/2020/honey-i-shrunk-our-libp2p-streams/ 

## Future Work
-   If the use of exchange requests and the negotiation phase for content transmission (RFC | BB | L1/2-01) is implemented, it makes sense that once identified a specific peer (or a group of them) as the ones storing a large number of the desired blocks, to request more advanced compression and network coding techniques for their transmission.

- Detect the type of data being exchanged in blocks and apply the most suitable compression for the data type, such as [image-specific compression ](https://developers.google.com/speed/webp/docs/compression)if images are being exchanged (for this approach, a node will need to have all the blocks for the data).
