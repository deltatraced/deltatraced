# 1 Journal

2025-07-26 Wk 30 Sat - 23:30

We currently use the `Roulette` lumina, which is as described:

 > 
 > Every hit has a 50% chance to deal either 50% or 200% of its damage

 > 
 > \[!NOTE\] Problem
 > Assuming every hit deals 100 damage, what's the expected amount of damage done after 50 attacks (a) with Roulette  and (b) without Roulette?

(a)

We have 50% chance to deal 50 damage and 50% chance to deal 200 damage. On average, we deal $\frac{50 + 200}{2}=125$ damage.

So we expect to deal more damage over time because the reward is worth our entire damage, while the punishment is only worth half our damage, and they are both equally as likely.

So on average we expect,

$$
\begin{aligned}
& \text{Expected} \\
& = 125 \ \frac{\text{dmg}}{\text{atk}} \times 50 \ \text{atk} \\
& = 6250 \ \text{dmg}
\end{aligned}
$$

(b)

$$
\begin{aligned}
& \text{Expected} \\
& = 100 \ \frac{\text{dmg}}{\text{atk}} \times 50 \ \text{atk} \\
& = 5000 \ \text{dmg}
\end{aligned}
$$

So we should use this. Normally this adds risk because the enemies can deal 200% damage on us. But with golgra, we cannot afford to be hit even once. So we have all the reward and none of the risk.
