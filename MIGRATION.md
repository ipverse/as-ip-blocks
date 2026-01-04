# Migration guide

This repository will be renamed from **asn-ip** to **as-ip-blocks**.

## Migration steps

1. If only consuming raw TXT files: just update the download URL, no code changes needed
2. Update repository URLs in your code from `ipverse/asn-ip` to `ipverse/as-ip-blocks`
3. If parsing JSON: handle the `metadata` nested object and `subnets` → `prefixes` rename

## What's changing

### Repository name
- Old: `ipverse/asn-ip`
- New: `ipverse/as-ip-blocks`

### File structure

The file structure remains the same:
```
as/1/aggregated.json
as/1/ipv4-aggregated.txt
as/1/ipv6-aggregated.txt
```

### JSON format

**Old:**
```json
{
  "asn": 1,
  "handle": "LVLT-1",
  "description": "Level 3 Parent LLC",
  "subnets": {
    "ipv4": [
      "177.75.88.0/24",
      "177.75.90.0/24"
    ],
    "ipv6": []
  }
}
```

**New:**
```json
{
  "asn": 1,
  "metadata": {
    "handle": "LVLT-1",
    "description": "Level 3 Parent LLC",
    "origin": "authoritative",
    "lastAnnounced": "2026-01-03"
  },
  "prefixes": {
    "ipv4": [
      "177.75.88.0/24",
      "177.75.90.0/24"
    ],
    "ipv6": []
  }
}
```

**Changes:**
- `subnets` renamed to `prefixes`
- `handle`, `description` now nested under `metadata` object
- Added `origin` field in metadata (values: `authoritative`, `inferred`, `overlaid`, `none`)
- Added `lastAnnounced` field in metadata (date or `null` for older files)

### TXT format

The TXT format remains the same with separate IPv4 and IPv6 files:

ipv4-aggregated.txt:
```
# AS1 (LVLT-1)
# Level 3 Parent LLC
#
177.75.88.0/24
177.75.90.0/24
```

ipv6-aggregated.txt:
```
# AS1 (LVLT-1)
# Level 3 Parent LLC
#
2a01:1234::/32
```

### URL changes

**Old:**
```
https://raw.githubusercontent.com/ipverse/asn-ip/master/as/1/aggregated.json
https://raw.githubusercontent.com/ipverse/asn-ip/master/as/1/ipv4-aggregated.txt
https://raw.githubusercontent.com/ipverse/asn-ip/master/as/1/ipv6-aggregated.txt
```

**New:**
```
https://raw.githubusercontent.com/ipverse/as-ip-blocks/master/as/1/aggregated.json
https://raw.githubusercontent.com/ipverse/as-ip-blocks/master/as/1/ipv4-aggregated.txt
https://raw.githubusercontent.com/ipverse/as-ip-blocks/master/as/1/ipv6-aggregated.txt
```