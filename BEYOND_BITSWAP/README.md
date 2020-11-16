# Project: Beyond Bitswap

## Motivation & Vision

File-transfer is at the core of IPFS and every subsystem inside IPFS is built to enable it in a fast and secure way, while maintaining certain guarantees (e.g. discoverability, data integrity and so on).

There are a thousand ways to slice a file into pieces and how to transfer it over the wire. However, finding what is the optimal way for the runtime (high powered, low powered device) and network conditions (stable, unstable, remote, offline) is the key challenge.

In high level, this project is about:
* Continuing the previous work on Block Exchange (i.e. Bitswap) and Graph Exchange (i.e. GraphSync)
* Creating a harness that enables to reproducibly run tests that demonstrate the performance of different file-transfer strategies.
* Research and prototype new strategies to acquire new speed ups.
* Acquire leverage by exposing the harness to the whole Open Source and Research community, in a way that others feel compelled to join the effort and try their own strategies.

In short, the aim of the project is two-fold: to drive speed-ups in file-sharing for IPFS and other P2P networks; and to enable a framework for anyone to join the quest of designing, implementing and evaluating brand new file-sharing strategies in P2P networks. 

## Why the project code name?

Bitswap has been for some time the file-sharing subsystem within IPFS, then Graphsync came to propose a new way of approaching file-sharing on IPFS. The scope of the project is not only to improve Bitswap's performance, but file-sharing in P2P networks as a whole. We don't restrict ourselves exlusively to Bitswap or IPFS for our exploration.

Being said that, the fact that IPFS had an infrastructure in place to start testing our ideas, and Bitswap being its file-sharing module, made us start our initial explorations over Bitswap and IPFS, but our aim is to go way farther and improve file-sharing performance with new protocols and proposals every P2P network can leverage and benefit from. In short, we want to go "Beyond Bitswap". The project can be considered a success if by the end of it one has a set of pluggable protocols and modules to achieve file-sharing in P2P environments, along with all the testbeds, tools and benchmarks required to improve this protocols and go _"Beyond Bitswap"_.

## 💌 Invite to Research with us

ResNetLab collaborates with over 10 Research Groups all over the world and Protocol Labs Research has developed research collaborations in multiples of ten in the last few years. We are always eager to collaborate with more researchers in all kinds of capacity, from thesis project (M.Sc or PhD), to Post-Doc, Grants, RFPs and independent research projects.

We are making all our contributions, ideas, testbed, benchmarking and analysis scripts available below. You are more than welcome to pick any of these assets and build on top of it. If you have questions, please [mail us](mailto:resnetlab@protocol.ai).

## Contributions & Results

### Documents

