# Releasing

This document describes how to make a release using GitHub Actions.

There are three parts to making a release:

* [Upgrade dependencies](#upgrade-dependencies)
* [Prepare the release](#prepare-the-release)
* [Make the release](#make-the-release)

If you're making a major release, there are special considerations, and you should make sure to [read that section](#major-release).

## Upgrade dependencies

Although we lean on Renovate to do automatic depencency upgrades, it doesn't take care of everything. Before making a release, do a manual check for any dependency upgrades needed, and take the time to upgrade everything that you can.

For JavaScript projects:

    npx npm-check-updates --upgrade

For Ruby projects:

    curl https://raw.githubusercontent.com/cucumber/.github/main/scripts/update-gemspec | bash
    # If the repo has its own scripts/update-gemspec - delete it!

For Java projects:

    mvn versions:force-releases
    mvn versions:update-properties -DallowMajorUpdates=true -Dmaven.version.rules="file://`pwd`/.versions/rules.xml"

For Go projects:

    go get -t -u ./...

Make a pull request with any dependency upgrades so that you get a full CI run. If it passes, merge the PR and you can continue with the release.

## Prepare the release

Anyone with permission to push to the `main` branch can prepare a release.

1. Add missing entries to `CHANGELOG.md`. Ideally the `CHANGELOG.md` should be up-to-date, but sometimes there will be accidental omissions when merging PRs.
    * Use `git log --format=format:"* %s (%an)" --reverse <last-version-tag>..HEAD` to list all commits since the last release.
1. Update contributors list (if applicable)
    * List recent contributors:
      ```
      git log --format=format:"%an <%ae>" --reverse <last-version-tag>..HEAD  | grep -vEi "(renovate|dependabot|Snyk)" | sort| uniq -i
      ```
    * For JavaScript: Update the contributors list in `package.json` (keep alphabetical order)
1. Decide what the next version number should be
    * Look at `CHANGELOG.md` to see what has changed since the last relesase
    * Use [semver](https://semver.org/) to decide on a version for the next release
    * If you are bumping the `MAJOR` version of `cucumber-{jvm,js,ruby}`, see the [Major release](#major-release) section
      ```
      export next_release=MAJOR.MINOR.PATCH[-rc.N]
      ```
1. Modify the changelog:
    * If you have the [`changelog`](https://github.com/cucumber/changelog) tool installed:
      ```
      changelog release $next_release --tag-format "v%s" -o CHANGELOG.md
      ```
    * If you don't have the `changelog` tool installed:
        * Under `[Unreleased]` at the top, add a new `[${version}] - ${YYYY-mm-dd}` header
        * Add a new `[${version}]` link at the bottom
        * Update the `[Unreleased]` link at the bottom
1. Update the version number in the relevant package decriptor(s), such as:
    * `package.json` - just run `npm version $next_release --no-git-tag-version`, will also update `package-lock.json`
    * `go.mod`
    * `pom.xml` **Remove the -SNAPSHOT suffix**
    * `VERSION` (for Ruby libraries)
    * `*.csproj` - update `<VersionNumber>`
    * `pyproject.toml`
1. Commit and push
   ```
   git commit -am "Release $next_release" && git push
   ```

## Make the release

Only people with permission to push to `release/*` branches can make releases.

1. Push to a new `release/*` branch to trigger the `release-*` workflows
   ```
   git push origin main:release/v$next_release
   ```
1. Wait until the `release-*` workflows in GitHub Actions have passed
1. Rerun individual workflows if they fail
1. (Java only) - in `pom.xml`, bump the **patch** version and append `-SNASHOT` (e.g. `1.2.4-SNAPSHOT`) and commit/push
1. Announce the release
   * in the `#newsletter` Slack channel
   * on the `@cucumberbdd` Twitter account
   * write a blog post

## Major release

If you are releasing `cucumber-{jvm,js,ruby}` and bumping the `MAJOR` version, make a `-rc.N` release candidate.
The release candidate should be available for at least a month to give users time to validate that there are no unexpected breaking changes.

If you are releasing a **library**, then you should **not** create a release candidate - just release the new major version.

Add entries with code examples to `UPGRADING.md` to help users migrate to the new major version. If the project doesn't have an `UPGRADING.md`
file yet, use this template:

````
# Upgrading

This document describes breaking changes and how to upgrade. For a complete list of changes including minor and patch releases, please refer to the [changelog](CHANGELOG.md).

## MAJOR.0.0

Previously `foo` could be `bar`:

```js
const foo = 'bar'
```

In this release you have to make `bar` be `foo`

```js
const bar = 'foo'
```
````
