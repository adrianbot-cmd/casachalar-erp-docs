# Módulo: Clientes

Directorio de clientes de Casa Chalar.

## Acceso

Menú lateral → **Clientes**

## Permisos

| Operación | Roles permitidos |
|-----------|-----------------|
| Ver listado | Todos los usuarios autenticados |
| Crear cliente | ADMIN, SUPER_ADMIN |
| Modificar cliente | ADMIN, SUPER_ADMIN |
| Dar de baja | ADMIN, SUPER_ADMIN |

## Campos

| Campo | Obligatorio | Descripción |
|-------|-------------|-------------|
| Razón social | ✅ | Nombre o razón social del cliente |
| RUT | — | RUT uruguayo |
| Teléfono | — | Teléfono de contacto |
| Email | — | Email de contacto |
| Dirección | — | Dirección fiscal o de entrega |

## Notas

- Los clientes son **independientes por empresa**.
- La baja es **lógica** (soft delete).

## API

```
GET    /api/v1/clientes           → Listar clientes activos de la empresa
GET    /api/v1/clientes/{id}      → Obtener cliente por ID
POST   /api/v1/clientes           → Crear cliente
PUT    /api/v1/clientes/{id}      → Actualizar cliente
DELETE /api/v1/clientes/{id}      → Dar de baja (soft delete)
```
