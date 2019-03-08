ARG GOLANG_VERSION
ARG ALPINE_VERSION
FROM docker.io/golang:${GOLANG_VERSION}-alpine${ALPINE_VERSION} AS go-cloud-debug-agent
ENV GO111MODULE=off
RUN set -eux && \
	apk --no-cache add \
		git
RUN set -eux && \
	CGO_ENABLED=0 GOOS="$(go env GOOS)" GOARCH="$(go env GOARCH)" \
	go get -a -v \
		-tags='osusergo netgo static static_build' \
		-ldflags="-d -s -w '-extldflags=-static'" \
		-installsuffix=netgo \
	cloud.google.com/go/cmd/go-cloud-debug-agent
