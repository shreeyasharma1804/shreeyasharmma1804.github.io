
## DNS

### DNS Overview

```mermaid
graph LR User[User<br/>Device]  --> Recursive[Caching/Recursive<br/>Name Server] Recursive --> Root[13 Root<br/>Servers] Root --> TLD[TLD Name Servers<br/>.com servers] TLD --> Auth[Authoritative<br/>Name Servers<br/>Thousands of servers] URL[www.facebook.com<br/><br/>Top Level Domain: .com] style User fill:#6ba3d4,stroke:#333,stroke-width:2px style Recursive fill:#95a5a6,stroke:#333,stroke-width:2px style Root fill:#95a5a6,stroke:#333,stroke-width:2px style TLD fill:#95a5a6,stroke:#333,stroke-width:2px style Auth fill:#95a5a6,stroke:#333,stroke-width:2px style URL fill:#fff,stroke:#333,stroke-width:1px
```
### DNS Configuration
```
;; SOA Record
shreeya.online	3600	IN	SOA	carrera.ns.cloudflare.com. dns.cloudflare.com. 2051573275 10000 2400 604800 3600
```
- shreeya.online: domain name
- 3600: TTL in cache
- IN: Internet Class
- SOA: Type
- carrera.ns.cloudflare.com: Primary authoritative nameserver which contains the zone file.
- 2051573275: Serial number of the zone file.
- 10000: How often secondary nameservers check for updates in this file.
- 2400: If refresh fails, secondary NS waits this long before retrying.
- 604800: If secondary NS can't contact primary for this long, it stops answering queries.
- 3600: Minimum TTL and default TTL for NXDOMAIN responses

```
;; NS Records
shreeya.online.	86400	IN	NS	carrera.ns.cloudflare.com.
shreeya.online.	86400	IN	NS	hassan.ns.cloudflare.com.
```
- 86400: TTL
- NS: Name Servers 

```
;; A Records
shreeya.online.	1	IN	A	84.32.84.32 ; cf_tags=cf-proxied:true
```
```
;; CNAME Records
python.shreeya.online.	1	IN	CNAME	d8d720b4-b463-4f6f-9acf-2fbfc2dee800.cfargotunnel.com. ; cf_tags=cf-proxied:true
www.shreeya.online.	1	IN	CNAME	shreeya.online. ; cf_tags=cf-proxied:true
```
```
;; CAA Records
shreeya.online.	1	IN	CAA	0 issuewild "letsencrypt.org"
shreeya.online.	1	IN	CAA	0 issue "letsencrypt.org"
```
CAA(Certification Authority Authorization) records act as a whitelist of authorized 
*issuewild*: The CA can issue certs for wildcards (*.shreeya.online)

```
;; PTR Records
shreeya.online.	1	IN	PTR	www.shreeya.online.
```
PTR records are used to define the results of reverse dns lookup queries

**Overview**

1. User's device → Recursive resolver: "What's the IP for shreeya.online?"
2. Recursive resolver → Root servers: "Where's .online?"
3. Root servers → ".online TLD servers handle this"
4. Recursive resolver → .online TLD: "Where's shreeya.online?"
5. TLD servers → "Ask carrera.ns.cloudflare.com or hassan.ns.cloudflare.com"
6. Recursive resolver → carrera.ns.cloudflare.com: "What's the IP?"
7. carrera.ns.cloudflare.com → Returns: "84.32.84.32"
8. User's device connects to 84.32.84.32

### DNS Load Balancing configuration

This configuration returns one of the CNAME from the servers, and performs regular health checks
```
domains:
  - <domain_name>
type: round_robin, geo
servers:
- data: <location specific domain>
  ttl:
  type: CNAME
  healthcheck:
    url: <healthcheck url>
    response_codes: [200]
```


