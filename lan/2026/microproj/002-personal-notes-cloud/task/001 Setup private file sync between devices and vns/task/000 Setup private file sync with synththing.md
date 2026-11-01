---
context_type: task
status: done
---

Parent: [lan/2026/microproj/002-personal-notes-cloud/task/001 Setup private file sync between devices and vns/001 Setup private file sync between devices and vns](../001%20Setup%20private%20file%20sync%20between%20devices%20and%20vns.md)

Spawned by: [lan/2026/microproj/002-personal-notes-cloud/task/001 Setup private file sync between devices and vns/001 Setup private file sync between devices and vns](../001%20Setup%20private%20file%20sync%20between%20devices%20and%20vns.md)

Spawned in: [^spawn-task-cd8ebc](../001%20Setup%20private%20file%20sync%20between%20devices%20and%20vns.md#spawn-task-cd8ebc)

Iterations: [001 Iterations for Setup private file sync with syncthing](../entry/001%20Iterations%20for%20Setup%20private%20file%20sync%20with%20syncthing.md)

# Journal

2026-06-26 Wk 26 Fri - 06:49 +03:00

Need to install go

https://go.dev/doc/install

````sh
sudo apt-get install golang-go
````

2026-06-26 Wk 26 Fri - 02:46 +03:00

https://github.com/syncthing/syncthing

````sh
mkdir -p ~/src/cloned/gh/syncthing/
cd ~/src/cloned/gh/syncthing/
git clone https://github.com/syncthing/syncthing.git
cd syncthing
./build.sh
./bin/syncthing
````

https://docs.syncthing.net/intro/getting-started.html

2026-06-26 Wk 26 Fri - 05:15 +03:00

Pretty straightforward use. Make sure you're under the same network, you can add a device, and share a folder with that device in editing the device and going to the sharing tab. On android there is an app [syncthing-fork](https://play.google.com/store/apps/details?id=com.github.catfriend1.syncthingandroid&hl=en&pli=1) that works with this, and you may have to go to the GUI mode there to see the notifications to accept sharing a folder.

With this I'm able to share an obsidian vault (or any folder) under my local network with my phone and PC.

Now we want to be able to set this up securely on different servers.

https://docs.syncthing.net/users/firewall.html

````sh
# in vps > ~/.port-forwarding
export port_internal=22000 && \
export port_incoming=22000 && \
port_forward "syncthing" tcp $port_internal $port_incoming && \

export port_internal=22000 && \
export port_incoming=22000 && \
port_forward "syncthing" udp $port_internal $port_incoming && \
````

Make sure to also port forward this with your VPS provider.

2026-06-26 Wk 26 Fri - 07:15 +03:00

Spawn [lan/2026/microproj/002-personal-notes-cloud/task/001 Setup private file sync between devices and vns/entry/001 Iterations for Setup private file sync with syncthing](../entry/001%20Iterations%20for%20Setup%20private%20file%20sync%20with%20syncthing.md) ^spawn-entry-54d48a

2026-06-26 Wk 26 Fri - 08:14 +03:00

1. https://forum.syncthing.net/t/configuring-syncthing-without-web-gui/7267/7
1. https://docs.syncthing.net/dev/rest.html

https://docs.syncthing.net/users/config.html

https://docs.syncthing.net/users/untrusted.html

Config is at `~/.local/state/syncthing`.

This can be configured directly with folder and device tags. We can also try to access the `8384` UI for easier config using SSH tunneling:

From https://linuxize.com/post/how-to-setup-ssh-tunneling/,

````sh
ssh -L [LOCAL_IP:]LOCAL_PORT:DESTINATION:DESTINATION_PORT [USER@]SSH_SERVER
````

So for example,

````sh
ssh -L 8001:localhost:8384 user@mysite.com
````

would allow me to access the settings page on `http://localhost:8001` on my host from my VPS.

It does mention

 > 
 > Username/Password has not been set for the GUI authentication. Please consider setting it up.
 > If you want to prevent other users on this computer from accessing Syncthing and through it your files, consider setting up authentication.

which is a good idea, although this is not publicly exposed and I am the only with access to this in my case.

In this case, we want to add a device for our own host `http://localhost:8384`,

1. click Add Remote Device
1. go to Advanced > Addresses,
1. set the port forwarded address instead of relying on discovery, so change `dynamic` to something like `tcp://mysite.com:22000`
1. Check Untrusted if needed.
1. Back in `General`, put the `Device ID` in accordance with the one found in `http://localhost:8001` under This Device > Identification
1. Configure some folder to share under `Sharing` and set the password if `Untrusted` was check.

2026-06-26 Wk 26 Fri - 09:37 +03:00

hello from mobile!
