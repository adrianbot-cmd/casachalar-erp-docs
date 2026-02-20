# Módulo: Proveedores

Directorio de proveedores de Casa Chalar (nacionales e importadores).

## Acceso

Menú lateral → **Proveedores**

## Permisos

| Operación | Roles permitidos |
|-----------|-----------------|
| Ver listado | Todos los usuarios autenticados |
| Crear proveedor | ADMIN, SUPER_ADMIN |
| Modificar proveedor | ADMIN, SUPER_ADMIN |
| Dar de baja | ADMIN, SUPER_ADMIN |

## Campos

| Campo | Obligatorio | Descripción |
|-------|-------------|-------------|
| Razón social | ✅ | Nombre o razón social del proveedor |
| RUT | — | RUT uruguayo (o identificación fiscal del país de origen) |
| Teléfono | — | Teléfono de contacto |
| Email | — | Email de contacto |
| Dirección | — | Dirección fiscal |

## Notas

- Los proveedores son **independientes por empresa**.
- La baja es **lógica** (soft delete).
- En el futuro se asociarán a órdenes de compra y carpetas de importación.

## API

```
GET    /api/v1/proveedores           → Listar proveedores activos de la empresa
GET    /api/v1/proveedores/{id}      → Obtener proveedor por ID
POST   /api/v1/proveedores           → Crear proveedor
PUT    /api/v1/proveedores/{id}      → Actualizar proveedor
DELETE /api/v1/proveedores/{id}      → Dar de baja (soft delete)
```
