# 1 Journal

[ai faq softmax](http://www.faqs.org/faqs/ai-faq/neural-nets/part2/section-12.html) [^3](001%20On%20Softmax.md#3) shows that $\text{softmax}$ is defined as follows

### 1.1.1 Definition: Softmax

(1)

Let $\vec{X}$ be a vector of real-valued inputs of size `n` where `n` denotes the number of elements in $\vec{X}$.

So $\vec{X} \equiv \lbrace x_1, \cdots, x_n \rbrace$ .

(2)

Let $\vec{Y}$ be the output vector computed through $\text{softmax}$.

(`Defn`)

Then,

$$
y_i \equiv \frac{e^{x_i}}{\sum^n_{j=1}{e^{x_j}}}
$$

^softmax-eq

### 1.1.2 Constraints

As [ai faq softmax](http://www.faqs.org/faqs/ai-faq/neural-nets/part2/section-12.html) [^3](001%20On%20Softmax.md#3) explains,

$\text{softmax}$ allows the following properties to be satisfied for $\vec{Y}$ :

1. The range of $\vec{Y}$ is $[0, 1]$.  ^softmax-constr1
1. $\text{sum}(\vec{Y}) = 1$ ^softmax-constr2

#### 1.1.2.1 Showing property 1 holds

(`Prbl 1`)

Show that the range of $\vec{Y} = \text{softmax}(\vec{X})$  must be $[0, 1]$ .

(`Prms 1.1`)

The first property [^softmax-constr1](001%20On%20Softmax.md#softmax-constr1) will hold if and only if

$$
e^{x_i} \le \sum_{j=1}^n{e^{x_j}} \ \text{for any i}
$$

(`Prms 1.2`)

For any positive real number $a$, and any negative, zero, or positive real number $b$,

$a^b$ is always a positive number.

* When  $b \ge 1$, then $a^b \ge a$.
* When $b \in (0, 1)$, then $a^b \in (0, a)$.
* When $b = 0$, then $a^b = 1$.

(`Prs 1.3`)

Given (`Prms 1.2`) where $a=e$ and $b=x_i$ for any i,

we expect that $e^{x_j}$ in $\sum_{j=1}^n{e^{x_j}}$ to always be positive, and to also include $e^{x_i}$ in the sum, so it can only be equal to it or greater.

(`Cncl 1`)

Given (`Prms 1.3`), (`Prms 1.1`) must be satisfied.

#### 1.1.2.2 Showing property 2 holds

(`Prbl 2`)

For values $\vec{Y} \equiv \lbrace y_1, \cdots, y_n \rbrace$ computed via $\vec{Y} = \text{softmax}(\vec{X})$, we want to show that $\text{sum}(\vec{Y}) = 1$

(`Prms 2.1`)

The sum $\text{sum}(\vec{Y})$ is given by

$$
\begin{aligned}
&\text{sum}(\vec{Y}) \\
&= \sum_{i=1}^n {\frac{e^{x_i}}{\sum^n_{j=1}{e^{x_j}}}} \\
&= \frac{1}{\sum^n_{j=1}{e^{x_j}}}\sum_{i=1}^n {e^{x_i}} \\
&= \frac{\sum_{i=1}^n {e^{x_i}} }{\sum^n_{j=1}{e^{x_j}}} \\
&= 1
\end{aligned}
$$

(`Cncl 2`)

(`Prms 2.1`) shows through definition application of $\text{sum}$ and $\text{softmax}$  that the result must be 1.

#### 1.1.2.3 Corrections

2025-08-11 Wk 33 Mon - 04:34

Before I have written

 > 
 > (`Prms 2.1`)
 > $\sum^n_{j=1}{e^{x_j}}$ can be interpreted as a weighted average of elements with weighting $e^{x_j}$.

But really it's the total sum. The average (mean) of an n-sized set $A \equiv \lbrace a_1, \cdots, a_n \rbrace$ is $\frac{\sum_{i=1}^n{a_i}}{n}$.

I likely made this mistake because I was interpreting $\text{softmax}$  to have a proportion of part to whole (one factor to the total sum).

But either way, none of these premises are needed to prove the property. So (`Prms 2.1`) and (`Prms 2.2`) are removed, and (`Prms 2.3`) is renamed to (`Prms 2.1`).

 > 
 > (`Prms 2.1`)
 > $\sum^n_{j=1}{e^{x_j}}$ can be interpreted as a weighted average of elements with weighting $e^{x_j}$.
 > (`Prms 2.2`)
 > In

[^softmax-eq](001%20On%20Softmax.md#softmax-eq)

 > 
 > We can see that $e^{x_i}$ can  be interpreted as one weight from the elements that are taken a weighted average of in $\sum^n_{j=1}{e^{x_j}}$.
