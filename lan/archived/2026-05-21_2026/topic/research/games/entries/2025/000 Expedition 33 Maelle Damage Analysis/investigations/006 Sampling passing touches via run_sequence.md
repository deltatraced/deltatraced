# 1 Journal

We will tweak this script and see what ranges give us a passing parry:

````python
INPUT_DEV='/dev/input/event{N}' ON_KEYS="KEY_KP6,KEY_2" KEY='18' INTERVALS_MS="380,1000,1000" python3 run_sequence.py
````

The source of noise here is the time it takes for us to trigger the parry sequence. We try to do this right on golgra's feet landing, which is an error source.

There is code overhead from callback detection to pressing the first key of about 26.96 ms - 28.72 ms. This is also the overhead between each successful ydotool presses.

|Column|Meaning|
|------|-------|
|Attack|The name of the attack sampled|
|TouchN (ms)|The interval before the Nth touch in a parry|
|Passed (#/all)|Num of touches passed, or all|
|Other|When re-running the test, we get N (M) where N is the number of times we get M passed.|
|Valid (#)|Num of times this test was reproduced to reduce noise|
|Invalid (#)|Num of times this test failed to be reproduced|

|Attack|Touch1 (ms)|Touch2 (ms)|Touch3 (ms)|Touch4 (ms)|Passed (#/all)|Other|Valid (#)|Invalid (#)|
|------|-----------|-----------|-----------|-----------|--------------|-----|---------|-----------|
|fast|300|1000|1000||0||1|0|
|fast|350|1000|1000||2||1|2|
|fast|380|1000|1000||1||1|0|
|fast|400|1000|1000||1|2 (2) 1 (0) 1 (all)|5|4|
|fast|410|1000|1000||1|3 (2) 3 (0) 1 (all)|4|8|
|fast|420|1000|1000||0||5|4|
|fast|430|1000|1000||0||8|6|
|fast|450|1000|1000||0||2|0|
