# Releasing

This document describes how to make a release using GitHub Actions. Not all projects use this process. If you were linked to this file from a project without additional instruction it is safe to assume it does. 

There are two parts to making a release:

* [Prepare the release](#prepare-the-release)
* [Make the release](#make-the-release)

If you're making a major release, there are special considerations, and you should make sure to [read that section](#major-release).

## Required tooling

Before making a release, make sure you have these tools installed:
 * Git - version > 2.25 
 * [polyglot-release](https://github.com/cucumber/polyglot-release) and its dependencies
 * [changelog](https://github.com/cucumber/changelog/)

## Prepare the release

Anyone with permission to push to the `main` branch can prepare a release.

1. Add new information to `CHANGELOG.md`. Ideally the `CHANGELOG.md` should be up-to-date, but sometimes there will be accidental omissions when merging PRs.
    * Use `git log --format=format:"* %s (%an)" --reverse <last-version-tag>..HEAD` to list all commits since the last release.
    * Add changelog details under the `## [Unreleased]` heading; `polyglot-release` will update this heading when it makes the release
1. Update contributors list (if applicable)
    * List recent contributors:
      ```
      git log --format=format:"%an <%ae>" --reverse <last-version-tag>..HEAD  | grep -vEi "(renovate|dependabot|Snyk)" | sort| uniq -i
      ```
    * For JavaScript: Update the contributors list in `package.json` (keep alphabetical order)


## Make the release
Only people with permission to push to `release/*` branches can make releases. Typically these permissions are assigned to the [release team](https://github.com/orgs/cucumber/teams/release).

1. Decide what the next version number should be
   * Look at `CHANGELOG.md` to see what has changed since the last relesase
   * Use [semver](https://semver.org/) to decide on a version for the next release
   * If you are bumping the `MAJOR` version of `cucumber-{jvm,js,ruby}`, see the [Major release](#major-release) section
2. From the root of the polyglot project you are trying to release run:
   
   ```
   polyglot-release <new version>
   ```
3. Wait until the `release-*` workflows in GitHub Actions have passed
4. Rerun individual workflows if they fail
5. Announce the release
   * in the `#commiters` Discord channel

### Major release

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
