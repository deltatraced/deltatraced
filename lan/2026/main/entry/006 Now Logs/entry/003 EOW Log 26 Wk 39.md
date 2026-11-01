---
context_type: entry
---

Parent: [lan/2026/main/entry/006 Now Logs/006 Now Logs](../006%20Now%20Logs.md)

Spawned by: [lan/2026/main/entry/006 Now Logs/entry/001 End of Week Log](001%20End%20of%20Week%20Log.md)

Spawned in: [^spawn-entry-8c131b](001%20End%20of%20Week%20Log.md#spawn-entry-8c131b)

Prev: [002 EOW Log 26 Wk 38](002%20EOW%20Log%2026%20Wk%2038.md)

Next: [005 EOW Log 26 Wk 40](../../007%20Thought%20Stream%20Weeks/entry/005%20EOW%20Log%2026%20Wk%2040.md)

# Journal

2026-09-28 Wk 40 Mon - 23:01 +03:00

[002 TSW 26 Wk 39](../../007%20Thought%20Stream%20Weeks/entry/002%20TSW%2026%20Wk%2039.md)

Like the previous week [002 EOW Log 26 Wk 38](002%20EOW%20Log%2026%20Wk%2038.md), we're continuing the 3D printing project, and also CQTS Lecture note studying for cubical agda.

2026-09-28 Wk 40 Mon - 23:02 +03:00

On the 3D Printing side,

At the beginning of week 39 I was diagnosing setting failures that cause me to do bad prints with PETG filament.

I didn't seem to be able do good prints with the PETG settings from https://github.com/Ankermgmt/prusaslicer-ankermake-ce-profiles. But doing it with the AnkerMake v1.5.21 default PETG Black settings seems to work alright.

Also running into new failure modes. The front case for the BC-250 case I was printing is just 10mm off just at the end of a ~11h print, because apparently the 3D printer filament roll got a knot. So we had to look into respooling it, but I don't have a respooling machine, so we tried to do it manually, and now it's printing the back case, going towards the end of a ~12h print.

The knot issue had occured 2 other times before, so it prompted me to try full respooling. It would be good to build a respooling machine as a future project:

https://www.printables.com/model/407688-bambu-lab-p1-x1-x1c-x1cc-filament-spool-switcher-w

https://www.youtube.com/watch?v=00lXbrWMZwo

There are also some small defects in the PETG prints:

![Pasted image 20260928232321.png](../../../../../../attachments/Pasted%20image%2020260928232321.png)

![Pasted image 20260928232333.png](../../../../../../attachments/Pasted%20image%2020260928232333.png)

Not exactly sure what causes this, although some of them could at least be cleaned up manually more.

I'm not sure if the nozzle is failing, since it's otherwise able to do very precise printing work. There is some slight vibration in the surface table I have 3D printer on, if this may be a relevant factor somehow.

2026-09-28 Wk 40 Mon - 23:20 +03:00

On the CQTS Lecture note studying for cubical agda,

I've now reached up to `ΩS¹≃ℤ` in lecture 2-6 univalence. Getting close to where I had stopped before in lecture 2-7. Currently struggling with formulating the `J` rule for its section proof.

`(encode base p)` has type ℤ but `(encode s q)` is of type `helix s`, so they won't fit as a type in the same places.
