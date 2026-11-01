---
status: todo
---

# Journal

2026-07-05 Wk 27 Sun - 22:26 +03:00

Notes are multimodal, it's text, it's images, it's video, etc. We open source this and link it in `codeberg.org`, but it is not sustainable to store huge assets that extend forever there. We need a different place to host assets that can be fetched alongside the repository. We do not want a service that provides endpoint access to a single asset, but the entire folder of assets, of arbitrary size, to be setup.

As of writing this,

````sh
# in /home/lan/src/cloned/cb/deltatraced/deltatraced

ls -al attachments | wc -l
# out {
	144
# }

du -h ./attachments
# out {
	30M     ./attachments
# }

du -h ./lan | tail -n1
# out {
	5.5M    ./lan
# }

du -h ./lan | sort -h | tail -n15
# out {
	468K    ./lan/archived/2026-05-21_2026/topic/study/books/math/2025/001 Probability - Theory and Examples
	472K    ./lan/archived/2026-05-21_2025/microproj
	496K    ./lan/archived/2026-05-21_2026/topic/study/books/math/2025
	500K    ./lan/archived/2026-05-21_2026/topic/study/books/math
	508K    ./lan/archived/2026-05-21_2026/topic/study/books
	524K    ./lan/archived/2026-05-21_2026/topic/practice/ctf
	720K    ./lan/archived/2026-05-21_2026/topic/practice
	760K    ./lan/archived/2026-05-21_2026/topic/study
	776K    ./lan/2026/topic
	1.2M    ./lan/archived/2026-05-21_2025
	1.6M    ./lan/2026
	2.1M    ./lan/archived/2026-05-21_2026/topic
	2.5M    ./lan/archived/2026-05-21_2026
	4.0M    ./lan/archived
	5.5M    ./lan
# }
````

And I've been trying to only use attachments when necessary.

2026-07-06 Wk 28 Mon - 22:14 +03:00

https://git-lfs.com/

* https://github.com/git-lfs/git-lfs
* https://github.com/git-lfs/git-lfs/tree/main/docs

https://www.anchorpoint.app/blog/5-alternatives-to-git-lfs-for-game-development

https://www.reddit.com/r/gamedev/comments/ly75cg/are_there_any_modern_alternatives_to_git_lfs/

* ├── https://dvc.org/
  * note
    * (company lakeFS https://pitchbook.com/profiles/company/470831-05 VC-private)
    * (open source)

We don't want to version these assets. Only retrieve them, and expect them to be immutable and append-only. We should be able to configure some cloud storage provider with `git-lfs`.

https://www.reddit.com/r/BuyFromEU/comments/1myweab/simple_cloud_storage_free_for_small_nonprofit_ngo/

* ├── https://filen.io/
  
  * note
    * (company Filen https://pitchbook.com/profiles/company/497864-26 Inc)
    * open source tooling around the cloud they provide
    * https://filen.io/pricing specifies they provide 10 Gb free storage.
* $\to$ https://filen.io/products/cli

* $\to$ https://github.com/FilenCloudDienste/filen-cli
  
  * note
    * https://github.com/FilenCloudDienste/filen-rs in active development to replace filen-cli
* https://proton.me/drive
  
  * note
    * (company Proton https://pitchbook.com/profiles/company/92813-32 Inc)
    * open source tooling around the cloud they provide
    * https://proton.me/drive/pricing 5 GB Storage for free
* ├── https://proton.me/community/open-source

* $\to$ https://github.com/ProtonDriveApps

10 GB is probably good. If we end up needing more, assets should be archived and provided via a self-hosted solution. We can also try to configure both a self-hosted solution and filen.io to provide service from one or the other.

This is a note repository, we shouldn't be too excessive with the assets that we want available on demand, but data accumulates with time, so archiving of prior year content can be a good strategy to shift to the self-hosted method.
