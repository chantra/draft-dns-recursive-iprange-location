%%%
title = "Recursive Resolver "
docName = "draft-bretelle-dnsop-recursive-iprange-location-latest"
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

This document helps providing uniform solutions to let recursive server operators distribute the list of IP ranges under which their servers are operating as well as possibly location up to the postal code granularity by leveraging [@!RFC8805] format



# Conventions and Definitions

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED", "NOT RECOMMENDED", "MAY", and "OPTIONAL" in this document are to be interpreted as described in BCP 14 [@!RFC2119] [@RFC8174] when, and only when, they appear in all capitals, as shown here.

# Security Considerations

# Acknowledgments

The authors would like to thank the following individuals for their useful input: John Todd.

{backmatter}
