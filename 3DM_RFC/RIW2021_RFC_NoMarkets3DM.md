RIW2021|RFC: No market 3DMs
===========================

Status:  draft; ready for review; ready to publish 

Area of Improvement: Cryptoeconomics

Estimated Effort Needed: <?>

Prerequisite(s): <?>

Priority: <? P0, P1, P2>

### Abstract

This RFC proposes an economic model for the retrieval network based on CID bounties instead of an actual market. Content publishers offer a bounty to a CID to reward for all the retrievals successfully performed for that CID for a period of time. All miners able to show proofs-of-storage of hot copies of the CID, and proofs-of-retrieval at the end of the specified period of time are rewarded with a split of the bounty.

### Proposal/Construction

Anyone looking to serve content through the retrieval network can offer a bounty for the CID that wants to be served. Ideally, a cold copy of this CID object will be stored in Filecoin storage, or a hot copy in the IPFS network. The bounty is determined for a specific period of time (mins, hours, or days). Miners have access to the pool of bounties available and can freely choose the CIDs they want to store and serve. This approach is similar to those of transaction fees in traditional Bitcoins, where according to the transaction fees, miners choose the transactions to include in the next block.

Miners can choose their own strategy to profit from retrievals: they may choose CIDs with large bounties because the probability of being rewarded is larger; or go for CIDs with small bounties as despite these being low, the competition to split the rewards is lower.

In order to be eligible for the reward, miners targeting the bounty for a CID must generate periodic proofs-of-storage for the hot copy of the CID. Proofs-of-storage only unlock a percentage of the bounty, in order to unlock the full bounty a minimum number of retrievals for the content need to be performed. Thus, by the end of the lifetime of the bounty, miners receive a split of the unlocked bounty proportional to the number of proof-of-storage they successfully committed.

This RFC doesn't reward retrieval but storage of hot copies and evenly splits the unlocked bounty for a CID between all the miners committed to serve the content. This favors the decentralization of the system and spreading hot copies throughout the network. The higher the bounty, the higher the probability of miners storing and serving a CID. Proofs-of-retrieval, i.e. the number of times the CID has been successfully retrieved, only determines the percentage of the bounty that is unlocked.

This RFC allows several alternative implementation:

-   The settlement and reward distribution is done only at the end of the lifetime of the bounty. This means that the content publisher stakes from scratch the amount of the bounty which is not distributed until the end. Throughout the lifetime of the bounty, a publisher is allowed to increase the bounty as an attempt to increase the perceived QoS of the CID's retrieval. However, the distribution to miners is not done until the end.

-   And additional implementation may be devised where a set of periodic checkpoints are determined throughout the lifetime of the bounty to unlock part of the bounty as rewards.

A more thorough analysis will need to be done in order to determine what implementation is better.

Impact

Simplifies the implementation of the economic model for the retrieval network. This RFC delegates the responsibility of its operation on reliable proof-of-storage and proof-of-retrieval constructions. Its simplicity makes it applicable as a supplement to other RFCs.

### Pros and Cons

Pros

-   Simple and elegant design.

-   Limited overhead over the rest of the system components.

Cons

-   It requires the construction of reliable proofs-of-storage and proofs-of-retrieval.

-   It doesn't prevent sybil and collusion attacks. Miners may run clients retrieving the content they are providing in order to unlock all of the available bounty. There needs to be a way to prevent these attacks.

### Implementation notes

Once the proof-of-storage and proof-of-retrieval constructions have been figured out, the implementation of this RFC is limited to a smart contract (or a spec actor) to set new CID bounties, track its lifetime, accept the committing of proofs by miners, and distributes the bounty at the end of its lifetime.

### Evaluation

Qualitative

-   The RFC increases the size of the retrieval market (demand for content retrieval) in the Filecoin network.

-   The RFC ensures the profitability of retrieval miners for their cost model and committed resources.

-   The UX of clients with this RFC is not penalized in any way (latency, QoS, etc.)

-   The model prevents sybil and collusion attacks, and fosters collaboration between entities.

Quantitative

-   Simulation analysis of the full economic model to understand the Nash Equilibrium.

-   How does the size of the bounty affect the perceived QoS of CID retrieval?

-   What takes for miners to keep a copy of  a CID?

-   Functional testing of implementation:

-   Generation of proofs and commitment.

-   Create and top-up bounties.

-   Bounty distribution and settlement.

-   Overhead that the RFC imposes over a file retrieval.

-   Testnet to validate all the assumptions.

### Prior work

-   [Filecoin's Proof-of-Storage](https://spec.filecoin.io/#section-algorithms.pos)

-   [Filecoin's economic model](https://spec.filecoin.io/#section-algorithms.cryptoecon)
