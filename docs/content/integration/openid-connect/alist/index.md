---
title: "Alist"
description: "Integrating Alist with the Authelia OpenID Connect 1.0 Provider."
summary: ""
date: 2025-05-15T21:18:09+11:00
draft: false
images: []
weight: 620
toc: true
support:
  level: community
  versions: true
  integration: true
seo:
  title: "" # custom title (optional)
  description: "" # custom description (recommended)
  canonical: "" # custom canonical URL (optional)
  noindex: false # false (default) or true
---

## Tested Versions

- [Authelia]
  - [v4.38.0](https://github.com/authelia/authelia/releases/tag/v4.38.0)
- [Alist]
  - [v3.44.0](https://github.com/AlistGo/alist/releases/tag/v3.44.0)

{{% oidc-common %}}

### Assumptions

This example makes the following assumptions:

- __Application Root URL:__ `https://alist.{{< sitevar name="domain" nojs="example.com" >}}/`
- __Authelia Root URL:__ `https://{{< sitevar name="subdomain-authelia" nojs="auth" >}}.{{< sitevar name="domain" nojs="example.com" >}}/`
- __Client ID:__ `alist`
- __Client Secret:__ `insecure_secret`

Some of the values presented in this guide can automatically be replaced with documentation variables.

{{< sitevar-preferences >}}

## Configuration

### Authelia

The following YAML configuration is an example __Authelia__ [client configuration] for use with [Alist] which will
operate with the application example:

```yaml {title="configuration.yml"}
identity_providers:
  oidc:
    ## The other portions of the mandatory OpenID Connect 1.0 configuration go here.
    ## See: https://www.authelia.com/c/oidc
    clients:
      - client_id: 'alist'
        client_name: 'Alist'
        client_secret: '$pbkdf2-sha512$310000$c8p78n7pUMln0jzvd4aK4Q$JNRBzwAo0ek5qKn50cFzzvE9RXV88h1wJn5KGiHrD0YKtZaR/nCb2CJPOsKaPK0hjf.9yHxzQGZziziccp6Yng'  # The digest of 'insecure_secret'.
        public: false
        authorization_policy: 'two_factor'
        redirect_uris:
          - 'https://alist.{{< sitevar name="domain" nojs="example.com" >}}/api/auth/sso_callback?method=sso_get_token'
          - 'https://alist.{{< sitevar name="domain" nojs="example.com" >}}/api/auth/sso_callback?method=get_sso_id'
        scopes:
          - 'openid'
          - 'profile'
          - 'email'
        userinfo_signed_response_alg: 'none'
        token_endpoint_auth_method: 'client_secret_post'
```

### Application

To configure [Alist] there is one method, using the [Web GUI](#web-gui).

#### Web GUI

To configure [Alist] to utilize Authelia as an [OpenID Connect 1.0] Provider, use the following instructions:

1. Go to the Manage menu, choose `Settings` and `Single Sign-on`.
2. Enable `Sso login enabled`.
3. Choose `OIDC` as the `Sso login platform`
4. Configure the following options:
   - Sso client id: `alist`
   - Sso client secret: `insecure_secret`
   - Sso oidc username key: `preferred_username`
   - Sso endpoint name: `https://{{< sitevar name="subdomain-authelia" nojs="auth" >}}.{{< sitevar name="domain" nojs="example.com" >}}`
   - Sso extra scopes: `openid profile email`

[Authelia]: https://www.authelia.com
[Alist]: https://alistgo.com
[OpenID Connect 1.0]: ../../openid-connect/introduction.md
[client configuration]: ../../../configuration/identity-providers/openid-connect/clients.md
