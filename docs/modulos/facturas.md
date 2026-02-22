# Módulo: Facturas

Las facturas documentan formalmente la venta. En Casa Chalar ERP, **siempre se crean a partir de un pedido confirmado** para garantizar trazabilidad completa.

## Acceso

Menú lateral → **Ventas** → **Facturas**

## Tipos de comprobante

| Tipo | Descripción |
|------|-------------|
| **Factura Crédito** | Factura con crédito (30d, 60d, etc.) |
| **Boleta Contado** | Venta al contado |
| **Contrarembolso** | Pago contra entrega |
| **Nota de Crédito** | Ajuste a favor del cliente |
| **Nota de Débito** | Ajuste a favor de la empresa |

## Ciclo de vida

```
BORRADOR → EMITIDA → (Recibo de cobro aplica pagos)
         ↘           ↘ ANULADA
```

| Estado | Descripción |
|--------|-------------|
| **BORRADOR** | Factura creada, aún no emitida. |
| **EMITIDA** | Factura oficial. Tiene saldo pendiente de cobro. |
| **ANULADA** | Factura anulada (el saldo se libera). |

## Crear una factura

La factura siempre se crea **desde un pedido**:

1. Hacé clic en **Nueva Factura**
2. **Paso 1:** Seleccioná el cliente
3. **Paso 2:** El sistema muestra los pedidos CONFIRMADOS y EN_PROCESO del cliente.  
   Hacé clic en **"Facturar este"** para seleccionar el pedido
4. **Paso 3:** Los ítems del pedido se precargan automáticamente.  
   Completá: Tipo de comprobante, Serie, Vendedor (opcional), Observaciones
5. Hacé clic en **Crear Factura**

!!! info "¿Qué pasa con el pedido?"
    Al crear la factura, el pedido pasa automáticamente a estado **FACTURADO**.

## Emitir una factura

Una factura en BORRADOR no genera saldo ni puede cobrarse. Para habilitarla:

1. En la tabla de facturas, buscá la factura en estado BORRADOR
2. Hacé clic en **Emitir**
3. El estado pasa a EMITIDA y aparece el saldo pendiente

## Saldo pendiente

Cada factura emitida tiene un campo **Saldo pendiente** que refleja el monto aún no cobrado. Al aplicar recibos de cobro, el saldo se descuenta automáticamente.

La tabla de facturas muestra:
- ✓ Cobrada (en verde) — saldo = 0
- Monto pendiente (en rojo) — saldo > 0

## Anular una factura

Solo facturas en estado EMITIDA pueden anularse. La anulación revierte el saldo pendiente.

## API

```
GET    /api/v1/facturas                             → Listar facturas de la empresa
POST   /api/v1/facturas                             → Crear factura (requiere pedidoId)
POST   /api/v1/facturas/{id}/emitir                 → Emitir factura
POST   /api/v1/facturas/{id}/anular                 → Anular factura
GET    /api/v1/facturas/cliente/{id}/pendientes      → Facturas con saldo pendiente del cliente
```
