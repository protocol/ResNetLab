# RIW2021 | RFC: ZKCP Optimizations

_Status:_ **draft**; **~~ready for review~~**; **~~ready to publish~~**

_Area of Improvement:_ Data Delivery Metering

_Estimated Effort Needed:_ &lt;?>

_Prerequisite(s):_ &lt;?>

_Priority:_ &lt;? P0, P1, P2>


### Abstract

TBWRITTEN

### **Proposal/Construction**

*   Assumption
    *   We assume that encrypting a file and issuing a Proof-of-Retrievability is an expensive operation
*   Optimization I
    *   encrypted the data once, then encrypt the encryption key to the client to do the fair exchange
        *   Pros
            *   Only issue s Proof-of-Retrievability once
            *   A group of providers can collaborate over a set of pre-encrypted files and issue the keys to the clients as they request
        *   Cons
            *   Opens the potential for a grieving attack, where multiple clients ask the provider to get the file, but then only one client pays for the decryption key
                *   Open Problem: Need to find a way for individual parties to be unable to share the keys to decrypt the same ciphertext.
                *    Perhaps the client specific key does one last, non expensive scramble?
*   Optimization II
    *   Only prove a small n of chunks out of N
    *   Getting n random chunks in an initial interaction for free might be fine as it would take O(n^3) time to collect the whole file that way
        *   Question for Steven: how do you prove random pieces belong to file that you want?
            *   Merkle-tree, share the merkle path
            *   If an IPLD dag is used, we could leak data on proving a path
    *   Attacker wouldn’t necessarily want to pull different pieces from multiple endpoints (expensive)

**Impact**

TBWRITTEN

### Pros and Cons

TBWRITTEN

### **Implementation **n**otes**

TBRITTEN

### **Evaluation**

TBRITTEN

### Prior work

TBRITTEN
