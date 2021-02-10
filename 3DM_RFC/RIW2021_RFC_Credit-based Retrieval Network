RIW2021|RFC: Credit-based Retrieval Network
===========================================

Status:  draft; ready for review; ready to publish 

Area of Improvement: Cryptoeconomics

Estimated Effort Needed: <?>

Prerequisite(s): <?>

Priority: <? P0, P1, P2>

### Abstract

This RFC proposes an economic model for retrieval networks based on a trustless credit network. Clients and content publishers exchange credits backed by an escrow deposit. These credits are used to pay retrieval providers for their services. For providers to mint their reward they need to commit the received credits on-chain. The total reward is proportional to the number of credits they hold.

### Proposal/Construction

This RFC consider the following participants in the system: 

-   Clients that retrieve content by CID from retrieval service. They may be charged with a monthly fee, or not charged at all for the services they use.

-   Publishers store content on the Filecoin (or IPFS) network, and register it with the retrieval market for a per-CID fee.

-   Storage miners (alternatively, the publishers) keep cold copies of the content.

-   Profit maximizing (retrieval) providers who are paid in proportion to their services and represent the basic infrastructure of the retrieval network.

The RFC proposes the design of a credit network for fast content trade between retrieval providers. The network uses an on-chain smart contract to keep track of participants' escrows and for settlement purposes.

The retrieval service is organized in consecutive global sessions of fixed duration. E.g., each calendar day is one session. (Each session is a smart contract, which is itself subordinate to a "master" contract which manages the succession of sessions.)

Retrievals in this RFC are organized in three stages:

-   Session setup: Specifies the content to be served through the retrieval network and performs all on-chain and off-chain setups required to make the session content available for retrieval (i.e. "hot and ready").

-   To serve content through the network, publishers need to store this content in Filecoin (or IPFS) or be willing to provide cold copies (with the right encoding) themselves.

-   In order to join a session, the publisher pays a fee to the session's contract; pays the retrieval fee in the contract; and advertises the CID  and the location of the cold copy (e.g. miner and sector or address of publisher). 

-   Providers create escrows of desired sizes (which depend on their expected traffic imbalance). These escrows back an independent credit system for payments between providers, and are entirely independent of the session contracts. Escrows can be refreshed at any time as needed.

-   Once the session has been established, providers download the list of authorized clients, as well as the cold storage locations of the participating content CIDs.

-   Content delivery: Providers deliver content to clients and to other providers, upon request. Clients pay providers (for content) using client-credits (which represent shares of session fees), whereas providers pay providers using provider-credits (which represent hard currency, e.g. FIL, stored in the provider escrows). 

-   Clients request content from a chosen provider by CID. 

-   The provider authenticates the client (needed only if clients are required to pay for service).

-   The provider looks queries the retrieval network for cached copies of the CID (leveraging the graph forming infrastructure in place, i.e. DHT, gossipsub, NDN, etc.).

-   If the content is cached by other providers, the provider may pick the closest one and buy content, paying with provider-credits. Otherwise, if there is no hot copy of the content in the retrieval network, the content must be bought from cold storage (using FIL) for its subsequent caching (or downloaded from the publisher themselves). After a hot copy of the content is retrieved, the content is forward to the client. The client pays for the content using client-credits.

-   Settlement: As a result of delivering content, providers accumulate client-credits and provider-credits. Both of these are redeemed for value (e.g. a hard currency, like FIL) during settlement after the end of a session. 

-   Provider-credits are redeemed for the value they correspond to, based on the escrows they are backed by. Note that provider credits correspond to value one-for-one, as they are entirely based on a conventional credit system (as described in the relevant papers).

-   Client-credits are different. A client-credit corresponds to a share from the "session revenue", which is the sum of all fees paid by all clients and all publishers (this does not include the escrows paid by providers!).

-   Revenue distribution is computed after the end of the session period as follows:

-   Revenue = client fees + publisher fees.

-   Provider revenue share = number of client-credits collected by provider / total number of client-credits

-   Provider revenue = total revenue * provider revenue share

