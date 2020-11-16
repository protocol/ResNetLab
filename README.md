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
    <th><b>Networks Observability</b></th>
    <th>NEEDs OPEN PROBLEM</th>
    <th></th>
  </tr>   

  <tr>
    <th><b>Resilliency in Adversarial Networks</b></th>
    <th>NEEDs OPEN PROBLEM</th>
    <th></th>
  </tr>   

  <tr>
    <th><b>Heterogeneous Runtimes</b></th>
    <th>NEEDs OPEN PROBLEM</th>
    <th>(e.g. Browsers, IoT, Low Powered and/or Battery powered devices)</th>
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

- Hydra Booster
- Gossipsub v1.1 
- drand
- [Beyond Bitswap](BEYOND_BITSWAP)
- P2P Observatory

### RFPs

- ACTIVE
  - n/a
- PAST
  - <a href="https://github.com/protocol/research-RFPs/blob/master/RFPs/rfp-7-MLDHT.md">RFP-7: Multi-Level DHT Design and Evaluation</a>
  - <a href="https://github.com/protocol/research-RFPs/blob/master/RFPs/rfp-8-pubsub.md">RFP-8: Scalability Bounds of P2P Pub/Sub (libp2p floodsub, gossipsub and episub)</a>

### Collaborations

- PulsarCast
- DClaims

### Publications, Talks & Trainings

- 2019, Jul - [`Workshop` A Brief History of Information-Centric Networks](https://github.com/protocol/research/issues/14#issuecomment-517048457)
- 2019, Sep - [`Workshop` ACM ICN Tutorial: The InterPlanetary File System (IPFS)](https://conferences.sigcomm.org/acm-icn/2019/tutorial-IPFS.php)
- 2019, Dec - [`Paper` DClaims: A Censorship Resistant Web Annotations System using IPFS and Ethereum](https://arxiv.org/pdf/1912.03388.pdf)
  - [zkcapital - paper of the week](https://zkcapital.substack.com/p/this-week-in-blockchain-research-4de)
  - This paper will be presented http://www.sigapp.org/sac/sac2020/
- 2020, Feb - [The effective Distributed Systems developer curriculum, with Golang](https://github.com/protocol/ResNetLab-private/issues/51)
  - [Curriculum](https://docs.google.com/document/d/1nPcEWymKFAsGJJQiQb5PSiF_2EiAw7GCuLfZMV5r6Y8/edit#heading=h.l73q2rxlx59z)

### Team

- David Dias - Research Engineer & ResNetLab Lead (PI)
- Yiannis Psaras - Research Scientist
- Alfonso de la Rocha - Research Engineer
- Vasilis Giotsas - Research Engineer

### Contact

You can reach out to us anytime with your question and interest in these projects by emailing [resnetlab@protocol.ai](mailto:resnetlab@protocol.ai)