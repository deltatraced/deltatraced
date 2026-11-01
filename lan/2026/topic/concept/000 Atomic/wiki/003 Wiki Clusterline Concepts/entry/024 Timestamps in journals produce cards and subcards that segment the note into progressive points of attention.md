---
context_type: entry
---

Parent: [lan/2026/topic/concept/000 Atomic/wiki/003 Wiki Clusterline Concepts/003 Wiki Clusterline Concepts](../003%20Wiki%20Clusterline%20Concepts.md)

Spawned by: [lan/2026/topic/concept/000 Atomic/wikiproc/003 Wiki Proc Clusterline Concepts/003 Wiki Proc Clusterline Concepts](../../../wikiproc/003%20Wiki%20Proc%20Clusterline%20Concepts/003%20Wiki%20Proc%20Clusterline%20Concepts.md)

Spawned in: [^spawn-entry-0729a5](../../../wikiproc/003%20Wiki%20Proc%20Clusterline%20Concepts/003%20Wiki%20Proc%20Clusterline%20Concepts.md#spawn-entry-0729a5)

---

One important part of the [process note](../concept/000%20Term%20Process%20Note.md) `# Journal` section is the timestamp, which looks like this:

2026-07-06 Wk 28 Mon - 20:47 +03:00

It adds chronology to the journal and gives us a way of segmenting the ideas within the journal like paragraphs segment an essay.

We say that they form a card because we interpret that everything below a timestamp forms a whole, until the next timestamp arrives. These are not meant to micromanage when something is written, but to break down the journal into a list (or tree) of points.

````
2026-07-06 Wk 28 Mon - 20:51 +03:00

AAA

BBB

CCC

2026-07-06 Wk 28 Mon - 20:51 +03:00

I like ice cream
````

Here there are two cards, one with the `AAA BBB CCC` and another with the `I like ice cream`. Later on, we might still come back and add `DDD` to the first card, even if it's days or hours later. Typically though, after a long period of time when our mind no longer holds this in attention, we just create a new card.

Timestamp cards can be added into the journal out of order, which may produce a more natural grouping of cards by relevance.

We can also use Timestamp subcards to associate them with the first above timestamp card of lower level. Because timestamp cards chain together to create a list of chronological points of attention, the subcards upgrade this to a tree.

We use `--/` before the timestamp card, and then put a `--/` at the end to clarify the segment, like so:

````
2026-07-06 Wk 28 Mon - 20:54 +03:00

Some topic I was writing on a long time ago

--/ 2026-07-06 Wk 28 Mon - 20:54 +03:00

Actually, I changed my mind 2 weeks later. It should be like this.

--/ 2026-07-06 Wk 28 Mon - 20:55 +03:00

Phew, we got it done.

--/

2026-07-06 Wk 28 Mon - 20:54 +03:00

I moved on, and now I am handling the next thing
````

Here we can see the first timestamp card has 2 timestamp subcards under it. We can have arbitrary levels, just use `--/--/` for level 2, and `--/--/--/` for level 3 and so on.

We can also use this format for more inline logs:

````
--/ 2026-07-10 Wk 28 Fri - 20:38 +03:00 | Some action
--/ 2026-07-10 Wk 28 Fri - 20:38 +03:00 | Some other action
--/ 2026-07-10 Wk 28 Fri - 20:38 +03:00 | Basically we use the vertical bar to separate logs, now they are one-per-line.
--/
````
