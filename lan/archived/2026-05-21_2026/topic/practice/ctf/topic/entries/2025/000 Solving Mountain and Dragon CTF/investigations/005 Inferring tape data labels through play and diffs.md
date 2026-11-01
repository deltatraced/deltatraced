# 1 Journal

From [^spawn-invst-b57770](005%20Inferring%20tape%20data%20labels%20through%20play%20and%20diffs.md#spawn-invst-b57770) in [3.9 Create a driver to search for death and infinity strings](005%20Inferring%20tape%20data%20labels%20through%20play%20and%20diffs.md#39-create-a-driver-to-search-for-death-and-infinity-strings)

2025-08-02 Wk 31 Sat - 02:27

These are the tape changes on the string `S ^ ^ D`   (Start, up, up, dead)

````
/diff-tape
orig.length: 13229, tape.length: 13260
13229: NA != 4
13230: NA != 4
13231: NA != 2
13232: NA != undefined
13233: NA != 1
13234: NA != 1
13235: NA != 0
13236: NA != 0
13237: NA != undefined
13238: NA != 0
13239: NA != 1
13240: NA != undefined
13241: NA != undefined
13242: NA != undefined
13243: NA != undefined
13244: NA != undefined
13245: NA != undefined
13246: NA != undefined
13247: NA != undefined
13248: NA != undefined
13249: NA != undefined
13250: NA != undefined
13251: NA != undefined
13252: NA != undefined
13253: NA != undefined
13254: NA != undefined
13255: NA != undefined
13256: NA != undefined
13257: NA != undefined
13258: NA != 1
13259: NA != 1
````

Those last two entries

````
13258: NA != 1
13259: NA != 1
````

mark the input string "up up"

````
13258: NA != 2
13259: NA != 2
13260: NA != 2
13261: NA != 2
13262: NA != 2
13263: NA != 2
13264: NA != 2
13265: NA != 2
13266: NA != 2
13267: NA != 2
13268: NA != 2
13269: NA != 2
13270: NA != 2
13271: NA != 2
13272: NA != 2
13273: NA != 2
13274: NA != 2
13275: NA != 2
13276: NA != 2
13277: NA != 2
13278: NA != 2
13279: NA != 2
13280: NA != 2
13281: NA != 2
````

It's also able to track how many times we're pressing Right.

It just keeps going. Since this starts from 13258, so far I've reached $13538 - 13258 + 1 =  281$ inputs.

So let's map all the input values:

````
L R F B I U
0 2 1 3 4 5
````

^input-data-values

2025-08-02 Wk 31 Sat - 02:37

Here is what happens on reset and string `S ^ ^ D`

````
/reset
Resetting

/diff-last-tape
snap.length: 13260, tape.length: 13240
13229: 4 != 0
13230: 4 != 0
13231: 2 != 0
13233: 1 != undefined
13234: 1 != undefined
13258: 1 != NA
13259: 1 != NA

Press F

/diff-last-tape
snap.length: 13240, tape.length: 13259
13231: 0 != 1
13233: undefined != 1
13234: undefined != 1
13240: NA != undefined
13241: NA != undefined
13242: NA != undefined
13243: NA != undefined
13244: NA != undefined
13245: NA != undefined
13246: NA != undefined
13247: NA != undefined
13248: NA != undefined
13249: NA != undefined
13250: NA != undefined
13251: NA != undefined
13252: NA != undefined
13253: NA != undefined
13254: NA != undefined
13255: NA != undefined
13256: NA != undefined
13257: NA != undefined
13258: NA != 1

Press F

/diff-last-tape
snap.length: 13259, tape.length: 13260
13229: 0 != 4
13230: 0 != 4
13231: 1 != 2
13259: NA != 1

Key inputs no longer make a difference

/diff-last-tape
snap.length: 13260, tape.length: 13260
````

On each right press that doesn't lead to death:

````
/diff-last-tape
snap.length: 13276, tape.length: 13277
13231: 18 != 19
13276: NA != 2

/diff-last-tape
snap.length: 13277, tape.length: 13278
13231: 19 != 20
13277: NA != 2
````

So likely `13231` is the input string length. Let's display those values in hex since we've done everything else like that.

Spawn [5.6 How to print zero-padded hex in typescript](005%20Inferring%20tape%20data%20labels%20through%20play%20and%20diffs.md#56-how-to-print-zero-padded-hex-in-typescript) ^spawn-howto-98bf9d

````
/diff-last-tape
snap.length: 13259, tape.length: 13260
0x33af: 1 != 2
0x33cb: NA != 2

/diff-last-tape
snap.length: 13260, tape.length: 13261
0x33af: 2 != 3
0x33b6: 0 != 2
0x33cc: NA != 2
````

We also need labels as per this [task](005%20Inferring%20tape%20data%20labels%20through%20play%20and%20diffs.md#38-reconstruct-labels-in-the-tape-program) to document what those values mean.

````
/diff-last-tape
snap.length: 13623, tape.length: 13625
0x33af: 365 != 367
0x3537: NA != 2
0x3538: NA != 2
````

This is after going right 367 times.

So starting `0x33ca` we have a `u8[]` signifying input strings. We should add the labels

````
0x33af num    d_inp_str_len
0x33ca u8[]   d_inp_str
````

2025-08-02 Wk 31 Sat - 03:55

````
/diff-last-tape
snap.length: 13259, tape.length: 13260
0x33af: 1 != 2
0x33b1: 1 != 4
0x33b2: 1 != 4
0x33cb: NA != 4
````

These `0x33b1` and `0x33b2` seem to be flags. T

On pressing Right, we get

````
/diff-last-tape
snap.length: 13260, tape.length: 13261
0x33af: 2 != 3
0x33b1: 4 != 2
0x33b2: 4 != 2
0x33cc: NA != 2
````

Then pressing left...

````
/diff-last-tape
snap.length: 13262, tape.length: 13263
0x33ad: 0 != 1
0x33ae: 0 != 1
0x33af: 4 != 5
0x33b1: 2 != 0
0x33b2: 2 != 0
0x33ce: NA != 0
````

Actually they seem to map to the input pressed? So pressing X should give us 5 and Z 4 according to

[^input-data-values](005%20Inferring%20tape%20data%20labels%20through%20play%20and%20diffs.md#input-data-values)

````
Press Z

/diff-last-tape
snap.length: 13260, tape.length: 13261
0x33af: 2 != 3
0x33b1: 2 != 4
0x33b2: 2 != 4
0x33cc: NA != 4

Press X

/diff-last-tape
snap.length: 13261, tape.length: 13262
0x33af: 3 != 4
0x33b1: 4 != 5
0x33b2: 4 != 5
0x33cd: NA != 5
````

They do. Call them `d_last_inp1` and `d_last_inp2`.

### 1.1.1 Capturing diff on last input for a death string

2025-08-02 Wk 31 Sat - 04:03

Capturing the diff for the last input

On String `S ^ ^ D`,

````
/diff-last-tape
snap.length: 13259, tape.length: 13260
0x33ad: 0 != 4
0x33ae: 0 != 4
0x33af: 1 != 2
0x33cb: NA != 1
````

On String `S < < > > > D`,

````
/diff-last-tape
snap.length: 13262, tape.length: 13263
0x33ad: 0 != 2
0x33ae: 0 != 2
0x33af: 4 != 5
0x33ce: NA != 2
````

On String `S ^ < D`,

````
/diff-last-tape
snap.length: 13259, tape.length: 13260
0x33ad: 0 != 1
0x33ae: 0 != 1
0x33af: 1 != 2
0x33b1: 1 != 0
0x33b2: 1 != 0
0x33cb: NA != 0
````

At least we know that `0x33ad` and `0x33ae` mirror one another and seem to update on death. So call them `unk_death_33ad_1`  and `unk_death_33ad_2`.

[^death-strings-1](005%20Inferring%20tape%20data%20labels%20through%20play%20and%20diffs.md#death-strings-1)

2025-08-02 Wk 31 Sat - 04:27

On String `S 4> 2v D`,

````
/diff-last-tape
snap.length: 13263, tape.length: 13264
0x33ad: 0 != 8
0x33ae: 0 != 8
0x33af: 5 != 6
0x33cf: NA != 3
````

Actually, this is the snapshot diff on `S 4> 1v`,

````
/diff-last-tape
snap.length: 13262, tape.length: 13263
0x33af: 4 != 5
0x33b1: 0 != 1
0x33b2: 2 != 3
0x33ce: NA != 3
````

Note that `0x33b1 <d_last_inp1>` and `0x33b2 <d_last_inp2>` are different. They're not mirrors here.

[^input-data-values](005%20Inferring%20tape%20data%20labels%20through%20play%20and%20diffs.md#input-data-values)

So 2->3 for `0x33b2 <d_last_inp2>`  does seem to signify R->B as we expect. So let's call that `0x33b2 <d_last_inp>` and revert `0x33b1 <d_unk_33b1>`.

but why 0->1 for `0x33b1 <d_unk_33b1>`?

For consistency, all data labels will be `d_xxx`.  Jumps/labels within code will be `l_xxx`.  And if we think something marks a function, then `fn_xxx`.

2025-08-02 Wk 31 Sat - 06:25

This is in the beginning, the very first `/diff-last-tape` from launching the game.

````
/diff-last-tape
snap.length: 13229, tape.length: 13240
0x33ad: NA != 0
0x33ae: NA != 0
0x33af: NA != 0
0x33b0: NA != undefined
0x33b1: NA != undefined
0x33b2: NA != undefined
0x33b3: NA != 0
0x33b4: NA != 0
0x33b5: NA != undefined
0x33b6: NA != 0
0x33b7: NA != 1
````

2025-08-02 Wk 31 Sat - 06:26

After performing each string, read the value of `0x33b1 <d_unk_33b1>` with

````
/reset

Do the specified string

/read 0x33b1
````

[^input-data-values](005%20Inferring%20tape%20data%20labels%20through%20play%20and%20diffs.md#input-data-values)

|Row|String0|String1|String2|String3|String4|String5|String6|
|---|-------|-------|-------|-------|-------|-------|-------|
|Strings|`S`|`S 1>`|`S 2>`|`S 3>`|`S 4>`|`S 4> 1v`|`S 4> 2v D`|
|0x33b1|`NaN`|2|2|2|0|1|1|
|0x33b2|`NaN`|2|2|2|2|3|3|
|Strings|`S 4> 1<`|`S 4> 2<`|`S 4> 1v 1^`|`S 4> 1v 1^ 1v`|`S 4> 1v 1^ 1v 1^`<br>|||
|0x33b1|2|2|3|1|3|||
|||||||||

2025-08-02 Wk 31 Sat - 08:20

Oops. I had to correct `0x3bb1` to `0x33b1` here. But I should have been using the correct address in data collection. `0x3bb1` cannot be accessed under many strings.

2025-08-02 Wk 31 Sat - 08:22

We can see with `/watch 0x33b1` that it tracks with the input code most of the time until death.

### 1.1.2 Pend
