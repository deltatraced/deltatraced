---
parent: '[[001 Math Problems]]'
spawned_by: '[[000 Spawn Logs for Math Problems]]'
context_type: task
status: todo
---

Parent: [001 Math Problems](../001%20Math%20Problems.md)

Spawned by: [000 Spawn Logs for Math Problems](../entries/000%20Spawn%20Logs%20for%20Math%20Problems.md)

Spawned in: [^spawn-task-7f31a8](../entries/000%20Spawn%20Logs%20for%20Math%20Problems.md#spawn-task-7f31a8)

Problem: [000 NW Alg Top S1 Mult 1](000%20NW%20Alg%20Top%20S1%20Mult%201.md)

---

Problem can be found in [Two-dimensional surfaces: the sphere | Algebraic Topology 3 | NJ Wildberger](https://www.youtube.com/watch?v=R_gDV17X7pc&list=PLIljB45xT85D7wczwyUQdwDe2duZ7wPTf&index=3), 48:20.

# 1 Problem

* $e : \mathbb{Q} \to \mathbb{Q^2}$
* $e = \frac{1 - h^2}{1 + h^2} , \frac{2h}{1 + h^2}$

$\text{Prop}_1 : \forall (h_1\ h_2 : \mathbb{Q}) \to (e\ h_1) * (e\ h_2) \equiv (e\ h_3)$

where

* $h_3 : \mathbb{Q}$
* $h_3 = \frac{h_1 + h_2}{1 - h_1 \cdot h_2}$

# 2 Error

Pairwise multiplication defined leading to simulation discovery of other multiplication definitions

# 3 Argument

We will need to make some assumptions. How do we interpret `*` for $\mathbb{Q^2}$?

Let's define it as follows:

* `_*_`$: (a\ b : \mathbb{Q}^2) \to \mathbb{Q}^2$
* $a * b\ \text{.fst} = a \text{.fst} * b \text{.fst}$
* $a * b\ \text{.snd} = a \text{.snd} * b \text{.snd}$

Similarly we need

* `_`$\pm$`_` $: (a\ b\ : \mathbb{Q}^2) \to \mathbb{Q}^2$
* $a \pm b\ \text{.fst} = a \text{.fst} \pm b \text{.fst}$
* $a \pm b\ \text{.snd} = a \text{.snd} \pm b \text{.snd}$

for either addition or subtraction.

We can begin to simplify each side, and also re-arrange the equation. $Prop_1$ builds an equational reasoning chain and $\leftrightarrow\text{-Prop}_1$  builds an implication chain to other equivalent equations. Since we are exploring with $\leftrightarrow\text{-Prop}_1$, we leave the right endpoint as a hole $?_0$ .

In context,

* $h_3 : \mathbb{Q}$

* $h_3 = \frac{h_1 + h_2}{1 - h_1 \cdot h_2}$

* $e : \mathbb{Q} \to \mathbb{Q^2}$

* $e = \frac{1 - h^2}{1 + h^2} , \frac{2h}{1 + h^2}$

* $\text{Prop}_1 : \forall (h_1\ h_2 : \mathbb{Q}) \to (e\ h_1) * (e\ h_2) \equiv (e\ h_3)$

* $\text{Prop}_1\ h_1\ h_2 =$

$$1.1.\ (e\ h_1) * (e\ h_2)$$

$\equiv \langle$ unfold $e$ $\rangle$

$$1.2.\ (
\\frac{1 - h_1^2}{1 + h_1^2},
\\frac{2 \cdot h_1}{1 + h_1^2})

* (
  \\frac{1 - h_2^2}{1 + h_2^2} ,
  \\frac{2 \cdot h_2}{1 + h_2^2})
  $$

$\equiv \langle$ unfold $*$ $\rangle$

$$1.3.\ 
\frac
	{\color{green}(1 - h_1^2) \cdot (1 - h_2^2)}
	{\color{green}(1 + h_1^2) \cdot (1 + h_2^2)} , 
\frac
	{\color{orange}(2 \cdot h_1) \cdot (2 \cdot h_2)}
	{\color{green}(1 + h_1^2) \cdot (1 + h_2^2)}$$

$\equiv \langle$

* $\color{green}\forall (a\ b\ c\ d : \mathbb{Q}) \to	(a \pm b) \cdot (c \pm d) \equiv ac \pm ad \pm bc + bd$
* $\color{orange}\forall (a\ b\ c : \mathbb{Q}) \to (a \cdot b) \cdot (a \cdot c) \equiv a^2 \cdot b \cdot c$ $\rangle$

$$1.4.\ 
\frac
	{1 - h_2^2 - h_1^2 + h_1^2 \cdot h_2^2}
	{1 + h_2^2 + h_1^2 + h_1^2 \cdot h_2^2} , 
\frac
	{(2^2 \cdot h_1 \cdot h_2)}
	{1 + h_2^2 + h_1^2 + h_1^2 \cdot h_2^2}
$$

$\equiv \langle$ $?_{1p}$ $\rangle$

$$ 2.3.\ 
\frac
	{1 - 
		(\frac{h_1 + h_2}{1 - h_1 \cdot h_2})^2
	}{1 + 
		(\frac{h_1 + h_2}{1 - h_1 \cdot h_2})^2
	} , 
\frac
	{2 \cdot 
		(\frac{h_1 + h_2}{1 - h_1 \cdot h_2})
	}{ 1 + 
		(\frac{h_1 + h_2}{1 - h_1 \cdot h_2})^2
	}
$$

$\equiv \langle$ sym (unfold $h_3$) $\rangle$

$$2.2.\ 
\frac
	{1 - h_3^2}
	{1  + h_3^2} , 
\frac
	{2 \cdot h_3}
	{1 + h_3^2}$$

$\equiv \langle$ sym (unfold $e$) $\rangle$

$$2.1.\ (e\ h_3)$$

$\blacksquare$

Now let's use the simplifications we've done and re-arrange the equation:

$$\leftrightarrow\text{-Prop}_1 : 
	\forall (h_1\ h_2 : \mathbb{Q}) \to 
		(e\ h_1) * (e\ h_2) \equiv (e\ h_3)\ 
	\leftrightarrow\ ?_0$$

* $\leftrightarrow\text{-Prop}_1\ h_1\ h_2\ =$

$$
1.1.\ (e\ h_1) * (e\ h_2) \equiv (e\ h_3)
$$

$\leftrightarrow \langle$ ap

* $\forall (a\ b\ : \mathbb{Q^2}) \to a \equiv b \leftrightarrow a - b \equiv 0, 0$ $\rangle$

$$1.2.\ \color{orange} (e\ h_1) * (e\ h_2) \color{white}- \color{cyan} (e\ h_3) \color{white} \equiv 0 , 0
$$

$\leftrightarrow \langle$ $\color{orange}\leftrightarrow\text{-transport}\ (\text{subpath}\ (\text{Prop}_1\ h_1\ h_2)\ 1.1\  1.4\ )\ \color{cyan}\leftrightarrow\text{-transport}\ (\text{subpath}\ (\text{Prop}_1\ h_1\ h_2)\ 2.1\ 2.3)$  $\rangle$

$$1.3.\
\\frac
{1 - h_2^2 - h_1^2 + h_1^2 \cdot h_2^2}
{1 + h_2^2 + h_1^2 + h_1^2 \cdot h_2^2} ,
\\frac
{(2^2 \cdot h_1 \cdot h_2)}
{1 + h_2^2 + h_1^2 + h_1^2 \cdot h_2^2}

\\ -\

\\frac
{1 -
(\frac{h_1 + h_2}{1 - h_1 \cdot h_2})^2
}{1 +
(\frac{h_1 + h_2}{1 - h_1 \cdot h_2})^2
} ,
\\frac
{2 \cdot
(\frac{h_1 + h_2}{1 - h_1 \cdot h_2})
}{ 1 +
(\frac{h_1 + h_2}{1 - h_1 \cdot h_2})^2
}

\\equiv 0 , 0
$$

$\leftrightarrow \langle$ ap-≡ (unfold `_-_` $: \mathbb{Q}^2 \to \mathbb{Q}^2 \to \mathbb{Q}^2$) $\rangle$

$$1.4.\
\\frac{1 - h_2^2 - h_1^2 + h_1^2 \cdot h_2^2}{1 + h_2^2 + h_1^2 + h_1^2 \cdot h_2^2} -
\\frac{1 - (\frac{h_1 + h_2}{1 - h_1 \cdot h_2})^2}{1 + (\frac{h_1 + h_2}{1 - h_1 \cdot h_2})^2}
,

\\frac{(2^2 \cdot h_1 \cdot h_2)}{1 + h_2^2 + h_1^2 + h_1^2 \cdot h_2^2} -
\\frac{2 \cdot (\frac{h_1 + h_2}{1 - h_1 \cdot h_2})}{1 + (\frac{h_1 + h_2}{1 - h_1 \cdot h_2})^2}
\\equiv 0 , 0
$$

$\leftrightarrow \langle$ ap-≡ $\forall (a\ b\ c\ d : \mathbb{Q}) \to \frac{a}{b} - \frac{c}{d} \equiv \frac{a*d - b*c}{b*d}$  $\rangle$

$$1.5.\ 
\frac {
	\color{white}
	(1 - h_2^2 - h_1^2 + h_1^2 \cdot h_2^2) \cdot
	(1 + (\frac{h_1 + h_2}{1 - h_1 \cdot h_2})^2)
	\color{white} - \color{white}
	(1 + h_2^2 + h_2^2 + h_1^2 \cdot h_2^2) \cdot
	(1 - (\frac{h_1 + h_2}{1 - h_1 \cdot h_2})^2)
	\color{white}
} {
	\color{yellow}
	(1 + h_2^2 + h_1^2 + h_1^2 \cdot h_2^2) \cdot 
	(1 + (\frac{h_1 + h_2}{1 - h_1 \cdot h_2})^2)
	\color{yellow}
}
$$
$$,
\\frac {
((2^2 \cdot h_1 \cdot h_2)) \cdot
(1 + (\frac{h_1 + h_2}{1 - h_1 \cdot h_2})^2) -
(1 + h_2^2 + h_1^2 + h_1^2 \cdot h_2^2) \cdot
(2 \cdot (\frac{h_1 + h_2}{1 - h_1 \cdot h_2})
} {
\\color{yellow}
(1 + h_2^2 + h_1^2 + h_1^2 \cdot h_2^2) \cdot
(1 + (\frac{h_1 + h_2}{1 - h_1 \cdot h_2})^2)
\\color{white}
}

\\equiv 0 , 0
$$

$\leftrightarrow \langle$ ap-≡ $\color{yellow}{abstract}$ $\rangle$

$$1.6.
\text{let } A =
	\color{orange}
	(1 + h_2^2 + h_1^2 + h_1^2 \cdot h_2^2) \cdot 
	(1 + (\frac{h_1 + h_2}{1 - h_1 \cdot h_2})^2)\ \color{lightgray} \text{in}
$$
$$
\frac {
	\color{teal}
	(1 - h_2^2 - h_1^2 + h_1^2 \cdot h_2^2) \cdot
	(1 + (\frac{h_1 + h_2}{1 - h_1 \cdot h_2})^2)
	-
	(1 + h_2^2 + h_2^2 + h_1^2 \cdot h_2^2) \cdot
	(1 - (\frac{h_1 + h_2}{1 - h_1 \cdot h_2})^2)
	\color{white}
} { A }
$$
$$
,
\frac {
	((2^2 \cdot h_1 \cdot h_2)) \cdot
	(1 + (\frac{h_1 + h_2}{1 - h_1 \cdot h_2})^2) -
	(1 + h_2^2 + h_1^2 + h_1^2 \cdot h_2^2) \cdot
	(2 \cdot  (\frac{h_1 + h_2}{1 - h_1 \cdot h_2})
} { A }
\equiv 0 , 0
$$

$\leftrightarrow \langle$ ap-≡

* $\color{orange}\forall (a\ b\ c : \mathbb{Q}) \to (1 + a + b + a \cdot b) \cdot (1 + c^2) \equiv 1 + a + b + a \cdot b + c^2 + a \cdot c^2 + b \cdot c^2 + a \cdot b \cdot c^2$
* $\color{teal}\forall (a\ b\ c : \mathbb{Q}) \to (1 - a - b + a \cdot b) \cdot (1 + c^2) - (1 + a + b + a \cdot b) \cdot (1 - c^2) \equiv$
  * $\color{teal}2 \cdot (a \cdot b \cdot c^2 - a - b + c^2)$ [subproblem](001%20NW%20Alg%20Top%20S1%20Mult%201%20Subproblem%201.md) $\rangle$ ^5cc9b7

$$1.7.\ 
\text{let } A = 1 + (h_2^2) + (h_1^2) + (h_1^2 \cdot h_2^2) + 
(\frac{h_1 + h_2}{1 - h_1 \cdot h_2})^2 + 
(h_2^2 \cdot (\frac{h_1 + h_2}{1 - h_1 \cdot h_2})^2)\ +
$$
$$
(h_1^2 \cdot (\frac{h_1 + h_2}{1 - h_1 \cdot h_2})^2) + 
(h_2^2 \cdot h_1^2 \cdot (\frac{h_1 + h_2}{1 - h_1 \cdot h_2})^2)
$$
$$
2 \cdot
\frac {
	h_2^2 \cdot h_1^2 \cdot (\frac{h_1 + h_2}{1 - h_1 \cdot h_2})^2 - h_2^2 - h_1^2 + (\frac{h_1 + h_2}{1 - h_1 \cdot h_2})^2
} { A }
$$
$$
,
\frac {
	((2^2 \cdot h_1 \cdot h_2)) \cdot
	(1 + (\frac{h_1 + h_2}{1 - h_1 \cdot h_2})^2) -
	(1 + h_2^2 + h_1^2 + h_1^2 \cdot h_2^2) \cdot
	(2 \cdot  (\frac{h_1 + h_2}{1 - h_1 \cdot h_2})
} { A }
\equiv 0 , 0
$$

$\equiv \langle$ $\forall(a\ b\ c : \mathbb{Q}) \to (c \ne 0) \to  \frac{a}{c} , \frac{b}{c} \equiv 0 , 0 \leftrightarrow a , b \equiv 0 , 0$ $\rangle$

$$1.8.\ 
2 \cdot \left(
	h_2^2 \cdot h_1^2 \cdot (\frac{h_1 + h_2}{1 - h_1 \cdot h_2})^2 - h_2^2 - h_1^2 + (\frac{h_1 + h_2}{1 - h_1 \cdot h_2})^2 \right)
$$
$$
,
((2^2 \cdot h_1 \cdot h_2)) \cdot
(1 + (\frac{h_1 + h_2}{1 - h_1 \cdot h_2})^2) -
(1 + h_2^2 + h_1^2 + h_1^2 \cdot h_2^2) \cdot
(2 \cdot  (\frac{h_1 + h_2}{1 - h_1 \cdot h_2})
\equiv 0 , 0
$$

$\leftrightarrow \langle$ $\forall (a\ b : \mathbb{Q}) \to a , b \equiv 0 , 0 \leftrightarrow (1 - h_1 \cdot h_2)^2 \cdot a , (1 - h_1 \cdot h_2)^2 \cdot b \equiv 0 , 0$ $\rangle$

$$1.9.\ 
\color{orange}
(1 - h_1 \cdot h_2)^2 \cdot
2 \cdot 
\left(
	h_2^2 \cdot h_1^2 \cdot (\frac{h_1 + h_2}{1 - h_1 \cdot h_2})^2 - h_2^2 - h_1^2 + (\frac{h_1 + h_2}{1 - h_1 \cdot h_2})^2 \right)
$$
$$
,
\color{pink}
(1 - h_1 \cdot h_2)^2 \cdot (
(2^2 \cdot h_1 \cdot h_2) \cdot
(1 + (\frac{h_1 + h_2}{1 - h_1 \cdot h_2})^2) -
(1 + h_2^2 + h_1^2 + h_1^2 \cdot h_2^2) \cdot
$$
$$
\color{pink}
(2 \cdot  (\frac{h_1 + h_2}{1 - h_1 \cdot h_2})))
\color{lightgray}
\equiv 0 , 0
$$

$\leftrightarrow \langle$ ap-≡ (

* $\color{orange}\forall (a\ b\ c\ d : \mathbb{Q}) \to a^2 \cdot 2 \cdot (b \cdot c \cdot \frac{d^2}{a^2} - b - c + \frac{d^2}{a^2}) \equiv 2 \cdot (b \cdot c \cdot d^2 - b \cdot a^2 - c   \cdot a^2 + d^2)$
* $\color{pink}\forall (a\ b\ c\ d : \mathbb{Q}) \to a^2 \cdot \left( (2^2 \cdot b \cdot c) \cdot (1 + \frac{d^2}{a^2}) - (1 + c^2 + b^2 + b^2 \cdot c^2) \cdot (2 \cdot \frac{d}{a}) \right)$
  * $\color{pink}\equiv  2 \cdot \left( (2 \cdot b \cdot c) \cdot (a^2 + d^2) - (1 + c^2 + b^2 + b^2 \cdot c^2) \cdot (d \cdot a) \right)$
    )$\rangle$

$$1.10.\ 
2 \cdot \left( h_2^2 \cdot h_1^2 \cdot (h_1 + h_2)^2 - h_2^2 \cdot (1 - h_1 \cdot h_2)^2 - h_1^2 \cdot (1 - h_1 \cdot h_2)^2 + (h_1 + h_2)^2  \right)
$$
$$
,
2 \cdot ((2 \cdot h_1 \cdot h_2) \cdot ((1 - h_1 \cdot h_2)^2 + (h_1 + h_2)^2) - (1 + h_2^2 + h_1^2 + h_1^2 \cdot h_2^2) \cdot\ 
$$
$$
((h_1 + h_2) \cdot (1 - h_1 \cdot h_2)))
\equiv 0 , 0
$$

$\equiv \langle$

* $\forall (a\ b : \mathbb{Q}) \to b^2 \cdot a^2 \cdot (a + b)^2 - b^2 \cdot (1 - a \cdot b)^2 - a^2 \cdot (1 - a \cdot b)^2 + (a + b)^2$
  * $\equiv b^2 \cdot a^2 \cdot (a^2 + 2 \cdot a \cdot b + b^2) - b^2 \cdot (1 - a \cdot b)^2 - a^2 \cdot (1 - a \cdot b)^2 + (a^2 + 2 \cdot a \cdot b + b^2)$
  * $\equiv b^2 \cdot a^2 \cdot (a^2 + 2 \cdot a \cdot b + b^2) - b^2 \cdot (1 - 2 \cdot a \cdot b + a^2 \cdot b^2) - a^2 \cdot (1 - 2 \cdot a \cdot b\ +$
    $a^2 \cdot b^2) + (a^2 + 2 \cdot a \cdot b + b^2)$
  * $\equiv b^2 \cdot a^2 \cdot a^2 + b^2 \cdot a^2 \cdot (2 \cdot a \cdot b) + b^2 \cdot a^2 \cdot b^2 - b^2 + b^2 \cdot (2 \cdot a \cdot b) - b^2 \cdot (a^2 \cdot b^2) -$
    $a^2 + a^2 \cdot (2 \cdot a \cdot b) + a^2 \cdot (a^2 \cdot b^2) + a^2 + 2 \cdot a \cdot b + b^2$
  * $\equiv a^4 \cdot b^2 + 2 \cdot a^3 \cdot b^3 + a^2 \cdot b^4 + 2 \cdot a \cdot b^3 - a^2 \cdot b^4 + 2 \cdot a^3 \cdot b + a^4 \cdot b^2 + 2 \cdot a \cdot b$
  * $\equiv 2 \cdot a^4 \cdot b^2 + 2 \cdot a^3 \cdot (b^3 + b) + 2 \cdot a \cdot (b^3 + b)$
    $\rangle$

$$
2.1.\ 	?_0
$$

$\leftrightarrow\blacksquare$

This did not simplify out to 0 as expected, so we likely hit an issue somewhere, either in the derivation or in our assumption about `_*_`.

We should have first quickly tested it computationally:

````python
def e(h: float) -> (float, float):
	return ((1.0 - (h**2)) / (1.0 + h**2), (2.0 * h) / (1.0 + h**2))
	
def h3(h1: float, h2: float) -> float:
	return (h1 + h2) / (1 - h1 * h2)
	
def pair_mult_1(p1: (float, float), p2: (float, float)) -> (float, float):
	return (p1[0] * p2[0], p1[1] * p2[1])

def pair_equiv_approx(p1: (float, float), p2: (float, float), error: float) -> bool:
	if abs(p1[0] - p2[0]) > error:
		return False
	if abs(p1[1] - p2[1]) > error:
		return False
	return True

def test1():
	"""
	We simulate `∀ (h₁ h₂ : ℚ) → P h₁ h₂` by using a subregion
	and fixed resolution for `ℚ × ℚ`.
	We are testing `∀ (h₁ h₂ : ℚ) → (e h₁) * (e h₂) ≡ (e h₃)` with *
	defined as `pair_mult_1`.
	"""
	eps: float = 1E-2
	error = 1E-7
	period: int = 1E2
	max_samples = int((1.0 / eps) * period)
	
	for i in range(0, max_samples):
		for j in range (0, max_samples):
			h1 = (i * eps)
			h2 = (j * eps)
			
			lhs = pair_mult_1(e(h1), e(h2))
			rhs = e(h3(h1, h2))
			
			if not pair_equiv_approx(lhs, rhs, error):
				print(f"Failed the test at i={i}, j={j}, ", end='')
				print(f"h1={h1}, h2={h2}, lhs={lhs}, rhs={rhs}.")
				return
	print("OK")
````

We fail this test, so this definition of `_*_` cannot be right:

````
Failed the test at i=0, j=1, h1=0.0, h2=0.01, lhs=(0.9998000199980002, 0.0), rhs=(0.9998000199980002, 0.019998000199980003).
````

Let's experiment with other definitions of `_*_` that can get this right.

````python
from typing import Callable, Tuple

def e(h: float) -> (float, float):
	return ((1.0 - (h**2)) / (1.0 + h**2), (2.0 * h) / (1.0 + h**2))
	
def h3(h1: float, h2: float) -> float:
	return (h1 + h2) / (1 - h1 * h2)
	
def pair_mult_1(p1: (float, float), p2: (float, float)) -> (float, float):
	return (p1[0] * p2[0], p1[1] * p2[1])

def pair_equiv_approx(p1: (float, float), p2: (float, float), error: float) -> bool:
	if abs(p1[0] - p2[0]) > error:
		return False
	if abs(p1[1] - p2[1]) > error:
		return False
	return True

FloatPair = Tuple[float, float]

def test(mult_op : Callable[[FloatPair, FloatPair], FloatPair]):
	"""
	We simulate `∀ (h₁ h₂ : ℚ) → P h₁ h₂` by using a subregion
	and fixed resolution for `ℚ × ℚ`.
	We are testing `∀ (h₁ h₂ : ℚ) → (e h₁) * (e h₂) ≡ (e h₃)` with *
	defined by `mult_op`.
	"""
	eps: float = 1E-2
	error = 1E-7
	period: int = 1E2
	max_samples = int((1.0 / eps) * period)
	
	for i in range(0, max_samples):
		for j in range (0, max_samples):
			h1 = (i * eps)
			h2 = (j * eps)
			
			lhs = mult_op(e(h1), e(h2))
			rhs = e(h3(h1, h2))
			
			if not pair_equiv_approx(lhs, rhs, error):
				print(f"Failed the test at i={i}, j={j}, ", end='')
				print(f"h1={h1}, h2={h2}, lhs={lhs}, rhs={rhs}.")
				return
	print("OK")
````

[Failed attempts](../entries/001%20Failed%20attempts%20while%20Defining%20Multiplication%20to%20satisfy%20Alg%20Top%20S1%20Mult%201.md).

# 4 Template

Equational reasoning:

$\equiv \langle$ ... $\rangle$
$\blacksquare$

Logical equivalence chains:
$\leftrightarrow \langle$ ... $\rangle$
$
