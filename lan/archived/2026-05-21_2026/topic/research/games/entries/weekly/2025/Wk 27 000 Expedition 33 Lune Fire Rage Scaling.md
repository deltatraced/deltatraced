---
resolved: 'true'
---

\#lan #entry #gaming #experiment #research

2025-07-02 Wk 27 Wed - 21:42

# 1 Objective

![Pasted image 20250702232815.png](../../../../../../../../../attachments/Pasted%20image%2020250702232815.png)

* [x] To see whether this `Fire Rage` attack can scale beyond 9999 damage in this fight
* It cannot.

I am not able to find this information in the wikis.

# 2 Context

![Pasted image 20250702214157.png](../../../../../../../../../attachments/Pasted%20image%2020250702214157.png)

![Pasted image 20250702214342.png](../../../../../../../../../attachments/Pasted%20image%2020250702214342.png)

We are fighting golgra.

# 3 Journal

## 3.1 Recording damage every time we act against golgra

## 3.2 Measuring Scaling of Fire Rage Damage

2025-07-02 Wk 27 Wed - 21:44

The experiment is to screencast each run and record the trial number, and the damage due to Fire Rage.

Effects Applied Flags:
\[P\]erfect
\[C\]ritical (1.5x bonus, orange)

Time will be recorded after the attack lands.

|Trial (#)|Turn (#)|Damage|Effects Applied|Time  (MM:SS)|
|---------|--------|------|---------------|-------------|
|0|1|690||NA|
|0|2|2588|P|NA|
|1|1|690|P|NA|
|1|2|2588|P|NA|
|2|1|690|P|00:06|
|2|2|1725|P|00:51|
|2|3|3881|P C|01:37|
|2|4|3380||02:24|
|2|5|5019|P|03:13|
|2|6|9841|C|04:00|
|2|7|8273|P|04:48|
|2|8|9266||05:33|
|2|9|9999|P|06:21|
|2|10|9999|P|07:06|
|2|11|9999|P|07:54|
|2|12|9999|P|08:42|

## 3.3 Measuring Parry Damage

Parry damage characteristics

![Pasted image 20250702220521.png](../../../../../../../../../attachments/Pasted%20image%2020250702220521.png)

![Pasted image 20250702224021.png](../../../../../../../../../attachments/Pasted%20image%2020250702224021.png)

|Trial (#)|Turn (#)|Parry1|Parry2|Parry3|Parry4|Parry5|
|---------|--------|------|------|------|------|------|
|1|1|3234|2156|2156|2156|3234|
|2|1|3234|3234|2156|2156|3234|
|2|2|2156|2156|2156|2156|2156|
|2|3|3234|2156|3234|2156|2156|
|2|4|2156|2156|2156|2156|2156|
|2|5|2156|3234|3234|3234|2156|

## 3.4 Parry Damage analysis

2025-07-02 Wk 27 Wed - 22:08

3234 glows orange for parries. 2156 is white.

Using [answer for ratio formatting](https://unix.stackexchange.com/a/581608),

````sh
function frac() {
	input_nums="$1"
	awk -v prec=0.00005 -v max=20000 '
	   function fract(n, k, kr, kf, d){
	       for(k=1;k<max;k++){
	           kf=n*k; kr=int(kf+.5); d=kr-kf; if(d<0)d=-d;
	           if(d<prec){return kr"/"k}
	       }
	       return n" ??"
	   }
	   BEGIN{for(i=1;i<ARGC;i++)print fract(ARGV[i])}
	' "$input_nums"
}

frac "3.1415926535897932384"
````

````sh
frac $(python3 -c "print(2156/3234)")
````

yields $\frac{2}{3}$.  So orange yields an increase of 50%

````sh
frac $(python3 -c "print((3234-2156)/2156)")
````

and is equal to $1 \frac{1}{2} \times$ white.

## 3.5 Maelle and Sword Ballet

![Pasted image 20250702232746.png](../../../../../../../../../attachments/Pasted%20image%2020250702232746.png)

So far this is my best chance against golgra.
