# 1 Journal

* [ ] 

2025-08-01 Wk 31 Fri - 11:12

We want to be able to document labels, which are positions in the program that are used as `addr16` in commands.

2025-08-02 Wk 31 Sat - 03:04

We need to create a labels file that can be both manipulated by tools and by us. We need basically three pieces of information: `addr16`, `type`, `label`.

For example,

````
0x01ac u16[]    some_input_array
0x0200 addr16[] some_pointer_arr
````

The types should be space-padded according to the largest type string. Addresses should be 4 0-padded. I put the type last before, but because there is much variance in the size of the label strings, it's better that they are in the middle with known widths to reduce visual noise.

Supported types will be

````
u8 u16 u8[] u16[] addr16 addr16[] unk8 unk16 unk8[] unk16[] num num[]
````

For arrays like `unk8[]`, size is inferred by the address difference from the next label.

Type `num` is to acknowledge that numbers can be arbitrarily large in javascript. Remember this is not really a tape but a javascript array, and its elements can get arbitrarily large. Still it can be useful to think of them as logical bytes. For example `inst8` are mostly bytes themselves and addresses are consistently 16-bit.

### 3.8.1 Pend
