# RIW2021 | RFC: Name-based Request Forwarding for Nearest Replica Routing

_Status:_ < draft >

_Area of Improvement:_ < Distribution Graph Forming >

_Estimated Effort Needed:_ <?>

_Prerequisite(s):_ <?>

_Priority:_ <? P0, P1, P2>


### Abstract


This RFC assumes that Retrieval Miners (RMs) act as routers for requests. It also assumes that every client is connected to one or more RMs to which they forward their requests. Content is explicitly named by CID. Every RM declares and serves some network prefix. Progressively, these prefixes form the topology. Content items are initially pegged to the network location of their publisher, e.g., /de/berlin/my-cid, but  nothing prevents other RMs from serving content with any name, achieving nearest-replica routing.

### Proposal/Construction

- A retrieval miner is seen as an entity that covers some geographic area, somewhat similar to a DNS server. It is the entry point of the clients to the Filecoin network.
  - More than one miners can serve some geographic area, either split between the miners, or in some hierarchical manner, where the initial RM is a higher-tier RM for the rest of the RMs.
  - Example: the first RM deployed in Germany declares the prefix “/de” as their service area. Other RMs joining the system can then start serving “/de/berlin/”, “/de/munich/” etc.
- A client should ideally be connected to more than one retrieval miner.
- Every retrieval miner keeps a record of: i) all the content they store locally, ii) the content items they have heard about in terms of their CID, as well as iii) “where did they hear the content from” (defined similarly to a forwarding network interface of a router, binding the full name of a content to the IP address of the RM on that direction).
  - Example: the RM serving the network area /de/berlin/ sees request /de/munich/cid1. It forwards the request towards /de/munich/ and populates its routing table with: cid1 -> /de/munich/ | <IP of /de/munich/>.
- Retrieval miners are interconnected with each other in a mesh structure. Every retrieval miner should be connected to at least one other retrieval miner. The degree of connectivity is to be defined.
- When content publishers publish their content, they have to link it to the RM in the geographic area where the content is stored.
  - Example: content item with cid2 is published by a content publisher or a hosting company connected to the RM that serves the area /us/nyc/. The full network name of this CID is /us/nyc/cid2.
  - The RM itself can also store the content if the content publisher has a deal with the RM directly. This does not change the model.
  - The model does not prevent another retrieval miner, say, /us/boston/ from storing and serving the content. The cryptoeconomic model needs to work out when and at which cost for the new RM this should happen.
- If the retrieval miner receives a request for a content item which it does not know, it forwards the request according to the prefixes it sees.
  - Example: if the RM /de/berlin/ receives a request for /us/nyc/cid2, which it has never heard about, it does longest prefix matching on the request and forwards it accordingly. If it knows the IP address of the RM in /us/nyc/ it directs the request there. If not,then it forwards to a RM “listening”/serving the prefix /us/. If it doesn’t know any RM serving prefix /us/, it will have to flood its neighbours. 
  - To avoid the above situation where a RM does not know anyone serving some prefix, we can have a prerequisite that there is basic connectivity between all RMs serving top-level prefixes. Populating routing tables can be done in advance by a routing protocol.
- When routers see repeated requests for the same CID, they do not need to forward all of them towards the source. Instead, they can keep the request and, when they receive the content, they can forward it to satisfy all received requests. This way, the system can support a time-shifted multicast model, which can increase performance significantly, especially for heavy and popular content, e.g., popular HD video.
  - The downside of this approach is that bandwidth requirements increase significantly if all traffic needs to travel through all RMs where the request has been forwarded through. This will be the case for non-popular content and we expect to have a tradeoff, where after the tipping point the overall bandwidth spent by this approach is lower.
  - An alternative is to follow the above approach (called symmetric routing) only after the request count for an object surpasses a threshold. Below that threshold, the request is forwarded through the graph of overlay RMs, but the actual data is transferred through a direct connection from the RM to the client.
- A RM that sees repeated requests for some particular CID can fetch, cache and serve the content. This behaviour should be encouraged by the cryptoeconomic model, as it will improve the performance of the network for popular content.
  - Assuming the geo-localised connectivity of clients to miners, we can assume that a client is always connected to a miner that is less than $k$ms away (say $k=50ms$).
  - Every content item is linked with a record that specifies: i) the price they have set in order to serve the item, and ii) the latency under which they can deliver it to the client (assuming a maximum of $k$ms one-way latency).

### Impact

If designed and implemented correctly, this scheme can provide significant performance benefits, especially for popular and heavy content.

### Pros and Cons

Pros:

- Clean and relatively simple routing and forwarding model
- Can achieve nearest-replica routing, which will be very beneficial for popular content
- Can achieve time-shifted multicast, which can save a lot of bandwidth for heavy content


Cons:
- It’s a new design with little deployment and testing to date in real systems
- Security properties need to be seen carefully, as there haven’t been lots of studies, or experiences from previous deployments.
- It is not clear how accounting can work on top of this setup.

### Implementation notes

TBA

### Evaluation

- Evaluate the extra bandwidth requirement that symmetric routing is imposing into the system.
- Define the point where a switch between symmetric and asymmetric routing is the best option in terms of bandwidth consumption and delivery delay.
- Approximate the scalability of the system as the content catalogue of the entire network gets bigger. Routing tables might become too big, which will become non-viable for RMs. Can big routing tables be served from fast memory?
- Based on a content catalogue size, network size and a request distribution pattern, what is the probability of routers to receive requests where they have no information about any of the prefixes in the content name.
- Routing protocol for route setup and updates: how long does it take to propagate route prefixes.

### Prior Work

- The work is inspired in large parts from the [Named-Data Networking Architecture](https://dl.acm.org/doi/pdf/10.1145/2656877.2656887).
- Further optimisations for content delivery have been proposed at the [iCDN approach](https://dl.acm.org/doi/pdf/10.1145/3405656.3418716).
- The vast literature on the NDN architecture and supporting protocols can be very useful to find solutions to the challenges of the approach.

