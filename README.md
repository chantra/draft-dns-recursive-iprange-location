#

This is the working area for the individual Internet-Draft, "".

* [Editor's Copy](https://chantra.github.io/draft-dns-recursive-iprange-location/#go.draft-bretelle-dnsop-recursive-iprange-location.html)
* [Individual Draft](https://tools.ietf.org/html/draft-bretelle-dnsop-recursive-iprange-location)
* [Compare Editor's Copy to Individual Draft](https://chantra.github.io/draft-dns-recursive-iprange-location/#go.draft-bretelle-dnsop-recursive-iprange-location.diff)

# Goal

A document to describe a mechanism for recursive resolver operators to distribute the IP blocks their servers are using, possibly with locations.

Those lists can then be used by operators to apply network rules (IP ranges), or Geo-locations based answers (IP ranges + locations)

## Current solutions

Currently, different operators may share the network ranges at which their DNS recursive resolvers operate by making their IP ranges available via a web page, those web pages may or may not be easily consumable by a program, do not use the same format:

* [Google Public DNS](https://developers.google.com/speed/public-dns/faq#locations_of_ip_address_ranges_google_public_dns_uses_to_send_queries)
* [Cloudflare](https://www.cloudflare.com/ips/): not specific to recursive resolvers
* [OpenDNS](https://www.opendns.com/data-center-locations/)
* [AWS](https://ip-ranges.amazonaws.com/ip-ranges.json): not specific to their recursive resolvers

Some share this information over DNS:

* Google: `dig @publicdns.goog locations.publicdns.goog. txt`

Or sometimes, it may just be a CSV document shared over emails.

# Possible solutions

* Using an [underscored name](https://www.iana.org/assignments/dns-parameters/dns-parameters.xhtml#underscored-globally-scoped-dns-node-names), one could query this name under an organization's owned name and either receive a list of IP ranges/locations (ala Google Public DNS), or a URI to a location where to get that list.
* Formats of the lists should be defined so entities consuming those can do so in a uniform way.
* Highlighted by wkumari in DNS OARC's mattermost: IP range holder to list the URL to a Geofeeds serve using the inetnum object [OpsAWG list thread](https://mailarchive.ietf.org/arch/msg/opsawg/cbwW1YqibIV5EoGxDG0ggqlFVt4/) [drafty-mbk-opsawg-finding-geofeeds](https://tools.ietf.org/html/draft-ymbk-opsawg-finding-geofeeds-00)

## Building the Draft

Formatted text and HTML versions of the draft can be built using `make`.

```sh
$ make
```

This requires that you have the necessary software installed.  See
[the instructions](https://github.com/martinthomson/i-d-template/blob/master/doc/SETUP.md).


## Contributing

See the
[guidelines for contributions](https://github.com/chantra/draft-dns-recursive-iprange-location/blob/main/CONTRIBUTING.md).
