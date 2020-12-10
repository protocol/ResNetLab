# Networking in Heterogeneous Runtimes

## Short Description
Annual global IP traffic will reach 396 exabytes per month. The number of devices connected to the internet is expected to double in the same time. Access to high speed broadband connectivity is limited by poor last mile connections. Optic-fiber deployments are cost-prohibitive and slow to deploy [1](https://www.cisco.com/c/en/us/solutions/collateral/executive-perspectives/annual-internet-report/white-paper-c11-741490.html).

Edge  computing  has  emerged  as  a  distributed  computing paradigm to overcome practical scalability limits of cloud computing.  The main principle of edge computing is to leverage computational resources outside of the cloud to perform computations closer to data sources, avoiding unnecessary data transfers to the cloud and enabling faster responses for clients. Given the enormous amount of data that is expected to be produced at the edge of the network (by end-user devices), the edge-computing principle builds on the fact that “it is cheaper to bring computation to data, rather than data to computation”.

Data storage and computation points at the edge of the network, be it in cell stations, WiFi access points, or even the same user devices that produce the data ultimately form a peer-to-peer network.

*Both IPFS and libp2p can have a pivotal role in this new trend of distributed technologies and expansion of the Internet beyond traditional boundaries*. Both technologies already have implementations in several programming languages and are compatible for their execution in a great gamut of runtimes (desktop, browser, cloud, etc.) and network conditions (public IP addresses, behind NATs, etc.). Nevertheless, there is significant work to be done to make IPFS and libp2p compatible with every runtime and network condition in a way that offers a flawless experience, just like one expects when using the net or http package of any language. This open problem is built upon *the vision of making libp2p / IPFS modules and protocols the de-facto distributed substrate for connected devices in the near-future Internet*.

At high-level, the aim of this research endeavor is to:

-  Enable libp2p to leverage the available protocols from the multiple runtimes, so that it can execute and form a p2p network. This will require a coordinated approach to cover as much ground possible (possible approach inspired by [Are we yet?](https://wiki.mozilla.org/Areweyet) initiatives)
-   Allow global connectivity between devices. Any two libp2p nodes should be able to communicate using a compatible network link available without having to rely on a connection to the broad Internet (e.g direct connectivity through wireless transports, mesh connectivity, etc.). 
-   (Bonus) Libp2p/IPFS should include a general execution framework to be easily extensible for any runtime using the same format.
  -   [Extensibility through Web Assembly](https://istio.io/latest/blog/2020/wasm-announce/)
  -   [Web Assembly for proxies (ABI specification)](https://github.com/proxy-wasm/spec)
  -   [Self-described blocks (embedded IPLD codecs)](https://hackmd.io/AAfd9WnWQZSC7HT7Pr5G9A?view)

## Long Description
This open problem can be divided in the following subproblems:

### 📲️ Area I: Runtimes
The aim of this open problem would be to have libp2p and essential IPFS modules and protocols running in different runtimes (ensuring their compatibility). A lot of work has already been done around this line with libp2p and IPFS running in the browser and implemented in several programming languages.

By the end of this research effort we should consider at least supporting the following runtime categories (each category includes a list of related projects):
-   Browsers and Desktop
-   Embedded systems (IoT, low powered devices)
    -   [IPFS Tiny](https://gitlab.com/librespacefoundation/ipfs-tiny/-/wikis/home)
    -   [Embed-ipfs](https://github.com/ipfs-rust/ipfs-embed)
-   Routing devices:
    -   [OpenWrt](https://openwrt.org/)
    -   [Gotenna](https://gotenna.com/)
    -   [Liberouter](http://www.liberouter.mobi/)
-   Mobile devices:
    -   [Thali Project](http://thaliproject.org/)
-   VR/AR devices.
-   Trusted Execution Environments
    -   [Enarx](https://github.com/enarx/enarx/wiki/Enarx-Introduction)
    -   [T-Rust](http://t-rust.com/#/): [TEA (Trusted Execution and Attestation)](http://t-rust.org/#/doc_list/What_is_TEA%3F%2FREADME.md)

We may be able to support many of the aforementioned with the same protocol implementations (e.g. routing devices and embedded systems). This work will set a strong foundation for the rest of the projects to come.

#### What defines a complete solution?
- TBD

#### References: 
-   Support for libp2p intra process: <https://github.com/libp2p/libp2p/issues/61> 
-   Build for gomobile: <https://github.com/libp2p/go-libp2p/issues/529> 
-   Use of vias for protocols: <https://github.com/libp2p/notes/issues/12>
-   Enarx SDK: <https://github.com/enarx/enarx/wiki/Enarx-Introduction> 
-   Are we distributed yet? <https://arewedistributedyet.com/> 
-   Issues listing open problems to support full distribution: <https://github.com/arewedistributedyet/arewedistributedyet/issues>

### 📦️ Area II: Data Link / Network Connectivity
Apart from being able to run libp2p nodes anywhere, we should also be able to connect two libp2p nodes using different types of connectivities. Not every node will have a proper Internet connection, or be able to directly connect to every single peer using the exact same connectivity data link. Analogously to how it is currently done in libp2p at a transport level with the transport upgrader, we should have similar support for different data link technologies. Nodes will try to fallback to the common data link for connectivity. Some of the data link technologies we should consider supporting are:

-   Wireless communications
    -   Bluetooth, Zigbee, WiFi direct.
-   Side-channels
    -   NFC, RFID.
    -   Audio waves
-   Relays and bridges 
    -   See the next section.

#### What defines a complete solution?
-   Demonstrate that two libp2p nodes within range of each other can discover each other:
    -   through a local, fixed, network element, such as WiFi access point, raspberry pi, or similar
    -   directly, without relying on any fixed network element, i.e., device-to-device (D2D)
    -   the devices can connect on demand, i.e., when one of the two devices wants to request (pull) or send (push) content from the other
-   The setup and supporting protocol stack should make use of at least one of the wireless connectivity technologies mentioned above.
-   Bonus: the protocol setup should enable the option of seamless connectivity between the devices, that is, without the user having to accept incoming connections as is currently the case with bluetooth pairing. This is needed to enable large-scale mesh-networks and applications to disseminate content in mobile environments.

#### References: 
-   Alternative transport / discovery protocols for libp2p: <https://github.com/libp2p/notes/issues/6> 
    -   Bluetooth: <https://github.com/libp2p/js-libp2p/issues/261>
        -   <https://github.com/noble/noble>
-   Wave-share: <https://github.com/ggerganov/wave-share>

### 🛣️ Area III: Routing

One of the big aims of this open problem is to achieve global connectivity between libp2p nodes. Supporting several runtimes and connectivities will allow "anything" to become a connected libp2p node. Unfortunately, to be able to connect any two libp2p nodes independently of their network setting and location, and to offer node mobility, additional libp2p routing infrastructure may be needed (e.g. for the translation of data links, or bridge unreachable network segments). This infrastructure will be responsible for discovering and forwarding messages to directly unreachable peers. In this project, we can open several lines of research and exploration:
-   Relay and bridges
    -   Use bridges as gateways for low-powered device with limited support for network ([IPFS tiny bridges](https://gitlab.com/librespacefoundation/ipfs-tiny/-/wikis/home))
    -   Libp2p routers and gateways to discover and forward data to peers at unreachable segments of the network (extending the current implementation of relays).
-   Mesh networking.
    -   Hierarchical routing of libp2p traffic through the construction of interconnected local mesh networks.

#### What defines a complete solution?
-   Set up an environment where low-powered devices can communicate between them through a gateway of some form (e.g., a WiFi access point).
-   Demonstrate that one device can send data to the other, on demand.
-   Bonus: more powerful mobile devices can act as relays, realising a multi-hop environment.

#### References: 
-   OpenR - Distributed routing protocol for mesh networks: <https://github.com/facebook/openr> 
-   Liberouter: <http://www.liberouter.mobi/>
-   OpenWrt - Embedded firmware OS: <https://openwrt.org/>

### 📴️ Area IV: Offline-first

Libp2p and IPFS nodes should be able to work offline-first and offer capabilities to easily implement offline-first applications and protocols over them. Being offline first means being able to easily recover from network interruptions, and operate seamlessly under unstable network conditions. To achieve this we may require the use of CRDTs and "synchronizable" datastores.

#### What defines a complete solution?
-   Assume an environment where several mobile devices run the same application and the application content between devices needs to be synchronised at all times. When one of the devices produces new content, the content should propagate to the rest of the devices realising a "push" model.
-   Setup an environment with a sample application that produces content every so often.
-   The devices can either be permanently connected in a mesh, or connect on demand and transfer content to each other.
-   As nodes propagate content, they have to optimise the amount of data they exchange. Instead of just flooding full messages, nodes should first check whether their neighbours already have the latest content items. This can be achieved through CRDTs, or bloom-filters. At least one should be demonstrated.

#### References: 
-   Improve offline support: <https://github.com/libp2p/notes/issues/16>
-   Offline message queue: <https://github.com/libp2p/notes/issues/2>
-   IPFS CRDT datastore: <https://github.com/ipfs/go-ds-crdt>
-   Optimize Circuit Relays to make it packet-oriented in libp2p: <https://github.com/libp2p/go-libp2p/issues/1019>
-   NAT Traversal effors around libp2p: <https://github.com/libp2p/go-libp2p/issues?q=is%3Aissue+is%3Aopen+NAT+Traversal>

### 🛠️ (Bonus) Area V: Extensibility / P2P VM a.k.a InterPlanetary Runtime
This area of research is still a work in progress, and it may becoming a full-fledge open problem of its own, but for now we still consider it as an area inside the heterogeneous runtime problem.
Maintaining and growing libp2p protocols for the gamut of runtimes and connections we want to support can be cumbersome. This is why we should consider embedding a common execution framework to every libp2p implementation so libp2p modules are compiled targeting this common framework and can be thus used in any of the runtimes. With this, protocols and modules are implemented once and used anywhere.

Not only protocol implementations can benefit from embedding an *"InterPlanetary Runtime* in libp2p nodes, but also any other standard computation in the ecosystem such as IPLD codecs. IPLD codecs can be implemented targeting the *IP Runtime* so they don't have to also be implemented for every libp2p implementation and runtime. This opens the door to future lines of research such as *self-describing blocks*, where the block descriptions embeds the type of data and a pointer to the codec (and which can be another block in stored in the network); *content-addressable code executions*, where code snippets are stored in the network, and data is specified in the code as pointers to blocks in th network; *computation delegation*, as once every node in the network shares the same common runtime and has a way to retrieve code and data directly from the network with a standard interface, delegating the execution of code to other's is as easy as sharing a link to the code and the data. Some of these themes, such as the use of *self-describing blocks*, although useful for the interoperability and extensibility problems of networking heterogeneous runtimes, they belong to the [Distributed Type Systems](https://github.com/protocol/resnetlab#areas) open problem, and may be finally tackled there.

#### What defines a complete solution?
-   TBD

### References
- Wasmtime (Wasm runtime): <https://wasmtime.dev/>
- UnisonWeb: Example of content-addressable programming language: <https://www.unisonweb.org/>
- Wasmer (Wasm runtime): <https://wasmer.io/>
- WASM ❤️ IPLD: <https://hackmd.io/AAfd9WnWQZSC7HT7Pr5G9A?edit>
- For this project we can take inspiration from the[Wasm extensibility approach](https://istio.io/latest/blog/2020/wasm-announce/) currently introduced in Istio and Envoy. Thus, our libp2p node implementations would expose the basic operation with a common runtime framework from which new Wasm modules are instantiated and run to extend the operation of our node with additional modules and protocols.
- Wasm bindgen designs: <https://rustwasm.github.io/docs/wasm-bindgen/contributing/design/index.html>

### 🛰️ (Bonus) Area VI: High Latency Connection (e.g. Space)
In the future, we may want to extend the edge computing trend into space, and follow the same approach we've followed to support libp2p for existing runtimes and link connectivities but considering the requirements and limitations of space communications. This research are may also become in the future its own open problem.

#### What defines a complete solution?
-   TBD

#### References: 
-   Libp2p for space.
-   [[2021 Theme Proposal] IPFS ❤️ Starlink](https://github.com/ipfs/roadmap/issues/72#)
-   SpaceX Starlink: <https://github.com/ipfs/notes/issues/432>
-   Issue with ideas: <https://github.com/ipfs/notes/issues/432#issuecomment-639801794> 
    -   Software update among satellites
    -   Load-balancing and rerouting when sat failure
    -   Sky-based bootstrap/ Peer discovery, if Starlink is cooperative
    -   **Retrieval Market Filecoin Nodes (@adlrocha: my personal favorite)**
    -   Organic CDN (because of IPFS)
    -   Provider paid CDN ('coz offer and demand, baby)
    -   Open source embedded devices coms / sat coms
-   Efficient Telemetry Storage on IPFS: <http://blog.klaehn.org/2018/06/10/efficient-telemetry-storage-on-ipfs/>

### Additional areas of research to potentially explore.
Other research areas and projects that may land this heterogeneous runtime open problem. Additional inspiration can be taken from [this paper](https://asc.di.fct.unl.pt/~jleitao/pdf/NewEdgeApplications.pdf).
-   Libp2p in 5G RAN infrastructure
-   Distributed monitoring
-   Collaborative services in libp2p (execution, monitoring, relaying).
-   Distributed Resource Management.

## Use cases
### Use Case I: User-Operated Mobile CDN
#### Brief Overview
Assume an environment where content publishers publish content through mobile phone applications. Content publishers utilise the mobility of users, the mobile device storage and connectivity opportunities to disseminate content. Users’ devices connect with each other as they move around to check whether their app instance has the latest content available and if not they update with the neighbours’ content.
In this use case, we explore the potential of a user-operated, smartphone-centric content distribution model for smartphone applications. In particular, we assume source nodes that are updated directly
from the content provider (e.g., BBC, CNN), whenever updates are available; destination nodes are then directly updated by source nodes in a device-to-device (D2D) manner. We leverage on sophisticated information-aware and application-centric connectivity techniques to distribute content between mobile devices in densely-populated urban environments.
#### Business case
Content publishers utilise storage and connectivity opportunities of mobile devices as a medium of content distribution and save from CDN costs. They pay users for the amount of storage and mobility opportunities that they contribute to the mobile CDN.

See [this paper](https://drive.google.com/file/d/1UpH6r3Q0gKaf00CgEImgK_SImENR8NQ9/view) for more details.

### Use Case II: Local Social Network
#### Brief Overview
Imagine a crowded entertainment or business event, such as a concert, sports event, or conference. People gather having the same interest, that is, “consume” content from the event. Everyone is capturing content on their mobile device and everyone is interested in the best content available. Connectivity in these environments is normally terrible, if existent at all. Offline-first, D2D connectivity can facilitate content propagation between devices in the area.
#### Business case
Event organisers can save costs from expensive infrastructure setup (e.g., mobile cell stations, WiFi access points and bandwidth capacity) and instead subsidise users’ resources to disseminate content.

### Use Case III: Industry 4.0.
#### Brief Overview
The next generation of factory networks is expected to rely on smart, automated setups for the operation of production lines, robots, sensors and actuators. Connectivity between devices can be intermittent due to challenged environments, e.g., mines, or intentionally disconnected for security reasons. At the same time, connectivity and communication between devices needs to be stable given the expensive and time-critical operations.
#### Business case
Factories and industrial plants are an aggressive environment for wide-range wireless communications. Companies generally deploy private LTE networks to meet their needs of connectivity and performance.

### Use Case IV: Mobile Active Networks - aka Ubiquitous code
#### Brief Overview
Traditionally, applications are installed in the end-user devices. With mobile active networks we propose that instead of having to download an application in order to use it natively, users can just send a request to the network to run the application. The nearest devices would serve the code in the right format for the user to run the application seamlessly in his device. Instead of having to install apps, we “pin” apps. Furthermore, if the application in hand is computationally intensive, a device can delegate computations (like graphics rendering) to other capable devices in the surroundings (this is why I mentioned that the code is served in the “right format” according to the device. The code may include pointers to primitives that require the delegation of computation). This use case merges the Stadia approach (you don’t need hardware to play videogames because everything runs on the edge) with the browser (running an application means searching for an address to download the content).
#### Business case
Users in the edge can share their spare bandwidth/computational resources. Remove the need for expensive end-user devices to run expensive applications. Instead of storing code the device only stores user’s application-related data. This opens the door to the seamless sharing of data (in IPLD format) between different applications, and local-first applications. 
