---
parent: '[[001 Math Problems]]'
spawned_by: '[[000 Spawn Logs for Math Problems]]'
context_type: entry
---

Parent: [001 Math Problems](../001%20Math%20Problems.md)

Spawned by: [000 Spawn Logs for Math Problems](000%20Spawn%20Logs%20for%20Math%20Problems.md)

Spawned in: [^spawn-entry-b236db](000%20Spawn%20Logs%20for%20Math%20Problems.md#spawn-entry-b236db)

For Problem [002 NW Alg Top S1 Mult 1 Failed Attempt 1](../tasks/002%20NW%20Alg%20Top%20S1%20Mult%201%20Failed%20Attempt%201.md)

---

# 1 Code

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

# 2 Iterations

````python
# Failed the test at i=0, j=0, h1=0.0, h2=0.0, lhs=(0.0, 0.0), rhs=(1.0, 0.0).
def pair_mult_2(p1: (float, float), p2: (float, float)) -> (float, float):
	u = p1[0]
	v = p1[1]
	u_ = p2[0]
	v_ = p2[1]
	return (u * v + u_ * v_, u * v - u_ * v_)
test(pair_mult_2)

# Failed the test at i=0, j=0, h1=0.0, h2=0.0, lhs=(0.0, 0.0), rhs=(1.0, 0.0).
def pair_mult_2(p1: (float, float), p2: (float, float)) -> (float, float):
	u = p1[0]
	v = p1[1]
	u_ = p2[0]
	v_ = p2[1]
	return (u * v_ + u_ * v, u * v_ - u_ * v)
test(pair_mult_2)

# Failed the test at i=0, j=0, h1=0.0, h2=0.0, lhs=(0.0, 0.0), rhs=(1.0, 0.0).
def pair_mult_2(p1: (float, float), p2: (float, float)) -> (float, float):
	u = p1[0]
	v = p1[1]
	u_ = p2[0]
	v_ = p2[1]
	return (u * v_ - u_ * v, u * v_ + u_ * v)
test(pair_mult_2)

````

````python
# Failed the test at i=0, j=0, h1=0.0, h2=0.0, lhs=(1.0, 1.0), rhs=(1.0, 0.0).
def pair_mult_2(p1: (float, float), p2: (float, float)) -> (float, float):
	u = p1[0]
	v = p1[1]
	u_ = p2[0]
	v_ = p2[1]
	return (u * u_ - v * v_, u * u_ + v * v_)
test(pair_mult_2)
````

This last test gives us 1.0 for lhs as expected, at least for the trivial case. Let's try to vary `(u * u_ - v * v_, ?)`

````python
# ZeroDivisionError: float division by zero
def pair_mult_2(p1: (float, float), p2: (float, float)) -> (float, float):
	u = p1[0]
	v = p1[1]
	u_ = p2[0]
	v_ = p2[1]
	return (u * u_ - v * v_, u * v_ + u_ * v)
test(pair_mult_2)

````

````python
# Failed the test at i=1, j=0, h1=0.01, h2=0.0, lhs=(0.9998000199980002, -0.019998000199980003), rhs=(0.9998000199980002, 0.019998000199980003).
def pair_mult_2(p1: (float, float), p2: (float, float)) -> (float, float):
	u = p1[0]
	v = p1[1]
	u_ = p2[0]
	v_ = p2[1]
	return (u * u_ - v * v_, u * v_ - u_ * v)
test(pair_mult_2)
````

This was able to cover it for all cases at `i=0`
