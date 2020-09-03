# PubSub at Scale

## Short Description
> In one sentence or paragraph.

Publish/Subscribe (pub/sub) messaging systems have been proposed and traditionally used to disintermediate senders and receivers of messages. In pub/sub systems, publishers of content do not send the published messages directly to one or a group of receivers, but instead *publishers are sending messages to a topic.* The pub/sub system then needs to match subscribers' interests to published messages, or more commonly known, events. This model of communication enhances asynchronous communication and reduces significantly network traffic and bandwidth-requirements.

*Within the IPFS and libp2p ecosystetms, pub/sub is being used to push naming record updates to the decentralised Naming System of IPFS, acronymed IPNS* (https://docs.ipfs.io/guides/concepts/ipns/). As the IPFS system is evolving and growing, communicating new entries to IPNS is becoming an issue *due to the increased network and node load requirements*. The expected growth of the system to *multiple millions of nodes is going to create significant performance issues*, which might render the system unusable. Despite the significant amount of related literature on the topic of pub/sub, very few systems have been tested to that level of scalability, while those that have been were mostly cloud-based, managed and structured infrastructures. Instead, in the case of IPNS (and IPFS more in general), *the network is totally decentralised and therefore unmanaged*. This poses new challenges to the pub/sub protocol, which we want to explore through this Open Problem.

## Long Description

**P2P Overlays**

By and large, peer-to-peer (P2P) networks can be split in two categories: *i) structured P2P overlays* and *ii) unstructured P2P overlays*. In structured P2P overlays (or networks), the network has some structure, e.g., it is based on some topological or node hierarchy. In such cases, some nodes (often called Super Nodes) can be assigned more responsibilities than others, such as for example, relay published events to subscribed nodes. Those nodes are also assumed to be dedicated servers, hence, they do not present disconnection issues and can support routing of pub/sub messaging and other operations related to the pub/sub system.

Unstructured (or unmanaged) P2P overlay networks, on the other hand, *do not assume any topological structure or connectivity properties for any of its nodes*. That is, in unstructured P2P networks, nodes can be of any type (i.e., from always-on rack servers, to ephemerally connected laptops and mobile devices) and thus, connect and disconnect at random times. These random connectivity patterns make it impossible to assign extra event routing or message caching responsibilities to any node in unstructured P2P networks. In turn, designing message propagation and guaranteeing reliability of message delivery (that is, that a message will reach all nodes in the network within a given amount of time) is very difficult.

**Pub/Sub Systems Overview**

For the reasons mentioned above, unstructured P2P networks very often use pub/sub protocols that are closer to flooding, or random walks on the overlay to propagate event information. However, flooding is introducing a lot of extra traffic in the network, while random walks may take extended amounts of time before reaching all nodes.

Some pub/sub systems have been implemented in the form of broadcast trees, according to which, once messages are published, they are broadcast to all nodes in the tree. Although broadcast can ensure timely delivery of messages to all receipients, they have questionable scalability properties, as the delay to notify subscribers increases linearly with the number of nodes. They also usually cause severe stress to the system in terms of bandwidth needed to deliver the messages and are prone to increased levels of churn.

One of the main benefits of pubsub systems is that receivers of information (or subscribers) can individually pull information when they need to. This reduces a lot the strain put in the system to deliver messages as they are published. However, in case of large-scale systems, the overhead of publishing information to a topic can still overload the system.

While previous work has addressed many different aspects and requirements of pub/sub design for structured P2P networks (see next Section on State of the Art), little has been done for unstructured P2P networks. For instance, aspects related to the relation between gossip levels and the scalability of the system have not been addressed thoroughly in prior art.

**Pub/Sub in Unstructured P2P Overlay Systems**

