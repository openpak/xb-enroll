# Xbox Enrollment Server

OpenPak's Xbox Enrollment Server.

| | |
| --- | --- |
| Network | Original Xbox |
| Role | Public front for device-pairing enrolment: pairing codes and poll status for original Xbox consoles |
| Depends on | [`xb-account`](https://github.com/openpak/xb-account) |
| Image | `ghcr.io/openpak/xb-enroll` |
| Based on | [Xombie](https://github.com/XombieOnline/xombie) (AGPL-3.0) |
| Licence | AGPL-3.0-only |

## What it serves

| Endpoint | What |
| --- | --- |
| `POST /v1/enrol` | Starts pairing: returns a pairing code, expiry and poll token |
| `GET /v1/enrol/{pollToken}` | Pairing status: pending, approved or expired |
| `GET /healthz` | Container health probe |

## Run

```sh
cp deploy/xb-enroll.env.example .env   # replace the REPLACE_* values first
set -a; . ./.env; set +a
GOWORK=off go run ./cmd/xb-enroll
```

## Docs

- [Protocol](docs/protocol.md)
- [Evidence](docs/evidence.md)
- [Deploy](docs/deploy.md)
- [Notes](docs/notes.md)

## Licence

AGPL-3.0-only. See [LICENSE](LICENSE).
