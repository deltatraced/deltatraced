# Journal

2026-08-03 Wk 32 Mon - 11:26 +03:00

I think the integrity of items like `LGT. ARMOR PLATING` is all they provide. Reading the manual's survival tips, the core can be protected by the attached items. This item has 90 integrity, so it probably should always be replaced by any utility item of 90 integrity or more.

--/ 2026-08-04 Wk 32 Tue - 12:30 +03:00

Need to also take into account coverage of this item. It has coverage of 150 (21%).

--/

2026-08-03 Wk 32 Mon - 12:27 +03:00

Can the core get damaged if all component slots have attachments?

You can notice which attachment takes damage instead of the core. If it's being destroyed it will flash red.

Yeah whenever I am attacked, one of the attachment status color blocks starts pinging.

But I observed losing core HP even when having full attachments sometimes. So it is not 100% protection it seems.

When we're attacked by multiple enemies, we can also see multiple attachments ping status, since they are hit. Some

Let's try to count frequency, counting on every enemy attack from the last time my core was hit, the number of times attachments absorbed the hit. Some flags:

* `d` for last attachment was destroyed and we got hit too.
* `h` Attachments were pinged (`H` for multiple) and we still got hit.
* `m` for multiple enemies engaged

````

04 02dm 01mH 00mH 02md 01mh 00mh 01mh
````

Some items describe something called `Overflow damage`, and `critical strikes` which could affect calculations of how likely the core is to be targeted with full attachments.

2026-08-04 Wk 32 Tue - 12:30 +03:00

We can use `s` to show the core exposure percentage. And `c` to toggle how much our attachments cover our core. See the description of the item via shift+key, and it will give a coverage amount.

It helps to go into a room with a single door and stay at a diagonal with the door, enemies seem to just stay at the door as you melee attack them, so they queue up one by one.

Ended up running out of energy and I couldn't move, turns out you can disable keyboard mode and use the scroll wheel to wait. It says in the basic menu (F1). Though if I have energy, I usually just wait by moving back and forth. It can be strategically different though, esp when against enemies I want to come somewhere, since I can be off by one.

Oh if you check the advanced menu you'll find you can also just wait with `.`!

Currently I'm choosing mainly to get a `LRG. STORAGE UNIT` which puts our items at 15. Sort of makes us slow, but the bigger one makes us way too slow for 25 items.

I didn't know I could scroll the inventory with `]` and `[`.

You can also use `/` for swapping. I kept removing items, then attaching others.  Even if it's empty it will still swap. This lets you get items from the inventory filtered by attachment type! This move also removes, though if that's the intent, `d {key}` is faster.

`IMP.` for improved, as found in the description of `IMP. ALUMINUM LEG`

2026-08-05 Wk 32 Wed - 18:29 +03:00

Spawn [lan/2026/topic/game-notes/001 Grid Sage Games/entry/000 Cogmind Playing Thought Stream/entry/000 Cogmind discovered manual terminal commands](entry/000%20Cogmind%20discovered%20manual%20terminal%20commands.md) ^spawn-entry-8635f9

2026-08-11 Wk 33 Tue - 20:47 +03:00

Actually, we will use huge storage. The reason is because now we're focusing on propulsion a lot which gives us a lot of mass capacity.
