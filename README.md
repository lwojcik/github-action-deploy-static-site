# GitHub Action: Deploy Static Site to Netlify or Vercel

This GitHub Action deploys your static site to Netlify or Vercel by triggering a build hook from the convenience of your GitHub repository. You can also set it up to run by cron.

The initial plan for this Action was to save build minutes by disabling automatic builds on each push and triggering deployments only when build hooks are called. However, both platforms make it very complicated to have both of the following at once: (1) have automatic deployments disabled on each repo push, and (2) be able to trigger deployments via build hooks. I didn't want to use GitHub to build projects manually - built-in CI/CD tools in Netlify and Vercel are good enough. But I discovered that limitation way too late in the project and I didn't want to scrap so much code, so here it is. :) Leaving it intact in hope it becomes possible to make this Action work the way I wanted.

The Action will skip deployment when no changes are detected since the last run. To store the commit id of the current deployment GitHub cache mechanism is used.

You can also pass `always_deploy` set to `true` to trigger deployment without checking for changes.

## Usage with Netlify

To obtain build hook URL:

1. Sign in with Netlify.
2. Select the site you want to deploy. Go to _Site configuration_.
3. Select _Build & deploy_.
4. Scroll down to _Build hooks_ and click _Add build hook_.
5. Enter a descriptive name of your hook and click _Save_.
6. Your webook URL has the following format: `https://api.netlify.com/build_hooks/12345679abcdef`. You shouldn't share it with anyone else. I recommend saving it in _Secrets and variables_ section of your GitHub repository settings.
7. Create your workflow as follows:

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
          # always_deploy: true - if you want to skip checking for changes
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
          # always_deploy: true - if you want to skip checking for changes
```

## License

Licensed under MIT License. See [LICENSE](./LICENSE) for more information.
