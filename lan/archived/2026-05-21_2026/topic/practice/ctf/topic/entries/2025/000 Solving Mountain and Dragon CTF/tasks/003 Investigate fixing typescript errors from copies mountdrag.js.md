# 1 Journal

* [x] 

2025-07-30 Wk 31 Wed - 05:07

````js
g_mq.innerHTML += (g_inp >> 2) & (1 == 1) ? "F" : " ";
							     ~~~~~~~~

// error
The right-hand side of an arithmetic operation must be of type 'any', 'number', 'bigint' or an enum type.ts(2363)
````

Seems like type shorthand to get boolean?

````js
g_mq.innerHTML += ((g_inp >> 2) & 1) != 0 ? "F" : " ";
````

2025-07-30 Wk 31 Wed - 06:22

Moving parenthesis is not enough. values can be non-0 due to remaining flags.

2025-07-30 Wk 31 Wed - 06:29

````javascript
Function`$${"\x61 \x3D\x20\x6E\x65\x77 \x44\x61\x74\x65\x28\x29\x5B\x27\x67\x65\x74\x53\x65\x63\x6F\x6E\x64\x73\x27\x5D\x28\x29 "}$`();

// error
Argument of type 'TemplateStringsArray' is not assignable to parameter of type 'string'.ts(2345)
````

2025-07-30 Wk 31 Wed - 07:40

We can just replace it with the actual code we found in [5.2 Turn hex bytes into string in terminal](003%20Investigate%20fixing%20typescript%20errors%20from%20copies%20mountdrag.js.md#52-turn-hex-bytes-into-string-in-terminal).

````ts
g_a = new Date()['getSeconds']()
````
