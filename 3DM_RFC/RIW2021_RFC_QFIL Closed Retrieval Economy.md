

# RIW2021|RFC: QFIL Closed Retrieval Economy

_Status:_ **draft**; **~~ready for review~~**; **~~ready to publish~~ **

_Area of Improvement:_ Cryptoeconomics

_Estimated Effort Needed:_ &lt;?>

_Prerequisite(s):_ &lt;?>

_Priority:_ &lt;? P0, P1, P2>


### Abstract

This RFC models the economic model of the retrieval network as a closed economy backed by the Filecoin economy. It divides the model into two parts: an auction market between content publishers and retrieval miners to agree on the minimum deposit the publisher needs to stake to pay for the retrievals; and the closed retrieval economic orchestrated by the QFIL currency that rewards the parties involved in the retrievals and is backed by the publisher’s deposit. 


### **Proposal/Construction**

This RFC assumes content publishers (CPs) as the ones paying for retrievals, i.e. the entity willing to serve a specific CID through the retrieval network is the one paying for the retrievals. The content to be served through the retrieval network may be stored sealed in Filecoin, or in a hot (unsealed) state in IPFS.

To put content into the network, content publishers run a “take-it-or-leave-it” auction with retrieval miners. CPs offer a price and a desired QoS for retrievals. Retrieval miners accept offers from the pool according to their reserve price. Retrieval miners will only choose offers from the auction pool for which the offer price is over the miner’s reserve price (minimum price it is willing to accept for an offer). Miner’s compute their reserve price according to their expected costs of:



*   Query execution (e.g.. Unsealing data, cloud bandwidth costs, amortized storage costs for a cache, solving NP problems for metering --if the construction requires it--, searching data structure, etc.). 
*   Cost of bandwidth
*   Collaboration costs: costs incurred to mint rewards for other miners helping on retrievals.

When a retrieval miner accepts an offer, the CP needs to stake the payment deposit according to the base price and the specification of the deal, and from there on the accepting retrieval miner becomes the “deal owner”. This “take-it-or-leave-it” auction may be replaced by a simple negotiation phase between a miner and a content publisher. The result of the setup phase would be the same: a deposit to cover retrieval payments and back the rewards being minted in the closed retrieval economy, and a retrieval miner as “deal owner” of this stake.

The closed retrieval economy is governed by a retrieval currency (QFIL). QFIL is a volatile token that can be in two forms: 



*   Transient (TQFIL): Is the form it takes when it is minted. Commiting a valid voucher for a full (or partial) file exchange triggers the minting of new TQFIL by the deal owner to reward the parties involved in the exchange. TQFIL is associated with the deal owner, and it can’t be transferred, it can only be transformed into stable QFIL or FIL from the deal stake. TQFIL has a larger decay than stable QFIL to prompt a transformation decision on holders (preventing an entity from having a lot of power over a deal stake). In order for a deal owner to mint new TQFIL it needs to burn FIL or QFIL.
*   Stable (QFIL): Base stable currency of the closed economy. It has a lower decay than TQFIL, and it is burned to mint new TQFIL. Retrieval miners will be willing to help other miners in the system in order to be rewarded with QFIL, and avoid having to burn their FIL to pay for the retrievals of deals they own.

Instead of having a global system orchestrating all stakes and deals in the network, the role of the deal owner enables a local orchestration of deal funds, with QFIL as the glue of the full retrieval economy. As an analogy, TQFIL is the local currency, while QFIL is the global currency in the economy.

This RFC requires a reliable signal to trigger reward minting. This can be in the form of an on-chain commitment of a payment voucher, a transaction, etc. There is no specific requirement imposed in this sense. Ideally, if the construction of these signals allows the inclusion of the different parties involved in the retrieval (and not just the miner serving the file) more complex rewarding systems may be devised.

Finally, retrieval deals may include an additional “reward for QoS”. Content publishers may provision an additional stake in their deposit to unlock a reward if a retrieval was performed with the minimum QoS required by him in the deal. 

Retrievals under this model would look as follows: 



*   Clients send their requests to their local retrieval miner for a specific CID. If this miner is not able to fulfill the retrieval himself will pay QFIL to all the collaborators committing resources to help him with the deal. This collaboration may be in the form of hot copy sharing, forwarding services, or whatever other scheme we want to incentivize in the graph forming architecture.
*   The successful retrieval of the client generates a signal (as mentioned above in the form of a voucher or transaction) and triggers the minting of the rewards for the retrieval in the form of the deal owner’s TQFIL. This same signal triggers the unlock of the QoS additional reward if applicable. 
*   The reward is distributed almost entirely to the retrieval miner that performed the retrieval, except for a small amount reserved for the deal owner (that way incentivizing retrieval miners to accept deals even if they won’t be completely involved in the retrieval).
    *   An alternative reward mechanism may be devised where instead of minting rewards for the retrieval miner and the deal owner exclusively, and enable cross-payments between retrieval miners for collaboration, the rewards are directly minted for all the parties involved in the exchange. This may impose additional requirements to the system.
