# RIW2021 | RFC: ZKCP with Fair Exchanges of 1 bit

_Status:_ **draft**; **~~ready for review~~**; **ready to publish**

_Area of Improvement:_ Data Delivery Metering

_Estimated Effort Needed:_ &lt;?>

_Prerequisite(s):_ &lt;?>

_Priority:_ &lt;? P0, P1, P2>

### Abstract

In a ZKCP and ZKCSP, there are (at least) two events might happen that will lead to value loss, these are:

*   The client doesn’t deliver the payment to get access to the key, creating a grieving attack on the provider of the data and/or service
*   The provider vanishes before it sells the key to the client, leaving the client with unusable cyphertext

In order to solve this, we propose a continuously running Fair Exchanges of  \
1 bit of the key throughout the delivery of the file. Giving the provider some assurance of the payment and giving the client an option to only have to brute force a few bits (O(2^k)) of the key in case the provider disappears (simpler than brute forcing the whole key)

### **Proposal/Construction**

In a ZKCP for a file, it is agreed that at each n blocks out of m total blocks, a Fair Exchange for 1 bit of the key happens. The flow goes as follows:

*   Incremental fair exchanges 
*   Exchange 1 bit of the file decryption key for 1 bit of a redeemable payment ticket/signature of the same size.
*   For both of these, all bits are necessary to make any use
*   However, if k bits are missing, the remaining bits can be brute-forced in O(2^k)
*   If either party aborts, at any round, they only have one more bit than the other party.
*   That means the malicious party only gains an advantage of 2x the compute needed to get the resource they wanted (the decryption key or the payment)

**Impact**

Saves RTT at the end for selling the key, gives assurance to both parties. Somewhat similar to pay-per-packet

### Pros and Cons

Pros
*   Reduce an additional RTT at the end, piggybacking the fair exchange on the File Transfer itself 
*   Enables the client to signal satisfaction to the provider in case the provider is not delivering on the right bandwidth (e.g. not meeting the SLA, client stops paying for more bits)

Cons
*   Added complexity and overhead and proving each 1 bit for each key

### **Implementation **notes**

TBWRITTEN

### **Evaluation**

TBWRITTEN

### Prior work

ZKCP
