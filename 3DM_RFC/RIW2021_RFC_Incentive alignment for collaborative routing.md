# RIW2021 | RFC: Incentive alignment for collaborative routing

_Status:_ draft

_Area of Improvement:_ Distribution Graph Forming

_Estimated Effort Needed:_ <?>

_Prerequisite(s):_ <?>

_Priority:_ <? P0, P1, P2>

### Abstract

This proposal assumes that retrieval miners participate in the routing of requests in the network by creating paths from client to provider (or cache). The proposal builds (or rather suggests) an economic model to enable and incentivize routing miners to autonomously (but cooperatively) find solutions to improve network routing performance. For instance, all routers that forward a request to its destination get a portion of the reward back.

### Proposal/Construction

The core tenet of this proposal is that “if we have aligned incentives, good routing will follow naturally”. The ultimate target is to build a trustless mechanism to reach consensus on some appropriate global metric for network performance, and then everyone gets a “dividend” (from a block reward subsidy pool, for example, or out of a tax on transaction fees/value) proportional to that metric.

A brief description of the construction goes as follows:

- Routers keep routing information about how to route content. This is not defined in this proposal.
- Every client wanting to retrieve content from the Filecoin network is connected to a retrieval miner or router (terms used interchangeably here). 
- When a router receives a request it is in their best interest to forward it to the right place as they get a reward if the content transfer is successful.
  - This also incentivises them to learn and keep as much routing information as possible.
  - How routing information propagates in the network and populates the routers’ routing tables is not discussed here.
- The routing and forwarding of requests should not include any cryptographic operations (or include only minimal) in order to be as fast as regular data transfers.
- When the content transfer completes, the path of routers that have participated in the request routing and the content fetching gets a part of the reward.
- The economic model should be such that routers are not incentivised to collude or cheat. Ideally, two colluding routers should not be able to get more than the aggregate of the reward they would get if they behaved honestly.
- The return per router could be the same for all routers along the path, or it could have different weights depending on several factors, such as: 
  - degree centrality of nodes, with higher-degree nodes getting more reward, because likely the correct path to the content was found because this node had the most contacts/routing info. This incentivises routers to build as large routing tables as possible and keep many connections open, so eventually the network might become a full mesh with all nodes having roughly similar reward weight.
  - the latest nodes on the path get more reward and then the reward declines linearly as we get closer to the client. This incentivises routers to cache and serve the content locally.


### Open Questions

- How can you trustlessly evaluate global network performance?
  - Hard problem, but not necessarily harder than trustlessly evaluating performance pairwise between peers
- Will spam be a problem?
  - Possibly, in the same way as committed capacity in the Filecoin storage network. But it does not yield outside benefit to the spammer and spamming would come with traffic cost. 
- How do you punish those misbehaving?
  - Misbehaving is self-punishing; should be irrational to misbehave because there’s no individual advantage to gaming the metrics
  - At least assuming an honest majority. Might not work if everyone cheats.
    - Not unlike a traditional consensus problem with minimum security threshold
    - Maybe some analogue/lessons from EIP1559? The more miners join the collusion, the better it is to be outside.
    - Could this work under an optional trust model?
    - Network voting on good vs bad list; people on the good list could skip proofs. The whole network benefits from people being in the good list, so there is no incentive to “badlist” people unnecessarily. Self-destruct switch: if someone in the good list misbehaves, no one makes money.
      - Or perhaps staking on good votes + slashing if they turn out to be bad
- Do downloaders also get rewarded?
  - Part of the fair exchange 

- Can we get cache benefits out of symmetric routing?
  - If data is encrypted to the end-user (requester), this isn’t possible. We’d need an encryption scheme that allowed intermediary access.
  - One solution would be for intermediaries to pose as clients, forge a deal for the data, and then offer a deal to the requester (basically MITM the transaction).

### Impact

The impact of this proposal can be quite significant because it can solve two problems at once: the best routing protocol will be built naturally due to incentives and the router reward is also allocated according to the contribution of a router to the network’s routing.

### Pros and Cons

Pros
- It can solve two problems at once
- Given that routers participate in the resolution of content (i.e., name-based routing), the scheme can provide very fast resolution and delivery times.
- The scheme enables on-path caching, which can both reduce delivery time and save bandwidth for popular content.

Cons
- The approach is really interesting, but there are still lots to be figured out wrt both the economic constructions and the routing protocols.
- Might be a little complicated.

### Implementation notes

TBA

### Evaluation

Verify that colluding routers on a path cannot get more reward than the aggregate of the reward they would receive if they behaved honestly.
Define the best reward split algorithm across the path of routers: what makes more sense: evenly split, or based on some weight function? Should popularity of content play a role here?
Confirm that the routing protocol coming out of this construction is as fast as traditional shortest path routing and achieves nearest replica routing (i.e., find the closest copy of the content).

### Prior work

The [“Proof of Prestige”](https://www.ee.ucl.ac.uk/~ipsaras/files/Proof_of_Prestige-icbc19.pdf) approach can be very helpful here. It proves that colluding is avoided and the “progressive mining” operation proposed is very close to building the path from source to destination.
