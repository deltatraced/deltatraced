# 1 Journal

From [^spawn-task-d3ebdf](001%20Reconstructing%20tape%20content.md#spawn-task-d3ebdf) in [3.10 Reverse engineering the internal mini language](001%20Reconstructing%20tape%20content.md#310-reverse-engineering-the-internal-mini-language),

2025-07-31 Wk 31 Thu - 00:31

Let's reconstruct the tape. The simple sum checksum for it is 287251 and it has 13229 elements.

We know this is the math of the initial tape command retrieval

````ts
  while (g_exit_code == 0) {
    g_inst8 = get_and_adv_tape();

    var cmd_idx = Math.floor(g_inst8 / 2);

    var fn = g_unk_cmds[cmd_idx];
    if (fn) fn();
  }
````

`g_data_cur` begins as 0.  The first `g_inst8` is also 0. Then `g_data_cur` advanced to 1.

So the very first command executed `cmd_idx`, should be 0.

````ts
// 0
function () {
  g_reg = get_and_adv_tape_u16_and_load_on_odd_caller();
},

function get_and_adv_tape_u16_and_load_on_odd_caller() {
  var inst16 = get_and_adv_tape_u16();
  if (g_inst8 % 2 > 0) inst16 = g_tape[inst16] || 0;
  return inst16;
}
````

We're an even caller (`g_inst8` is 0 and even). So we just read the next 2 bytes and get them.

That means instruction 0 will always read 3 bytes. 1 for itself and 2 for its parameters, and then it will store the param data in the global `g_reg` register.

2025-07-31 Wk 31 Thu - 01:06

The next `g_inst8` 3 bytes in reads `2`. With floor math, this should give us a `cmd_idx` of `1`.

That's another comand of size-3 (itself 1 + u16 input)

````ts
// 1
function () {
  // Write register data to the next u16 inst
  g_tape[get_and_adv_tape_u16()] = g_reg;
},
````

It basically just writes the previously read `g_reg` (0) to the location specified in the tape. In our case for `2, 175, 51` that should be

````sh
python3 -c "n0=175; n1=51; print(hex((n1 << 8) + n0))"

# out
0x33af
````

2025-07-31 Wk 31 Thu - 01:28

Order of operations matter.

````py
# good
python3 -c "n=0x33af; print((n & 0xFF00) >> 8)"
51

# bad
python3 -c "n=0x33af; print(n & 0xFF00 >> 8)"
````

There are 4 `cmd01` invocations here.

````ts
function reconstruct_tape(): number[] {
  var tape: number[] = []; 

  tape.push(...cmd00_write_param_to_reg(/*param16*/ 0x0000));
  tape.push(...cmd01_store_reg_to_tape_addr(/*tape_addr16*/ 0x33af));
  tape.push(...cmd01_store_reg_to_tape_addr(/*tape_addr16*/ 0x33b3));
  tape.push(...cmd01_store_reg_to_tape_addr(/*tape_addr16*/ 0x33b4));
  tape.push(...cmd01_store_reg_to_tape_addr(/*tape_addr16*/ 0x33b6));
  tape.push(...g_tape_remaining);

  return tape;
}
````

I will add them on whenever I encounter them.

Next, we read `g_inst8` as 8, yielding a `cmd_idx` of 4.

````ts
// 4
function () {
  g_reg += get_and_adv_tape_u16_and_load_on_odd_caller();
},
````

This is also a 3-byte command `8, 1, 0,` . Just summing its param value into `g_reg`.

2025-07-31 Wk 31 Thu - 01:42

The next new `g_inst8` command reads 46, inferring a `cmd_idx` of 23. But commands only go 0-22.

This also triggers at that point

````ts
var fn = g_cmds[cmd_idx];
if (fn) {
  fn();
} else {
  console.log(`${g_inst8},${cmd_idx}`);
  throw new Error("breakpoint");
}

// out
46,23
Uncaught Error: breakpoint
````

It seems to be a size-1 no-op.

2025-07-31 Wk 31 Thu - 01:54

I think the reason for the floor divide by 2 math

````ts
var cmd_idx = Math.floor(g_inst8 / 2);
````

is because of

````ts
function get_and_adv_tape_u16_and_load_on_odd_caller() {
  var inst16 = get_and_adv_tape_u16();
  if (g_inst8 % 2 > 0) inst16 = g_tape[inst16] || 0;
  return inst16;
}
````

This makes it so that every command can have an odd or even variant which changes how this function behaves. So it's a padded bit for this and the rest is the command index.

2025-07-31 Wk 31 Thu - 01:59

The next new `g_inst8` command reads 30, inferring a `cmd_idx` of 15.

````ts
// 15
function () {
  g_tape_addr_reg -= 1;
  g_tape[g_tape_addr_reg] = g_data_cur + 2;
  g_data_cur = get_and_adv_tape_u16_and_load_on_odd_caller();
},
````

Another 3-size command. Until now we have not touched `g_tape_addr_reg`, so it should be 0. So our first time we're accessing it at -1? Let's confirm.

````ts
// 15
function () {
  g_tape_addr_reg -= 1;
  var before = g_tape[g_tape_addr_reg];
  var data_cur_before = g_data_cur;
  g_tape[g_tape_addr_reg] = g_data_cur + 2;
  g_data_cur = get_and_adv_tape_u16_and_load_on_odd_caller();
  
  console.log(`g_tape_addr_reg: ${g_tape_addr_reg}`);
  console.log(`slot@before: ${before}`);
  console.log(`slot@after: ${g_tape[g_tape_addr_reg]}`);
  console.log(`data_cur@before: ${data_cur_before}`);
  console.log(`data_cur@after: ${g_data_cur}`);
  
  throw new Error("debugging")
},

// out
g_tape_addr_reg: -1
slot@before: undefined
slot@after: 32
data_cur@before: 30
data_cur@after: 2992

Uncaught Error: debugging
````

So there's some calling functionality here. It retains the previous program counter in a known location and updates the program counter according to the given u16 value.

The writing to -1 behavior is strange.

So we can treat `g_tape_addr_reg -= 1;` as some sort of pop?  Renaming `g_data_cur` to `g_pc16` since we have evidence here that u16 data is loaded into it directly.

Also renaming `g_tape_addr_reg` to `g_sp16` for a u16 stack pointer.

So from

````
g_tape[g_sp16] = g_pc16 + 2;
````

We know the tape address space is `u16`, and its element space is also `u16`.

When we do math with n-sized commands, we assumed that we are being fed `u8`s. But this may not always be true as we can see here. Yet the original tape data really only contains `u8` values.

cmd15 seems to be a 3-size stack-preserving caller.

2025-07-31 Wk 31 Thu - 02:39

The next new `g_inst8` command reads 12, inferring a `cmd_idx` of 6.

````ts
// 6
function () {
  g_cond_reg = g_reg == get_and_adv_tape_u16_and_load_on_odd_caller();
},
````

Just a condition check that the current register value is the next `u16` on the tape. And with this we should rewrite `g_reg` to `g_reg16` since it is a `u16` variable.

This is a 3-size command `12, 0, 0,`.

2025-07-31 Wk 31 Thu - 02:45

The next new `g_inst8` command reads 26, inferring a `cmd_idx` of 13.

````ts
// 13
function () {
  var v = get_and_adv_tape_u16_and_load_on_odd_caller();
  g_pc16 = g_cond_reg ? v : g_pc16;
},
````

^cmd13

It's a 3-size command `26, 138, 3`.

It's a branch if equal. Let's just call the command `beq`.

2025-07-31 Wk 31 Thu - 02:56

There's much repetition now. Let's write a python script to autoreconstruct the commands we've encountered so far until the first encountered new command for us to investigate.

Spawn [3.5 Write python script to reconstruct tape until new undocumented command](001%20Reconstructing%20tape%20content.md#35-write-python-script-to-reconstruct-tape-until-new-undocumented-command) ^spawn-task-34592a

2025-08-01 Wk 31 Fri - 02:58

Now that we autoreconstruct the tape, we can find the next commands quicker.

Following the same note convention as before for documenting commands,

The next new `g_inst8` command reads 24, inferring a `cmd_idx` of 12.

````ts
// 12
function () {
	g_pc16 = get_and_adv_tape_u16_and_load_on_odd_caller();
},
````

This is just a goto on the u16 param.

Adding

````ts
export function cmd12_goto(tape_addr16: number): number[] {
  return [24, tape_addr16 & 0xFF, (tape_addr16 & 0xFF00) >> 8];
}
````

to `reconstructed_commands.ts`

and

````py
    CommandData("cmd12", 24, "cmd12_goto(/*tape_addr16*/ {PARAM0})", [16]),
````

to `commands` in `data/tape.py`.

Now let's reconstruct the tape and ensure tape OK

````sh
python3 data/tape.py reconstruct-tape
./build.sh
````

In the web console when launching `adventure.html` in the browser we see

````
tape OK
````

2025-08-01 Wk 31 Fri - 03:08

The next new `g_inst8` command reads 34, inferring a `cmd_idx` of 17.

````ts
// 17
function () {
  g_sp16 -= 1;
  g_tape[g_sp16] = g_reg16;
},
````

This is similar to command 15. It's simpler, it just writes a value on the stack rather than the program counter. Let's just call it `cmd17_push_reg16_to_stack`.

2025-08-01 Wk 31 Fri - 03:20

Oops. I made it take a `param16` when really this one takes nothing, so then we got an invalid command `51` afterwards

2025-08-01 Wk 31 Fri - 03:35

The next new `g_inst8` command reads 1, inferring a `cmd_idx` of 0.

This is our first odd command. It's the same as 0, which we already have. This is a 3-size command of `1, 179, 51,`

Recall that command 0 is

````ts
// 0
function () {
  g_reg = get_and_adv_tape_u16_and_load_on_odd_caller();
},
````

The odd bit triggers something new about `get_and_adv_tape_u16_and_load_on_odd_caller`

````ts
function get_and_adv_tape() {
  var v = g_tape[g_pc16];
  g_pc16 += 1;
  return v;
}

function get_and_adv_tape_u16() {
  var v = get_and_adv_tape();
  return v + (get_and_adv_tape() << 8);
}

function get_and_adv_tape_u16_and_load_on_odd_caller() {
  var inst16 = get_and_adv_tape_u16();
  if (g_inst8 % 2 > 0) inst16 = g_tape[inst16] || 0;
  return inst16;
}
````

Basically instead of loading the instruction as-is, it loads it from the address provided.

Let's add an explicit command for this instead of relying on the odd bit, since it will perform a different behavior: `cmd00_write_loaded_param16_to_reg16`.

Also renaming `param` and `reg` to `param16` and `reg16` in the command names for consistency.

2025-08-01 Wk 31 Fri - 03:47

The next new `g_inst8` command reads 36, inferring a `cmd_idx` of 18.

````ts
// 18
function () {
  g_reg16 = g_tape[g_sp16];
  g_sp16 += 1;
},
````

This is a `cmd18_pop_stack_to_reg16`. It's a 1-size command.

If I make a mistake and not update 36->38 here for cmd18,

````py
    CommandData("cmd17", 34, "cmd17_push_reg16_to_stack()", []),
    CommandData("cmd18", 34, "cmd18_pop_stack_to_reg16()", []),
````

I will still get OK but we will not advance from 34 because in my [script](https://github.com/LanHikari22/lan-exp-scripts/blob/main/files/2025/persistent/000-mountain-n-dragon-ctf/data/tape.py) I ensure that every command must have a unique activation byte:

````py
    command = (
        commands
        | pipe.OfIter[CommandData].filter(
            lambda command: command.activation_byte == command_byte
        )
        | pipe.OfIter[CommandData].to_list()
        | pipe.Of[List[CommandData]].map(lambda lst: lst[0] if len(lst) == 1 else None)
    )

    if command is None:
        return (None, tape)
````

2025-08-01 Wk 31 Fri - 03:56

The next new `g_inst8` command reads 28, inferring a `cmd_idx` of 14.

````ts
// 14
function () {
  var v = get_and_adv_tape_u16_and_load_on_odd_caller();
  g_pc16 = g_cond_reg ? g_pc16 : v;
},
````

This is a 3-size command `28, 201, 0,`.

This is basically like command 13

[^cmd13](001%20Reconstructing%20tape%20content.md#cmd13)

Except instead of `beq` it's `bne` for branch not equal.

2025-08-01 Wk 31 Fri - 05:17

The next new `g_inst8` command reads 9, inferring a `cmd_idx` of 4.

This is `cmd04_sum_reg16_loaded_param16_to_reg16`.

2025-08-01 Wk 31 Fri - 05:20

The next new `g_inst8` command reads 14, inferring a `cmd_idx` of 7.

````ts
// 7
function () {
  g_cond_reg = g_reg16 < get_and_adv_tape_u16_and_load_on_odd_caller();
},
````

This is a 3-size command. Let's call it `cmd07_check_reg16_lt_param16`.

2025-08-01 Wk 31 Fri - 05:23

The next new `g_inst8` command reads 10, inferring a `cmd_idx` of 5.

````ts
// 5
function () {
  g_reg16 -= get_and_adv_tape_u16_and_load_on_odd_caller();
},
````

This is a 3-size command. Call it `cmd05_sub_param16_from_reg16`

2025-08-01 Wk 31 Fri - 05:30

The next new `g_inst8` command reads 42, inferring a `cmd_idx` of 21.

````ts
// 21
function () {
  // Function`$${"\x61 \x3D\x20\x6E\x65\x77 \x44\x61\x74\x65\x28\x29\x5B\x27\x67\x65\x74\x53\x65\x63\x6F\x6E\x64\x73\x27\x5D\x28\x29 "}$`();
  // Hex for:
  // a = new Date()['getSeconds']()
  
  g_reg16 = new Date()['getSeconds']()
},
````

Right this one was obfuscated for some reason. I guess part of the game puzzle. It would hardcode the variable to be called `a`. So that could have broken things with us reverse engineering and renaming things in the typescript.

It's a 1-size command. Let's call it `cmd21_set_reg16_to_cur_seconds`

2025-08-01 Wk 31 Fri - 05:34

The next new `g_inst8` command reads 18, inferring a `cmd_idx` of 9.

````ts
// 9
function () {
  g_reg16 = g_reg16 & get_and_adv_tape_u16_and_load_on_odd_caller();
},
````

This is a 3-size command. Let's call it `cmd09_check_reg16_and_param16`

2025-08-01 Wk 31 Fri - 05:37

We've reached PC `0x0817`.

The next new `g_inst8` command reads 4, inferring a `cmd_idx` of 2.

````ts
// 2
function () {
  g_tape[get_and_adv_tape_u16()] = g_tape[g_reg16];
},
````

This is a 3-size command. Call it `cmd02_store_loaded_reg16_to_loaded_param16`.

2025-08-01 Wk 31 Fri - 05:41

We've reached PC `0x0a2a`.

The next new `g_inst8` command reads 38, inferring a `cmd_idx` of 19.

````ts
// 19
function () {
  g_exit_code = 1;
},
````

Here it is, the command that terminates the current mainloop and forces a UI update.

Let's rename it from `g_exit_code` to `g_new_frame`. It does not exit the actual game, it only forces a the while loop of `mainloop` to terminate and immediately updates inner HTML elements for the inputs and requests a new animation frame.

This is a 1-sized command. Call it `cmd19_issue_new_frame`.

2025-08-01 Wk 31 Fri - 05:48

We've reached PC `0x0a2b`. Just the very next command.

The next new `g_inst8` command reads 40, inferring a `cmd_idx` of 20.

````ts
// 20
function () {
  g_inp = 0;
  g_inp +=
    g_joyp[37] +
    (g_joyp[39] << 1) +
    (g_joyp[38] << 2) +
    (g_joyp[40] << 3) +
    (g_joyp[90] << 4) +
    (g_joyp[88] << 5);
  g_reg16 = g_inp;
},
````

Ooh! Joypad!

This is a 1-sized command. Let's call it  `cmd20_read_joypad_input_and_set_to_reg16`

2025-08-01 Wk 31 Fri - 05:52

We've reached PC `0x0a36`.

The next new `g_inst8` command reads 44, inferring a `cmd_idx` of 22.

````ts
// 22
function () {
  this.document.location.href = "" + l(g_reg16) + ".html";
},
````

Hmm. Will have to investigate why set the href. See investigation below.

Spawn [6.2 Why is href being set in cmd22?](001%20Reconstructing%20tape%20content.md#62-why-is-href-being-set-in-cmd22) ^spawn-invst-8f21a6

This is a 1-sized command. Call it `cmd22_set_dom_href_to_reg16`.

2025-08-01 Wk 31 Fri - 06:03

We've reached PC `0x0a9d`.

The next new `g_inst8` command reads 7, inferring a `cmd_idx` of 3.

````ts
// 3
function () {
  g_tape[g_reg16] = get_and_adv_tape_u16_and_load_on_odd_caller();
},
````

It's the odd version of this. This is a 3-sized command. Call it `cmd03_set_loaded_reg16_to_loaded_param16`

2025-08-01 Wk 31 Fri - 06:07

We've reached PC `0x0aa9`.

The next new `g_inst8` command reads 13, inferring a `cmd_idx` of 6.

3-size command. Loaded version of cmd06. Call it `cmd06_check_reg16_is_loaded_param16`

2025-08-01 Wk 31 Fri - 06:11

We've reached PC `0x0ab8`.

The next new `g_inst8` command reads 6, inferring a `cmd_idx` of 3.

We've created the loaded version of this 3-sized command. This one is `cmd03_set_loaded_reg16_to_param16`.

2025-08-01 Wk 31 Fri - 06:15

We've reached PC `0x0abe`.

The next new `g_inst8` command reads 32, inferring a `cmd_idx` of 16.

````ts
// 16
function () {
  g_pc16 = g_tape[g_sp16];
  g_sp16 += 1;
},
````

Ooh! It's a return!

This is a 1-sized command. Let's call it `cmd16_return`!

2025-08-01 Wk 31 Fri - 06:20

We've reached PC `0x0b48`.

The next new `g_inst8` command reads 15, inferring a `cmd_idx` of 7.

This is a 3-sized command `cmd07_check_reg16_lt_loaded_param16`.

Renaming all `tape_addr16` to `addr16` for brevity.

2025-08-01 Wk 31 Fri - 06:24

We've reached PC `0x0b51`.

The next new `g_inst8` command reads 11, inferring a `cmd_idx` of 5.

There's many loaded versions of commands being used. Let's revert our design decision to name those "loaded" commands and just handle the loaded instruction flag automatically.

Spawn [3.7 Handle loaded param16 flag automatically in commands when reconstructing tape program](001%20Reconstructing%20tape%20content.md#37-handle-loaded-param16-flag-automatically-in-commands-when-reconstructing-tape-program) ^spawn-task-781da8

2025-08-01 Wk 31 Fri - 07:06

We seem to end up at an invalid command 51. This is a common data byte... See issue below.

Spawn [4.4 Tape reaches invalid command 51 at 0x0b89](001%20Reconstructing%20tape%20content.md#44-tape-reaches-invalid-command-51-at-0x0b89) ^spawn-issue-0b89

2025-08-01 Wk 31 Fri - 10:04

We added a bug command `cmd25_bug`... We know this would not lead to any critical failure for the game.

We've reached PC `0x0bb5`.

The next new `g_inst8` command reads 23, inferring a `cmd_idx` of 11.

````ts
// 11
function () {
  g_reg16 = g_reg16 ^ get_and_adv_tape_u16_and_load_on_odd_caller();
},
````

Call this `cmd11_check_reg16_xor_param16`.

2025-08-01 Wk 31 Fri - 10:13

We've reached PC `0x0bc6`.

The next new `g_inst8` command reads 16, inferring a `cmd_idx` of 8.

Let's just go for completeness here and cover any remaining commands we haven't yet mapped.

These would be only 8, 10...

Let's do 8 and see if we hit 10 in activation 20/21.

````ts
// 8
function () {
  g_cond_reg = 0 != (g_reg16 & get_and_adv_tape_u16_and_load_on_odd_caller());
},

// 9
function () {
  g_reg16 = g_reg16 & get_and_adv_tape_u16_and_load_on_odd_caller();
},

// 10
function () {
  g_reg16 = g_reg16 | get_and_adv_tape_u16_and_load_on_odd_caller();
},

````

We called command 9 `cmd09_check_reg16_and_param16` but this is false. it's storing mask results. Change that to `cmd09_mask_reg16_and_param16_to_reg16`

Call command 8 `cmd08_check_reg16_and_param16` and command 10 `cmd08_check_reg16_or_param16`

2025-08-01 Wk 31 Fri - 10:30

We've reached PC `0x0c98`.

We've encountered command 10. Let's add it too.

2025-08-01 Wk 31 Fri - 10:38

We've reached PC `0x1088`.

We've hit a `255`. There are many `255`s.

2025-08-01 Wk 31 Fri - 11:07

Here is a frequency analysis of the commands from data captured in `experiments/tape_program_data.csv`

<img src="https://raw.githubusercontent.com/delta-domain-rnd/delta-trace/refs/heads/main/attachments/Pasted%20image%2020250801110727.png" />

`cmd16_return` is only referenced 10 times. Do we assume we have 10 functions?

`cmd15_stack_preserve_call` is called 63 times. 63 function calls?

We need to document labels here for this program. Let's reconstruct the labels too.  See [task](001%20Reconstructing%20tape%20content.md#38-reconstruct-labels-in-the-tape-program). ^spawn-task-reconstruct-labels

### 1.1.1 Pend
