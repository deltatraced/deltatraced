---
context_type: concept
---

Parent: [lan/2026/topic/concept/000 Atomic/wiki/003 Wiki Clusterline Concepts/003 Wiki Clusterline Concepts](../003%20Wiki%20Clusterline%20Concepts.md)

Spawned by: [lan/2026/topic/concept/000 Atomic/wikiproc/003 Wiki Proc Clusterline Concepts/003 Wiki Proc Clusterline Concepts](../../../wikiproc/003%20Wiki%20Proc%20Clusterline%20Concepts/003%20Wiki%20Proc%20Clusterline%20Concepts.md)

Spawned in: [^spawn-cncpt-9fa184](../../../wikiproc/003%20Wiki%20Proc%20Clusterline%20Concepts/003%20Wiki%20Proc%20Clusterline%20Concepts.md#spawn-cncpt-9fa184)

---

A context may have many independent components or subcontexts that compose it.

Multiple subproblems may need to be solved to advance on a math problem. Multiple components on a motherboard need to be designed and validated individually. Parallel reduction is about identifying independent aspects of a composite context that assemble together to describe it via their composition structure.

[002 Serial Reduction of Context](002%20Serial%20Reduction%20of%20Context.md) suggested that we do not need to capture a full step-by-step reduction of a context to completion, but we can leave the last step as "... the remaining".

The same applies with parallel reduction, so long as we can spatially divide the context into some independent parts, one of those can be made to denote "the rest". Although depending on the problem, we may find there is strong logical reasons for how to break down a context in parallel exhaustively, such as a mathematical problem already worked out at the high level, but needs the subcomponents filled out.

This is a strong aspect of parallel reduction of context, which is to operate at a high level of abstraction and assume problems that need to be solved to have been solved, and then finish the given context with that assumption. The result of this is a parallel separation of the current abstraction layer and that of the components.

Parallel reduction of context can work similarly to type hole-based programming. The higher context constraints the lower context by usage, even before that lower context is defined. We can use aspiring notes to achieve this, so that by the time we write the lower context, we already have constraints to go off of based on its dependencies.

# Analogy

Think of serial reduction of context as producing a chain of subcontexts. Each sets a stage for the next, and each must be completed before the next.

Think of parallel reduction of context as subdividing the context geometrically. If the context is a bag, then it may have multiple compartments, and multiple objects simultaneously inside it. They do not set a stage for one another, they all come together to provide a description of the content of the bag. Similarly, the bag my have another bag inside it, which recursively has more items or bags inside it, so we get tree-like segmentation of a shape with context.

Serial reduction asks "what's next", producing a series of next considerations.

Parallel reduction answers "what parts make this?", producing tree-like subset divisions of a context.

# Differentiation

So how do we differentiate whether a new context is a serial reduction or a parallel reduction?

One way is to use different styles of bullet points in overview notes ([003 Overview notes in a cluster clarify how that cluster ought be navigated](../entry/003%20Overview%20notes%20in%20a%20cluster%20clarify%20how%20that%20cluster%20ought%20be%20navigated.md)):

* `MyRobot`
  * `Code`
    * `Physics`
    * `MessageBroker`
  * `Electronics`
    * `PCB`
    * `Microcontroller`
      1. Pick a powerful microcontroller for prototyping purposes
      1. Await `[[Research.1]]` for better understanding of the physical environment
         1. get sensor parts that meet the minimum requirements
      * `Communication`
        1. Start prototyping communications using `I2C`
        1. Await `[[Research.2]]` to specify expected networking protocols
    * `Power`
      1. Pick the parts that minimize power usage within our spec
      1. Create stress tests and measure current consumption of our parts at high load
  * `Research`
    1. Research the problem domain; are we able to navigate the environment safely?
    1. Derive a specification for the robot that guides the remaining divisions.

Note that linking allows us to "go into the page" so to speak, so this does not need to be written in one page. The details of a bulletpoint may be found once we click on it. `[[Electronics]]` might be its own page, with its own parts.

Put these plans under a consistent section, like `# Tasks`, or `# Plan` to foster tooling analysis and visualization.

Note also how we can allow a parallel division to both have further subdivisions and sequential planning at the same time.

Note also how parallel reduction allowed us to be more expressive about the stages of our plan, `Microcontroller` stages may not progress until `Research` progresses to a certain point using `Await`.
