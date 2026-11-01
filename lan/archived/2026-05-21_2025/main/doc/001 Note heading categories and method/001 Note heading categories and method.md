\#needs-updating

# 1 Objective

Here we state the objective of the note in a concise matter. This may include how it relates to other notes and problems, and may have tasks for objectives we need to get done like

* [ ] File a bug report

## 1.1 Key information

top headings `Tasks`, `Issues`, `HowTos`, `Investigations`, `Ideas`, or `Side Notes` are categories of entries, where each level-2 heading is an entry in them.

Any entry may spawn/create any other entry, and this is labeled two-way.

|Heading|Meaning|
|-------|-------|
|Objective|Scope and purpose of the entire note file|
|Journal|Mostly sequential logs of progress towards our objective|
|Tasks|Entries with a clear scope and measure of completion. Think "do X". They're operative.|
|Issues|Encountered problems to be resolved. Each problem has an entry with a short description of the problem. Think "Program crashes with error 5".|
|HowTos|Knowledge checks. Often entries about using a tool or performing an action. If we need to research online, we often create a howto entry to be clear we did a context switch to searching online. Names are descriptive of the actions we want to learn to perform. For example, "read a file in python3" which could be read as "\[how to\] read a file in python3".|
|Investigations|Learning, diagnostics, debugging, questions, reducing noise and increasing signal. Those can be theoretical in nature and we may come in not knowing much to estimate scope for them like tasks. Entries here may have names in the form of questions "What are the major components of a game engine?", or in the form of an investigation objective "Look into totem video player crash".|
|Ideas|Might not be immediately operative, but any related ideas to this note context can be captured here.|
|Side Notes|I learned about something cool! I don't want to dilute the context of other entries so I capture them here as entries.|
|External Links|Capture any external resources that refer to this note file|
|References|Shared references for everything in this document. Can also<br> additional include resources.|

Order of the headings is by objectivity in scope.

1. Objective is most clear in scope.
1. Journal is clear progress logged towards the objective.
1. Tasks have a clear signal to completion,
1. Issues can but may not have a clear signal to resolution
1. HowTos can but may not have a clear way to perform them.
1. Investigations could be open-ended in scope
1. Ideas are very open-ended
1. Side Notes is for anything outside the scopes.

This should be all you need. If you want, you can go through a short tutorial below following from Journal.

# 2 Journal

This is a sequential section where we log progress on the task or entry as we move along. It's meant to be linearly read. Other sections below, on the other hand, are record-based. For example we may spawn a task, issue, or investigation from here. The order with which they are spawned will hint at some linearity or progress, but subsequent tasks can also reference prior ones so the ordering is not as strict.

If you are viewing this on github, note since this has a lot of headings, you can view the headings on github via `Outline`

\<![Pasted image 20250812150047.png](../../../../../../attachments/Pasted%20image%2020250812150047.png)

To get something like this that you can jump through!

\<![Pasted image 20250812150118.png](../../../../../../attachments/Pasted%20image%2020250812150118.png)

Let's spawn a task. Let's write some special keyword, like SPW0. This allows this line to be easily identified so that we can create a back reference from the spawned task back here.

Now when we add the new task under the Tasks section, we can just write `[ [ ^SPW0` and select the line and Obsidian generates a random record for it quickly. We can follow that link back with `Ctrl + .` and replace `SPW0` with  `Spawn {internal link}.`

This lets Obsidian generate a link for the line quickly. Then we change `^{xxxxxx}` to `^spawn-task-{xxxxxx}` to make the spawn category clear.

Let's go to the new task!

Spawn [3.1 File a bug report](001%20Note%20heading%20categories%20and%20method.md#31-file-a-bug-report) ^spawn-task-c33094

2025-08-12 Wk 33 Tue - 13:55

I wouldn't normally do the "Go back to Spawner" thing when taking actual notes, but It's a cool thing to insert linearity into our tutorial which uses a non-linear note taking style.

We've now covered what many sections do!

The template for these headings can be found here:

`(now deleted>>) Note Headings`

Although there are some manual things right now, like spawning new records, or referencing items from References, they can be done quickly. They could be automated in an obsidian plugin in the future!

Thank you for going through my non-linear note taking style tutorial!

# 3 External Links

If you've linked this note anywhere in the world, maybe take note of that here!

# 4 References
