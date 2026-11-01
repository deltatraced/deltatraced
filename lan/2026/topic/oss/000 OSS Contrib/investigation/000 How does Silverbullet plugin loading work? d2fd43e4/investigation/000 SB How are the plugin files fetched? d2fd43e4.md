---
context_type: investigation
status: done
---

Spawned by: [lan/2026/topic/oss/000 OSS Contrib/investigation/000 How does Silverbullet plugin loading work? d2fd43e4/000 How does Silverbullet plugin loading work? d2fd43e4](../000%20How%20does%20Silverbullet%20plugin%20loading%20work%3F%20d2fd43e4.md)

Spawned in: [^spawn-invst-9222a7](../000%20How%20does%20Silverbullet%20plugin%20loading%20work%3F%20d2fd43e4.md#spawn-invst-9222a7)

# Resolution

Initially, all the files are requested via HTTP GET request with `client/spaces/http_space_primitives.ts > fn HttpSpacePrimitives::authenticatedFetch`.

The request is handled by `server-common/src/space/disk.rs > <DiskSpacePrimitives as impl SpacePrimitives>::fetch_file_list`.

For the plugins themselves, they are concerned with all `*.plug.js` files that come out of this.

This now filters according to gitignore in the space repository.

# Journal

2026-06-18 Wk 25 Thu - 20:05 +03:00

`client/client_system.ts > fn reloadPlugsFromSpace`

* uses  `client/space.ts > fn listPlugs` which just searches all the files in the space for a `*.plug.js`.
  1. $\to$  `client/space.ts > deduplicatedFileList`
  1. $\to$ `client/spaces/space_primitives.ts > fn SpacePrimitives::fetchFileList`
     * note
       * There are multiple implementations of this, but we expect we're interested in `HttpSpacePrimitives` since we see GET requests in the network traffic for files in the client.
       * `HttpSpacePrimitives` is initialized in `client/client.ts > fn Client::initSpace`, which sets the base url that `fetchFileList` gets all the file from.
     * impls
       * `client/spaces/datastore_space_primitives.ts > fn <DataStoreSpacePrimitives as impl SpacePrimitives>::fetchFileList`
         * note
           * kv is initialized via `client/service_worker.ts > self.addEventListener("message", ...) > case "config" `
             * This is based on the config `config.enableClientEncryption` whether we get `IndexedDBKvPrimitives` or `EncryptedKvPrimitives` which wraps `IndexedDBKvPrimitives`.
             * There is also `MemoryKvPrimitives` which directly uses `fs` but it isn't referenced anywhere.
         * config querying
           1. `client/data/kv_primitives.ts > fn KvPrimitives::query`
           1. `client/data/indexeddb_kv_primitives.ts > fn <IndexedDBKvPrimitives as impl KvPrimitives>::query`
              * note
                * This uses  `IDBPObjectStore`
                  * https://developer.mozilla.org/en-US/docs/Web/API/IDBObjectStore
  1. $\to$ `client/spaces/http_space_primitives.ts > fn <HttpSpacePrimitives as impl SpacePrimitives>::fetchFileList`
  1. $\to$  `client/spaces/http_space_primitives.ts > fn HttpSpacePrimitives::authenticatedFetch`
     * note
       * This issues the GET request. From the Network packets we can see that we request a filename `/.fs/{space_relative_path}`. Now we need to see where this is handled on the server side, which is now in Rust. For the case of `fetchFileList`, it will actually just fetch `/.fs/` for everything.  Corresponding in rust to `fs::handle_fs_list`.
  1. $\to$ `server/src/router.rs > fn build_router`
  1. $\to$ `server/src/handlers/fs.rs > fn handle_fs_list`
  1. $\to$ `server-common/src/types.rs > SpacePrimitives::fetch_file_list`
  1. $\to$ `server-common/src/space/disk.rs > <DiskSpacePrimitives as impl SpacePrimitives>::fetch_file_list`
