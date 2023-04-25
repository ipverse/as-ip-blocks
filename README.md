# ipverse-asn-ip

IPv4 and IPv6 networks in CIDR notation (e.g. 100.2.30.0/22) organized by announcing autonomous systems (AS).
All networks are aggregated to save space.
The data is available in both **TXT** plaintext and **JSON** format.  

This is a IPv4 TXT plaintext example for AS1234:
```
# AS1234 (FORTUM)
# Fortum
#
132.171.0.0/16
137.96.0.0/16
193.110.32.0/21
```

And this is a JSON example for the same AS (JSON format includes both, IPv4 and IPv6):
```
{
  "asn": 1234,
  "handle": "FORTUM",
  "description": "Fortum",
  "subnets": {
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

This repository is updated daily by pulling all route prefix announcements from the BGP routing table and merging it with a list of known autonomous systems.

For the list of autonomous systems with their AS number (ASN) and description see [ipverse-asn-info](https://github.com/ipverse/asn-info)

## Use cases
- Firewalling, e.g. to ban all IP addresses from that notorious, Spam-friendly network provider
- Route advertisment check, see if/how the routes of a specific autonomous system are seen (even over time, thanks to Git's changelog)
- Statistical analysis purposes, e.g. the number of public IPv4 addresses currently announced vs unused/unassigned
- OSINT/CTI Cyber Threat Intelligence

## How to use

To download the announced networks for a specific autonomous system (AS1234 IPv4 adresses in this example), try:  
```$ curl https://raw.githubusercontent.com/ipverse/asn-ip/master/as/1234/ipv4-aggregated.txt```

The same for all IPv6 networks from AS1234:  
```$ curl https://raw.githubusercontent.com/ipverse/asn-ip/master/as/1234/ipv6-aggregated.txt```

The data (IPv4 + IPv6 combined) is available in JSON format as well:  
```$ curl https://raw.githubusercontent.com/ipverse/asn-ip/master/as/1234/aggregated.json```

To download the latest autonomous system list which is used enhance the generated route data:  
```$ curl -O https://raw.githubusercontent.com/ipverse/asn-info/master/as.csv```

See [ipverse-asn-info](https://github.com/ipverse/asn-info) for more information on ```as.csv```  

If you plan to use the routing data for firewalling purposes, have a look at:

  - [ipset-blacklistlist](https://github.com/trick77/ipset-blacklist) ipset/iptables based Bash script, IPv4 only
  - [ipverse-tools-crowdsec](https://github.com/ipverse/tools/blob/main/crowdsec/README.md) Ban networks using Crowdsec's `cscli` command
  - [todo](https://localhost) insert link to a popular project with nftables and IPv6 support

## Yeah, but how do I get the ASN for an IP address?

Check out this excellent blog post: https://blog.jiayu.co/2018/10/quick-url-to-asn-lookups/
