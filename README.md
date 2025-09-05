# bknd: Code-Only

A simple Cloudflare Workers setup using a pure code-only approach. This architecture provides:

-  **Code-First Configuration**: All settings defined directly in TypeScript with full type safety
-  **Environment-Based Secrets**: Sensitive values injected via environment variables

## How It Works

This setup runs in "code" mode only - all configuration is defined in your TypeScript files, with secrets provided through environment variables. No UI adjustments are possible, keeping the deployment simple and secure.

### Architecture

1. **Split Configuration**

   -  `bknd.config.ts` - CLI configuration with platform proxy for Cloudflare resource access
   -  `config.ts` - App configuration that prevents bundling CLI dependencies with your worker

2. **Environment Integration**

   -  Secrets and configuration values come from environment variables
   -  Type-safe access to environment variables through Wrangler types
   -  Static configuration with runtime environment injection

## Scripts

### Development

-  **`dev:cf`** - Cloudflare Workers development server with local environment
-  **`predev`** - Generate TypeScript types before development

### Deployment

-  **`deploy`** - Deploy to Cloudflare Workers production
-  **`preview`** - Test production mode locally with actual Cloudflare environment

### bknd CLI

-  **`bknd`** - Base CLI command with platform proxy and TypeScript support

### Utilities

-  **`typegen`** - Generate Wrangler types for environment variables
-  **`postinstall`** - Auto-setup: generate types and copy required assets

## Quick Start

1. **Install**: `npm install`
2. **Develop**: `npm run dev:cf`
3. **Deploy**: `npm run deploy`

## Configuration

### Code Configuration

All application configuration is defined in `config.ts`. The example includes:

-  **Data Models**: Entity definitions using bknd's schema builder
-  **Media Storage**: S3-compatible storage configuration
-  **Mode**: Always set to "code" for static configuration

### Environment Variables

Secrets and environment-specific values are injected via environment variables:

```typescript
// Example from config.ts
media: {
  adapter: {
    type: "s3",
    config: {
      access_key: env.S3_ACCESS_KEY,
      secret_access_key: env.S3_SECRET_ACCESS_KEY,
      url: env.S3_URL,
    },
  },
},
```

## Deployment Setup

### Before Deploy

-  Create required Cloudflare resources: `npx wrangler d1 create <database-name>`
-  Update `wrangler.json` with your resource IDs

### Environment Variables

Set your environment variables using Wrangler CLI:

```bash
npx wrangler secret put S3_ACCESS_KEY
npx wrangler secret put S3_SECRET_ACCESS_KEY
npx wrangler secret put S3_URL
```

Or use the Cloudflare Dashboard to set them via the web interface.
