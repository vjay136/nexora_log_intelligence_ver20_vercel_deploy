# Nexora Log Intelligence V20 Web Deployment

This repository contains only the Vite WebApp and its same-origin Vercel API proxy. It excludes the controller/client source, RPM build inputs, Supabase schema, browser tests, and local operator scripts.

This deployment mirror was generated from Nexora Log Intelligence `20.0.0-6`,
source revision `0066a9b017156cf7cb6a78518c883fdb0c0d4e69`.

## Deploy on Vercel

1. Import this repository and keep **Root Directory** set to `./`.
2. Select the supported **Vite** preset. The committed `vercel.json` supplies the install, build, output, function, and security-header settings.
3. Add one server-side environment variable in Production and Preview:
   `NLI_CONTROLLER_ORIGIN=https://<your-stable-public-controller-hostname>`
4. Use a public HTTPS DNS hostname provided by a named Cloudflare Tunnel or another trusted reverse proxy. Do not use an EC2 private DNS name, RFC1918 address, localhost, a port, a path, or a trailing slash.
5. Deploy. The browser calls same-origin `/api/v1/*`; the included serverless function forwards those requests to the controller.

Never add the enrollment token, controller HMAC/encryption keys, Supabase secret keys, AI keys, or `.env*` files to this repository or Vercel browser variables.

## Static and serverless contract

The single application implementation is the Vite build in `web/`; its production files are emitted to `build/vite-web`. A host that serves static files can use that directory for the UI. Authenticated controller access also requires a server-side function at `/api/v1/*` that forwards requests to `NLI_CONTROLLER_ORIGIN` while preserving the request method, path, query, headers, cookies, and response headers. Keep controller credentials and the origin out of browser code. Platform-specific function adapters belong in the deployment repository for that host; do not fork the WebApp source.

See [DEPLOYMENT.md](DEPLOYMENT.md) for the platform-neutral contract and Vercel details.

## Verify locally

Run `npm ci --prefix web --ignore-scripts` and `npm run build`. The output is generated at `build/vite-web`.
