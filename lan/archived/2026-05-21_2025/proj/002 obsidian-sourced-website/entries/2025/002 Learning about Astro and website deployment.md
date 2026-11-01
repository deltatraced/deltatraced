---
status: todo
---

# 1 Objective

In [000 Get a domain name to publish in for self-hosting](../../tasks/2025/000%20Get%20a%20domain%20name%20to%20publish%20in%20for%20self-hosting/000%20Get%20a%20domain%20name%20to%20publish%20in%20for%20self-hosting.md) we setup an [astro website](https://docs.astro.build/en/getting-started/) starter over [wasmer](https://wasmer.io).

Here we learn more about those different tools!

# 2 Journal

# 3 Tasks

# 4 Issues

# 5 HowTos

# 6 Investigations

## 6.1 Inspecting astro deployment

* [ ] 

2025-08-20 Wk 34 Wed - 00:53

From [000 Get a domain name to publish in for self-hosting](../../tasks/2025/000%20Get%20a%20domain%20name%20to%20publish%20in%20for%20self-hosting/000%20Get%20a%20domain%20name%20to%20publish%20in%20for%20self-hosting.md),

2025-08-26 Wk 35 Tue - 20:37

Looking at [wasmer templates](https://wasmer.io/templates). Their [static website template](https://wasmer.io/templates/static-website) is probably the simplest example there is. Their [tutorial (broken)](https://docs.wasmer.io/edge/tutorials/cdn) for it is a broken link.

Ok looking at the [static website repo](https://github.com/static-web-server/static-web-server) it seems not simple, it's using Rust and many things. Need just basic HTML + CSS + JS hosting.

Let's create a reference and see if we can deploy it. [001 Create a reference basic website and host it with wasmer](../../tasks/2025/001%20Create%20a%20reference%20basic%20website%20and%20host%20it%20with%20wasmer/001%20Create%20a%20reference%20basic%20website%20and%20host%20it%20with%20wasmer.md).

2025-08-26 Wk 35 Tue - 22:04

There's also a [simple python server on wasmer](https://wasmer.io/templates/python-http-server).

### 6.1.1 Pend

## 6.2 Brainstorming webdev things to look into

2025-08-26 Wk 35 Tue - 08:32

Need to pick a stack. Say, Next.js, [prisma](https://www.prisma.io/), ...

2025-08-26 Wk 35 Tue - 08:57

Can also get an idea from [frontend roadmap](https://roadmap.sh/frontend)

Interestingly it has React > Astro for server side rendering (SSR).

This one is the [backend roadmap](https://roadmap.sh/backend) and a [fullstack roadmap](https://roadmap.sh/full-stack).

2025-08-26 Wk 35 Tue - 09:13

For rust... [are we web yet?](https://www.arewewebyet.org/)

2025-08-26 Wk 35 Tue - 20:35

### 6.2.1 Pend

## 6.3 Looking into CSS Resources

2025-09-01 Wk 36 Mon - 18:09

This [geeksforgeeks post](https://www.geeksforgeeks.org/blogs/best-css-frameworks-for-frontend-developers/) has some recommendations. One of them is [gh jgthms/bulma](https://github.com/jgthms/bulma) which is open source and minimal without need to invoke javascript.

They also say!

 > 
 > Bulma is a **CSS** framework. As such, the sole output is a single CSS file: [bulma.css](https://github.com/jgthms/bulma/blob/main/css/bulma.css)

which is cool!

Their website design is also cool! [bulma.io](https://bulma.io/)

2025-09-01 Wk 36 Mon - 18:27

Since this just generates a css file, we still fairly fundamental as we can inspect this file!

## 6.4 Looking into the npm package manager

2025-09-01 Wk 36 Mon - 18:30

The docs are at [npmjs](https://docs.npmjs.com/).

This [medium post](https://medium.com/@jayjethava101/node-js-project-structure-best-practices-and-example-for-clean-code-3e1f5530fd3b) offers an idea for the project structure.

They use a module, service, controller, and routes structure... They have an [explanation post](https://medium.com/google-developer-indonesia/understanding-the-concept-of-module-controller-and-service-in-nestjs-c6c91a291e18).

So the controller is for HTTP-based functionality, the module imports everything for a feature, and the service is the business logic part of things. Also the controller seems to define the API in this case, the parts where the database is accessed for example is done in the service. This seems to be a NextJS concept.

[nestjs](https://docs.nestjs.com/) also has an explanation of [controllers](https://docs.nestjs.com/controllers). There is a routing mechanism that picks which controller to use.

Their [providers](https://docs.nestjs.com/providers) concept utilizes [dependency injection](https://www.geeksforgeeks.org/system-design/dependency-injectiondi-design-pattern/) as they mention [here](https://docs.nestjs.com/providers#dependency-injection). They also mention that following [SOLID](https://en.wikipedia.org/wiki/SOLID) principles and [Inversion of control](https://en.wikipedia.org/wiki/Inversion_of_control) principle.

They include services as providers here, and they have similar responsibilities such as database use after being invoked by a controller.

### 6.4.1 Pend

# 7 Ideas

# 8 Side Notes

## 8.1 Astro Resources

[gh astro-examples](https://github.com/MicroWebStacks/astro-examples).

# 9 External Links

# 10 References
