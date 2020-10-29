%%%
title = "Recursive Resolvers IP Ranges location distribution and discovery"
docName = "draft-bretelle-dnsop-recursive-iprange-location-01"
abbrev = "dns-recursive-iploc"
category = "std"

stand_alone = "yes"

ipr = "trust200902"
area = "Internet"
workgroup = "dnsop"
keyword = ["Internet-Draft"]

[pi]
toc = "yes"
sortrefs = "yes"
symrefs = "yes"

[[author]]
initials = "E."
surname = "Bretelle"
fullname = "Emmanuel Bretelle"
organization = "Facebook"
  [author.address]
  email = "chantra@fb.com"

%%%

.# Abstract

This document specifies a way for recursive resolvers operators to signal the IP ranges and locations used by their server pools.

{mainmatter}

# Introduction

Big distributed recursive resolver pools tend to be distributed across the world, operating under multiple countries and possibly using IP ranges for which the country is not necessarily perfectly matching the location of the service. This has lead to sub-optimal answers being returned to those server pools. An solution to this problem has been to use EDNS Client Subnet (ECS) [@RFC7871], but this require support
from both the recursive resolvers and the name servers authorities, comes with its own Security Considerations, and increased resources usage.

DNS server operators are commonly receiving spoofed DNS traffic over UDP, common techniques have been to reply with TC bit set to force legitimate clients to use TCP, if the load is still too high, they may start to drop traffic from selected subnets. While this may protect their resources, it has the possibility of denying the service to legitimate resolvers.

So far, operators have resorted to ad-hoc mechanism, ranging from exchanging list by email, providing IP ranges and location via webpages, or specific DNS queries, like [Google Public DNS](https://developers.google.com/speed/public-dns/faq#locations_of_ip_address_ranges_google_public_dns_uses_to_send_queries), or [Cloudflare](https://www.cloudflare.com/ips/), or [OpenDNS](https://www.opendns.com/data-center-locations/). When web pages are available, they are rarely found at consistent locations, neither are they formatted in a uniform way, essentially making name server operators' task rather complicated and brittle.

This document helps providing uniform solutions to let recursive server operators distribute the list of IP ranges under which their servers are operating as well as possibly location up to the postal code granularity by leveraging [@!RFC8805] format.



# Conventions and Definitions

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED", "NOT RECOMMENDED", "MAY", and "OPTIONAL" in this document are to be interpreted as described in BCP 14 [@!RFC2119] [@RFC8174] when, and only when, they appear in all capitals, as shown here.

# Publishing Resolver pool IP ranges

An entity willing to share the IP ranges used by their recursive servers would publish a record under the special name `_rdns.example.com`. The IP ranges can be distributed either using a `TXT` record or `HTTPS` resource record [@!I-D.ietf-dnsop-svcb-https]
An entity can share IP information in 2 ways: via an IP Geolocation Feed, or list of IP ranges in a TXT record.

## TXT Resource Record

An entity that wishes to share the IP ranges they are using with their recursive resolvers can distribute it via a TXT record.

The record is expressed as a single line of text found in the RDATA. Multiple TXT resource records for the same owner name may be permitted.

The record is made of a list of space separated IP ranges with optional comma separated Geolocation. The Geolocation field **MUST** be a 2-letter ISO country code conforming to ISO 3166-1 alpha 2 [@!ISO.3166.1alpha2]. Parsers SHOULD treat this field case-insensitively.

This example illustrate a record without geolocation:
```
_rdns.example.com. 3600 IN TXT "192.0.2.0/24 198.51.100.0/24 2001:db8::/56 "
                               "2001:db8:00:ab00::/56"
```

This example illustrate a record with geolocation information:

```
_rdns.example.com. 3600 IN TXT "192.0.2.0/24,xa 198.51.100.0/24,xb "
                               "2001:db8::/56,xc 2001:db8:00:ab00::/56,xa"
```

As the number of IP ranges increases, the size of the DNS response can become a source for amplification attacks. This is being discussed in (#security).

## HTTPS Resource Record

Another approach is share an IP geolocation feed [@!RFC8805] via an HTTPS Resource Record [@!I-D.ietf-dnsop-svcb-https]. This record has the benefit of providing a format which can provide more granularity if the entity sharing it wishes to, and can scale even when the number of IP ranges increases.

```
_rdns.example.com. 3600 IN HTTPS . (
                              uri=https://foo.example.com/geofeed )
```


# Security Considerations {#security}

Unless the record is DNSSEC-signed [@RFC4033], the answers returned cannot be trusted. In HTTPS Resource Record is requested, the client can possibly trust the content if the URI is within the same zone cut, and HTTPS can authenticate the domain.

When using the TXT Resource Record, the answer returned can quickly become big and the name server operator should aggressively limit the size of the answer it will return to the client, and Truncate it if needed.

# IANA Consideration

## Underscored Node Name

This document updates the IANA registry "Underscored and Globally Scoped DNS Node Names" at https://www.iana.org/assignments/dns-parameters/dns-parameters.xhtml#underscored-globally-scoped-dns-node-names

The following entries have been added to the registry:

~~~ ascii-art
+--------------+----------------+
| RR Type      | HTTPS          |
| Node Name    | _rdns          |
| Reference    | This document  |
+--------------+----------------+

+--------------+----------------+
| RR Type      | TXT            |
| Node Name    | _rdns          |
| Reference    | This document  |
+--------------+----------------+
~~~

## URI DNS Service Parameter

This document adds a parameter to the "Service Binding (SVCB)
Parameter" registry.  The allocation request is TBD, taken from the
to the First Come First Served range.

If present, this parameters indicates the URI template of an IP Geolocation feed. This
is a string encoded as UTF-8 characters.

Name:  uri

~~~ ascii-art
+--------------+-------------------------------+
| SvcParamKey  | TBD                           |
| Meaning      | URI to an IP Geolocation feed |
| Reference    | This document                 |
+--------------+-------------------------------+
~~~

# Acknowledgments

The authors would like to thank the following individuals for their useful input: John Todd.

<reference anchor='ISO.3166.1alpha2' target='http://www.iso.org/iso/home/standards/country_codes/iso-3166-1_decoding_table.htm'>
    <front>
        <title>ISO 3166-1 decoding table</title>
        <author fullname='ISO'>
            <organization>ISO</organization>
        </author>
    </front>
</reference>

{backmatter}


# Document history

## Changes between -00 and -01

* Editorial change: making the title more explicit than "Recursive Resolver"
* Editorial change: Fix examples format to use break lines and fix ascii art table
