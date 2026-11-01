# 1 Journal

* [x] 

The script for this is [here](https://github.com/LanHikari22/lan-exp-scripts/blob/main/files/2025/persistent/000-mountain-n-dragon-ctf/data/tape.py).

2025-08-01 Wk 31 Fri - 04:03

The program counter is mostly the sum of the prior bytes read, make it a 4-digit 0 padded hex number like in the params.

Instead of

````ts
tape.push(...c.cmd00_write_param16_to_reg16(/*param16*/ 0x0000));
````

do

````ts
/*0x0000*/ tape.push(...c.cmd00_write_param16_to_reg16(/*param16*/ 0x0000));
````

2025-08-01 Wk 31 Fri - 04:16

````ts
export function reconstruct_tape(): number[] {
  var tape: number[] = [];

  /*0x0000*/ tape.push(...c.cmd00_write_param16_to_reg16(/*param16*/ 0x0000));
  /*0x0003*/ tape.push(...c.cmd01_store_reg16_to_tape_addr(/*tape_addr16*/ 0x33af));
  /*0x0006*/ tape.push(...c.cmd01_store_reg16_to_tape_addr(/*tape_addr16*/ 0x33b3));
  /*0x0009*/ tape.push(...c.cmd01_store_reg16_to_tape_addr(/*tape_addr16*/ 0x33b4));
  /*0x000c*/ tape.push(...c.cmd01_store_reg16_to_tape_addr(/*tape_addr16*/ 0x33b6));
  /*0x000f*/ tape.push(...c.cmd04_sum_reg16_param16_to_reg16(/*param16*/ 0x0001));
  /*0x0012*/ tape.push(...c.cmd01_store_reg16_to_tape_addr(/*tape_addr16*/ 0x33b7));
  /*0x0015*/ tape.push(...c.cmd00_write_param16_to_reg16(/*param16*/ 0x0c8c));
  /*0x0018*/ tape.push(...c.cmd23_noop());
  /*0x0019*/ tape.push(...c.cmd00_write_param16_to_reg16(/*param16*/ 0x0d22));
  /*0x001c*/ tape.push(...c.cmd23_noop());
  /*0x001d*/ tape.push(...c.cmd15_stack_preserve_call(/*tape_addr16*/ 0x0bb0));
  /*0x0020*/ tape.push(...c.cmd06_check_reg16_is_param16(/*param16*/ 0x0000));
  /*0x0023*/ tape.push(...c.cmd13_beq(/*tape_addr16*/ 0x038a));

  // ...
````

OK
