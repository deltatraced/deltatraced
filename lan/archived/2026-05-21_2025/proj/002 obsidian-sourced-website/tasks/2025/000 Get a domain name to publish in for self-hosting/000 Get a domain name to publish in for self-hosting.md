---
has_goal: '[[000 First prototype of a publicly hosted obsidian-sourced website]]'
status: done
---

# 1 Objective

I need to have a domain name that I can configure for a self-hosted website to have a nice URL.

* [x] Purchased domain name
  Purchased www.deltatraced.com!

* [x] Hosted a website using it
  This is done! www.deltatraced.com  can be deployed via [wasmer](https://wasmer.io/)!

This task is part of the goal [000 First prototype of a publicly hosted obsidian-sourced website](../../../archived/goals/2025/000%20First%20prototype%20of%20a%20publicly%20hosted%20obsidian-sourced%20website.md).

See also:

* [000 On Search Engine Optimization](../../../entries/2025/000%20On%20Search%20Engine%20Optimization.md)
* [001 Searching for services with free website hosting](../../../entries/2025/001%20Searching%20for%20services%20with%20free%20website%20hosting/001%20Searching%20for%20services%20with%20free%20website%20hosting.md)
* [002 Learning about Astro and website deployment](../../../entries/2025/002%20Learning%20about%20Astro%20and%20website%20deployment.md)

# 2 Journal

2025-08-12 Wk 33 Tue - 21:18

To deploy changes to wasmer based on changes the first time,

````sh
curl https://get.wasmer.io -sSfL | sh
npm install
npm run build
wasmer deploy
````

This CLI tool immediately authenticates you via a web browser link if you're logged in already which is very cool.

From [wasmer docs cli](https://docs.wasmer.io/edge/cli),

Use this to delete a deployed app:

````sh
wasmer app delete
````

But then you'd have to create a new app:

````sh
wasmer app create
````

But we have a website hosted on our own cool domain name now! Objective complete!

www.deltatraced.com

![Pasted image 20250813071716.png](../../../../../../../../attachments/Pasted%20image%2020250813071716.png)
