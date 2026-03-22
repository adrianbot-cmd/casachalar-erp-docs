# Módulo: Sucursales

Las sucursales representan los puntos de operación físicos de la empresa (locales comerciales, depósitos habilitados para venta, etc.). Cada sucursal tiene su propio depósito de stock, caja y personal asignado.

## Acceso

Menú lateral → **Configuración** → **Sucursales**

## Permisos

| Operación | Roles permitidos |
|-----------|-----------------|
| Ver sucursales | Todos los usuarios autenticados |
| Crear sucursal | ADMIN, SUPER_ADMIN |
| Modificar sucursal | ADMIN, SUPER_ADMIN |
| Dar de baja | SUPER_ADMIN |

## Campos

### Datos básicos

| Campo | Obligatorio | Descripción |
|-------|-------------|-------------|
| Nombre | ✅ | Nombre de la sucursal (ej: "Casa Central", "Sucursal Montevideo") |
| Código | ✅ | Código corto para identificación interna (ej: "CC", "MVD") |
| Dirección | — | Dirección física de la sucursal |
| Teléfono | — | Teléfono de la sucursal |
| Email | — | Email de contacto de la sucursal |

### Configuración de stock

| Campo | Obligatorio | Descripción |
|-------|-------------|-------------|
| **Depósito de stock** (`deposito_stock_id`) | ✅ | Depósito del que se descuenta el stock al generar facturas de venta |

!!! warning "Depósito de stock"
    Este campo es crítico. Define **de qué depósito se descuenta la mercadería** cuando se confirma una venta en esta sucursal. Si no está configurado, el sistema no podrá procesar las salidas de stock correctamente.

!!! tip "Asignación de depósito"
    El depósito debe existir previamente en el módulo [Depósitos](depositos.md). Generalmente cada sucursal tiene su propio depósito PROPIO asociado.

## Crear una sucursal

1. Ir a **Configuración** → **Sucursales**
2. Hacer clic en **Nueva Sucursal**
3. Completar los datos básicos (nombre, código)
4. Seleccionar el **Depósito de stock** correspondiente
5. Completar datos opcionales (dirección, teléfono, email)
6. Guardar

## Editar una sucursal

1. En el listado, hacer clic en el ícono de edición de la sucursal
2. Modificar los campos necesarios
3. Guardar cambios

!!! note "Cambio de depósito de stock"
    Si cambiás el depósito de stock de una sucursal, el cambio aplica a partir de ese momento. Las ventas anteriores no se modifican retroactivamente.

## Dar de baja una sucursal

La baja es **lógica** (soft delete). La sucursal queda inactiva y no aparece en los selectores de nuevos comprobantes, pero el historial de operaciones se conserva.

!!! warning "Antes de dar de baja"
    Verificá que la sucursal no tenga caja abierta ni operaciones pendientes.

## Relación con otros módulos

| Módulo | Relación |
|--------|----------|
| **Cajas** | Cada sucursal tiene una o más cajas. Al crear la sucursal, se genera automáticamente una "Caja Principal". |
| **Stock** | El campo `deposito_stock_id` determina de qué depósito se descuenta al vender. |
| **Facturas** | Al crear una factura, se selecciona la sucursal emisora. |
| **Usuarios** | Los usuarios pueden tener una sucursal asignada por defecto. |

## Modelo de datos

```
sucursales
├── id
├── empresa_id
├── nombre
├── codigo
├── direccion
├── telefono
├── email
├── deposito_stock_id   → FK a depositos (el depósito que descuenta en ventas)
└── activo
```

## API

```
GET    /api/v1/sucursales           → Listar sucursales activas de la empresa
GET    /api/v1/sucursales/{id}      → Obtener sucursal por ID
POST   /api/v1/sucursales           → Crear sucursal
PUT    /api/v1/sucursales/{id}      → Actualizar sucursal
DELETE /api/v1/sucursales/{id}      → Dar de baja (soft delete)
```

### Ejemplo: crear sucursal

```json
POST /api/v1/sucursales
{
  "nombre": "Casa Central",
  "codigo": "CC",
  "direccion": "Av. 18 de Julio 1234, Montevideo",
  "depositoStockId": 1
}
```
