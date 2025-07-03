# Reportmate

**Reportmate** is a modular, cross-platform endpoint reporting system powered by **osquery**, with support for our favorite software deployment primates: **Munki** and **Cimian**.

- A modern T3 stack web app — built with **Next.js**, **Prisma**, **tRPC**, and **Tailwind**, packaged in Docker  
- A native **Swift app for macOS** *(planned)*  
- A native **C# app for Windows** *(planned)*  
- Multi-cloud infrastructure support (Azure and AWS) for ingestion and processing  

Designed for real-time telemetry across macOS and Windows fleets, Reportmate delivers visibility, modularity, and modern deployment workflows — all open to community extension through standalone module repositories.

## Core Repositories

- **reportmate-web-app** – A React + Next.js web dashboard for visualizing device telemetry and module data  
- **reportmate-swift-app** – A native macOS/iOS app for visualizing reports and managing configuration  
- **reportmate-csharp-app** – A native Windows app for viewing device state and local configuration  
- **reportmate-mac-client** – macOS client for executing reporting queries  
- **reportmate-windows-client** – Windows client for executing reporting queries  
- **reportmate-infra-azure** – Terraform and serverless telemetry ingestion for Azure  
- **reportmate-infra-aws** – Terraform and serverless telemetry ingestion for AWS  

## Modules

Modules are published individually under the naming convention: `reportmate-module-*`

Examples:

- `reportmate-module-hardware`  
- `reportmate-module-network`  
- `reportmate-module-security`  

Each module is self-contained and may define:

- osquery query sets  
- reporting schema  
- frontend widget (React or SwiftUI)  
- platform compatibility flags

![Dashboard](https://github.com/reportmate/.github/blob/442e3a85657a92bd24821f00474216c854cfa96a/reportmate_dashboard.png)  
![Devices](https://github.com/reportmate/.github/blob/442e3a85657a92bd24821f00474216c854cfa96a/reportmate_devices.png)  
![Device Detail](https://github.com/reportmate/.github/blob/442e3a85657a92bd24821f00474216c854cfa96a/reportmate_device.png)  
![Managed Installs](https://github.com/reportmate/.github/blob/442e3a85657a92bd24821f00474216c854cfa96a/reportmate_installs.png)
