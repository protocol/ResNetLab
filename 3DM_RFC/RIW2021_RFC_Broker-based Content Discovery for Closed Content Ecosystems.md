# RIW2021 | RFC: Broker-based Content Discovery for Closed Content Ecosystems

_Status:_ < draft >

_Area of Improvement:_ Distribution Graph Forming

_Estimated Effort Needed:_ <?>

_Prerequisite(s):_ <?>

_Priority:_ <? P0, P1, P2>

### Abstract

This RFC targets content discovery, resolution and delivery (for closed content ecosystems, e.g., Spotify), where content cannot be consumed outside the application. The proposal is based on the fact that in the majority of cases content is linked to some “context”. This can be, for example, a video linked from a webpage. Metadata attached to the aforementioned video link point to the broker that either has a copy of the content, or knows  where to find it.


### Proposal/Construction

Assumptions of the proposal include:
- Users find CIDs through links in websites or apps.
- Content publishers play the role of the tracker. They know the retrieval miner with which they have a deal with. They tell their users to go find the content there.
- Content publishers can outsource this operation to independent brokers. For instance, the broker for my website is the hosting company. We call content publishers brokers in the following, for simplicity.
- The links resemble bittorrent-like magnet-links, which include the CID, the content publisher, or broker, as well as metadata. Metadata is a very central point in this proposal. What is included in the metadata is something to be defined, but could be the map of brokers storing the content, or a semantically meaningful name. Metadata should help with discovery. Metadata generally help finding seeders.

The architecture consists of:
1. end-users requesting content, 
2. Retrieval miners (RMs) that store content
3. Brokers that know which RM stores which content/CID
4. Websites that link to content, through a CID and metadata that help with content discovery (e.g., provide a map of brokers that know about this CID)
5. A DHT, where RMs publish provider records. The DHT is consulted by the brokers in regular time-intervals to get updated information.

The main content retrieval operation is as follows (see also figure):
1. Client finds “magnet link” from website (clicks)
2. Link returns the broker’s address
3. Client requests CID from broker
4. Broker returns RM address (alternative: broker forwards request to RM directly)
5. Client requests CID from RM (alternative: RM delivers content to client)
6. RM delivers content to client



Things that happen in the background to support the above sequence are:
1. There is a DHT where RMs publish provider records for the content they store
2. Brokers ask the DHT for the providers of the CIDs they’re keeping an index of in regular time intervals, but off band (i.e., not in real-time when they receive the request from clients). This ensures that they have up-to-date information and the request does not have to wait for the slow DHT walk in order to complete.
3. In case content becomes popular at some other network/geographic area, the local RM in that area should fetch the content and let the broker know, so that he promotes them too. It is not clear how the RM that wants to replicate content will know which broker to update. A potential solution is for this information to be included in the metadata of the magnet link so that a new RM can get it from there. In this case, there is an issue when the publisher decides to change broker.
4. The fetching of content by the new RM discussed above should be supported by the economic model: the original RM will want to satisfy all requests themselves in order to get all the profit. However, we should incentivise replication of content in order to achieve lower delivery latency.

### Impact

This is a stable and simple design for closed content ecosystems, which applies to a large proportion of applications we use today (e.g., spotify, netflix), where content is consumed from within an application (or perhaps website) and the content publisher has complete control over the content (what is published, where it is stored etc.).

### Pros and Cons

Pros
- Brokers always have up-to-date information. The fact that clients get links/CIDs through websites or applications means that a broker is never (under normal circumstances) going to be asked for a link/CID they don’t know.
- It is a clever way to cover a large number of use-cases in a safe manner, i.e., without content discovery failures.
- It can be as fast as DNS.
- Simple design

Cons
- The system seems to break if the content publisher decides to move their content to a different broker, since it is very difficult/impossible to update all websites, forums, or even browser bookmarks that include the magnet link. This will result in content discovery failure. A potential solution is for the user to go the slow path and ask the DHT. However, the above will not be an issue in “closed content ecosystems”, such as Spotify where you can’t consume content outside the application and the publisher has complete control over the content through the app.
- It is not clear how the RM that wants to replicate content will know which broker to update. A potential solution is for this information to be included in the metadata of the magnet link so that a new RM can get it from there. In this case, there is an issue when the publisher decides to change broker.
- An issue that arises is how can we update the magnet link with the list of brokers. Ex.: I don’t want to always be asking a broker very far away to let me know of RMs near me to fetch the content. How can the system assign a new broker near me to keep track of RMs near me that store the content? To my understanding this cannot happen automatically and the only way to do it is by the content publisher doing it manually.

Other notes:
- Is the scheme giving too much power to the brokers?
- How can we avoid giant broker monopolies and centralisation?
- Who is paying the broker?
- There could be a “finder’s fee” for the broker
- How do you make sure that a broker is not giving you wrong content.
- It seems that by having the broker as a very central entity that is responsible for the content discovery, the system loses the benefits of content addressing. Ex.: the system does not seem to be able to track a user near me that has the content and route my request to them instead of going to the content provider that is far away.

### Implementation notes

The design can step on existing protocols within the IPFS ecosystem. For example:

- The DHT structure -> libp2p DHT
- When connecting the client to the list of RMs, the broker can use bitswap 
- RM updates (i.e., when a new RM stores a hot copy of a content) can propagate to brokers through libp2p’s pubsub/gossipsub.

### Evaluation

**Qualitative**
- We should make sure the scheme does not result in content discovery failure. It does not seem to be the case for closed content ecosystems, but need to double-check.
- For more general content consumption we should have a backup solution. This could be that when a content publisher changes broker and therefore the metadata of CIDs in websites do not point to the right broker anymore, the request ends up going through the DHT. This can become the default way in the long run when content has moved between several different brokers.
- We should find a solution for the case where new brokers are added to the list by the content provider. What is the impact on the system? How can we update the list of brokers?

**Quantitative**
- Taking into account connection establishment, how long does it take to resolve content and start the delivery?
- How efficient is the proposal in finding the nearest replica in RMs closest to the client that has decided to keep a hot copy?
- Can we integrate “Transient Retrieval Miners” (i.e., normal peers) in the scheme?


### Prior work
Related work where the proposal takes inspiration from.
