---
context_type: task
status: todo
---

Parent: [[001 Wiki Mathematics]]

Spawned by: [[001 Wiki Proc Mathematics]]

Spawned in: [[001 Wiki Proc Mathematics#^spawn-task-4186cd|^spawn-task-4186cd]]

# Problem

We want to define a function `e` that takes a unique rational parameter `h` and sends it to a 2D vector representing a unique point on the circle, with the idea being that we then have a nearly invertible mapping from the rational line to rational points on the circle. It is nearly invertible because the map `e` does not need to send anything to the point (-1, 0), which is motivated by the fact that a slope-sweep based solution would blow up to slope $\pm \infty$ at that point, assuming that it is designated as the source point in the problem.

# Proof

See the [[#3 Explanation|Explanation]] below.

```haskell
record Vect2 : Type where
  constructor vect2
  field
    x : ℚ
    y : ℚ

-- Problem: Define `e` so that it continuously sends `h` to a point on the circle.
	e : (h : ℚ) → Vect2
	e h .x = (1 - h²) / (1 + h²)
 -- e h .x = E₁ᵣ i1
	e h .y = 2h / (1 + h²)
 -- e h .y = E₂ᵣ i1

-- For proof that the above map `e` really does send an `h` to a point on the circle
module _
	(x₁ y₁ h : ℚ)
	(A  : vect2 -1 0)
	(Bₕ : vect2  0 h)
	(Bₓ : vect2 x₁ 0)
	(B  : vect2 x₁ y₁)
	
	on-unit-circle : (x y ∈ ℚ) → Σ[ x y ∈ ℚ ] (x² + y² ≡ 1)

	(cir-A : on-unit-circle (A .x) (A .y))
	(cir-B : on-unit-circle (B .x) (B .y))

	-- for now, let's use some opaque notion of colinearity to encode the observation of points
	-- A Bₕ B being on the same line
	colinear : (a b c : Vect2) → Type
	
	L₁ : colinear A Bₕ B
	
	slope : Vect2 → Vect2 → ℚ
	slope a b = (b .y - a .y) / (b .x - a .x)
	
	-- opaque definition due to colinear being opaque. Should agree with intuition for now.
	same-slope-1 : (a b c : Vect2) → colinear a b c → slope a b ≡ slope a c
	
	L₁-same-slope : slope A Bₕ ≡ slope A B
	L₁-same-slope = same-slope-1 A Bₕ B L₁
	
	where
		E₀ : slope A Bₕ ≡ slope A B ↔ y₁ ≡ h (x₁ + 1)
		E₀ =
			slope A Bₕ ≡ slope A B               ↔⟨ _ {- unfold defn. slope -} ⟩ 
			
			(Bₕ .y) - (A .y)   (B .y) - (A .y)
			–––––––––––––––– ≡ –––––––––––––––
			(Bₕ .x) - (A .x)   (B .x) - (A .x)   ↔⟨ _ {- unfold defn. projections  -} ⟩
			
	     	h       - 0        y₁     - 0
			–––––––––––––––– ≡ –––––––––––––––
			0       - (-1)     x₁     - (-1)     ↔⟨ _ ⟩
			
	     	      y₁ 
			h ≡ –––––––
			    x₁ + 1                           ↔⟨ _ ⟩
				  
			y₁ ≡ h (x₁ + 1)                      ↔∎
			
		E₀ᵣ : y₁ ≡ h (x₁ + 1)  
		E₀ᵣ = (E₀ .fst) L₁-same-slope
		
			                      1 - h²
		E₁ : x₁² + y₁² ≡ 1 ↔ x₁ ≡ ––––––
							      1 + h²
		E₁ = 
			x₁² + y₁² ≡ 1                        ↔⟨ ap-≡ₗ (ap (λ a → x₁² + a²) E₀ᵣ) ⟩
			x₁² + (h (x₁ + 1))² ≡ 1              ↔⟨ ap-≡ₗ (ap (λ a → x₁² + a²) (use (∀ (a b : ℚ) → 
			                                        (a · (b + 1))² ≡ a² · (b + 1)²) ))⟩
			x₁² + (h² (x₁ + 1)²) ≡ 1             ↔⟨ _ ⟩
			h² (x₁ + 1)² ≡ 1 - x₁²               ↔⟨ _ ⟩
			
                  1 - x₁²
			h² ≡ ––––––––– 
                 (x₁ + 1)²                       ↔⟨ ap-≡ᵣ (ap (λ a → a / (x₁ + 1)²) (use ∀ (a : ℚ) → 
                                                    (1 - a²) ≡ (1 - a)(1 + a)) ))⟩

                 (1 - x₁)(1 + x₁)
			h² ≡ –––––––––––––––– 
                    (x₁ + 1)²                    ↔⟨ ap-≡ᵣ (use (∀ (a : ℚ) → 
                                                    ((1 - a)(1 + a)) / (a + 1)² ≡ (1 - a) / (1 + a))) ⟩

                 1 - x₁
			h² ≡ –––––– 
                 1 + x₁                          ↔⟨ _ ⟩
				 
			h² (1 + x₁) ≡ 1 - x₁                 ↔⟨ _ ⟩
			h² (1 + x₁) + x₁ ≡ 1                 ↔⟨ _ ⟩

			h² + h²x₁ + x₁ ≡ 1                   ↔⟨ _ ⟩
			x₁ (1 + h²) + h² ≡ 1                 ↔⟨ _ ⟩
			x₁ (1 + h²) ≡ 1 - h²                 ↔⟨ _ ⟩
			
			     1 - h²
			x₁ ≡ –––––– 
			     1 + h²                          ↔∎
				 
				   1 - h²
		E₁ᵣ : x₁ ≡ ––––––
				   1 + h²
		E₁ᵣ = (E₁ .fst) (cir-B .snd)
		
									  2h
		E₂ : y₁ ≡ h (x₁ + 1) ↔ y₁ ≡ ––––––
								    1 + h²
		E₂ =
			y₁ ≡ h (x₁ + 1)                      ↔⟨ ap-≡ᵣ (ap (λ a → h (a + 1)) E₁ᵣ) ⟩

				    1 - h²
			y₁ ≡ h (–––––– + 1)                  
				    1 + h²                       ↔⟨ _ ⟩
					
				    1 - h²   1 + h²
			y₁ ≡ h (–––––– + ––––––)
				    1 + h²   1 + h²              ↔⟨ _ ⟩
					
				     2
			y₁ ≡ h ––––––
				   1 + h²                        ↔⟨ _ ⟩
					
				   2h
			y₁ ≡ ––––––             
			     1 + h²                          ↔∎

					 2h
		E₂ᵣ : y₁ ≡ ––––––
			       1 + h²
		E₂ᵣ = (E₂ .fst) E₀ᵣ
		

```

# Explanation

Starting from a point `A` on the unit circle, let it be `(-1, 0)`, we want to cast a ray to any other point on the circle, call it `B`. When we do this, we always cross the y-axis at a point, whose y-component distance from the origin we will label `h`. There is a unique point on the circle for every unique point we intersect the y-axis. Because of this, we are interested to find a map from `h` to the coordinates of the point `B`. Let's graph what we have so far:

![[Pasted image 20260430150504.png]]


We have further labeled `x₁` and `y₁` as the x-component and y-component values of point `B`. We also labeled point `Bₕ` for the point intersecting the y-axis in the line `AB` and point `Bₓ` for the x-projection of `B`.

Now let's solve a map from `h` to the coordinates `(x₁, y₁)`:

```haskell
record Vect2 : Type where
  constructor vect2
  field
    x : ℚ
    y : ℚ
	

module _
	(x₁ y₁ h : ℚ)
	(A  : vect2 -1 0)
	(Bₕ : vect2  0 h)
	(Bₓ : vect2 x₁ 0)
	(B  : vect2 x₁ y₁)
	
	on-unit-circle : (x y ∈ ℚ) → Σ[ x y ∈ ℚ ] (x² + y² ≡ 1)
	
	(cir-A : on-unit-circle (A .x) (A .y))
	(cir-B : on-unit-circle (B .x) (B .y))

	-- for now, let's use some opaque notion of colinearity to encode the observation of points
	-- A Bₕ B being on the same line
	colinear : (a b c : Vect2) → Type
	
	L₁ : colinear A Bₕ B
	
	where
		e : (h : ℚ) → Vect2
		e h .x = {! 0!}
		e h .y = {! 1!}
```

In order to obtain an expression `y₁ ≡ ? x₁ h ` for some map `? : ℚ → ℚ → ℚ`, consider that the line segments `ABₕ` and `AB` lie on the same line, and thus share the same slope:

```haskell
module _
	slope : Vect2 → Vect2 → ℚ
	slope a b = (b .y - a .y) / (b .x - a .x)
	
	-- opaque definition due to colinear being opaque. Should agree with intuition for now.
	same-slope-1 : (a b c : Vect2) → colinear a b c → slope a b ≡ slope a c
	
	L₁-same-slope : slope A Bₕ ≡ slope A B
	L₁-same-slope = same-slope-1 A Bₕ B L₁
	
	where
		{- ... -}
```

Let's elaborate the equality of the slopes of the lines `ABₕ` and `AB`:

```haskell
module _
	{- ... -}
	where
		E₀ : slope A Bₕ ≡ slope A B ↔ y₁ ≡ {! 2!}
		E₀ =
			slope A Bₕ ≡ slope A B               ↔⟨ _ {- unfold defn. slope -} ⟩ 
			
			(Bₕ .y) - (A .y)   (B .y) - (A .y)
			–––––––––––––––– ≡ –––––––––––––––
			(Bₕ .x) - (A .x)   (B .x) - (A .x)   ↔⟨ _ {- unfold defn. projections  -} ⟩
			
	     	h       - 0        y₁     - 0
			–––––––––––––––– ≡ –––––––––––––––
			0       - (-1)     x₁     - (-1)     ↔⟨ _ ⟩
			
	     	      y₁ 
			h ≡ –––––––
			    x₁ + 1                           ↔⟨ _ ⟩
				  
			y₁ ≡ h (x₁ + 1)                      ↔∎
```

Now we can fill hole `{! 2!}` with `h (x₁ + 1)`. This gives us an expression of `y₁` in terms of `x₁` and `h`.

```haskell
module _
	{- ... -}
	where
		E₀ : slope A Bₕ ≡ slope A B ↔ y₁ ≡ h (x₁ + 1)

		E₀ᵣ : y₁ ≡ h (x₁ + 1)  
		E₀ᵣ = (E₀ .fst) L₁-same-slope
```

So for `y₁ ≡ ? x₁ h`, the expression in `?` we were looking for wasextra point at infinity to account for this, or some other solution

```haskell
_ : ℚ → ℚ → ℚ
_ x₁ h = h (x₁ + 1)
```

Next we want to express `x₁` in terms of `h`. To be able to do this, we will make use of our new expression for `y₁` above and the fact that the point `B` lies on the circle.

```haskell
module _
	{- ... -}
	where
		E₁ : x₁² + y₁² ≡ 1 ↔ x₁ ≡ {! 2!}
		E₁ = 
			x₁² + y₁² ≡ 1                        ↔⟨ ap-≡ₗ (ap (λ a → x₁² + a²) E₀ᵣ) ⟩
			x₁² + (h (x₁ + 1))² ≡ 1              ↔⟨ ap-≡ₗ (ap (λ a → x₁² + a²) (use 
			                                        (∀ (a b : ℚ) → (a · (b + 1))² ≡ a² · (b + 1)²) ))⟩
			x₁² + (h² (x₁ + 1)²) ≡ 1             ↔⟨ _ ⟩
			h² (x₁ + 1)² ≡ 1 - x₁²               ↔⟨ _ ⟩
			
                  1 - x₁²
			h² ≡ ––––––––– 
                 (x₁ + 1)²                       ↔⟨ ap-≡ᵣ (ap (λ a → a / (x₁ + 1)²) (use ∀ (a : ℚ) → 
                                                    (1 - a²) ≡ (1 - a)(1 + a)) ))⟩

                 (1 - x₁)(1 + x₁)
			h² ≡ –––––––––––––––– 
                    (x₁ + 1)²                    ↔⟨ ap-≡ᵣ (use (∀ (a : ℚ) → 
                                                    ((1 - a)(1 + a)) / (a + 1)² ≡ (1 - a) / (1 + a))) ⟩

                 1 - x₁
			h² ≡ –––––– 
                 1 + x₁                          ↔⟨ _ ⟩
				 
			h² (1 + x₁) ≡ 1 - x₁                 ↔⟨ _ ⟩
			h² (1 + x₁) + x₁ ≡ 1                 ↔⟨ _ ⟩
			h² + h²x₁ + x₁ ≡ 1                   ↔⟨ _ ⟩
			x₁ (1 + h²) + h² ≡ 1                 ↔⟨ _ ⟩
			x₁ (1 + h²) ≡ 1 - h²                 ↔⟨ _ ⟩
			
			     1 - h²
			x₁ ≡ –––––– 
			     1 + h²                          ↔∎
```

Now we have an expression for `x₁` to fill in the hole `{! 2!}`:

```haskell
module _
	{- ... -}
	where
			                      1 - h²
		E₁ : x₁² + y₁² ≡ 1 ↔ x₁ ≡ ––––––
							      1 + h²

								  
				   1 - h²
		E₁ᵣ : x₁ ≡ ––––––
				   1 + h²
		E₁ᵣ = (E₁ .fst) (cir-B .snd)
```

Recall that we expressed `y₁` in terms of `x₁` and `h` in `E₀ᵣ`. Now that we have expressed `x₁` in terms of `h`, let's further elaborate `y₁` solely in terms of `h`:

```haskell
module _
	{- ... -}
	where
		E₂ : y₁ ≡ h (x₁ + 1) ↔ y₁ ≡ {! 2!} -- eliminate x₁ and simplify
		E₂ =
			y₁ ≡ h (x₁ + 1)                      ↔⟨ ap-≡ᵣ (ap (λ a → h (a + 1)) E₁ᵣ) ⟩

				    1 - h²
			y₁ ≡ h (–––––– + 1)                  
				    1 + h²                       ↔⟨ _ ⟩
					
				    1 - h²   1 + h²
			y₁ ≡ h (–––––– + ––––––)
				    1 + h²   1 + h²              ↔⟨ _ ⟩
					
				     2
			y₁ ≡ h ––––––
				   1 + h²                        ↔⟨ _ ⟩
					
				   2h
			y₁ ≡ ––––––             
			     1 + h²                          ↔∎
```

Now we have the expression for `y₁` in terms of `h` to put in hole `{! 2!}:

```haskell
module _
	{- ... -}
	where
									  2h
		E₂ : y₁ ≡ h (x₁ + 1) ↔ y₁ ≡ ––––––
								    1 + h²
									  
					 2h
		E₂ᵣ : y₁ ≡ ––––––
			       1 + h²
		E₂ᵣ = (E₂ .fst) E₀ᵣ
```

Now we can finish off the original problem. We wanted a map `e` that sends a value for `h` to a point on the circle. By sweeping `h`, we should get all rational points on the circle.

```haskell
module _
	{- ... -}
	where
		e : (h : ℚ) → Vect2
		e h .x = {! 0!}
		e h .y = {! 1!}
```

Based on our investigation, `E₁ᵣ` shows us how we must implement `e h .x` and `E₂ᵣ` shows us how we must implement `e h .y` so that this map guarantees sending `h`  to a unique point on the circle:

```haskell
module _
	{- ... -}
	where
		e : (h : ℚ) → Vect2
		e h .x = (1 - h²) / (1 + h²)
		e h .y = 2h / (1 + h²)
	 -- e h .x = E₁ᵣ i1
	 -- e h .y = E₂ᵣ i1
```

The full solution is put in the proof section.