---
context_type: issue
status: done
---

Parent: [000 SB Option to only have base name in title](../000%20SB%20Option%20to%20only%20have%20base%20name%20in%20title.md)

Spawned by: [000 SB Impl opt in tile as basename only via Std defined service](../task/000%20SB%20Impl%20opt%20in%20tile%20as%20basename%20only%20via%20Std%20defined%20service.md)

Spawned in: [^spawn-issue-b202e7](../task/000%20SB%20Impl%20opt%20in%20tile%20as%20basename%20only%20via%20Std%20defined%20service.md#spawn-issue-b202e7)

# Journal

2026-06-12 Wk 24 Fri - 18:21 +03:00

Why are we getting  `Error: No services matching: customizePageTitle`? It is defined, and it is giving us output via its `run`.

So it only happens at service reload, which means very early before services were initialized. We get exactly two errors.

We know `client.clientSystem.serviceRegistry` is already defined by the point we attempt to invoke best match.

One thing to note, is that searching `invokeBestMatch` shows no precedent in use of it in core code, besides in `./client/space_lua/stdlib/net.ts`.

We should only trigger `invokeBestMatch` when `client.clientSystem.scriptsLoaded`, since they are defined in lua scripts.

This is still happening even with checking that  `client.clientSystem.scriptsLoaded`

Let's make use of discovery then invocation directly instead of

````ts
// in plug-api/lib/ref.ts > fn customizePageTitleViaService
return await client.clientSystem.serviceRegistry.invokeBestMatch("customizePageTitle", path);
````

by replacing it with

````ts
// in plug-api/lib/ref.ts > fn customizePageTitleViaService
const services = await client.clientSystem.serviceRegistry.discover("customizePageTitle", path);

if (services.length === 0) {
  return new Promise(function (resolve, _reject) {
	resolve("")
  });
} else {
  return await client.clientSystem.serviceRegistry.invoke(services[0], path);
}
````

This will prevent the errors but then we will get pages with no title until we interact with the page!

We can try to change the dependency in

````ts
// in client/editor_ui.tsx > fn ViewComponent
useEffect(() => {
  // Ask a service to possibly customize the title, or otherwise just use the path.
  customizePageTitleViaService()
	.then(title => {
		viewState.pageTitle = title;
	})
}, [viewState.current, ]);
````

and add `client.clientSystem.scriptsLoaded`.

No good. Weirder is the title shows immediately on reload then disappears.

2026-06-12 Wk 24 Fri - 19:53 +03:00

Adding some debugging logs.

````ts
// in plug-api/lib/ref.ts > fn customizePageTitleViaService
const services = await client.clientSystem.serviceRegistry.discover("customizePageTitle", path);
console.log(`AAA00 customizePageTitleViaService (cur (j ${JSON.stringify(client.ui.viewState.current)})) (loaded? ${client.clientSystem.scriptsLoaded}) (services (j ${JSON.stringify(services)}) `);

// in client/service_registry.ts > fn ServiceRegistry::define
const id = globalThis.crypto.randomUUID();
console.log(`AAA01 ServiceRegistry::define (spec (j ${JSON.stringify(spec)}))`)
````

`AAA01` never triggers, and `AAA00` always has it as true for `loaded`. Often runs twice per page.

There was a big indexing job, which seems to have affected the behavior here...

````
// 12 more AAA01 logs for other selectors above
[Client] AAA01 ServiceRegistry::define (spec (j {"selector":"customizePageTitle","match":{"priority":1}}))

[Client] AAA01 ServiceRegistry::define (spec (j {"selector":"customizePageTitle","match":{"priority":1}}))

[Client] AAA01 ServiceRegistry::define (spec (j {"selector":"customizePageTitle","match":{"priority":1}}))

[Client] AAA00 customizePageTitleViaService (cur (j {"path":"...","meta":{"name":"...","size":3584,"contentType":"text/markdown","created":"2026-06-12T17:54:53.046","lastModified":"2026-06-11T02:11:07.534","perm":"rw","ref":"...","tag":"page","tags":[]}})) (loaded? true) (services (j [{"priority":1,"id":"8839e7cd-ea46-4530-910f-bfd61bca835d"},{"priority":1,"id":"be3d34f9-c416-4189-a703-1102e2d603b9"},{"priority":1,"id":"861aa944-794b-48a1-9f2f-63d45574d16d"}])

[Client] AAA00 customizePageTitleViaService (cur (j ...)) (loaded? true) (services (j [{"priority":1,"id":"8839e7cd-ea46-4530-910f-bfd61bca835d"},{"priority":1,"id":"be3d34f9-c416-4189-a703-1102e2d603b9"},{"priority":1,"id":"861aa944-794b-48a1-9f2f-63d45574d16d"}]) 

[Client] AAA00 customizePageTitleViaService (cur (j ...)) (loaded? true) (services (j [])

[Client] AAA00 customizePageTitleViaService (cur (j ...)) (loaded? true) (services (j [])

// The 15 AAA01 logs repeat below one more time
````

For some reason the service discovery seems to be intermittent, it's re-initialized, and is emptied right around when we check for it.

2026-06-13 Wk 24 Sat - 07:53 +03:00

Whether table of contents appears or not also seems intermittent.

Updating logs,

* For `AAA00`, loaded has always been true, and we don't need to log the current page medata

````ts
// in plug-api/lib/ref.ts > fn customizePageTitleViaService
const services = await client.clientSystem.serviceRegistry.discover("customizePageTitle", path);
console.log(`AAA00 customizePageTitleViaService (services (j ${JSON.stringify(services)}))`);

// in client/service_registry.ts > fn ServiceRegistry::define
const id = globalThis.crypto.randomUUID();
console.log(`AAA01 ServiceRegistry::define (spec (j ${JSON.stringify(spec)}))`)
````

Also updating

````diff
// in client/editor_ui.tsx > fn ViewComponent()
useEffect(() => {
  // Ask a service to possibly customize the title, or otherwise just use the path.
  customizePageTitleViaService()
	.then(title => {
		viewState.pageTitle = title;
	})
-}, [viewState.current, ]);
+}, [viewState.current === undefined, ]);
````

Because we don't care about content changes, only presence.

Now `AAA00` triggers exactly once, as we want. We are not sensitive to metadata changes of the file, only that there *is* a current page. In total, we do expect that `customizePageTitleViaService` runs twice per page load, once before `viewState.current` is defined, and another when it is.

This still doesn't explain why `AAA01` runs three times per `customizePageTitle` before and then after `AAA00` alongside the other service defines, but it no longer seems to affect this feature since the page title rendering seems stable now.