In distributed storage systems and in the case of the IPFS ecosystem in particular, pub/sub can be used for several purposes, including  content routing, i.e., one of the most central and vital functions of the system. IPFS is a content-addressable, distributed P2P storage network with hundreds of thousands of daily users. Users can participate in the network as unreliable nodes, e.g., using laptop devices and with frequent disconnections. The gossip-based pub/sub protocol used so far (acronymed "gossipsub" - see pointers below) was developed within Protocol Labs with those system and environment requirements in mind (i.e., unmanaged network and unreliable nodes) and is currently used to **push naming record updates to the decentralised naming system of IPFS, the InterPlanetary Naming System (IPNS)**.

Pub/Sub and in particular gossipsub will also be used in the very near future by emerging P2P systems, such as the sharding version of the Ethereum Network (ETH2) and filecoin, as the main routing protocol.

**Problem Statement**

Most pub/sub systems that have been proposed before mostly have proven scalability properties in *managed and cloud-based environments*. Few pub/sub protocols have been proposed for unstructured P2P overlays and those that have, have rarely been tested with more than 10,000 nodes and high rates of churn. IPFS already has *hundreds of thousands of daily users and is expected to grow exponentially to multiple millions*. ETH1.0 already has more than 16,000 users and when ETH2.0 arrives this number is expected to rise by several orders of magnitude. Furthermore, systems such as ETH2 and filecoin are primarily financial systems and are expected to carry *transaction messages worth millions in monetary value*. That said, a *thorough evaluation of the performance of the existing protocol **or its redesign** is essential before it gets deployed* in those systems. This is the goal that this Open Problem intends to pursue.


## State of the Art

> This survey on the State of the Art is not by any means complete, however, it should provide a good entry point to learn what are the existing work. If you have something that is fundamentally missing, please consider submitting a PR to augment this survey. 

General purpose pub/sub messaging systems can prove very useful from several different aspects (from management and operation to performance) in P2P networks and as such come with many tradeoffs. Due to the wide variety of applications building on top of pubsub systems, not all tradeoffs apply to all systems and many of them are contradictory to each other. Below, we discuss some of them.

**Reliable Delivery.** In case of no node downtime, all published messages should be delivered to all subscriber nodes. Pub/Sub systems should be *robust against node churn* and should still reach most nodes (i.e., achieve high hit rate). Apart from *robustness against churn*, *fast recovery from churn* is also a necessary feature.

**Load Balancing.** The event message relay load should be roughly equally split between nodes. Assuming a scaled up system where nodes might be subscribed to 5K events, relaying messages is becoming a heavy task and therefore, the more nodes a node is connected to (in terms of degree), the more the relay tasks it will have to carry out.

**Scalability.** Given the growth of networked systems, both in a cloud environment, but also in a P2P network, the system should be able to scale up to millions of nodes. There have been very few (if any) systems that achieved scalability of that order in unmanaged, P2P environments.

**Resource-Efficient.** The system should avoid duplicate messages (i.e., deliver the same message to a node twice). This increases load on individual relay nodes, but also the overall system's bandwidth requirements.

**Striking the right balance, especially in an unmanaged, unstructured P2P overlay of massive scale has not seen a solution to date. This is the gap that we are looking to fill with the outcome of this Open Problem.**

### Within the broad Research Ecosystem
> How do people try to solve this problem?

- We've collected a vast amount of Research Material around PubSub. It can be found at https://github.com/libp2p/research-pubsub. Please keep in mind that the literature in this space is vast. The purpose of this section is not to survey the majority of the proposals, but instead to point to the most important contributions from which lessons can be learned and applied to an unstructured, unmanaged, P2P pub/sub overlay.

Related literature in the broader research community has worked towards two incarnations of pubsub systems: i) topic-based pub/sub and ii) content-based pub/sub.

**Topic-Based Pubsub**

In topic-based pubsub systems, subscribers declare interest in some topic. Publishers add/tag topics to their publication and those topics that match subscribers' interests are broadcast to them. Depending on the nature of the application running on top, topic-based pubsub can reduce the complexity of the system.

Scalability in pub/sub systems was a key requirement in cloud environments, as cloud-based systems had to scale to accommodate demand. Amazon (Simple Notification Service, SNS) and Google/Firebase Cloud Messaging both have pub/sub protocols in operation, although their operational details have not been widely revealed.

