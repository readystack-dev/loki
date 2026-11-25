# Provenance — Loki (`v3.7.10-CE-ubuntu24.04-r2`)

Every item below is copied from a record in this archive; the record is named in brackets.

## Source

- Upstream repository: https://github.com/grafana/loki
- Revision built: `v3.7.10` (commit `c792f58577a46c64b0a8e808d7c643ed0e8c5e83`)
- Source tree: `9d5ae892da846aa56675f55cfa9a39126c62a3c8d0e70539fff87f79434d7592` over 23183 files [`docs/provenance/SOURCE_TREE_HASH.json`]
- Community Edition only: enterprise code is removed or refused before the build (CE audit at acquire).

## Build

- Method: `source-rebuild-go`; base image `scratch + a chiselled Ubuntu 24.04 runtime (vendored/chisel/slices.txt, packages.tsv)`
- Compiled with `--network=none` from vendored source, dependencies and base images; BuildKit pinned to `moby/buildkit:v0.31.2` (shipped in `toolchain/`).
- SOURCE_DATE_EPOCH: `1795161097` [`SOURCE_DATE_EPOCH.txt`]
- Frozen inputs: [`docs/provenance/FREEZE_MANIFEST.json`], build recipe: [`docs/provenance/BUILD_RECIPE.json`].

## Output

- Published image: `readystack/loki:v3.7.10-CE-ubuntu24.04-r2` — index digest `sha256:a1068432af3e754550b2bfcc0524772a837573c4d292eab82017811d15aeb13b` [`docs/provenance/LOCKED_DIGEST.txt`]
- linux/amd64 image digest `sha256:610a15ec41ac200474f32fef8a43ca3ca3444f989174cd3bc969401138297523` — reproduced from this archive before publishing [`docs/provenance/REPRO_VERIFICATION.json`].

## Attestations and signature

- SBOM + SLSA provenance attached to the image: attached: an SPDX SBOM and SLSA v1 provenance [`docs/provenance/SCOUT_VERDICT.json`]
- cosign signature by the ReadyStack release key: verified before publishing. Check it: `cosign verify --key https://readystack.dev/keys/cosign.pub readystack/loki:v3.7.10-CE-ubuntu24.04-r2`

## Pinned inputs, and why

- 37 pinned inputs, each with where it came from, why this one, how it was checked and when it was decided: `docs/PINS.md` [`docs/provenance/PINS.json`] (upstream source 2, base and build images 3, image digests 2, dependency versions readystack sets 11, pins in the kit's dockerfiles 12, readystack's build engine 5, dependencies upstream chose 2).

## Build trail

- How this release passed through the ReadyStack harness, record by record (every chain, drive, stage, review round, defect and check, each naming the record it is copied from): `ReadyStack-loki-v3.7.10-CE-ubuntu24.04-r2.trail.json`, published beside this archive and signed with the release key (`ReadyStack-loki-v3.7.10-CE-ubuntu24.04-r2.trail.json.sigstore`), and readable at https://readystack.dev/products/loki/trail/v3.7.10-CE-ubuntu24.04-r2.

