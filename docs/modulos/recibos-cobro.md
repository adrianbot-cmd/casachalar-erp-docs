# Módulo: Recibos de Cobro

Los recibos de cobro registran los pagos recibidos de clientes y los aplican contra las facturas pendientes. Soportan cobros de **múltiples facturas en un solo recibo**.

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

## Crear un recibo de cobro

1. Hacé clic en **Nuevo Recibo**
2. **Paso 1:** Seleccioná el cliente
3. **Paso 2:** El sistema muestra todas las facturas del cliente con saldo pendiente.  
   Marcá las facturas que se van a cobrar (podés seleccionar varias)
4. **Paso 3:** Para cada factura seleccionada, podés ajustar el monto a aplicar  
   (útil para cobros parciales)
5. Seleccioná el **medio de pago** y completá observaciones opcionales
6. El total se calcula automáticamente sumando los montos seleccionados
7. Hacé clic en **Registrar Cobro**

## Medios de pago disponibles

| Medio | Descripción |
|-------|-------------|
| **Efectivo** | Pago en billetes/monedas |
| **Transferencia** | Transferencia bancaria |
| **Cheque** | Cheque de pago |
| **Tarjeta** | Tarjeta de débito o crédito |

## Cobros parciales

Podés cobrar un monto menor al saldo total de una factura. El campo **"A cobrar"** en la tabla de facturas permite ajustar el monto para cada una. El saldo restante queda disponible para futuros recibos.

## Aplicar un recibo

Un recibo en BORRADOR no descuenta el saldo de las facturas. Para registrarlo:

1. En la tabla de recibos, buscá el recibo en estado BORRADOR
2. Hacé clic en **Aplicar**
3. El saldo de cada factura se actualiza automáticamente

## Anular un recibo

Solo recibos en estado APLICADO pueden anularse. Al anular, las facturas recuperan el saldo correspondiente.

## API

```
GET    /api/v1/recibos-cobro                        → Listar recibos de la empresa
POST   /api/v1/recibos-cobro                        → Crear recibo
POST   /api/v1/recibos-cobro/{id}/aplicar           → Aplicar recibo (descuenta saldo facturas)
POST   /api/v1/recibos-cobro/{id}/anular            → Anular recibo (restaura saldo facturas)
```
