# RIW2021 | RFC: Consume Global, Serve Local

_Status:_ < draft >

_Area of Improvement:_ < Opportunistic Deployments >

_Estimated Effort Needed:_ <?>

_Prerequisite(s):_ <?>

_Priority:_ <? P0, P1, P2>

### Abstract

This RFC addresses the use case of heavy content distribution (ideally video) in local neighborhood environments, where the Retrieval Miner (RM) is connected to residential users. The content catalogue of a service (say Netflix or the national broadcaster) is split in chunks and stored in end-user home devices. The RM is connected to a critical mass of tens of thousands of local users and redirects requests to serve content locally.

### Setup & Assumptions

- Picture the environment where the ISP’s DSLAM/GPON is connected to ~20k-40k houses. Now, replace the ISP’s DSLAM/GPON with the Retrieval Miner’s (RM) gear
- Consider a content publisher (e.g., Netflix, or local national broadcaster) who has a video content catalogue.
- The videos are split in chunks and stored in the residential premises. This can be in user devices (laptops, desktops), wifi APs or set top boxes.
- Depending on what user devices we consider, the connections can be quite stable, but we still consider the environment opportunistic, as users might disappear, stop serving  content, or unplug things from mains.
- The RM would need to have random access to the content, content would need to be in small chunks so that it is served from several users and avoid saturating one user’s uplink. Erasure coding can help with collecting pieces from several peers.


### Construction

1. Clients request content to their local RM by CID.
2. The RM keeps track of which users store what content (identified by CID).
3. The RM redirects requests to the users that store the requested CID
4. Users return content to the client through the RM.
5. Given that the RM sees all the content flowing through itself, it can judge whether the user’s uplink is saturated and redirect to other users (if available), or apply DASH to reduce the rate.
    - If the users are within WiFi proximity of each other, they can connect directly.
    - The transfer can avoid going through the overlay RM and connect directly through the ISP’s infrastructure, but in this case, the RM will not be able to monitor, adjust rate and learn about new peers that have replicated the content. It is not clear how much faster the direct connection can be.
6. If neighbours can support the requesting user’s HD, then all good, but if not, the client connects back to the main server or higher-tier retrieval miner. This is monitored by the RM who can make the decision whether to continue streaming by the local peer, or go to a higher-tier retrieval miner.
7. The local RM can keep local copies of very popular content.
    - When edge node saturation reaches “critical mass”, RM no longer needs to store if kept in higher order cache (ie: Europe -> Germany -> Neighborhood/City)
    - RM can assess rate of delivery, and then determine better options
      - Ie: netflix dropping quality from 1080p to 480p for faster delivery



- Both RMs and end-users serving content should be rewarded and therefore supported in the cryptoeconomic model.
- What if peers cut the RM out of the loop?
  - They will likely experience reduced quality, as the RM cannot monitor and adjust
  - [Wi-Stitch](https://dl.acm.org/doi/10.1145/3098208.3098211) paper is related.

### Pros and Cons

Pros:
- Product-oriented solution that is easily implemented in a content-addressable network
- Provides huge bandwidth savings and benefits to ISPs
- Decreases load and delivery times for end-users
- Decreases storage and delivery costs for the content publisher

Cons:
- Privacy issues not solved in current RFC
- Upload bandwidth saturation needs to be avoided and a smart algorithm plus evaluation needed for that
- Unclear where the RM should be located in the network topology

### Implementation notes

- Metering and trust model
  - We can consider a model where the original content publisher (e.g., netflix) gets notified by the application itself, when the user clicks ‘play’. This helps a lot with accounting.
  - How about accountability and the fact that the RM or other content providers/caches can send random bits? The original publisher could provide a key to decrypt the content, or check the hash of the whole content, or some random parts of it or some combination.
  - QoS can be checked by the client-side media player. The above technique with “surprise checks” can also be done to check if content is delivered timely.

- Connectivity and bandwidth
  - Clients connect to a provider, who has deployed a cluster of nodes and the question is how do clients get connected to their neighbours.
    - Through the RM who is connected to everyone.
    - Preconfiguration of local peers with the local RM.
  - How can app developers control the rate? How does the switchover happen, when my neighbours’ bandwidth is not enough to serve HD video requests?

- Privacy
  - Opportunistic setups are not privacy-friendly, generally speaking. Neighbours can see what one is requesting.
    - There are simple encryption techniques that are not super strong, but can make things better.
    - Another idea is to hash the hash of the content, as has been discussed for IPFS denylists.

### Evaluation

- Upload capacity of users should not be saturated. Evaluation is needed to identify at which point QoS suffers
- Assess erasure coding based approaches and their benefits to avoid saturation of uplink
- Time to First Byte compared to a traditional CDN setup


### Prior work

- Suh, K., Diot, C., Kurose, J., Massoulie, L., Neumann, C., Towsley, D., Varvello, M., [Push-to-peervideo-on-demand system: Design and evaluation](https://ieeexplore.ieee.org/abstract/document/4395129?casa_token=QVwdZGNLekoAAAAA:WIbAVHxdIRvKRj1AY6AsamLcdvJwNSTP6oglwYLb1iRjg_QxxlivFdmUFjHQ7PjLEar9hUahFQ), IEEE Journal on Selected Areas in Communica-tions, 25(9), 2007.
- [WiStich](https://dl.acm.org/doi/10.1145/3098208.3098211)
- Amiri, M.M., Gündüz, D., [Fundamental limits of coded caching: Improved delivery rate-cache capacitytradeoff](https://ieeexplore.ieee.org/abstract/document/7782423?casa_token=nq0PcmikCKAAAAAA:kAgWYU8gjIm2wiIRgrbOHquPzsfcCGBuX4pgqzZ97rWYq0aSwjSlFYNzyOnFyVxPchj134lcGQ), IEEE Transactions on Communications, 65(2):806–815, 2017.
- Anjum, N., Karamshuk, D., Shikh-Bahaei, M., Sastry, N., [Survey on peer-assisted content delivery networks](https://www.sciencedirect.com/science/article/pii/S1389128617300464), Computer Networks, 116:79–95, 2017.
- Shanmugam,  K.,  Golrezaei,  N.,  Dimakis,  A.G.,  Molisch,  A.F.,  Caire,  G., [Femtocaching: Wireless content delivery through distributed caching helpers](https://ieeexplore.ieee.org/abstract/document/6600983?casa_token=kLqwrjL-QDAAAAAA:kBs_OkhEFTMoByEoCKXqUM_EKojv_GgV27VC2hdwz7YlLl3T6ggjDAR-G-D1Zx3MYREzIIFdpg), IEEE Transactions on Information Theory,59(12):8402–8413, 2013



