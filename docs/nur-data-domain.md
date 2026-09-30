# Nur Data deployment

The shared frontend has a `nur-data` profile in `users/assets/js/client-config.js`.
The `nurdatasub.com` and `www.nurdatasub.com` hostnames select that profile,
which sends browser API requests to `https://app.nurdatasub.com/api` and uses
the public licence identifier `PPT-NUR-DATA`. The existing `mbrdata.com`
hostnames continue selecting the MBR Data profile and API.

## Before publishing

1. Deploy a **separate Nur Data backend** at `https://app.nurdatasub.com`.
   Its `/api` routes must be reachable. Configure its own database, CORS origin
   (`https://nurdatasub.com` and optionally `https://www.nurdatasub.com`), and
   signed licence for `app.nurdatasub.com`. Keep private licence tokens and API
   credentials on that backend, never in this frontend repository.
2. Add Nur Data's support email and WhatsApp number to the public client
   profile and public landing/legal pages. Review the legal policy text with
   the business owner before publication.
3. Publish the shared frontend through a **second Pages deployment** with
   `nurdatasub.com` as that deployment's custom domain. Keep the current
   MBR Data Pages deployment and its `mbrdata.com` setting unchanged. The
   second deployment can build from this same source repository; it does not
   require a separately maintained frontend codebase.
4. After the second Pages site is configured, replace the Namecheap URL
   forwarding for `@` and `www` with the DNS records shown by GitHub Pages.
   Verify HTTPS and both hostnames in the Pages settings.

Do not point `nurdatasub.com` at the current MBR Data Pages deployment: a
GitHub Pages site accepts one custom domain and the current landing page
contains MBR Data content until the public branding pass is complete.
