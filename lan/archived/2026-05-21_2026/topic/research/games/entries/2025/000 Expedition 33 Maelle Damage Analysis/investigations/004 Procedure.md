# 1 Journal

2025-08-24 Wk 34 Sun - 09:22

### 1.1.1 Sampling second parry

|Times|Color|Stats|Damage|Attack|Variant|
|-----|-----|-----|------|------|-------|
|1|white|`Sl --- Bk -- R--`|1361|counter|1|
|1|white|`Sl --- Bk -- R++`|5445|counter|2|
|1|yellow|`Sl Bm1 Bk Br R--`|2042|counter|3.1|
|4|yellow|`Sl Bm1 Bk Br R-- Cb`|2042|counter|3.2|
|1|yellow|`Sl Bm1 Bk Br R++`|8168|counter|4.1|
|3<br>|yellow|`Sl Bm1 Bk Br R++ Cb`|8168|counter|4.2|

2025-08-24 Wk 34 Sun - 09:47

It doesn't seem to make a difference what attack we parry, maelle will still deal the same damage under the same characteristics, so we'll just call it the counter attack.

Also Critical burn or no critical burn seems to make no damage difference given all else.

Burn mark `Bm` may be identical to `Brulerum` `Br`.

We sample second parry because the first one has the noise of first offensive which never occurs again.

We seem to always see a burn icon but not always a burn mark ("burn" text) for parry

2025-08-24 Wk 34 Sun - 10:08

For roulette, we may either get 50% damage or 200% damage.

(math)

(1)

Let `Atk` be normal damage

(2)

$R^-$ represents the 50% damage

$$
R^- = \frac{1}{2} Atk
$$

$$
\text{Atk} = 2R^-
$$
(3)

$R+$ represents 200% damage

$$
R^+ = 2\text{Atk}
$$

$$
\begin{aligned}
& R^+ \\
& = 2\text{Atk} \\
& = 2(2R^-) \\
& = 4R^- \\\end{aligned}
$$

(/math)

So if you divide two damages and see a factor of 4, know you went $R^-$ -> $R^+$.

So correcting

|Times|Color|Stats|Damage|Attack|Variant|
|-----|-----|-----|------|------|-------|
|1|white|`Sl --- Bk -- R-?`|1361|counter|1|
|1|white|`Sl --- Bk -- R+?`|5445|counter|2|

to

|Times|Color|Stats|Damage|Attack|Variant|
|-----|-----|-----|------|------|-------|
|1|white|`Sl --- Bk -- R--`|1361|counter|1|
|1|white|`Sl --- Bk -- R++`|5445|counter|2|

Also white -> yellow is a factor of 1.5, so we now know that those are related

|Times|Color|Stats|Damage|Attack|Variant|
|-----|-----|-----|------|------|-------|
|1|white|`Sl --- Bk -- R++`|5445|counter|2|
|1|yellow|`Sl Bm1 Bk Br R+?`|8168|counter|4.1|
|3<br>|yellow|`Sl Bm1 Bk Br R+? Cb`|8168|counter|4.2|

Correct them to also be $R^+$.

2025-08-24 Wk 34 Sun - 10:24

Those are related with a 1.5x white -> yellow.

|Times|Color|Stats|Damage|Attack|Variant|
|-----|-----|-----|------|------|-------|
|1|white|`Sl --- Bk -- R--`|1361|counter|1|
|1|yellow|`Sl Bm1 Bk Br R-?`|2042|counter|3.1|
|3|yellow|`Sl Bm1 Bk Br R-? Cb`|2042|counter|3.2|

Correct them to $R^-$.

2025-08-24 Wk 34 Sun - 10:25

So far the only relevant stats for parry damage are $R^-$/$R^+$ and white/yellow. Others like critical burn etc might be more relevant for chip damage via burns later or for increasing the likelihood that we get yellow rather than white (critical hit).

### 1.1.2 Sampling Second Turn Shots

2025-08-24 Wk 34 Sun - 10:33

|Times|Color|Stats|Damage|Attack|Variant|
|-----|-----|-----|------|------|-------|
|1|white|`Sl --- Bk -- R-- Cb Aa Ms`|82|shot|1|
|1|white|`Sl Bm1 Bk -- R-- Cb Aa Bs`|82|shot|1|
|6|yellow|`Sl Bm1 Bk Br R++ Cb Aa`|490|shot|2.1|
|1|yellow|`Sl Bm2 Bk Br R++ Cb Aa`|490|shot|2.2|
|1|yellow|`Sl Bm1 Bk Br R++ Cb Aa Vr`|490|shot|2.3|
|4|yellow|`Sl Bm1 Bk Br R-- Cb Aa `|123|shot|3.1|
|1|yellow|`Sl Bm1 Bk Br R-- Cb Aa Vr`|123|shot|3.2|
|1|yellow|`Sl Bm1 Bk Br R++ Cb Aa Md`|735|shot|4|

2025-08-24 Wk 34 Sun - 10:45

123 -> 490 is a 4x factor. This is roulette effect.

123 -> 82 is a 1.5x factor. This is critical hit effect.

$\frac{735}{490} = 1.5$. This is a marked effect!

2025-08-27 Wk 35 Wed - 11:00

Ontology update, `Bm` for burn mark is not enough. Sometimes we inflict multiple burns, and we see the text "Burn" multiple times, so now we have `BmN` where N is some number. If you see it once, it's `Bm1`, if you see it twice it's `Bm2`, etc.

