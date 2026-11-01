---
context_type: task
status: done
---

Parent: [lan/2026/microproj/001-ping-pong-score-ts/entry/000 Getting started ping-pong-score-ts/000 Getting started ping-pong-score-ts](../000%20Getting%20started%20ping-pong-score-ts.md)

Spawned by: [lan/2026/microproj/001-ping-pong-score-ts/entry/000 Getting started ping-pong-score-ts/000 Getting started ping-pong-score-ts](../000%20Getting%20started%20ping-pong-score-ts.md)

Spawned in: [^spawn-task-5ae0df](../000%20Getting%20started%20ping-pong-score-ts.md#spawn-task-5ae0df)

Overview: [000 Overview Getting started ping-pong-score-ts](../entry/000%20Overview%20Getting%20started%20ping-pong-score-ts.md)

# Journal

2026-06-26 Wk 26 Fri - 17:37 +03:00

From the requirements in [000 Wiki ping-pong-score-ts](../../../wiki/000%20Wiki%20ping-pong-score-ts/000%20Wiki%20ping-pong-score-ts.md), here are some things we want true about our table:

````
- It recognizes the first player and colors the scores for each two turns to indicate whose turn it is to serve
- Score keeping should be quick, click on the row or the name and they get a point.
````

We know the table is for ping pong. Maybe we can have 6 entries per row, for 6 turns. Each game will likely take 2 rows or lines, but some might be longer.

2026-06-28 Wk 26 Sun - 21:26 +03:00

````sh
# in /home/lan/src/idea/cb/lan22h-experiments/ping-pong-score-ts
git commit
[main 691636c] impl basic game score keeping
````

![Pasted image 20260628212633.png](../../../../../../../attachments/Pasted%20image%2020260628212633.png)

Basic UI that does the job. I'm able to play on the overview table above alone. The Score history is not yet data sourced, the number are dsummies, but the values in the overview table above track who scores.

It applies the rules that every two turns the one colored red alternates to signal who serves. If it becomes pink, that player is in danger, if they lose one more, game over. If they draw > 10 it goes back to alternating.

We can undo scores if a wrong button is pressed.

This is functional enough I got feedback after a few games played with it:

1. Add a mode to choose randomly who is left and who is right player.
1. Add a winner-stays-but-random-opponent mode.
1. Add a winner-stays-but-alternating-opponents mode.
1. Add also a way to reset names and suggest names instead of having them be typed everytime.

I think it would also be good to have the overview table side itself be clickable, which is more intuitive than the small buttons `Score Left` and `Score Right`.

2026-06-28 Wk 26 Sun - 21:33 +03:00

Now that there is we meet the basic objective, this is no longer an idea. Publishing.

Spawn [lan/2026/microproj/001-ping-pong-score-ts/entry/000 Getting started ping-pong-score-ts/task/003 Get ppst setup on vps](003%20Get%20ppst%20setup%20on%20vps.md) ^spawn-task-2437da
