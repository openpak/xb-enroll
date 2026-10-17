# xb-enroll

Thin public front for device-pairing enrolment of an original Xbox. Stateless
(rate-limit counters only, in memory). It exposes only:

- `POST /v1/enrol` `{machineID, machineKey, clientInfo?}` -> `{pairingCode, expiresAt, pollToken}`
- `GET /v1/enrol/{pollToken}` -> `{status: pending|approved|expired, expiresAt}`
- `GET /healthz` (container probe, touches nothing)

All state and decisions live in xb-account, reached over mTLS with the client
identity `spiffe://openpak/xb-enroll`; xb-account authorizes that identity for
the two enrolment routes only. The console-facing contract, limits, error codes
and the state machine are in workspace `docs/xbox/enrolment-contract.md`.

Configuration: `deploy/env/xb-enroll.env.example`. Image: `deploy/docker/Dockerfile.xb-enroll`
(or this directory's Dockerfile). Quadlet template: `deploy/quadlet/openpak-xb-enroll.container`.
Nothing here is deployed. Local verification: `GOWORK=off go test -race ./...`.

## Repository

- Module `github.com/openpak/xb-enroll`, image `ghcr.io/openpak/xb-enroll` (static, scratch, uid 10001).
- Shared packages: `github.com/openpak/xb-common` (private; see its README for access). Service map: workspace docs `xbox/native-service-architecture.md`.
- Build and test: `GOWORK=off go build ./... && go vet ./... && go test -race ./...` (with `GOPRIVATE=github.com/openpak/*` and `gh auth setup-git`).
- Image: `podman build --secret id=netrc,src=$HOME/.netrc -t ghcr.io/openpak/xb-enroll:dev .`
- Release: push a `vX.Y.Z` tag; `.github/workflows/release.yml` (tags and manual dispatch only) tests and publishes the multi-arch image. CI reads xb-common with the job's `GITHUB_TOKEN` through a netrc build secret; nothing secret is in git or in the image.
- `deploy/` holds the env example and a Podman quadlet example (digests in it are examples; production pins its own).
- Provenance and licence: AGPL-3.0-only, see `LICENSE` and `NOTICE`. Derived from the Xombie project in part; the legacy Rust workspace remains in https://github.com/openpak/xbox-live.
