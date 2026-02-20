# Módulo: Artículos

El catálogo de artículos contiene todos los productos y repuestos que Casa Chalar comercializa.

## Acceso

Menú lateral → **Artículos**

## Permisos

| Operación | Roles permitidos |
|-----------|-----------------|
| Ver listado | Todos los usuarios autenticados |
| Crear artículo | ADMIN, SUPER_ADMIN, DEPOSITO |
| Modificar artículo | ADMIN, SUPER_ADMIN, DEPOSITO |
| Dar de baja (soft delete) | ADMIN, SUPER_ADMIN |

## Campos

| Campo | Obligatorio | Descripción |
|-------|-------------|-------------|
| Código | ✅ | Código interno del artículo (único por empresa) |
| Código de barras | — | Código de barras Code 128 |
| Descripción | ✅ | Descripción completa (hasta 300 caracteres) |
| Descripción corta | — | Nombre corto para listados (hasta 100 caracteres) |
| Unidad de medida | — | `UN`, `KG`, `MT`, etc. (default: `UN`) |

## Notas

- Los artículos son **independientes por empresa**. Cada empresa tiene su propio catálogo.
- La baja es **lógica** (soft delete): el artículo queda inactivo pero no se borra de la base de datos.
- Para el **stock** de cada artículo, ver el módulo [Stock](stock.md).

## API

```
GET    /api/v1/articulos           → Listar artículos activos de la empresa
GET    /api/v1/articulos/{id}      → Obtener artículo por ID
POST   /api/v1/articulos           → Crear artículo
PUT    /api/v1/articulos/{id}      → Actualizar artículo
DELETE /api/v1/articulos/{id}      → Dar de baja (soft delete)
```

Todos los endpoints requieren header `Authorization: Bearer {token}`.  
SUPER_ADMIN puede usar `X-Empresa-Id: {id}` para operar sobre otra empresa.
