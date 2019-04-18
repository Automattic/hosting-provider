# hosting-provider Well-Known Resource Identifier

URI suffix: hosting-provider
Change controller: Automattic Inc. <https://automattic.com>
Specification document(s): https://github.com/Automattic/hosting-provider

## Purpose

The hosting-provider well-known resource endpoint is a lightweight response that can be used to hint at the responsible hosting provider for a given domain or subdomain. It allows hosting providers to indicate that a site is hosted by a specific provider or reseller at the web server level. When combined with hostname and IP checks, this can allow a host or external site to determine with reasonable accuracy which provider account is actively serving a site's content. CDN and DDoS mitigation services should honor this endpoint if it exists to allow backtracking to the original site content source as well.

## Representation

A request for `.well-known/hosting-provider` at a domain or subdomain served by a participating hosting provider is expected to return a response of content-type text/plain containing an unformatted text string. This string can be any of:
- A valid URL for the hosting provider, generally the provider's home page, e.g. https://wordpress.com
- The hosting provider's main domain used for conducting business, e.g. wordpress.com
- The hosting provider's registered business name or text service mark, e.g. WordPress.com
It is possible for sites at a domain and its various subdomains to return different values for this resource if they are hosted with different providers.

A hosting provider that allows resellers to operate may optionally substitute the reseller identifier for its own as a response to requests to this endpoint for sites hosted by the reseller accounts.

## Use Cases

This endpoint provides a positive indicator of the over-the-wire hosting provider when combined with other checks against hostname and IP. Setting this in the server configuration allows the controller of the web process to determine which hosting service or reseller is actually serving the site contents.

Reseller account records may not be reliable as sites can be rehosted simply by changing name servers with the registrar without notifying all possible hosts of the change.

CDN and DDoS mitigation services can also further obscure the hosting provider of content. Setting up such a service for a given domain frequently involves updating the domain name servers to those of the service, rendering hostname lookups unreliable for easily determining the hosting provider of a site.

## Security Considerations

The resource value should not be treated as reliable because bad actors can easily spoof the resource and self-report as a different hosting provider.
