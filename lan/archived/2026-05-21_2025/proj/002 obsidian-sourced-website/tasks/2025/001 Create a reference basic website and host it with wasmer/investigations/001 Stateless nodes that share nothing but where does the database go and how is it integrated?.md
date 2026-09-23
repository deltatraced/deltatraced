# 1 Journal

* [ ] 

From [^spawn-invst-f3bfe8](001%20Stateless%20nodes%20that%20share%20nothing%20but%20where%20does%20the%20database%20go%20and%20how%20is%20it%20integrated%3F.md#spawn-invst-f3bfe8) in [3.1 Follow along wasmer documentation](001%20Stateless%20nodes%20that%20share%20nothing%20but%20where%20does%20the%20database%20go%20and%20how%20is%20it%20integrated%3F.md#31-follow-along-wasmer-documentation)

2025-08-26 Wk 35 Tue - 23:45

In [wasmer architecture](https://docs.wasmer.io/edge/architecture),

Nodes follow the [shared-nothing architecture](https://en.wikipedia.org/wiki/Shared-nothing_architecture), so we expect that a request can be fulfilled by a single node, and each node can provide full independent service.

In [Wasmer Distributed Networking (DNET)](https://docs.wasmer.io/edge/architecture#wasmer-distributed-networking-dnet),

They mention that its principles include being fully stateless:

 > 
 > Control planes add complexity and create single pointers of failure thus if one is able to deliver the same functionality without a control plane then it is a better design.

So this may not apply to nodes specifically.

In fact each node is a distributed monolith, which is meant to include [Wasmer Storage](https://docs.wasmer.io/edge/architecture#wasmer-storage). But not much on this is explained at this time.

2025-08-27 Wk 35 Wed - 00:10

Spawn [Drawing 2025-08-26 23.59.32.excalidraw](../../../../../../../2026-05-21_2026/main/drawings/Drawing%202025-08-26%2023.59.32.excalidraw.md)

![Pasted image 20250827001107.png](../../../../../../../../../attachments/Pasted%20image%2020250827001107.png)

So this is one idea, where we create a cluster of nodes, each containing only one stateful db node they communicate with.

This should not break the shared-nothing constraint, because each node can be assumed to have its own independent hardware. All state is only stored by the DB node. And there is no single point of failure database instance, because sync can happen between all db nodes. We could run some fault tolerance algorithms here, like best-2-of-3 of 3 duplicate processes fulfilling a user request that depends on the db. And all 3 node clusters must agree, or at least 2 of 3, or the operation is deemed a failure. On pass, information syncs between other triplet clusters.

Anyway we need to keep in mind that each node here is likely a fully functional service and we're trying to scale service here. Any further module break-up is on the application level, and not the service level.

Still I am trying to understand how we can go about ensuring that all spawned services give the user the same user account information for example without a single point of failure node.

### 1.1.1 Backlog