![](https://lh3.googleusercontent.com/lqFf_okKtzpRRe6tznT8uOFXuI8L6y7ytqo3T4gxXFa6rhCaeQOV2LuoBC2h6TsKaK2gtRggAyn-i5gqNkhTvjp5iKvqz8sOplRvQa69bCHtwDvmFlo2u1O_ZeweLxq8Hr6VZxRg)

### Settlement speed optimization

Every chain-based system that settles off-chain tokens (in our case client-credits and provider-credits) for on-chain value requires that the tokens be uploaded to the chain. In our application, the number of tokens is extremely large (it roughly equals the number of requests in a day) making it impractical to upload all of them to the chain. To solve this problem we utilize a "lottery" technique (described in the [MicroCash paper](https://arxiv.org/abs/1911.08520)).

This technique is generic: It is independent of how value is assigned to a token. Thus, in our case, it can be used for both client-credits and provider-credits, separately.

Here is how the lottery algorithm works in short.

Suppose T is an off-chain token, held by party P, that must be redeemed for on-chain value.

Denote by V(T) the value assigned to ticket T by the application logic.

The protocol for redeeming T is as follows:

1.  Determine if T is a "winning" token. Each token is "winning" independently with probability W, say 5%, based on a common public randomness source.

2.  If T is winning, P uploads T to the chain and receives value V(T)/P for it,

3.  If T is not winning, nothing transpires.

What is notable about this protocol:

-   Only a fraction W, in our example 5%, of the tokens are uploaded to the chain

-   The expected value of each token is V(T).

When a large number of tokens is redeemed, the actual value received by a participant is almost exactly equal to the expected value. (This is a simple consequence of the [Chernoff bound](https://en.wikipedia.org/wiki/Chernoff_bound), and is proved in the MicroCash paper.)

### Design discussion

This RFC is designed to showcase two different payment models for content:

-   Clients pay fixed per-session fees,

-   Providers pay per-CID fees.

These two models are implemented, respectively, by:

-   Client-credits, which represent share from the revenue from fees, and

-   Provider-credits, which are tethered to value (e.g. a hard currency) through escrows.

A simplified version of this design can be obtained by removing the client role (and the client-credit logic). This results in a retrieval market where all participants assume the "provider" role and pay per CID. Note that being a "provider" does not obligate you to cache and sell content, thus real-world clients can use the "provider" role as well.

The upshot of the simplified design is that it is a stepping stone towards the complete design, which adds time-based subscription models. We advise that a first implementation attempt targets the simplified design.

### Technical details

#### Credit systems and double spending

Our design is agnostic to the type of credit lines used.

There are two types of credit systems in the literature, which differ in the type of the credit constraint:

-   In a bilateral credit system a credit line pertains to one borrower and one lender

-   In a multilateral credit system a credit line pertains to (one or) multiple borrowers and (one or) multiple lenders

For instance, the traditional credit card system implements a multilateral credit system, where the credit of one borrower (the customer) pertains to all lenders (the merchants of goods).

For instance, in this RFC, in the provider-credit system, providers are both borrowers and lenders.

-   If provider-credit is bilateral, each provider will need an individual credit line (and therefore escrow) for each other provider they intend to do business with.

-   If provider-credit is multilateral, each provider will need a single credit line (backed by a single escrow) for all trades regardless of counterparty.

Multilateral credit is strictly more flexible than bilateral credit. However, there is a trade off. Decentralized implementations of bilateral credit can prevent double-spending (in real time), whereas multilateral implementations detect double-spending at settlement time.

In summary:

|\
 |

Bilateral

 |

Multilateral

 |
|

Double-spending in decentralized setting

 |

Prevented

 |

Detected at settlement

 |
|

User friendliness

 |

Participants can trade directly with a small set of chosen counterparties

 |

Any pair of participants can trade

 |

Our recommendation is to use a multilateral credit system and address detected double-spending by "freezing" the account of the offender for a duration of time, proportional to the overspent amount. (The approach taken by the US Bankruptcy system.) Since penalties are assigned to on-chain accounts, for the former to be effective, accounts must be Sybil-resistant. This is addressed in the next section.

#### Sybil-resistant accounts for penalty attribution

Each participant in the retrieval market is an "account" on-chain, identified by a public key. Account identities are used to authenticate parties in a trade, as well as to assign penalties when double spending is detected. The state of an account (e.g. outstanding penalties) is stored on-chain for everyone to see.

Users may be tempted to create new accounts, when old ones are penalized, thereby performing a Sybil attack. To counter this, we utilize a mechanism that provides higher quality of service to accounts that have successfully completed a larger number of trades in the past, thereby introducing a significant cost to abandoning an existing account (e.g. to avoid penalties). This is accomplished with the following protocol:

-   The chain stores the total volume of successfully settled past transactions for every account, and

-   Providers prioritize requests, based on the historical volume of the requesting account.

#### Collusion attacks

Collusion opportunities depend on the payment model. Thus per-CID and per-session fees are analyzed separately. Both are collusion-resistant:

-   Per-CID payments are collusion-resistant because content is directly exchanged for value, so everyone's wealth is preserved after each trade.

-   Per-session payments (where client-credits correspond to a share of the deposited fee) are collusion-resistant, because (by design) all credits issued by a single client represent shares only of that client's deposited fees.

Impact

The RFC proposes a reward model that can be embedded to any of the design proposals designed for the rest of the components of the system. In particular, it does not address fair-exchange, and instead assumes that a fair-exchange protocol (for exchanging credits for content) is given.

It can be easily pluggable into any design and imposes no requirements to the rest of the components. It enables for the coexistence of several economic models over the RFC's infrastructure.

### Pros and Cons

Pros

-   Provides high-bandwidth payments, using a low-bandwidth chain. (The credit network is a "layer 2" infrastructure.)

-   Supports multiple payment models.

Cons

-   Assumes a fair-exchange protocol.

### Evaluation

Qualitative

-   The RFC increases the size of the retrieval market (demand for content retrieval) in the Filecoin network.

-   The RFC provides high-liquidity: Given a fixed supply and demand, the market maximizes the number of matches.

Quantitative

-   End-user latency and bandwidth are sufficient for good UI experience (e.g. snappy browsing).

-   Simulation analysis of the full economic model to understand the Nash Equilibrium.

-   Is there an equilibrium?

-   Is the equilibrium fair?

-   End-user overhead of retrieval.

### Prior work

-   [MicroCash: Practical Concurrent Processing of Micropayments](https://arxiv.org/abs/1911.08520)

-   [Liquidity in Credit Networks: A Little Trust Goes a Long Way](https://arxiv.org/abs/1007.0515)

-   [Liquidity in Credit Networks with Constrained Agents](https://arxiv.org/abs/1910.02194)
