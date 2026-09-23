# 1 Journal

* [ ] 

2025-07-30 Wk 31 Wed - 08:34

2025-07-30 Wk 31 Wed - 08:42

Experiment results in `experiments/cmd_idx_idle.csv` (n=42) for

````ts
  while (g_exit_code == 0) {
    g_chr = get_and_adv_tape();

    var cmd_idx = Math.floor(g_chr / 2);
    console.log(`${cmd_idx},${g_data_cur},${g_chr}`);

    var fn = g_unk_cmds[cmd_idx];
    if (fn) fn();
  }

````

<img src="https://raw.githubusercontent.com/delta-domain-rnd/delta-trace/refs/heads/main/attachments/Pasted%20image%2020250730084258.png" />

2025-07-31 Wk 31 Thu - 00:17

Using experiment setup in [3.4 Send web messages to terminal to collect experiment data](004%20Build%20a%20frequency%20map%20of%20the%20commands%20being%20run%20and%20tape%20locations%20being%20hit%20on%20idle.md#34-send-web-messages-to-terminal-to-collect-experiment-data),

Experiment results in `experiments/cmd_idx_idle.csv` (n=30360) for

````ts
  var i = 0;

  while (g_exit_code == 0) {
    g_chr = get_and_adv_tape();

    var cmd_idx = Math.floor(g_chr / 2);

    if (webio.m_connected) {
      g_socket.send(`${i++},${cmd_idx},${g_data_cur},${g_chr}`)
    }

    var fn = g_unk_cmds[cmd_idx];
    if (fn) fn();
  }
````

Using visidata,

````sh
vd experiments/cmd_idx_idle.csv
# select cmd_idx
# F for frequency analysis
````

<img src="https://raw.githubusercontent.com/delta-domain-rnd/delta-trace/refs/heads/main/attachments/Pasted%20image%2020250731002037.png" />

Those are the commands in effect out of the 23 commands used in idle activity.

2025-08-01 Wk 31 Fri - 08:11

````sh
vd experiments/cmd_idx_idle.csv
# select g_chr
# F for frequency analysis
````

<img src="https://raw.githubusercontent.com/delta-domain-rnd/delta-trace/refs/heads/main/attachments/Pasted%20image%2020250801081102.png" />
^freq-analysis-gchr

