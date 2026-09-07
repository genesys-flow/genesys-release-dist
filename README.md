# Genesys Release — downloads

This repository holds the **release artifacts** of Genesys Release, a
self-hosted push-to-deploy platform on k3s: the `genesys` CLI archives and
their `checksums.txt`. It contains no source code; the source is private.

## Install the platform

On a fresh Linux server, as root:

```sh
curl -fsSL https://get.genesysflow.dev | sh -s -- --domain apps.example.com --email ops@example.com
```

The script downloads the CLI from the [Releases](../../releases) page of this
repository, verifies it against `checksums.txt`, and runs `genesys install`.

Pin a version or point at another repository:

```sh
curl -fsSL https://get.genesysflow.dev | GENESYS_VERSION=v0.1.0 sh -s -- --domain ... --email ...
curl -fsSL https://get.genesysflow.dev | GENESYS_REPO=me/mirror sh -s -- --domain ... --email ...
```

## Install just the CLI

Download the archive for your OS and architecture from
[Releases](../../releases), verify it, and put `genesys` on your `PATH`:

```sh
tag=$(curl -fsSLI -o /dev/null -w '%{url_effective}' https://github.com/genesysflow/genesys-release-dist/releases/latest | sed 's#.*/##')
curl -fsSLO "https://github.com/genesysflow/genesys-release-dist/releases/download/$tag/genesys_${tag#v}_linux_amd64.tar.gz"
curl -fsSLO "https://github.com/genesysflow/genesys-release-dist/releases/download/$tag/checksums.txt"
sha256sum --check --ignore-missing checksums.txt
tar -xzf "genesys_${tag#v}_linux_amd64.tar.gz" genesys && sudo install genesys /usr/local/bin/
```

The controller image is published at `ghcr.io/genesysflow/genesys-release-controller`.
