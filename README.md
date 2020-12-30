# action-create-v-docs

A GitHub action to create documentation for V modules.

Under the hood it's a wrapper around `v doc`.

## Usage

This action is supposed to work with with `nocturlab/setup-vlang-action` and `test-room-7/action-update-file` actions.

### Example workflow

This workflow generates documentation for a module and pushes a new commit if documentation is changed.

```yml
name: Docs
on:
  push:
    branches: [master]
  pull_request:
    types: [opened, synchronize]
jobs:
  update-docs:
    name: Update docs
    runs-on: ubuntu-latest
    steps:
      - name: Checkout the repository
        uses: actions/checkout@v1

      - name: Install V
        uses: nocturlab/setup-vlang-action@v1
        with:
          v-version: master

      - name: Generate documentation
        uses: test-room-7/action-create-v-docs@v0
        with:
          docs-dir: docs

      - name: Update documentation
        uses: test-room-7/action-update-file@v1
        with:
          file-path: docs/*
          commit-msg: Update documentation
          github-token: ${{ secrets.GITHUB_TOKEN }}
```

Once this workflow is executed, the `docs` directory with documentation will be added (or updated if necessary) to your repository. Optionally, you can set up GitHub Pages for your repository to have documentation available at yourname.github.io/yourmodule.

[Live documentation][live-example] and the [repository][example-repo] as an example how can this action be used.

It's worth noting that the action output depends on the V `doc` tool, so consider using a stable V version in your workflow (see the `v-version` input of `nocturlab/setup-vlang-action` action) to make sure generated documentation is consistent and has no issues (or they are known or minor).

### Inputs

#### Required inputs

- `docs-dir`: a directory where documentation will be placed.

#### Optional inputs

- `module-dir`: a directory where module source files are placed.

## License

Licensed under the [MIT License](./LICENSE.md).

[example-repo]: https://github.com/alexesprit/colors
[live-example]: https://alexesprit.com/colors/
