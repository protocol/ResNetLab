# Routing at Scale (1M, 10M, 100M, 1B.. nodes)

## Short Description
> In one sentence or paragraph.

Routing in content-oriented or content-centric networks means that any content published in this network needs to have a *routable address*. While traditionally the number of routable addresses needed (i.e., IP addresses) was bounded by the number of physical machines/end-nodes, switching to addressable content means that the number of routable addresses is automatically exploding to the number of different content items (i.e., any file) that is published in the Internet.

That said, *content-addressable networks face the challenge of routing scalability*, as the amount of addressable elements in the network rises by several orders of magnitude compared to the host-addressable Internet of today.

In the case of IPFS and libp2p, content routing is realised by means of a Distributed Hash Table (DHT). Although DHTs are known to scale with increasing number of nodes, *the decentralised and totally unmanaged structure of the IPFS and libp2p systems presents challenges when it comes to dialability of nodes and look-up latency in the underlying network.*

## Long Description

Routing in content-oriented and content-centric networks has traditionally been a painful open issue. Following the conventions of the Internet Protocol to store the pre-computed routing/forwarding state for _all routable names or prefixes_ at the network nodes (where content name prefixes need to be stored) raises scalability concerns in content-oriented networks.

**Keeping State per Content**

Attempting to address content directly means that routing tables have to keep state per content, as opposed to keeping state per end-host as is the case today. This applies to both content-addressable overlay P2P networks and native network layer content-centric networks. Furthermore, in a native network-layer content-addressable network, routing will likely have to be done *per content chunk*, where each chunk is similar in size to an IP packet to avoid further fragmentation. Although the size of a content chunk is subject to debate, it is assumed that it is going to be in the order of a few KBs. This, in turn, means that routing nodes have to *keep state per content packet*, as opposed to end-host machine, which increases even more the size requirements of routing tables. In case of P2P overlay networks, this requirement can be relaxed and addressable items can be of bigger size. However, as the addressable space gets bigger (i.e., more content is published), keeping state of how to resolve content is becoming an issue in overlay networks too.

**Content Naming and its Role in Content Routing**

Clearly, naming of content items plays a central role in content resolution and routing and therefore, the choice of naming convention is a key in addressing routing. Traditionally, there have been two approaches to content naming: *i) routing on flat names, and ii) routing on hierarchical names.* Flat naming schemes have been mostly used in overlay P2P architectures, where hierarchy does not necessarily represent the physical topology. Hierarchical naming has mainly been proposed to realise hop-by-hop routing at the network layer, as is the case in a few architectures in the area of Information-Centric Networks.

Each of those approaches to naming is defining the overall network architecture. In case of routing on flat names, the resolution is done based on some Name Resolution System (such as the DHT in case of IPFS and the NetInf architecture or in name-based DNS systems in the COMET architecture). The corresponding routing approach is generally called *“name-resolution-based routing”* and always involves talking to a higher-layer entity. In turn, this means that there is an *overlay network built on top of the underlying physical network topology*. In contrast, in case of hierarchical names, every routing node is identifying the next-hop node through pointers in the FIB table. This routing approach is generally called *“name-based routing”*, given that there is hop-by-hop routing on every network node.

**Unmanaged P2P Overlays**

In overlay P2P networks routing is influenced by whether there is structure or not in the formation of the network. Unstructured P2P networks are the easiest ones to form, as nodes simply connect to the network and start connections with a random set of other peers. When requesting some content, nodes have to flood the network, that is, ask as many peers as possible whether they have a specific piece of content or not. In contrast, in structured overlay networks the overlay is organised into some topology using external means. Distributed Hash Tables (DHTs) have been extensively used to structure overlay P2P networks. Publish/Subscribe algorithms have also been proposed to disseminate information in P2P networks. Both structured and unstructured P2P systems come with advantages and disadvantages. Unstructured networks are more robust to high rates of churn (high numbers of nodes joining and leaving at random), but on the other hand require more bandwidth resources to resolve content as requests need to be flooded to the network. Structured P2P networks do not need to flood the network to resolve some content, but are less robust to high rates of churn.

