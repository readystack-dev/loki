# Loki v3.7.10 — ReadyStack release `v3.7.10-CE-ubuntu24.04-r2`

Generated 2026-11-22 from this release's build records and strategy. Statements that come from a single record name it in brackets; the others come from this release's strategy and build log.

## Release

- **Image:** `readystack/loki:v3.7.10-CE-ubuntu24.04-r2` (linux/amd64). Index digest `sha256:a1068432af3e754550b2bfcc0524772a837573c4d292eab82017811d15aeb13b` [`docs/provenance/LOCKED_DIGEST.txt`].
- **Image manifest digest:** `sha256:610a15ec41ac200474f32fef8a43ca3ca3444f989174cd3bc969401138297523` [`TARGET_DIGEST.txt`].
- **Upstream:** Loki v3.7.10 built from source at `v3.7.10` of https://github.com/grafana/loki — upstream notes: https://github.com/grafana/loki/releases/tag/v3.7.10 [`docs/provenance/BUILD_RECIPE.json`].
- **Base image:** none (`FROM scratch`): a chiselled Ubuntu 24.04 runtime, the files Canonical's chisel cut from Ubuntu's own packages (`vendored/chisel/slices.txt`, `vendored/chisel/packages.tsv`); runtime user `loki`.
- **Build:** `source-rebuild-go`, compiled offline with `--network=none`.
- **Supersedes:** ReadyStack release `v3.7.10-CE-ubuntu24.04-r1`.

## What changed in this release

- Upstream version unchanged (v3.7.10); ReadyStack rebuild.
- Minimum dependency versions ReadyStack enforces on top of upstream (raised only where upstream's own version was lower); shipped versions from the SBOM [`docs/sbom/SBOM.json`]:
  - `golang.org/x/crypto` at least `v0.58.0`: not raised, upstream already pins `v0.59.0` (shipped: `v0.59.0`)
  - `golang.org/x/net` at least `v0.62.0`: not raised, upstream already pins `v0.62.0` (shipped: `v0.62.0`)
  - `github.com/apache/thrift` at least `v0.25.0`: not raised, upstream already pins `v0.25.0` (shipped: `v0.25.0`)
  - `google.golang.org/grpc` at least `v1.83.2`: not raised, upstream already pins `v1.85.0` (shipped: `v1.85.0`)
  - `github.com/klauspost/compress` at least `v1.20.2`: not raised, upstream already pins `v1.20.3` (shipped: `v1.20.3`)
  - `github.com/andybalholm/brotli` at least `v1.2.7`: not raised, upstream already pins `v1.2.7` (shipped: `v1.2.7`)
- Loki v3.7.10 is unchanged from the release this supersedes (v3.7.10-CE-ubuntu24.04-r1); this release rebuilds it. Changes: the image is rebuilt on the current ubuntu:24.04 image (sha256:9c7f200c07e3…), which its maintainers updated after v3.7.10-CE-ubuntu24.04-r1 was built on sha256:2198e4c65076…. Settings, ports and data volumes are those of v3.7.10-CE-ubuntu24.04-r1: replace the container with this image on the same volumes.

## Security status at release (measured 2026-11-20)

- **Docker Scout:** 0 fixable critical/high vulnerabilities; runs as `loki` (non-root); SBOM + provenance attestations present (SPDX SBOM + SLSA v1) [`docs/provenance/SCOUT_VERDICT.json`].
- **OSV cross-check:** no fixable critical/high vulnerability beyond Docker Scout's findings.
- **Signature:** cosign signature verified by ReadyStack's release gate before publishing; check it yourself with `cosign verify --key https://readystack.dev/keys/cosign.pub readystack/loki:v3.7.10-CE-ubuntu24.04-r2` (signatures are recorded in the public Sigstore transparency log).
- **ReadyStack release check at release:** passed (ReadyStack's own test, not a Docker Scout grade). Vulnerabilities are disclosed every day and the image never changes, so it is re-checked daily, beside Docker Scout's own dated grade, at https://readystack.dev/queue.

## Reproducibility

Rebuilds offline from this archive to the identical image digest `sha256:610a15ec41ac200474f32fef8a43ca3ca3444f989174cd3bc969401138297523`: run `./BUILD_OFFLINE.sh`, then `./VERIFY_OFFLINE.sh` (add `--online` to confirm the Docker Hub tag holds the same digest). Before publishing, ReadyStack rebuilt the kit with `./BUILD_OFFLINE.sh` and compared the result with the published digest [`docs/provenance/REPRO_VERIFICATION.json`], then ran `./BUILD_OFFLINE.sh` and `./VERIFY_OFFLINE.sh` from this archive (without `--online`).

## Validation

- 5 of 5 use cases passed (5 authored) against the running image [`docs/validation/test-results.json`].

## Known issues

- None recorded.

## Defects fixed

- 27 problems found before release and fixed before it shipped (27 by review); each is listed in `docs/defects/defects.json`.

## Verify this archive

```
sha256sum -c CHECKSUMS.sha256
./BUILD_OFFLINE.sh && ./VERIFY_OFFLINE.sh --online
cosign verify --key https://readystack.dev/keys/cosign.pub readystack/loki:v3.7.10-CE-ubuntu24.04-r2
```

ReadyStack records every signature in the public Sigstore transparency log (Rekor), so the command needs no extra flags.

