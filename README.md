# Loki: ReadyStack build kit

The offline build kit for the Docker image `readystack/loki:v3.7.10-CE-ubuntu24.04-r2`. It holds what the image is built from (upstream source, base images, toolchain, pinned dependencies) and the documents, so the image can be rebuilt with no network access (`docker build --network=none`).

- Release: [v3.7.10-CE-ubuntu24.04-r2](https://github.com/readystack-dev/loki/releases/tag/v3.7.10-CE-ubuntu24.04-r2)
- Image: [`docker.io/readystack/loki:v3.7.10-CE-ubuntu24.04-r2`](https://hub.docker.com/r/readystack/loki/tags), manifest digest `sha256:610a15ec41ac200474f32fef8a43ca3ca3444f989174cd3bc969401138297523`
- Product page: https://readystack.dev/products/loki
- Source and build recipe: https://readystack.dev/products/loki/recipe
- Build trail: https://readystack.dev/products/loki/trail/v3.7.10-CE-ubuntu24.04-r2
- Defects: this repository's Issues (defects recorded from 25 November 2026 on) and https://readystack.dev/defects.html?product=loki

## The image

Single container, single static Go binary running in single-binary monolithic mode (-target=all, all components in one process), foreground process. Log aggregation with the tsdb index + chunk object-store BOTH on the local filesystem under /loki; NO relational/SQL database, NO external object store, NO sidecar. The image IS the complete running instance.

## Who uses Loki and why

Operations and platform teams who collect logs from many services, especially on Kubernetes, and read them alongside metrics in Grafana.

Loki is a log aggregation system that indexes only a set of labels for each log stream instead of the full text, which keeps it simpler to operate and cheaper to run.

A typical use: A team runs many services on Kubernetes and needs their logs in one place. An agent ships each pod's logs to Loki with the pod's labels; when an alert fires, an engineer filters by those labels in Grafana and reads the matching logs next to the metrics.

From the upstream project's own description: https://github.com/grafana/loki, https://grafana.com/docs/loki/latest/.

## This release

- Docker Scout policy grade at release: **A** (94%), graded 2026-11-22 with `docker scout policy` (CLI 1.26.0, docker-default configuration).
  - Docker Scout counts 10 packages with copyleft licences (GPL, LGPL, MPL), all of them operating-system packages (it counts only packages whose licence it can read; the SBOM lists every component).
- Signed with ReadyStack's release key (fingerprint `04daa92344a52dbc`): https://readystack.dev/keys/cosign.pub.
- Runs as a non-root user (`loki`).
- Attestations on the image: an SPDX SBOM and SLSA provenance v1.
- Rebuilt from this kit with network egress blocked on 2026-11-22; the rebuilt image digest is the published one.
- Base: Ubuntu 24.04.

The current grade, measured again every day, is on https://readystack.dev/products/loki.

Loki is distributed under its upstream licence (AGPL-3.0-only); the kit contains upstream's source under that licence, and each vendored component under its own.

## Download

From the [v3.7.10-CE-ubuntu24.04-r2 release](https://github.com/readystack-dev/loki/releases/tag/v3.7.10-CE-ubuntu24.04-r2): `ReadyStack-loki-v3.7.10-CE-ubuntu24.04-r2.tar.gz` (607.1 MB), SHA-256 `db4abe730f4558815b270a4671a4838b6400ad7f9084d5e50c2ff3149d5b596d`.

```sh
sha256sum --check --ignore-missing SHA256SUMS
```

## Verify and rebuild

The rebuild statement is signed with the ReadyStack release key. Check the signature before you rely on it:

```sh
cosign verify-blob --key https://readystack.dev/keys/cosign.pub \
  --bundle ReadyStack-loki-v3.7.10-CE-ubuntu24.04-r2.rebuild.json.sigstore ReadyStack-loki-v3.7.10-CE-ubuntu24.04-r2.rebuild.json
```

Unpack with `-p` so file modes are kept, then follow the kit's own README.md and docs/REBUILD.md:

```sh
tar -xpzf ReadyStack-loki-v3.7.10-CE-ubuntu24.04-r2.tar.gz
cd v3.7.10-CE-ubuntu24.04-r2
less README.md docs/REBUILD.md
```

The build trail (`ReadyStack-loki-v3.7.10-CE-ubuntu24.04-r2.trail.json`, signed the same way, and its Markdown copy) records how this release was built and checked.

## This repository

`recipe/` holds the kit's recipe files (Dockerfiles, build scripts, pins) as published at https://readystack.dev/products/loki/recipe. The complete kit is the release download. Each release is a tag here.

— ReadyStack · https://readystack.dev
