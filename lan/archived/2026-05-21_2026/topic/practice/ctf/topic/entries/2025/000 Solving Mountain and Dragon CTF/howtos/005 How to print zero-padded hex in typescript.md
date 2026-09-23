# 1 Journal

* [x] 

From [^spawn-howto-98bf9d](005%20How%20to%20print%20zero-padded%20hex%20in%20typescript.md#spawn-howto-98bf9d) in [6.6 Inferring tape data labels through play and diffs](005%20How%20to%20print%20zero-padded%20hex%20in%20typescript.md#66-inferring-tape-data-labels-through-play-and-diffs)

2025-08-02 Wk 31 Sat - 02:51

In python3, we can do something like

````python
f'0x{pc:04x}'
````

for a 4 0-padded hexadecimal print

This \[stackoverflow answer\] points to `.toString(16)` :

````ts
var g: number = 255;
alert(g.toString(16));
````

But this wouldn't pad it with 4 zeros.

This [stackoverflow answer](https://stackoverflow.com/a/63297748/6944447) points to `.padStart(n, "0")`:

````ts
str.padStart(9 ,"0")
````

So put together we get...

````ts
`0x${pc.toString(16).padStart(4, "0")}`
````

We can also generalize

````ts
export function addr16(n: number): string {
  return `0x${n.toString(16).padStart(4, "0")}`;
}

// ...

`${addr16(pc)}`
````
