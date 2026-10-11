# Base44 Setup Notes

## Project
Static single-page landing site for Kaylee Studio. One self-contained `index.html` with embedded base64 fonts — no build step, no backend, no external runtime requests.

## Running
`docker compose -f docker-compose.base44.yml up -d` — nginx:alpine serves `index.html` on host port 3000.

## Notes
- The contact form posts to Netlify Forms via `fetch`. This only works when deployed to Netlify; in the sandbox preview the POST will fail silently and the in-page confirmation still shows (by design — the footer email is the fallback).
- No secrets or environment variables are required.
