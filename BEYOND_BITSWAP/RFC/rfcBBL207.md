#  RFC|BB|L2-07: Request minimum piece size and content protocol extension
* Status: `Brainstorm`
* Implementation here: https://github.com/

<!-- Full description here: https://docs.google.com/document/d/1zjJCZel8zJzgK3XuHK0YZlNffEHThq7tUOssGgRTryY/edit#heading=h.6qnrq913vou6 -->

## Abstract
This RFC introduces the concept of pieces as an indivisible aggregation of blocks. In order not to modify the underlying chunking and storage of blocks by IPFS, we propose the use of pieces as an aggregation of blocks. Pieces will have a unique identifier independent from those of their underlying blocks so that they can be accessed independently. Thus, if a piece is requested, only nodes having all of the blocks of the piece are allowed to perform the transmission.


## Shortcomings
IPFS block size of 256 KB favors deduplication but can harm the efficiency of content discovery and transmission. Evaluating different block sizes or the use of dynamic block sizes could benefit the performance of file-sharing in the network.


## Description

Wanlist messages could include a "minimum piece size" parameter, specifying that only peers with enough blocks to fill a full piece should be allowed to answer with a HAVE message. Blocks are still stored using the same size, but this allows us to identify nodes that will serve us at least a certain amount of blocks. This can become really interesting once RFC | BB | L1/2-01 is implemented and in combination with RFC | BB | L2-03 so we can choose the piece size, and in accordance with this, select the best compression or network coding algorithm to be used in the transmission.

We can introduce the concept of piece as an irreducible structure where underlying blocks for a piece are not assigned a CID so the piece is only identified by the root CID of the structure.

-   `/ipfs/<CID>`: Every block is identified by a CID.

-   `/ipfs/pc/<multi_hash>`: Identifies a piece. Pieces can be seen as irreducible groups of blocks. Some nodes may store some of the blocks conforming the piece, and they can be requested directly through their CID. However, if a /ipfs/pc/<multi_hash> request we are signalling that only nodes storing the full piece should send the data. For large pieces this could lead for content to become rare in the network. Fortunately, you can always resort to finding blocks one by one through /ipfs/<cid> identifier. Hence:

    -   Add `/ipfs/pc/` content chunks data in pieces of the specific size, and chunks these pieces in different blocks. The add request sent to the network is /ipfs/pc/<multi_hash> for every piece, indicating that all the blocks conforming a piece should be stored in the same node. Others can store sub-blocks for the piece, but add operations for pieces are performed so that all the blocks are stored as a single data unit.

    -   Requesting `/ipfs/pc/<multi_hash>` finds full pieces before finding individual blocks pending.

    -   If no one has the full piece, the node needs to increase the granularity of the request and search by blocks. This adds an additional layer of abstraction for content identification and discovery.

This scheme would allow us to represent larger blocks of content without removing the benefits of a 256KB block.

Linking with this idea, we could think of a multiblock system analogous to libp2p's multistream, where anyone could implement his own DAG/Block structure while the underlying structure used (CIDs, 4KB blocks) is maintained, the same way multistreams are built over TCP/UDP/QUIC or existing protocols. Hosts already know how to talk TCP, and they build protocols over it, here IPFS nodes know how to talk 4KB blocks and IPLD, but we allow the implementation of new schemes over this foundation.

## Implementation plan
TBD

# Impact

## Evaluation Plan

## Prior Work


## Results


## Future Work
