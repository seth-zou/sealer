# sealer Dev Container

This dev container provides a ready-to-code environment for the [sealer](https://github.com/alibaba/sealer) project, matching the toolchain used in CI (see `.github/workflows/go.yml`).

## What's included

- **Go 1.17.3** — pinned to the same version as the CI pipeline.
- **golangci-lint v1.39.0** — pinned to the version declared in `.golangci.yml`, so local lint results match CI.
- **addlicense v1.0.0** — required by `make filelicense` to keep license headers in sync.
- **gosec v2.2.0** — required by `make gosec` for security scanning.
- **VS Code Go extension** with `gopls`, `goimports` formatting, and `golangci-lint` lint-on-save.

## Using the dev container

### VS Code / GitHub Codespaces

1. Install the [Dev Containers extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-containers) (or use GitHub Codespaces).
2. Open the repository in VS Code.
3. Run the **Dev Containers: Reopen in Container** command from the Command Palette (`F1`).

The container builds the image defined in [`Dockerfile`](./Dockerfile) and installs the VS Code extensions listed in [`devcontainer.json`](./devcontainer.json).

## Common tasks

These work exactly as they do in CI, since the toolchain versions match:

```bash
make style      # run fmt + vet + lint
make build      # build sealer and seautil binaries (output in _output/bin)
make linux      # cross-compile for linux/amd64
make gosec      # run security scanning
make filelicense # check / fix license headers
```

## Versions rationale

All tool versions are deliberately pinned to the same versions used by CI so that
failures reproduced locally are reproducible in CI and vice versa:

| Tool            | Version | Source                          |
|-----------------|---------|---------------------------------|
| Go              | 1.17.3  | `.github/workflows/go.yml`      |
| golangci-lint   | 1.39.0  | `.golangci.yml` / CI            |
| addlicense      | 1.0.0   | `.github/workflows/go.yml`      |
| gosec           | 2.2.0   | `Makefile` (`install-gosec`)    |
