# RIW2021 | RFC: Omniscient Routers

_Status:_ draft

_Area of Improvement:_ Distribution Graph Forming

_Estimated Effort Needed:_ <?>

_Prerequisite(s):_ <?>

_Priority:_ <? P0, P1, P2>

### Abstract

The core goal of this design is to give clients knowledge about where the hot copies of the content they are looking for are stored. The RFC builds on the premise that the entire content catalogue of provider records of the 3DM ecosystem will be in the order of TBs, hence, routers will be capable of storing it in its entirety. Given this assumption, the RFC explores mechanisms to support and optimise the distribution of record updates, as well as how the network forms.

### Proposal/Construction

We consider an architecture where clients connect to one of their local (or known) routers or Provider nodes. Routers are peers in the network with content routing information, who act as request forwarders. Provider nodes on the other hand are nodes that act as routers, but also store hot copies of content. In the context of this RFC, Providers are a subset of routers, although the role of routers has not been crystalised from the cryptoeconomic point of view, so eventually the role of a router might have to collapse into that of a Provider. In this RFC, we use the terms Provider and router interchangeably.

Provider nodes and routers will always be looking to gather as much information as possible about content records, ideally storing information about all content records in the network in their local database. 

We assume that the required size to store the amount of records (i.e. hot copies of individual objects stored in the retrieval network) will be in the order TBs, so it is not unreasonable to think of a router keeping information about every single content record (in slow memory). Routers will look to have as much updated information about client records as possible, as in this way they would contribute to serving retrieval requests. In turn, being able to serve more retrieval requests means that the router will gather more information about popular content, which it could utilise to fetch and serve popular content from its own cache (hence, receive rewards). Routers will want to be in “the best path” for the content in order to have access to the reward. That said, the underlying retrieval economic model will be enough to incentivize routers to act honestly and keep their routing tables up to date. 

In order to minimize the required size of routing information, we propose using probabilistic set structures (such as bloom filters, cuckoo filters, or interactive protocols) to compress routing data and exchange it with other peers. For instance, peer A would build a bloom filter with a low probability of false positives representing all the content records he knows about, or knows how to reach. Peer A shares this with other peers, such as peer B. When peer B receives a request for CID1, it starts checking it against its own bloom filter, and the bloom filter of all the other peers from which it has received information. Routers will try to have as much information in their routing tables as possible, so finding a router with the routing information for a record should be straightforward.

Alternative ways to spread provider records could include: 
1. Routers share records with their x-hop neighbours, where x is a value that guarantees traffic remains manageable.
2. Routers keep a record for requests they serve only and progressively build as big a table as possible.
3. If names have hierarchical structure, then routers can only keep prefixes that point towards “network directions”, rather than keeping a separate full name entry per content. This enables name aggregation, hence, routing table scalability.

Routers can update the freshness of their routing information in several ways:
- Through an “updates” pub-sub channel where routers exchange deltas over their routing structures to update the view of others.
- Inspecting the requests being forwarded to them by other peers.
- Using “keep-alive” messages to see if the content has expired.

The flow of a client request with this scheme in place would be as follows:
1. The client sends the request to its local Provider node, which acts as the gateway to the retrieval network.
2. If this local content service has a hot copy for the content it directly serves it to the client. If on the other hand it has knowledge about who has a hot copy for the content in its routing table, it replies with the peer (or group of peers) with the copy (similar to what traditional CDN’s DNS systems currently do).
3. If the local content service doesn’t know where a peer is storing the content, it points to a bigger content service (other router with more information about content records). In this way, we try to recursively find the hot copies. 
4. Once the client receives a response with a dialable peer storing a hot copy of the content, the retrieval starts, and the client metering comes into action.
5. Retrieval itself does not follow symmetric routing, i.e., the content does not flow through the Provider node/router that pointed to the hot copy. Optimisations here could include symmetric routing when a router/Provider receives multiple requests for the same content (i.e., time-shifted multicast).

A name-based routing approach, based on hierarchical names could be an improvement here.

### Impact

This is an impactful proposal for two reasons:
1. It provides an easy (to conceptualise and implement) way to kickstart the 3DM network (i.e., all Provider nodes/routers know everything about every content object), which will be viable in the beginning.
2. It gives us a nice way to experiment with request forwarding through the network of Providers. There is a lot of optimisation that can be carried out afterwards, but having a working playground will be very valuable.

### Pros and Cons

Pros
1. Fast retrieval of content
2. Easy to implement
3. The network of Provider nodes becomes the 3DM graph itself, which makes performance optimisation easier, e.g., multicast can be easily implemented on top.

Cons
1. Having symmetric routing on by default would increase bandwidth consumption of routers/Providers.
2. A lot of memory needed to keep provider records.
3. There is no backup solution for the case when the local Provider/router does not have the content and does not know anyone who has the provider record. The proposal mentions  some hierarchical structure so that the request is forwarded  to a parent cache, but this has to be better defined.

### Implementation notes

TBA

### Evaluation

- Size of routing table relative to the content catalogue size
- Bloom/Cuckoo filter efficiency evaluation
- Symmetric vs Asymmetric retrieval routing - what is the right point to switch between the two and what is the bandwidth overhead
- Pubsub protocol evaluation for content record propagation relative to the network size and the content catalogue
- Is the speed at which a record is served an issue? Given the size of the routing table, entries will have to be stored in slow memory.

### Prior work

N/A

