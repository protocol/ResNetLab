# RIW2021 | RFC: OS-level OppNet Component

_Status:_ < draft >

_Area of Improvement:_ < Opportunistic Deployments >

_Estimated Effort Needed:_ <?>

_Prerequisite(s):_ <?>

_Priority:_ <? P0, P1, P2>

### Abstract

Provide a uniform opportunistic networking interface for applications to register and use, in a way that preserves resources and guarantees benefits flow back to the user. The component would be responsible for optimising the resources allocated to connect with nearby devices in a horizontal manner across different applications.

### Construction

- OS-level provider (think COVID contact tracing service provided by Android and iOS)
  - Avoid having multiple always-on opportunistic clients, one for each application (fragmentation & waste of resources)
  - Guarantees benefit flows to the user vs. the application developer
  - Sidesteps restrictions on background applications
  - Applications register with OS provider 
- It should be as invisible as possible, requiring little intervention/configuration by user
  - But the OS could proactively prompt users for help distributing specific content given need and revenue opportunity
- Use local information (e.g. calendar events with location) to decide which information to fetch and store
  - Look at upcoming locations and declared interests in said locations
  - Leaks minimal data; does not require sharing of contact or location data by opportunistic retrieval miner

**Open Questions**
- How do you predict what data will be desirable?
- How do you cross the border with this information?
  - Data is not encrypted to destination (that would limit efficiency). You could encrypt in-transit or time lock but is that sensible/sufficient?
- How do you communicate said interests in a privacy-preserving way?
- How do you accommodate network needs while avoiding tracking?

- Would the data be stale by the time it gets there?
  - If based on your calendar, this can easily be prevented by having timed interests
- Could machine learning help?
- Does the OS have the concept of CID? Why do it at OS level?
  - Android or iOS modules, not POSIX API
  - Intended to circumvent OS restrictions on BG activity and guarantee user benefit
  - Getting this into a mobile OS seems pretty hard, given complexity and resource requirements. Would require a critical mass so that they can’t ignore it, i.e. bootstrapping problem

### Impact

The proposed component would provide benefit to the performance of any opportunistic networking application and therefore, its impact would be significant. However, it is not an add-on software component, or application and would therefore need to be adopted (or perhaps even developed) by the OS vendor(s).

### Pros and Cons

Pros:
- The construction would provide performance benefits at the device level.
- The construction would be useful to many applications and could work simultaneously to serve all of them
Cons:
- It’s not an add-on software component and would therefore have to be integrated within the multiple OSes, i.e., OS vendors would have to accept it
- Not something that PL could implement, or make as a product

### Implementation notes

TBA

### Evaluation

Compare performance benefit of one OS-level component that serves horizontally several applications, against several applications with their own component.

### Prior work

TBA


