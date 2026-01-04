# Extension Publish Action

GitHub Action to automatically build and publish browser extensions to Chrome Web Store and Firefox Add-ons.

## Usage

```yaml
name: Publish Extensions

on:
  push:
    branches:
      - main

jobs:
  publish:
    runs-on: ubuntu-latest
    permissions:
      contents: write
      id-token: write

    steps:
      - uses: YOUR_USERNAME/extension-publish-action@v1
        with:
          chrome-client-id: ${{ secrets.CHROME_CLIENT_ID }}
          chrome-client-secret: ${{ secrets.CHROME_CLIENT_SECRET }}
          chrome-refresh-token: ${{ secrets.CHROME_REFRESH_TOKEN }}
          chrome-publisher-id: ${{ secrets.CHROME_PUBLISHER_ID }}
          chrome-extension-id: ${{ secrets.CHROME_EXTENSION_ID }}
          firefox-jwt-issuer: ${{ secrets.FIREFOX_JWT_ISSUER }}
          firefox-jwt-secret: ${{ secrets.FIREFOX_JWT_SECRET }}
```

## Required Inputs

At least one of Chrome or Firefox credentials must be provided.

### Chrome Web Store (all required if publishing to Chrome)
- `chrome-client-id` - Chrome Web Store OAuth Client ID
- `chrome-client-secret` - Chrome Web Store OAuth Client Secret
- `chrome-refresh-token` - Chrome Web Store OAuth Refresh Token
- `chrome-publisher-id` - Chrome Web Store Publisher ID
- `chrome-extension-id` - Chrome Web Store Extension ID

### Firefox Add-ons (all required if publishing to Firefox)
- `firefox-jwt-issuer` - Firefox JWT Issuer
- `firefox-jwt-secret` - Firefox JWT Secret

## Optional Inputs

- `node-version` - Node.js version (default: `20`)
- `pnpm-version` - pnpm version (default: `9`)
- `manifest-path` - Path to manifest.json (default: `src/manifest.json`)
- `build-chrome-command` - Build command for Chrome (default: `pnpm run build:chrome`)
- `build-firefox-command` - Build command for Firefox (default: `pnpm run build:firefox`)
- `chrome-zip-path` - Chrome extension zip path (default: `dist/chrome-extension.zip`)
- `firefox-zip-path` - Firefox extension zip path (default: `dist/firefox-extension.zip`)
- `firefox-guid` - Firefox Add-on GUID (auto-detected from manifest if not provided)
- `create-release` - Create GitHub release (default: `true`)
- `github-token` - GitHub token (default: `${{ github.token }}`)

## How It Works

1. Checks version from manifest.json
2. Compares with latest Git tag
3. If version is newer, builds extensions (Chrome and/or Firefox based on provided credentials)
4. Publishes to Chrome Web Store (if Chrome credentials provided)
5. Publishes to Firefox Add-ons (if Firefox credentials provided)
6. Creates Git tag v{version}
7. Creates GitHub release with extension artifacts

## Requirements

- manifest.json with version field
- pnpm package manager
- Build scripts: build:chrome and/or build:firefox (depending on which stores you're publishing to)
- At least one set of credentials: Chrome Web Store API credentials OR Firefox Add-ons API credentials

## Privacy Policy

This action does not collect, store, or process any Personal Data. All credentials (Chrome Web Store API credentials, Firefox Add-ons API credentials) are provided by the user via GitHub Secrets and are used solely for the purpose of publishing extensions to their respective stores. The action does not transmit any data to third parties except for the necessary API calls to Chrome Web Store and Firefox Add-ons for publishing purposes. No user data, analytics, or tracking information is collected or stored by this action.

## License

MIT License - see [LICENSE](LICENSE) file for details.
