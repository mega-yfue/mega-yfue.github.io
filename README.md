# mega-yfue.github.io

Publishes the eufy SDK documentation site at <https://mega-yfue.github.io/>.

**This repo contains no documentation.** There is nothing here to edit — every page, image, and API
reference entry is generated from the SDK repo's docs folder on each run of
[`.github/workflows/pages.yml`](.github/workflows/pages.yml). Edit the guides in the SDK repo; the
site picks them up on the next build.

## How a change reaches the site

The SDK repo cannot notify this one without a cross-repo token, so the site rebuilds:

- **nightly**, on a schedule, and
- **on demand** — Actions → `pages` → *Run workflow*, for an immediate publish.

## Configuration

Nothing is hardcoded except the trigger branch and the cron expression, which GitHub evaluates
before any context exists. Everything else is a repository variable under
Settings → Secrets and variables → Actions, each falling back to the public-SDK setup:

| Variable       | Default              | What it selects                |
| -------------- | -------------------- | ------------------------------ |
| `SDK_REPO`     | `mega-yfue/eufy-sdk` | SDK repo to build from         |
| `SDK_REF`      | `main`               | branch, tag, or SHA            |
| `NODE_VERSION` | `24.5.0`             | build toolchain                |
| `DOCS_DIR`     | `docs/guide`         | docs site folder inside the SDK |
| `DOCS_BASE`    | `/`                  | site base path                 |

While the SDK is private, the secret `SDK_TOKEN` holds a fine-grained PAT scoped to that repo alone
with *Contents: read*. Migrating to the public SDK repo is a matter of deleting the overriding
variables and that secret — the defaults take over and the checkout falls back to this repo's own
`GITHUB_TOKEN`. See the comments at the top of the workflow.

`SDK_TOKEN` expires. A checkout failing with a 404 on a repo that plainly exists is the symptom.

## What the build actually does

It checks out the SDK, generates the API reference from the library's public JSDoc with TypeDoc,
sweeps the result for internal detail, and builds the VitePress site from the docs guide folder
alone. The SDK's `docs/` root holds maintainer notes that are never published, and two guards keep
them off the site — read the workflow header before changing any of it.
