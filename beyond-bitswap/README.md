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

## Contributions & Results

### Documents
* [Related Work](https://docs.google.com/document/d/14AE8OJvSpkhguq2k1Gfc9h0JvorvLgOUSVrj3CnOkQk/edit#heading=h.nxkc23tlbqhl): It gives an overview of the problem, how it will be tackled, and a collection of references and community proposals.
* [Beyond Bitswap Slides](https://docs.google.com/presentation/d/18_aRTye2t6Xs_VhKwEbhvCYYu9ePaLgamIrJkpUDtfY/edit#slide=id.p): Set of slides introducing the project and summarizing the Related Work document from above. These slides were used to introduce the project in the following [talk]().
* [Survey of the state of the art](https://docs.google.com/document/d/172q0EQFPDrVrWGt5TiEj2MToTXIor4mP1gCuKv4re5I/edit#heading=h.nxkc23tlbqhl): It summarizes a list of papers on file-sharing strategies in P2P networks used as a groundwork for the projects.
* [Evaluation Plan](https://docs.google.com/document/d/1LYs3WDCwpkrBdfrnB_LE0xsxdMCIhXdCchIkbzZc8OE/edit#heading=h.nxkc23tlbqhl): Document describing the testbed and evaluation plan designed to test the performane of current implementation of file-sharing systems, and compare it with the improvements implemented within the scope of this work.
* [Enhancements RFC](https://docs.google.com/document/d/1zjJCZel8zJzgK3XuHK0YZlNffEHThq7tUOssGgRTryY/edit#heading=h.nxkc23tlbqhl): A list of enhancements proposals and ideas to improve file-sharing in IPFS.
* [Test Results](https://docs.google.com/document/d/1zPpgnr9ykJr5PAvShJBGhKKRDRbsglb00MPc5eVEU4Q/edit#): This document collects the results of the tests performed in the scope of the project.

### Enhancement RFCs
* [RFC|BB|L1-04: Track WANT messages for future queries](./rfc/rfcBBL104.md)
* [RFC|BB|L2-03A: Use of compression and adjustable block size](./rfc/rfcBBL203A.md)
* [RFC|BB | L2-07: Request minimum piece size and content protocol extension](./rfc/rfcBBL207.md)
* [RFC| BB | L12-01: Bitswap/Graphsync exchange messages extension and transmission choice](./rfc/rfcBBL1201.md)
* [RFC| BB | L1-02: TTLs for rebroadcasting WANT messages](./rfc/rfcBBL102.md)


### Code
* [Testbed and related assets](https://github.com/adlrocha/beyond-bitswap/): This repo includes all the code used for the implementation and other auxiliary testing assets. Additional documentation will be provided in the repo.
* [Bitswap fork](https://github.com/adlrocha/go-bitswap): This fork of `go-bitswap` is the one being used to implement some of the RFCs and where additional metrics that want to be tracked in the testbed are being included. Expect RFCs to be imeplemented in different branches.

### Talks
* [Introduction to Beyond Bitswap project](): Introductory talk to show the initial work and motivate the project.
* [How rfcBBL104 was implemented](https://drive.google.com/file/d/1YS3RoNdeeG1vauJpfvHvKUQzPHr97eHF/view?usp=sharing): Video on how the implementation of rfcBBL104 was approached. 

### Blog Posts
* [Beyond Bitswap](https://adlrocha.substack.com/p/adlrocha-beyond-bitswap-i)
* [Network Coding in P2P Networks](https://adlrocha.substack.com/p/adlrocha-network-coding-in-p2p-networks)
* [Hash Array Mapped Tries](https://adlrocha.substack.com/p/adlrocha-hash-array-mapped-tries)