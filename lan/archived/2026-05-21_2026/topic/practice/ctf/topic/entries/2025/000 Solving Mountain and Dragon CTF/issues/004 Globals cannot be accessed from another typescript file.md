# 1 Journal

* [x] 

From [^spawn-issue-704ee0](004%20Globals%20cannot%20be%20accessed%20from%20another%20typescript%20file.md#spawn-issue-704ee0) in [3.9 Create a driver to search for death and infinity strings](004%20Globals%20cannot%20be%20accessed%20from%20another%20typescript%20file.md#39-create-a-driver-to-search-for-death-and-infinity-strings)

2025-08-01 Wk 31 Fri - 13:32

When I tried to move the global files to another file so that multiple files could access them, I got errors like

````ts
Cannot assign to 'reg16' because it is a read-only property.ts(2540)
````

a wrapper allows them to be accessed:

````ts
import * as reconstructed_tape from './autogen/reconstructed_tape.ts'

export const g = {
  inp: 0,
  mq: document.getElementById("marquee"),
  cond_reg: false,
  reg16: 0,
  pc16: 0,
  sp16: 0,
  inst8: 0,
  new_frame: 0,
  tape: reconstructed_tape.reconstruct_tape(),
  joyp: {},
};
````

So I can access them out like this

````ts
import * as g from "./globals.ts";

// 1
function () {
  // Write register data to the next u16 inst
  g.g.tape[get_and_adv_tape_u16()] = g.g.reg16;
},
````
