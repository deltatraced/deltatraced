# 1 Journal

2025-08-12 Wk 33 Tue - 17:08

From [hostinger domain name tutorial](https://www.hostinger.com/tutorials/what-is-a-domain-name),

Something like www.google.com is a domain name. It's broken into 3 levels:

* Top-Level Domain (TLD)

In our case, `www.google.com` -> `.com`.

It's also referred to as the domain name extension.

`.com` is very popular. These can have regulatory meanings like `.org`, `.uk`, `.gov`...

You can find a list of different top level domains in [wikipedia](https://en.wikipedia.org/wiki/List_of_Internet_top-level_domains) -> [iana.org domain db](https://www.iana.org/domains/root/db).

* Second-Level Domain (SLD)

In our case, `www.google.com` -> `google`.

* Third-Level Domain (subdomain)

This is the `www` in `www.google.com`, but it also could be the `howtos` in `howtos.mywebsite.com`

2025-08-12 Wk 33 Tue - 17:16

A Domain Name System (DNS) server needs to convert URLs like `www.google.com` into specific IP addresses for routing. A request  is made to a DNS server to convert a domain name to an IP address.

We can look up information on domain names using [ICANN lookup](https://lookup.icann.org/en/lookup).

2025-08-12 Wk 33 Tue - 17:30

Let's say we have a website with the URL `https://howtos.mywebsite.com/howtos/1234` to load a specific `HowTo` number 1234. The domain name part of this is `howtos.mywebsite.com`. It's using the HTTPS protocol, and has the page path `/howtos/1234`.

2025-08-12 Wk 33 Tue - 17:35

There are services that allow us to buy domain names like [hostinger](https://www.hostinger.com/domain-name-search).

2025-08-12 Wk 33 Tue - 17:36

So now we know that a DNS is a domain name server that is able to fetch domain names. So what's DDNS?

From [AWS post on dynamic dns](https://aws.amazon.com/what-is/dynamic-dns/),

so that stands for Dynamics DNS (DDNS).

A DDNS allows for dynamic rouiting of IP addresses to the same domain name.

This mentions DHCP use of reassignment of IP addresses to devices on a network, which could lead to issues with static DNS that expects static IPs.
