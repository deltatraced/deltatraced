# 1 Journal

* [x] 

2025-08-01 Wk 31 Fri - 13:49

So I could do

````ts
export var m_tape_snapshot: number[] = g.g.tape;
````

But is this a shallow copy or just setting a reference?

[this post](https://medium.com/@ziyoshams/deep-copying-javascript-arrays-4d5fc45a6e3e) explains that it is indeed just a reference. For our purposes a shallow copy suffices, but we can use the spread operator syntax `[...arr]`  for a deep copy.
