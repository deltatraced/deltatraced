---
context_type: investigation
status: done
---

Parent: [lan/2026/proj/003-clusterline-md/entry/000-clusterline-md-getting-started/000-clusterline-md-getting-started](../000-clusterline-md-getting-started.md)

Spawned by: [lan/2026/proj/003-clusterline-md/entry/000-clusterline-md-getting-started/issue/001 Using async for js extern instead of future leads to wrong pkg generation via wasm-pack](../issue/001%20Using%20async%20for%20js%20extern%20instead%20of%20future%20leads%20to%20wrong%20pkg%20generation%20via%20wasm-pack.md)

Spawned in: [^spawn-invst-d48fec](../issue/001%20Using%20async%20for%20js%20extern%20instead%20of%20future%20leads%20to%20wrong%20pkg%20generation%20via%20wasm-pack.md#spawn-invst-d48fec)

# Problem

Look at the rust reference, for example in https://doc.rust-lang.org/reference/comments.html and you will see it's all directed acyclic graphs which branch and then eventually merge back to the same point:

````
		   b1  
           /---\
o --- a ---     --- c --- o
           \---/
           b2
o ----------------------> o
````

Can we configure this compile-time in Rust, such that by trait bound calculation, we can compile down a complex expression? For example, see

````rust
pub type NoteUrlTokenTree = (
    CharToken<'[', 2>, (
    TextToken, (
    Either<
        NoToken,
        (CharToken<'#', 1>, IdentToken),
    >, (
    Either<
        NoToken,
        (CharToken<'|', 1>, TextToken),
    >,
    CharToken<']', 2>,
))));
````

So long as the tokens are already defined, something like this should be enough to parse a `NoteUrl`.

# Journal

2026-06-05 Wk 23 Fri - 10:26 +03:00

Spawn [lan/2026/proj/003-clusterline-md/entry/000-clusterline-md-getting-started/investigation/001 Rust Can we make a typed recursive tuple for an arbitrary series of calculations on a given number?](001%20Rust%20Can%20we%20make%20a%20typed%20recursive%20tuple%20for%20an%20arbitrary%20series%20of%20calculations%20on%20a%20given%20number%3F.md) ^spawn-invst-11da35

2026-06-06 Wk 23 Sat - 09:34 +03:00

Apparently it's called a Series-Parallel DAG.

* https://en.wikipedia.org/wiki/Series%E2%80%93parallel_graph
* https://www.cis.upenn.edu/~alur/POPL23-SSPG.pdf

This is what we can reach with `Then<B, A>` and `Either<L, R>`. Since one is a series connection and the other a parallel connection.
