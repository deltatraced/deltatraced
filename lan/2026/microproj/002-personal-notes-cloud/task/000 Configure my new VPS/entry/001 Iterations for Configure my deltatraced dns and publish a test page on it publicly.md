---
context_type: entry
---

Parent: [lan/2026/microproj/002-personal-notes-cloud/task/000 Configure my new VPS/000 Configure my new VPS](../000%20Configure%20my%20new%20VPS.md)

Spawned by: [lan/2026/microproj/002-personal-notes-cloud/task/000 Configure my new VPS/task/000 Configure my deltatraced dns and publish a test page on it publicly](../task/000%20Configure%20my%20deltatraced%20dns%20and%20publish%20a%20test%20page%20on%20it%20publicly.md)

Spawned in: [^spawn-entry-60e8e3](../task/000%20Configure%20my%20deltatraced%20dns%20and%20publish%20a%20test%20page%20on%20it%20publicly.md#spawn-entry-60e8e3)

# Iteration 1.0 Configured an internal dns server

## Journal

2026-06-25 Wk 26 Thu - 15:06 +03:00

No more need for wasmer and its limitations now! We can host about anything, and route freely between services under our dns.

The domain:

1. [000 Get a domain name to publish in for self-hosting](../../../../../../archived/2026-05-21_2025/proj/002%20obsidian-sourced-website/tasks/2025/000%20Get%20a%20domain%20name%20to%20publish%20in%20for%20self-hosting/000%20Get%20a%20domain%20name%20to%20publish%20in%20for%20self-hosting.md)
1. www.deltatraced.com

https://dns.studio/dns-records/

* `A Record (Address Record)`: Points a domain to an IPv4 address
* `CNAME Record (Canonical Name)`: Used to point a domain to `www`.

https://ubuntu.com/server/docs/how-to/networking/install-dns/

https://linuxvox.com/blog/setup-dns-server-ubuntu/

* `Forward Zones`: ip $\to$ name
* `Reverse Zones`: name $\to$ ip

````sh
# in vps
sudo apt install bind9 bind9utils bind9-doc
sudo apt install dnsutils
````

Taking `{FQDN}` (fully qualified domain name) to mean something like `example.com`.

Taking `{VPSIP}` to resolve to the VPS's IP address.

Given an IPV4 in the form `A.B.C.D`,

* We take `{VPSIP~..-1}` to be `A.B.C`
* We take `{VPSIP~..-1R}`  to be `C.B.A`
* We take `{VPSIP~-1}` to be `D`.

We want to be a primary server to do the hosting. Edit `/etc/bind/named.conf.local`:

````
zone "{FQDN}" {
	type master;
	file "/etc/bind/zones/db.{FQDN}";
};
````

````sh
# in vps
mkdir zones
sudo mkdir /etc/bind/zones
sudo touch /etc/bind/zones/db.{FQDN}
````

Edit `/etc/bind/zones/db.{FQDN}`:

````
$TTL    604800
@       IN      SOA     ns1.{FQDN}.     admin.{FQDN}. (
                              2         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      ns1.{FQDN}.
ns1     IN      A       {VPSIP}
www     IN      A       {VPSIP}
````

For reverse zone, edit `/etc/bind/named.conf.local`:

````
zone "{VPSIP~..-1R}.in-addr.arpa" {
	type master;
	file "/etc/bind/zones/db.{VPSIP~..-1}";
};
````

Create and Edit `/etc/bind/zones/db.{VPSIP~..-1}`:

````
$TTL    604800
@       IN      SOA     ns1.{FQDN}.     admin.{FQDN}. (
                              2         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      ns1.{FQDN}.
{VPSIP~-1}     IN      PTR     ns1.{FQDN}.
{VPSIP~-1}     IN      PTR     www.{FQDN}.
````

Check `BIN9` config for syntax errors.

For `named-checkconf`, No output = pass.

For `named-checkzone`, it should output `OK`.

````sh
# in vps
sudo named-checkconf
sudo named-checkzone {FQDN} /etc/bind/zones/db.{FQDN}
sudo named-checkzone {VPSIP~..-1R}.in-addr.arpa /etc/bind/zones/db.{VPSIP~..-1}
````

Restart `BIND9` service:

````sh
# in vps
sudo systemctl restart bind9
````

Allow the DNS server through the firewall:

````sh
# in vps
sudo ufw allow 53/tcp
sudo ufw allow 53/udp
````

Test the DNS server:

````sh
# in vps
nslookup www.{FQDN} {VPSIP}
````

This still won't work outside:

````sh
nslookup www.{FQDN} {VPSIP}
````

https://www.digitalocean.com/community/tutorials/iptables-essentials-common-firewall-rules-and-commands

This will replace `ufw`:

````sh
# in vps
sudo apt install iptables-persistent
````

This saves the rules (IPv4 and IPv6) to `/etc/iptables/rules.v4` and `/etc/iptables/rules.v6`.

We take `{interface}` to be the associated interface like `eth0` for the `{VPSIP}`, check with `ifconfig`.

Let's also serve a test website to test with:

````sh
# in vps
python3 -m http.server
````

This will serve a simple listing of the current directory files at http://0.0.0.0:8000/

Also, the work we've done so far configures an internal domain name server, but we do need to give our `{VPSIP}`

## Iteration 1.0 Branch Reason

It might be later on we need to do this DNS configuration when we want to differentiate subdomains, but our dns name provider is the one responsible for the primary IP \<-> domain name correspondence, so the content is out of order and we branched.
