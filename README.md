# goodfood-ms-sav

Customer support & ticketing microservice for the [GoodFood](https://github.com/RMurier/BAC-5-CUBE-1-COLLABORATIF) platform. ("SAV" = *Service Après-Vente*.)

**Status:** 🚧 Scaffold — this is currently an unmodified `dotnet new webapi` template (only the sample `/weatherforecast` endpoint). No ticketing logic has been written yet. See [`goodfood-ms-auth`](https://github.com/RMurier/goodfood-ms-auth#readme) for what a fleshed-out service in this platform looks like.

## Table of Contents

- [Intended Purpose](#intended-purpose)
- [Tech Stack](#tech-stack)
- [Environment Variables](#environment-variables)
- [Running Locally](#running-locally)
- [Tests](#tests)
- [CI/CD](#cicd)

## Intended Purpose

Own customer support tickets — complaints, refund requests, order issues — likely referencing orders from `ms-commandes` and triggering messages via `ms-notifications`.

## Tech Stack

- .NET 9 / ASP.NET Core Web API
- SQL Server, via `Microsoft.EntityFrameworkCore.SqlServer` (not yet added — connection string is wired up, no `DbContext` exists yet)

## Environment Variables

| Variable | Description |
|----------|--------------|
| `ASPNETCORE_ENVIRONMENT` | `Development` or `Production` |
| `ConnectionStrings__DefaultConnection` | SQL Server connection string (database `GoodFood_SAV_Dev` in dev) |

## Running Locally

### Via the platform's docker-compose

From the [parent repo](https://github.com/RMurier/BAC-5-CUBE-1-COLLABORATIF):

```bash
docker compose -f docker-compose.dev.yml up -d db-sql-dev ms-sav-dev
```

Runs on `http://localhost:3006`, Swagger at `http://localhost:3006/swagger`.

### Standalone

```bash
cd GoodFood.Sav.Api
dotnet restore
dotnet run
```

## Tests

```bash
dotnet test GoodFood.Sav.Tests/GoodFood.Sav.Tests.csproj --verbosity normal
```

Currently a single sanity check ([`SanityTests.cs`](GoodFood.Sav.Tests/SanityTests.cs)), there to keep the CI test job green while the service is empty.

## CI/CD

Built, scanned (SonarQube, Trivy, OWASP Dependency-Check, GitGuardian) and published on every push, gated on all of them passing — see the [parent repo's CI/CD Pipeline section](https://github.com/RMurier/BAC-5-CUBE-1-COLLABORATIF#cicd-pipeline) for how the pipeline is wired across repos.
