# 1 Journal

* [x] 

From [^spawn-issue-0b89](003%20Tape%20reaches%20invalid%20command%2051%20at%200x0b89.md#spawn-issue-0b89) in [3.8 Reconstruct labels in the tape program](003%20Tape%20reaches%20invalid%20command%2051%20at%200x0b89.md#38-reconstruct-labels-in-the-tape-program)

2025-08-01 Wk 31 Fri - 07:18

We seem to be misreading the tape here:

````
0, 2, 74, 10, 0, 62, 0, 2, 132, 11, 0, 125, 0, 2, 73, 10, 24, 197, 10, 3, 8,
1, 0, 2, 176, 51, 14, 12, 0, 36, 26, 197, 10, 36, 0, 6, 0, 2, 175, 11, 0, 253,
````

Let's try to route the commands manually to see. We're seeing 51 on the next command byte in the remaining tape but this should be incorrect.

````
0, [2, 74, 10,] [0, 62, 0,] [2, 132, 11,] [0, 125, 0,] [2, 73, 10,] [24, 197, 10,] [3, 8,
1,] [0, 2, 176,] 51, 14, 12, 0, 36, 26, 197, 10, 36, 0, 6, 0, 2, 175, 11, 0, 253,
````

2025-08-01 Wk 31 Fri - 08:07

We can confirm through `experiments/cmd_idx_idle.csv` that we never trigger a command whose character is `51`.

[^freq-analysis-gchr](003%20Tape%20reaches%20invalid%20command%2051%20at%200x0b89.md#freq-analysis-gchr)

2025-08-01 Wk 31 Fri - 08:38

Oh I forgot to increment the bytes for cmdl by 1...

I added `+0` to all `cmd` commands so that when I mirror, I also change `+0` to `+1` at scale.

I thought I had a `tape OK` signal but the web console wasn't updating...

We are getting `tape OK` now with the `+1` change but we're still expecting `51` after `0x0b89`...

2025-08-01 Wk 31 Fri - 08:54

I also double checked the command sizes. All up to command 15 are 3-sized commands. 16 onward are 1-sized. And I got that correct.

Another possibility is that we've left the text section and entered the data section of the tape...

2025-08-01 Wk 31 Fri - 09:05

16 is 32. So Anything $\ge32$  is a 1-sized command.

````
  2, 179, 51, 
  32, 
  0, 81, 50, 
  46, 
  24, 136, 12, 
  0, 142, 50, 
  46, 
  32, 
  12, 2, 0,
  
  
  28, 197, 10, 
  34, 
  1, 188, 51, 
  12, 0, 0, 
  36, 
  26, 197, 10, 
  34, 
  1, 187, 51, 
  12, 0, 0, 
  28, 71, 11, 
  
  42, 
  8, 1, 0, 
  2, 187, 51, 
  36, 
  24, 197, 10, 
  0, 10, 42, 
  8, 1, 0,
  15, 187, 51, 
  28, 84, 11, 
  8, 60, 0, 
  
  11, 187, 51, 
  13, 188, 51, 
  26, 103, 11, 
  0, 0, 0, 
  2, 188, 51, 
  36, 
  24, 197, 10, 
  36, 
  0, 121, 30, 
  0, 10, 0, 
  
  2, 133, 11, 
  0, 8, 0, 
  2, 74, 10, 
  0, 62, 0, 
  2, 132, 11, 
  0, 125, 0, 
  2, 73, 10, 
  24, 197, 10, 
  3, 8, 1, 
  0, 2, 176, 
  
  51, 
````

I thought a command started with 71 but I missed a 1-sized command 42...

These values don't seem wrong. So maybe We intentionally hit an invalid command to mark the data section?

2025-08-01 Wk 31 Fri - 09:47

You can go the other way too... There's just not enough grammar to this for error to be likely. If it is $\lt 32$  you form a 3-size box. If it's $\ge 32$ you form a 1-size box.  The only error signal is that in case of $\ge 32$ the value exceeds 46, which is the `no-op`  right out of index.

Even if the code is false, no error will be triggered by the interpreter itself. It will simply ignore it and advance the tape.

2025-08-01 Wk 31 Fri - 09:56

Let's just add a 1-size bug command `cmd25_bug` and `cmdl25_buf`. for 50/51.
