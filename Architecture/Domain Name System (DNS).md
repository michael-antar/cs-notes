Translates human-readable domain names into machine-readable *records* (i.e., [[IP Addresses|IP addresses]]).

If not first found in the *cache*, the process of finding the record follows *4 layers of servers*:
1. The browser first asks the **Recursor** (usually provided by your [[Internet Service Provider|ISP]]) to find an IP address. The Recursor is what moves through the following layers.
2. The Recursor then asks a **Root Nameserver** for the location of the "Top Level Domain" server for that specific domain extension.
3. The Root Nameserver provides the address of the **Top Level Domain (TLD) Server**. Each manages a specific extension.
4. The TLD Server finally points to an **Authoritative Nameserver** that holds the specific records for that domain.
## Record Types
Any domain can hold almost any record type, and often hold many types simultaneously. 

| Record Type | Description                                                                                                                                         | Example Use                                                                 |
| ----------- | --------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------- |
| **A**       | **Address**: IPv4 address                                                                                                                           | `example.com` -> `93.184.216.3                                              |
| **AAAA**    | **Quad A**: IPv6 address                                                                                                                            | `example.com` -> `2606:2800:220.                                            |
| **CNAME**   | **Canonical Name**: An alias to another domain                                                                                                      | `www.example.com` -> `example.                                              |
| **MX**      | **Mail Exchange**: Directs emails to a mail server                                                                                                  | `user@example.com` goes to Gmail se                                      s  |
| **TXT**     | **Text**: Stores text notes, often used for verification (security)                                                                                 | Verifying you own a domain for Goog                                     AWS |
| **NS**      | **Nameserver**: Lists which servers are authoritative for a do TLD server  says to use `NS ns1.google.com` to find the IP address for google e.com` |                                                                             |

The one exception is that if a domain has a CNAME (alias), it cannot have any other record types for that same domain.

When a browser is looking for a website, it will only be looking for the type A or type AAAA record type for that domain. If it finds a CNAME for a domain, it will restart the search using that domain name. If it cannot find any of those, regardless of if the domain has other types listed, it will throw an error like `ERR_NAME_NOT_RESOLVED`
## Caching
**Caching** prevents most queries from even beginning. They typically store between *50-1,000 sites* at any given time *within RAM*. 

Although they can store many more, the biggest limiting factor is the **Time-To-Live (TTL)** of every DNS record. Most websites set this between *5 minutes - 24 hours*.

In order to prevent the cache from eating too much RAM, operating systems impose a *hard limit* of around 1,000-2,000 entries, before it starts deleting the oldest sites.

| Cache Location   | Typical Capacity   | Typical Duration (TTL) |
| ---------------- | ------------------ | ---------------------- |
| Web Browser      | ~100-1,000 sites   | ~1 minute              |
| Operating System | ~1,000-5,000 sites | 1 hour - 24 hours      |
| Home Router      | ~10,000+ sites     | 24 hours - 48 hours    |
## Troubleshooting
DNS is often an issue because it is critical infrastructure that is *invisible*, *distributed*, and *aggressively cached*.

**Distributed Caching** (*"It works for me"* problem): Because DNS caches exist at every layer, two engineers sitting next to each other can see two different realities.
- If you update an API endpoint from an old IP to a new one, you can clear your cache and see it working, but a coworker can still be holding the old IP for another 3 hours because of the TTL. 

**Propagation Delay**: Important to remember that DNS is *eventual consistency*. If you make a mistake on a DNS record and publish it, the mistake can be cached around the world for 24 hours (if the TTL was high). Even if you fix it within 5 minutes, the "bad" records will stay there until the TTL runs out. You just have to wait it out.

**Error Messages**: Error messages rarely say "DNS error", and often blames the application or the network.

**Trailing Dot**: You *must* include the final dot when setting `CNAME target.example.com.` or else it will append the origin domain name and result as `api.example.com` $\to$ `target.example.com.example.com`

**Service Discovery**: When using something like Kubernetes, services find each other via internal DNS. If the internal CoreDNS pod crashes or becomes overloaded, the connection between them services are all burnt.