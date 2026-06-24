<div align="left">

# ReportMate

### Endpoint management that just works

Monitor, manage, and report on your **macOS and Windows** fleet from a single dashboard.
Real-time hardware, software, security, and network data — collected at the endpoint, streamed to a live dashboard and API.

**Self-host it for free, or let us run it for you.**

[**Website**](https://reportmate.app) · [**Live Demo**](https://demo.reportmate.app) · [**API Docs**](https://docs.reportmate.app)

</div>

<div align="center">

![ReportMate dashboard]([https://raw.githubusercontent.com/reportmate/.github/main/reportmate_dashboard.png](https://github.com/reportmate/reportmate-website/blob/main/public/dashboard.webp?raw=true))

</div>

## What it does

Lightweight native agents collect data on-device with **osquery**, **bash**, and **pwsh** — with deep support for the **Munki** and **Cimian** software deployment tools — and POST a unified JSON payload to the API. The dashboard is a reader: heavy lifting happens at the endpoint, not in the cloud.

- **Cross-platform** — native agents for macOS (Swift) and Windows (C#/.NET)
- **Ten modules** — hardware, network, security, applications, installs, inventory, system, management, peripherals, identity
- **Real-time dashboard** — Next.js frontend with live fleet overview, device drill-down, and posture views
- **REST API** — FastAPI backend with OpenAPI docs, versioned endpoints, rate limiting, and pagination
- **Multi-cloud** — Terraform modules for Azure and AWS, or self-host on any infrastructure
- **AI-ready** — query your fleet from the terminal (CLI) or expose it to AI agents (MCP server)

## Architecture

**Collection → Transmission → Ingestion → Storage → Display**

```
Agents (macOS / Windows)
  │  POST /api/v1/events
  ▼
FastAPI backend ──── PostgreSQL  (one JSONB row per device per module)
  │
  ├── Web PubSub / SignalR  (real-time push)
  ▼
Next.js dashboard  (fleet views, device detail)
```

## Repositories

### Server
| Repo | Stack | License | |
|---|---|---|---|
| [reportmate-api](https://github.com/reportmate/reportmate-api) | Python · FastAPI | AGPL-3.0 | Ingestion + query REST API |
| [reportmate-app-web](https://github.com/reportmate/reportmate-app-web) | TypeScript · Next.js | AGPL-3.0 | Real-time web dashboard |

### Endpoint agents
| Repo | Stack | License | |
|---|---|---|---|
| [reportmate-client-mac](https://github.com/reportmate/reportmate-client-mac) | Swift | MIT | macOS telemetry agent |
| [reportmate-client-win](https://github.com/reportmate/reportmate-client-win) | C# · .NET | MIT | Windows telemetry agent |

### Tooling
| Repo | Stack | License | |
|---|---|---|---|
| [reportmate-cli](https://github.com/reportmate/reportmate-cli) | Rust | AGPL-3.0 | Query and manage your fleet from the terminal |
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
| [reportmate-website](https://github.com/reportmate/reportmate-website) | Astro | MIT | Marketing site — [reportmate.app](https://reportmate.app) |

## Open core

ReportMate is genuinely open source. The **server** (API and web dashboard) is **AGPL-3.0**; the **endpoint agents**, **Terraform modules**, and **deployment tooling** are **MIT**. A separate commercial license is available for organizations whose policies do not permit AGPL.

Self-host the whole stack for free, or pick a [managed plan](https://reportmate.app/pricing) and we'll run it for you.

## Contributing

See [CONTRIBUTING.md](https://github.com/reportmate/.github/blob/main/CONTRIBUTING.md). Contributions are accepted under the project [CLA](https://github.com/reportmate/.github/blob/main/CLA.md) with a [DCO](https://developercertificate.org/) sign-off (`git commit -s`).
