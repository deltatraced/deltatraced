---
status: todo
---
#external

# 1 What is this?

A CTF Game!

It can be found [here](https://2tie.rustedlogic.net/games/adv/adventure.html) [[#^1]].

# 2 Journal

2026-08-26 Wk 35 Wed - 00:02 +03:00

`/home/lan/src/cloned/cb/deltatraced/deltatraced/lan/archived/2026-05-21_2026/topic/practice/ctf/topic/entries/2025/000 Solving Mountain and Dragon CTF/000 Solving Mountain and Dragon CTF.md`

```sh
echo "en000 000 2026-08-26 Wk 35 Wed - 00:02 +03:00" | sha1sum | head -c6

# out
a04fd2
```

Files for this project are moved to `/home/lan/src/cloned/cb/lan22h-experiments/note-files/proj/a04fd2-mountain-n-dragon-ctf`.

2025-07-30 Wk 31 Wed - 04:06

<img src="https://raw.githubusercontent.com/delta-domain-rnd/delta-trace/refs/heads/main/attachments/Pasted%20image%2020250730040151.png" />

Outer HTML of that suspicious span is

```html
<span id="marquee" style="background-color:black;color:white;">        </span>
```

The HTML of that page itself isn't much...

```html
<html> <head><title>game</title></head> <body onload="mainloop()"> </br></br></br></br></br></br> <center><pre><span ID="marquee" style="background-color:black;color:white;"> </span></pre></center> <script src="[mountdrag.js](view-source:https://2tie.rustedlogic.net/games/adv/mountdrag.js)"></script></br></br> <center> Welcome, adventurers, to Mountain & Dragon!</br> This is a simple adventure game where the goal is to slay the dragon in its roost at the top of the mountains.</br> (to save your village or gain riches or blah blah)</br></br> The controls are simple:</br> arrow keys to move in a direction (Forward, Backward, Left or Right)</br> z key to Interact with the environment (such as throwing a lever or picking up an item)</br> x key to Use your currently held item</br></br> In case of death, refresh the page to restart your adventure! </center> </body> </html>
```

Oh that black spot responds to input in the order `L R F B I U`  corresponding to inputs `<- -> ^ v z x`.

After some inputs, it no longer responds, indicating that I died?

The files for this game, including ones to be modified, are to be references in  [lan-exp-scripts files](https://github.com/LanHikari22/lan-exp-scripts/tree/main/files/2025/persistent/000-mountain-n-dragon-ctf) [[#^2]].

# 3 References
1. [the game](https://2tie.rustedlogic.net/games/adv/adventure.html) ^1

2. [lan-exp-scripts files](https://github.com/LanHikari22/lan-exp-scripts/tree/main/files/2025/persistent/000-mountain-n-dragon-ctf) ^2

> Supplementary content and source files for this challenge.