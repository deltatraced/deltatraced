---
parent: '[[000 Implement the Event Accumulator]]'
spawned_by: '[[004 Choosing accumulations or events as keys in events]]'
context_type: judgment
status: done
---

Parent: [000 Implement the Event Accumulator](../000%20Implement%20the%20Event%20Accumulator.md)

Spawned by: [004 Choosing accumulations or events as keys in events](../tasks/004%20Choosing%20accumulations%20or%20events%20as%20keys%20in%20events.md)

Spawned in: [^spawn-jdgmt-fe09db](../tasks/004%20Choosing%20accumulations%20or%20events%20as%20keys%20in%20events.md#spawn-jdgmt-fe09db)

# 1 Judgment

In order to prevent [(1) complex partial accumulations](../inferences/003%20Use%20of%20event%20keys%20in%20events%20would%20result%20in%20complex%20partial%20accumulations.md) and ensure joins are standard and correct where one record refers to a [(2) unique historical record](../inferences/004%20IDs%20in%20events%20must%20be%20to%20a%20unique%20object,%20which%20cannot%20be%20object%20id%20for%20a%20historic%20table.md), all IDs in events must either be to a non-historical table or a historical table that is the product of event accumulation and not events themselves.

# 2 Reasons

(1)

2025-10-03 Wk 40 Fri - 05:06 +03:00

Spawn [003 Use of event keys in events would result in complex partial accumulations](../inferences/003%20Use%20of%20event%20keys%20in%20events%20would%20result%20in%20complex%20partial%20accumulations.md) ^spawn-infer-c52a0d

(2)

2025-10-03 Wk 40 Fri - 05:11 +03:00

Spawn [004 IDs in events must be to a unique object, which cannot be object id for a historic table](../inferences/004%20IDs%20in%20events%20must%20be%20to%20a%20unique%20object,%20which%20cannot%20be%20object%20id%20for%20a%20historic%20table.md) ^spawn-infer-0bd9ad

# 3 Related

This judgment relies on having made the judgment [000 To materialize grouped events and accumulated objects into tables via software](000%20To%20materialize%20grouped%20events%20and%20accumulated%20objects%20into%20tables%20via%20software.md)

# 4 Journal
