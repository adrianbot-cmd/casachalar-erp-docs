# Módulo: Clientes

Directorio de clientes de Casa Chalar. Incluye datos comerciales, condiciones de pago, listas de precio asignadas y **descuentos por defecto** que se aplican automáticamente en los comprobantes de venta.

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

### Datos básicos

| Campo | Obligatorio | Descripción |
|-------|-------------|-------------|
| Razón social / Nombre | ✅ | Nombre comercial o razón social |
| Tipo | — | Persona jurídica o física (default: Jurídica) |
| RUT / CI | — | Documento de identificación fiscal |
| Email | — | Email de contacto |
| Teléfono | — | Teléfono de contacto |
| Dirección | — | Dirección fiscal o de entrega |

### Ubicación

| Campo | Descripción |
|-------|-------------|
| Ciudad | Ciudad del cliente |
| Departamento | Departamento uruguayo |
| País | País (default: Uruguay) |

### Condiciones comerciales

| Campo | Descripción |
|-------|-------------|
| Condición de pago | Contado / 30d / 60d / 90d / 120d / 180d |
| Límite de crédito (USD) | Monto máximo de crédito en dólares |
| Lista de precios | Lista asignada al cliente (determina precios de venta) |

### Descuentos del cliente ⭐

Los descuentos son un campo clave del perfil de cada cliente. Se **heredan automáticamente** en todos los comprobantes (cotizaciones, pedidos, facturas).

| Campo | Descripción |
|-------|-------------|
| **Descuento 1 %** | Descuento de línea aplicado a cada ítem del comprobante |
| **Descuento 2 %** | Descuento financiero adicional (condición de pago) |

!!! tip "Herencia automática"
    Al seleccionar un cliente en una cotización, pedido o factura, los descuentos se pre-completan automáticamente en todos los ítems. Podés ajustarlos ítem por ítem si necesitás una excepción puntual.

!!! info "Origen legacy"
    Estos campos se corresponden con `TER_Desc1` y `TER_Desc2` del sistema legacy. Los clientes migrados ya tienen sus descuentos precargados.

### Contacto

| Campo | Descripción |
|-------|-------------|
| Nombre de contacto | Persona de referencia en el cliente |
| Cargo | Cargo del contacto (ej: Gerente de compras) |

## Formulario de creación/edición

A diferencia de otros sistemas, Casa Chalar ERP usa **formulario inline** (sin popups ni modales) para garantizar visibilidad completa de todos los campos, incluyendo los descuentos.

## Tabla de clientes

La lista de clientes muestra columnas de descuento (`Desc. 1%` y `Desc. 2%`) para identificar rápidamente los clientes con condiciones especiales.

## API

```
GET    /api/v1/clientes           → Listar clientes activos de la empresa
GET    /api/v1/clientes/{id}      → Obtener cliente por ID
POST   /api/v1/clientes           → Crear cliente
PUT    /api/v1/clientes/{id}      → Actualizar cliente
DELETE /api/v1/clientes/{id}      → Dar de baja (soft delete)
```

### Campos del response

```json
{
  "id": 1,
  "razonSocial": "Ferretería del Sur S.A.",
  "rut": "214869100015",
  "condicionPago": "30_DIAS",
  "limiteCreditoUsd": 5000.00,
  "descuento1Pct": 10.00,
  "descuento2Pct": 2.50,
  "listaPrecioId": 2,
  "listaPrecioNombre": "Precio de Lista",
  "activo": true
}
```

## Notas

- Los clientes son **independientes por empresa** (multiempresa).
- La baja es **lógica** (soft delete) — los datos no se borran.
- Los clientes con `descuento1Pct > 0` aparecen destacados en verde en la tabla.
