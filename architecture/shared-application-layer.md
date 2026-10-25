# Shared application layer

Status: proposal. This document describes a direction for ReportMate, not a shipped framework or an approved API contract. Component source and tests establish current behavior.

Adoption tracking: [issue #11](https://github.com/reportmate/.github/issues/11).

## Purpose

ReportMate's immediate opportunity is collection contracts, normalization, reporting semantics and consistent access enforcement. Approvals, document numbering and procurement machinery become relevant only when ReportMate owns workflows needing them.

ReportMate already uses frameworks for HTTP, web UI and native UI. The missing opportunity is making the product's own shared definitions explicit across collectors, API, web dashboard, native apps, CLI and MCP. Adding a module or measurement should become cheaper without requiring each component to rediscover what the data means.

This layer can develop within the existing architecture. It does not require a separately deployed service, a new public framework project or a rewrite.

## Existing foundations and gaps

The initial source review found these foundations. These are starting points to validate during design, not a comprehensive audit of every component.

| Area | Existing foundation | Gap to address |
| --- | --- | --- |
| Module definitions | The API declares valid module names; the web source contains manifests, registration and initialization code | A contract shared across the product; confirm which existing web registry paths are actually used before extending them |
| Collection and storage | Native agents submit module payloads; the API preserves flexible JSONB data | Explicit measurement definitions, compatibility rules and normalization boundaries |
| Data interpretation | Windows report fields resolve alternative Mac and Windows paths for the same measurement | Canonical names, types, units and applicability rather than repeated fallback knowledge |
| Security evaluation | The Mac evaluator explicitly ports the web evaluator | One behavioral specification with shared fixtures proving that implementations agree |
| Reporting | Windows has report specifications; Mac has a reusable fleet-report scaffold | Shared definitions for columns, filters, grouping and exports while preserving native presentation |
| API consumers | OpenAPI is published; CLI tests check route parity | Stronger response schemas and an explicit decision about generated types and clients |
| Access | The API supports authentication and read, ingest and admin scopes | Verify consistent authorization on every supported delivery path; scope support alone does not establish record or field policies |

Source entry points:

- [API models and module names](https://github.com/reportmate/reportmate-api/blob/main/dependencies.py)
- [Web module registry](https://github.com/reportmate/reportmate-app-web/blob/main/src/lib/modules/ModuleRegistry.ts) and [initialization](https://github.com/reportmate/reportmate-app-web/blob/main/src/lib/modules/ModuleInit.ts)
- [Web security evaluator](https://github.com/reportmate/reportmate-app-web/blob/main/src/lib/rules/evaluateSecurity.ts) and [Mac security evaluator](https://github.com/reportmate/reportmate-app-swift/blob/main/Sources/ReportMateKit/Rules/SecurityEvaluator.swift)
- [Windows report specification](https://github.com/reportmate/reportmate-app-csharp/blob/main/src/Views/Fleet/ReportSpec.cs)
- [Mac fleet-report scaffold](https://github.com/reportmate/reportmate-app-swift/blob/main/Sources/ReportMateUI/Views/Reports/FleetReportScaffold.swift)
- [CLI contract testing](https://github.com/reportmate/reportmate-cli#contract-tests)

These links follow component development branches and may move. Update this assessment as implementation changes.

## Proposed shared contracts

### Modules and measurements

A versioned module definition should identify its purpose, supported platforms, measurements and compatibility expectations. A measurement definition should establish:

- A stable identifier and canonical meaning, distinct from its display label.
- Type, unit, cardinality and any enumerated values.
- Platform applicability and the distinction between absent, unknown, unavailable and not applicable.
- Collection source and provenance, observation time and freshness expectations.
- Permitted filtering, sorting, grouping and aggregation behavior.
- Sensitivity and any applicable access requirements.
- Versioning, deprecation and extension rules.

For example, encryption disabled, encryption unknown and encryption not applicable must remain distinguishable. A Windows-only measurement must not count every Mac as failing it. Physical memory expressed through different collector paths must resolve to the same unit and meaning before fleet aggregation.

### Collection and normalization

Preserve flexible raw telemetry while publishing a deliberate interpreted contract. An evolving collector payload should not silently redefine a report field.

Specify where each transformation happens and give it one authoritative definition. Agents should continue collecting platform-specific evidence on the endpoint. The design must decide which canonical values agents can reliably supply and which historical or compatibility transformations the API must perform.

Normalization must preserve provenance and distinguish unavailable evidence from a negative observation. Compatibility rules must explain how older collectors and newer consumers coexist, how unknown fields are handled and how unsupported versions are reported. Introducing contract validation must not silently discard existing telemetry.

### Rules and derived values

Define security severity, applicability, inventory mapping and other derived meanings independently of UI code. Shared examples must cover rule precedence, missing values, platform differences and configuration versions.

The API is a candidate authority for fleet evaluation. Native apps also read local agent caches without an API, so the design must explicitly support offline evaluation where needed. Shared fixtures can prove equivalent behavior across languages even when executable code cannot be shared. Every computed result should have enough version context to explain which rules produced it.

### Reports and queries

Define reusable report semantics: the row's identity and grain, fields, columns, filters, sorting, grouping, aggregation, freshness and export behavior. Define how filters treat missing values, multiple values, platform applicability and derived states.

Counts, tables, drill-downs and exports must agree about which observations they describe. Fleet queries should specify server-side filtering and pagination so consumers do not mistake a partial page for the whole fleet.

Share definitions and behavioral expectations across clients. SwiftUI, WPF and web controls should remain appropriate to their platforms; the contract need not describe their complete layouts. Specialized device views can remain purpose-built.

### Access enforcement

The API must enforce applicable access policies for reads, queries, aggregates, exports, mutations and real-time delivery. Hiding a field or action in a client is not authorization. CLI and MCP access must follow the same API policies as interactive applications.

Begin by documenting and testing the current scope model. Decide whether ReportMate needs additional restrictions before designing a generic policy engine. If record or field restrictions are introduced, specify their behavior for counts, searches, exports, caches and subscriptions so alternate paths cannot reveal restricted data.

Local cache access has a different trust boundary from an authenticated fleet request. Document its reliance on operating-system access and explicitly decide what may be cached; do not imply that API policy can revoke an already downloaded local copy.

## Component responsibilities

| Component | Proposed responsibility |
| --- | --- |
| Organization repository | Own the architecture, shared vocabulary, decisions and adoption tracking |
| Mac and Windows collectors | Collect platform evidence, report provenance and supported contract versions, preserve collection efficiency |
| API | Own the published fleet contract, normalization boundary, query behavior and authoritative access enforcement |
| Web dashboard | Render contract-backed reports and use shared rule semantics; keep web interaction details local |
| Mac and Windows apps | Present native views and support local-cache use; demonstrate semantic parity with shared examples |
| CLI | Expose the API faithfully with stable machine-readable behavior and contract coverage |
| MCP | Offer task-oriented tools through authorized API calls without inventing separate fleet semantics |
| Deployment repositories | Declare any required configuration through their established IaC and reviewed deployment paths |

The package format and repository owning machine-readable definitions remain open. This architecture document does not itself create a dependency between repositories.

## Adoption plan

### 1. Establish the first contract

Use one hardware report as the first complete slice. Inventory its collector fields, API transformations, client fallbacks and query behavior. Propose the smallest useful contract and shared fixtures before changing implementations.

Acceptance: the design identifies ownership, versioning, normalization location, row grain, missing-value behavior and compatibility with existing collector payloads. Expected results are specified for both supported endpoint platforms and an offline native consumer.

### 2. Implement hardware across consumers

Adopt the contract incrementally in the collectors, API and relevant consumers. Generate types or metadata where this removes manual duplication, and keep adapters where compatibility requires them. Change only the deployment repositories that actually need configuration changes.

Acceptance: equivalent hardware values produce equivalent fleet results across web, native apps, CLI and MCP; units and platform applicability are consistent; filtering, pagination and exports describe the same population. Existing payloads remain readable under a documented compatibility policy.

### 3. Prove reuse with security

Use security to exercise applicability, unknown observations, inventory context, configurable rules and severity. Reuse the hardware contract mechanisms before introducing new abstractions.

Acceptance: shared fixtures prove rule precedence and equivalent outcomes across applicable consumers, including local-cache use. API authorization tests exercise the relevant query and delivery paths. Distinguish failures from unavailable or inapplicable evidence in reports and aggregates.

### 4. Extend through real module work

Migrate other modules when they need changes. Extract reusable mechanisms demonstrated by both initial slices. Retire duplicated definitions only after consumers have adopted their replacements; avoid a synchronized rewrite of all repositories.

## Tracking and issue boundaries

Keep one open cross-repository tracking issue in this repository, linked to this document. Use it to record contract decisions, acceptance evidence and links to implementation work. Publishing this proposal does not complete the adoption work.

Create component issues after the first contract design identifies concrete changes and dependencies. Do not pre-create placeholder issues in every repository. Each implementation issue should link to the relevant document section, identify its contract version, describe observable behavior and include acceptance criteria. Start with the hardware slice; scaffold security work when hardware has demonstrated the reusable approach.

Component documentation continues to own implementation and development guidance. Update this document as decisions settle, and change its status only when the agreed architecture is established. Keep proposals and delivered behavior distinguishable throughout adoption.

## Open decisions

1. Where should machine-readable definitions and shared fixtures live, and how should releases reach consumers?
2. Which definitions belong in JSON Schema, OpenAPI or a separate manifest, and which artifacts should be generated?
3. Where should canonicalization occur for new observations and historical payloads?
4. Which rules require offline execution, and how should rule and configuration versions travel with results?
5. Which access restrictions are actual product requirements beyond the current scopes?
6. What version negotiation and deprecation policy lets collectors and consumers upgrade independently?
7. What is the smallest hardware report that exercises both platforms and all consumer paths without expanding into a general UI engine?

## Boundaries

This proposal introduces no ERP workflow engine, approvals system, procurement module or document service. It does not replace native UI frameworks, require a plugin marketplace or make every view metadata-driven. Those capabilities need their own demonstrated ReportMate use cases.

The measure of success is practical: a new measurement or report requires less repeated interpretation, and every consumer agrees about its meaning and access rules.