* [Related Work](https://docs.google.com/document/d/14AE8OJvSpkhguq2k1Gfc9h0JvorvLgOUSVrj3CnOkQk/edit#heading=h.nxkc23tlbqhl): It gives an overview of the problem, how it will be tackled, and a collection of references and community proposals.
* [Beyond Bitswap Slides](https://docs.google.com/presentation/d/18_aRTye2t6Xs_VhKwEbhvCYYu9ePaLgamIrJkpUDtfY/edit?usp=sharing): Set of slides introducing the project and summarizing the Related Work document from above. <!-- These slides were used to introduce the project in the following [talk](???). -->
* [Survey of the state of the art](https://docs.google.com/document/d/172q0EQFPDrVrWGt5TiEj2MToTXIor4mP1gCuKv4re5I/edit?usp=sharing): It summarizes a list of papers on file-sharing strategies in P2P networks used as a groundwork for the projects.
* [Evaluation Plan](https://docs.google.com/document/d/1LYs3WDCwpkrBdfrnB_LE0xsxdMCIhXdCchIkbzZc8OE/edit#heading=h.nxkc23tlbqhl): Document describing the testbed and evaluation plan designed to test the performane of current implementation of file-sharing systems, and compare it with the improvements implemented within the scope of this work.
* [Enhancements RFC](#enhancements-rfcs): A list of enhancements proposals and ideas to improve file-sharing in IPFS and P2P networks.<!-- * [Test Results](https://docs.google.com/document/d/1zPpgnr9ykJr5PAvShJBGhKKRDRbsglb00MPc5eVEU4Q/edit#): This document collects the results of the tests performed in the scope of the project. -->

### Enhancement RFCs

This section shares a list of improvement RFCs that are being currently tackled, discussed and prototyped. Each RFC aims to test a specific idea or assumption, and they may initially be implemented over Bitswap, but that doesn't mean the conclusions drawn are exclusively applicable to the Bitswap protocol. RFCs are divided in the different layers for file-sharing in P2P sytems identified in the [Related Work](https://docs.google.com/document/d/14AE8OJvSpkhguq2k1Gfc9h0JvorvLgOUSVrj3CnOkQk/edit#heading=h.nxkc23tlbqhl).

If you want to familiarize with our work, we highly recommend exploring first the RFCs in `prototype` state, and then move to the ones at a `draft` or `brainstorm` state. `prototyped` RFCs are in a stage where there is working prototype you can start evaluating and playing with. The `draft` state means that the RFC is ready for implementation, while `brainstorm` RFCs require further discussions and design work.


| RFC                                                                                                         | Status      |
|-------------------------------------------------------------------------------------------------------------|-------------|
| [RFC\|BB\|L0-09: Hashing algorithm improvements](./RFC/rfcBBL009)                                             | `brainstorm`|
| [RFC\|BB\|L1-04: Track WANT messages for future queries](./RFC/rfcBBL104.md)                                  | `prototype` |
| [RFC\|BB\|L1-02: TTLs for rebroadcasting WANT messages](./RFC/rfcBBL102.md)                                   | `prototype` |
| [RFC\|BB\|L1-06: Content Anchors](https://github.com/protocol/ResNetLab/issues/6)                             | `brainstorm`|
| [RFC\|BB\|L1/2-05: Use of super nodes and decentralized trackers](./RFC/rfcBBL1205.md)                        | `brainstorm`|
| [RFC\|BB\|L12-01: Bitswap/Graphsync exchange messages extension and transmission choice](./RFC/rfcBBL1201.md) | `draft`     |
| [RFC\|BB\|L2-03A: Use of compression and adjustable block size](./RFC/rfcBBL203A.md)                          | `prototype` |
| [RFC\|BB\|L2-03B: Use of network coding and erasure codes](./RFC/rfcBBL203B.md)                               | `brainstorm`|
| [RFC\|BB\|L2-07: Request minimum piece size and content protocol extension](./RFC/rfcBBL207.md)               | `brainstorm`|
| [RFC\|BB\|L2-08: Delegate download to other nodes (bandwidth aggregation)](./RFC/rfcBBL208.md)                | `brainstorm`|

**Layer 0: Data Structure:**
* [RFC|BB|L0-09: Hashing algorithm improvements](./RFC/rfcBBL009):

**Layer 1 RFCs: Discovery and announcement of content:**
* [RFC|BB|L1-04: Track WANT messages for future queries](./RFC/rfcBBL104.md): Evaluates how using information from a nodes surrounding can help the discovery and fetching of popular content in the network. 
* [RFC|BB|L1-02: TTLs for rebroadcasting WANT messages](./RFC/rfcBBL102.md): It evaluates how broadcasting exchange requests TTL hops away, and allowing other nodes to discover and retrieve content on behalf of other peers, may help the discovery of content improving performance.
* [RFC|BB|L1/2-05: Use of super nodes and decentralized trackers](./RFC/rfcBBL1205.md): Aknowledge the fact that P2P networks are also social networks and there are different types of nodes in the network. Explore the use of side-channel discovery mechanisms.
* [RFC|BB|L1-06: Content Anchors](https://github.com/protocol/ResNetLab/issues/6): Evaluate the use of gossipsub to perform more efficient content routing.

**Layer 2 RFCs: Negotiation and transmission of content:**
* [RFC|BB|L12-01: Bitswap/Graphsync exchange messages extension and transmission choice](./RFC/rfcBBL1201.md): Proposes dividing the exchange of content in two phases: a negotiation phase used to discover the holders of the different chunks of a file, and a transfer file to explicitly request blocks from different chunk holders. This opens the door to additional exchange strategies and schemes to improve performance.
* [RFC|BB|L2-03A: Use of compression and adjustable block size](./RFC/rfcBBL203A.md): Evaluates the potential performance improvementes on the use of compression for the exchange of content in P2P networks.
* [RFC|BB|L2-03B: se of network coding and erasure codes](./RFC/rfcBBL203B.md): Evaluates the potential performance improvementes on the use of network coding and erasure codes to leverage the transmission of content from multiple streams.
* [RFC|BB|L2-07: Request minimum piece size and content protocol extension](./RFC/rfcBBL207.md): Evaluates how the size of the chunks that comprises content requested in a P2P network may affect performance.
* [RFC|BB|L2-08: Delegate download to other nodes (bandwidth aggregation)](./RFC/rfcBBL208.md): Leverage the resources of other peer "friends" to collaboratively discover and retrieve content, and perform faster content retrievals.

Feel free to jump into the discussions around the project or to propose your own RFC opening an issue in the repo.

### Code & Testbed

* [Testbed, benchmarking, analysis scripts and related assets](https://github.com/protocol/beyond-bitswap/): All the code used for the implementation and other auxiliary testing assets. Additional documentation is provided in the repo.
* [Bitswap fork](https://github.com/adlrocha/go-bitswap): This fork of `go-bitswap` is the one being used to implement and evaluate some of the RFCs and where additional metrics that want to be tracked in the testbed are being included. RFCs are imeplemented in different branches with the name of the RFC code.

### Talks / Videos

<!-- * [Introduction to Beyond Bitswap project](??): Introductory talk to show the initial work and motivate the project. -->
* [Progress update September 2020](https://drive.google.com/file/d/1vUWnfQMIqz9hoqWB941vbzqkP16-_ydd/view?usp=sharing): Progress update of the project explaining the RFCs implemented, the testbed and some preliminary results.
* [How rfcBBL104 was implemented](https://drive.google.com/file/d/1YS3RoNdeeG1vauJpfvHvKUQzPHr97eHF/view?usp=sharingg): Video on how the implementation of rfcBBL104 was approached. 
* [A Deep Dive In Bitswap](https://drive.google.com/file/d/1jgTOFFtRL0UYeDk98NHoNlEuujBaK08b/view?usp=sharing): Workshop describing in detail the operation of Bitswap and the implementation of some of the improvement RFCs.
* [Demo of compression in libp2p](https://drive.google.com/file/d/1YcemfkS5ZNnH66-tTGmerNrgrsW-bbpD/view?usp=sharing): A demo of the exchange of files between two IPFS nodes with compression enabled in libp2p.

### Publications
* ["Two ears, one mouth": how to leverage bitswap chatter for faster transfers](https://research.protocol.ai/blog/2020/two-ears-one-mouth-how-to-leverage-bitswap-chatter-for-faster-transfers/)
* [Honey, I shrunk our libp2p streams](https://research.protocol.ai/blog/2020/honey-i-shrunk-our-libp2p-streams/)
* [Beyond Bitswap](https://adlrocha.substack.com/p/adlrocha-beyond-bitswap-i)
* [Network Coding in P2P Networks](https://adlrocha.substack.com/p/adlrocha-network-coding-in-p2p-networks)
* [Hash Array Mapped Tries](https://adlrocha.substack.com/p/adlrocha-hash-array-mapped-tries)