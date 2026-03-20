# ci-terraform-runner

A container image for running Terraform integration tests and CI operations. Bundles Go, Terraform, AWS CLI v2, and IBM Cloud CLI into a single toolbox image.

## Tools Included

| Tool | Description |
|---|---|
| **Go** | Go toolchain (from UBI10 go-toolset base) |
| **Terraform** | Infrastructure as Code CLI (pinned to 1.14.x) |
| **AWS CLI v2** | Amazon Web Services command-line interface |
| **IBM Cloud CLI** | IBM Cloud command-line interface with `vpc-infrastructure` and `cloud-object-storage` plugins |
| **git** | Version control (from base image) |
| **jq** | JSON processor |
| **curl, bash, unzip** | Utility tools (from base image) |

## Base Image

Built on `registry.access.redhat.com/ubi10/go-toolset` (RHEL 10 / UBI 10). Runs as non-root user (UID 1001).

## Usage

The image is a generic toolbox with no fixed entrypoint. Use it in CI pipelines by specifying the command directly:

```yaml
# GitLab CI example
integration-test:
  image: quay.io/konflux-ci/ci-terraform-runner:latest
  script:
    - cd tests/integration
    - go test -v -timeout 30m ./...
```

```bash
# Local usage
podman run --rm ci-terraform-runner:latest terraform version
podman run --rm ci-terraform-runner:latest aws --version
podman run --rm ci-terraform-runner:latest go version
```

## Building Locally

```bash
podman build -t ci-terraform-runner:latest -f Containerfile .
```

## Dependency Updates

Tool versions are managed by Renovate/MintMaker. Terraform is constrained to the 1.14.x range via `renovate.json`.

## License

Apache License 2.0 — see [LICENSE](LICENSE).
