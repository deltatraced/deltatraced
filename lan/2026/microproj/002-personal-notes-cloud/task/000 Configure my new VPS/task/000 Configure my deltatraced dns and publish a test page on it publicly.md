---
context_type: task
status: done
---

Parent: [lan/2026/microproj/002-personal-notes-cloud/task/000 Configure my new VPS/000 Configure my new VPS](../000%20Configure%20my%20new%20VPS.md)

Spawned by: [lan/2026/microproj/002-personal-notes-cloud/task/000 Configure my new VPS/000 Configure my new VPS](../000%20Configure%20my%20new%20VPS.md)

Spawned in: [^spawn-task-42d8bc](../000%20Configure%20my%20new%20VPS.md#spawn-task-42d8bc)

Iterations: [001 Iterations for Configure my deltatraced dns and publish a test page on it publicly](../entry/001%20Iterations%20for%20Configure%20my%20deltatraced%20dns%20and%20publish%20a%20test%20page%20on%20it%20publicly.md)

# Journal

## Definitions

We take `{FQDN}` (fully qualified domain name) to mean something like `example.com`.

We take `{VPSIP}` to resolve to the VPS's IP address.

We take `{interface}` to be the associated interface like `eth0` for the `{VPSIP}`, check with `ifconfig` or `ip addr`.

We take `{port_incoming}` to be the outward facing port to connect through. For example, `8001`.

We take `{port_internal}` to be the internal port served on. For example, `8080`.

## Journal

2026-06-25 Wk 26 Thu - 20:43 +03:00

Spawn [lan/2026/microproj/002-personal-notes-cloud/task/000 Configure my new VPS/entry/001 Iterations for Configure my deltatraced dns and publish a test page on it publicly](../entry/001%20Iterations%20for%20Configure%20my%20deltatraced%20dns%20and%20publish%20a%20test%20page%20on%20it%20publicly.md) ^spawn-entry-60e8e3

2026-06-25 Wk 26 Thu - 20:43 +03:00

Check your domain name provider, and configure an `A Record` pointing `{VPSIP}` to the domain name `{FQDN}`. Configure `CNAME Record` and point it to `{FQDN}`.

https://dns.studio/dns-records/

* `A Record (Address Record)`: Points a domain to an IPv4 address
* `CNAME Record (Canonical Name)`: Used to point a domain to `www`.

Then

````sh
sudo resolvectl flush-caches
ping {FQDN}
````

Now let's port forward a test http server temporarily. With `tmux`, and `Ctrl-b c` to create a new pane and `Ctrl-b p` to switch between panes, have a pane do

````sh
python3 -m http.server
python3 -m http.server -b {VPSIP} {port_internal} -d some_dir
````

This will serve a simple listing of the current directory files at http://0.0.0.0:8000/. It will also serve the `index.html` there instead of listing a directory.

Then `Ctrl-b p` back to the control pane. You could also do it all in the same window, and use `Ctrl+z` and later `fg`.

2026-06-26 Wk 26 Fri - 00:06 +03:00

Spawn [001 Setup port forwarding with iptables  vps provider](001%20Setup%20port%20forwarding%20with%20iptables%20%20vps%20provider.md) ^spawn-task-4ddedd
