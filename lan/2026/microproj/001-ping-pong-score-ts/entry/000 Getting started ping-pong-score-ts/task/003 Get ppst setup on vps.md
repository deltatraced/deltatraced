---
context_type: task
status: todo
---

Parent: [lan/2026/microproj/001-ping-pong-score-ts/entry/000 Getting started ping-pong-score-ts/000 Getting started ping-pong-score-ts](../000%20Getting%20started%20ping-pong-score-ts.md)

Spawned by: [lan/2026/microproj/001-ping-pong-score-ts/entry/000 Getting started ping-pong-score-ts/task/002 impl score table component proto 1](002%20impl%20score%20table%20component%20proto%201.md)

Spawned in: [^spawn-task-2437da](002%20impl%20score%20table%20component%20proto%201.md#spawn-task-2437da)

Issues encountered: [001 Issues during Get ppst setup on vps](../entry/001%20Issues%20during%20Get%20ppst%20setup%20on%20vps.md)

# Journal

2026-06-28 Wk 26 Sun - 22:00 +03:00

Spawn [lan/2026/microproj/001-ping-pong-score-ts/entry/000 Getting started ping-pong-score-ts/entry/001 Issues during Get ppst setup on vps](../entry/001%20Issues%20during%20Get%20ppst%20setup%20on%20vps.md) ^spawn-entry-0b4708

````sh
# in /home/lan/src/cloned/cb/lan22h-experiments
mv ~/src/idea/cb/lan22h-experiments/ping-pong-score-ts .
cd ping-pong-score-ts
git remote add origin ssh://git@codeberg.org/lan22h-experiments/ping-pong-score-ts.git
git push -u origin main
````

2026-06-28 Wk 26 Sun - 21:56 +03:00

````sh
# in vps > /home/lan/src/cloned/cb/lan22h-experiments/ping-pong-score-ts
sudo apt install npm
npm install -D concurrently typescript 
sudo apt-get install node-typescript
````
