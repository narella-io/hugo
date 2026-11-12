# narella-io/hugo

A fork of [gohugoio/hugo](https://github.com/gohugoio/hugo) that carries dependency
security bumps for the Narella homepage image until upstream releases them. It has no
feature changes.

## narella/v0.169.0 (tag v0.169.0-narella.1)

Base: upstream tag `v0.169.0` (1da10fc44701c566b1669cdb2d12638f155138bd).

Change: `golang.org/x/net` v0.61.0 -> v0.62.0 in go.mod/go.sum. That is the whole diff;
`go mod tidy` changed nothing else.

Why: v0.169.0 is the newest Hugo release and its binary links x/net v0.61.0, which Trivy
reports as CVE-2026-78661 (HIGH) and CVE-2026-97045 (MEDIUM). The homepage image runs
Hugo at runtime, so the binary cannot be left out of it.

Build (as `narella-io/narella` homepage/Dockerfile does; same flags as the upstream
`hugo_extended` release binary, Go 1.27.x):

    CGO_ENABLED=1 go build -tags extended \
        -ldflags "-s -w -X github.com/gohugoio/hugo/common/hugo.vendorInfo=narella" .

Retire this fork once an upstream release ships x/net >= v0.62.0: point the Dockerfile
back at the upstream tag.
