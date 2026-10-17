# Day 1 — Networking Fundamentals

**Study date:** 14 October 2026  
**Status:** Theory notes completed; detailed browser timing breakdown still pending.

## Objective

Understand core networking concepts and observe an HTTP document request in browser DevTools.

## 1. IP addresses

An IP address is used to address a network interface so packets can be routed between networks.

- **IPv4:** 32-bit addresses written as four decimal octets, for example `192.168.1.10`.
- **IPv6:** 128-bit addresses written in hexadecimal groups.
- **Private IP:** Used within private networks; common IPv4 ranges include `10.0.0.0/8`, `172.16.0.0/12`, and `192.168.0.0/16`.
- **Public IP:** Used for addressing across the public internet.
- **Loopback:** `127.0.0.1` for IPv4; commonly accessed through `localhost`.

An IP address does not, by itself, identify a person.

## 2. Ports

A port helps a host direct transport-layer traffic to the appropriate application endpoint. Port numbers range from 0 to 65535, separately for TCP and UDP.

| Port | Common use |
|---|---|
| 22/TCP | SSH |
| 53/UDP and 53/TCP | DNS |
| 80/TCP | HTTP |
| 443/TCP | HTTPS over TLS; HTTP/3 commonly uses QUIC over UDP |
| 25/TCP | SMTP |

These are conventional port assignments, not proof that a service is running.

## 3. TCP vs UDP

**TCP** provides a reliable, ordered byte stream. It uses acknowledgements, retransmission and flow control.

**UDP** sends datagrams without TCP's built-in delivery or ordering guarantees. Applications may implement additional reliability where needed.

Common examples: HTTPS over HTTP/1.1 or HTTP/2 usually uses TCP; DNS commonly uses UDP; QUIC and HTTP/3 use UDP.

Neither protocol is automatically secure simply because of its transport properties.

## 4. TCP three-way handshake

A new TCP connection ordinarily begins with this exchange:

1. **SYN:** Client requests a connection and sends an initial sequence number.
2. **SYN-ACK:** Server acknowledges the client's sequence number and sends its own.
3. **ACK:** Client acknowledges the server's sequence number.

The handshake establishes TCP connection state. It does not encrypt traffic or verify the website's identity; TLS and certificate validation address separate security requirements for HTTPS.

## 5. DNS

The Domain Name System (DNS) lets clients look up records associated with names such as `example.com`.

A resolver may consult caches or query DNS hierarchy servers before returning an answer. Cached answers can avoid a full lookup each time.

Common record types:

- **A:** IPv4 address.
- **AAAA:** IPv6 address.
- **CNAME:** Canonical name alias.
- **MX:** Mail server information.
- **NS:** Authoritative name server information.
- **TXT:** Text records used for purposes such as domain verification and email policies.

DNS lookups can be affected by caches and TTLs. DNSSEC can validate DNS data authenticity; encrypted DNS can protect queries in transit to a supported resolver. Neither guarantees that a destination is safe.

## 6. DHCP

Dynamic Host Configuration Protocol (DHCP) can automatically provide a device with network settings, including an IP address, subnet mask, default gateway and DNS resolver.

A typical DHCPv4 exchange is called **DORA**:

- Discover
- Offer
- Request
- Acknowledge

An address is commonly assigned as a lease for a period rather than permanently.

## 7. NAT

Network Address Translation (NAT) changes network address information as packets pass through a translating device. Home routers commonly use IPv4 port address translation so multiple private devices can share a public IPv4 address.

NAT is not the same as a firewall and should not be treated as a complete security control.

## 8. Firewalls

A firewall applies traffic rules based on properties such as IP addresses, ports, connection state or application-layer content.

- **Packet filtering:** Checks packet attributes such as address and port.
- **Stateful firewall:** Uses tracked connection state when filtering traffic.
- **Web application firewall (WAF):** Filters HTTP traffic to help protect web applications against certain application-layer attacks.

Firewalls are one layer of defence; they do not replace secure coding, authentication, patching or endpoint protection.

## 9. OSI model

| Layer | Name | Examples / concepts |
|---|---|---|
| 7 | Application | HTTP, DNS |
| 6 | Presentation | Data representation and transformations |
| 5 | Session | Session management concepts |
| 4 | Transport | TCP, UDP, ports |
| 3 | Network | IP addressing and routing |
| 2 | Data Link | Ethernet, Wi-Fi link functions, MAC addressing |
| 1 | Physical | Radio signals, cables, electrical and optical signals |

OSI is a conceptual model; real-world protocols do not always map neatly to one layer.

## Practical Lab — Browser DevTools

**Website:** `https://example.com`  
**Tool:** Chrome DevTools → Network  
**Request selected:** Main document (`Doc`)

### Observations from the screenshot

| Field | Observed value |
|---|---|
| HTTP status | `200` |
| Displayed request duration | `65 ms` |
| Displayed transferred size | Approximately `0.5 kB` |

These values are from the main-document request visible in the supplied screenshot. The 65 ms figure is the displayed request duration; it is **not** necessarily the DNS lookup, TCP connection, or TLS negotiation duration.

### Detailed timing values

| Timing phase | Result |
|---|---|
| DNS lookup | Not yet measured |
| Initial connection | Not yet measured |
| SSL/TLS | Not yet measured |
| Waiting for server response (TTFB) | Not yet measured |

To complete this part, select the main document request in DevTools and open its **Timing** tab. Record the displayed values rather than estimating them. Some phases may be absent if a connection was reused or the phase was not separately needed.

## Commands to practise

```bash
# Inspect network interfaces (macOS)
ifconfig

# Send four ICMP echo requests
ping -c 4 1.1.1.1

# Query DNS
nslookup example.com

# Query the IPv4 A record
dig example.com A
```

Record command outputs only after running them. A failed ping does not necessarily mean that internet connectivity is unavailable, because ICMP may be blocked.

## Key Takeaways

- IP addresses support packet addressing and routing; ports help identify application endpoints.
- TCP and UDP provide different transport behaviours.
- The TCP handshake establishes connection state, not encryption.
- DNS lookup results may be served from caches.
- DHCP automates common network configuration.
- NAT translates addresses and sometimes ports; it is not equivalent to a firewall.
- The OSI model is a troubleshooting framework.
- Browser timing measurements need to be observed directly and interpreted in context.

## Sources

- [Cloudflare Learning Center](https://www.cloudflare.com/learning/)
- [Cloudflare: What is DNS?](https://www.cloudflare.com/learning/dns/what-is-dns/)
- [Professor Messer](https://www.professormesser.com/)
- [MDN: HTTP](https://developer.mozilla.org/en-US/docs/Web/HTTP)

## Next Steps

- [ ] Capture the main document's Timing tab and fill in measured values.
- [ ] Run and document the networking commands.
- [ ] Explain TCP's handshake, DNS, and the OSI layers aloud.
