# 1 Definition

The symbol $-\!\!\!\in$ reads "of type".  $a_1\  -\!\!\!\in A$ reads $a_1$ of-type $A$.

This is in contrast to $\in$ which means element-of, or of-set to be consistent with of-type. $a_1 \in A$ indicated that there exists a set $A$, but $a_1\  -\!\!\!\in A$ does not. Instead, there exists a type $A$, which a set of rules for generating values.

# 2 Examples

Types can correspond with algorithms that generates values of that type.

For example we have the [Nat](000%20Nat.md) type:

## 1.2 nLab, Type

Defined in [nlab](https://github.com/ncatlab/nlab-content/blob/dff071804718922fdd4052148e33652b7760010c/pages/5/6/9/8/8965/content.md?plain=1#L158) as follows:

````
Inductive nat : Type :=
 | zero : nat
 | succ : nat -> nat.
````

Using this definition, you can construct any natural number value. The main point is that this is a definition of construction, and not a set of objects.

# 3 Use

Currently, the symbol is not being loaded. You can alternatively use the symbol $:$ for of-type.
