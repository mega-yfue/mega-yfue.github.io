# mega-yfue.github.io

Publishes the eufy SDK documentation site at <https://mega-yfue.github.io/>.

**This repo contains no documentation.** Every page, image, and API reference entry is generated from
the [SDK repo](https://github.com/mega-yfue/eufy-sdk)'s docs folder on each run of
[`.github/workflows/pages.yml`](.github/workflows/pages.yml). Edit the guides there; the site picks
them up on the next build.

The build rebuilds nightly and on demand — Actions → `pages` → *Run workflow*.
