---
status: todo
---

# 1 Journal

## 1.1 The beginning

2025-07-27 Wk 30 Sun - 15:11

Gonna put any sketches in [Wk 30 Drawing Math Book 000 Starting out.excalidraw](../../../../../../../../../main/drawings/Wk%2030%20Drawing%20Math%20Book%20000%20Starting%20out.excalidraw.md)

## 1.2 Annotations

2025-07-27 Wk 30 Sun - 15:19

 > 
 > vpg 12
 > There is a special symbol, ∈,
 > used to express the idea that an element belongs to a set.

I guess for some set $A = \lbrace a_1, a_2, a_3 \rbrace$, we would say $a_1$ element-of A, or $a_1 \in A$ .

When I was looking into some constructive math stuff, I was also thinking in terms of `of-type` rather than `element-of`, since types detail how data is interpreted rather than being really a set.

I would write

![Pasted image 20250727152418.png](../../../../../../../../../../../../attachments/Pasted%20image%2020250727152418.png)

to say $a_1$ is `of-type` A.

When I figure out including LaTeX stuff here I'd bring it back as `\oft`...

2025-07-27 Wk 30 Sun - 15:26

 > 
 > vpg 12
 > The order of the elements in a set does not matter, so we could have written
 > S = {1, 2, 3} as S = {1, 3, 2}, or as S = {3, 2, 1}. If an element is repeated in a set,
 > we do not count the multiplicity. Thus {1, 2, 3, 1} is the same set as S = {1, 2, 3}.

I'm pretty sure I've talked with someone before who disagreed with this convention and said that I may be more referring to the programmatic set as having these constraints. They said that a set in mathematics is the most basic notion, and only includes restrictions like no repetition if you specify so. But here they do mean that normal notion of set.

 > 
 > vpg 13
 > We use the fancy letter “Z” because the word “integer” in German is “Zahlen.”

2025-07-27 Wk 30 Sun - 15:36

 > 
 > vpg 14
 > We read the colon as “such that,” \[...\] Writing sets with a colon is called set-builder notation.

so $\lbrace 2x : x \in Z \rbrace$ reads "The set of the form 2x such-that x is-in Z".

 > 
 > vpg 15
 > When A is a subset of B we write A ⊆ B and if it is not a subset we write A ⊈ B.

subset-of relation!

 > 
 > vpg 15
 > Fix A = {1, 5, 6} and B = {1, 5, 6, 7, 8}. Is A a subset of B?

They have interesting convention for specifying inputs to a problem. `Fix "inputs". Problem query"`. I guess I usually see Given {inputs}, query.

 > 
 > vpg 16
 > Definition 1.13.
 > If A ⊆ S and A̸ = S, we say that A is a proper subset of S, and we write A ⊊ S.
 > ^defn1-13

Hmm. I figured they would use $A \subset B$ but then again, subset-or-include is built off of this.
