# 1 Journal

* [ ] 

2025-08-01 Wk 31 Fri - 12:28

So I thought it was a map of some sorts, but these strings defy assumptions of a static map:

````
[ ][ ][ ][ ][ ][ ][ ][ ][ ]
[ ][ ][ ][ ][ ][ ][ ][ ][ ]
[ ][ ][ ][ ][D][^][^][ ][ ]
[ ][ ][ ][D][.][.][.][D][ ]
[ ][ ][ ][<][S][>][v][ ][ ]
[ ][ ][ ][ ][v][ ][ ][ ][ ]
[ ][ ][ ][ ][ ][ ][ ][ ][ ]
[ ][ ][ ][ ][ ][ ][ ][ ][ ]
[ ][ ][ ][ ][ ][ ][ ][ ][ ]


Death Strings
S 2^ D
S 1^ 1< D
S 1^ 3> D
S 1^ 2> 1< D
S 1^ 1> 2< D
S 4> 2v D
S 2< 2> 1^ D

Infinite Stretch 
S 1< I
S 1v I 
S 1> I
````

^death-strings-1

We could search this space and set a parameter for what counts as a stretch from a savepoint. The issue is resetting the game... Normally we have to refresh our browser but what if we can reset the game state ourselves?

2025-08-01 Wk 31 Fri - 12:33

Let's create a control interface for testing. Let's start with a `/hello` command over websocket and a `/help` command to list all commands.

Start a server over `localhost:3004` for control

````sh
websocat -s 3004
````

2025-08-01 Wk 31 Fri - 12:55

Okay they work. Let's add `/reset` and register a handler from the main game to reset the data to be called by it.

2025-08-01 Wk 31 Fri - 13:04

`/reset` works! We just had to set every global back to its initial state, and reconstruct the tape!

What if we gather diff data over the tape? Implement a `/diff-tape` command that dump a diff from original and a `/diff-tape-last` command that would diff since the last snapshot (whenever `/diff-tape` or `/diff-tape-last` are called, update last snapshot.)

2025-08-01 Wk 31 Fri - 13:18

Moving all globals to `globals.ts` so that they could be interfaced in other scripts and we don't have to modify the main script too much from the original.

Ran into an issue where I had to put them in a dictionary to export write. See issue below.

Spawn [4.5 Globals cannot be accessed from another typescript file](010%20Create%20a%20driver%20to%20search%20for%20death%20and%20infinity%20strings.md#45-globals-cannot-be-accessed-from-another-typescript-file) ^spawn-issue-704ee0

2025-08-01 Wk 31 Fri - 13:47

Okay so `/reset` works again and all in `web_control.ts` this time. Synchronized by frame end.

2025-08-02 Wk 31 Sat - 01:27

````
/diff-tape
Error: The arrays must be of equal size: 13229 != 13240
````

It's adding new elements to the tape...?

2025-08-02 Wk 31 Sat - 01:37

````
/verify-tape
Warn: Expected tape length 13229 but got 13259
Warn: Expected tape sum 287251 but got NaN
/verify-tape
Warn: Expected tape length 13229 but got 13260
Warn: Expected tape sum 287251 but got NaN
````

This is done after one input arrow key up: 13259 -> 13260.  See investigation below.

Spawn [6.5 How is the game adding data to the end of the tape?](010%20Create%20a%20driver%20to%20search%20for%20death%20and%20infinity%20strings.md#65-how-is-the-game-adding-data-to-the-end-of-the-tape) ^spawn-invst-0b60ed

We need to account for new content.

2025-08-02 Wk 31 Sat - 02:12

Changing `/diff-tape-last` to `/diff-last-tape` just so it's easy to use `StartsWith` to route them.

2025-08-02 Wk 31 Sat - 04:25

See investigation below for inferring data meaning with `/diff-tape`.

Spawn [6.6 Inferring tape data labels through play and diffs](010%20Create%20a%20driver%20to%20search%20for%20death%20and%20infinity%20strings.md#66-inferring-tape-data-labels-through-play-and-diffs)  ^spawn-invst-b57770

2025-08-02 Wk 31 Sat - 06:02

Adding `/read {addr16}` and `/write {addr16} {val}`. These just interact with the element at `{addr16}`, as it may be written or read as any value `num`.

Spawn [5.7 How to convert string to num in javascript](010%20Create%20a%20driver%20to%20search%20for%20death%20and%20infinity%20strings.md#57-how-to-convert-string-to-num-in-javascript) ^spawn-howto-01bbd2

2025-08-02 Wk 31 Sat - 07:09

Adding `/watch {addr16}` and `/nowatch {addr16}`. This allows the value to be pinged every second to the web console. If it's unwatched with `nowatch`, then it will stop pinging.

Spawn [5.8 Javascript execute some code every second on interval](010%20Create%20a%20driver%20to%20search%20for%20death%20and%20infinity%20strings.md#58-javascript-execute-some-code-every-second-on-interval) ^spawn-howto-8a31a9

2025-08-02 Wk 31 Sat - 08:06

We have `/watch {addr16}`. But then it becomes very hard to issue commands... We need a separate data channel from the control channel. We're starting a new websocket with `/data-connect` over `localhost:3005`.

### 3.9.1 Pend
