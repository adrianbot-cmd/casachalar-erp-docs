# Módulo: Bancos

Los bancos son un maestro que referencia las instituciones bancarias. Se usan principalmente para identificar el banco librador o depositario de un cheque.

## Acceso

Menú lateral → **Tesorería** → **Bancos**

## Bancos pre-cargados

Al inicializar el sistema, se cargan automáticamente los principales bancos uruguayos:

| Banco | Código BCU |
|-------|-----------|
| Banco de la República Oriental del Uruguay (BROU) | 001 |
| Itaú | 113 |
| Santander | 137 |
| HSBC | 128 |
| Scotiabank | 149 |
| BBVA | 091 |
| Banco Heritage | 162 |
| OCA | — |
| Banco Hipotecario del Uruguay (BHU) | 091 |
| Banco República | 001 |

!!! info "Bancos globales"
    Los bancos pre-cargados son globales (no pertenecen a ninguna empresa específica) y están disponibles para todas las empresas del sistema.

## Agregar un banco

Si necesitás un banco que no está en la lista:

1. Ir a **Tesorería** → **Bancos**
2. Clic en **Nuevo Banco**
3. Completar:
   - **Nombre**: nombre del banco
   - **Código BCU** *(opcional)*: código de identificación del Banco Central del Uruguay
4. Guardar

## Permisos

| Operación | Roles permitidos |
|-----------|-----------------|
| Ver bancos | Todos los usuarios autenticados |
| Crear / editar bancos | ADMIN, SUPER_ADMIN |

## Modelo de datos

```
bancos
├── id
├── nombre
├── codigo_bcu        (código BCU, opcional)
├── empresa_id        (null = banco global, pre-cargado del sistema)
└── activo
```

## API

```
GET  /api/v1/bancos           → Listar bancos disponibles (globales + de la empresa)
POST /api/v1/bancos           → Crear banco
PUT  /api/v1/bancos/{id}      → Editar banco
```
