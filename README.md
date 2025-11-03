# ChainVue API Documentation

Beautiful, interactive API documentation powered by [Scalar](https://github.com/scalar/scalar).

## Overview

This repository automatically syncs and displays the ChainVue API documentation from our OpenAPI specification.

- **API Spec Source**: `https://api.chainvue.io/api-docs/openapi.json`
- **Live Docs**: (Will be available after deployment)

## Features

- Modern, beautiful UI with dark mode
- Interactive API playground ("Try it out")
- Searchable documentation
- Automatic updates via GitHub Actions
- Zero-config deployment

## How It Works

1. **Automatic Sync**: GitHub Actions fetches the latest OpenAPI spec every 6 hours
2. **Manual Updates**: Trigger updates manually via GitHub Actions UI
3. **Webhook Support**: External systems can trigger updates via repository dispatch
4. **Auto-Deploy**: Automatically deploys to Vercel on every run

## Setup

### Initial Vercel Deployment

1. **Deploy to Vercel** (one-time setup):
   ```bash
   cd api-docs
   vercel --prod
   ```
   Or use: [![Deploy with Vercel](https://vercel.com/button)](https://vercel.com/new/clone?repository-url=https://github.com/chainvue/api-docs)

2. **Get Vercel Credentials**:
   ```bash
   # Get your Vercel token
   # Go to: https://vercel.com/account/tokens
   # Create a new token

   # Get project details
   vercel link
   cat .vercel/project.json
   ```

   This will show you:
   - `projectId` (your VERCEL_PROJECT_ID)
   - `orgId` (your VERCEL_ORG_ID)

3. **Add GitHub Secrets**:

   Go to: https://github.com/chainvue/api-docs/settings/secrets/actions

   Add these secrets:
   - `VERCEL_TOKEN`: Your Vercel token from step 2
   - `VERCEL_ORG_ID`: Your org ID from `.vercel/project.json`
   - `VERCEL_PROJECT_ID`: Your project ID from `.vercel/project.json`

4. **Test the workflow**:
   - Go to Actions tab
   - Run "Update API Documentation" workflow manually
   - It should fetch the spec and deploy to Vercel

### GitHub Secrets Required

| Secret Name | Description | How to Get |
|-------------|-------------|------------|
| `VERCEL_TOKEN` | Vercel authentication token | https://vercel.com/account/tokens |
| `VERCEL_ORG_ID` | Your Vercel organization ID | From `.vercel/project.json` after `vercel link` |
| `VERCEL_PROJECT_ID` | Your Vercel project ID | From `.vercel/project.json` after `vercel link` |

## Triggering Updates

### Manual Update (GitHub UI)

1. Go to [Actions](../../actions/workflows/update-docs.yml)
2. Click "Run workflow"
3. Optionally add a reason for the update
4. Click "Run workflow" button

### Webhook Trigger (API)

```bash
curl -X POST \
  https://api.github.com/repos/chainvue/api-docs/dispatches \
  -H "Authorization: token YOUR_GITHUB_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{"event_type":"api-updated"}'
```

**Creating a GitHub Token:**
1. Go to GitHub Settings → Developer settings → Personal access tokens → Tokens (classic)
2. Generate new token with `repo` scope
3. Use this token in the webhook call

### Scheduled Updates

The workflow automatically runs every 6 hours to keep docs in sync.

## Local Development

To preview the documentation locally:

```bash
# Clone the repository
git clone https://github.com/chainvue/api-docs.git
cd api-docs

# Serve with any static file server
npx serve .

# Or use Python
python3 -m http.server 8000

# Open http://localhost:8000
```

## Deployment

### Vercel (Recommended)

[![Deploy with Vercel](https://vercel.com/button)](https://vercel.com/new/clone?repository-url=https://github.com/chainvue/api-docs)

```bash
# Install Vercel CLI
npm i -g vercel

# Deploy
vercel --prod
```

### Netlify

[![Deploy to Netlify](https://www.netlify.com/img/deploy/button.svg)](https://app.netlify.com/start/deploy?repository=https://github.com/chainvue/api-docs)

```bash
# Install Netlify CLI
npm i -g netlify-cli

# Deploy
netlify deploy --prod
```

### GitHub Pages

1. Go to repository Settings → Pages
2. Select "Deploy from a branch"
3. Choose `main` branch and `/` (root)
4. Save

Your docs will be available at: `https://chainvue.github.io/api-docs/`

## Customization

### Themes

Edit `index.html` and change the `theme` value:

```javascript
"theme": "purple"  // Options: purple, blue, green, red, orange, etc.
```

### Configuration Options

See [Scalar configuration docs](https://github.com/scalar/scalar#configuration) for all available options:

- `layout`: "modern" or "classic"
- `darkMode`: true/false
- `searchHotKey`: keyboard shortcut for search
- `hiddenClients`: hide specific HTTP clients
- And more...

## File Structure

```
api-docs/
├── .github/
│   └── workflows/
│       └── update-docs.yml    # Automation workflow
├── index.html                 # Scalar documentation page
├── openapi.json              # Cached OpenAPI spec
└── README.md                 # This file
```

## Workflow Details

The GitHub Actions workflow:

1. **Triggers**: Manual, webhook, or scheduled (every 6 hours)
2. **Fetches**: Downloads latest spec from API
3. **Validates**: Ensures the spec is valid JSON
4. **Compares**: Checks for changes
5. **Commits**: Pushes changes if spec updated
6. **Deploys**: Automatically deploys to Vercel (whether changed or not)

## Troubleshooting

### Workflow fails to fetch spec

- Check if `https://api.chainvue.io/api-docs/openapi.json` is accessible
- Verify the API is not rate-limiting GitHub Actions
- Check workflow logs in Actions tab

### Documentation not updating

- Check if the workflow ran successfully
- Verify hosting platform is connected to the repo
- Clear your browser cache

### Webhook not working

- Verify GitHub token has correct permissions (`repo` scope)
- Check token is not expired
- Verify the repository name is correct

### Vercel deployment fails

- Verify all three secrets are set correctly in GitHub
- Check that `VERCEL_TOKEN` is not expired
- Ensure project is linked: run `vercel link` locally
- Check workflow logs for specific error messages
- Verify Vercel project exists and is accessible

## Support

For issues or questions:
- GitHub Issues: [Create an issue](../../issues)
- Scalar Docs: https://github.com/scalar/scalar

## License

MIT
