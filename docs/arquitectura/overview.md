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
| Migraciones | Flyway | V1–V34 |
| Contenedores | Docker + Compose | — |
| Registry | GCP Artifact Registry | — |
| Hosting | GCP Compute Engine (e2-micro) | southamerica-west1-a (dev) / southamerica-east1-c (prod) |
| CI/CD | GitHub Actions | — |
| Code review | CodeRabbit | — |

## Ambientes

| Ambiente | VM | IP pública | Rama |
|----------|----|----|------|
| Dev | erp-dev | 34.176.45.29 | develop |
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
│       └── db/migration/ # Flyway migrations V1–V21
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
| V10 | Creación de `categorias_articulo` y `marcas` |
| V11 | Extensión de `articulos` (categoria, marca, iva_tasa, peso, proveedor_habitual) |
| V12 | Creación de `listas_precio` y `articulo_lista_precio` |
| V13 | Extensión de `clientes` (condicion_pago, limite_credito, lista_precio, ubicacion, contacto) |
| V14 | Creación de `vendedores` |
| V15 | Creación de `cotizaciones` y `cotizacion_items` |
| V16 | Creación de `pedidos` y `pedido_items` |
| V17 | Creación de `facturas` y `factura_items` |
| V18 | Creación de `recibos_cobro`, `recibo_facturas` y `recibo_medios_pago` |
| V19 | Datos de vendedores iniciales |
| V20 | Migración de vendedores desde sistema legacy |
| V21 | Extensión de `clientes` con `descuento1_pct` y `descuento2_pct` |
| V22 | `fecha_vencimiento` en facturas + autoEmitirYCobrar (boleta contado) |
| V23 | Unidades de medida y condiciones de pago |
| V24 | Módulo Depósitos (PROPIO / ZF / CONSIGNACION) |
| V25–V26 | Jerarquía Rubro → Familia → Categoría |
| V27 | Direcciones de entrega |
| V28–V30 | Bancos + Cheques (COMUN/DIFERIDO, PROPIO/TERCEROS, estados) |
| V31 | Módulo Compras — Órdenes de Compra + ítems |
| V32 | Recepciones de Compra (stock ENTRADA, costo promedio) |
| V33 | Facturas Proveedor — CxP con 3-way match |
| V34 | Requerimientos + Cotizaciones de Compra (adjudicación automática) |

## API REST — Endpoints disponibles

Todos los endpoints requieren autenticación JWT (Bearer token).  
Header opcional para SUPER_ADMIN: `X-Empresa-Id: {id}`

| Módulo | Base URL | Operaciones principales |
|--------|----------|--------------------------|
| Auth | `/api/v1/auth` | POST /login |
| Artículos | `/api/v1/articulos` | CRUD + GET /selector + GET /mejor-precio |
| Categorías | `/api/v1/categorias-articulo` | CRUD |
| Marcas | `/api/v1/marcas` | CRUD |
| Listas de Precio | `/api/v1/listas-precio` | CRUD + GET /{id}/precios |
| Proveedores | `/api/v1/proveedores` | CRUD |
| Clientes | `/api/v1/clientes` | CRUD + GET /selector + GET /page |
| Vendedores | `/api/v1/vendedores` | CRUD |
| Usuarios | `/api/v1/usuarios` | CRUD |
| Stock | `/api/v1/stock` | GET + GET /sucursal/{id} + POST /ajuste |
| Sucursales | `/api/v1/sucursales` | GET (lectura) |
| Roles | `/api/v1/roles` | GET (lectura) |
| Tipo de cambio | `/api/v1/tipo-cambio` | GET /hoy + GET /{fecha} + POST /manual |
| Cotizaciones | `/api/v1/cotizaciones` | CRUD + POST /{id}/estado + GET /para-pedido |
| Pedidos | `/api/v1/pedidos` | CRUD + POST /{id}/confirmar + GET /para-facturar |
| Facturas | `/api/v1/facturas` | POST + POST /{id}/emitir + POST /{id}/anular + GET /cliente/{id}/pendientes |
| Recibos Cobro | `/api/v1/recibos-cobro` | POST + POST /{id}/aplicar + POST /{id}/anular |
| Depósitos | `/api/v1/depositos` | CRUD |
| Cheques | `/api/v1/cheques` | GET (filtros) + PUT /{id}/depositar + PUT /{id}/descontar + PUT /{id}/endosar + PUT /{id}/rechazar |
| Bancos | `/api/v1/bancos` | CRUD |
| Cashflow | `/api/{empresaId}/cashflow` | GET (cheques EN_CARTERA + CxC) |
| Categorías (jerarquía) | `/api/v1/rubros` `/api/v1/familias` | CRUD |
| Direcciones Entrega | `/api/v1/direcciones-entrega` | CRUD |
| Órdenes de Compra | `/api/v1/ordenes-compra` | CRUD + POST /{id}/anular |
| Recepciones Compra | `/api/v1/recepciones-compra` | POST + GET |
| Facturas Proveedor | `/api/v1/facturas-proveedor` | POST + GET |
| Requerimientos | `/api/v1/requerimientos` | CRUD + POST /{id}/estado |
| Cotizaciones Compra | `/api/v1/cotizaciones-compra` | CRUD + POST /{id}/adjudicar |
| Health | `/api/v1/health` | GET |

### Documentación interactiva (Swagger)

- Dev: `http://34.176.45.29/swagger-ui.html`
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
