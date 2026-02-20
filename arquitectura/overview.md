# Arquitectura del Sistema

## Stack

| Capa | Tecnología | Versión |
|------|-----------|---------|
| Backend | Spring Boot | 3.5 |
| Runtime | Java | 21 |
| Frontend | React + TypeScript | 18 |
| Build tool FE | Vite | 6 |
| Base de datos | PostgreSQL | 16 |
| ORM | JPA / Hibernate | - |
| Migraciones | Flyway | - |
| Contenedores | Docker + Compose | - |
| Registry | GCP Artifact Registry | - |
| Hosting | GCP Compute Engine (e2-micro) | - |
| CI/CD | GitHub Actions | - |
| Code review | CodeRabbit | - |

## Ambientes

| Ambiente | VM | IP | Rama |
|----------|----|----|------|
| Dev | erp-dev | 34.151.246.79 | develop |
| Prod | erp-prod | 35.198.12.188 | main |
| DB | casachalar-db (Cloud SQL) | 34.151.249.127 | - |

## Diagrama de flujo de deploy

```
[Adri escribe código]
        ↓
[PR a develop]
        ↓
[CI: risk-policy-gate → tests → CodeRabbit review]
        ↓
[Merge → build Docker → push Artifact Registry]
        ↓
[Deploy automático → erp-dev]
        ↓
[Validación manual / QA]
        ↓
[PR a main → aprobación]
        ↓
[Deploy automático → erp-prod]
```

## Estructura del repositorio

```
casachalar-erp/
├── backend/          # API REST Spring Boot
│   ├── src/main/java/com/casachalar/erp/
│   │   ├── domain/   # Entidades y lógica de negocio
│   │   ├── api/      # Controllers REST
│   │   ├── service/  # Servicios
│   │   └── security/ # Autenticación y autorización
│   └── src/main/resources/
│       └── db/migration/  # Flyway migrations
├── frontend/         # React SPA
│   └── src/
├── docs/             # Documentación técnica (este directorio)
├── .github/
│   └── workflows/    # CI/CD pipelines
├── risk-policy.yaml  # Política de riesgo Code Factory
└── docker-compose.yml
```
