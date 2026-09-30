# Nur Data deployment

The shared frontend has a `nur-data` profile in `users/assets/js/client-config.js`.
The `nurdatasub.com` and `www.nurdatasub.com` hostnames select that profile,
which sends browser API requests to `https://app.nurdatasub.com/api` and uses
the public licence identifier `PPT-NUR-DATA`. The existing `mbrdata.com`
hostnames continue selecting the MBR Data profile and API.

## Deploying the frontend on the Nur Data server

1. Keep the **separate Nur Data backend** at `https://app.nurdatasub.com`.
   Configure its own database, CORS origins (`https://nurdatasub.com` and
   optionally `https://www.nurdatasub.com`), and signed licence for
   `app.nurdatasub.com`. Keep private licence tokens and API credentials on
   that backend, never in this frontend repository.
2. Generate a Nur Data package from this shared frontend source:

   ```sh
   node scripts/build-client.mjs nur-data
   cd dist/nur-data
   zip -qr ../nur-data-frontend.zip .
   ```

   The package uses the Nur Data logo and `support@nurdata.com` across static
   pages. Until a Nur Data support number is provided, its public WhatsApp
   links are omitted. Review the legal policy content before publication.
3. In cPanel, open **Domains** and find the document root of
   `nurdatasub.com`. Upload `dist/nur-data-frontend.zip` to that exact folder
   using File Manager, then extract it **there**. `index.html`, `users/`, and
   `admin/` must be directly inside the document root, not inside an extra
   `nur-data` folder. Do not upload the package into `app.nurdatasub.com`,
   which is the Laravel API's document root.
4. Open `https://nurdatasub.com/`, `/users/login/`, and `/admin/`. Confirm
   that the page title says Nur Data and browser API calls go to
   `https://app.nurdatasub.com/api`. If `Index of /` still appears, the files
   were extracted into the wrong directory or the document root is incorrect.

The MBR Data Pages site and its `mbrdata.com` domain stay unchanged. A future
Nur Data release is built from the same source and uploaded to Nur Data's
document root.
