---
parent: '[[001 Proc Math Problems]]'
spawned_by: '[[001 Proc Math Problems]]'
context_type: entry
---

Parent: [001 Proc Math Problems](../001%20Proc%20Math%20Problems.md)

Spawned by: [001 Proc Math Problems](../001%20Proc%20Math%20Problems.md)

Spawned in: [^spawn-entry-8de42c](../001%20Proc%20Math%20Problems.md#spawn-entry-8de42c)

# 1 Journal

2026-04-28 Wk 18 Tue - 18:47 +03:00

Spawn [003 Archived Rational Parameterization of the circle](../../../001%20Math%20Problems/tasks/003%20Archived%20Rational%20Parameterization%20of%20the%20circle.md) ^spawn-task-1d5e62

2026-05-11 Wk 20 Mon - 19:22 +03:00

Spawn [003 002 Errata for Rational Parameterization of Circle](003%20002%20Errata%20for%20Rational%20Parameterization%20of%20Circle.md) ^spawn-entry-b771b9

2026-04-28 Wk 18 Tue - 19:09 +03:00

[dmehrle The Unit Circle: A Rich Example for Gaining Perspective](https://pi.math.cornell.edu/~dmehrle/notes/old/alggeo/02UnitCircleExample.pdf)

This post has some hints for the derivation.

2026-04-30 Wk 18 Thu - 14:12 +03:00

We have a solution now! It just depends on the similarity of two triangles, one embedded in the other, and the circle constraint on the pointed projected onto in the circle, and then algebraic simplifications to get the parameterization. Basically we can sweep the slope of a line, and from the slope `h` we can send it to the corresponding point on the circle. Since `h` can be a rational number, the circle can get arbitrarily densely generated. While we cannot get the starting point this way, we can if we extend the field we're working with to include infinity, and so the slope at $\pm \infty$ gives us our original point.

2026-04-30 Wk 18 Thu - 14:59 +03:00

The graph

![Pasted image 20260430150504.png](../../../../../../../../../../../attachments/Pasted%20image%2020260430150504.png)

was created using [geometry desmos](https://www.desmos.com/geometry)!
extra point at infinity to account for this, or some other solutio
2026-04-30 Wk 18 Thu - 16:31 +03:00

Writing the explanation, I want to codify my proof, so I have to reach for a definition of colinear to make my argument about points `A`

2026-04-30 Wk 18 Thu - 19:19 +03:00

We pseudo-formalized the proof now. Some missing details is to prove that our choice implementation for `e`:

````haskell
	e : (h : ℚ) → Vect2
	e h .x = (1 - h²) / (1 + h²)

	e h .y = 2h / (1 + h²)
 -- e h .x = E₁ᵣ i1
 -- e h .y = E₂ᵣ i1
````

Is continuous and (nearly) bijective with the circle. We know it is injective from the line where `h` is sweeped to the circle, but the original point `A` on the circle cannot be mapped onto continuously. If you wiggle the output around `A` on the circle, you will be oscillating between $\pm \infty$ on the line. So we need `h` to be on a type with an extra point at $\infty$ or some other solution that gives us a bijective map. Then we can say that this type and the circle are an equivalence. For now though, we did obtain an implementation of `e` that sends points on the line to points on the circle!

The uniqueness proof also was left to geometric intuition, in that as the slope sweeps the circle, that sweep is continuous, and thus a unique `h` goes to a unique point `B`. But this should also be proven.

2026-05-02 Wk 18 Sat - 14:05 +03:00

````haskell
Circleₚ : Type
Circleₚ = ∑ [ p ∈ Vect2 ] (p ≢ -1, 0 ∧ (p .fst)² + (p .snd)² ≡ 1)
````

````
e : ℚ → Circleₚ
e h .fst .fst = (1 - h²) / (1 + h²)
e h .fst .snd = 2h / (1 + h²)
e h .snd = {- Proof (e h .fst) satisfies the partial circle constraints -}

e⁻¹ : Circleₚ → ℚ
e⁻¹ ((x , y), _) = (1 - x) / y
````

To prove that ℚ ≃ Circleₚ we need

````haskell
to-fro : ∀ (p : Circleₚ) → e (e⁻¹ p) ≡ p
fro-to : ∀ (h : ℚ) → e⁻¹ (e h) ≡ h

to-fro needs to also be broken apart:
P₀ : e (e⁻¹ p) .fst .fst ≡ p .fst
P₁ : e (e⁻¹ p) .fst .snd ≡ p .snd
P₂ : e (e⁻¹ p) .snd ≡ p .snd
````

Then connected together:

````haskell
-- Something like this, but not really. ×-map-≡ won't work for constructing a
-- path between two dependant pairs.
to-fro = ×-map-≡ (×-map-≡ P₀ P₁) P₂
````

I verified that we have P₀ and P₁ but P₂ is not trivial. We might need facts of proposition contractibility for this.

The subtype we made, Circleₚ, requires a path between two dependant pairs, which requires a path between proofs.

In Rocq for example with ℚ, we defined an equivalence proposition `≃ℚ : (a b : ℚ) → Prop`, which only relates the delta-trace
portions of a and b, and ignores the proofs that a and b are rational (mainly ℚ being a quotient type over ℤ² where the second components,
is non-zero).

Our use of `×-map-≡` is also insufficient for `P₂`  because it only works for non-dependent pairs, but our sigma type is a dependent pair.

2026-05-02 Wk 18 Sat - 15:58 +03:00

The other problem is we're not really justified in using `ℚ` because we have still not defined it in cubical Agda. we assume a quotient structure, so we know that it has two representations, like 2/2 and 3/3, which are identical. But this isn't something we can build off of a dependant pair. A dependant pair may give us data in `ℤ²` like (1 , 4) with proof that the second component is non-zero but it will not tell us that (2, 8) and (1, 4) are in fact the same point.

Here are some resources that use quotient types: [gh kcsmnt0/quotient](https://github.com/kcsmnt0/quotient)