Clearly, structured P2P networks have better scalability properties. However, structured *but unmanaged* P2P networks, that is, networks where there is some structure (e.g., through some DHT), but there is no central authority to manage the network, are challenged as the network grows and peers join and leave at random times. *Being able to deliver content within reasonable timeframes and with network sizes in the order of millions of active nodes is becoming a challenge.*

**Summary: There are two fundamental steps to resolve content: i) content-name to location mapping, and ii) path-formation to get to that location.** This is what defines **the architecture** and those two steps need to be designed in a way that is scalable and also efficient in terms of performance. *In the context of IPFS and libp2p, the first of the above functions (content-name to location mapping) is being taken care of by IPFS, while the second step (path formation) is done through libp2p.*

The current version of the system is using a single DHT for all content published in IPFS and even within individual IPFS Clusters. Although this has been working fine up until now, it is expected to face scalability problems as the system is scaling up and attracting more users and orders of magnitude more traffic. Furthermore and more importantly, **given that IPFS is running as an overlay on top of the physical topology, dialability of nodes that are physically far away from the requesting node is increasing content look-up latency to prohibitive levels.**

As a system that is expected to provide *timely delivery* of content to users, IPFS needs to take into account topological characteristics of stored content and resolve the closest possible copies. Closest can be defined both as number of network hops, but also as a round-trip delay. It has been clear from the growth and dominance of CDNs and their extensive localised caching techniques that resolution of content from stored copies needs to be topologically embedded, that is, try to find and resolve local content first before getting to copies that are (topologically) far away.


## State of the Art

> This survey on the State of the Art is by no means complete, however, it should provide a good entry point to learn what is the existing work. If you have something that is fundamentally missing, please consider submitting a PR to augment this survey. 

### Within the libp2p Ecosystem
> Existing attempts and strategies

In the wider IPFS, libp2p and IPLD systems, each of the components is taking care of a separate part of the architecture. In particular:

- IPLD: (largely) takes care of naming content items
- IPFS: takes care of the “content to location mapping” (i.e., CID to peerID) through DHT
- libp2p: takes care of building the path from peerID to the IP address of the node (using protocols such as pubsub/gossipsub and bitswap).

