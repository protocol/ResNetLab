# RIW2021 | RFC: Optimistic ZKC(S)P

_Status:_ **draft**; **~~ready for review~~**; **~~ready to publish~~ **

_Area of Improvement:_ Data Delivery Metering

_Estimated Effort Needed:_ &lt;?>

_Prerequisite(s):_ &lt;?>

_Priority:_ &lt;? P0, P1, P2>


### Abstract

In this RFC, we propose a way to rely on the ZKCP only for resolving a dispute when one of the parties decides to go rogue at the end, only incurring the cost of issuing the proofs in case of misbehavior.

Note: Watch the full recording with Davidad’s presentation as it is a very complete description (LINK HERE)

### **Proposal/Construction**

*   On a payment channel:
    *   Agree on the query - both parties sign
        *   In case the query is simply retrieving a CID, then privacy can be maintained even for a dispute, by revealing only the hash of the query to the chain
    *   Agree on the prices
    *   Buyer deposits enough coins for query execution + bandwidth + overhead
    *   Seller executes query
        *   If seller aborts at this point, query times out and coins return to buyer
    *   Seller sends result, in encrypted chunks
        *   If seller aborts at this point, buyer goes to on-chain dispute resolution
    *   Buyer acknowledges receipt of encrypted chunks & pays per-chunk for bandwidth
        *   If buyer aborts at this point, seller is out of luck - reputation risk
        *   Possibly, buyer could also report the speed (timestamping the ACKs)
            *   If buyer starts reporting slower than it actually is, the seller will throttle to match
    *   Seller reveals decryption key
        *   If seller aborts, buyer can go on-chain and claim a slash against the seller (and seller can reveal decryption key, with a proof, to “appeal” the slash)
    *   Buyer verifies query satisfaction
    *   Buyer acknowledges receipt of decryption key 
        *   If buyer aborts, seller goes to on-chain and makes a proof

**Impact**

TBWRITTEN

### Pros and Cons

TBWRITTEN

### **Implementation **notes**

TBWRITTEN

### **Evaluation**

TBWRITTEN

### Prior work

ZKCP, ZKCSP
