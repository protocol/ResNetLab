# RIW2021 | RFC: On-Demand Opportunistic Resource Deployment

_Status:_ < draft >

_Area of Improvement:_ < Opportunistic Deployments >

_Estimated Effort Needed:_ <?>

_Prerequisite(s):_ <?>

_Priority:_ <? P0, P1, P2>

### Abstract

The goal of the proposal discussed here is to match supply to demand peaks by deploying resources around specific needs, on-demand. We can think of this as a swarm of collocated users that are “called in” when demand for storage or bandwidth increases. A potential extension could be plug-n-play devices (e.g., Raspberry Pis) that run dedicated webapps to serve local demand.

### Construction

- Two user categories: (i) users with resources but not full Provider nodes; (ii) Provider nodes that only aim to provide short-term service driven by high/profitable demand.
- A potential third user category would be plug-n-play devices, such as Raspberry Pis that run dedicated software/webapps and can provide support for local demand, when plugged in.
- There is no mobility issue in this scenario. The main concern is how to incentivise the deployment of capacity to address specific local surges in demand (not unlike Uber surge pricing).
- Requires some infrastructure to distribute information related to real demand (not publisher requests). Could be a gossip market of deals.

**Open Questions**

- How do we price short-term needs? (out of scope of this RFC)
- What are the steps to advertise as a provider?
- How does someone analyse the network to determine needs?
- How does a Provider recruit, or “call-in” devices on demand? Do they listen to some channel constantly and “wake-up” when they receive some specific beacon?
- What are example applications here?
  - Are floating-content applications applicable?
  - Is message propagation in disaster-scenarios applicable?
- Can we listen to existing CID requests?
- Is there a way to provide advance information, pre-empt actual needs?
  - This would need to be vetted
- This service has customers: clients and publishers. If publishers pay, it’s easy to call Providers to the event.

### Impact

It would be nice to have “pop-up” capacity on-demand. Many interesting applications can be built on top, such as floating content-like applications, message propagation in disaster scenarios, where the network is down, or even “local social networks” in large events.

### Pros and Cons

Pros:
- Applies to several use-cases where it can enable communication and applications that are not possible today.

Cons:
- Tricky to implement: how would devices be “called in”?
- Difficult to integrate an economic model into this. The return for users being “called in” every once in a while will be minimal and therefore, users will likely not bother. It could apply in cases where there is a real need (e.g., disaster), or a common spirit/vision (e.g., football club/stadium)

### Implementation

Implementation through different wireless media (e.g., WiFi Direct vs Bluetooth) need to be investigated. There are different performance limitations by using each one of them

### Evaluation

Evaluate how quickly can a swarm of on-demand devices be formed and start contributing to the network.

### Prior work

[Composable Distributed Mobile Applications and Services in Opportunistic Networks](http://www.netlab.tkk.fi/~jo/papers/2018-06-wowmom-composable-apps.pdf)


