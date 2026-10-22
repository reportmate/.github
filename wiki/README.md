# ReportMate project wiki

ReportMate collects Mac and Windows endpoint data with native agents and serves it through one API to a web dashboard, native apps, a CLI and an MCP server. This repository holds the organization profile, screenshots and contribution terms. Implementation and development guidance live with each component.

## Repository guides

| Component | Guide |
| --- | --- |
| API | [reportmate-api](https://github.com/reportmate/reportmate-api#readme) · [production readiness](https://github.com/reportmate/reportmate-api/blob/main/PRODUCTION-READINESS.md) |
| Web dashboard | [reportmate-app-web](https://github.com/reportmate/reportmate-app-web#readme) · [URL state conventions](https://github.com/reportmate/reportmate-app-web/blob/main/URL_STATE_CONVENTIONS.md) |
| Mac agent | [reportmate-client-mac](https://github.com/reportmate/reportmate-client-mac#readme) · [docs](https://github.com/reportmate/reportmate-client-mac/tree/main/docs) |
| Windows agent | [reportmate-client-win](https://github.com/reportmate/reportmate-client-win#readme) · [docs](https://github.com/reportmate/reportmate-client-win/tree/main/docs) |
| ReportMate for Mac | [reportmate-app-swift](https://github.com/reportmate/reportmate-app-swift#readme) · [docs](https://github.com/reportmate/reportmate-app-swift/tree/main/docs) |
| ReportMate for Windows | [reportmate-app-csharp](https://github.com/reportmate/reportmate-app-csharp#readme) |
| CLI | [reportmate-cli](https://github.com/reportmate/reportmate-cli#readme) |
| MCP server | [reportmate-mcp](https://github.com/reportmate/reportmate-mcp#readme) |
| Azure Terraform | [terraform-azurerm-reportmate](https://github.com/reportmate/terraform-azurerm-reportmate/tree/main/wiki) |
| AWS Terraform | [terraform-aws-reportmate](https://github.com/reportmate/terraform-aws-reportmate#readme) |
| Self-hosted Docker | [selfhosted-docker-reportmate](https://github.com/reportmate/selfhosted-docker-reportmate#readme) |
| Website and API docs | [reportmate-website](https://github.com/reportmate/reportmate-website#readme) · [reportmate.app/docs](https://reportmate.app/docs) |

A component's current source and tests establish implemented behavior; roadmap descriptions remain plans until delivered.

## Product architecture

The agents collect on the endpoint and POST one JSON payload per run to the API. The API owns ingestion, storage (one PostgreSQL JSONB row per device per module) and the versioned HTTP contract published as the [OpenAPI spec](https://reportmate.app/openapi.json). The web dashboard, native apps, CLI and MCP server are readers of that contract. The Terraform modules and the Docker stack deploy the server components.

The [organization profile](../profile/README.md) introduces the product, with its screenshots and demo button in the same folder. [Brand assets](../branding) hold the shared logo, mark and app icon source. Link these from component documentation instead of creating copies.

## Documentation conventions

Use module for the data areas the agents collect and the dashboard shows, such as Hardware, Installs and Security. Describe implemented behavior in the present tense; link issues for planned work.

Each component's documentation is versioned with its source and reviewed with it. Update the existing authoritative page when behavior changes, keep commands copyable and use fictional serials, hostnames and users in examples. Fleet-specific inventories, credentials, signing identities and deployment procedures stay with the deployment owner.
