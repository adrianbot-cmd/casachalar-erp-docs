# Módulo: Vendedores

El maestro de vendedores permite registrar a los representantes de ventas de la empresa. Los vendedores se asocian a los comprobantes de venta (cotizaciones, pedidos, facturas) para llevar el seguimiento de cada operación por responsable.

## Acceso

Menú lateral → **Configuración** → **Vendedores**

## Permisos

| Operación | Roles permitidos |
|-----------|-----------------|
| Ver listado | Todos los usuarios autenticados |
| Crear vendedor | ADMIN, SUPER_ADMIN |
| Modificar vendedor | ADMIN, SUPER_ADMIN |
| Dar de baja | ADMIN, SUPER_ADMIN |

## Campos

| Campo | Obligatorio | Descripción |
|-------|-------------|-------------|
| Nombre | ✅ | Nombre completo del vendedor |
| Código | — | Código corto para identificación (ej: "JG", "CB") |
| Email | — | Email de contacto del vendedor |
| Teléfono | — | Teléfono de contacto |
| Comisión % | — | Porcentaje de comisión por venta (informativo) |
| Activo | ✅ | Estado activo/inactivo |

## Crear un vendedor

1. Ir a **Configuración** → **Vendedores**
2. Hacer clic en **Nuevo Vendedor**
3. Completar los datos requeridos
4. Guardar

## Relación con comprobantes de venta

Al crear o editar una cotización, pedido o factura, se puede asignar el vendedor responsable. Este dato queda registrado en el comprobante y permite:

- Filtrar comprobantes por vendedor
- Reportes de ventas por vendedor
- Seguimiento de cartera de clientes por vendedor

!!! tip "Vendedor en el cliente"
    Se puede asignar un vendedor por defecto a cada cliente. Al crear un nuevo comprobante para ese cliente, el vendedor se pre-completa automáticamente.

## Dar de baja un vendedor

La baja es **lógica** (soft delete). El vendedor queda inactivo y no aparece en los selectores de nuevos comprobantes, pero los comprobantes históricos conservan su referencia.

## Diferencia con Usuarios

Los vendedores son un **maestro de datos** independiente de los usuarios del sistema. Un vendedor puede:
- **Tener un usuario asociado** (el mismo vendedor opera el sistema)
- **No tener usuario** (vendedor externo o de campo que no accede al ERP)

Un usuario del sistema con rol VENDEDOR no es automáticamente un vendedor del maestro; deben configurarse por separado si se quiere el seguimiento por vendedor en comprobantes.

## Modelo de datos

```
vendedores
├── id
├── empresa_id
├── nombre
├── codigo
├── email
├── telefono
├── comision_pct
└── activo
```

## API

```
GET    /api/v1/vendedores           → Listar vendedores activos de la empresa
GET    /api/v1/vendedores/{id}      → Obtener vendedor por ID
POST   /api/v1/vendedores           → Crear vendedor
PUT    /api/v1/vendedores/{id}      → Actualizar vendedor
DELETE /api/v1/vendedores/{id}      → Dar de baja (soft delete)
```
