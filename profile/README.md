<div align="left">

# ReportMate

## Your whole fleet. Always reporting in.

Monitor, manage, and report on your **Mac and Windows** fleet from a single dashboard.
Real-time hardware, software, security, and network data — collected at the endpoint, streamed to a live dashboard, an API, native apps, and a command-line tool.

**Self-host it for free, or let us run it for you.**

[**Website**](https://reportmate.app) · [**Live Demo**](https://demo.reportmate.app) · [**API Docs**](https://reportmate.app/docs) · [**OpenAPI spec**](https://reportmate.app/openapi.json)

</div>

<div align="center">

![ReportMate dashboard](https://github.com/reportmate/reportmate-website/blob/main/public/dashboard.webp?raw=true)

</div>

## What it does

Lightweight native agents collect data on-device with **osquery**, **bash**, and **pwsh** — with deep support for the **Munki** and **Cimian** software deployment tools — and POST a unified JSON payload to the API. Everything downstream is a reader: heavy lifting happens at the endpoint, not in the cloud.

- **Cross-platform** — native agents for macOS (Swift) and Windows (C#/.NET)
- **Eleven modules** — hardware, network, security, applications, installs, inventory, system, management, peripherals, identity, profiles
- **Real-time dashboard** — Next.js frontend with live fleet overview, device drill-down, and posture views
- **Native apps** — the same fleet views as a SwiftUI app on the Mac and a WPF app on Windows, each also able to read the local agent's cache with no API
- **REST API** — FastAPI backend with a published OpenAPI spec, versioned endpoints, rate limiting, and pagination
- **Command line** — `reportmateutil`, one binary with a command for every API route, printing the API's JSON unchanged
- **Multi-cloud** — Terraform modules for Azure and AWS, or self-host on any infrastructure
- **Agentic** — query your fleet from scripts and coding agents through the CLI, or through the MCP server (work in progress)

## Architecture

**Collection → Transmission → Ingestion → Storage → Display**

```
Swift and C# agents (Mac / Windows)
  │  POST /api/v1/events
  ▼
FastAPI backend ──── PostgreSQL  (one JSONB row per device per module)
  │
  ├── Web PubSub / SignalR  (real-time push)
  ▼
Next.js dashboard · ReportMate for Mac · ReportMate for Windows · reportmateutil · MCP
```

## The command line: `reportmateutil`

`reportmateutil` is the reference client for the API. Every read, report, maintenance and settings endpoint has a command, `--output json` prints exactly what the API returned, and a contract test in the repository fails the moment the API gains a route the tool does not know. The tool is named `reportmateutil` so that `reportmate` stays the name of the app that ships it.

Configure it with the instance and one credential, then ask:

```
export REPORTMATE_API_URL=https://api.reportmate.example
export REPORTMATE_API_KEY=rm_yourclient_yoursecret
```

```
reportmateutil devices --limit 20
reportmateutil device SERIAL --module installs
reportmateutil module network --output json | jq '.[].raw.activeConnection.ipAddress'
reportmateutil events failures --hours 24
reportmateutil logs munki --summary
```

A scoped API key is the preferred credential (`read`, `ingest`, `admin` scopes; issued with `reportmateutil api-keys create`). An OIDC bearer token (`REPORTMATE_TOKEN`) and the shared client passphrase (`REPORTMATE_PASSPHRASE`) also work. `reportmateutil --help` lists every command.

**Where it comes from.** Each release attaches one tarball per platform (macOS arm64, x86_64 and universal; Windows x64 and arm64; Linux x64), unsigned, and an unsigned macOS installer package. The native apps bundle the binary from these releases at build time: on the Mac it lives at `ReportMate.app/Contents/Helpers/reportmateutil` with a `/usr/local/bin/reportmateutil` symlink, and on Windows it sits beside the app as `C:\Program Files\ReportMate\reportmateutil.exe`, on the PATH. Installing the app installs the tool.

## Repositories

### Server
| Repo | Stack | License | |
|---|---|---|---|
| [reportmate-api](https://github.com/reportmate/reportmate-api) | Python · FastAPI | AGPL-3.0 | Ingestion + query REST API; exports the OpenAPI spec on every build |
| [reportmate-app-web](https://github.com/reportmate/reportmate-app-web) | TypeScript · Next.js | AGPL-3.0 | Real-time web dashboard |

### Endpoint agents
| Repo | Stack | License | |
|---|---|---|---|
| [reportmate-client-mac](https://github.com/reportmate/reportmate-client-mac) | Swift | MIT | macOS telemetry agent |
| [reportmate-client-win](https://github.com/reportmate/reportmate-client-win) | C# · .NET | MIT | Windows telemetry agent |

### Native apps
| Repo | Stack | License | |
|---|---|---|---|
| [reportmate-app-swift](https://github.com/reportmate/reportmate-app-swift) | Swift · SwiftUI | — | ReportMate for Mac: the fleet dashboard as a native app, bundling `reportmateutil` |
| [reportmate-app-csharp](https://github.com/reportmate/reportmate-app-csharp) | C# · WPF | — | ReportMate for Windows: the fleet dashboard as a native app, bundling `reportmateutil` |

### Tooling
| Repo | Stack | License | |
|---|---|---|---|
| [reportmate-cli](https://github.com/reportmate/reportmate-cli) | Rust | AGPL-3.0 | `reportmateutil`: query and manage your fleet from the terminal |
| [reportmate-mcp](https://github.com/reportmate/reportmate-mcp) | Python · FastMCP | AGPL-3.0 | Expose your fleet to AI agents |

### Deployment
| Repo | Stack | License | |
|---|---|---|---|
| [terraform-azurerm-reportmate](https://github.com/reportmate/terraform-azurerm-reportmate) | HCL | MIT | Azure infrastructure module |
| [terraform-aws-reportmate](https://github.com/reportmate/terraform-aws-reportmate) | HCL | MIT | AWS infrastructure module |
| [selfhosted-docker-reportmate](https://github.com/reportmate/selfhosted-docker-reportmate) | Docker · Packer | MIT | Compose stack + appliance image |

### Project
| Repo | Stack | License | |
|---|---|---|---|
| [reportmate-website](https://github.com/reportmate/reportmate-website) | Astro | MIT | Marketing site and API docs — [reportmate.app](https://reportmate.app); the published spec is regenerated from the API source daily |

## Releases and signing

Public repositories build **unsigned** artifacts on every push and publish them as GitHub release assets on a version tag: the agents' packages, the apps' bundles and installers, and the CLI's tarballs. Signing, notarization and deployment to endpoints happen downstream, in whatever management pipeline runs a fleet, against a pinned release. No signing identity or fleet configuration lives in these repositories.

## Open core

ReportMate is open source. The **server** (API and web dashboard) and the **CLI** are **AGPL-3.0**; the **endpoint agents**, **Terraform modules**, and **deployment tooling** are **MIT**. A separate commercial license is available for organizations whose policies do not permit AGPL.

Self-host the whole stack for free, or pick a [managed plan](https://reportmate.app/pricing) and we'll run it for you.

## Contributing

See [CONTRIBUTING.md](https://github.com/reportmate/.github/blob/main/CONTRIBUTING.md). Contributions are accepted under the project [CLA](https://github.com/reportmate/.github/blob/main/CLA.md) with a [DCO](https://developercertificate.org/) sign-off (`git commit -s`).