*   The deal owner needs to hold enough QFIL or a FIL stake to mint the TQFIL for the reward.
*   Finally, all the rewarded parties choose what to do with their TQFIL, if to cash out into FIL (leave the closed economy), or keep QFIL to back future retrievals of the deals they own.

_Sidenote: We called QFIL as Quantum FIL because it can be in two forms, but it ends up decaying to a stable form._

**Impact**

This RFC proposes an economic framework backed by the Filecoin economy. The model presents all the elements required to design any incentive system required in the rest of the part of the system: client metering, content payment, and graph forming. By building this economic model as a closed economy we require no changes on the Filecoin economy or impose additional requirements to it. With this, the retrieval economy will be able to evolve independently of the Filecoin economy.


### Pros and Cons

**Pros**



*   Economic model decoupled from the Filecoin economy and with all the elements for the design of the incentive systems required in the network.
*   The only requirement for the model to work is a reliable signal of successful retrieval to trigger the rewards and payments.
*   The design of QFIL fosters the collaboration between entities in the system.
*   The fact that every reward minted in the system requires some currency to be burnt prevents sybil attacks (and potentially even collusion attacks). Even more, the minting of every reward is backed by its equivalent in FIL.

**Cons**



*   Fine-tuning all the parameters of the model may be hard. There are a lot of moving parts: design of the rewards, QFIL and TQFIL decay speeds, etc.
*   A more thorough analysis of the Nash equilibrium of the system is required to ensure that the design of QFIL indeed fosters collaboration and achieves our goals.
*   How to reward collaboration between miners is really tied to the graph forming design, so nothing can be specified in that sense until the design of the graph forming, and the interaction between retrieval miners is fleshed out.
*   This model doesn’t prevent an entity from trying to bankrupt a specific content publisher. Someone may want to indiscriminately retrieve a file from a content provider to dry its stake. The fact that content is cached, and the resources required may lead to economies of scale that make the impact of this attack negligible.


### **Implementation **n**otes**



*   All the logic behind the minting of rewards and deposits should be implemented in a smart contract (or Filecoin actor). This smart contract receives signals from successful retrievals, and when new stakes for a deal have been set up.
*   The logic for the auction (or negotiation phase) will be independent from the aforementioned actor. 


### **Evaluation**

**Qualitative**



*   The RFC increases the size of the retrieval market (demand for content retrieval) in the Filecoin network.
*   The RFC ensures the profitability of retrieval miners for their cost model and committed resources.
    *   How does miners' reserve price be computed?
*   The UX of clients with this RFC is not penalized in any way (latency, QoS, etc.)
*   The model prevents sybil and collusion attacks, and fosters collaboration between entities.

**Quantitative**



*   Simulation analysis of the full economic model to understand the Nash Equilibrium.
    *   How should the decay of QFIL be designed?
    *   Does the use of a stable QFIL make sense for miners? Would they be willing to transform TQFIL into QFIL instead of FIL, or an additional reward should be considered making 1TQFIL = alphaQFIL with alpha > 1?
*   Functional testing of implementation:
    *   QFIL mechanism and rewards.
    *   Auction / Negotiation.
    *   Payments / Collaboration between miners
*   Overhead that the RFC imposes over a file retrieval.
*   Testnet to validate all the assumptions.


### Prior work



*   [ObservableHQ modelling a preliminary proposal of this RFC](https://observablehq.com/@protocol/3dms-cryptoecon-proposal)

This RFC takes inspiration from the following papers:



*   [Edge-MAP: Auction Markets for Edge Resource Provisioning:](https://drive.google.com/file/d/1g-kbaPUosWnY1a9098T9nh6TDx9ekQeJ/view?usp=sharing) Inspiration for the use of local auctions.
*   [An economic mechanism for request routing and resource allocation in hybrid CDN–P2P networks](https://drive.google.com/file/d/19_OPhuR4qkbH55SkG8ucgMYn-js445Q0/view?usp=sharing): Cost prediction in hybrid CDNs.
*   [Proof-of-Prestige: A Useful Work Reward System for Unverifiable Tasks](https://drive.google.com/file/d/13gcJP0DZnCCpcIy5BFbe7uLaxxGpL7JW/view?usp=sharing): Use of a volatile token.
*   [A Market Protocol for Decentralized Task Allocation](https://drive.google.com/file/d/1GC3DU0I_-6NSMtfoTl55t6fKPptIkCL5/view?usp=sharing): Use of a reserve price to achieve Nash equilibrium in a decentralized auction system.
