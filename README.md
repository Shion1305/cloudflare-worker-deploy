# Cloudflare Worker Deploy

GitHub Action to automatically generate `wrangler.toml` and deploy Cloudflare Workers.

Simplifies deployment by eliminating the need to commit `wrangler.toml` to your repository. Instead, use a template and let the action configure and deploy your worker.

## Usage

### Basic Usage (Default Template)

The action includes a default template with common Cloudflare Workers configuration:

```yaml
- name: Deploy to Cloudflare Workers
  uses: Shion1305/cloudflare-worker-deploy@v1
  with:
    worker_name: my-worker
    cloudflare_api_token: ${{ secrets.CLOUDFLARE_API_TOKEN }}
    account_id: your-account-id
    # Optional
    main_path: .output/server/index.mjs
    assets_dir: .output/public
    compat_date: "2025-09-28"
    print_toml: false
```

The default template generates:

```toml
name = "my-worker"
account_id = "your-account-id"
main = ".output/server/index.mjs"
compatibility_date = "2025-09-28"
compatibility_flags = ["nodejs_compat"]

[assets]
directory = ".output/public"
binding = "ASSETS"
```

### Custom Template Usage

For more complex configurations, create a `wrangler.template.toml` file:

**wrangler.template.toml:**

```toml
name = "{{WORKER_NAME}}"
account_id = "{{ACCOUNT_ID}}"
main = "{{MAIN_PATH}}"
compatibility_date = "{{COMPAT_DATE}}"

[vars]
ENVIRONMENT = "production"
API_URL = "https://api.example.com"

[[kv_namespaces]]
binding = "MY_KV"
id = "your-kv-namespace-id"
```

**Workflow:**

```yaml
- name: Deploy to Cloudflare Workers
  uses: Shion1305/cloudflare-worker-deploy@v1
  with:
    worker_name: my-custom-worker
    wrangler_template: wrangler.template.toml
    cloudflare_api_token: ${{ secrets.CLOUDFLARE_API_TOKEN }}
    account_id: your-account-id
    # Optional
    main_path: ./dist/index.js
    compat_date: "2025-10-01"
    print_toml: true
```

## Inputs

| Name | Description | Required | Default |
|------|-------------|----------|---------|
| `worker_name` | Name of Cloudflare Worker | Yes | - |
| `wrangler_template` | Path to wrangler.template.toml in the repo | No | (uses default) |
| `cloudflare_api_token` | Cloudflare API token with Workers:Edit permission | Yes | - |
| `account_id` | Cloudflare Account ID | Yes | - |
| `main_path` | Path to the main entry point file | No | `.output/server/index.mjs` |
| `assets_dir` | Path to the assets directory | No | `.output/public` |
| `compat_date` | Cloudflare Workers compatibility_date | No | `2025-09-28` |
| `print_toml` | Print rendered wrangler.toml to logs | No | `true` |

## Template Placeholders

When using a custom `wrangler.template.toml`, the following placeholders are available:

| Placeholder | Replaced With | Description |
|-------------|---------------|-------------|
| `{{WORKER_NAME}}` | `inputs.worker_name` | Name of the worker |
| `{{ACCOUNT_ID}}` | `inputs.account_id` | Cloudflare account ID |
| `{{MAIN_PATH}}` | `inputs.main_path` | Path to main entry point file |
| `{{ASSETS_DIR}}` | `inputs.assets_dir` | Path to assets directory |
| `{{COMPAT_DATE}}` | `inputs.compat_date` | Compatibility date |

All other content in the template file is preserved as-is, allowing you to customize bindings, environment variables, KV namespaces, and other Cloudflare Workers configuration.

## How It Works

1. Generates `wrangler.toml` with your specified configuration
2. Optionally prints the generated config to logs for debugging
3. Deploys to Cloudflare Workers using [cloudflare/wrangler-action](https://github.com/cloudflare/wrangler-action)

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

## Issues

If you encounter any problems, please [open an issue](https://github.com/Shion1305/cloudflare-worker-deploy/issues) with details about your setup and the error.

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
