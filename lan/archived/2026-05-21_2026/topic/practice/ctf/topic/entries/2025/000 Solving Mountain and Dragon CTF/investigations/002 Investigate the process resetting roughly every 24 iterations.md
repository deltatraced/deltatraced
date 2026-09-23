# 1 Journal

* [ ] 

2025-08-01 Wk 31 Fri - 09:42

The experiment data we gathered in `experiments/cmd_idx_idle.csv` show that we're in some sort of control loop 23 instructions long every frame.  Let's examine what that loop does.

The frequency analysis over `i` also shows uniform indices. All indices have exactly 1265 counts.  The sample size is 30360... $1265 \times 24 = 30360$  . It seems that when we shut down the experiment, it did so on frame update which aligns exactly at the end of this loop.

### 6.3.1 Pend
