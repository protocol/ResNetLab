

# RIW2021|RFC: ZK-friendly IPLD data model

_Status:_ &lt;**draft**; **~~ready for review~~**; **~~ready to publish~~**>

_Area of Improvement:_ &lt; Data Delivery Metering | Distribution Graph Forming | Opportunistic Deployments | Cryptoeconomics >

_Estimated Effort Needed:_ &lt;?>

_Prerequisite(s):_ &lt;?>

_Priority:_ &lt;? P0, P1, P2>


### Abstract

Verifying a CID for a full IPLD data model can be complex and computationally extensive. Building an ZK-compatible IPLD data model, would allow the inclusion of a ZK proof of the CID avoiding the verification computation. Using ZK-compatible hot copies for 3DMs would increase the ability of generating proofs, benefiting the design of the client metering protocol.


### **Proposal/Construction**

In order to verify data structures inside a SNARK, the computations that have been done to calculate the hash has to be redone inside the SNARK. Currently IPLD does not use SNARK friendly hash functions, nor repeatable data structures in many places. For instance, any change in the structure of building the Merkle Tree or how the hash of a file is constructed means writing a new SNARK circuit, increasing complexity and cost. A large part of decisions on data structures for hashing and hash functions in Filecoin have been designed to work well with SNARKs.

This RFC proposes the design of ZK-compatible IPLD structures so proofs can be embedded on IPLD structures. This is a complementary proposal that may benefit the design of other constructions and protocols of 3DMs.

IPLD -> Raw bytes -> Encrypt data -> Merklize with Poseidon Hash -> ZKCP protocol proof -> Decrypt -> Generate IPLD -> Verify 

**Impact**

It makes hot copies in 3DMs ZK-ready, opening the door to the design of proof-based protocols.


### Pros and Cons

**Pros**



*   Creates a data model in which generating proofs is easier than in the current IPLD model.
*   Eases the construction of protocols that require proof over the data in the retrieval network.
*   It has been done successfully in Filecoin.

**Cons**



*   Requires changes over the current IPLD structure.
*   It may be hard to impose all the data served through the retrieval network to follow this structure. Additional encoding may be required to serve the data in this format into the network.


### **Implementation **n**otes**

&lt;If possible, some pointers on how to implement the proposal>


### **Evaluation**

Generating proofs and validating data in IPLD structures can be done in an efficient way.


### Prior work



*   [Filecoin Merkle Proofs](https://spec.filecoin.io/#section-algorithms.sdr.merkle-proofs)
