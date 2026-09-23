---
context_type: entry
---

Parent: [000 Wiki OSS Contribution](../000%20Wiki%20OSS%20Contribution.md)

Spawned by: [000 Wiki Proc OSS Contribution](../../../wikiproc/000%20Wiki%20Proc%20OSS%20Contribution/000%20Wiki%20Proc%20OSS%20Contribution.md)

Spawned in: [^spawn-entry-2f4e45](../../../wikiproc/000%20Wiki%20Proc%20OSS%20Contribution/000%20Wiki%20Proc%20OSS%20Contribution.md#spawn-entry-2f4e45)

---

2026-06-02 Wk 23 Tue - 09:03 +03:00

In general,

* I see many `?` where a parameter can be missing, and it doesn’t seem to always make sense. For example, check `LuaCollectionQuery` in `@silverbulletmd/silverbullet/client/space_lua/query_collection.ts`. Why is `distinct` optional? It is a boolean, if it is not provided, does that mean it takes on a default value of false? It is not specified, and it is unclear why all these fields *need* to be optional. Is it just a developer habit to not have to specify everything when building these objects? This is what I assume.

In `@silverbulletmd/silverbullet/plug-api/syscalls/mq.ts`,

For `getQueueStats` it is unclear why `queue` is optional. If no queue is specified, what are we getting `MQStats` for? It doesn’t specify.
