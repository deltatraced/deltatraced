---
context_type: task
status: todo
---

Parent: [lan/2026/topic/oss/000 OSS Contrib/task/000 SB Option to only have base name in title/000 SB Option to only have base name in title](../000%20SB%20Option%20to%20only%20have%20base%20name%20in%20title.md)

Spawned by: [lan/2026/topic/oss/000 OSS Contrib/task/000 SB Option to only have base name in title/000 SB Option to only have base name in title](../000%20SB%20Option%20to%20only%20have%20base%20name%20in%20title.md)

Spawned in: [^spawn-task-502552](../000%20SB%20Option%20to%20only%20have%20base%20name%20in%20title.md#spawn-task-502552)

Overview: [001 Overview SB Option to only have base name in title](../entry/001%20Overview%20SB%20Option%20to%20only%20have%20base%20name%20in%20title.md)

# Journal

2026-06-18 Wk 25 Thu - 18:02 +03:00

https://github.com/silverbulletmd/silverbullet/pull/2023

This PR was closed likely due to the `make fmt` commit, so let's reopen without it.

It did seem like a bad idea to just modify all these files, but I did put the result of `make fmt` in its own commit. I thought this should make it easy to just check the other commit for review, and advise on the `make fmt` to be reverted. But I guess they would rather that the commit doesn't exist at all. Hopefully the PR is okay to review without this commit.

````sh
# in /home/lan/src/cloned/gh/LanHikari22/forked/silverbulletmd/branches/silverbullet@title-text-config
git lg

# out
* 528b5741 (HEAD -> title-text-config, origin/title-text-config) chore: run make fmt
* a86350da Add option to strip title full path to name via service (#2016)
````

````sh
# in /home/lan/src/cloned/gh/LanHikari22/forked/silverbulletmd/branches/silverbullet@title-text-config
git reset --hard HEAD~1
git push origin title-text-config --force
````

https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/working-with-forks/configuring-a-remote-repository-for-a-fork

````sh
# in /home/lan/src/cloned/gh/LanHikari22/forked/silverbulletmd/branches/silverbullet@title-text-config
git remote add upstream git@github.com:silverbulletmd/silverbullet.git
git config pull.rebase false
git pull upstream main
````

I would rather my commit be at the top, and to not have to merge if I can help it.

````
commit a86350dabbbe0c22c69d9373f8fc959d9c0d7d08 (origin/title-text-config)
* / a86350da (origin/title-text-config) Add option to strip title full path to name via service (#2016)
````

[so ans How can I generate a Git patch for a specific commit?](https://stackoverflow.com/a/6658352/6944447)

````sh
# in /home/lan/src/cloned/gh/LanHikari22/forked/silverbulletmd/branches/silverbullet@title-text-config
git format-patch -1 a86350da

# out
0001-Add-option-to-strip-title-full-path-to-name-via-serv.patch
````

````sh
# in /home/lan/src/cloned/gh/LanHikari22/forked/silverbulletmd/branches/silverbullet@title-text-config
mv 0001-Add-option-to-strip-title-full-path-to-name-via-serv.patch ~/tmp/
````

Remove repo and start over,

````sh
# in /home/lan/src/cloned/gh/LanHikari22/forked/silverbulletmd/branches/silverbullet@title-text-config
mv ..
rm -rf silverbullet@title-text-config
git clone git@github.com:LanHikari22/silverbullet.git
mv silverbullet silverbullet@title-text-config
````

````sh
# in /home/lan/src/cloned/gh/LanHikari22/forked/silverbulletmd/branches/silverbullet@title-text-config > branch main
git remote add upstream git@github.com:silverbulletmd/silverbullet.git
git config pull.rebase false
git pull upstream main
git push origin main
git checkout -b title-text-config
````

````sh
# in /home/lan/src/cloned/gh/LanHikari22/forked/silverbulletmd/branches/silverbullet@title-text-config
git apply --verbose ~/tmp/0001-Add-option-to-strip-title-full-path-to-name-via-serv.patch

# out
/home/lan/tmp/0001-Add-option-to-strip-title-full-path-to-name-via-serv.patch:135: trailing whitespace.
     -- Other plugs of higher priority may override this behavior.
/home/lan/tmp/0001-Add-option-to-strip-title-full-path-to-name-via-serv.patch:142: trailing whitespace.
  run = function(path)
Checking patch client/editor_ui.tsx...
Hunk #2 succeeded at 289 (offset 31 lines).
Hunk #3 succeeded at 441 (offset 32 lines).
Checking patch client/reducer.ts...
Hunk #1 succeeded at 231 (offset 2 lines).
Checking patch client/types/ui.ts...
error: while searching for:
  showConfirm: boolean;
  confirmMessage?: string;
  confirmCallback?: (value: boolean) => void;
};

export const initialViewState: AppViewState = {

error: patch failed: client/types/ui.ts:73
error: client/types/ui.ts: patch does not apply
Checking patch libraries/Library/Std/Config.md...
Checking patch libraries/Library/Std/Pages/Name Customizations.md...
Checking patch plug-api/lib/ref.ts...
````

````sh
# in /home/lan/src/cloned/gh/LanHikari22/forked/silverbulletmd/branches/silverbullet@title-text-config
git apply --check ~/tmp/0001-Add-option-to-strip-title-full-path-to-name-via-serv.patch
error: patch failed: client/types/ui.ts:73
error: client/types/ui.ts: patch does not apply
````

````sh
# in /home/lan/src/cloned/gh/LanHikari22/forked/silverbulletmd/branches/silverbullet@title-text-config
git am -3 < ~/tmp/0001-Add-option-to-strip-title-full-path-to-name-via-serv.patch
````

[so ans Update git commit author date when amending](https://stackoverflow.com/a/35515190/6944447)

````sh
# in /home/lan/src/cloned/gh/LanHikari22/forked/silverbulletmd/branches/silverbullet@title-text-config
git commit --amend --date=now --no-edit
````

Need to amend with some formatting changes:

* `libraries/Library/Std/Pages/Name Customizations.md` has space at the end of some lines
* `client/reducer.ts` my change used tabs. They are using spaces.

2026-06-18 Wk 25 Thu - 19:01 +03:00

````sh
# in /home/lan/src/cloned/gh/LanHikari22/forked/silverbulletmd/branches/silverbullet@title-text-config
make setup
make test

# out
 Test Files  19 failed | 63 passed (82)
      Tests  873 passed (873)
   Start at  19:02:24
   Duration  3.48s (transform 20.57s, setup 0ms, import 29.06s, tests 4.08s, environment 20ms)
````

So test fails this time.

This is the same failure we get from upstream main now.

We're in the middle of a migration from a go-based server to a rust-based one. The instructions for running the server changed from `air <PATH-TO-YOUR-SPACE>` in the README to `./target/release/silverbullet <PATH-TO-YOUR-SPACE>`

2026-06-20 Wk 25 Sat - 11:55 +03:00

The plug still works after the rust migration. We can see now that the service is only added once, before I've seen it three times:

````ts
export async function customizePageTitleViaService(): Promise<string> {
  if (client.ui.viewState.current === undefined) {
    return new Promise(function (resolve, _reject) {
      resolve("");
    });
  } else {
    console.log(`AAA00.00`);
    let path = getNameFromPath(client.ui.viewState.current.path);
    console.log(`AAA00.01 (path ${path})`);

    const services = await client.clientSystem.serviceRegistry.discover(
      "customizePageTitle",
      path,
    );
    console.log(`AAA00.02 (services (j ${JSON.stringify(services)}))`);

    if (services.length === 0) {
      return new Promise(function (resolve, _reject) {
        // Just give the path until we have service. This can happen during big index jobs
        console.log(`AAA00.03a`);
        resolve(path);
      });
    } else {
      console.log(`AAA00.03b`);
      return await client.clientSystem.serviceRegistry.invoke(
        services[0],
        path,
      );
    }
  }
}
````

````
[Client] AAA00.00
[Client] AAA00.01 (path lan/2026/main/wikiproc/000 Wiki Proc Weekly/entry/000 Proc Tasks From 2026 Wk 23)
[Client] AAA00.02 (services (j [{"priority":1,"id":"8bf72faf-3f82-4c56-a35d-3f287a4d331f"}]))
[Client] AAA00.03b
````

2026-06-20 Wk 25 Sat - 11:58 +03:00

Okay we're ready to submit a new PR for this since it still works. Let's make sure it's at the top again:

````sh
# in /home/lan/src/cloned/gh/LanHikari22/forked/silverbulletmd/branches/silverbullet@title-text-config
git format-patch -1 886ec37e
mv ~/tmp/0001-Add-option-to-strip-title-full-path-to-name-via-serv.patch ~/tmp/0001-Add-option-to-strip-title-full-path-to-name-via-serv.patch.0
mv 0001-Add-option-to-strip-title-full-path-to-name-via-serv.patch ~/tmp/
git checkout main
git branch -D title-text-config
git pull upstream main
````

This time all tests pass from upstream!

````sh
# in /home/lan/src/cloned/gh/LanHikari22/forked/silverbulletmd/branches/silverbullet@title-text-config
git checkout -b title-text-config
git am -3 < ~/tmp/0001-Add-option-to-strip-title-full-path-to-name-via-serv.patch
git commit --amend --date=now --no-edit
````

Testing manually again. There's a big index job again and the service won't kick off. and even though through [000 SB How are the plugin files fetched? d2fd43e4](../../../investigation/000%20How%20does%20Silverbullet%20plugin%20loading%20work%3F%20d2fd43e4/investigation/000%20SB%20How%20are%20the%20plugin%20files%20fetched%3F%20d2fd43e4.md) we learned that the rust backend now respects `.gitignore`, it is still indexing things under `node_modules/` which I gitignore in my plug. But adding them to my space `.gitignore` does filter them from indexing.

Manual testing OK. Automatic testing OK. We're ready for PR.
