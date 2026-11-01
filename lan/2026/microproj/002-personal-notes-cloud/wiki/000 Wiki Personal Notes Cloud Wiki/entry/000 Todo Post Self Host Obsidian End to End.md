---
context_type: entry
---

Parent: [000 Wiki Personal Notes Cloud Wiki](../000%20Wiki%20Personal%20Notes%20Cloud%20Wiki.md)

Spawned by: [000 Wiki Proc Personal Notes Cloud](../../../wikiproc/000%20Wiki%20Proc%20Personal%20Notes%20Cloud/000%20Wiki%20Proc%20Personal%20Notes%20Cloud.md)

Spawned in: [^spawn-entry-568fa0](../../../wikiproc/000%20Wiki%20Proc%20Personal%20Notes%20Cloud/000%20Wiki%20Proc%20Personal%20Notes%20Cloud.md#spawn-entry-568fa0)

---

# Why not Obsidian Sync?

As of this writing, obsidian sync's $4/mo plan only gives us 1GB of storage, and it seems service limited. We can do better. If you wish to support the obsidian team for their free note taking app, you can donate to them directly.

# Obtain a Server

First, check if you are able to obtain a static IP via your ISP, or get a `$5/mo` VPS like https://www.ionos.com/servers/vps (recommended for the 90GB storage in it. See notes for some specs: [000 PNC What services can we make use of?](../../../wikiproc/000%20Wiki%20Proc%20Personal%20Notes%20Cloud/investigation/000%20PNC%20What%20services%20can%20we%20make%20use%20of%3F.md))

The following assumes an ubuntu 26.04 instance:

````sh
$ lsb_release -a

No LSB modules are available.
Distributor ID: Ubuntu
Description:    Ubuntu 26.04 LTS
Release:        26.04
Codename:       resolute
````

# Configure syncthing

Follow similar process to

1. [001 Setup port forwarding with iptables  vps provider](../../../task/000%20Configure%20my%20new%20VPS/task/001%20Setup%20port%20forwarding%20with%20iptables%20%20vps%20provider.md)
1. [000 Setup private file sync with synththing](../../../task/001%20Setup%20private%20file%20sync%20between%20devices%20and%20vns/task/000%20Setup%20private%20file%20sync%20with%20synththing.md)

Once syncthing is accessible through your VPS you can use SSH tunneling to open the config UI in your host via something like

````sh
ssh -L 8001:localhost:8384 user@mysite.com
````

Then whether it's your phone or host, both can be configured by adding a device and your target folder where your obsidian vault is. You can also set the untrusted option and use a password so they are encrypted on the VPS. Since the VPS is planned to be the central device that would share between your hosts across networks, then you will have to decrypt on those trusted peripheral devices.

This is a general method to share files in a self-hosted manner, and since obsidian is just markdown files, it should work with it as well.

The password thing seems to be a one-time done as you add a folder to a trusted device to decrypt encrypted data from an untrusted device so it's not so bad.
