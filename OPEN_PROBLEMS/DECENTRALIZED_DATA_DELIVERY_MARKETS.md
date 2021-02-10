# Decentralized Data Delivery Markets (3DMs) - Open Problem Statement

DO NOT MERGE WITHOUT CREATING DOCTOC https://www.npmjs.com/package/doctoc

# Overall

## Short description

With the emergence of Decentralized Storage Networks and the rapid decrease in the price of storage services and hardware, there is a rapidly growing need to leverage the additional storage capacity contributed to Decentralized Storage Networks by several new players, such as end-users and use it to deliver a reliable and high quality storage but also delivery services. Similarly to Content Delivery Networks (CDNs) for the traditional Cloud Storage market, we now have the opportunity to build **Decentralised CDNs **for Decentralised Storage Networks. The Decentralized Data Delivery Markets (3DMs) Open Problem covers all the essential areas of work that need to be studied in order to create **a fully permissionless free market for data delivery that supports fair data exchange** on the service provided.

## Long description 

Serving content globally at scale is a hard technical problem as evidenced by the multiple decades of innovation and improvements in the Content Delivery Networks field. This challenge becomes even more interesting once we move away from centralized cloud infrastructures, which is typically managed and monitored by a single party, to a decentralized network that is permissionless, likely anonymous, constantly changing (i.e. high node churn) and without access to the convenience of a third party mediator system that facilitates the fair exchange of goods (e.g. credit providers). The benefits of moving to a decentralised setting are significant, however: cheaper storage and delivery services, resilience against business failures (i.e., the network and all its components are not dependent on business decisions made by a single entity), moving away from the personal data-based business models, as well as a significantly lower barrier to entry for new players who want to contribute to the system.

At the same time, common problems addressed in the past, in traditional CDNs, reappear, albeit in a different dimension as we move to a decentralized network design space. Some of these problems are: the global orchestration of the system, the efficient allocation and use of the resources, or a clear visibility of the content being retrieved from the network.

We introduce the field of Decentralized Data Delivery Markets (3DMs) to the world as a new field of research and innovation and one that has a rapidly increasing necessity of existence.  Decentralised storage networks, such as Filecoin, have reached unprecedented storage capacity commitments (of over 2.5EB of cold storage in Jan 2021) and continue to grow rapidly. There is an urgent need to complement decentralised storage with decentralised data delivery as these storage networks are seeking for a solution to deliver the data stored in their network to their end users, while meeting the expectations that end-users are used to with the centralised services of today.

We envision a 3DM as being:

*   A permissionless and open data delivery market, composed of one or more crypto-economic models
*   A fair exchange relationship between service providers and users
*   A cost efficient way for publishers to distribute their content
*   A unique set of business opportunities including:
    *   Delivering data to end users
    *   Carrying data between multiple disconnected locations or with high latency between them (Data Muling and/or Package Switching)
    *   Create ephemeral distribution networks for large gatherings
    *   Power the next generation of Web 3.0 applications

This new field is ripe with unique business opportunities given the growing demand from users for access to large datasets, videos and so on, the panoply of super-powered mobile devices that end users have access to and the limitations imposed by physics in ultimately moving increasingly large data at high speeds through fiber.

In this Open Problem definition, we introduce  the multiple areas of research (or subOpenProblems), what we know so far for each one of them and link to some new ideas being explored.

### Definition of the actors

For convenience and shared understanding, we define here the agents within a 3DM network. These are:

*   **Clients** - Fetching data from Providers
*   **Providers** - Offer the service of data delivery to clients
*   **Content Publishers** - The creators of content (or intermediaries) that want to have content distributed. There might be situations in which Content Publishers are willing to pay for the service consumed by Clients


### Expected requirements

We present this list as a guide to protocol designers of what we expect a 3DM implementation to meet. This list is non-exhaustive and presents some flexibility between MUST and SHOULD. The requirements identified are:

*   **_MUST be Decentralized and not just federated_**
    *   Anyone should be able to join and leave at any time, without requiring permission
*   **_The exchanges of value_** **_MUST be verifiable_** 
    *   The payment for bandwidth/latency in token should match what has been agreed and fulfilled
    *   The payment should be fulfilled if the SLA is fulfilled
    *   Parties should be able to verify that the service provided was correct (e.g. received the right file)
*   **_SHOULD be Trustless_**
    *   Ideally, the network can perform the operations without having to leverage Trust in third parties and/or the participants of the exchange
    *   Nevertheless, Trust can, and should, be considered as many designs might benefit from an element of Trust to increase the quality of service (e.g. reputation supports relaxing constraints)
*   **_The network SHOULD do resource allocation in an efficient way_** (or as efficiently as possible)
    *   Should not be an afterthought process
*   **_Providers SHOULD coordinate and accept pre-fetching of content (warm up the catches)_**
    *   This is in contrast to IPFS’s core principle of replicating only after explicit request by a peer
    *   This is done in order to make the network more efficient. However it is not mandatory with the setting being left up to the Provider.

Additional expectations (i.e. bonus points):