Scribe was one of the very first pub/sub systems that proposed a decentralised multicast overlay, on top of the Pastry DHT. DHTs have been used in several pub/sub systems (see Meghdoot, Bayeux), where in most cases the DHT is used to find where subscriptions are located and route to them (as in Meghdoot), or as a rendezvous point (as in Scribe) for a topic. Poldercast is an interesting approach which uses a ring overlay (and additional links as optimisation). Subscribers to a topic are connected to this ring and messages propagate to all other subscribers. Clearly, the latency to inform all nodes is increasing linearly, unless direct links exist and can be exploited. Dynatops was proposed as a self-configured topic-based pub/sub system which can deal with short-lived subscriptions. All these systems require a broker network that extends across a wide-area network to cover subscribers. Dynamoth was proposed to reduce latency as a hybrid between the dissintermediated pub/sub and the directly-connected client-server models. It is, however, exclusively applicable to cloud-based systems and not to P2P overlays.

Systems such as Vitis, Tera and Rappel use gossiping to propagate information and build on unstructured P2P overlays. In particular, the authors in Vitis are arguing that most similar systems are building one separate overlay per topic, which results in nodes being members of an extensive number of overlays. This, in turn, increases overhead for relay nodes, which might get disincentivised and leave the system. In contrast, Vitis is dealing with this problem by bounding the number of connections per node. This is done by using gossip messages to sample the topics that other nodes have subscribed to. Then grouping nodes into the same overlay where topics overlap reduces the number of overlays needed, while at the same time keeping nodes subscribed to the topics of their choice.

Rappel is targeting low overhead and noise to subscriber nodes, that is, receiving messages for topics that the node is not subscribed to. Rappel achieves that by building a network of "friends overlay" building on interest locality. Rappel also targets fast dissemination of messages by taking into account network locality on top of interest locality.

While these have been very interesting approaches to gossip-based pub/sub for unstructured P2P overlays, none of them has been tested for scalability at massive scales (e.g., millions of nodes), or significant node churn. Rappel is claiming to be robust for up to 25% node churn, while Tera leaves this part for future evaluation.


Related Literature

- Amazon SND: https://aws.amazon.com/pub-sub-messaging/
- Google/Firebase Cloud Messaging: https://firebase.google.com/docs/cloud-messaging/
- Pastry: Scalable, decentralized object location androuting for large-scale peer-to-peer systems, http://rowstron.azurewebsites.net/PAST/pastry.pdf
- Meghdoot: Content-Based Publish/Subscribeover P2P Networks, http://opendl.ifip-tc6.org/db/conf/middleware/middleware2004/GuptaSAA04.pdf
- Bayeux: An Architecture for Scalable and Fault-tolerant Wide-area Data Dissemination, https://people.eecs.berkeley.edu/~adj/publications/paper-files/bayeux.pdf
- PolderCast: Fast, Robust, and Scalable Architecture for P2P Topic-Based Pub/Sub, https://hal.archives-ouvertes.fr/hal-01555561
- DYNATOPS: A Dynamic Topic-basedPublish/Subscribe Architecture, DEBS. pp. 75–86 (2013), https://www.ics.uci.edu/~yez/papers/dynatops_tr.pdf
- Dynamoth: A Scalable Pub/Sub Middleware forLatency-Constrained Applications in the Cloud,  IEEE ICDCS 2015, http://preprints.juliengs.ca/2015/gascon2015dynamoth.pdf
- Vitis: A Gossip-based Hybrid Overlay for Internet-scale Publish/SubscribeEnabling Rendezvous Routing in Unstructured Overlay Networks, https://dcatkth.github.io/papers/vitis.pdf
- TERA: Topic-based Event Routing for peer-to-peer Architectures, https://www.ics.uci.edu/~hjafarpo/Files/PubSubPapers/DEBS2007/TERA.pdf
- Rappel: Exploiting interest and network locality to improve fairness in publish-subscribe systems, http://dprg.cs.uiuc.edu/docs/rappel_journal/rappel_journal.pdf
- Eugster, P.T., Felber, P.A., Guerraoui, R., Kermarrec, A.M.: The many faces of publish/subscribe. ACM Comput. Surv. 35(2), 114–131 (2003)



