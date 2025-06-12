# Prerequisites

Add these plugins to your plugin list

```clojure
[org.clojars.jj/bump "1.0.4"]
```

# Supported workflows

1. clojars-release-major
2. clojars-release-minor
3. clojars-release-patch
4. clojars-release-snapshot
5. lein-test

# How to use

## Test workflow

Add this workflow configuration to ``.github/workflows/test.yaml``

```yaml 
name: Test

on:
  push:
  pull_request:

jobs:
  run-lein-tests:
    name: Lein test
    uses: ruroru/workflows/.github/workflows/lein-test.yaml@master
```

## Release workflow

### Secrets

These secrets are necessary for the workflow:

* CLOJARS_USER - your clojars user
* CLOJARS_PASS - deploy key from clojars
* GPG_SIGNING_KEY - base64 encoded gpg key

### Workflow

Create ``.github/workflows/release-snapshot.yaml`` with this content

```yaml
name: Release Snapshot to clojars

on:
  workflow_dispatch: { }

jobs:
  release:
    permissions:
      contents: write
    uses: ruroru/workflows/.github/workflows/clojars-release-snapshot.yaml@master
    secrets:
      GPG_SIGNING_KEY: ${{ secrets.GPG_SIGNING_KEY }}
      CLOJARS_USER: ${{ secrets.CLOJARS_USER }}
      CLOJARS_PASS: ${{ secrets.CLOJARS_PASS }}
```

## Inputs
| Input        | Default value | plugin needed                                                   |
| ------------ | ------------- | --------------------------------------------------------------- |
| bump_readme  | true          | [bump-md](https://clojars.org/org.clojars.jj/bump-md)           |
| strict_check | true          | [strict-check](https://clojars.org/org.clojars.jj/strict-check) |
