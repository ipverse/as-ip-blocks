# as-ip-blocks (formerly asn-ip)

> **📢 Heads up:** This repo has a new name and the data format has changed. Terribly sorry but if you're using this in production, check out [MIGRATION.md](MIGRATION.md) for what you need to update. The data is provided as-is on a best-effort basis.

Daily-updated IP prefix datasets for every autonomous system, sourced from BGP routing table announcements. No APIs, no databases - just simple file downloads.

Each AS gets its own directory with aggregated IPv4 and IPv6 prefixes in both plaintext and JSON formats. Perfect for firewall rules, network analysis, or tracking what IP ranges belong to specific organizations. Git history lets you see how an AS's announcements change over time.

Available formats: plaintext and JSON

**Plaintext format** (AS1234 IPv4):
```
# AS1234 (FORTUM)
# Fortum
#
132.171.0.0/16
137.96.0.0/16
193.110.32.0/21
```

**JSON format** (includes both IPv4 and IPv6):
```json
{
  "asn": 1234,
  "metadata": {
    "handle": "FORTUM",
    "description": "Fortum",
    "origin": "authoritative"
  },
  "prefixes": {
    "ipv4": [
      "132.171.0.0/16",
      "137.96.0.0/16",
      "193.110.32.0/21"
    ],
    "ipv6": [
      "2405:1800::/32"
    ]
  }
}
```

**Metadata fields:**
- **origin**: Metadata source indicator
  - `authoritative`: From authoritative source
  - `inferred`: Derived from incomplete or missing authoritative data; may be inaccurate
  - `overlaid`: Overlay from [as-overlay](https://github.com/ipverse/as-overlay) applied
  - `none`: No metadata available

For AS metadata (ASN, handle, description, country code) see [as-metadata](https://github.com/ipverse/as-metadata)

## Update notes

- **2026-01-03**: Repository renamed to `as-ip-blocks`, JSON format changed (`subnets` → `prefixes`, metadata nested). See [MIGRATION.md](MIGRATION.md) for details.
- 2025-08-03: Removed opinionated handle cleanup
- 2023-09-03: Removed PEM certificates from description field

## How to use

Download the announced prefixes for a specific autonomous system:

**AS1234 IPv4 addresses:**
```bash
curl https://raw.githubusercontent.com/ipverse/as-ip-blocks/master/as/1234/ipv4-aggregated.txt
```

**AS1234 IPv6 addresses:**
```bash
curl https://raw.githubusercontent.com/ipverse/as-ip-blocks/master/as/1234/ipv6-aggregated.txt
```

**AS1234 combined (IPv4 + IPv6) in JSON format:**
```bash
curl https://raw.githubusercontent.com/ipverse/as-ip-blocks/master/as/1234/aggregated.json
```

For AS metadata (ASN, handle, description, country code), see [as-metadata](https://github.com/ipverse/as-metadata).

### Firewall integration

If you plan to use the routing data for firewalling purposes, have a look at:

- [ipset-blacklist](https://github.com/trick77/ipset-blacklist) - ipset/iptables based Bash script, IPv4 only
- [ipverse-tools-crowdsec](https://github.com/ipverse/tools/blob/main/crowdsec/README.md) - Ban prefixes using Crowdsec's `cscli` command

### How do I get the ASN for an IP address?

Check out this excellent blog post: https://blog.jiayu.co/2018/10/quick-url-to-asn-lookups/

## Use cases
- Block entire AS at the firewall (goodbye spam-friendly hosting providers)
- Check what routes an AS is announcing (Git history lets you track changes over time)
- Network research and statistical analysis
- Threat hunting and security research
- Figure out which IPs belong to a specific organization
- Pretty much anything where you need to map ASNs to their announced prefixes

## Questions or issues?

Head over to the [feedback repo](https://github.com/ipverse/feedback) if you have questions, issues, or suggestions.

## License

This data is released under [CC0 1.0 Universal](LICENSE).