**Content-Based Pubsub**

In content-based pubsub, newly published content is tagged with a set of attribute/value pairs. In turn, subscriptions are expressed as predicates of the attributes. When predicates match attributes, the subscriber receives the information. Content-based pub/sub systems have been extensively studied in the past, but also more recently in the context of Information-Centric Networks. Generally speaking, content-based (or sometimes called attribute-based) pub/sub systems *can provide finer granularity matching between publishers and subscribers, but in order to achieve this they require more compute resources*. 

BlueDove is one of the well-known approaches in this space. It supports multi-dimensional attributes and is organising overlay servers in a scalable topology. E-StreamHub is proposed as a middleware with the interesting feature that it adds and removes nodes based on their load. Both of these approaches are exclusively cloud-based and although they achieve scalability and low-latencies, they do not apply to unmanaged P2P networks. In fact, most content-based pub/sub systems either target cloud environments, or some broker-based infrastructure (see Elvin, Sienna, HERMES, Gryphon).


Related literature

- Enabling Publish/Subscribe in ICN, IRTF ICNRG Internet-Draft 2015, https://tools.ietf.org/html/draft-jiachen-icn-pubsub-00
- BlueDove: A Scalable and Elastic Publish/Subscribe Service, IPDPS 2011, http://www.ece.sunysb.edu/~fanye/papers/IPDPS11.pdf
- Elastic Scaling of a High-Throughput Content-Based Publish/Subscribe Engine, https://ieeexplore.ieee.org/document/6888932
- Content Based Routing with Elvin4, https://pdfs.semanticscholar.org/b962/7968660c8299a8a48474c086682d5e0b42c7.pdf
- Pietzuch, P., Bacon, J.: Hermes: a distributed event-based middleware architecture. In: ICDCS Workshops. pp. 611–618 (2002)
- Rosenblum, D.S., Wolf, A.L.: A design framework for internet-scale event observation and notification. In: ESEC. pp. 344–360 (1997)
- Siena: Scalable Internet Event Notification Architectures, https://www.inf.usi.ch/carzaniga/siena/
- Gryphon: An Information Flow Based Approach to Message Brokering, https://arxiv.org/abs/cs/9810019
- Ahmed, N., Linderman, M., Bryant, J.: Papas: Peer assisted publish and subscribe. In: Workshop on Middleware for Next Generation Internet Computing (MW4NG). pp. 7:1–7:6 (2012)
- Li, M., Ye, F., Kim, M., Chen, H., Lei, H.: A scalable and elastic publish/subscribe service. In: IPDPS. pp. 1254–1265 (2011)
- Zhang, B., Jin, B., Chen, H., Qin, Z.: Empirical evaluation of contentbased pub/sub systems over cloud infrastructure. In: Intl. Conference on Embedded and Ubiquitous Computing (EUC). pp. 81–88 (2010)


**Gossip-based Pub/Sub**

GossipSub is a gossip-based pubsub system developed within libp2p (see next section). Gossipsub borrows concepts from related literature (see list below) and blends them together to produce an efficient pubsub protocol. We are therefore providing a brief overview of how gossip-based approaches work.

Traditionally, the concept behind gossiping is to address the issue of load-balancing between all nodes forwarding messages in the system. According to gossiping, once a node receives a message, it does not broadcast the message directly to all nodes subscribed to some topic, but instead it is choosing a fraction of nodes (we can use *d* to denote the number of nodes chosen) to distribute the message. In turn, those *d* nodes are choosing a further *d* nodes to distribute the message further. Clearly, receiving the message more than once is perfectly possible in gossiping systems. If a node receives a message twice, it discards the second (and any subsequent) message it receives.

