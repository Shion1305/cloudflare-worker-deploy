# Cloudflare Worker Deploy

GitHub Action to automatically generate `wrangler.toml` and deploy Cloudflare Workers.

Simplifies deployment by eliminating the need to commit `wrangler.toml` to your repository. Instead, use a template and let the action configure and deploy your worker.

## Usage

```yaml
- name: Deploy to Cloudflare Workers
  uses: Shion1305/cloudflare-worker-deploy@v1
  with:
    worker_name: my-worker
    wrangler_template: wrangler.template.toml
    cloudflare_api_token: ${{ secrets.CLOUDFLARE_API_TOKEN }}
    account_id: your-account-id
```

## Inputs

| Name | Description | Required | Default |
|------|-------------|----------|---------|
| `worker_name` | Name of Cloudflare Worker | Yes | - |
| `wrangler_template` | Path to wrangler.template.toml in the repo | Yes | - |
| `cloudflare_api_token` | Cloudflare API token with Workers:Edit permission | Yes | - |
| `account_id` | Cloudflare Account ID | Yes | - |
| `compat_date` | Cloudflare Workers compatibility_date | No | `2025-09-28` |
| `print_toml` | Print rendered wrangler.toml to logs | No | `true` |

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
