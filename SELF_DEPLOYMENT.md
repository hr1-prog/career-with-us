# Career With Us — Self-Deployment Guide

This package contains the complete source code for the Career With Us recruitment portal. It includes the React frontend, Express/tRPC backend, Drizzle schema and migrations, owner authentication flow, candidate applications, job and blog management, managed image-upload integration, tests, and SEO files.

## Requirements

Use Node.js 20 or newer, pnpm 10 or newer, and a MySQL-compatible database such as MySQL or TiDB. The application is designed to run as one Node.js web process. Install dependencies with `pnpm install`, then run the development server with `pnpm dev` or build and start it with `pnpm build` followed by `pnpm start`.

## Environment variables

Create a deployment-specific `.env` file or configure these variables in your hosting provider. Do not commit the file to source control.

| Variable | Purpose |
|---|---|
| `DATABASE_URL` | MySQL/TiDB connection string |
| `JWT_SECRET` | Secret used to sign authentication sessions |
| `VITE_APP_ID` | Manus OAuth application ID, if using the bundled OAuth flow |
| `OAUTH_SERVER_URL` | OAuth backend URL |
| `VITE_OAUTH_PORTAL_URL` | Frontend OAuth login portal URL |
| `OWNER_OPEN_ID` | Owner identity used for admin access |
| `OWNER_NAME` | Owner display name |
| `BUILT_IN_FORGE_API_URL` | Storage and platform API base URL |
| `BUILT_IN_FORGE_API_KEY` | Server-side platform API credential |
| `VITE_FRONTEND_FORGE_API_URL` | Frontend platform API base URL |
| `VITE_FRONTEND_FORGE_API_KEY` | Frontend platform API credential |
| `VITE_ANALYTICS_ENDPOINT` | Optional analytics endpoint |
| `VITE_ANALYTICS_WEBSITE_ID` | Optional analytics website identifier |

The bundled Manus OAuth and managed-storage integrations require credentials from the corresponding provider. Replace those values with credentials available in your own deployment environment; never reuse or expose private credentials from the development workspace.

## Database setup

After configuring `DATABASE_URL`, generate migrations if the schema changes with `pnpm drizzle-kit generate`. Apply the included migration SQL using your database migration workflow. The current schema includes users, jobs, candidate applications, blog posts, and featured-image metadata.

## Production start

Run `pnpm build` to produce the frontend and server bundles, then run `NODE_ENV=production pnpm start`. Configure your host to route HTTP traffic to the port supplied by the runtime environment. Do not hardcode a production port in the application.

## Admin access

Open `/login` and authenticate through the configured OAuth provider. Only the configured owner identity is allowed to access `/admin`. From the owner workspace, you can create, edit, publish, unpublish, and delete jobs and blog posts, upload blog featured images, and review candidate applications.

## Important deployment notes

The supplied logo is referenced through managed storage. If deploying outside the original platform, replace the logo URL with an asset URL hosted by your own storage provider and update the `LOGO` constant in `client/src/App.tsx`. Likewise, replace the `storagePut` implementation or provide an equivalent object-storage adapter for blog image uploads.

The included `robots.txt` and `sitemap.xml` use the project domain. Update their domain before production launch, and submit the resulting sitemap to your search engine webmaster tools. The site contains no salary or payout fields.

## Verification

Run `pnpm check`, `pnpm test`, and `pnpm build` before deployment. The test suite covers authentication logout, owner access control, job and blog mutation success paths through mocked database helpers, application input and submission behavior, and invalid application rejection.
