# glide-builder
ARG GOLANG_VERSION
ARG ALPINE_VERSION
FROM docker.io/golang:${GOLANG_VERSION}-alpine${ALPINE_VERSION} AS glide-builder
ENV OUTDIR=/out

RUN set -eux && \
	apk --no-cache add \
		git
RUN set -eux && \
	mkdir -p "${OUTDIR}/usr/bin" && \
	GOBIN=${OUTDIR}/usr/bin/ go get -a -u -v -tags 'osusergo netgo static static_build' -installsuffix 'netgo' -ldflags='-d -s -w -extldflags=-static' \
		github.com/Masterminds/glide

# glide
ARG GOLANG_VERSION
ARG ALPINE_VERSION
FROM docker.io/golang:${GOLANG_VERSION}-alpine${ALPINE_VERSION} AS glide
LABEL maintainer "container-image https://github.com/container-image"
COPY --from=glide-builder /out/ /
RUN set -eux && \
	apk --no-cache add \
		ca-certificates \
		git
ENTRYPOINT ["/usr/bin/glide"]
