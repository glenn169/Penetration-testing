# Active Recon

active recon is basically gaining information about the domain or target by directly engaging with it.

DNS (Domain Name System) → it is used to resolve the domain name to the IP address.

What contains on the DNS server ?

A → Resolves domain/hostname to the IPv4 address

AAAA → Resolves domain/hostname to IPv6 address

NS → Reference to the domain nameserver

MX → reference the domain to mailserver

CNAME → Used for domain aliases

TXT → text records

HINFO → Host Information 

SOA → Domain Authority  

SRV → Service Record

PTR → Resolves ip address to an hostname

### DNS Interrogation:

It is the process of enumerating DNS record for a specific domain (like A, AAAA, TXT, NS, MX….etc)

## DNS Zone transferring

- In some cases the admin wants to transfer zone files form one DNS server to another. This process is known as Zone Transfer.
- If it is misconfigured or left unsecured, this functionality can be abused by attackers to copy the zone files from the primary DNS server to another DNS server.
- DNS zone transfer can provide penetration tester with a holistic view of an organizations network layer.
- In some cases organizations Internal n/w addresses may be found on the organizations DNS server.

Practical:

`dnsenum`

1. First use the passive method to find then go for active recon method
2. check for zonetransfer automatically using `dnsenum` 

```bash
dnsenum <domain_name>
```

1. you can sometimes get internal subdomains
2. If you find CNAME pointing to other domain so it means that i is redirecting to other domains

`dig` 

1. dig is similar to `dnsenum` but it gives more beautified results 

```bash
dig axfr @<name_server> <domain_name>

you can find name_server using **dnsrecon** tool
axfr -> is used to check for zone transfer
```

`fierce` 

1. it is a semi-lightweight scanner that helps to locate non-contagious IP space in hostname
