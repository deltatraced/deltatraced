---
context_type: entry
---

Parent: [lan/2026/microproj/002-personal-notes-cloud/task/001 Setup private file sync between devices and vns/001 Setup private file sync between devices and vns](../001%20Setup%20private%20file%20sync%20between%20devices%20and%20vns.md)

Spawned by: [lan/2026/microproj/002-personal-notes-cloud/task/001 Setup private file sync between devices and vns/task/000 Setup private file sync with synththing](../task/000%20Setup%20private%20file%20sync%20with%20synththing.md)

Spawned in: [^spawn-entry-54d48a](../task/000%20Setup%20private%20file%20sync%20with%20synththing.md#spawn-entry-54d48a)

# Journal

## Iteration 1.0 Some investigation into tailscale and friends

2026-06-26 Wk 26 Fri - 05:15 +03:00

One thing I encountered is https://tailscale.com/ (https://github.com/tailscale/tailscale)

 > 
 > **aside** They require [Developer Certificate of Origin](https://en.wikipedia.org/wiki/Developer_Certificate_of_Origin) which is used in kernel development. I know this was recently emphasized as something LLMs must never put themselves, but not sure if it's related here.

Not all of tailscale is open source, and doesn't seem out of the box designed with self-hosting in mind.

* https://www.reddit.com/r/selfhosted/comments/18evofr/a_word_of_caution_about_tailscale/
  * company
    * https://pitchbook.com/profiles/company/268781-05
  * branch
    * https://github.com/juanfont/headscale
      * note
        * Integrates with tailscale for self hosting, replacing a closed-source component it uses by default in networking
    * https://www.zerotier.com/ (https://github.com/zerotier/ZeroTierOne)
      * company
        * https://pitchbook.com/profiles/company/104527-00
      * note
        * Not open source: https://github.com/zerotier/ZeroTierOne/blob/dev/nonfree/LICENSE.md
* https://www.reddit.com/r/zerotier/comments/jfpj5r/alternatives_to_zerotier/
  * note
    * There seems to be a post here by a representative from zerotier explaining about their payment strategy.
  * branch
    * https://netbird.io/ (https://github.com/netbirdio/netbird)
    * https://github.com/slackhq/nebula ([MIT](https://github.com/slackhq/nebula/blob/master/LICENSE))
    * https://www.twingate.com/  (doesn't seem open source)

|Service|Fully Open Source?|Fully Source Available?|
|-------|------------------|-----------------------|
|[tailscale](https://github.com/tailscale/tailscale)|No|No|
|[zerotier](https://github.com/zerotier/ZeroTierOne)|No|Yes|
|[netbird](https://github.com/netbirdio/netbird)|Yes: [BSD-3-clause](https://opensource.org/license/BSD-3-clause), [agpl-3-0](https://opensource.org/license/agpl-3-0)|Yes|

### Branch Reason

2026-06-26 Wk 26 Fri - 06:29 +03:00

Removed as it's not made clear it's necessary to investigate all of this. There's no clear use case. We've seen it posted in relation to syncthing, but that context was likely outside a self-hosting context. For our use case, self-hosting with just port forwarding will likely suffice as we make use of encryption options with syncthing. Syncthing already only allows folder sharing for devices that both agree to sync also.