The intrinsic redundancy inserted in the system through gossiping is improving the resilience of the system. At the same time, in order to maintain redundancy, every node in the system has to keep membership information for the entire system. Although gossip-based protocols reduce the stress put in the system in terms of bandwidth and connectivity requirements, it clearly poses scalability concerns due to the state that all nodes need to keep.

In order to overcome those issues, several gossip systems implement what is called *partial views*, according to which the following strategies can be used to propagate messages throughout the network:

- **Eager push:** This is the traditional approach, where once nodes receive a message they forward the message (together with its payload) to a random set of *d* peers.
- **Pull:** Nodes interested in a set of topics periodically send request messages to random nodes to inquire about newly received messages. If queried nodes have updates in the topic specified by the subscriber node, they forward the message to this node.
- **Lazy push:** When a node (from within the *d* group of another node) receives a message, it forwards a message identifier (i.e., *not the payload of the message*) to a number of random peers. If those nodes have not already received the message, they send a subsequent pull request to get the full payload of the message.

*Tradeoffs*

As usual with most communication systems, there are tradeoffs in the design choices, which affect the performance and resilience of the system. Inserting more traffic in the system (e.g., *eager push*) increases bandwidth requirements, but achieve higher reliability and lower latency. On the other hand, more relaxed approaches (e.g., *lazy push*) reduce both bandwidth requirements, but also load on end nodes.

Related Literature

- Epidemic Broadcast Trees, 2007, DOI: 10.1109/SRDS.2007.27, http://www.gsd.inesc-id.pt/~ler/docencia/rcs1617/papers/srds07.pdf
- HyParView: a membership protocol for reliable gossip-based broadcast, 2007, DOI: 10.1109/DSN.2007.56, http://asc.di.fct.unl.pt/~jleitao/pdf/dsn07-leitao.pdf
- GoCast: Gossip-enhanced Overlay Multicast for Fast and Dependable Group Communication, 2005, http://citeseerx.ist.psu.edu/viewdoc/download?doi=10.1.1.75.4811&rep=rep1&type=pdf
- Exposing and eliminating vulnerabilities to denial of service attacks in secure gossip-based multicast, DSN 2004
- Emergent structure in unstructured epidemic multicast, DSN 2007


### Within the libp2p Ecosystem
> Existing attempts and strategies

##### libp2p pubsub (floodsub, gossipsub & episub)

For a detailed description of the pubsub protocols currently used, or currently implemented in order to be used in the near future, please follow the links below. Here we provide a brief description of the three main versions of pubsub in libp2p. In particular, floodsub was initially implemented and used, but had poor scalability properties and bandwidth requirements. Gossipsub is currently been used and Episub is currently being implemented to improve performance by proximity aware epidemic broadcast.

- PubSub Implementation: https://github.com/libp2p/specs/blob/master/pubsub/README.md
- Floodsub and Gossipsub: https://github.com/libp2p/specs/blob/master/pubsub/gossipsub/README.md
- Episub: https://github.com/libp2p/specs/blob/master/pubsub/gossipsub/episub.md

The initial pubsub experiment in libp2p was floodsub. It implements pubsub in the most basic manner, with two defining aspects: i) ambient peer discovery; and ii)  most basic routing: flooding. In ambient peer discovery, the task of finding peers is not part of the protocol, but instead provided by external means, e.g., using the DHT. The system works well with floodsub when the network size is small. It can propagate messages with minimum latency, *but* it has got prohibitively high overehead.

Further experimentation resulted in what was called *randomsub*, according to which nodes are randomly forwarding messages to a subset of peers within a topic. This reduces a lot the amount of traffic, but is non-deterministic, resulting in route instability. *Meshsub* was constructed as an optimisation to *randomsub*. According to meshsub, instead of randomly selecting peers on a per message basis, meshsub forms an overlay mesh where each peer forwards to a subset of its peers on a *stable basis*. The overlay is initially constructed in a random fashion, based only on topics.

