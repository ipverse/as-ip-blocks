# as-ip-blocks (formerly asn-ip)

> **📢 New and improved:** This repo has been upgraded with a clearer name (`as-ip-blocks`) and enhanced JSON format. 
> Check out [MIGRATION.md](MIGRATION.md) for the quick changes needed (plaintext files are unchanged).

## 🔍 Try it online

Look up any AS or IP prefix at **[Lens by ipverse](https://lens.ipverse.net)** to explore announced prefixes and metadata.

## Overview

Daily-updated datasets of autonomous systems (AS) with active BGP prefix announcements.
Only includes AS that have announced at least one prefix within the last 90 days.
No APIs, no databases - just simple file downloads.

Each AS gets its own directory with aggregated IPv4 and IPv6 prefixes in both JSON and plaintext formats.
Prefixes are aggregated (adjacent and overlapping CIDR blocks are merged into larger blocks where possible). 
Perfect for firewall rules, network analysis, or tracking what IP ranges belong to specific organizations.
Git history lets you see how an AS's announcements change over time.

## Update notes

- **2026-02-08**: Added `category` and `networkRole` fields to JSON metadata
- **2026-01-17**: Added bulk download archive in [releases](https://github.com/ipverse/as-ip-blocks/releases/latest)
- **2026-01-08**: AS directories are now removed if no prefixes have been announced in the last 90 days. Historical metadata for removed AS may still be available in [as-metadata](https://github.com/ipverse/as-metadata).
- **2026-01-05**: Removed `lastAnnounced` field to reduce git delta size. This field is still available in [as-metadata](https://github.com/ipverse/as-metadata).
- **2026-01-03**: Repository renamed to `as-ip-blocks`, JSON format changed (`subnets` → `prefixes`, metadata nested). See [MIGRATION.md](MIGRATION.md) for details.
- 2025-08-03: Removed opinionated handle cleanup
- 2023-09-03: Removed PEM certificates from description field

## Available formats

JSON and plaintext

**JSON format** (includes both IPv4 and IPv6):
```json
{
  "asn": 1234,
  "metadata": {
    "handle": "FORTUM",
    "description": "Fortum",
    "countryCode": "FI",
    "country": "Finland",
    "origin": "authoritative",
    "category": "business",
    "networkRole": "stub"
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

**Plaintext format** (AS1234 IPv4):
```
# AS1234 (FORTUM)
# Fortum
#
132.171.0.0/16
137.96.0.0/16
193.110.32.0/21
```

**Plaintext format** (AS1234 IPv6):
```
# AS1234 (FORTUM)
# Fortum
#
2405:1800::/32
```

**Metadata fields:**
- **countryCode**: ISO 3166-1 alpha-2 country code (may be `null` if unknown)
- **country**: Country name in English (may be `null` if unknown)
- **origin**: Metadata source indicator
  - `authoritative`: From authoritative source
  - `inferred`: Inferred from routing information; may be inaccurate
  - `overlaid`: Metadata overlay from [as-overlay](https://github.com/ipverse/as-overlay) applied
  - `none`: No metadata available
- **category**: AS classification (`isp`, `hosting`, `business`, `education_research`, `government`); `null` if unclassified
- **networkRole**: Network role (`tier1_transit`, `major_transit`, `midsize_transit`, `access_provider`, `content_network`, `stub`); `null` if unknown

## How to use

Download the announced prefixes for a specific autonomous system:

**AS1234 in JSON format:**
```bash
curl https://raw.githubusercontent.com/ipverse/as-ip-blocks/master/as/1234/aggregated.json
```

**AS1234 IPv4 addresses:**
```bash
curl https://raw.githubusercontent.com/ipverse/as-ip-blocks/master/as/1234/ipv4-aggregated.txt
```

**AS1234 IPv6 addresses:**
```bash
curl https://raw.githubusercontent.com/ipverse/as-ip-blocks/master/as/1234/ipv6-aggregated.txt
```

### Bulk download

Download all AS data in a single archive from the [latest release](https://github.com/ipverse/as-ip-blocks/releases/latest):
```bash
curl -LO https://github.com/ipverse/as-ip-blocks/releases/latest/download/as-ip-blocks.tar.gz
tar -xzf as-ip-blocks.tar.gz
```

For AS metadata (ASN, handle, description, country code), see [as-metadata](https://github.com/ipverse/as-metadata).

### Firewall integration

If you plan to use the routing data for firewalling purposes, have a look at:

- [ipset-blacklist](https://github.com/trick77/ipset-blacklist) - ipset/iptables based Bash script, IPv4 only
- [ipverse-tools-crowdsec](https://github.com/ipverse/tools/blob/main/crowdsec/README.md) - Ban prefixes using Crowdsec's `cscli` command

## Use cases
- Block entire AS at the firewall (goodbye spam-friendly hosting providers)
- Check what routes an AS is announcing (Git history lets you track changes over time)
- Network research and statistical analysis
- Threat hunting and security research
- Figure out which IPs belong to a specific organization
- Pretty much anything where you need to map ASNs to their announced prefixes

## Related projects

- **[as-metadata](https://github.com/ipverse/as-metadata)**: Complete AS metadata including `lastAnnounced` timestamps and additional fields not included in the per-AS files here.
- **[as-overlay](https://github.com/ipverse/as-overlay)**: Autonomous system metadata overlays that supplement and enhance the authoritative data in this repository. When overlay data is applied, entries will have an `origin` value of `overlaid` in the JSON format.

## Questions or issues?

Head over to the [feedback repository](https://github.com/ipverse/feedback) if you have questions, issues, or suggestions.

## License

This data is released under [CC0 1.0 Universal](LICENSE).