*   Data SHOULD be readily available without having to wait for the [unsealing process](https://spec.filecoin.io/#section-glossary.seal) at storage nodes (e.g. Filecoin Storage Miners).
*   Data retrieval SHOULD be anonymous, privacy-preserving.

Other considerations (not requirements but good to think about):

*   It is expected that content will always be identified and retrieved by CIDs
*   The provider network will likely not provide random access to files, i.e., items will be named at the object level.
*   In case there is an auction market, it should always be available (Liveness)
    *   There needs to be a clever and fast way to match offers to bids

# Areas of Work

Throughout the exploration of the design space, we identified 3 (+1 bonus opportunity) areas of work that need to be explored in order to make 3DMs Fair, Decentralized and Efficient. The areas are listed below.

## Data Delivery Metering & Fair Exchange

We believe that this area of work is where the main crux for Decentralized Data Delivery Markets come to reality. Without it (i.e. without relying on a verified mediator to serve as escrow and referee), it will be impossible to have a fully permissionless, decentralized and open market for data delivery.

#### How it is done traditionally 

In traditional or Web 2.0 setups (e.g. CDNs, Mirrors, Static Servers and so on), the correct metering happens on the server side and it is done by the provider which the clients trust to do the right measurement. This measurement translates into a statement of how much the service got used and a respective invoice and request for payment is issued. This payment is facilitated by a trusted third party (e.g. credit card company) and a legal contract that can be used to resolve disputes in case the client fails to pay to the provider and/or the provider fails to deliver the service promised to the client. 

In a decentralized environment that strives to be permissionless, we can’t expect to rely on such legal agreements and third parties and so, we essentially need a way to prove fair exchange. That is, we need to be able to prove that the service was delivered correctly and the metering of the provided service was done correctly. This will, in turn, verify the exchange between the two parties and issue the correct payment (i.e. fair exchange). 

This trustless property is really important as we know that markets with no mediators, escrows and other assurance services (e.g. underground markets) are prone to scams, as the users have to put a lot of trust that the provider will execute on the agreement. In a trustless environment, once the transfer is done, there is no way to contest.

#### Properties desired

*   The exchanges of value MUST be verifiable and correct
    *   Fairness: 
        *   The payment MUST be fulfilled if the SLA is fulfilled
        *   The payment for bandwidth/latency in token SHOULD match what has been agreed and fulfilled
    *   Verifiability:
        *   Both parties MUST be capable of verifying that the exchange was performed correctly
        *   Bonus: Anyone SHOULD be able to verify that the exchange was performed correctly
*   SHOULD be Trustless
    *   Ideally, the network can perform the operations without having to leverage Trust
    *   Nevertheless in many designs it might be needed to leverage reputation as a trust vehicle to guarantee high quality of service and/or reliability.

#### How is Data Delivery Metering different from Fair Exchange

The field of Fair Exchange overlaps in part with Service Metering. Some notable differences are:

*   Fair Exchange can be used for non-digital goods also
*   Fair Exchange is traditionally between 2 parties
*   Metering should be available to properly measure the service quality and service provided by one or more parties (e.g. streaming from multiple endpoints)

In review, the Metering & Fair Exchange fields end up contributing to each other and it is likely that the intersection of both is needed for a sound solution. 

#### Known challenges

*   Fairness
  *   Making it Trustless
    *   How to verify that the provider has the ability to delivery the file (e.g. stored locally or that can pull it from the network)
    *   How to verify that the file being transferred is the one requested, before a large amount of resources has been invested (i.e., before the entirety of the file has been transmitted)
    *   How to verify that the client has received the file
  *   How to verify and reward that SLA were guaranteed?
  *   How to avoid collusion when adding a third-party (e.g. Referee)?
  *   How to avoid grieving attacks (make one party pay for fees)?
  *   How to avoid a malicious actor causing un-rewarded work, hence wasting the others’ resources?
*   Experience
  *   How to make the transfers start instantaneously?
  *   How to support others (e.g. Content Publishers) paying for the usage?
  *    How to make it private (e.g. that others don’t know who is requesting what)?
*   Performance
  *   How to overcome the send-and-halt in order to max bandwidth throughput 
      *   How to support multipath (i.e. fetching from multiple sources)

### State-of-the-art

When it comes to the state of the art, we found that the existing solutions fall within one of the following categories:

#### Pay-per-packet

INSERT DESCRIPTION

[(David) Original Filecoin payment channels](https://docs.google.com/document/d/1hXTP0VRmQsSFcnbxk3Z4sVmdW0CPajogJ6SXTe7zJ34/edit#heading=h.pc4bfmnw2ke0)	[4](https://docs.google.com/document/d/1hXTP0VRmQsSFcnbxk3Z4sVmdW0CPajogJ6SXTe7zJ34/edit#heading=h.pc4bfmnw2ke0)
[(David) Theta Whitepaper](https://docs.google.com/document/d/1hXTP0VRmQsSFcnbxk3Z4sVmdW0CPajogJ6SXTe7zJ34/edit#heading=h.82adp2bb1rys)	[4](https://docs.google.com/document/d/1hXTP0VRmQsSFcnbxk3Z4sVmdW0CPajogJ6SXTe7zJ34/edit#heading=h.82adp2bb1rys)

**Known shortcomings of these approaches:**
*   TOFILL


#### Lock/Unlock access to the resource

INSERT DESCRIPTION

[(David) Solving the Buyer and Seller’s Dilemma: A Dual-Deposit Escrow Smart Contract for Provably Cheat-Proof Delivery and Payment for a Digital Good without a Trusted Mediator](https://docs.google.com/document/d/1hXTP0VRmQsSFcnbxk3Z4sVmdW0CPajogJ6SXTe7zJ34/edit#heading=h.nnf8jfjvpmgc)	[4](https://docs.google.com/document/d/1hXTP0VRmQsSFcnbxk3Z4sVmdW0CPajogJ6SXTe7zJ34/edit#heading=h.nnf8jfjvpmgc)
[(David+Alfonso) FairSwap: How to fairly exchange digital goods](https://docs.google.com/document/d/1hXTP0VRmQsSFcnbxk3Z4sVmdW0CPajogJ6SXTe7zJ34/edit#heading=h.cbnzknlpc633)	[5](https://docs.google.com/document/d/1hXTP0VRmQsSFcnbxk3Z4sVmdW0CPajogJ6SXTe7zJ34/edit#heading=h.cbnzknlpc633)
(David) Zero-Knowledge Contingent Payments Revisited: Attacks and Payments for Services	[5](https://docs.google.com/document/d/1hXTP0VRmQsSFcnbxk3Z4sVmdW0CPajogJ6SXTe7zJ34/edit#heading=h.it3yajc2rdz0)

**Known shortcomings of these approaches:**
*   TOFILL

#### Optimistic Fair Exchange

INSERT DESCRIPTION

[(David) OptiSwap: Fast Optimistic Fair Exchange](https://docs.google.com/document/d/1hXTP0VRmQsSFcnbxk3Z4sVmdW0CPajogJ6SXTe7zJ34/edit#heading=h.f2mby9dpar4f)	[6](https://docs.google.com/document/d/1hXTP0VRmQsSFcnbxk3Z4sVmdW0CPajogJ6SXTe7zJ34/edit#heading=h.f2mby9dpar4f)
[(Irene) Asynchronous Protocols for Optimistic Fair Exchange](https://docs.google.com/document/d/1hXTP0VRmQsSFcnbxk3Z4sVmdW0CPajogJ6SXTe7zJ34/edit#heading=h.6ac4zrrdyhqs)	[6](https://docs.google.com/document/d/1hXTP0VRmQsSFcnbxk3Z4sVmdW0CPajogJ6SXTe7zJ34/edit#heading=h.6ac4zrrdyhqs)
[(Irene) Optimistic Fair Exchange with Multiple Arbiters](https://docs.google.com/document/d/1hXTP0VRmQsSFcnbxk3Z4sVmdW0CPajogJ6SXTe7zJ34/edit#heading=h.sjnrx6peoi0w)	[7](https://docs.google.com/document/d/1hXTP0VRmQsSFcnbxk3Z4sVmdW0CPajogJ6SXTe7zJ34/edit#heading=h.sjnrx6peoi0w)

**Known shortcomings of these approaches:**
*   TOFILL

#### Reputation based

INSERT DESCRIPTION

[(David) Proof of Delivery in a Trustless Network](https://docs.google.com/document/d/1hXTP0VRmQsSFcnbxk3Z4sVmdW0CPajogJ6SXTe7zJ34/edit#heading=h.g219mq8bs8ad)	[7](https://docs.google.com/document/d/1hXTP0VRmQsSFcnbxk3Z4sVmdW0CPajogJ6SXTe7zJ34/edit#heading=h.g219mq8bs8ad)
[(David) Proof-of-Prestige: A Useful Work Reward System for Unverifiable Tasks](https://docs.google.com/document/d/1hXTP0VRmQsSFcnbxk3Z4sVmdW0CPajogJ6SXTe7zJ34/edit#heading=h.9sqsu17nv29v)	[8](https://docs.google.com/document/d/1hXTP0VRmQsSFcnbxk3Z4sVmdW0CPajogJ6SXTe7zJ34/edit#heading=h.9sqsu17nv29v)
[(David) Mechanisms for Outsourcing Computation via a Decentralized Market](https://docs.google.com/document/d/1hXTP0VRmQsSFcnbxk3Z4sVmdW0CPajogJ6SXTe7zJ34/edit#heading=h.8lcwzhd2jj73)	[9](https://docs.google.com/document/d/1hXTP0VRmQsSFcnbxk3Z4sVmdW0CPajogJ6SXTe7zJ34/edit#heading=h.8lcwzhd2jj73)
[(Irene) Gradual Fair Exchange Class of Constructions](https://docs.google.com/document/d/1hXTP0VRmQsSFcnbxk3Z4sVmdW0CPajogJ6SXTe7zJ34/edit#heading=h.nq0pu1otlwlb)	[10](https://docs.google.com/document/d/1hXTP0VRmQsSFcnbxk3Z4sVmdW0CPajogJ6SXTe7zJ34/edit#heading=h.nq0pu1otlwlb)

**Known shortcomings of these approaches:**
*   TOFILL

#### Privacy focused

INSERT DESCRIPTION

[(David) Blockchain based Privacy-Preserving Software Updates with Proof-of-Delivery for Internet of Things](https://docs.google.com/document/d/1hXTP0VRmQsSFcnbxk3Z4sVmdW0CPajogJ6SXTe7zJ34/edit#heading=h.3n5ydbvkkmoc)	[10](https://docs.google.com/document/d/1hXTP0VRmQsSFcnbxk3Z4sVmdW0CPajogJ6SXTe7zJ34/edit#heading=h.3n5ydbvkkmoc)
[(David) SilentDelivery: Practical Timed-delivery of Private Information using Smart Contracts](https://docs.google.com/document/d/1hXTP0VRmQsSFcnbxk3Z4sVmdW0CPajogJ6SXTe7zJ34/edit#heading=h.2q9trcjswmyw)	[10](https://docs.google.com/document/d/1hXTP0VRmQsSFcnbxk3Z4sVmdW0CPajogJ6SXTe7zJ34/edit#heading=h.2q9trcjswmyw)
[(David) Bulletproofs+: Shorter Proofs for Privacy-Enhanced Distributed Ledger](https://docs.google.com/document/d/1hXTP0VRmQsSFcnbxk3Z4sVmdW0CPajogJ6SXTe7zJ34/edit#heading=h.bweiezm16dvh)	[10](https://docs.google.com/document/d/1hXTP0VRmQsSFcnbxk3Z4sVmdW0CPajogJ6SXTe7zJ34/edit#heading=h.bweiezm16dvh)

**Known shortcomings of these approaches:**
*   TBD

#### Traditional approaches (close to Web 2.0 world)

INSERT DESCRIPTION

[(David) The multimedia blockchain: a distributed and tamper-proof media transaction framework](https://docs.google.com/document/d/1hXTP0VRmQsSFcnbxk3Z4sVmdW0CPajogJ6SXTe7zJ34/edit#heading=h.tsv8bvhhclyh)	[10](https://docs.google.com/document/d/1hXTP0VRmQsSFcnbxk3Z4sVmdW0CPajogJ6SXTe7zJ34/edit#heading=h.tsv8bvhhclyh)
[(Alfonso) Reliable Client Accounting for P2P-Infrastructure Hybrids](https://docs.google.com/document/d/1hXTP0VRmQsSFcnbxk3Z4sVmdW0CPajogJ6SXTe7zJ34/edit#heading=h.mnagl72zaw3u)	[11](https://docs.google.com/document/d/1hXTP0VRmQsSFcnbxk3Z4sVmdW0CPajogJ6SXTe7zJ34/edit#heading=h.mnagl72zaw3u)

**Known shortcomings of these approaches:**
*   TOFILL

### Known attacks to be mitigated

Here we list the attacks to consider when designing a solution. These are:

*   Malicious actor forces provider to spend bandwidth without issuing payment (also known as Grieving Attack)
    *   Type: client fraud 
    *   Consequence: bandwidth-spent 
*   Malicious actor forces provider to pull the file from storage point (e.g. Filecoin, Cloud Storage, etc), paying the cost without the provider ever getting paid 
    *   Type: client fraud: 
    *   Consequence: currency is spent
*   Provider claims it has sent the file, but actually never did 
    *   Type: Provider fraud
    *   Consequence: Client gets penalized without receiving the service
*   Metering Inflation 
    *   Providers report to have used more resources to serve content to clients than what they have actually done.
*   Sybil / Throttle attacks
    *   Description: A set of nodes collude against a content-provider to try and make him go bankrupt (loss of all its “SLA stake”). They all try to download content from that content provider at the same time, so that Providers start consuming the stake from the content providers. If clients are not paying for the service we take the risk of enabling this attack. 

### New ideas being explored

ResNetLab organized a Research Intensive Workshop on 3DMs, out of which, the following ideas, structured as RFCs emerged:

* Link to RFCs:
  * TOFILL

## Distribution Graph Forming

The area of graph forming for Decentralised Data Delivery Markets revolves around three main areas, which simultaneously compose the different goals of the Graph Forming problem:

*   **Content discovery & routing:** the system can forward requests to find content
*   **Content placement:** the system should have ways to proactively distribute/replicate copies of data across different Providers in different geographic regions in order to be able to serve content fast.
*   **Content copy selection:** the system can choose the optimal copy of the content to serve if multiple copies exist in several Providers, where potentially:
    *   every copy has a different “asking” price (defined by the cryptoeconomic model) and latency to deliver the content item, and
    *   every miner achieves different performance and has a different reputation profile.

In order to achieve the above goals the system needs to have an architectural structure, that is, where do end-users connect (e.g., to a retrieval miner, or to a separate content resolution system), which entity in the architecture makes content resolution and request forwarding decisions and finally, which entity(ies) has(ve) the required knowledge to forward requests to the closest (to the requesting user) copy, _aka_ nearest replica routing.

We generally consider that retrieval miners are relatively powerful devices with local storage dedicated to storing hot copies of the content, high uptime (close to zero churn) and ideally public IP (i.e., reachable from anywhere). The architecture should ideally be able to take advantage of storage in less-powerful and more ephemerally-connected end-user devices, such as laptops and mobile phones - this is discussed as a different area further down. Although not a strict requirement, this is what will make the decentralised storage network take the full benefit of planetary-scale unused storage.

It is worth highlighting that the system should be able to serve different use-cases and therefore different setups and architectures could apply depending on the use-case. For instance, the content resolution system can be different between the cases where: i) the application operates based on a closed-content ecosystem (e.g., subscriber-based music or video streaming, where control of the content is solely with the content publisher) and ii) the application operates on a totally open content space, e.g., web.

Last but not least, the system of overlay retrieval miner nodes has to be permissionless and decentralised. No entity has full control of the network of nodes, as is the case with traditional CDNs, where a single entity is in charge of the network setup and the content served by the system.

How it is done traditionally (CDNs, P2P CDNs)

Traditionally, content is stored, hosted, replicated and delivered by centralised CDNs. In those setups, **the CDN entity has full control over: i) the formation of the network**, e.g., where to place servers and how to interconnect them, both in terms of topology and in terms of bandwidth deployed between servers, **ii) the placement and replication of content**, i.e., where to put hot copies of content, as well as **iii) the network elements that are responsible for content resolution**.

In a centralized setup it is much easier to optimise performance and provide guarantees, compared to the case where **every network node is operated by a separate (legal) entity that operates rationally in a profit-maximizing way**.

Closer to our setup are P2P CDNs, where end-user equipment is “employed” to contribute to the storage and dissemination of content. Node churn, unreliable connectivity and low bandwidth speeds become a reality in P2P CDNs, but **again the setup of P2P CDNs is controlled and monitored by a single entity/company.** This makes decisions with regard to content placement and replication easier. The most important advantage of a single entity taking care of the P2P CDN setup is **monitoring**, that is, being able to observe the performance of peers and make decisions on where and when to replicate content or assign content distribution decisions.

Properties

*   The system MUST always be able to discover content and satisfy content requests
    *   There should never be a “404 Content not Found” error.
*   The system MUST replicate content to different storage points in order to reduce delivery times and maximize performance.
*   Providers MUST follow the crypto-economic model and the system MUST make sure that Providers do not misbehave.
*   The network MUST be a content-addressable network and operate based on content identifiers.
*   The system MUST be permissionless and trustless
    *   Anyone should be free to join and set up a retrieval miner to contribute to the network.


### State-of-the-art

The target area and expected outcome of this topic is to form the network, the interactions between different network entities, as well as the basic protocols through which network entities will communicate. Given this target, we split our investigation in the following three main areas:

**P2P CDNs:** the network architecture setup in these systems is similar in some ways to that of a 3DM

*   LiveSky: Enhancing CDN with P2P
*   P2P as a CDN: A new service model for file sharing (2012)
*   OblivP2P: An Oblivious Peer-to-Peer Content Sharing System

**P2P VoD:** video delivery is an ideal target use-case, with very strict requirements; therefore, the mechanisms proposed in this field and used for P2P VoD can be inspiring

*   Challenges, Design and Analysis of a Large-scale P2P-VoD System
*   A Framework for Lazy Replication in P2P VoD
*   InstantLeap: Fast Neighbor Discovery in P2P VoD Streaming (2008)
*   Proactive Video Push for Optimizing Bandwidth Consumption in Hybrid CDN-P2P VoD Systems

**Content-Centric Networking:** there are several network architectures and protocols proposed for content-/information-centric networks which can be great inspiration for a permissionless, content-addressable P2P network.

*   Named Data Networking (NDN)
*   iCDN - An NDN based CDN
*   Analysis and Improvement of Name-based Packet Forwarding over Flat ID Network Architectures (2018)
*   High Throughput Forwarding for ICN with Descriptors and Locators (2016)

Our initial, but thorough investigation resulted in the following potential groups of designs for content discovery and resolution:

#### DHT-based: 

DHTs are very popular constructions in P2P networks. The DHT system is used as a content resolution, as well as a content and peer routing system. The routing table is split between peers that participate in the DHT, providing higher resilience and scalability. In a DHT-based system, clients “walk”  the DHT (iteratively, or recursively) to find the provider record and then directly connect to the peer included in the provider record.

*   **Pros:** tested in the past, engineers have lots of experience with these structures, can become faster and less expensive than the IPFS DHT setup, if: i) re-publishing is done at coarser granularities, which is reasonable if we assume stable connectivity, i.e., low peer churn (reasonable to assume for Providers), and ii) some payment is associated with publishing. It is also reasonable to assume public IP connectivity for Providers.
*   **Cons:** slow, several round-trips needed to resolve content (especially in case of iterative DHT lookup). The solution will not scale in the longer term.

#### Name-based routing: 

In Name-based routing systems, routing hints are integrated as part of the content name; routing tables are filled with routing hints at network setup time by a routing protocol. Routers make hop-by-hop forwarding decisions based on content names seen in requests and routing hints in their routing tables.. Matching between hints and names depend on the structure of the names, e.g., hierarchical vs flat.

*   **Pros:** very fast, enables multicast-like opportunities and on-path caching, which can result in significant savings in case of popular and heavy content (e.g., HD VoD). Also makes routing from browser/limited connectivity clients more feasible.
*   **Cons:** design can be(come) complex, security properties not fully studied in the literature.

#### DNS-style, object-level indexing service:

The Domain Name System (DNS) uses index tables to resolve names (URLs) to IP addresses of hosts that store the requested name. It is considered by many to be a decentralised system, in the sense that DNS servers can be deployed by anyone running an ISP. At the top-level, however, it is administered by [ICANN](https://www.icann.org/) which is in charge of assigning names and therefore, does not comply with the permissionless nature of 3DMs. We could envision a similar, index-based naming system, which is governed by the [Ethereum Name Service](https://docs.ens.domains/) (ENS) to bypass centralization concerns. Cloudflare has recently introduced a [Name Resolver for the Distributed Web](https://blog.cloudflare.com/cloudflare-distributed-web-resolver/), where IPFS content can be accessed through ENS.

*   **Pros:** can be very fast
*   **Cons:** need to rely on ENS (which is fine given the ENS system is deployed and tested in the wild, albeit in smaller scales), or build a similar service

#### PubSub-style:

In pubsub systems information propagates in the network based on topics that nodes subscribe to. Notifications about new content being published in the system can propagate through a dedicated channel, or more realistically, applications can have their own topics to disseminate information about content published within the remit of their application.

*   **Pros:** simple approach, lots of literature and testing in dozens of systems, engineers have lots of experience with similar protocols
*   **Cons:** too slow for real-time content delivery, can take seconds to find content, can’t scale in the longer term

### Known attacks to be mitigated

*   Providers provide false content discovery and routing information
*   Providers serve bogus content
*   Providers provide false provider records (i.e., they claim that they have and can serve content that they actually do not have)
*   Cache poisoning
*   Privacy attacks: there is a bunch of privacy-related to be taken into account 

### New ideas being explored

ResNetLab organized a Research Intensive Workshop on 3DMs, out of it, the following ideas, structured as RFCs emerged:

*   Link to RFCs TO FILL

## CryptoEconomic model for Data Delivery

The aim of the cryptoeconomic model is to build an incentive system to ensure that all actors in the network are aligned towards the same goal: to deliver data efficiently. The cryptoeconomic model is tightly coupled to all of the areas presented above, offering a way to incentivize desired behaviors in the network.  Ideally, the economic model should offer the substrate to build a decentralized and trustless infrastructure for data delivery able to compete against traditional CDN infrastructures in terms of cost, scalability, and price.

#### How it is done traditionally 

In traditional and centralized setups, there is no underlying need to align the incentives of all the participants in a data delivery network. The relationships between entities in the system are governed by a set of multilateral legal agreements between the different parties. These agreements offer the basic level of trust to ensure the good behavior of all entities in the system (clearly stating the SLA, and punishments for misbehaving or not fulfilling the contract). We currently see this setup in the multilateral agreements between CDNs, ISPs, and Cloud Providers to share resources from their infrastructures for a certain price in order to build their network of relationships that allow them to provide their content delivery services efficiently.

In decentralized setups, it is not possible to use legal agreements to orchestrate the relationship between entities in the system. In recent deployments of hybrid CDNs, traditional, centralised CDNs leverage the resources of end users to deliver data with enhanced QoS reducing the load on their infrastructure and as a result the cost of upgrading/extending their infrastructure. End users are incentivized to contribute to the system with their upload bandwidth by making them eligible for enjoying a better service. This additional reward is used to incentivize a behavior in the system (in this case users contributing with bandwidth in hybrid CDNs). However, the fact that there is still a centralized infrastructure managed by an entity with a greater power over the rest of actors in the system is enough to prevent misbehaviors. This removes the need to design stronger reputation or reward systems to prevent misbehaviors. This does not represent a trustless setup.

For decentralized and trustless setups, two main schemes have been traditionally used to align the incentive of the system’s participants and prevent misbehaviors:

*   Trustless networks governed by a common consensus, like public blockchains, use economic models based on token rewards and clear punishments built upon a secure consensus algorithm. The consensus algorithm gives a basic layer of trust where all participants “keep an eye on one another” preventing misbehaviors, and upon which the economic model is built to align all their goals. A good example of this model is Bitcoin, where the Proof of Work consensus algorithm prevents attacks, and the mining rewards and deflationary nature of the currency ensures that Providers will be incentivized to keep serving, and users to keep using and holding the network’s currency.
*   For trustless P2P networks where there is no common protocol run by all the participants of the network, other schemes such as reputation systems, or credit networks need to be implemented to orchestrate the relationships and good behaviors of all entities in the system.

In the same way public blockchains leverage the schemes and security properties in place to design their economic model, 3DMs will have to leverage the metering and graph forming designs in place to design the right economic model.

Finally, auction markets and game theoretical models have been traditionally used to study and achieve efficient resource allocation in P2P networks and decentralized systems.

#### Properties desired

*   The model MUST foster fair competition and avoid the creation of monopolies.
    *   We should prevent superlinear advantages from winning/completing  a high percentage of retrieval deals in the system.
*   The model SHOULD discourage, prevent, and punish misbehaviors.
    *   Or the other way around, good behaving entities should be rewarded by the model.
    *   In its different layers, the system will include schemes to prevent attacks in the network (client metering, sybil attacks, data-ransoming, etc.). However, this economic model should disincentivize these kinds of misbehavior in advance (from a game theoretical approach, the system should target a Nash Equilibrium in the system where misbehaviors are disincentivized).
    *   It should also avoid “malicious economic attacks” such as a content provider trying to bankrupt its competition by draining the stake of his retrieval deal.
*   The model SHOULD foster collaboration between entities to benefit users perceived QoS / QoE. 
    *   The model should reward entities that collaborate to serve content efficiently and with high QoS to users.
    *   This will also result in an efficient allocation of resources in the network.
*   If any part of the economics of the system ends up being orchestrated by an auction market, this market MUST be always available.
    *   And should match offers to bids in a fast and efficient way.

### State-of-the-art

As described above, the outcome of this topic should be to design an economic model that aligns the goals of all profit-maximizing entities in the system while preventing misbehaviors. Given this target, we split our investigation in the following areas:

##### Economics of Hybrid CDNs and P2P networks

The economics of current Hybrid CDN deployments give a good understanding of the system and cost models involved in the delivery of content at scale. This, added to the different analysis and design proposals of economic models for content delivery in P2P networks represent a good foundation to understand the complexity of the problem and potential ways to tackle it.

**_Related Papers:_**

*   An economic mechanism for request routing and resource allocation in hybrid CDN–P2P networks
*   An economic replica placement mechanism for streaming content distribution in Hybrid CDN-P2P networks
*   How Neutral is a CDN? An Economic Approach
*   Value Networks and Two-Sided Markets of Internet Content Delivery
*   Bilateral and Multilateral Exchanges for Peer-Assisted Content Distribution

**_Learnings: _**

*   Protocols and systems involved in the cost model of a Hybrid-CDN.
*   Models of P2P content delivery networks.

##### Game theoretical models and reward/reputation systems in P2P networks

How to design the right incentive and reputation models to incentivize certain behaviors in P2P networks has been a widely studied problem for some time now. From reputation systems where nodes track the level of good behavior of their peers and cooperate with them according to their grade; to incentive systems where good behavior is explicitly rewarded. To theoretically evaluate the strength of these schemes, they are generally mod game theoretical models are generally an objective way to evaluate the strength of these schemes, and to understand if the proposals can achieve Nash Equilibrium.

**Related Papers:**

*   Scrivener: Providing Incentives in Cooperative Content Distribution Systems
*   Proof-of-Prestige: A Useful Work Reward System for Unverifiable Tasks
*   Incentivizing Peer-Assisted Services:A Fluid Shapley Value Approach
*   Incentive and Service Differentiation in P2P Networks: A Game Theoretic Approach
*   On Incentivizing Caching for P2P-VoD Systems
*   Bar Gossip

**_Learnings: _**

*   Use different metrics to evaluate the level of good behavior of peers.
*   Examples of game theoretic models to approach the evaluation of these protocols.
*   Schemes to avoid attacks to which these systems can be vulnerable (such as sybil attacks).

##### Credit networks and Token Designs

Credit networks provide liquidity in markets where transaction volumes are close to balanced in both directions. They offer a way of performing payments and rewarding behaviors without the need of a common consensus or dedicated agreements between all the entities in the systems. Credit networks require no “a priori” trust relationships when they are backed by on-chain escrows, so they represent an interesting approach to tackle the economics of 3DMs. 

**_Related Papers:_**

*   Collusion-resilient Credit-based Reputations for Peer-to-peer Content Distribution
*   Liquidity in Credit Networks: A Little Trust Goes a Long Way
*   Liquidity in Credit Networks with Constrained Agents
*   CAPnet: A Defense Against Cache Accounting Attacks on Content Distribution Networks
*   MicroCash: Practical Concurrent Processing of Micropayments
*   Proof-of-Prestige: A Useful Work Reward System for Unverifiable Tasks

**_Learnings: _**

*   Economics of credit networks.
*   Using credit networks for fast payments and reputation systems. 
*   Credit networks for content distribution in P2P networks.

##### Auctions, decentralized markets, and efficient resource allocation

Auctions and decentralized markets have traditionally been a good way of allocating resources  efficiently in a decentralized manner. These papers analyze different proposals of auction systems and decentralized markets that can aid the design of efficient resource allocation in 3DMs.

**_Related Papers:_**

*   Edge-MAP: Auction Markets for Edge Resource Provisioning
*   A Market Protocol for Decentralized Task Allocation
*   On the Efficiency of Sharing Economy Networks
*   Resource Allocation in Market-based Grids Using a History-based Pricing Mechanism
*   Content Pricing in Peer-to-Peer Networks

**_Learnings: _**

*   Use of auctions as an efficient way to allocate resources in a decentralized system.
*   Local auction protocols. to drive the offer and demand of resources.
*   Decentralized market protocols for efficient pricing and distribution of resources.

### Known attacks to be mitigated

*   **Sybil attacks:** Actors trying to game the system by indiscriminately generating a large amount of entities and gaining large influence over the system (forging client retrievals, overestimating the use of resources of a provider, preventing access from some resource in the network, making actors in the system dedicate resources to useless work, etc.).
*   **Collusion attacks:** Several independent actors in the system colluding to perform attacks in order to gain large influence in the network. The type of attacks that can be performed with a collusion attack are similar to the ones performed in a sybil attack, but without having to generate “pseudonymous identities”. 
*   **Data ransoming:** This attack takes place when a provider agrees to serve some data from a publisher, but ends up preventing its retrieval from clients until a ransom is paid. An alternative form of this attack occurs when a provider is serving chunks of content to a client, and doesn’t deliver the last couple of chunks until it pays a ransom.
*   **Malicious economic attacks:** Attacks aimed at economically harming actors in the systems. For instance, clients trying to get a provider or content publisher bankrupt, providers offering abusive prices to content publishers, providers and clients colluding to avoid the economic rewards of target entities, etc.

### New ideas being explored

ResNetLab organized a Research Intensive Workshop on 3DMs, out of it, the following ideas, structured as RFCs emerged:

*   Link to RFCs TO FILL

## Additional: Opportunistic deployment

An area adjacent to the Distribution Graph Forming one is that of extending the network beyond Providers, as defined earlier, to also include more ephemerally-connected, end-user devices. Such devices can include laptops, desktops, (futuristic) storage-equipped WiFi Access Points, or even mobile smartphone and tablet devices.

We call these environments “opportunistic deployments” to reflect their unpredictability in terms of availability, uptime, resource quality and quantity. In contrast to RMs, as defined earlier that are expected to have stable, public and high-bandwidth connectivity, 3DM opportunistic deployments can utilise everyday user devices to extend the storage footprint of RMs and create a wealth of new network connectivity and business opportunities.

Although this area seems to sit on the periphery of 3DMs, it is actually a highly impactful area, as it realises the vision of Decentralised Storage Networks in general and Filecoin, in particular, of regular end-users sharing their own resources to contribute to the network. That said, we place high value in capturing this opportunity and offering to end-users the opportunity of being rewarded for their contribution to the network.

### State-of-the-art

The literature in the field of Opportunistic and Delay-Tolerant Networks is vast. We have narrowed down the scope of the literature that we reviewed and identified the following most-promising 3DM applicability areas.

#### Transient Providers

We consider end-users that store content in their ephemerally connected devices (laptops, or mobile phones) and provide them to the network through (one or more) Providers, as defined in the “Distribution Graph Forming” area. These service providers are called “Transient Providers (TRMs) to depict their ephemeral nature in resource availability and time. TRMs can have some economic relationship with one or more RMs, e.g., RMs can “recruit” TRMs to expand their storage capacity and footprint. In this case, RMs have to maintain their own monitoring mechanisms and do resource allocation to ensure that they are getting desirable levels of service from their group of TRMs.

Opportunistic D2D

This area of research and deployment is closer to traditional Delay-Tolerant Networks (DTNs). We envision applications where mobile devices contribute to the distribution of content in the mobile domain. These can be akin to previously proposed [User-Operated Mobile Content Distribution Networks](https://drive.google.com/file/d/1vvtQ7MJb4YFRXxeo2GewGh8EVYEqV-iJ/view?usp=sharing), or applications to realise concepts such as “[Floating Content](https://drive.google.com/file/d/1yOniHbAjLYv3q09pff7gnulOwT_NYCYG/view?usp=sharing)”. 

### Known shortcomings 

**User Mobility**

When a user is “registered” under one address/network location to serve content (at least as far as its Retrieval Miner is concerned) and then moves to another network location, the content is not possible to find anymore.

*   Brings up content mobility issues and “content churn”
*   Similar to node churn, this is a very tricky problem: how can you find content that was linked to some network address, when this address is not valid anymore. You can, of course, update the record if you know where the record lives, but if the record is “allowed” to be provided by anyone, then it’s difficult to even find it.
*   Simple solution: route through “home” gateway, similar to Mobile IP. Increases delay, is not ideal, but can work
*   There is a lot of work on “producer mobility” in the literature that we should look at.

**Privacy - Efficiency Tradeoff**

In mobile opportunistic communications, the two are often conflicting. On one extreme, you have efficient (low-replication) solutions that make use of a lot of node data (geolocation history/patterns, contact history/patterns, current geographic destination, current velocity vector). On the other extreme, you have naive epidemic dissemination solutions that require no node profiling but introduce data redundancy. There is a continuum in between, as well as a third variable, which is QoS: in the extreme, if willing to wait forever for delivery, you can blindly pass a message around without replication.

Keeping with the ethos of our projects, we believe that the most suitable solutions are those that don’t require sensitive data, including data related to location or contact history, from which identity and behaviour can be easily extracted. Aggregate transitive metrics (e.g. total expected time to destination measures) can provide a measure of delivery likelihood without disclosing the full contact vector or the nature of the contact (direct vs. indirect), and may be a useful compromise. Other data, including interests/subscriptions, is akin to what is already used e.g. in pubsub protocols and not considered to be any more privacy-invading.

**Security**

Like with any other distributed routing/forwarding approach, malicious nodes can interfere with message propagation and delivery. Because of the disconnected nature of the network, such attacks are harder (or at least slower) to detect and counteract. Secure-by-design approaches are likely to suffer from additional overhead (either in data or complexity) and _may_ be precocious when compared to the security model of other, more critical, parts of the network.

**Energy Efficiency and Battery Consumption**

Opportunistic communications that rely heavily (or exclusively) on user devices always come with the challenge of usability from the energy/battery perspective. Of course, incentives through FIL will alleviate some of it, but even then the incentive would have to be such that it overcomes the “running out of battery” shock. Techniques to overcome this can be as simple as disabling user participation when the battery level is below X%.

**Incentives**

The incentive to participate and the cryptoeconomic model behind the opportunistic part of the network is quite likely going to be incompatible with the one in the main Distribution Graph Forming area that applies to large/stable Providers. Nevertheless, incentives is a core part of this area and has traditionally been a stumbling block for the deployment of opportunistic networks.

### New ideas being explored

ResNetLab organized a Research Intensive Workshop on 3DMs, out of it, the following ideas, structured as RFCs emerged:

Link to RFCs
*   TO FILL