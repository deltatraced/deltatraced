# 1 Journal

2025-09-01 Wk 36 Mon - 18:30

The docs are at [npmjs](https://docs.npmjs.com/).

This [medium post](https://medium.com/@jayjethava101/node-js-project-structure-best-practices-and-example-for-clean-code-3e1f5530fd3b) offers an idea for the project structure.

They use a module, service, controller, and routes structure... They have an [explanation post](https://medium.com/google-developer-indonesia/understanding-the-concept-of-module-controller-and-service-in-nestjs-c6c91a291e18).

So the controller is for HTTP-based functionality, the module imports everything for a feature, and the service is the business logic part of things. Also the controller seems to define the API in this case, the parts where the database is accessed for example is done in the service. This seems to be a NextJS concept.

[nestjs](https://docs.nestjs.com/) also has an explanation of [controllers](https://docs.nestjs.com/controllers). There is a routing mechanism that picks which controller to use.

Their [providers](https://docs.nestjs.com/providers) concept utilizes [dependency injection](https://www.geeksforgeeks.org/system-design/dependency-injectiondi-design-pattern/) as they mention [here](https://docs.nestjs.com/providers#dependency-injection). They also mention that following [SOLID](https://en.wikipedia.org/wiki/SOLID) principles and [Inversion of control](https://en.wikipedia.org/wiki/Inversion_of_control) principle.

They include services as providers here, and they have similar responsibilities such as database use after being invoked by a controller.

### 1.1.1 Pend
