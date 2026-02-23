# Módulo: Recibos de Cobro

Los recibos de cobro registran los pagos recibidos de clientes y los aplican contra las facturas pendientes. Soportan cobros de **múltiples facturas en un solo recibo** y **cobros parciales**.

## Acceso

Menú lateral → **Ventas** → **Recibos Cobro**

## Ciclo de vida

```
BORRADOR → APLICADO
         ↘ ANULADO
```

| Estado | Descripción |
|--------|-------------|
| **BORRADOR** | Recibo creado, aún no aplicado a facturas. |
| **APLICADO** | Cobro registrado. Las facturas reducen su saldo pendiente. |
| **ANULADO** | Recibo anulado. Las facturas recuperan el saldo. |

!!! info "Recibos automáticos"
    Las **Boletas Contado** generan automáticamente un recibo en estado APLICADO al ser creadas. No es necesario crearlo manualmente.

---

## Crear un recibo de cobro

### Paso 1: Seleccionar cliente

Buscá y seleccioná el cliente. Aparecen todas sus facturas con saldo pendiente.

### Paso 2: Seleccionar facturas a cobrar

- Hacé clic en una fila para seleccionar (o deseleccionar) una factura
- Podés seleccionar varias facturas del mismo cliente en un solo recibo
- La columna **"A cobrar $"** muestra el monto a aplicar (igual al saldo por defecto)
- Para **cobros parciales**: modificá el campo "A cobrar" al monto efectivo que pagó el cliente

!!! note "Moneda"
    Todos los montos se muestran en **pesos uruguayos (UYU)**, que es como el sistema almacena los valores internamente.

### Paso 3: Detalle del cobro

- **Medio de pago**: Efectivo / Transferencia / Cheque / Tarjeta
- **Fecha de vencimiento del cheque**: aparece solo al seleccionar Cheque — ingresá la fecha del cheque *(obligatorio)*
- **Observaciones**: nota opcional (ej. número de transferencia, banco)

El total se calcula automáticamente sumando los montos de todas las facturas seleccionadas.

Hacé clic en **Registrar Cobro** para guardar en BORRADOR.

---

## Medios de pago

| Medio | Campo adicional |
|-------|----------------|
| **Efectivo** | — |
| **Transferencia** | Observaciones (número de transferencia) |
| **Cheque** | **Fecha de vencimiento del cheque** *(obligatorio)* |
| **Tarjeta** | — |

!!! warning "Cheques"
    Al cobrar con cheque, siempre ingresá la **fecha de vencimiento**. El sistema la guarda en el recibo y es fundamental para el seguimiento de cheques en cartera.

---

## Aplicar un recibo

Un recibo en BORRADOR no descuenta el saldo de las facturas. Para registrarlo:

1. En la tabla de recibos, buscá el recibo en estado BORRADOR
2. Hacé clic en **Aplicar**
3. El saldo de cada factura se actualiza automáticamente
4. Las facturas totalmente cobradas pasan a "✓ cobrada" (saldo = $0)

---

## Anular un recibo

Solo recibos APLICADO pueden anularse. Al anular:

- Las facturas recuperan el saldo correspondiente
- El recibo queda en estado ANULADO (histórico, no se elimina)

---

## Ver detalle

Hacé clic en **Ver** para abrir el detalle lateral con:
- Datos del cliente y fecha
- Listado de facturas cobradas con monto aplicado
- Medio de pago (y fecha de vencimiento del cheque, si aplica)
- Total cobrado

---

## API

```
GET    /api/v1/recibos-cobro                        → Listar recibos de la empresa
POST   /api/v1/recibos-cobro                        → Crear recibo
POST   /api/v1/recibos-cobro/{id}/aplicar           → Aplicar recibo (descuenta saldo facturas)
POST   /api/v1/recibos-cobro/{id}/anular            → Anular recibo (restaura saldo facturas)
```
