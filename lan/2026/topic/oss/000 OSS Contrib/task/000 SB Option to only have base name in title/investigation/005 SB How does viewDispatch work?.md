---
context_type: investigation
status: done
---

Parent: [000 SB Option to only have base name in title](../000%20SB%20Option%20to%20only%20have%20base%20name%20in%20title.md)

Spawned by: [000 SB Impl opt in tile as basename only via Std defined service](../task/000%20SB%20Impl%20opt%20in%20tile%20as%20basename%20only%20via%20Std%20defined%20service.md)

Spawned in: [^spawn-invst-b898d2](../task/000%20SB%20Impl%20opt%20in%20tile%20as%20basename%20only%20via%20Std%20defined%20service.md#spawn-invst-b898d2)

Overview: [001 Overview SB Option to only have base name in title](../entry/001%20Overview%20SB%20Option%20to%20only%20have%20base%20name%20in%20title.md)

# Journal

## Where is viewDispatch pointing?

2026-06-14 Wk 24 Sun - 19:09 +03:00

````ts
// in client/editor_ui.tsx > MainUI
viewDispatch: (action: Action) => void = () => {};
````

This definition is rather opaque, how is this actually used?

I'm also not finding reference to `dispatch` in https://preactjs.com/guide/v10/api-reference so this might be something related to code mirror: https://codemirror.net/docs/

Here: https://codemirror.net/docs/ref/#view.EditorView.dispatch, but it doesn't actually have a `viewDispatch`.

Here they actually use both:

````ts
// in client/client_system.ts > fn ClientSystem::constructor
    this.commandHook.on({
      commandsUpdated: (commandMap) => {
        this.client.ui?.viewDispatch({
          type: "update-commands",
          commands: commandMap,
        });
        // Replace the key mapping compartment (keybindings)
        this.client.editorView.dispatch({
          effects: this.client.commandKeyHandlerCompartment?.reconfigure(
            createCommandKeyBindings(this.client),
          ),
        });
      },
    });
````

We can actually trace to codemirror through `editorView.dispatch`, but `viewDispatch` is still opaque.

Ok it's actually set in `ViewComponent()`:

````ts
// in client/editor_ui.tsx
  ViewComponent() {
    const [viewState, dispatch] = useReducer(reducer, initialViewState);
    this.viewState = viewState;
    this.viewDispatch = dispatch;
````

So this *is* a preact mechanism, it was just one layer of indirection far away. It is coming from `useReducer`, which is similar to `useState` that we have encountered in the preact tutorials in [004 SB How can we pass async determined data for the page title?](004%20SB%20How%20can%20we%20pass%20async%20determined%20data%20for%20the%20page%20title%3F.md) :

https://preactjs.com/tutorial/

For preact reference, https://preactjs.com/guide/v10/hooks#usereducer

We need to follow the logic of our custom `reducer` which takes a state and an action and gives us a new state.

## How do I indicate an update for one variable with it?

I need to just mark that I have updated `AppViewState.pageTitle` and rendering needs to take this into account.

It seems there is no general purpose way. We need to add our own action for updating the page title. It needs to be added to `client/types/ui.ts > Action` and `client/reducer.ts > fn reducer`.

Now we never set `pageTitle` directly, we always use the action associated with updating it via dispatch.
