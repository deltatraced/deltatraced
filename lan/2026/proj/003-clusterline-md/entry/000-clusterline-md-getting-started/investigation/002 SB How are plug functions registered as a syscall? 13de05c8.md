---
context_type: investigation
status: done
---

Parent: [lan/2026/proj/003-clusterline-md/entry/000-clusterline-md-getting-started/000-clusterline-md-getting-started](../000-clusterline-md-getting-started.md)

Spawned by: [lan/2026/proj/003-clusterline-md/entry/000-clusterline-md-getting-started/task/005 Add a custom fuzzy selector window with some text](../task/005%20Add%20a%20custom%20fuzzy%20selector%20window%20with%20some%20text.md)

Spawned in: [^spawn-invst-618590](../task/005%20Add%20a%20custom%20fuzzy%20selector%20window%20with%20some%20text.md#spawn-invst-618590)

# Journal

2026-06-21 Wk 25 Sun - 19:16 +03:00

Registeration occurs through `client/plugos/system.ts > fn registerSyscalls`

`client/plugos/hooks/syscall.ts > fn <SyscallHook as impl Hook<SyscallHookT>>::registerSyscalls` registers sys calls per plug function.

These values come from `plug.manifest!.functions`

* https://www.typescriptlang.org/docs/
  * `?` for optional properties: https://www.typescriptlang.org/docs/handbook/2/everyday-types.html#optional-properties
  * `!` in an expression is a non-null assertion operator: https://www.typescriptlang.org/docs/handbook/2/everyday-types.html#non-null-assertion-operator-postfix-
    * not checked, it tells the type checker we know this is the case.
    * This is for terms and is NOT to be confused with definite assignment assertion operator `!`.
  * `!` in a type is a definite assignment assertion operator: https://www.typescriptlang.org/docs/handbook/2/classes.html#--strictpropertyinitialization
    * This means that a class field is initialized by means other than the class constructor

The manifest is loaded in `client/plugos/plug.ts > fn Plug<HookT>::createLazily<HookT>` via `system.options.manifestCache!.getManifest`

How are the functions loaded?

1. `client/plugos/plug.ts > fn Plug<HookT>::createLazily<HookT>`
1. `client/plugos/manifest_cache.ts > fn ManifestCache<T>::getManifest`
   * investigation
     * [x] How is it initialized for plugins?
       1. `client/plugos/system.ts > System<Hook> > fn constructor`
          * note
            * Initializes `client/plugos/manifest_cache.ts > InMemoryManifestCache<T>`
1. `client/plugos/manifest_cache.ts > fn <InMemoryManifestCache<T> as impl ManifestCache<T>>::getManifest`
   * investigation
     * [x] How does this cache the manifest?
       * note
         * As an `XCache` it just caches the operation, which in this case is `getManifest` internally for performance.
       1. `client/plugos/system.ts > System<Hook> > fn constructor`
       1. `client/plugos/manifest_cache.ts > fn <InMemoryManifestCache<T> as impl ManifestCache<T>>::getManifest`
   * initializes
     1. `client/plugos/sandboxes/sandbox.ts > fn Sandbox<HookT>::init`
     1. `client/plugos/sandboxes/worker_sandbox.ts > fn <WorkerSandbox<HookT> as impl Sandbox<HookT>>::init`
1. `client/plugos/sandboxes/sandbox.ts > SandBox<HookT>::manifest`
1. `client/plugos/sandboxes/worker_sandbox.ts > fn <WorkerSandbox<HookT> as impl SandBox<HookT>>::manifest`
   * $\downarrow$ written by
1. `client/plugos/sandboxes/worker_sandbox.ts > fn <WorkerSandbox<HookT> as impl Sandbox<HookT>>::init`

The manifest file should declare public functions as specified in `client/plugos/types.ts > fn Manifest<HookT>::functions`.

The manifest file might be the `plugname.plug.yaml` file.

`client/plugos/types.ts > FunctionDef` doesn't include `command` nor `command.name` which is included in the JSON `plugname.plug.yaml`.

Also, `plugname.plug.yaml` is instead retrieved as a path through `build/build_plugs.ts > fn buildPlugsAndLibraries`

The manifest is compiled in `client/plugos/plug_compile.ts > fn compileManifest`

Type `plug-api/types/manifest.ts > CommandDef` includes the `name` property.

`client/plugos/plug_compile.ts > fn compileManifest` also sets the code that posts for the message event that likely is subscribed to by the listener at `client/plugos/sandboxes/worker_sandbox.ts > fn <WorkerSandbox<HookT> as impl Sandbox<HookT>>::init`.

`client/plugos/hooks/command.ts > fn validateManifest` checks for `functionDef.command` and retrieves the `name` property as expected.
