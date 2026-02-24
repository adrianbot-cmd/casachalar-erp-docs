# Módulo: Cotizaciones de Compra

Las cotizaciones de compra permiten solicitar y comparar precios de distintos proveedores antes de emitir una Orden de Compra. Se puede tener N cotizaciones por cada requerimiento, una por proveedor.

## Acceso

Menú lateral → **Compras** → **Cotizaciones**

## Flujo de estados

```
PENDIENTE → RECIBIDA → ADJUDICADA
                     ↘ RECHAZADA
```

| Estado | Descripción |
|--------|-------------|
| **PENDIENTE** | Cotización enviada al proveedor, esperando respuesta |
| **RECIBIDA** | El proveedor respondió con precios y condiciones |
| **ADJUDICADA** | Se eligió esta cotización. Generó una Orden de Compra automáticamente |
| **RECHAZADA** | No fue seleccionada (descartada al adjudicar otra) |

## Crear una cotización de compra

1. Ir a **Compras** → **Cotizaciones**
2. Clic en **Nueva Cotización**
3. Seleccionar el **Requerimiento** (debe estar en estado APROBADO)
4. Seleccionar el **Proveedor** al que se le pide precio
5. Los ítems del requerimiento se precargan automáticamente
6. Completar por ítem:
   - **Precio unitario**
   - **Moneda** (UYU / USD)
   - **Condición de pago**
7. Guardar — queda en estado **PENDIENTE**

!!! note "Múltiples cotizaciones por requerimiento"
    Podés crear tantas cotizaciones como proveedores quieras consultar para el mismo requerimiento. Cada una es independiente y tiene su propio proveedor y precios.

## Registrar la respuesta del proveedor

Cuando el proveedor responde con precios:

1. Abrir la cotización en estado PENDIENTE
2. Completar los precios recibidos
3. Clic en **Marcar como recibida**
4. El estado pasa a **RECIBIDA**

## Adjudicar una cotización

Una vez que tenés las respuestas de los proveedores:

1. Ir a la lista de cotizaciones del requerimiento
2. Comparar precios y condiciones
3. Clic en **Adjudicar** en la cotización ganadora

Al adjudicar, el sistema automáticamente (en una sola transacción):

1. ✅ La cotización adjudicada pasa a **ADJUDICADA**
2. ✅ Las otras cotizaciones del mismo requerimiento pasan a **RECHAZADA**
3. ✅ Se genera una **Orden de Compra** con los datos de la cotización
4. ✅ El requerimiento asociado pasa a **PROCESADO**

!!! info "Adjudicación atómica"
    Todo el proceso de adjudicación ocurre en una única transacción. Si algo falla, ningún cambio queda a medias.

## Modelo de datos

```
cotizaciones_compra
├── id
├── requerimiento_id
├── proveedor_id
├── estado             (PENDIENTE / RECIBIDA / ADJUDICADA / RECHAZADA)
├── condicion_pago_id
└── observaciones

cotizaciones_compra_items
├── id
├── cotizacion_id
├── articulo_id
├── cantidad
├── precio_unitario
└── moneda
```

## Permisos

| Operación | Roles permitidos |
|-----------|-----------------|
| Ver cotizaciones | Todos los usuarios autenticados |
| Crear / editar | ADMIN, SUPER_ADMIN, COMPRAS |
| Adjudicar | ADMIN, SUPER_ADMIN |

## API

```
GET  /api/v1/cotizaciones-compra                         → Listar cotizaciones
GET  /api/v1/cotizaciones-compra?requerimientoId={id}    → Cotizaciones de un requerimiento
POST /api/v1/cotizaciones-compra                         → Crear cotización
PUT  /api/v1/cotizaciones-compra/{id}/recibir            → Marcar como recibida
PUT  /api/v1/cotizaciones-compra/{id}/adjudicar          → Adjudicar (genera OC automáticamente)
```

### Ejemplo: crear cotización de compra

```json
POST /api/v1/cotizaciones-compra
{
  "requerimientoId": 12,
  "proveedorId": 55,
  "condicionPagoId": 3,
  "observaciones": "Pedir descuento por volumen",
  "items": [
    {
      "articuloId": 234,
      "cantidad": 20,
      "precioUnitario": 450.00,
      "moneda": "USD"
    }
  ]
}
```
