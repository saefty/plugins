# syntax=docker/dockerfile:1.27
FROM --platform=$BUILDPLATFORM golang:1.27.3-trixie@sha256:af1bfc2614ebb1dbd31af514b78740fd678850e76498762b425ecee4c5c6ea80 AS build

ARG TARGETOS TARGETARCH
ENV CGO_ENABLED=0 GOOS=$TARGETOS GOARCH=$TARGETARCH

RUN --mount=type=cache,target=/go/pkg/mod \
    go install -ldflags="-s -w" -trimpath github.com/mercari/grpc-federation/cmd/protoc-gen-grpc-federation@v1.28.0 \
 && mv /go/bin/${GOOS}_${GOARCH}/protoc-gen-grpc-federation /go/bin/protoc-gen-grpc-federation || true

FROM scratch
COPY --from=build --link --chown=root:root /etc/passwd /etc/passwd
COPY --from=build --link --chown=root:root /go/bin/protoc-gen-grpc-federation .
ENV GOROOT /
USER nobody
ENTRYPOINT [ "/protoc-gen-grpc-federation" ]