The *meshsub* router offers a baseline construction with good amplification control properties, which has been augmented with gossip about message flow, *aka gossipsub*. The gossip is emitted to random subsets of peers not in the mesh. Using gossip messages we can propagate metadata about message flow throughout the network. The metadata can be arbitrary, but as a baseline we include the message IDs that the router emitting the gossip has seen in the last few seconds. The router can use this metadata to improve the mesh and create epidemic broadcast trees.

Essentially, *gossipsub is a blend of meshsub for data and randomsub for mesh metadata*. Gossipsub provides bounded degree and amplification factor with the *meshsub* construction and augments it using gossip propagation of metadata with the *randomsub* technique.

The optimisation to gossipsub, which is currently in implementation is *episub*, a protocol that implements epidemic broadcast trees, but inserts a proximity factor, which is expected to improve performance significantly.


##### peer-base collaboration messaging protocol (Dias Peer Set)

- https://github.com/peer-base/peer-base/blob/master/docs/PROTOCOL.md#peer-base---protocol-explanation

There has been some discussion around the peer-base collaboration protocol. This protocol is building collaboration hashrings between nodes in the P2P overlay. It implements a simple way of building a membership base through gossip messages and adding/removing nodes from the hashring. Then there is the collaboration part of the protocol, which is composed of a push and a pull mode of operation. When a node initiates a connection, it is "pushing" information, whereas when a node is receiving a connection request it is "pulling" information.

### Known shortcomings of existing solutions
> What are the limitations on those solutions?

The scale required by systems such as name registry propagation in IPNS as the IPFS network grows exponentially, or request routing in filecoin and transaction routing in ETH2.0 can be orders of magnitude higher than the systems tested in the past for unstructured, unmanaged P2P overlays.

*Performance Evaluation and Scalability*

The current implementation of pub/sub (i.e., gossipsub) has been shown to perform well and is expected to scale to millions of nodes. Gossipsub provides bounded degree and amplification factor with the *meshsub* construction and augments it using gossip propagation of metadata with the *randomsub* technique. A thorough performance evaluation is currently been carried out and results will be reported soon. Gossipsub has its own simulator, which can be found here: https://github.com/vyzo/gerbil-simsub.

However, it is not clear what are the properties and performance guarantees that gossipsub and episub can provide for: i) orders of magnitude higher nodes, ii) latency-sensitive applications (e.g., ETH2.0 and filecoin transaction data), iii) message re-ordering and its impact.

These are not necessarily known shortcomings, but rather unknown factors that have to be thoroughly tested.

## Solving this Open Problem

As mentioned earlier, there are several tradeoffs at play in the design of the system. Those tradeoffs are made more serious as scalability requirements come into the picture, that is, as the protocol is requested to serve orders of magnitude more users and more pubsub topics. Below, we provide a very brief description of **the main challenges and requirements** that a sophisticated pubsub protocol needs to be able to deal with.

- *Load-balancing:* Keeping membership state and forwarding pubsub messages is loading both the memory and communication/networking requirements of a node. This is especially so for p2p systems, where end-nodes are not necessarily powerful servers. Furthermore, as some content is becoming popular, more load is put on the nodes that are relaying those messages. *A sophisticated (gossiping) pubsub protocol needs to be able to balance load among nodes.*

- *Latency:* Some applications require that messages are delivered to all nodes subscribed to a topic with the least possible delay. As pubsub systems are built as overlays on top of the physical Internet infrastructure, the underlying hop-count does not necessarily correspond to the overlay picture. Furthermore, approaches such as "eager-push" or "flooding" can reduce the delivery latency, but increase bandwidth requirements. *Latency to inform subscribed nodes should be bounded to acceptable levels, which are defined by the applications running on top.*

