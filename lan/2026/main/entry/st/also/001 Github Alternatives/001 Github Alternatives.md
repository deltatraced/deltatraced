# Journal

2026-06-09 Wk 24 Tue - 08:41 +03:00

These provide some reasons to seek alternatives, which include its stance on AI and data collection:

* https://lord.io/leaving-github/

For a centralized forge, my current choice is:

* https://codeberg.org/

It's run by a non-profit, I've already encountered it in the wild multiple times, promises not to track, use, or sell data. and we can check and contribute to the source: https://codeberg.org/Codeberg-Infrastructure.

For decentralized,

https://tangled.org/

Also open source code: https://tangled.org/tangled.org/core

Although unlike codeberg, this is venture capital-backed: https://pitchbook.com/profiles/company/819965-89

2026-06-09 Wk 24 Tue - 10:06 +03:00

For now let's use codeberg. Do note that they mostly support FOSS software, and have restrictions on private repos, so that might require to be self hosted, but it can be done with Forgejo which they use directly: https://forgejo.org/

On AI scraping,

* https://codeberg.org/Codeberg/Community/issues/1585

Codeberg is also working on federation: https://codeberg.org/ForgeFed/forgefed, https://forgefed.org/spec/

We also might be interested in maintaining mirrors on github, and being mainly on codeberg.

2026-06-26 Wk 26 Fri - 17:19 +03:00

Now that we have a VPS self-hosting is also an option. Maybe look into federation or some discovery mechanism for self-hosted instances to connect with others?

https://github.com/go-gitea/gitea/issues/18240

 > 
 > **aside** https://codeberg.org/forgejo/sustainability/src/branch/main/README.md they have funding records and also suggest it's useful data for others to do this practice so we can track sustainability for open source development. They also track time for volunteers.
