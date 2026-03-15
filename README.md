# adamwhiles.com

Personal portfolio and tech blog built with [Astro](https://astro.build) and Tailwind CSS. Deployed to Azure Static Web Apps.

## Development

```bash
npm install
npm run dev
```

## Build

```bash
npm run build
npm run preview  # preview the build locally
```

## Deployment

Pushes to `main` automatically deploy via GitHub Actions to Azure Static Web Apps. Pull requests create preview environments that are cleaned up on close.

Requires `AZURE_STATIC_WEB_APPS_API_TOKEN` set as a GitHub Actions secret.
