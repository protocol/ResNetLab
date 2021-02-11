# RIW2021 | RFC: Hybrid CDN with Recruiter Miners

_Status:_ < draft >

_Area of Improvement:_ < Opportunistic Deployments >

_Estimated Effort Needed:_ <?>

_Prerequisite(s):_ <?>

_Priority:_ <? P0, P1, P2>

### Abstract

Retrieval Miners (RMs) recruit storage from local, ephemeral devices (laptops and desktops) to increase their storage footprint. The target is not to save on aggregate storage or bandwidth of the RM - in some cases the RM might have to use more bandwidth if they act as relays. The target is to discover local content, where available and serve it locally. The proposal also has the potential to save on concentrated bandwidth and request-handling load.

This proposal is similar to the [“Consume Global, Serve Local”](https://github.com/protocol/ResNetLab/pull/26) RFC.

### Construction

We consider a Hybrid-CDN-like architecture, where retrieval miners act as the centralized controllers orchestrating all the devices in their surroundings (or directly connected to them). When peer A is looking for some content, it sends the request to its local retrieval miner (RM). RM answers with a list of peers storing the content. These peers may be other RMs, or opportunistic deployments that RM knows are near peer A. Peer A then requests the retrieval of the chunks of content to any of these peers. A Bitswap-like protocol may be used for these retrievals. Instead of directly sending the request to a local retrieval miner, peer A can send in parallel requests to devices in their local area network. Additionally, in the RM response, there is always some fallback retrieval miner with a hot copy of the content for the case when the opportunistic deployments storing the copy are not connected anymore. 

Devices can build resource reputation by testing QoS specifying different permutations of the chunks to each peer, so as to optimize expected time to completion.

Ideally, nodes would be opportunistically trading data (and micropayments) with their spatial neighbors in a way that maximizes their EV (of reselling data and of coins)
We may need a way to do offline micropayments.

**Open Questions**

- We may need a way to do offline micropayments for the cases where devices are only locally connected to each other. 
- Should we be concerned about privacy?
- How is the model affected by device mobility and churn?

### Impact

Recruiting every-day user devices to serve in the 3DM is a big win as we really achieve the goal of low-resource devices participating, contributing and being rewarded by the network. The impact can be huge.

### Pros and Cons

Pros:
- The idea of having everyday devices participating in the network is super impactful and interesting
- Performance and capacity of the network will increase dramatically, if users are given the right incentives

Cons:
- Privacy may become an issue
- Limitations such as “only requested content is cached to a node’s storage” need to be lifted in order for the scheme to scale and be successful.
- The cryptoeconomic model is challenging (but this is not new to this).
- NAT traversal needs to have a solution for the scheme to be viable. A simple solution is for “recruited” devices to keep a permanent connection open with the client

### Implementation notes

- Privacy needs to be looked at
- NAT traversal needs to have a solution for the scheme to be viable. A simple solution is for “recruited” devices to keep a permanent connection open with the client

### EValuation

TBA

### Prior Work

TBA
