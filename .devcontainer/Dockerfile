# Copyright © 2021 Alibaba Group Holding Ltd.
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#     http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.

# Development container image for the sealer project.
#
# sealer is a Go project that builds Kubernetes ClusterImages. The CI pipeline
# (see .github/workflows/go.yml) pins Go 1.17.3 and golangci-lint v1.39.0, so the
# dev container matches those versions to keep local development consistent with
# CI. The image is based on the official golang image so contributors get a
# working Go toolchain out of the box.

FROM golang:1.17.3-bullseye

# golangci-lint version pinned to match .golangci.yml and .github/workflows/go.yml
ARG GOLANGCI_LINT_VERSION=v1.39.0

# Install golangci-lint at the version used by CI.
RUN curl -sSfL https://raw.githubusercontent.com/golangci/golangci-lint/master/install.sh \
        | sh -s -- -b "$(go env GOPATH)/bin" ${GOLANGCI_LINT_VERSION}

# addlicense is used by `make filelicense` to keep license headers in sync. CI
# fetches a pre-built x86_64 binary, but that is not portable; build it from
# source so the dev container works on any architecture (amd64/arm64).
RUN git clone --depth 1 --branch v1.0.0 https://github.com/google/addlicense /tmp/addlicense-src \
    && cd /tmp/addlicense-src \
    && go build -o "$(go env GOPATH)/bin/addlicense" \
    && cd / \
    && rm -rf /tmp/addlicense-src

# gosec is used by `make gosec` for security scanning. v2.2.0's go.mod
# declares a non-v2 module path, so it cannot be installed via `go install`
# or `go get`. The release also lacks a pre-built linux/arm64 binary, so build
# it from the source tag instead, which works on every supported platform.
RUN git clone --depth 1 --branch v2.2.0 https://github.com/securego/gosec /tmp/gosec-src \
    && cd /tmp/gosec-src \
    && go build -o "$(go env GOPATH)/bin/gosec" ./cmd/gosec/ \
    && cd / \
    && rm -rf /tmp/gosec-src

# The base golang:1.17.3-bullseye image already ships git, make, curl, tar and
# ca-certificates, so no extra apt-get step is needed. hack/build.sh relies on
# git (for `git rev-parse`), make (for the Makefile targets) and tar (to produce
# the release archives), all of which are available out of the box.

ENV GO111MODULE=on
