#  RFC|BB|L12-01: Bitswap/Graphsync exchange messages extension and transmission choice
* Status: `Draft`
* Implementation here: https://github.com/

## Abstract
This RFC proposes expanding Bitswap and Graphsync exchange messages with additional information. This information is used in content requests for receivers to be able to understand clearly the content requested; and in responses so responders can share his specific level of fulfillment of the request to the requestor. With this information the requestor can select the best nodes to perform the actual request to have the best performance possible in the transmission.


<!-- Full description here: https://docs.google.com/document/d/1zjJCZel8zJzgK3XuHK0YZlNffEHThq7tUOssGgRTryY/edit#heading=h.6qnrq913vou6 -->


## Shortcomings

Bitswap and Graphsync’s current discovery process is blind and optimistic. An IPLD selector or a plain WANT list with the list of request CIDs are shared with connected peers hoping that someone will have the content. When a peer answers saying that it has the requested block, a subsequent request needs to be performed to get the rest of the blocks belonging to the DAG structure of the requested CID. The idea behind this RFC is to add a way for requestor and connected peers to give more directed feedback about the result of the request.

## Description
To request content to the network, instead of sending plain WANT messages or an IPLD selector, the requests will include the following information:

-   Plain legacy request (want list or IPLD selector). This would allow this RFC to be backward compatible with existing exchange interfaces.

-   Parameters for the exchange protocol (such as "send blocks directly if you have them", or "send only leaf blocks", "send all the DAG structure for the root CIDs I send", or any other extension we may come up with).

-   Specific requirements (such as the minimum latency of the bandwidth desired for the exchange).

-   Any additional data that may be useful and that we can act upon at a protocol level.

Nodes receiving this message will respond with the level of fulfillment of the request (number/range  of blocks belonging to the request that the node stores , and if they fulfill or not the specified transmission requirements). This request can also include the list of blocks under the CID/IPLD select the request will eventually look for. No blocks are shared (except explicitly specified) in this exchange, it is only used as a way of  "polling the surroundings" for the content.

With this information, the requestor inspects the characteristics and percentage of fulfillment of all the responses and chooses the best peers to request the blocks from distributing the load depending on the nodes it is connected to, and to parallelize as much as possible the exchange. This offers peers an opportunity to try and find  the optimal distribution of requests for blocks that maximizes the output. The transmission flow with the chosen peers is triggered through a TRANSFER message, where the desired blocks and the transmission parameters are specified (this opens the door to the use of compression, network coding and other schemes in the transmission phase).

While the requester is receiving blocks through different transmission flows, it can trigger new rounds of discovery sending additional request messages to connected peers or selected peers in the DHT to increase the overall level of fulfilment or find better transmission candidates. The discovery and transmission loop will be permanently communicating.

### Implementation

Nodes receiving this manifest will answer with the level of fulfillment of the request. Upon reception of these responses, the node can start transmission requests to all the desired nodes. Meanwhile, we can resort to the DHT to send these exchange requests to peers we are not directly connected to. The flow of the protocol would be:

-   Send exchange requests (IPLD selector/list of blocks, network conditions, node conditions) to connected peers.

-   Receive responses: R1=50% fulfillment; R2=30% fulfillment; R3=5% fulfillment; We select the peers that lead to a larger level of fulfillment U(R1, R2)=75% fulfillment, and request the start of a transmission flow with them. Meanwhile, we resort to the DHT or perform an additional lookup to find the data pending for full fulfillment of the request. All of these phases should be in constant contact, so in case we receive better responses from peers we can act upon start new transmission or adapt to the conditions of the network.

The above proposal may present a few shortcomings for which we would have to include schemes to prevent such as:

-   Reducing the number of RTTs when the number of blocks requested and their size is small. We need to include a way of merging the discovery and transmission phases to minimize the RTTs when appropriate.

-   For large files send only the first 2 layers in the response before the requestor triggers the transmission phase.

-   Use of accumulators in the level of fulfilment in responses to improve checks and the time between request and transmission phase.

-   Avoid response forgery. This is out of the scope of this RFC but is something worth exploring in the future work.

## Implementation plan
- [ ] Include additional information for exchange requests in WANT messages.

- [ ]  Determine the basic structure of exchange requests, the information included in it, and how it will be leveraged by nodes. When designing these messages we need to ensure that it is compatible with existing WANT messages for backward compatibility. Thus, if an outdated Bitswap node receives an exchange request it still knows how to interpret the request. Along with this exchange request, the TRANSFER message should be designed.

    - [ ]  Use of Graphsync selectors in WANT messages.

- [ ]  Design and implement the message exchange protocol for the content discovery and negotiation phases:

    - [ ]  1\. Send exchange requests and collect responses for content availability and network status.

    - [ ]  2\. Start transmission channels (TRANSFER) with peers fulfilling the request and keep the content discovery loop open in case better content servers appear (either because they are found through the exchange request broadcast, or because we chose to extend the lookup through the DHT and found better peers).

    - [ ]  3\. Fine-tune peer interaction for best performance.

- [ ]  Performance benchmark with plain Bitswap to fine-tune protocol configuration.

- [ ]  Implement more complex queries in request messages.

    - [ ]  Use a utility function / score to evaluate the "best peers" for content discovery and transmission.

# Impact
Adding these exchange request and negotiation phases opens the door to the clear differentiation of content discovery and transmission. This enables the inclusion of new schemes to optimize both levels according to the needs of an application. It will also enable the parallelization of many processes in current exchange interfaces. It also enables a way for clients to influence the operation of the protocol. 


## Evaluation Plan
- [The IPFS File Transfer benchmarks.](https://docs.google.com/document/d/1LYs3WDCwpkrBdfrnB_LE0xsxdMCIhXdCchIkbzZc8OE/edit#heading=h.nxkc23tlbqhl)

## Prior Work

## Results


## Future Work
