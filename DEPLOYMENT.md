# Deployment contract

This export has one UI implementation: `web/`, built by Vite into `build/vite-web`. Choose a host that can serve that static directory and run a server-side function route for `/api/v1/*`.

The function reads the server-only `NLI_CONTROLLER_ORIGIN` setting and forwards the incoming HTTP method, path, query, headers, cookies, and body to the controller. It returns the controller status, response headers, and body to the same-origin browser request. The browser must use same-origin API paths. The controller origin must be a public HTTPS origin reachable by the selected host.

Vercel is configured as the supported preset in `vercel.json`: Vite build output, an API catch-all function, and security headers. For another static/serverless platform, configure its static output and function routing to satisfy the contract above; retain this Vite source and do not create a second frontend implementation. Add platform-specific adapter files only in the dedicated export repository.

Never expose controller credentials, enrollment tokens, encryption keys, Supabase secret keys, or AI provider keys to the browser or deployment build output.
