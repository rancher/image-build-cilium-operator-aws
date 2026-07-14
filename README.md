# image-build-cilium-operator-aws

This repo builds a hardened image for the Cilium `operator-aws` component that
rke2 mirrors, from [github.com/cilium/cilium](https://github.com/cilium/cilium), packaged in a minimal
SLE BCI (`registry.suse.com/bci/bci-nano`) based image.

Cilium Operator (AWS)

Binaries are compiled against [`rancher/hardened-build-base`](https://github.com/rancher/image-build-base),
which provides the latest supported Go toolchain (FIPS/BoringCrypto-enabled on amd64).

## Images produced

- `rancher/hardened-cilium-operator-aws`

## Building locally

```sh
make build-image-all          # build for the host architecture
make image-scan               # run Trivy against the built image(s)
```

The upstream version is controlled by the `TAG` variable in the [`Makefile`](./Makefile).
A `-buildYYYYMMDD` suffix (`BUILD_META`) is appended automatically and is required on
release tags.

## Automated updates

[Updatecli](./updatecli) keeps the upstream version and the
`rancher/hardened-build-base` version current via daily PRs.

## CI

- **Build**: builds the image and runs a [Trivy](https://github.com/aquasecurity/trivy) scan
  (`CRITICAL,HIGH`) on every PR/push.
- **Release**: on a published GitHub release (or manual dispatch), builds multi-arch images
  and pushes them to the Rancher Prime registry (`registry.rancher.com/rancher/hardened-cilium-operator-aws`).
