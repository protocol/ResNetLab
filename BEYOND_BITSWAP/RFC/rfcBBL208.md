#  RFC|BB|L2-08: Delegate download to other nodes (bandwidth aggregation)
* Status: `Brainstorm`

### Abstract

This RFC proposes the delegation of discovery and downloads to other nodes in the network with better capabilities than ours. Before triggering the download of a file, a node can send a TRANSMISSION_GRAFT message to other peers to request them to become part of its file-sharing cluster. Nodes in a file-sharing cluster search and download content in-line. 

### Shortcomings

A node alone may have a hard time discovering and downloading all the blocks of a file, especially if it has limited network capabilities and it is trying to download large files. By inviting other nodes to be part of the transmission process, resources are aggregated and the processes can be parallelized.

### Description

Basic ideas to develop:

-   Nodes can ask other peers to become part of their cluster.
-   Whenever someone in the cluster wants to get content, the list of blocks are distributed between peers in the cluster so each is responsible for the download of part of it. We can set a level of overlap between the blocks distributed to each peer so that even if a peer doesn't get all the blocks the original content can be recovered.
-   When a peer downloads all its assigned blocks, it opens a transmission channel with the requestor and directly sends the downloaded blocks.
-   Clusters may be created on-demand (a peer requests to build a cluster for a specific file), or a "default cluster" may be created by default according, for instance, to the existing GossipSub mesh a peer belongs to (following a similar approach to [RFC|BB|L1-06: Content Anchors](https://github.com/protocol/ResNetLab/issues/6)).

## Implementation plan

# Impact

## Evaluation Plan

## Prior Work
* This RFC can leverage (or even be combined with) the work done in [RFCBBL102](./rfcBBL102) where a TTL was added in Bitswap messages so nodes can discover and request content on behalf of other nodes.
* A similar idea to this one was also discussed in [this issue](https://github.com/ipfs/notes/issues/386.

## Results

## Future Work