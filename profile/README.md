# ReportMate

**ReportMate** is a modular cross-platform endpoint reporting system powered by **osquery**, with support for our favorite software deployment primates: **Munki** and **Cimian**.

- A modern T3 stack web app — built with **Next.js**, **Prisma**, **tRPC**, and **Tailwind**, packaged in Docker  
- Multi-cloud infrastructure support (Azure and AWS) for ingestion and processing  

Designed for real-time telemetry across macOS and Windows fleets, ReportMate delivers visibility, modularity, and modern deployment workflows — all open to community extension through standalone module repositories.

## Core Repositories

- **reportmate-web-app** – A React + Next.js web dashboard for visualizing device telemetry and module data  
- **reportmate-mac-client** – macOS client for executing reporting queries  
- **reportmate-windows-client** – Windows client for executing reporting queries  
- **terraform-azurerm-reportmate** – Terraform and serverless telemetry ingestion for Azure  
- **terraform-aws-reportmate** – Terraform and serverless telemetry ingestion for AWS  

![Dashboard](https://github.com/reportmate/.github/blob/442e3a85657a92bd24821f00474216c854cfa96a/reportmate_dashboard.png)  
![Devices](https://github.com/reportmate/.github/blob/442e3a85657a92bd24821f00474216c854cfa96a/reportmate_devices.png)  
![Device Detail](https://github.com/reportmate/.github/blob/442e3a85657a92bd24821f00474216c854cfa96a/reportmate_device.png)  
![Managed Installs](https://github.com/reportmate/.github/blob/442e3a85657a92bd24821f00474216c854cfa96a/reportmate_installs.png)
