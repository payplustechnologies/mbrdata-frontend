# Nur Data deployment

The shared frontend has a `nur-data` profile in `users/assets/js/client-config.js`.
The `nurdatasub.com` and `www.nurdatasub.com` hostnames select that profile,
which sends browser API requests to `https://app.nurdatasub.com/api` and uses
the public licence identifier `PPT-NUR-DATA`. The existing `mbrdata.com`
hostnames continue selecting the MBR Data profile and API.

## Deploying the frontend on GitHub Pages

1. Keep the **separate Nur Data backend** at `https://app.nurdatasub.com`.
   Configure its own database, CORS origins (`https://nurdatasub.com` and
   optionally `https://www.nurdatasub.com`), and signed licence for
   `app.nurdatasub.com`. Keep private licence tokens and API credentials on
   that backend, never in this frontend repository.
2. The separate GitHub repository
   `payplustechnologies/nur-data-frontend` is the Pages deployment target. It
   is a *generated deployment target*, not a second source codebase.
   Keep `mbrdata-frontend` as the source of truth for both clients.
3. Generate the Nur Data site from the shared frontend source:

   ```sh
   node scripts/build-client.mjs nur-data
   ```

   The package uses the Nur Data logo and `support@nurdata.com` across static
   pages. Until a Nur Data support number is provided, its public WhatsApp
   links are omitted. Review the legal policy content before publication.
4. Publish the *contents* of `dist/nur-data` to the second repository's
   Pages deployment. Its published root must contain `index.html`, `users/`,
   and `admin/` directly. The build creates the `CNAME` file for
   `nurdatasub.com`. Never publish `.env`, backend code, or private licence
   keys. The MBR Data repository keeps
   `mbrdata.com` as its own custom domain.
5. At Nur Data's DNS provider, replace the apex `@` A record for
   `104.207.79.73` with GitHub Pages' four A records:
   `185.199.108.153`, `185.199.109.153`, `185.199.110.153`, and
   `185.199.111.153`. Point `www` to `payplustechnologies.github.io` with a
   CNAME if that hostname should work too.
   Remove any conflicting A/AAAA/CNAME records for the same hostname. Keep
   `app.nurdatasub.com` pointed to `104.207.79.73` for the separate Laravel
   backend server. Do not change MX/TXT records used for email.
6. Open `https://nurdatasub.com/`, `/users/login/`, and `/admin/`. Confirm
   that the page title says Nur Data and browser API calls go to
   `https://app.nurdatasub.com/api`.

The MBR Data Pages site and its `mbrdata.com` domain stay unchanged. A future
Nur Data release is built from the same source and published to Nur Data's
Pages repository. A single GitHub Pages repository cannot serve two different
custom domains, so each client needs a separate deployment target even though
the editable source remains shared.
