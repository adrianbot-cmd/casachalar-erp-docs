# Módulo: Facturas de Proveedor (Cuentas por Pagar)

Las facturas de proveedor registran la deuda con proveedores originada en compras. Aplican el **3-way match** para garantizar que solo se pague lo que se acordó y se recibió efectivamente.

## Acceso

Menú lateral → **Compras** → **Cuentas por Pagar**

## ¿Qué es el 3-way match?

El 3-way match valida que la factura del proveedor sea coherente con la OC y la recepción:

| Verificación | Descripción |
|-------------|-------------|
| **Cantidad** | Lo facturado ≤ lo recibido (no podés pagar más de lo que llegó) |
| **Precio** | Precio facturado ≤ precio de la OC (no podés pagar más de lo acordado) |
| **Condición** | La condición de pago coincide con la acordada en la OC |

!!! warning "Si el match falla"
    Si la factura no pasa el 3-way match, el sistema muestra un error descriptivo indicando qué no coincide. En ese caso, coordiná con el proveedor para una Nota de Crédito o una factura corregida.

## Flujo de estados

```
PENDIENTE → PAGADA
          ↘ VENCIDA    (automático cuando supera fecha_vencimiento)
          ↘ ANULADA
```

| Estado | Descripción |
|--------|-------------|
| **PENDIENTE** | Factura registrada, con saldo a pagar |
| **PAGADA** | Saldo cancelado totalmente |
| **VENCIDA** | La fecha de vencimiento fue superada y sigue pendiente |
| **ANULADA** | Factura anulada |

## Registrar una factura de proveedor

1. Ir a **Compras** → **Cuentas por Pagar**
2. Clic en **Nueva Factura de Proveedor**
3. Seleccionar la **OC** (debe tener al menos una recepción registrada)
4. Completar:
   - **Número de factura** del proveedor
   - **Fecha** de la factura
   - **Condición de pago** (debe coincidir con la OC)
   - **Moneda**
5. Revisar los ítems — se precargan desde la OC con las cantidades recibidas
6. Ajustar precios si hay diferencias menores (dentro del match permitido)
7. Guardar

El sistema calcula automáticamente la **fecha de vencimiento** según la condición de pago seleccionada.

## Dashboard de Cuentas por Pagar

La pantalla principal muestra un resumen ejecutivo:

| Panel | Descripción |
|-------|-------------|
| **Total pendiente** | Suma de todas las facturas en estado PENDIENTE |
| **Total vencido** | Facturas vencidas (en rojo) — pago urgente |
| **Próximos vencimientos** | Facturas ordenadas por fecha de vencimiento |

!!! info "Pago de facturas de proveedor"
    El registro del pago efectivo de facturas de proveedor (desde Tesorería) está **pendiente de implementación**. Por ahora, el estado se puede actualizar manualmente.

## Modelo de datos

```
facturas_proveedor
├── id
├── empresa_id
├── proveedor_id
├── oc_id
├── numero_factura
├── fecha
├── condicion_pago_id
├── moneda
├── monto_total
├── monto_pendiente
├── estado                (PENDIENTE / PAGADA / VENCIDA / ANULADA)
└── fecha_vencimiento     (calculada automáticamente por condición de pago)

facturas_proveedor_items
├── id
├── factura_proveedor_id
├── articulo_id
├── cantidad_facturada
├── precio_unitario
└── iva_tasa
```

## Permisos

| Operación | Roles permitidos |
|-----------|-----------------|
| Ver facturas / dashboard | Todos los usuarios autenticados |
| Registrar factura | ADMIN, SUPER_ADMIN, COMPRAS |
| Anular factura | ADMIN, SUPER_ADMIN |

## API

```
GET  /api/v1/facturas-proveedor                  → Listar facturas de proveedor
GET  /api/v1/facturas-proveedor/dashboard        → Resumen CxP (totales + alertas)
POST /api/v1/facturas-proveedor                  → Registrar factura (aplica 3-way match)
PUT  /api/v1/facturas-proveedor/{id}/anular      → Anular factura
```

### Ejemplo: registrar factura de proveedor

```json
POST /api/v1/facturas-proveedor
{
  "ocId": 7,
  "proveedorId": 55,
  "numeroFactura": "A-0001234",
  "fecha": "2026-02-24",
  "condicionPagoId": 3,
  "moneda": "USD",
  "items": [
    {
      "articuloId": 234,
      "cantidadFacturada": 15,
      "precioUnitario": 448.50,
      "ivaTasa": 22
    }
  ]
}
```
