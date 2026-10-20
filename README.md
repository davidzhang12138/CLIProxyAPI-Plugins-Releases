# CLIProxyAPI-Plugins Releases

This public repository hosts the release automation for [CLIProxyAPI-Plugins](https://github.com/davidzhang12138/CLIProxyAPI-Plugins). It does not contain private source code, and **no Releases are stored here**.

## Release flow

- `sync-private-tags` reads version tags from the private source repository every 5 minutes and dispatches a `release` run for every tag that has not been published yet. A specific tag can also be published immediately via manual dispatch.
- `release` checks out the exact private source tag, builds macOS (ARM64 and Intel), Windows (AMD64), and Linux (glibc AMD64 and ARM64) plugin dynamic library archives for all active plugins (`hyper`, `channel-guard`, `websearch`), runs unit and official CPA host integration tests, then creates the GitHub Release **in the private source repository** together with all archives and `checksums.txt`.

Private source is only checked out temporarily on trusted GitHub-hosted runners. It is never committed to this repository, and workflows do not use GitHub Actions build caches, SBOM, or provenance attestations.

Cross-repository writes (creating Releases and uploading assets in the private source repository) require a PAT with `contents: write` on the source repository, stored as the `MAIN_REPO_TOKEN` secret. Read access to the private source is provided by the `SOURCE_DEPLOY_KEY` deploy key.

The first automatically synchronized version is `v0.2.0`.