As mentioned earlier, the current version of the system is using a *single DHT* for all content published in IPFS. The system is building on the **Kademlia DHT**. Although other DHT versions have been discussed, there was not enough consensus to suggest *significantly improved performance* in order to shift to some other DHT proposal. Apart from the DHT, libp2p is also implementing a pubsub protocol, which is based on gossiping and acronymed [gossipSub](https://github.com/libp2p/specs/tree/master/pubsub/gossipsub). GossipSub and its optimisation [episub](https://github.com/libp2p/specs/blob/master/pubsub/gossipsub/episub.md) can be used independently from the DHT for content routing, although gossipSub is currently not used for this purpose in libp2p.

There has also been discussion within the libp2p and IPFS ecosystems for a *Multi-Layer DHT*, where different layers would cover different topics or applications, or different geographical areas by topologically embedding the structure of the DHT over the physical underlying network. The ultimate target in all those cases is to **reduce the content look-up time by reducing the number of underlying network hops that requests and content has to travel**.

### Within the broad Research Ecosystem
> How do people try to solve this problem?

#### Structured P2P Overlays

Peer-to-Peer networks have received tremendous attention by the networking research community in the last 15 years or so, with interest generally declining over time. As discussed earlier, P2P networks have generally been divided in two main categories: "structured" and "unstructured". Here we will focus on structured P2P overlay networks, as the P2P system implemented in libp2p falls in this category and would not see any performance benefit if it moved to an unstructured network. We will briefly survey the most prominent proposals for routing in structured P2P networks, where the dominating approach has always been the use of a DHT table. The specifics of the DHT table itself as well as the structure of the overall system differentiates the proposals in terms of features and performance. For a comprehensive review of P2P Overlay Networks you are strongly encouraged to look at the following papers:

- [A Survey and Comparison of Peer-to-Peer Overlay Network Schemes](http://snap.stanford.edu/class/cs224w-readings/lua04p2p.pdf), IEEE Communications Surveys & Tutorials, March 2004
- [State-of-the-art survey on P2P overlay networks in pervasive computing environments](https://www.sciencedirect.com/science/article/pii/S1084804515000879), Journal of Network and Computer Applications
Volume 55, September 2015, Pages 1-23
- [A Survey on content-oriented networking for efficient content delivery](https://ieeexplore.ieee.org/abstract/document/5723809), IEEE Communications Magazine, DOI: 10.1109/MCOM.2011.5723809
- [A Survey of Peer-to-Peer Security Issues](https://www.cs.rice.edu/~dwallach/pub/tokyo-p2p2002.pdf), 2003.

1) CAN: Content Addressable Network (2001)

CAN was one of the first proposals in the area of content-addressable space and made use of DHTs, but with the distinct feature (as compared to those that followed) that the in the DHT nodes were not arranged in rings, but rather in a virtual multi-dimensional Cartesian coordinate space. CAN has been radical in its design when proposed with very promising performance, but it is not resilient under network partitions, it does not handle well load-balancing between nodes and in order to deal with churn a node needs to have many neighbours in the same in the overlay neighbour set.

S.  Ratnasamy,  P.  Francis,  M.  Handley,  R.  Karp,  and  S.  Shenker,  [“A scalable  content  addressable  network”](https://people.eecs.berkeley.edu/~sylvia/papers/cans.pdf), In Processings of the ACM SIGCOMM, 2001, pp. 161–172

2) Chord (2003)

Chord is also one of the very early proposals that use DHTs, but is building on consistent hashing to allocate keys to the peers forming the ring. By using consistent hashing, Chord can deal effectively with load-balancing between nodes, as well as limited churn rates. It has been shown to drop in performance under increased churn rates, as well as sub-optimal look-up times.

I. Stoica, R. Morris, D. Karger, M. F. Kaashoek, and H. Balakrishnan, [“Chord:  A  scalable  peer-to-peer  lookup  protocol  for  internet  applications”](https://pdos.csail.mit.edu/papers/ton:chord/paper-ton.pdf), IEEE/ACM Transactions on Networking, vol. 11, no. 1, pp. 17–32, 2003.

3) Pastry (2001) & Tapestry (2004)

Both Pastry and Tapestry use the Plaxton (1997) prefix routing scheme to provide scalable routing and take into account network locality. Pastry has been shown to scale well to six-figure network sizes, which has been a remarkable figure for the time (circa 2000), but not so much so for present-day deployments. Dealing with node churn is also not one of the strong points of Pastry. Tapestry improves performance in terms of scalability, network locality (as it applies redundancy) and fault tolerance, at least as compared to Pastry.

A. Rowstron and P. Druschel, [Pastry: Scalable, decentralized object location and routing for large-scale peer-to-peer systems](https://www.cs.rice.edu/~druschel/publications/Pastry.pdf), in Proceedingsof the Middleware, 2001.

B. Y. Zhao, L. Huang, J. Stribling, S. C. Rhea, A. D. Joseph, and J. D. Kubiatowicz, [“Tapestry:  A  resilient  global-scale  overlay  for  service deployment”](https://www.srhea.net/papers/tapestry_jsac.pdf), IEEE  Journal  on  Selected  Areas  in  Communications,vol. 22, no. 1, pp. 41–53, January 2004.

C. Plaxton, R. Rajaraman, and A. Richa, [“Accessing nearby copies ofreplicated objects in a distributed environment,”](https://dl.acm.org/citation.cfm?id=258523), in Proceedings of the 9th Annual ACM Symposium on Parallel Algorithms and Architectures,1997 [slides](http://homepage.divms.uiowa.edu/~ghosh/8.Plaxton.pdf)

4) Kademlia

Kademlia takes the novel approach of assigning nodes with PeerIDs and stores content items to nodes whose PeerIDs are _close_ to Content IDs. _Close_ here is defined as the XOR difference between the PeerID and the ContentID. This way, the NodeID is used by the routing algorithm to get to the node that stores the requested content. Kademlia performs in _O(logN)_ in terms of resource discovery and although it does not have any clear performance benefits in terms of network locality, or scalability, it has been the most widely-used protocol in real systems (e.g., BitTorrent).

_Summary:_ The above are just some of the most popular proposals for structured P2P overlay systems. There have been many more proposals that have tried to optimise in one of the many challenges, i.e., scalability, churn, look-up time (see above-referenced surveys for many more references). However, as the Internet has grown exponentially over the past decades, the requirements of today's systems demand much more than those proposals can realistically offer to systems such as IPFS/libp2p, where: i) networks size is already above 100,00k active daily nodes, ii) node churn can be huge, and, iii) end-users expect look-up and delivery times an order of magnitude less than those expected when the above protocols were proposed.

In order to address those issues, but at the same time aligning with the dominant content-addressability line of work, below we discuss newer proposals, in the direction of Information- or Content-Centric Networks. These proposals fundamentally have much wider scope (exploiting in-network entities and attempting to build new Internet-wide architectures) and as such can be a great source of inspiration for the IPFS and libp2p ecosystems.


#### Information-/Content-Centric Networks

In parallel to the initial development of IPFS and libp2p, there has been a parallel stream of work mainly driven by the academic and research community to shift the focus from host-based networking to content- or information-centric networking (CCN/ICN). Most of the work kicked off around 2006, mainly from Van Jacobson and in the form of talks - see [1] for a very inspiring talk by Van Jacobson. This later materialised in several papers and is still going on today under the umbrella of the Named-Data Networking project (see below).

A number of projects have since emerged (in the US, Europe and Asia) to investigate the properties of a content-centric network, where content itself is the addressable and routable primitive and to build a scalable, efficient and secure Future Internet Architecture. The work has (mostly) assumed that this future Internet architecture will build on top of IP and will involve in-network routing and forwarding entities (i.e., network routers), which will be able to “speak” ICN language and related protocols.

1) Named Data Networking (NDN)

NDN is a US-driven project, which however is attracting attention worldwide. NDN follows a “name-based routing” approach, where names are hierarchical and routers do longest-prefix matching to find the next node to forward the request to. This is the most radical of the ICN architectures, which assumes that routers in the network (or at least some of them) understand the NDN stack and support in-network content caching. Although it comes with several challenges (e.g., scaling the routing table size), its expected benefits are significant, being able to natively support client mobility, in-network caching and multicast. 

Material: 
- Website (includes a ton of material): named-data.net 
- Original 2009 paper: https://named-data.net/publications/networkingnamedcontent/ 
- Follow up 2014 paper: https://named-data.net/publications/named_data_networking_ccr/ 

2) DONA: Data-Oriented (and beyond) Network Architecture

DONA is a 2007 ACM SIGCOMM paper, which advocates a hierarchical data resolution structure, where there is at least one “resolution handler” in each ISP domain. Names are structured according to the form P:L, where P is the cryptographic hash of the principal’s (publisher’s) public key and L is a label given by the principal/publisher which needs to ensure that names are unique. Given the hierarchical structure of the architecture, a challenge for DONA is the stress on the top-level resolution-handler (assuming we’re addressing the entire Internet). 

Material: 

- Paper: http://ccr.sigcomm.org/online/files/fp177-koponen1.pdf  

3) NetInf: Network of Information

NetInf is the outcome of an EU project (which ended around 2015 or so) which resembles a lot the IPFS structure. NetInf is building on name-resolution-based routing (through a Name-Resolution System - NRS). The NRS is a multi-level DHT and results have reported that the architecture can scale to several millions of nodes with lookup latencies of less than 100ms. 

Material:
- Overall Architecture paper: https://www.sciencedirect.com/science/article/pii/S0140366413000364 
- Measurement/Simulation study: https://www.sciencedirect.com/science/article/pii/S0140366413000418 

[1] Van Jacobson’s 2006 Google Tech Talk - A New Way to Look at Networking: https://www.youtube.com/watch?v=gqGEMQveoqg

Surveys on the topic

- A Survey of Information-Centric Networking Research (2013), https://ieeexplore.ieee.org/abstract/document/6563278
- A Survey of Information-Centric Networking (2012), https://ieeexplore.ieee.org/abstract/document/6231276

### Known shortcomings of existing solutions
> What are the limitations on those solutions?

All of the above-mentioned approaches, both in P2P and in the ICN space are facing scalability issues. This is primarily due to the vast amount of content items that need to be reachable. In the case of P2P systems, the proposals discussed above are modelling systems with less users and much less traffic. Since then (early 2000s), the Internet has grown tremendously, both in terms of numbers of connected nodes and in terms of volume of data. IPFS, for instance, is already counting users in the order of hundreds of thousands per day and is not even considered one of the very big systems of today. IPFS is expected to grow exponentially.

On the other hand, and in the case of Information-Centric Networking proposals, we should not forget that most of the research proposals are targeting the whole of the Internet as a future network architecture that is expected to gradually replace host-based networking. That said, that set of proposals is aiming at massive scale adoption and is tested and evaluated against such deployments. This fact makes proposals in that space attractive to consider for systems such as IPFS, since scalability problems when attempting to address the Internet are not necessarily seen in distributed storage systems such as IPFS. Nevertheless, there have been several proposals to scale ICN architectures, which are also worth considering:

- [On Demand Routing for Scalable Name-Based Forwarding](https://conferences.sigcomm.org/acm-icn/2018/proceedings/icn18-final53.pdf), ACM ICN 2018.
- [A Note on Routing Scalability in Named Data Networking](https://named-data.net/wp-content/uploads/2019/08/zhang2019a-note.pdf), 2017

## Solving this Open Problem

In terms of scalability, a Multi-Layer DHT should be constructed, which should ideally be topologically embedded, meaning that the nodes participating in one DHT should be geographically and (therefore) topologically close to each other.

In addition to the name-resolution based system currently deployed (through the DHT system), we should involve an element of name-based routing (where possible) in order to avoid hitting the DHT with every request. This could be an extension of the IPFS gateway, or a proposal for a separate network entity. Recall that DHTs are by design stochastically suboptimal, as the overlay structure does not necessarily represent the network topology (i.e., one hop on the DHT could translate to tens of network-layer router hops) and therefore delivery delay increases.

A very recent related paper discussing those issues is here:
[Towards Peer-to-Peer Content Retrieval Markets: Enhancing IPFS with ICN](https://conferences.sigcomm.org/acm-icn/2019/proceedings/icn19-34.pdf), ACM ICN 2019.

### What is the impact

Solving routing scalability for IPFS and libp2p is of utmost importance. As the network grows and more traffic is inserted in the system, a single DHT is unlikely to perform according to expectations. This will reduce user expectations and user satisfaction and can become a show-stopper for wider adoption.

### What defines a complete solution?
> What hard constraints should it obey? Are there additional soft constraints that a solution would ideally obey?

- The system should **scale to tens of millions of active and reachable nodes**. Routing scalability should be demonstrated through the right metrics, one of which should be look-up delay.
- The proposed algorithms should **account for both reduced look-up time but also reduced delivery delay**. In this respect, IPFS should not only be seen as a storage system, but also as a _timely content delivery network_, similar in nature to present-day CDNs (without necessarily targetting similar SLAs though). _Smart content replication and caching_ should therefore be considered in order to achieve those goals.
- The system should be **able to deal with churn in the order of hundreds of thousands of nodes**. When this volume of nodes leave the system, the routing algorithm should still be able to route to the requested content item. This includes both routing stability, but also content replication.
- In all cases the system should be able to **guarantee 100% success in content resolution**, i.e., all content should be reachable at all times (even if look-up time is longer than usual).
- The system should be **able to deal with sudden increase in traffic demand**, e.g., an order of magnitude more requests than normal. In dealing with increased demand, the system should be able to load-balance between nodes, i.e., not overload a few nodes when others are underutilised.


## Other

### Existing Conversations/Threads

- https://github.com/ipfs/notes/issues/291
- https://github.com/ipfs/notes/issues/198
- https://github.com/ipfs/notes/issues/162
- https://github.com/ipfs/notes/issues/15
- https://github.com/libp2p/notes/issues/10
- https://github.com/libp2p/notes/issues/3
- https://github.com/libp2p/research-dht/issues/6

### Extra notes
