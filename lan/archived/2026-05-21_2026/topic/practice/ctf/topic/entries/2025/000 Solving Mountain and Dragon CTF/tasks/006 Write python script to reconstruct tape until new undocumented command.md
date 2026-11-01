# 1 Journal

* [x] 

From [^spawn-task-34592a](006%20Write%20python%20script%20to%20reconstruct%20tape%20until%20new%20undocumented%20command.md#spawn-task-34592a) in [3.11 Reconstructing tape content](006%20Write%20python%20script%20to%20reconstruct%20tape%20until%20new%20undocumented%20command.md#311-reconstructing-tape-content)

The script for this is [here](https://github.com/LanHikari22/lan-exp-scripts/blob/main/files/2025/persistent/000-mountain-n-dragon-ctf/data/tape.py).

2025-07-31 Wk 31 Thu - 02:59

We will write this in `data/tape.py`. We used that script to give us the checksum and length, but we can use it to generate this function for us given the currently known commands:

````ts
function reconstruct_tape(): number[] {
  var tape: number[] = []; 

  tape.push(...cmd00_write_param_to_reg(/*param16*/ 0x0000));
  tape.push(...cmd01_store_reg_to_tape_addr(/*tape_addr16*/ 0x33af));
  tape.push(...cmd01_store_reg_to_tape_addr(/*tape_addr16*/ 0x33b3));
  tape.push(...cmd01_store_reg_to_tape_addr(/*tape_addr16*/ 0x33b4));
  tape.push(...cmd01_store_reg_to_tape_addr(/*tape_addr16*/ 0x33b6));
  tape.push(...cmd04_sum_reg_param_to_reg(/*param16*/ 0x0001));
  tape.push(...cmd01_store_reg_to_tape_addr(/*tape_addr16*/ 0x33b7));
  tape.push(...cmd00_write_param_to_reg(/*param16*/ 0x0c8c));
  tape.push(...cmd23_noop());
  tape.push(...cmd00_write_param_to_reg(/*param16*/ 0x0d22));
  tape.push(...cmd23_noop());
  tape.push(...cmd15_stack_preserve_call(/*tape_addr16*/ 0x0bb0));
  tape.push(...cmd06_check_reg16_is_param16(/*param16*/ 0x0000));
  tape.push(...cmd13_beq(/*tape_addr16*/ 0x038a));
  tape.push(...cmd06_check_reg16_is_param16(/*param16*/ 0x0002));
  tape.push(...cmd13_beq(/*tape_addr16*/ 0x0059));
  tape.push(...cmd06_check_reg16_is_param16(/*param16*/ 0x0001));
  tape.push(...cmd13_beq(/*tape_addr16*/ 0x046a));
  tape.push(...g_tape_remaining);

  return tape;
}
````

Let's start by adding some argparse stuff in there for the reconstruction or reporting. See [template](https://github.com/LanHikari22/lan-exp-scripts/blob/main/templates/2025/topics/py3/persistant/000-argparse/template_with_subcommands.py).

2025-08-01 Wk 31 Fri - 02:29

Now we should be able to see where the next undocumented command is!

````ts
python3 data/tape.py reconstruct-tape

// out
function reconstruct_tape(): number[] {
  var tape: number[] = [];

  tape.push(...cmd00_write_param_to_reg(/*param16*/ 0x0000));
  tape.push(...cmd01_store_reg_to_tape_addr(/*tape_addr16*/ 0x33af));
  tape.push(...cmd01_store_reg_to_tape_addr(/*tape_addr16*/ 0x33b3));
  tape.push(...cmd01_store_reg_to_tape_addr(/*tape_addr16*/ 0x33b4));
  tape.push(...cmd01_store_reg_to_tape_addr(/*tape_addr16*/ 0x33b6));
  tape.push(...cmd04_sum_reg_param_to_reg(/*param16*/ 0x0001));
  tape.push(...cmd01_store_reg_to_tape_addr(/*tape_addr16*/ 0x33b7));
  tape.push(...cmd00_write_param_to_reg(/*param16*/ 0x0c8c));
  tape.push(...cmd23_noop());
  tape.push(...cmd00_write_param_to_reg(/*param16*/ 0x0d22));
  tape.push(...cmd23_noop());
  tape.push(...cmd15_stack_preserve_call(/*tape_addr16*/ 0x0bb0));
  tape.push(...cmd06_check_reg16_is_param16(/*param16*/ 0x0000));
  tape.push(...cmd13_beq(/*tape_addr16*/ 0x038a));
  tape.push(...cmd06_check_reg16_is_param16(/*param16*/ 0x0002));
  tape.push(...cmd13_beq(/*tape_addr16*/ 0x0059));
  tape.push(...cmd06_check_reg16_is_param16(/*param16*/ 0x0001));
  tape.push(...cmd13_beq(/*tape_addr16*/ 0x046a));
  tape.push(...cmd06_check_reg16_is_param16(/*param16*/ 0x0003));
  tape.push(...cmd13_beq(/*tape_addr16*/ 0x016a));
  tape.push(...cmd06_check_reg16_is_param16(/*param16*/ 0x0005));
  tape.push(...cmd13_beq(/*tape_addr16*/ 0x0044));
  tape.push(...cmd15_stack_preserve_call(/*tape_addr16*/ 0x0ac5));
  tape.push(...remaining_tape.data);

  return tape;
}
````

So far the commands we have documented are

````py
commands = [
    CommandData("cmd00", 0, "cmd00_write_param_to_reg(/*param16*/ {PARAM0})", [16]),
    CommandData(
        "cmd01", 2, "cmd01_store_reg_to_tape_addr(/*tape_addr16*/ {PARAM0})", [16]
    ),
    CommandData("cmd04", 8, "cmd04_sum_reg_param_to_reg(/*param16*/ {PARAM0})", [16]),
    CommandData(
        "cmd06", 12, "cmd06_check_reg16_is_param16(/*param16*/ {PARAM0})", [16]
    ),
    CommandData("cmd13", 26, "cmd13_beq(/*tape_addr16*/ {PARAM0})", [16]),
    CommandData(
        "cmd15", 30, "cmd15_stack_preserve_call(/*tape_addr16*/ {PARAM0})", [16]
    ),
    CommandData("cmd23", 46, "cmd23_noop()", []),
]
````

This script will reconstruct `remaining_tape.ts` such that the first bytes there are of a new command that we have not registered yet.

2025-08-01 Wk 31 Fri - 02:38

To make this more seamless, I'm extracting also the reconstructed commands and reconstructed tape into their own typescript files. This way I can automatically write `reconstructed_tape.ts`. Also just to be clear they are autogen, we put them in an `autogen/` folder: `autogen/reconstructed_tape.ts` and `autogen/remaining_tape.ts`.

So `reconstructed_tape.ts` contents should look like this:

````ts
import * as c from "../reconstructed_commands.ts"
import * as remaining_tape from "./remaining_tape.ts";

export function reconstruct_tape(): number[] {
  var tape: number[] = [];

  tape.push(...c.cmd00_write_param_to_reg(/*param16*/ 0x0000));
  tape.push(...c.cmd01_store_reg_to_tape_addr(/*tape_addr16*/ 0x33af));
  tape.push(...c.cmd01_store_reg_to_tape_addr(/*tape_addr16*/ 0x33b3));
  tape.push(...c.cmd01_store_reg_to_tape_addr(/*tape_addr16*/ 0x33b4));
  tape.push(...c.cmd01_store_reg_to_tape_addr(/*tape_addr16*/ 0x33b6));
  tape.push(...c.cmd04_sum_reg_param_to_reg(/*param16*/ 0x0001));
  tape.push(...c.cmd01_store_reg_to_tape_addr(/*tape_addr16*/ 0x33b7));
  tape.push(...c.cmd00_write_param_to_reg(/*param16*/ 0x0c8c));
  tape.push(...c.cmd23_noop());
  tape.push(...c.cmd00_write_param_to_reg(/*param16*/ 0x0d22));
  tape.push(...c.cmd23_noop());
  tape.push(...c.cmd15_stack_preserve_call(/*tape_addr16*/ 0x0bb0));
  tape.push(...c.cmd06_check_reg16_is_param16(/*param16*/ 0x0000));
  tape.push(...c.cmd13_beq(/*tape_addr16*/ 0x038a));
  tape.push(...c.cmd06_check_reg16_is_param16(/*param16*/ 0x0002));
  tape.push(...c.cmd13_beq(/*tape_addr16*/ 0x0059));
  tape.push(...c.cmd06_check_reg16_is_param16(/*param16*/ 0x0001));
  tape.push(...c.cmd13_beq(/*tape_addr16*/ 0x046a));
  tape.push(...c.cmd06_check_reg16_is_param16(/*param16*/ 0x0003));
  tape.push(...c.cmd13_beq(/*tape_addr16*/ 0x016a));
  tape.push(...c.cmd06_check_reg16_is_param16(/*param16*/ 0x0005));
  tape.push(...c.cmd13_beq(/*tape_addr16*/ 0x0044));
  tape.push(...c.cmd15_stack_preserve_call(/*tape_addr16*/ 0x0ac5));
  tape.push(...remaining_tape.data);

  return tape;
}
````

where `reconstructed_commands.ts` is written manually.

2025-08-01 Wk 31 Fri - 02:49

````sh
python3 data/tape.py reconstruct-tape

# out
Created autogen/remaining_tape.ts
Created autogen/reconstructed_tape.ts
````

````sh
./build.sh
````

````ts
function verify_tape_integrity() {
  const expected_simple_checksum = 287251;
  const expected_num_elements = 13229;

  if (g_tape.length != expected_num_elements) {
    throw new Error(`Expected tape length ${expected_num_elements} but got ${g_tape.length}`);
  }

  var sum = 0;
  
  for (var i=0; i<g_tape.length; i++) {
    sum += g_tape[i];
  }

  if (sum != expected_simple_checksum) {
    throw new Error(`Expected tape sum ${expected_simple_checksum} but got ${sum}`);
  }

  console.log("tape OK");
}

verify_tape_integrity()

# out
tape OK
````
