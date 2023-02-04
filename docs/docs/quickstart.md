---
sidebar_label: Quickstart
---

# 🚀 Quickstart

In <5mins, learn how to access & manage OAuth tokens for any API, using Github as an example. Ready? Go! ⏰

First, clone and start Nango:

```bash
git clone https://github.com/NangoHQ/quickstart.git && cd quickstart
docker compose up
```

In a new tab, add any Github OAuth App to Nango (optionally [register your own Github OAuth App](https://docs.github.com/en/developers/apps/building-oauth-apps/creating-an-oauth-app)):

```bash
npx nango config:create github-dev github 57876b21174fed02b905 e43242c9a67fa06141e8d219c2364283d14f9ad1 "user,public_repo"
```

Authorize Github on this [example page](https://docs.nango.dev/demo/github). Nango will securely retrieve, store and refresh OAuth credentials. Now try:

```bash
npx nango token:get 1 github-dev
```

Congrats 🥳 You have a fresh token to access the Github API! Let's make sure it works (❗️replace `TOKEN`):

```bash
curl -XGET -G "https://api.github.com/users/bastienbeurier/repos" -H "Authorization: Bearer TOKEN"
```

(In practice, you should use our [backend SDKs](https://docs.nango.dev/reference/guide#node-sdk) to fetch tokens from your codebase.)

Wanna go live? Go through the more detailed [Step-By-Step Guide](reference/guide.md). You can [self-host Nango](category/deploy-nango-sync-open-source) or use [Nango Cloud](cloud).
