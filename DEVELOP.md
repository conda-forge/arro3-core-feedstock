
# arro3-specific notes

While this feedstock is titled `arro3-core-feedstock`, it publishes conda-forge libraries for all namespace modules in <https://github.com/kylebarron/arro3>.

This currently includes:

- arro3-core
- arro3-io
- arro3-compute

## Updating Python version

This feedstock is a little tricky because arro3-io and arro3-compute depend on arro3-core, but those might fail to build if arro3-core hasn't yet been published yet. For example, this made it difficult to [update to support Python 3.13](https://github.com/conda-forge/arro3-core-feedstock/pull/18), because arro3-core wasn't released yet.

So this documents the process for handling these interlinked updates:

### Publish a new version of _just_ arro3-core.

1. Remove the file `conda_build_config.yaml`.
2. Replace all instances of `{{ arro3_module }}` with `arro3-core` . Look at https://github.com/conda-forge/arro3-core-feedstock/pull/2/files for reference and do the inverse.
3. Create a PR building just arro3-core. In the PR, [rerender](https://conda-forge.org/docs/maintainer/updating_pkgs/#rerendering-feedstocks) by adding a comment `@conda-forge-admin, please rerender`.
4. Merge the PR.
5. If there was a bot PR to update for e.g. a new Python version, such as [#18](https://github.com/conda-forge/arro3-core-feedstock/pull/18), apply the `bot-rerun` label to the PR so that it gets created again from the latest main.

### Then publish new versions of other namespace modules

1. Restore the file `conda_build_config.yaml`.
2. Replace all instances of `arro3-core` with `{{ arro3_module }}` . Look at https://github.com/conda-forge/arro3-core-feedstock/pull/2/files for reference and do the inverse.
3. In the PR, [rerender](https://conda-forge.org/docs/maintainer/updating_pkgs/#rerendering-feedstocks) by adding a comment `@conda-forge-admin, please rerender`.
