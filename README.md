<p align="center">
  <a href="https://research.protocol.ai/research/groups/resnetlab/" title="ResNetLab">
    <img src="https://research.protocol.ai/images/resnetlab/resnetlab_logo_blue.svg" width="244" />
  </a>
</p>

> A resilient system or network is fundamentally uncompromised by an isolated failure or network split. The system is malleable, adaptable to different conditions and capable of evolving to meet new requirements over time. That said, resilience here identifies also as a characteristic against changing system or network conditions, i.e., the system's core operating principles adjust so that *performance* remains when the system scales up to serve increasing demand from more users. **Building resilience into foundational infrastructure is the key in building a computing and networking fabric for human knowledge**.

**Welcome to the ResNetLab public repository. In this place, you will be able to find information about our Open Problems, RFPs, Open Positions, Research Areas, Research Projects and much more.** You can also consult the version on the [Protocol Labs Research website](https://research.protocol.ai/research/groups/resnetlab)

## Table of Contents

- [The ResNetLab](#theresnetlab)
  - [Mission & Vision](#mission--vision)
  - [Motivation & Description](#motivation--description)
- [Research](#research)
  - [Areas](#research)
  - [Projects](#research)
  - [Collaborations](#collaborations)
- [Collab projects tracking threads](#collab-projects-tracking-threads)
- [Publications, Talks & Trainings](#publications-talks--trainings)
- [Team](#team)
- [Contact](#contact)

## The `ResNetLab`

### Mission & Vision

<br/>
<p align="center">
  <strong>The Resilient Networks Lab mission</strong> is to <strong>build resilient distributed systems</strong>, by creating and operating a platform where researchers can <strong>collaborate openly and asynchronously on deep technical work.</strong>
</p>
<br/>

### Motivation & Description

The lab's genesis comes from a need present in the IPFS and libp2p projects to amp their research efforts to tackle the critical challenges of scaling up networks to planet scale and beyond. The Lab is designed to take ownership of the earlier stages on the research pipeline, from ideas to specs and to code.

![](https://research.protocol.ai/images/resnetlab/research-pipeline-map.png)

## Research

### Areas

<table>
  <tr>
    <th><b>Research Area</b></th>
    <th><b>Open Problem(s)</b></th>
    <th><b>Short Description</b></th>
  </tr>

  <tr>
    <th><b>Resilliency in Adversarial Networks</b></th>
    <th>NEEDs OPEN PROBLEM</th>
    <th></th>
  </tr>

  <tr>
    <th><b>Networks Observability</b></th>
    <th>NEEDs OPEN PROBLEM</th>
    <th></th>
  </tr>

  <tr>
    <th><b>Decentralized Data Delivery Markets</b></th>
    <td><b><a href="https://github.com/protocol/ResNetLab/blob/master/OPEN_PROBLEMS/DECENTRALIZED_DATA_DELIVERY_MARKETS.md">General open problem</a></b></td>
    <td> Content storage and delivery services have traditionally been under the ownership, control and management of centralised entities. Decentralised storage platforms are emerging and are in operation today. However, without actual delivery of content to users, these networks can be of little value, limited to serve as cold storage solutions only. With this Open Problem, we introduce the field of decentralised delivery, or decentralised CDNs to complement decentralised storage systems and come one step closer to the whole set of solutions that traditional CDNs offer today. </td>
  </tr>

  <tr>
    <th><b>Heterogeneous Runtimes</b></th>
    <td><b><a href="https://github.com/protocol/ResNetLab/blob/master/OPEN_PROBLEMS/HETEROGENEOUS_RUNTIMES.md">General open problem</a></b></td>
    <td>Making libp2p / IPFS modules and protocols the de-facto distributed substrate for connected devices in the near-future Internet. Enable the execution of libp2p nodes and its underlying protocols anywhere (any device and any runtime). Allow global connectivity between devices. Enable “offline-first” applications.
</td>
  </tr>

  <tr>
    <th><b>Content Addressing</b></th>
    <td><b><a href="https://github.com/protocol/ResNetLab/blob/master/OPEN_PROBLEMS/ROUTING_AT_SCALE.md">Routing at Scale (1M, 10M, 100M, 1B.. nodes)</a></b></td>
    <td>Content-addressable networks face the challenge of routing scalability, as the amount of addressable elements in the network rises by several orders of magnitude compared to the host-addressable Internet of today.</td>
  </tr>

  <tr>
    <th></th>
    <td><b><a href="https://github.com/protocol/ResNetLab/blob/master/OPEN_PROBLEMS/PRESERVE_USER_PRIVACY.md">Preseve full users' privacy when providing and fetching Content</a></b></td>
    <td>How to ensure that the user's of the IPFS network can collect and provide information while mainting their full anonymity.</td>
  </tr>

  <tr>
    <th><b>Mutability</b></th>
    <td><b><a href="https://github.com/protocol/ResNetLab/blob/master/OPEN_PROBLEMS/MUTABLE_DATA.md">Mutable Data (Naming, Real-Time, Guarantees)</a></b></td>
    <td>Enabling a multitude of different patterns of interactions between users, machines and both. In other words, what are the essential primitives that must be provided for dynamic applications to exist, what are the guarantees they require (consistency, availability, persistancy, authenticity, etc) from the underlying layer in order create powerful and complete applications in the Distributed Web.</td>
  </tr>

  <tr>
    <th></th>
    <td><b><a href="https://github.com/protocol/ResNetLab/blob/master/OPEN_PROBLEMS/HUMAN_READABLE_NAMING.md">Human Readable Naming</a></b></td>
    <td>You can only have two of three properties for a name: Human-meaningful, Secure and/or Decentralized. This is Zooko's Trilemma. Can we have all 3, or even more? Can context related to some data help solve this problem?</td>
  </tr>

  <tr>
    <th></th>
    <td><b><a href="https://github.com/protocol/ResNetLab/blob/master/OPEN_PROBLEMS/PUBSUB_AT_SCALE.md">PubSub at Scale (1M, 10M, 100M, 1B.. nodes)</a></b></td>
    <td>As the IPFS system is evolving and growing, communicating new entries to the IPNS is becoming an issue due to the increased network and node load requirements. The expected growth of the system to multiple millions of nodes is going to create significant performance issues, which might render the system unusable. Despite the significant amount of related literature on the topic of pub/sub, very few systems have been tested to that level of scalability, while those that have been are mostly cloud-based, managed and structured infrastructures.</td>
  </tr>

  <tr>
    <th><b>Data Exchange</b></th>
    <td><b><a href="https://github.com/protocol/ResNetLab/blob/master/OPEN_PROBLEMS/ENHANCED_BITSWAP_GRAPHSYNC.md">Enhanced Bitswap/GraphSync with more Network Smarts </a></b></td>
    <td>Bitswap is a simple protocol and it generally works. However, we feel that its performance can be substantially improved. One of the main factors that hold performance back is the fact that a node cannot request a subgraph of the DAG and results in many round-trips in order to “walk down” the DAG. The current operation of bitswap is also very often linked to duplicate transmission and receipt of content which overloads both the end nodes and the network.</td>
  </tr>

  <tr>
    <th><b>Distributed Type Systems</b></th>
    <td><b><a href="https://github.com/ipfs/notes/pull/394">Improved layouts to represent data in hash-linked graphs (using IPLD) </a></b></td>
    <td>Future™ ⚙️</td>
  </tr>
</table>

### Projects

- ONGOING
  - 3DMs: Decentralised Data Delivery Markets
  - Content Routing Scale Up
  - P2P Observatory
  - ResNetLab on (Virtual) Tour:
We have built a half-day tutorial to introduce the DWeb, the IPFS ecosystem, the IPFS architecture and its supporting protocols, and the high-level design decisions of the Filecoin network. In 2020, we have participated in multiple conferences and other academic events to discuss the exciting projects we're working on and invite great researchers to collaborate with us.
The tutorial is primarily composed of lecture material, and many of our tutorials have been very interactive. In 2021, we are enhancing the tutorial with hands-on sessions, so it will be even more exciting for students and researchers with a passion to tinker as they learn. You can find summaries of our talks in the "Publications and Talks" section further down.

- COMPLETED
  - **Hydra Booster:** The Hydra booster is a new type of DHT node designed to accelerate the Content Resolution & Content Providing on the IPFS Network. This new type of peer exists  to augment the network by creating multiple distributed identities across the DHT address space, enabling it to contribute to the storage and discovery of content provider records. All of these identities are linked by the same backend datastore, which from the other peers’ perspective, creates the effect of multiple peers being present and holding a vast collection of the provider records in the network. The Hydra Booster has been instrumental for the stability and fast content resolution of the IPFS network. 
  Hydra boosters have been designed, implemented and are in operation today in the public IPFS network as of [release go-ipfs 0.5](https://blog.ipfs.io/2020-04-28-go-ipfs-0-5-0/). The Hydra booster is open source, lives in [this repository](https://github.com/libp2p/hydra-booster) and can be deployed by anyone.
  
  - **Gossipsub v1.1:** Gossipsub is one of the many libp2p PubSub routers used to disseminate IPNS records in the IPFS network, enable real-time distributed applications, and much more. GossipSub was adopted as a messaging layer by Filecoin and ETH2.0, due to its functionality and fast performance on permissionless networks. This has led us to invest additional effort to protect it against sybil attacks and malicious behavior.
Together with the libp2p team, we embarked on a mission to harden the protocol’s behavior. The outcome is a hardened version of the Gossipsub protocol that integrates several mitigation strategies at the protocol level.
You can learn more about this project in this [blogpost](https://research.protocol.ai/blog/2020/gossipsub-an-attack-resilient-messaging-layer-protocol-for-public-blockchains), this [paper](https://research.protocol.ai/publications/gossipsub-attack-resilient-message-propagation-in-the-filecoin-and-eth2.0-networks/) and this [talk](https://www.youtube.com/watch?v=APVp-20ATLk&feature=youtu.be&t=3612) at the Matrix.org “Open Tech will save us all” event. Our extensive performance evaluation can be found in this [report](https://research.protocol.ai/publications/gossipsub-v1.1-evaluation-report/). The specification of the protocol can be found [here](https://github.com/libp2p/specs/blob/master/pubsub/gossipsub/gossipsub-v1.1.md).
  - **[drand](https://drand.love/):** drand is an unbiasable source of randomness which other platforms and applications can publicly verify. Randomness is at the core of many of the security-critical operations we perform online every day, and until 2020, there wasn’t a single reliable and trustworthy source. That changed in 2020 with the drand project.
  drand is hosted by more than 15 independent members of the League of Entropy, and is available for any project that needs randomness to use. You can find more details in the platform's [launch blog post](https://drand.love/blog/2020/08/10/drand-launches-v1-0/). We organised a whole-day summit to talk about the technical details of drand and distributed randomness. You can watch all the recordings at [randomness2020.com](https://randomness2020.com/).
  - **[Beyond Bitswap](https://github.com/protocol/beyond-bitswap):** File-transfer is at the core of IPFS and every subsystem inside IPFS is built to enable it in a fast and secure way, while maintaining certain guarantees (e.g. discoverability, data integrity and so on). The aim of the project was two-fold: to drive speed-ups in file-sharing for IPFS and other P2P networks; and to enable a framework for anyone to join the quest of designing, implementing and evaluating brand new file-sharing strategies in P2P networks. The outcomes of the project are impressive: we produced [10 improvement proposals](https://github.com/protocol/beyond-bitswap#enhancement-rfcs), many of them prototyped, a complete [testbed](https://github.com/protocol/beyond-bitswap/tree/master/testbed) to benchmark and debug file sharing in IPFS, a [paper](https://research.protocol.ai/publications/accelerating-content-routing-with-bitswap-a-multi-path-file-transfer-protocol-in-ipfs-and-filecoin/) that provides further details about our design proposals and [several blogposts](https://research.protocol.ai/blog/) to summarise our results.


### RFPs

- ACTIVE
  - n/a
- PAST
  - <a href="https://github.com/protocol/research-RFPs/blob/master/RFPs/rfp-7-MLDHT.md">RFP-7: Multi-Level DHT Design and Evaluation</a>
  - <a href="https://github.com/protocol/research-RFPs/blob/master/RFPs/rfp-8-pubsub.md">RFP-8: Scalability Bounds of P2P Pub/Sub (libp2p floodsub, gossipsub and episub)</a>

### Collaborations

- PRESENT
  - [Prof. George Polyzos](https://www.aueb.gr/en/faculty_page/polyzos-george), [Dr. Spyros Voulgaris](https://acropolis.aueb.gr/~spyros/www/) and their team at the [Athens University of Economics and Business (AUEB)](https://mm.aueb.gr/), Greece. Our project focuses on the design and implementation of a Multi-Layer DHT for IPFS.
  - [Dr. Hidehiro Kanemitsu](https://www.teu.ac.jp/grad/english/teacher/cs\_spc/index.html?id=45) and [Prof. Hidenori Nakazato](https://waseda.pure.elsevier.com/en/persons/hidenori-nakazato) and their teams at the [Tokyo University of Technology](https://www.teu.ac.jp/english/index.html) and [Waseda University](https://www.waseda.jp/top/en/), respectively. Our project focuses on optimisation of DHT lookup times.
  - [Dr. Sreeram Kannan](https://people.ece.uw.edu/kannan_sreeram/) and [Dr. Shaileshh Bojja Venkatakrishnan](https://cse.osu.edu/people/bojjavenkatakrishnan.2) at the [University of Washington](https://www.ece.uw.edu/) and [Ohio State University](https://engineering.osu.edu/), respectively. Our project focuses on the optimisation of GossipSub protocol in terms of speed and scalability.
  - [Dr Joao Leitao](https://asc.di.fct.unl.pt/~jleitao/) and his team at [NOVA University of Lisbon](https://www.fct.unl.pt/en/research/nova-laboratory-computer-science-and-informatics). 

You can read more about these projects in this [blogpost](https://research.protocol.ai/blog/2020/meet-the-latest-protocol-labs-research-grant-recipients/).

- PAST
  - [PulsarCast](https://github.com/JGAntunes/pulsarcast)
  - [DClaims](https://research.protocol.ai/publications/dclaims-a-censorship-resistant-web-annotations-system-using-ipfs-and-ethereum/)

### Publications, Talks & Trainings

#### Talks

- February 2021: `Talk` FOSDEM (blogpost coming soon)
- December 2020: [`Tutorial` IEEE Globecom](https://research.protocol.ai/blog/2021/ieee-globecom-2020-the-interplanetary-file-system-and-the-filecoin-network/)
- November 2020: [`Tutorial` CNSM](https://research.protocol.ai/blog/2020/ieee/ifip-cnsm-2020-the-interplanetary-file-system-and-the-filecoin-network/)
- July 2020: [`Tutorial` IFIP/IEEE DSN](https://research.protocol.ai/blog/2020/ieee/ifip-dsn-2020-the-interplanetary-file-system-and-the-filecoin-network/)
- May 2020: [`Talk` NGN Group](https://research.protocol.ai/blog/2020/next-generation-networks-ngn-group-talk-a-high-level-overview-of-the-interplanetary-file-system/)
- May 2020: [`Tutorial` IEEE ICBC](https://research.protocol.ai/blog/2020/ieee-icbc-2020-the-interplanetary-file-system-and-the-filecoin-network/)
- April 2020: [`Talk` IRTF DINRG](https://research.protocol.ai/blog/2020/ipfs-talk-at-the-irtf-decentralised-internet-infrastructure-research-group-meeting)
- March 2020: [`Talk` NDN Project Consortium](https://research.protocol.ai/blog/2020/ndn-seminar-a-high-level-overview-of-the-interplanetary-file-system/)
- Spet 2019 - [`Tutorial` ACM ICN Tutorial: The InterPlanetary File System (IPFS)](https://conferences.sigcomm.org/acm-icn/2019/tutorial-IPFS.php)
- July 2019 - [`Talk` A Brief History of Information-Centric Networks](https://github.com/protocol/research/issues/14#issuecomment-517048457)


#### Papers

- Dec 2020 - [`Paper` Accelerating Content Routing with Bitswap: A multi-path file transfer protocol in IPFS and Filecoin](https://research.protocol.ai/publications/accelerating-content-routing-with-bitswap-a-multi-path-file-transfer-protocol-in-ipfs-and-filecoin/)
- July 2020 - [`Paper` Gossipsub: Attack-resilient message propagation in the Filecoin and ETH2.0 networks](https://research.protocol.ai/publications/gossipsub-attack-resilient-message-propagation-in-the-filecoin-and-eth2.0-networks/)
- July 2020 - [`Paper` - PulsarCast](https://github.com/JGAntunes/pulsarcast/blob/master/paper/paper.pdf)
- Dec 2019 - [`Paper` DClaims: A Censorship Resistant Web Annotations System using IPFS and Ethereum](https://arxiv.org/pdf/1912.03388.pdf)
  - [zkcapital - paper of the week](https://zkcapital.substack.com/p/this-week-in-blockchain-research-4de)
  - This paper will be presented http://www.sigapp.org/sac/sac2020/

#### Training & Seminars
- 2020, Feb - [The effective Distributed Systems developer curriculum, with Golang](https://github.com/protocol/ResNetLab-private/issues/51)
  - [Curriculum](https://docs.google.com/document/d/1nPcEWymKFAsGJJQiQb5PSiF_2EiAw7GCuLfZMV5r6Y8/edit#heading=h.l73q2rxlx59z)

### Team

- [David Dias](https://research.protocol.ai/authors/david-dias/) - Research Engineer & ResNetLab Lead (PI)
- [Yiannis Psaras](https://research.protocol.ai/authors/yiannis-psaras/) - Research Scientist
- [Alfonso de la Rocha](https://research.protocol.ai/authors/alfonso-delarocha/) - Research Engineer
- [Petar Maymounkov](https://research.protocol.ai/authors/petar-maymounkov/) - Research Scientist
- Adrian Lanzafame - Research Engineer

### Contact

You can reach out to us anytime with your question and interest in these projects by emailing [resnetlab@protocol.ai](mailto:resnetlab@protocol.ai)
