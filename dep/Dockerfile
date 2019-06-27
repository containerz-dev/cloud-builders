# dep-builder
ARG GOLANG_VERSION
ARG ALPINE_VERSION
FROM docker.io/golang:${GOLANG_VERSION}-alpine${ALPINE_VERSION} AS dep-builder
ENV OUTDIR=/out

RUN set -eux && \
	apk --no-cache add \
		git
RUN set -eux && \
	mkdir -p "${OUTDIR}/usr/bin" && \
	DEPNOLOCK=1 IMPORT_DURING_SOLVE='true' GOBIN=${OUTDIR}/usr/bin/ go get -a -u -v -tags 'osusergo netgo static static_build' -installsuffix 'netgo' -ldflags='-d -s -w -extldflags=-static' \
		github.com/golang/dep/cmd/dep

# dep
ARG GOLANG_VERSION
ARG ALPINE_VERSION
FROM docker.io/golang:${GOLANG_VERSION}-alpine${ALPINE_VERSION} AS dep
LABEL maintainer "containerz https://github.com/containerz-dev/cloud-builders"
COPY --from=dep-builder /out/ /
RUN set -eux && \
	apk --no-cache add \
		bzr \
		ca-certificates \
		git \
		mercurial \
		subversion
ENTRYPOINT ["/usr/bin/dep"]
