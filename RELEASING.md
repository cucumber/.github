# Releasing

This document describes how to make a release using GitHub Actions.
There are two parts to making a release. First, prepare the release, then make the release.

## Preparing a release

Anyone with permission to push to the `main` branch can prepare a release.

1. Make sure the CI build is passing
1. Decide what the next version number should be
    * Look at `CHANGELOG.md` to see what has changed since the last relesase
    * Use [semver](https://semver.org/) to decide on a version for the next release.
      ```
      export next_release=MAJOR.MINOR.PATCH
      ```
1. Modify the changelog:
    * If you have the [`changelog`](https://github.com/cucumber/changelog) too installed:
      ```
      changelog release $next_release -o CHANGELOG.md
      ```
    * If you don't have the `changelog` tool installed:
        * Under `[Unreleased]` at the top, add a new `[${version}] - ${YYYY-mm-dd}` header
        * Add a new `[${version}]` link at the bottom
        * Update the `[Unreleased]` link at the bottom
1. Update the version number in the relevant package decriptor(s), such as:
    * `package.json`
    * `go.mod`
    * `pom.xml`
    * `VERSION` (for Ruby libraries)
3. Commit and push
   ```
   git add .
   git commit -m "Release $next_release"
   git push
   ```

### Making a release

Only people with permission to push to `release/*` branches can make releases.

1. Push to a new `release/*` branch to trigger the `release-*` workflows
   ```
   git push origin main:release/v$next_release
   ```
1. Wait until the `release-*` workflows in GitHub Actions have passed
1. Rerun individual workflows if they fail
1. Consider announcing the release on Slack/Twitter/Blog
