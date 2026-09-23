---
context_type: task
status: done
---

Parent: [lan/2026/microproj/002-personal-notes-cloud/task/000 Configure my new VPS/000 Configure my new VPS](../000%20Configure%20my%20new%20VPS.md)

Spawned by: [lan/2026/microproj/002-personal-notes-cloud/task/000 Configure my new VPS/task/000 Configure my deltatraced dns and publish a test page on it publicly](000%20Configure%20my%20deltatraced%20dns%20and%20publish%20a%20test%20page%20on%20it%20publicly.md)

Spawned in: [^spawn-task-4ddedd](000%20Configure%20my%20deltatraced%20dns%20and%20publish%20a%20test%20page%20on%20it%20publicly.md#spawn-task-4ddedd)

# Process

## Setup

````sh
# in vps
sudo apt install iptables-persistent
````

## port forwarding file

````sh
#!/bin/sh

VPSIP=TODO
interface=TODO

r1_protocol=TODO
r1_port_internal=TODO
r1_port_incoming=TODO

port_forward() {
	name=$1
	protocol=$2
	port_internal=$3
	port_incoming=$4

	echo "> $name: iptables prerouting" && \
	sudo iptables \
		-A PREROUTING \
		-t nat \
		-p $protocol \
		-i $interface \
		--dport $port_incoming \
		-j DNAT \
		--to-destination $VPSIP:$port_internal && \
	echo "> $name: iptables postrouting" && \
	sudo iptables \
		-A POSTROUTING \
		-t nat \
		-p $protocol \
		-d $VPSIP \
		--dport $port_internal \
		-j MASQUERADE && \
	echo "> $name: iptables forward" && \
	sudo iptables \
		-A FORWARD \
		-p $protocol \
		-d $VPSIP \
		--dport $port_internal \
		-m state \
		--state NEW,ESTABLISHED,RELATED \
		-j ACCEPT
}

echo "> enable port forwarding" && \
sudo sysctl net.ipv6.conf.$interface.forwarding=1 && \

echo "> flush iptables rules" && \
sudo iptables -F && \
sudo iptables -X && \
sudo netfilter-persistent flush && \

port_forward "test_http" $r1_protocol $r1_port_internal $r1_port_incoming && \

echo "> save iptables rules" && \
sudo netfilter-persistent save
````

# Journal

## Definitions

We take `{FQDN}` (fully qualified domain name) to mean something like `example.com`.

We take `{VPSIP}` to resolve to the VPS's IP address.

We take `{interface}` to be the associated interface like `eth0` for the `{VPSIP}`, check with `ifconfig` or `ip addr`.

We take `{port_incoming}` to be the outward facing port to connect through. For example, `8001`.

We take `{port_internal}` to be the internal port served on. For example, `8080`.

## Port forward a test http server

Now we need to port forward 8080.

Through the VPS provider, allow a firewall rule for `{port_incoming}` on tcp.

https://www.digitalocean.com/community/tutorials/iptables-essentials-common-firewall-rules-and-commands

https://bit.hosting/en/help/network/port-forwarding-nat

This will replace `ufw`:

````sh
# in vps
sudo apt install iptables-persistent
````

This saves the rules (IPv4 and IPv6) to `/etc/iptables/rules.v4` and `/etc/iptables/rules.v6`.

Save with `sudo netfilter-persistent save`

Flush with `sudo netfilter-persistent flush`

List the rules with `sudo iptables -L -n -v`

From https://serverfault.com/a/1017016,

To check whether port forwarding is enabled: `cat /proc/sys/net/ipv4/conf/{interface}/forwarding`

To allow it: `sudo sysctl net.ipv4.conf.{interface}.forwarding=1`

````sh
# in vps > ~/port-forwarding
sudo iptables -F
sudo netfilter-persistent flush
sudo iptables -A PREROUTING -t nat -p tcp -i {interface} --dport {port_incoming} -j DNAT --to-destination {VPSIP}:{port_internal}
sudo iptables -A POSTROUTING -t nat -p tcp -d {VPSIP} --dport {port_internal} -j MASQUERADE
sudo iptables -A FORWARD -p tcp -d {VPSIP} --dport {port_internal} -m state --state NEW,ESTABLISHED,RELATED -j ACCEPT
sudo netfilter-persistent save
````