### 1.1.3 Sampling Offensive Switch on Start of Second Turn

2025-08-24 Wk 34 Sun - 10:58

|Times|Color|Stats|Damage|Attack|Variant|
|-----|-----|-----|------|------|-------|
|1|yellow|`Sl Bm1 Bk Br R-? Cb`|599|Offensive Switch|1|
|1|yellow|`Sl Bm2 Bk Br R-? Cb Md`|980|Offensive Switch|2|

### 1.1.4 Sampling Burn Chip Damage

2025-08-24 Wk 34 Sun - 11:04

|Times|Color|Stats|Damage|Attack|Variant|
|-----|-----|-----|------|------|-------|
|1|white|`Sl Bm1 Bk -- R-? B01 b01 `|726|Burn|1|
|1|white|`Sl Bm1 Bk -- R-? B02 b02 `|1425|Burn|2|
|1|white|`Sl Bm1 Bk -- R-? B02 b02 Cb `|1425|Burn|2.2|
|1|white|`So Bm1 Bk -- R-? B16 b16 Cb `|3312|Burn|3|
|1|white|`So Bm1 Bk -- R+? B17 b16 Cb `|9999|Burn|4|
|1|white|`So Bm1 Bk -- R+? B16 b15 Cb `|9999|Burn|4.1|
|1|white|`So Bm1 Bk -- R+? B07 b07 Cb `|5082|Burn|5|

2025-08-24 Wk 34 Sun - 11:08

![Pasted image 20250824110819.png](../../../../../../../../../../attachments/Pasted%20image%2020250824110819.png)

This is on a burn. I'm assuming the `x2` is for

2025-08-27 Wk 35 Wed - 12:05

On end turn, golgra loses burn mark but no burn damage. Also sometimes there's a steep drop like 16->2, suggesting that the burns expire or something.

### 1.1.5 Sampling Offensive Stance Parries

|Times|Color|Stats|Damage|Attack|Variant|
|-----|-----|-----|------|------|-------|
|0|yellow|`Sl Bm1 Bk Br R--`|2042|counter|3.1|

### 1.1.6 Sampling Sword Ballet on Start of Third Turn

|Times|Color|Stats|Damage|Attack|Variant|
|-----|-----|-----|------|------|-------|
|4|yellow|`So Bm1 Bk Br R-? Cb Q12 `|1348|SwordBallet|1|
|1|yellow|`So Bm1 Bk Br R+? Cb Q12 `|5391|SwordBallet|2|
|3|yellow|`So Bm1 Bk Br R-? Cb Q00`|1715|SwordBallet|3|
|2|yellow|`So Bm1 Bk Br R+? Cb Q00`|6861|SwordBallet|4|

### 1.1.7 Sampling Sword Ballet on Start of Second Turn

|Times|Color|Stats|Damage|Attack|Variant|
|-----|-----|-----|------|------|-------|
|3|yellow|`Sl Bm1 Bk Br R-? Cb Q00 `|1143|SwordBallet|3|
|2|yellow|`Sl Bm1 Bk Br R-? Cb Q00 `|4573|SwordBallet|4|

### 1.1.8 Numerical Estimation of Golgra HP

2025-08-27 Wk 35 Wed - 12:23

![Pasted image 20250827122349.png](../../../../../../../../../../attachments/Pasted%20image%2020250827122349.png)

This is 1:37 minutes in, died in the third turn first attack.

![Pasted image 20250827122748.png](../../../../../../../../../../attachments/Pasted%20image%2020250827122748.png)

This box measures 16x7 pixels.

2025-08-27 Wk 35 Wed - 12:37

![Pasted image 20250827123703.png](../../../../../../../../../../attachments/Pasted%20image%2020250827123703.png)

Approximately 41 of those would fill the bar.

Another quick way to measure is to approximately box the HP bar

![Pasted image 20250827123754.png](../../../../../../../../../../attachments/Pasted%20image%2020250827123754.png)

This boundary is 659x7 pixels, and $\frac{659}{16} \approx 41.2$.

2025-08-27 Wk 35 Wed - 12:39

Now let's count the total damage for one box.

First Turn

9999, 726, 2042, 1452, 2042,

490, 123, 123, 123, 490, 184, 490, 184,

980,

Second Turn

3312, 9999, 9999, 2552, 9999, 3829, 1452, 3063,

1715, 6861, 1715, 1715, 6861,

Third Turn

5082,

So in total,

````sh
python3 -c "l=[9999, 726, 2042, 1452, 2042, 490, 123, 123, 123, 490, 184, 490, 184, 980, 3312, 9999, 9999, 2552, 9999, 3829, 1452, 3063, 1715, 6861, 1715, 1715, 6861, 5082,]; print(sum(l))"

# out
87602
````

So we estimate that golgra has $87,602 \times \frac{659}{16} \approx 3,608,107$ HP

2025-08-27 Wk 35 Wed - 13:28

If we keep dealing 43,801 $\frac{\text{dmg}}{\text{turn}}$ we could win within 83 turns.

2025-08-27 Wk 35 Wed - 13:58

Every turn seems to take around 50 seconds (video of 2 turns is around 1:41 minutes), so 83 turns can expect to take $50\ \frac{\text{s}}{\text{turn}} \times 83\ \text{turn} = 4150\ \text{s} \approx 69\  \text{min}$.
