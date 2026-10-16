# Conciliaciones: Case management platform for labor conciliators

A full-stack platform built solo, end to end, for the labor conciliators designated under DICLO (Dirección de Conciliación Laboral Obligatoria), a newly created mandatory labor conciliation authority in Santiago del Estero, Argentina.

The project was developed under the internal codename *Rivendel*.

## The Problem

When DICLO was established and its conciliators were designated, they had no system to manage their hearings or to issue the resulting conciliation records (actas). Everything depended on manual, ad hoc processes for a function that now had legal weight and recurring volume. I identified this gap and built Conciliaciones to fill it — not as an assigned task, but as a self-initiated project to solve a real, unaddressed institutional need.

## My Role

Solo full-stack developer. I owned every layer: domain modeling, backend architecture, frontend implementation, infrastructure, and deployment.

## Architecture

- **Frontend:** React + Vite
- **Backend:** NestJS + Prisma ORM
- **Database:** MySQL
- **Deployment:** Vercel (frontend), Render (backend), Aiven (MySQL)

The three deployment targets live on different domains, which meant solving cross-domain session persistence (secure, partitioned, `sameSite: 'none'` cookies) rather than relying on same-origin defaults — one of several production concerns that don't show up in a local dev environment.

## Key Technical Decisions

**Domain-specific form state over a form library.** The core claims module (*Reclamos*) involves interdependent steps, conditional fields, and dynamic document templates. Rather than adopt a general-purpose form library, I built a custom `useReducer`-based hook (`useReclamoForm`) to centralize state transitions into predictable, unit-testable actions. It cost more boilerplate upfront, but gave precise control over domain logic that a generic library would have fought against.

**Dynamic, legally accurate PDF generation.** Conciliation records aren't a single static template — content and layout change based on resolution type and on how many claimants and respondents are involved, down to whether each party’s and counsel’s signature line appears, which depends on whether that party was actually present at the hearing. Getting this right meant encoding real procedural rules into the document pipeline, not just formatting data.

**Diagnosing TLS and driver-level database connectivity issues with Aiven.** Connecting the NestJS backend to a managed MySQL instance on Aiven surfaced a chain of infrastructure-level problems that don't show up in local development: DNS resolution failures, TLS/SSL handshake errors with the MariaDB client, and IP allowlist restrictions that had to be configured against Render's dynamic outbound IPs. Resolving this required working through certificate handling via `NODE_EXTRA_CA_CERTS`, careful staged testing of IP filtering rules to avoid locking out the production database, and debugging shadow-database permission errors and migration checksum drift along the way — the kind of low-level, cross-service debugging that rarely gets exercised until an app actually leaves localhost.