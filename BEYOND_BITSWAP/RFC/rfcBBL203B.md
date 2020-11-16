#  RFC|BB|L2-03B: Use of network coding and erasure codes.
* Status: `Brainstorm`

### Abstract

This RFC proposes the exploration of applying network coding and erasure codes to the content exchanged by peers. These techniques go from:
-   The use of erasure codes in the transmission of blocks so they can be requested from different sources, and the original content can be regenerated even without the reception of all the blocks.
-   The use of rateless codes to make all blocks for a specific content equally valuable.
-   The use of erasure codes for storage (such as Reed Solomon).

These techniques could lead to additional improvements by including a negotiation phase in the exchange interface (see [RFC|BB|L1/2-01](./rfcBBL1201)).

### Shortcomings

In order to recover the content requested, peers need to receive every block of the content's DAG. This means that if just a single block is lost, is too rare, or it is not in the network anymore, it can lead to increased transmission times or in the worst case making the content "unretrievable". The use of erasure coding and network coding can benefit the discovery and transmission of blocks (especially if they are rare), making the content exchange more resilient to unforeseen events. These techniques also improve the transmission of content from several sources.

This RFC becomes really interesting in networks with high churn and large files. The aim is to parallelize the transmission from different sources.

### Description

Several nodes may receive complementary WANT messages from different connected peers. Instead of requesting the content from just one source, or explicitly requesting it from all of them potentially producing duplicates in the network, we could benefit from the use of network coding to enhance the transmission from the multiple sources.

We can really benefit from the fact that more than one peer may store the content exploring the use of techniques such as:

-   The use of erasure codes and network coding in the transmission of blocks so they can be requested from different sources and the original content can be regenerated even without the reception of all the blocks. Peers can send a linear combination of coded blocks so that the requestor is able to recover the content even if it doesn't receive all the original blocks. This can lead to improvements in transmission and the removal of duplicates in the network (the redundancy and linear combination used in block transmission can be related to the amount of duplicates and the split factor used by sessions).

-   The use of rateless codes to make all blocks for a specific content equally valuable. If several sources serve the content coded using rateless code, every block is equally valuable, and as long as a minimum number of them are received, the content can be recovered.

-   The use of erasure codes for storage (such as Reed Solomon). It adds a storage overhead but allows to regenerate the original content even if all the blocks are not retrieved. The proposal is to store blocks using their original CID (so their identifier doesn't change) but use Reed Solomon to code the content. This would increase the size of blocks, and poses several limitation on the codes to use to generate the Reed Solomon redundancy. 

Using the aforementioned techniques, several seeders fulfilling the request for content would be able to encode blocks and stream them so peers can receive blocks from different sources and reconstruct the original content once a minimum number of blocks have been received. This is a good way of parallelizing the transmission of blocks from different sources before [RFC|BB|L1/2-01](./rfcBBL1201). A problem to be solved to implement this RFC is how to orchestrate peer serving the request (the linear coding applied to the content needs to be deterministic). With RFC | BB | L1/2-01 more complex requests for blocks could be performed.

### Prior Work

This RFC takes inspiration from:

-   [This paper](https://www.mdpi.com/2076-3417/10/7/2206/htm)

-   Rateless coding. Check [this document](https://docs.google.com/document/d/1PdfuPZs5ti7u67R9p4lZl_JFBzk477CjmruiWbLQr4U/edit#heading=h.lrqjoh4tz0t6) and [Petar's paper](http://www.scs.stanford.edu/~dm/home/papers/maymounkov:rateless.pdf) for inspiration.

-   HackFS project on [Reed Solomon over IPFS](https://github.com/Wondertan/go-ipfs-recovery).

### Implementation Plan

- [ ] Evaluate potential improvements and overhead of using [IPFS Recovery](https://github.com/Wondertan/go-ipfs-recovery).

- [ ] Evaluate the use of rateless coding (or alternatives not IP protected). With rateless codes we can generate check blocks from the content desired and requested from different nodes so that as long as we receive a minimum number of them we can generate the original information. This could potentially remove duplicates blocks.

- [ ] If [RFC|BB|L1/2-01](./rfcBBL1201) ends up being implemented, more complex ideas could be evaluated at this end. Discovery and transmission would be two distinct stages, so nodes could eagerly request a compressed or networked coded transmission from a set of nodes.

### Impact

Improved transmission leveraging multiple streams, more reliable exchanges, and potential removal of duplicates in the network.

### Evaluation Plan

-   [The IPFS File Transfer benchmarks.](https://docs.google.com/document/d/1LYs3WDCwpkrBdfrnB_LE0xsxdMCIhXdCchIkbzZc8OE/edit#heading=h.nxkc23tlbqhl)

-   Test case where there are several seeders with the same content and leechers are connected to several of them.

### Future Work

If the negotiation phase from [RFC|BB|L1/2-01](./rfcBBL1201) is implemented, additional communications between seeders and leechers could be performed to enhance the use of these techniques. Thus, if a peer receives an overlapping level of fulfilment for its request from different sources, it can trigger the use of network coding and rateless codes so that, with a minimum number of blocks from both of the sources, the requested content can be retrieved.

Additionally, the use of "in-path" coding could be devised as future work, where intermediate nodes in a path upon the reception of several blocks for which fulfill the same request from different sources combine them to enhance the transmission (this requires further exploration). The impact of this improvement would significantly benefit [RFC|BB|L102](./rfcBBL102), where nodes can trigger relay session to request blocks on behalf of other nodes. 