- *Authentication:* Whether a pubsub system is open to the public or not, there needs to be some authentication to those that publish to specific topics/channels. As such, there has been discussion (e.g., in https://github.com/ipfs/notes/issues/236) about a pubsub authentication API. According to this, every topic is signed by a public key. Anyone can subscribe to this key, but those that want to publish information to this key/topic need to sign the content with the corresponding private key. In case of a private pubsub system, content can be encrypted and the corresponding keys to decrypt the content should be shared with those that are allowed access to the topics. *Content published in pubsub systems need to be authenticated and in case of a private pubsub system the content itself needs to be encrypted using authenticated encryption.*

- *Scalability:* The ultimate issue that comprises a challenge of its own is to be able to scale up and support orders of magnitude more users/nodes. As more nodes join the system, both bandwidth and networking resources increase accordingly. That said, the scalability challenge encompasses all of the issues discussed above. *A sophisticated pubsub system should be able to support orders of magnitude more nodes, but at the same time take care of load-balancing between nodes and latency requirements of corresponding applications.*

- *Node Churn:* A significant challenge in unstructured, unmanaged P2P overlay networks is the fact that nodes are not dedicated to the overlay, but rather can join and leave the network at random points. Node churn can impact significantly the system performance and can have catastrophic events if it is extensive. *We expect that an algorithm that can sustain a 30% threshold of node churn will be able to meet application requirements.*

### What is the impact

As the IPFS network grows and dependency on underlying libp2p (and supporting protocols) intensifies, we need to make sure that the design of the protocols is able to scale up and maintain performance. The libp2p pub/sub system is increasingly being adopted by several applications, many of them with strict requirements including latency guarantees. Failure to deal with the above-mentioned challenges and meet the requirements can render whole systems unusable or risky. This is especially so for applications that transfer monetary value (e.g., ETH2.0 and filecoin).


  ### What defines a complete solution?
  > What hard constraints should it obey? Are there additional soft constraints that a solution would ideally obey?


  In recent years, there has been significant momentum for design and deployment of decentralised Internet services and applications. Among others, such services include distributed and decentralised storage but also computation systems. The aim is to replace, or complement traditional centrally managed and operated cloud services. End-users are contributing part of their resources to the network and get rewarded according to contribution. These emerging systems are distributed in the sense of geographical spread and decentralised from the point of view of ownership, management and operation.

  We are, therefore, witnessing a trend towards building P2P overlays, where, in most cases, unreliable and non-dedicated end-user devices are active contributors to the network. In the absence of central control,  messaging in those systems is of utmost importance in order to communicate operational processes (e.g., find file or execute function), but also propagate management events.

  Pub/Sub has seen a surge in usage from distributed applications in the area of decentralised services, such as distributed chat and social networks (e.g., Mastodon), collaborative editing tools without a backend server (e.g., PeerPad), hosting of dynamic website content in unmanaged P2P networks (e.g., websites on IPFS), video-on-demand platforms (e.g., D-Tube), storage and synchronisation of evolving datasets (e.g., transaction data in distributed ledgers), to name a few.

  Those systems are growing exponentially and if they reach the adoption of their current (centralised) alternatives, they will have user-bases of tens or even hundreds of millions of users. Reputation algorithms for stable nodes can act as an optimisation to the fact that these networks are unmanaged, but distributed reputation systems (and the security therein) has traditionally been a notorious challenge to overcome.


## Other

Here are some other systems that use pub/sub:

https://www.w3.org/TR/activitypub https://activitypub.rocks/
https://xmpp.org/extensions/xep-0060.html
https://github.com/tootsuite/mastodon
https://www.scuttlebutt.nz/ (uses a gossip strategy)

### Existing Conversations/Threads

- [Authenticated PubSub](https://github.com/ipfs/notes/issues/236)
- [pub/sub - publish / subscribe](https://github.com/ipfs/notes/issues/64)
- [Messaging on IPFS](https://github.com/ipfs/notes/issues/33)
- [PubSub notes from the IPFS Workshop - recursion and corecursion, PubSub API and Self Certified Streams](https://github.com/ipfs/notes/issues/154)
- [PulsarCast M.Sc Thesis - Scaling libp2p PubSub](https://github.com/ipfs/notes/issues/266)

### Extra notes
