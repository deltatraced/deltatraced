---
context_type: task
status: done
---

Parent: [lan/2026/main/task/001 Install a new Gentoo system/001 Install a new Gentoo system](../001%20Install%20a%20new%20Gentoo%20system.md)

Spawned by: [lan/2026/main/task/001 Install a new Gentoo system/001 Install a new Gentoo system](../001%20Install%20a%20new%20Gentoo%20system.md)

Spawned in: [^spawn-task-7c6997](../001%20Install%20a%20new%20Gentoo%20system.md#spawn-task-7c6997)

Overview: [001 Overview Install a new Gentoo system](../entry/001%20Overview%20Install%20a%20new%20Gentoo%20system.md)

# Journal

2026-07-26 Wk 30 Sun - 14:26 +03:00

Okay so now we need to decide on a browser to install.

I have been using firefox for a while. But there has been controversy now:

* (1) https://arstechnica.com/tech-policy/2025/02/firefox-deletes-promise-to-never-sell-personal-data-asks-users-not-to-panic/
* (2) https://blog.mozilla.org/en/firefox/update-on-terms-of-use/

(1) mention that this was in Mozilla FAQ (https://www.mozilla.org/en-US/privacy/faq/):

````
Mozilla doesn’t sell data about you (in the way that most people think about “selling data”), and we don’t buy data about you. Since we strive for transparency, and the LEGAL definition of “sale of data”
     is extremely broad in some places, we’ve had to step back from making the definitive statements you know and love. We still put a lot of work into making sure that the data that we share with our partners
     (which we need to do to make Firefox commercially viable) is stripped of any identifying information, or shared only in the aggregate, or is put through our privacy preserving technologies (like OHTTP).
````

Here is the updated language from (2) as of this date (2026-07-26 Wk 30 Sun - 15:30 +03:00):

````
TL;DR Mozilla doesn’t sell data about you (in the way that most people think about “selling data”), and we don’t buy data about you. We changed our language because some jurisdictions define “sell” more broadly than most people would usually understand that word. Firefox has built-in privacy and security features, plus options that let you fine-tune your data settings.
````

````
In order to make Firefox commercially viable, there are a number of places where we collect and share some data with our partners, including our optional ads on New Tab and providing sponsored suggestions in the search bar. We set all of this out in our privacy notice. Whenever we share data with our partners, we put a lot of work into making sure that the data that we share is stripped of potentially identifying information, or shared only in the aggregate, or is put through our privacy preserving technologies (like OHTTP).
````

Currently a lot of browsers seem to be chrome or firefox forks.

While reading online I'm encountering some browsers which may be niche,

* Ladybird ([They mentioned they use AI](https://ladybird.org/posts/changing-how-we-develop-ladybird/), [They used claude code and codex (LLMs as Agents) for a rust migration](https://ladybird.org/posts/adopting-rust/)),

* (3) https://discussion.fedoraproject.org/t/why-is-mozilla-firefox-built-with-telemetry-reporting-explicity-enabled/146466/14
  
  * telemetry gathering

There are some cool vim-like browsers I thought could be cool to try, but these are based on webkit, trademarked by apple:

* https://github.com/fanglingsu/vimb
* https://github.com/qutebrowser/qutebrowser

Anyway let's use [librewolf](https://www.librewolf.net/).

From https://www.librewolf.net/privacy-policy/,

 > 
 > One of the goals of LibreWolf is to remove the data collection and telemetry from Firefox, and thus we don't collect any data from the user in the LibreWolf browser or on the LibreWolf website.
 > We can't always assure that no data is sent from the browser to Mozilla or other third parties, but we try our best to achieve that. For that case, also check out the Firefox Privacy Notice.

2026-07-26 Wk 30 Sun - 16:59 +03:00

Updated note title: `001 Install a browser on gentoo` $\to$ `001 Install a browser on gentoo - librewolf`

https://wiki.gentoo.org/wiki/LibreWolf

Spawn [lan/2026/main/task/001 Install a new Gentoo system/issue/000 eselect repository wont load while adding librewolf repo](../issue/000%20eselect%20repository%20wont%20load%20while%20adding%20librewolf%20repo.md) ^spawn-issue-5ac29f

````sh
su
emerge --ask app-eselect/eselect-repository
eselect repository add librewolf git https://codeberg.org/librewolf/gentoo.git
emaint sync -r librewolf
````

This adds:

````sh
# in /etc/portage/repos.conf/eselect-repo.conf
# created by eselect-repo

[librewolf]
location = /var/db/repos/librewolf
sync-type = git
sync-uri = https://codeberg.org/librewolf/gentoo.git
````

Spawn [lan/2026/main/task/001 Install a new Gentoo system/issue/001 Tried to emerge librewolf but it is not satisfied due to mask tilda amd64](../issue/001%20Tried%20to%20emerge%20librewolf%20but%20it%20is%20not%20satisfied%20due%20to%20mask%20tilda%20amd64.md) ^spawn-issue-2feca8

--/ 2026-07-26 Wk 30 Sun - 18:23 +03:00

Let's find a utility to explain use flags present:

https://wiki.gentoo.org/wiki/Equery#Listing_per-package_USE_flags_with_uses\_.28u.29

`equery keywords www-client/firefox` works but not `equery keywords www-client/librewolf` even though it can tab complete to it. Maybe because we added an external repo?

Actually no, we can see the uses for it with `equery uses www-client/librewolf`.

https://codeberg.org/librewolf/gentoo.git

````
From https://codeberg.org/librewolf/gentoo.git,

Personally I use diff -aur <librewolf ebuild> <firefox ebuild> when making version bumps.
````

--/

````sh
equery uses www-client/librewolf

# out (relevant)
- - eme-free         : Disable EME (DRM plugin) capability at build time
- - telemetry        : Send anonymized usage information to upstream so they can better understand our users
````

--/ 2026-07-26 Wk 30 Sun - 18:42 +03:00
`ques[t:status=pend t:topic-librewolf]` Telemetry being disabled makes sense. But why is `eme-free` disabled by default for librewolf?

If you do `equery uses www-client/firefox` you will by by default that `+telemetry -eme-free`.
--/

````sh
# in /etc/portage/package.accept_keywords/www-client/librewolf {
	www-client/librewolf ~amd64
# }

# in /etc/portage/package.accept_keywords/dev-libs/nss {
	>=dev-libs/nss-3.125 ~amd64
# }

# in /etc/portage/package.use/www-client/librewolf {
	www-client/librewolf eme-free
# }

# in /etc/portage/package.use/media-libs/libvpx {
	>=media-libs/libvpx-1.16.0 postproc
# }

su
emerge --ask www-client/librewolf
````

2026-07-26 Wk 30 Sun - 20:33 +03:00

Installed! Took around 1.5 hours for me.

Gonna get some extensions.

I like window titler, because I am able to give title to browser pages and then go to it via a window switcher.

https://addons.mozilla.org/en-US/firefox/addon/window-titler/

Extensions and themes > Settings Icon > Manage Extension Shortcuts > set ALT+W for "Activate toolbar button"

Now I can have labels like `rsch misc` for any quick research, `rsch mytopic` for a dedicated one, `mus` for music and so on.

I also like vimium to be more flexible moving around

addons.mozilla.org/en-US/firefox/addon/vimium-c/

Basically

* `f` for jumping to an element,
* `b` to fuzzy find a bookmark, which then I can filter by keywords to go to often visited pages quickly
  * Have to also allow permissions for vimium to access bookmarks for this.
* hjkl navigation
* sometimes also `/` to search the page.
* You can search text with `/` then use `v` to select and copy it, no need for mouse. Also `V` to select the whole line.

Also turn on vertical tabs. It can be toggled with Alt+Ctrl+Z, and can be used to view what we have in a labeled window that's win+f

Also learned about this when I went to toggle AI searches off in duckduckgo: https://addons.mozilla.org/en-US/firefox/addon/duckduckgo-no-ai-search/
