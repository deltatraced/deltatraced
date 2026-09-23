---
parent: '[[001 Proc Math Problems]]'
spawned_by: '[[001 Proc Math Problems]]'
context_type: entry
---

Parent: [001 Proc Math Problems](../001%20Proc%20Math%20Problems.md)

Spawned by: [001 Proc Math Problems](../001%20Proc%20Math%20Problems.md)

Spawned in: [^spawn-entry-735cc2](../001%20Proc%20Math%20Problems.md#spawn-entry-735cc2)

# 1 Journal

2026-04-24 Wk 17 Fri - 10:59 +03:00

While solving `000 NW Alg Top S1 Mult 1 (ea2c1a)` ,

$\text{Prop}_1 : \forall (h_1\ h_2 : \mathbb{Q}) \to (e\ h_1) * (e\ h_2) \equiv (e\ h_3)$
$\text{Prop}_1\ h_1\ h_2 =$
(...)

$$1.3.\ 
\frac
	{\color{green}(1 - h_1^2) \cdot (1 - h_2^2)}
	{\color{green}(1 + h_1^2) \cdot (1 + h_2^2)} , 
\frac
	{\color{orange}(2 \cdot h_1) \cdot (2 \cdot h_2)}
	{\color{green}(1 + h_1^2) \cdot (1 + h_2^2)}$$
$\equiv \langle$ ap $\color{green}\forall (a\ b\ c\ d : \mathbb{Q}) \to	(a \pm b) \cdot (c \pm d) \equiv ac \pm ad \pm bc + bd$
$\color{orange}\forall (a\ b\ c : \mathbb{Q}) \to (a \cdot b) \cdot (a \cdot c) \equiv a^2 \cdot b \cdot c$ $\rangle$
$$1.4.\ 
\frac
	{1 - h_2^2 - h_1^2 + h_1^2 \cdot h_2^2}
	{1 + h_2^2 + h_2^2 + h_1^2 \cdot h_2^2} , 
\frac
	{(2^2 \cdot h_1 \cdot h_2)}
	{1 + h_2^2 + h_1^2 + h_1^2 \cdot h_2^2}
$$

I made an error here, which propagated forward during later calculations. It should be $1 + h_2^2 + h_1^2$ in the denomenator. I havedone it correctly in the right but not the left. I noticed this while trying to abstract away the denomenator in this complicated expression, It *should* be shared by each component:

$$\leftrightarrow\text{-Prop}_1 : 
	\forall (h_1\ h_2 : \mathbb{Q}) \to 
		(e\ h_1) * (e\ h_2) \equiv (e\ h_3)\ 
	\leftrightarrow\ ?_0$$

$\leftrightarrow\text{-Prop}_1\ h_1\ h_2\ =$
(...)
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
	\color{white}
	(1 + h_2^2 + h_2^2 + h_1^2 \cdot h_2^2) \cdot 
	(1 + (\frac{h_1 + h_2}{1 - h_1 \cdot h_2})^2)
	\color{white}
}
$$
$$
,
\\frac {
((2^2 \cdot h_1 \cdot h_2)) \cdot
(1 + (\frac{h_1 + h_2}{1 - h_1 \cdot h_2})^2) -
(1 + h_2^2 + h_1^2 + h_1^2 \cdot h_2^2) \cdot
(2 \cdot  (\frac{h_1 + h_2}{1 - h_1 \cdot h_2})
} {
(1 + h_2^2 + h_1^2 + h_1^2 \cdot h_2^2) \cdot
(1 + (\frac{h_1 + h_2}{1 - h_1 \cdot h_2})^2)
}

\\equiv 0 , 0
$$
