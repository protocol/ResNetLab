# Beyond Bitswap

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

## Contributions & Results

### Documents
* [Related Work](https://docs.google.com/document/d/14AE8OJvSpkhguq2k1Gfc9h0JvorvLgOUSVrj3CnOkQk/edit#heading=h.nxkc23tlbqhl): It gives an overview of the problem, how it will be tackled, and a collection of references and community proposals.
* [Beyond Bitswap Slides](https://docs.google.com/presentation/d/18_aRTye2t6Xs_VhKwEbhvCYYu9ePaLgamIrJkpUDtfY/edit#slide=id.p): Set of slides introducing the project and summarizing the Related Work document from above. These slides were used to introduce the project in the following [talk]().
* [Survey of the state of the art](https://docs.google.com/document/d/172q0EQFPDrVrWGt5TiEj2MToTXIor4mP1gCuKv4re5I/edit#heading=h.nxkc23tlbqhl): It summarizes a list of papers on file-sharing strategies in P2P networks used as a groundwork for the projects.
* [Evaluation Plan](https://docs.google.com/document/d/1LYs3WDCwpkrBdfrnB_LE0xsxdMCIhXdCchIkbzZc8OE/edit#heading=h.nxkc23tlbqhl): Document describing the testbed and evaluation plan designed to test the performane of current implementation of file-sharing systems, and compare it with the improvements implemented within the scope of this work.
* [Enhancements RFC](https://docs.google.com/document/d/1zjJCZel8zJzgK3XuHK0YZlNffEHThq7tUOssGgRTryY/edit#heading=h.nxkc23tlbqhl): A list of enhancements proposals and ideas to improve file-sharing in IPFS and P2P networks.
* [Test Results](https://docs.google.com/document/d/1zPpgnr9ykJr5PAvShJBGhKKRDRbsglb00MPc5eVEU4Q/edit#): This document collects the results of the tests performed in the scope of the project.

### Enhancement RFCs
This section shares a list of improvement RFCs that are being currently tackled, discussed and prototyped. Each RFC aims to test a specific idea or assumption, and they may initially be implemented over Bitswap, but that doesn't mean the conclusions drawn are exclusively applicable to the Bitswap protocol. RFCs are divided in the different layers for file-sharing in P2P sytems identified in the [Related Work](https://docs.google.com/document/d/14AE8OJvSpkhguq2k1Gfc9h0JvorvLgOUSVrj3CnOkQk/edit#heading=h.nxkc23tlbqhl).

> Layer 1 RFCs: Discovery and announcement of content
* [RFC|BB|L1-04: Track WANT messages for future queries](./rfc/rfcBBL104.md): Evaluates how using information from a nodes surrounding can help the discovery and fetching of popular content in the network. 
* [RFC|BB|L1-02: TTLs for rebroadcasting WANT messages](./rfc/rfcBBL102.md): It evaluates how broadcasting exchange requests TTL hops away may help the discovery of content improving performance.

> Layer 2 RFCs: Negotiation and transmission of content.
* [RFC|BB|L2-07: Request minimum piece size and content protocol extension](./rfc/rfcBBL207.md): Evaluates how the size of the chunks that comprises content requested in a P2P network may affect performance.
* [RFC|BB|L12-01: Bitswap/Graphsync exchange messages extension and transmission choice](./rfc/rfcBBL1201.md): Proposes dividing the exchange of content in two phases: a negotiation phase used to discover the holders of the different chunks of a file, and a transfer file to explicitly request blocks from different chunk holders. This opens the door to additional exchange strategies and schemes to improve performance.
* [RFC|BB|L2-03A: Use of compression and adjustable block size](./rfc/rfcBBL203A.md): Evaluates the potential performance improvementes on the use of compression for the exchange of content in P2P networks.

Feel free to jump into the discussions around the project or to propose your own RFC opening an issue in the repo.


### Code
* [Testbed and related assets](https://github.com/adlrocha/beyond-bitswap/): This repo includes all the code used for the implementation and other auxiliary testing assets. Additional documentation is provided in the repo.
* [Bitswap fork](https://github.com/adlrocha/go-bitswap): This fork of `go-bitswap` is the one being used to implement and evaluate some of the RFCs and where additional metrics that want to be tracked in the testbed are being included. RFCs are imeplemented in different branches with the code of the RFC.
* [Graphsync fork](): This fork of `go-graphsync` is also being used to test some of the RFCs.
* [New exchange protocol?](): This is a work in progress to be determined with the result of our explorations.

### Talks
* [Introduction to Beyond Bitswap project](): Introductory talk to show the initial work and motivate the project.
* [How rfcBBL104 was implemented](https://drive.google.com/file/d/1YS3RoNdeeG1vauJpfvHvKUQzPHr97eHF/view?usp=sharing): Video on how the implementation of rfcBBL104 was approached. 
* [Progress update September 2020](https://drive.google.com/file/d/1vUWnfQMIqz9hoqWB941vbzqkP16-_ydd/view?usp=sharing): Progress update of the project explaining the RFCs implemented, the testbed and some preliminary results.

### Blog Posts
* [Beyond Bitswap](https://adlrocha.substack.com/p/adlrocha-beyond-bitswap-i)
* [Network Coding in P2P Networks](https://adlrocha.substack.com/p/adlrocha-network-coding-in-p2p-networks)
* [Hash Array Mapped Tries](https://adlrocha.substack.com/p/adlrocha-hash-array-mapped-tries)
