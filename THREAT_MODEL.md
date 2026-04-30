# Threat Model

In designing our threat model we take into consideration that Cucumber and its
components are a test-tool intended to be used with and within trusted code. 

As such the threat model focuses almost entirely on artifact security.

## Tainted Artifacts

> Attackers may pose as a vetted maintainer and/or offer artifacts that may be compromised.

### Mitigations

1. Cucumber publishes various artifacts to various registries such as Maven, NPM,
RubyGems (or many others). Users that consume artifacts in this fashion, should obtain
their artifacts there. The exact coordinates for each artifact can be found in
the Git repository for that artifact.
2. Users with a dependency on a Git repository should consider pinning their
dependencies to an exact hash.
