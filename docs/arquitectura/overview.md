# Arquitectura del Sistema

## Stack tecnológico

| Capa | Tecnología | Versión |
|------|-----------|---------|
| Backend | Spring Boot | 3.5 |
| Runtime | Java | 21 |
| Frontend | React + TypeScript | 18 |
| Build tool FE | Vite | 6 |
| Base de datos | PostgreSQL | 16 (Cloud SQL) |
| ORM | JPA / Hibernate | — |
| Migraciones | Flyway | V1–V9 |
| Contenedores | Docker + Compose | — |
| Registry | GCP Artifact Registry | — |
| Hosting | GCP Compute Engine (e2-micro) | southamerica-east1-c |
| CI/CD | GitHub Actions | — |
| Code review | CodeRabbit | — |

## Ambientes

| Ambiente | VM | IP pública | Rama |
|----------|----|----|------|
| Dev | erp-dev | 34.151.246.79 | develop |
| Prod | erp-prod | 35.198.12.188 | main |
| DB | casachalar-db (Cloud SQL) | 34.151.249.127 | — |

## Flujo de deploy

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
[Validación manual / QA con Julio y Carlos]
        ↓
[PR develop → main]
        ↓
[Deploy automático → erp-prod]
```

## Estructura del repositorio

```
casachalar-erp/
├── backend/
│   └── src/main/java/com/casachalar/erp/
│       ├── api/          # Controllers REST + DTOs
│       ├── model/        # Entidades JPA
│       ├── repository/   # Spring Data repos
│       ├── service/      # Lógica de negocio
│       ├── security/     # JWT + Spring Security
│       └── config/       # OpenAPI, configuración
│   └── src/main/resources/
│       └── db/migration/ # Flyway migrations V1–V9
├── frontend/
│   └── src/
│       ├── pages/        # Pantallas principales
│       ├── components/   # AppLayout, sidebar
│       ├── contexts/     # AuthContext (JWT)
│       └── services/     # API client (axios)
├── .github/
│   └── workflows/        # CI/CD pipelines
├── risk-policy.yaml      # Política de riesgo
└── docker-compose.yml
```

## Modelo de datos (migraciones Flyway)

| Versión | Descripción |
|---------|-------------|
| V1 | Creación de `empresas` |
| V2 | Creación de `sucursales` |
| V3 | Creación de `roles` y `usuarios` |
| V4 | Creación de `tipo_cambio` |
| V5 | Creación de `articulos` |
| V6 | Fix hash de contraseña admin (BCrypt) |
| V7 | Creación de `proveedores` |
| V8 | Creación de `clientes` |
| V9 | Creación de `stock_articulo` y `movimientos_stock` |

## API REST — Endpoints disponibles

Todos los endpoints requieren autenticación JWT (Bearer token).  
Header opcional para SUPER_ADMIN: `X-Empresa-Id: {id}`

| Módulo | Base URL | Operaciones |
|--------|----------|-------------|
| Auth | `POST /api/v1/auth/login` | Login → token JWT |
| Artículos | `/api/v1/articulos` | GET, GET/{id}, POST, PUT/{id}, DELETE/{id} |
| Proveedores | `/api/v1/proveedores` | GET, GET/{id}, POST, PUT/{id}, DELETE/{id} |
| Clientes | `/api/v1/clientes` | GET, GET/{id}, POST, PUT/{id}, DELETE/{id} |
| Usuarios | `/api/v1/usuarios` | GET, GET/{id}, POST, PUT/{id}, DELETE/{id} |
| Stock | `/api/v1/stock` | GET (todos), GET/sucursal/{id}, POST /ajuste |
| Sucursales | `/api/v1/sucursales` | GET (solo lectura) |
| Roles | `/api/v1/roles` | GET (solo lectura) |
| Tipo de cambio | `/api/v1/tipo-cambio` | GET /hoy, GET/{fecha}, POST /manual |
| Health | `GET /api/v1/health` | Estado del servicio |

### Documentación interactiva (Swagger)

- Dev: `http://34.151.246.79/swagger-ui.html`
- Prod: `http://35.198.12.188/swagger-ui.html`

## Seguridad

### Roles del sistema

| Rol | Descripción | Alcance |
|-----|-------------|---------|
| `SUPER_ADMIN` | Acceso total a todas las empresas | Multi-empresa |
| `ADMIN` | Administrador de su empresa | Por empresa |
| `DEPOSITO` | Gestión de stock y artículos | Por empresa |
| _(más roles a definir con Carlos)_ | — | — |

### Flujo de autenticación

```
POST /api/v1/auth/login
  { "username": "...", "password": "..." }
  → { "token": "eyJ..." }

Requests siguientes:
  Authorization: Bearer eyJ...
```

## Multiempresa

El sistema soporta múltiples empresas desde el inicio:

- Usuarios regulares operan siempre sobre **su empresa**
- `SUPER_ADMIN` puede operar sobre cualquier empresa via header `X-Empresa-Id`
- El stock, artículos y maestros son independientes por empresa
- Intercompany (transferencias entre empresas) está contemplado en la arquitectura, implementación futura
