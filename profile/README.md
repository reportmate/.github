# ReportMate

**ReportMate** is a modular endpoint reporting system for Macintosh and Windows devices powered by **osquery**, with deep support for our favorite software deployment tools: **Munki** and **Cimian**.

## Architecture

**Collection → Transmission → Ingestion → Storage → Display**

- **Frontend**: Next.js web dashboard with React, Tailwind CSS, and TypeScript
- **API**: FastAPI containerized REST API for device data ingestion and retrieval
- **Database**: PostgreSQL with modular schema (one table per module)
- **Deployment**: Docker containers with multi-cloud infrastructure support
- **Infrastructure**: Terraform-managed resources for your cloud infra of choise: Azure, AWS, and (planned) GCS support

**Key Features:**
- Real-time device telemetry and visibility across macOS and Windows fleets
- Modular data collection: Applications, Hardware, Installs, Network, Profiles, Security, System
- Client-side data processing (heavy lifting on device, not in cloud)
- Direct database ingestion with module-specific schemas
- Cloud-agnostic architecture with provider-specific Terraform modules

## Core Repositories

- **reportmate-app-web** – Next.js + React web dashboard for visualizing device telemetry
- **reportmate-client-mac** – macOS Swift client for collecting and transmitting data via osquery
- **reportmate-client-win** – Windows C# client for collecting and transmitting data via osquery  
- **terraform-azurerm-reportmate** – Terraform modules for Azure infrastructure (Container Apps, PostgreSQL, Functions)
- **terraform-aws-reportmate** – Terraform modules for AWS infrastructure (ECS, RDS, Lambda)
