---
parent: '[[000 Wk 37 Addressing shi PR 10]]'
context_type: investigation
status: done
---

Parent: [000 Wk 37 Addressing shi PR 10](../000%20Wk%2037%20Addressing%20shi%20PR%2010.md)

# 1 Journal

2025-09-08 Wk 37 Mon - 17:43

May said to remove `_quotation` [here](https://github.com/Utagai/shi/pull/10#discussion_r2328198664)

* $\to$ means it uses the parent
* $\leftarrow$ means it is used by the parent
* $\downarrow$ means it contains the parent
* $\uparrow$  means the parent is contained by it

The dependency tree:

* [`QuotePair`](https://github.com/Utagai/shi/blob/ef0428b1440153818ee5512adf378ba1544e0598/src/tokenizer.rs#L36)
* $\to$ [`find_quote_pairs`](https://github.com/Utagai/shi/blob/ef0428b1440153818ee5512adf378ba1544e0598/src/tokenizer.rs#L109)
* $\to$  [`split_into_quote_blobs`](https://github.com/Utagai/shi/blob/ef0428b1440153818ee5512adf378ba1544e0598/src/tokenizer.rs#L213)
* $\leftarrow$ [`construct_slices_from_pairs`](https://github.com/Utagai/shi/blob/ef0428b1440153818ee5512adf378ba1544e0598/src/tokenizer.rs#L168)
* where [`QuotePair`](https://github.com/Utagai/shi/blob/ef0428b1440153818ee5512adf378ba1544e0598/src/tokenizer.rs#L36)s are actually processed.
* $\to$ many tests use it
* $\to$ [`DefaultTokenizer -> tokenize`](https://github.com/Utagai/shi/blob/ef0428b1440153818ee5512adf378ba1544e0598/src/tokenizer.rs#L296)
* $\downarrow$ [`DefaultTokenizer`](https://github.com/Utagai/shi/blob/ef0428b1440153818ee5512adf378ba1544e0598/src/tokenizer.rs#L19)
* $\to$ [`Parser`](https://github.com/Utagai/shi/blob/ef0428b1440153818ee5512adf378ba1544e0598/src/parser.rs#L8)
* $\uparrow$ [`Parser -> parse`](https://github.com/Utagai/shi/blob/ef0428b1440153818ee5512adf378ba1544e0598/src/parser.rs#L269)
* $\to$ [`ExecCompleter -> complete`](https://github.com/Utagai/shi/blob/ef0428b1440153818ee5512adf378ba1544e0598/src/readline.rs#L374)
* Handles autocomplete functionality
* $\to$ [`Shell -> parse`](https://github.com/Utagai/shi/blob/ef0428b1440153818ee5512adf378ba1544e0598/src/shell.rs#L148)
* Used in eval directly

Just capturing this convention for later reuse:

Spawn [000 Call tree dependnecy arrow legend](../entries/000%20Call%20tree%20dependnecy%20arrow%20legend.md) ^spawn-entry-ee5e79

2025-09-16 Wk 38 Tue - 18:49 +03:00

[`find_quotes`](https://github.com/Utagai/shi/blob/ef0428b1440153818ee5512adf378ba1544e0598/src/tokenizer.rs#L84) serves a similar purpose to [`find_quote_pairs`](https://github.com/Utagai/shi/blob/ef0428b1440153818ee5512adf378ba1544e0598/src/tokenizer.rs#L109). There, quotation is actually used, but for [`QuoteLoc`](https://github.com/Utagai/shi/blob/ef0428b1440153818ee5512adf378ba1544e0598/src/tokenizer.rs#L27) instead.

* [`find_quotes`](https://github.com/Utagai/shi/blob/ef0428b1440153818ee5512adf378ba1544e0598/src/tokenizer.rs#L84)
* $\to$ [`split_into_quote_blobs`](https://github.com/Utagai/shi/blob/ef0428b1440153818ee5512adf378ba1544e0598/src/tokenizer.rs#L213)

They're used together...

2025-09-16 Wk 38 Tue - 19:15 +03:00

[`construct_slices_from_pairs`](https://github.com/Utagai/shi/blob/ef0428b1440153818ee5512adf378ba1544e0598/src/tokenizer.rs#L168) is where the [`QuotePair`](https://github.com/Utagai/shi/blob/ef0428b1440153818ee5512adf378ba1544e0598/src/tokenizer.rs#L36) s end up being consumed to create [`Blob`](https://github.com/Utagai/shi/blob/ef0428b1440153818ee5512adf378ba1544e0598/src/tokenizer.rs#L48)s.

2025-09-16 Wk 38 Tue - 19:41 +03:00

So it's not necessary to keep the specific quotations, since immediately after the library is only concerned with quoted blobs. They can just be removed, the importance of the exact quote characters is handled when [`QuoteLoc`](https://github.com/Utagai/shi/blob/ef0428b1440153818ee5512adf378ba1544e0598/src/tokenizer.rs#L27) are turned into [`QuotePair`](https://github.com/Utagai/shi/blob/ef0428b1440153818ee5512adf378ba1544e0598/src/tokenizer.rs#L36) by [`split_into_quote_blobs`](https://github.com/Utagai/shi/blob/ef0428b1440153818ee5512adf378ba1544e0598/src/tokenizer.rs#L213).

Removed its use.
