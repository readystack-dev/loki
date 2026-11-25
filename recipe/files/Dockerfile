# GENERATED L-009 hermetic build (--network=none, SBOM+SLSA-attestation-producing).
# Pinned bases from BASE_IMAGE_DIGESTS.lock; vendored Go tree + debs from Dockerfile.vendor.
FROM golang:1.26.11-bookworm@sha256:dc40a13b61304647ed7e5d581d246b86413dfc29d63c6d018310feb93cd98fc0 AS build_loki
WORKDIR /src
COPY vendored/loki-src/ .
ENV CGO_ENABLED=0 GOTOOLCHAIN=local GOWORK=off GOFLAGS="-mod=vendor -buildvcs=false" GOPROXY=off SOURCE_DATE_EPOCH=1795161097
RUN go build -trimpath -tags "netgo" -ldflags='-s -w -X github.com/grafana/loki/v3/pkg/util/build.Version=3.7.10 -X github.com/grafana/loki/v3/pkg/util/build.Branch=HEAD -X github.com/grafana/loki/v3/pkg/util/build.Revision=c792f585' -o /loki ./cmd/loki && /loki -version && \
    { readelf -lW /loki | sed -n 's/.*Requesting program interpreter: \(.*\)]$/\1/p'; readelf -dW /loki | sed -n 's/.*Shared library: \[\(.*\)\]$/\1/p'; } > /loki.needs
# rs-probe: the HEALTHCHECK helper (standard library only, from the kit's rs-probe.go)
FROM golang:1.26.11-bookworm@sha256:dc40a13b61304647ed7e5d581d246b86413dfc29d63c6d018310feb93cd98fc0 AS build_rs_probe
WORKDIR /probe
COPY rs-probe.go main.go
COPY rs-probe.mod go.mod
ENV CGO_ENABLED=0 GOTOOLCHAIN=local GOFLAGS="-buildvcs=false" GOPROXY=off SOURCE_DATE_EPOCH=1795161097
RUN go build -trimpath -ldflags='-s -w' -o /rs-probe .
# the chiselled rootfs (vendored/chisel, cut by Dockerfile.vendor), the service user, the binaries
FROM ubuntu:24.04@sha256:9c7f200c07e3d4a03e7249d2c16bf66a51120fb876aa2242cf31451412543289 AS rootfs
ARG SOURCE_DATE_EPOCH=1795161097
ADD vendored/chisel/rootfs.tar /rootfs/
RUN printf 'loki:x:10001:10001::/loki:/bin/sh\n' >> /rootfs/etc/passwd && printf 'loki:x:10001:\n' >> /rootfs/etc/group && \
    mkdir -p /rootfs/loki /rootfs/etc/loki /rootfs/loki/chunks /rootfs/loki/rules && chown -R 10001:10001 /rootfs/loki /rootfs/etc/loki /rootfs/loki/chunks /rootfs/loki/rules
COPY --from=build_loki /loki /rootfs/usr/bin/loki
COPY --from=build_loki /loki.needs /tmp/needs/loki
COPY --from=build_rs_probe /rs-probe /rootfs/usr/local/bin/rs-probe
COPY loki-config.yaml /rootfs/etc/loki/config.yaml
RUN chmod 755 /rootfs/usr/bin/loki /rootfs/usr/local/bin/rs-probe && \
    chown 10001:10001 /rootfs/etc/loki/config.yaml
RUN --mount=type=bind,source=vendored/chisel,target=/chisel set -eu; \
    for n in /tmp/needs/*; do while read -r lib; do [ -n "$lib" ] || continue; \
      case "$lib" in /*) [ -e "/rootfs$lib" ] ;; *) [ -n "$(find /rootfs/usr/lib /rootfs/usr/lib64 -name "$lib" 2>/dev/null | head -1)" ] ;; esac || \
      { echo "$(basename "$n") needs $lib, which the chiselled rootfs does not have: add its slice to runtime_base.slices" >&2; exit 1; }; \
    done < "$n"; done; rm -rf /tmp/needs; \
    [ -e /rootfs/etc/os-release ] || { echo "no /etc/os-release: base-files_release-info is missing" >&2; exit 1; }; \
    [ "$(grep -c '^Package: ' /rootfs/var/lib/dpkg/status)" = "$(wc -l < /chisel/packages.tsv)" ] || \
      { echo "/var/lib/dpkg/status does not list every package of vendored/chisel/packages.tsv" >&2; exit 1; }; \
    rm -rf /rootfs/var/log/dpkg.log /rootfs/var/log/alternatives.log /rootfs/var/log/apt /rootfs/var/cache/ldconfig/aux-cache /rootfs/var/cache/fontconfig
FROM scratch
ARG SOURCE_DATE_EPOCH=1795161097
COPY --from=rootfs /rootfs/ /
ENV PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
LABEL org.opencontainers.image.licenses="AGPL-3.0-only"
LABEL com.readystack.license.disclosure="This image includes Grafana Loki (https://github.com/grafana/loki), licensed under AGPL-3.0-only. Modifications: the build raises Go-module dependency versions in go.mod and go.sum where upstream's are older than the fixed, upstream-published releases (security updates; no Loki source file is edited); full diff of every upstream file the build changes at https://readystack.dev/source/loki/v3.7.10/loki-v3.7.10-readystack.patch (empty when the build changes none). Operating-system packages come from Ubuntu's archive, unmodified. ReadyStack license holders have access to the image build recipe and patched-package manifest on request via contact@readystack.dev."
LABEL org.opencontainers.image.title="Loki"
LABEL org.opencontainers.image.source="https://github.com/grafana/loki"
LABEL org.opencontainers.image.version="v3.7.10-CE-ubuntu24.04-r2"
LABEL org.opencontainers.image.revision="c792f58577a46c64b0a8e808d7c643ed0e8c5e83"
HEALTHCHECK --interval=5s --timeout=30s --start-period=25s \
  CMD /usr/local/bin/rs-probe http://127.0.0.1:3100/ready || exit 1
USER loki
EXPOSE 3100 9095
ENTRYPOINT ["loki", "-config.file=/etc/loki/config.yaml", "-target=all"]
