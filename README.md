# GitHub Action: Deploy Static Site to Netlify or Vercel

This GitHub Action deploys your static site to Netlify or Vercel by triggering a build hook from the convenience of your GitHub repository. You can also set it up to run by cron.

The Action will skip deployment when no changes are detected since the last run. To store the commit id of the current deployment GitHub cache mechanism is used.

## Usage with Netlify

To obtain build hook URL:

1. Sign in with Netlify.
2. Select the site you want to deploy. Go to _Site configuration_.
3. Select _Build & deploy_.
5. Scroll down to _Build hooks_ and click _Add build hook_.
6. Enter a descriptive name of your hook and click _Save_.
7. Your webook URL has the following format: `https://api.netlify.com/build_hooks/12345679abcdef`. You shouldn't share it with anyone else. I recommend saving it in _Secrets and variables_ section of your GitHub repository settings.
8. Create your workflow as follows:

```yaml
name: Deploy static site
on:
  workflow_dispatch:

jobs:
  DeploySite:
    runs-on: ubuntu-latest
    steps:
      - name: Deploy site
        uses: lwojcik/github-action-deploy-static-site@v1
        with:
          platform: netlify
          netlify_deploy_hook_url: ${{ secrets.NETLIFY_DEPLOY_HOOK_URL }}
```

## Usage with Vercel

To disable automatic builds and obtain build hook URL:

1. Sign in with Vercel.
2. Select the project you want to deploy. Go to _Settings_.
3. Select _Git_. Head to _Deploy Hooks_ section and create a new hook.
4. Your webhook URL has the following format: `https://api.vercel.com/v1/integrations/deploy/12345679abcdef`. You shouldn't share it with anyone else. I recommend saving it in _Secrets and variables_ section of your GitHub repository settings.
5. Create your workflow as follows:

```yaml
name: Deploy static site
on:
  workflow_dispatch:

jobs:
  DeploySite:
    runs-on: ubuntu-latest
    steps:
      - name: Deploy site
        uses: lwojcik/github-action-deploy-static-site@v1
        with:
          platform: vercel
          vercel_deploy_hook_url: ${{ secrets.VERCEL_DEPLOY_HOOK_URL }}
```

## License

Licensed under MIT License. See [LICENSE](./LICENSE) for more information.
