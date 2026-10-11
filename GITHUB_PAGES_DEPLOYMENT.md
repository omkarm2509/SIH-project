# GitHub Pages deployment notes

This project is configured for Next.js static export (`output: "export"`) and the existing GitHub Actions Pages workflow.

## What changed

- Converted ID-based detail pages to fixed static routes that read IDs from query strings, because GitHub Pages cannot run Next.js dynamic routes at request time.
- Updated navigation links to pass booking/worker IDs in query strings.
- Configured `basePath` from the `actions/configure-pages` output so repository Pages URLs and custom-domain Pages URLs can be built correctly.
- Enabled trailing slashes and unoptimized images for static export.
- Changed Leaflet map imports to client-only dynamic imports to avoid loading Leaflet during server-side prerendering.
- Added `public/.nojekyll` for GitHub Pages.

## Static detail URLs

- Booking detail: `/booking/detail?id=BOOKING_ID`
- Estimate: `/customer/estimate?id=BOOKING_ID`
- Payment: `/customer/payment?id=BOOKING_ID`
- Invoice: `/customer/invoice?id=BOOKING_ID`
- Worker job detail: `/worker/jobs/detail?id=BOOKING_ID`
- Federation request detail: `/federation/requests/detail?id=BOOKING_ID`
- Worker profile: `/workers/profile?workerId=WORKER_ID`

The values are read by client-side code and used to load data from the existing localStorage/demo data.

## GitHub settings checklist

1. Push these changes to the repository's `main` branch (the workflow listens to `main`).
2. In GitHub, open **Settings → Pages** and set the publishing source to **GitHub Actions**.
3. Open **Actions**, select **Deploy Next.js site to Pages**, and inspect the latest run.
4. The build job must complete and upload the `out/` directory before the deploy job can publish it.

## Build verification

Run `npm ci` and `npm run build` locally or let GitHub Actions run them. The build could not be executed in the editing environment because dependency installation did not complete there; a successful build has therefore not been claimed.
