# Módulo: Órdenes de Compra

La Orden de Compra (OC) es el documento formal que se envía al proveedor para confirmar la compra. Define qué se compra, en qué cantidad, a qué precio y para cuándo se necesita.

## Acceso

Menú lateral → **Compras** → **Órdenes de Compra**

## Flujo de estados

```
ABIERTA → PARCIALMENTE_RECIBIDA → COMPLETADA
        ↘ ANULADA
```

| Estado | Descripción |
|--------|-------------|
| **ABIERTA** | OC emitida, sin recepciones o recepción parcial aún no registrada |
| **PARCIALMENTE_RECIBIDA** | Se recibió parte de la mercadería, queda saldo pendiente |
| **COMPLETADA** | Se recibió toda la mercadería (dentro de la tolerancia configurada) |
| **ANULADA** | OC cancelada. No se puede anular si ya tiene recepciones registradas |

## Crear una OC

Una OC puede crearse de dos formas:

### Automáticamente (desde adjudicación)

Al adjudicar una cotización de compra, la OC se genera automáticamente con todos los datos de la cotización ganadora.

### Manualmente

1. Ir a **Compras** → **Órdenes de Compra**
2. Clic en **Nueva Orden de Compra**
3. Completar:
   - **Proveedor**
   - **Condición de pago**
   - **Dirección de entrega** *(opcional)*
   - **Fecha de necesidad** — cuándo se necesita recibida la mercadería
   - **Tolerancia de recepción (%)** — porcentaje de diferencia aceptable entre lo pedido y lo recibido (default: 5%)
   - **Activo fijo** — marcar si la compra es un bien de uso que se va a activar
   - **Observaciones** *(opcional)*
4. Agregar ítems:
   - Artículo, cantidad, precio unitario, moneda, tasa de IVA
5. Guardar

## Número de OC

El sistema asigna automáticamente un número correlativo por empresa con formato `OC-YYYY-NNN`:

- Ej: `OC-2026-001`, `OC-2026-002`, ...

## Tolerancia de recepción

El campo **Tolerancia (%)** permite recibir una cantidad ligeramente distinta a la pedida sin que se considere un error:

- Si pediste 100 unidades con tolerancia 5%, podés recibir entre 95 y 105 sin que requiera autorización especial
- Si la recepción supera la tolerancia, el sistema muestra una advertencia

## Anular una OC

Solo se puede anular una OC que **no tenga recepciones registradas**. Una vez que se recibió aunque sea un ítem, la OC no puede anularse.

## Permisos

| Operación | Roles permitidos |
|-----------|-----------------|
| Ver OC | Todos los usuarios autenticados |
| Crear / editar OC | ADMIN, SUPER_ADMIN, COMPRAS |
| Anular OC | ADMIN, SUPER_ADMIN |

## Modelo de datos

```
ordenes_compra
├── id
├── empresa_id
├── numero                    (OC-YYYY-NNN)
├── fecha
├── proveedor_id
├── condicion_pago_id
├── direccion_entrega_id
├── fecha_necesidad
├── tolerancia_recepcion_pct  (default: 5)
├── activo_fijo               (boolean)
├── estado
└── observaciones

ordenes_compra_items
├── id
├── oc_id
├── articulo_id
├── cantidad
├── cantidad_recibida         (actualizado por recepciones)
├── precio_unitario
├── moneda
└── iva_tasa
```

## API

```
GET  /api/v1/ordenes-compra              → Listar OC de la empresa
GET  /api/v1/ordenes-compra/{id}         → Detalle de una OC
POST /api/v1/ordenes-compra              → Crear OC
PUT  /api/v1/ordenes-compra/{id}         → Editar OC (solo estado ABIERTA)
PUT  /api/v1/ordenes-compra/{id}/anular  → Anular OC
```

### Ejemplo: crear OC manualmente

```json
POST /api/v1/ordenes-compra
{
  "proveedorId": 55,
  "condicionPagoId": 3,
  "fechaNecesidad": "2026-03-15",
  "toleranciaRecepcionPct": 5,
  "activoFijo": false,
  "observaciones": "Entregar en depósito central",
  "items": [
    {
      "articuloId": 234,
      "cantidad": 20,
      "precioUnitario": 450.00,
      "moneda": "USD",
      "ivaTasa": 22
    }
  ]
}
```
