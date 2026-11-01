\#math/approximationn

Below are notes I got when I worked on this

2025-08-05 Wk 32 Tue - 07:04

I'm trying to use sympy to do some symbolic math, particularly I wanna try to apply newton's methods of finding approximations to zeros in functions
or any y=k
so could solve y=x^2 at y=2, to get an approximation of sqrt(2)
then based on how many times I run newton's method, I should get closer approximations

<img src="https://images-ext-1.discordapp.net/external/HZalgLpqZtVvL61RXFQrEaoO53CA1dGRj-hIopaDbXk/https/upload.wikimedia.org/wikipedia/commons/5/5c/Metodo_de_Newton_anime.gif?width=495&height=495" />

you start at some random guess point x0, get the line tangent to the curve at f(x0), and where that line meets the x-axis is x1
then get the line tangent to the curve at f(x1) and where that line meets the x-axis is x2
and keep going
and you'll solve the function!
your answer will converge to a solution
newton didn't have computers but he was practical!
ran his algos himself
but just like that, I should be able to calculate sqrt myself! just by using his method to go backwards from x^2, finding an approximate solution for some given y
and it should generalize for other scary looking functions that we might not be able to inverse easily

I did it!

````
Iteration 0
rn 3/2

Iteration 1
rn 17/12

Iteration 2
rn 577/408

Iteration 3
rn 665857/470832

Iteration 4
rn 886731088897/627013566048
approximations of sqrt(2) after just 5 iterations
and they're all exact rationals!
````

the last one gives 1.4142135623730951

I implemented a sqrt function! using newton's method!

<img src="https://github.com/delta-domain-rnd/delta-trace/blob/main/attachments/Pasted%20image%2020250805070708.png?raw=true" />

<img src="https://github.com/delta-domain-rnd/delta-trace/blob/main/attachments/Pasted%20image%2020250805070713.png?raw=true" />

<img src="https://github.com/delta-domain-rnd/delta-trace/blob/main/attachments/Pasted%20image%2020250805070718.png?raw=true" />

<img src="https://github.com/delta-domain-rnd/delta-trace/blob/main/attachments/Pasted%20image%2020250805070726.png?raw=true" />

<img src="https://github.com/delta-domain-rnd/delta-trace/blob/main/attachments/Pasted%20image%2020250805070731.png?raw=true" />

that recursive series above can get us the sqrt(k) for any rational k

<img src="https://github.com/delta-domain-rnd/delta-trace/blob/main/attachments/Pasted%20image%2020250805070747.png?raw=true" />
