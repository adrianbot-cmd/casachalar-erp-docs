# Módulo: Facturas

Las facturas documentan formalmente la venta. En Casa Chalar ERP, **siempre se crean a partir de un pedido confirmado** para garantizar trazabilidad completa.

## Acceso

Menú lateral → **Ventas** → **Facturas**

## Tipos de comprobante

| Tipo | Descripción | Vencimiento | Cobro |
|------|-------------|-------------|-------|
| **Factura Crédito** | Factura con crédito | 30 días desde la fecha | Manual (vía Recibo de Cobro) |
| **Boleta Contado** | Venta al contado | Mismo día | Automático al crear |
| **Contrarembolso** | Pago contra entrega | Mismo día | Manual |
| **Nota de Crédito** | Ajuste a favor del cliente | Mismo día | — |
| **Nota de Débito** | Ajuste a favor de la empresa | Mismo día | — |

## Ciclo de vida

```
BORRADOR → EMITIDA → (Recibo de cobro aplica pagos)
                   ↘ ANULADA
```

| Estado | Descripción |
|--------|-------------|
| **BORRADOR** | Factura creada, aún no emitida. |
| **EMITIDA** | Factura oficial. Tiene saldo pendiente de cobro. |
| **ANULADA** | Factura anulada (el saldo se libera). |

---

## Crear una factura

La factura siempre se crea **desde un pedido**:

1. Hacé clic en **Nueva Factura**
2. **Seleccioná el cliente** — aparecen sus pedidos CONFIRMADOS y EN_PROCESO
3. Hacé clic en **"Facturar este"** para seleccionar el pedido
4. Los ítems del pedido se precargan automáticamente
5. Completá:
   - **Tipo de comprobante** (ver tabla arriba)
   - **Serie** — letra de la serie (ej: A)
   - **Vendedor** (opcional)
   - **Observaciones** (opcional)
6. Hacé clic en **Crear Factura**

!!! info "¿Qué pasa con el pedido?"
    Al crear la factura, el pedido pasa automáticamente a estado **FACTURADO**.

---

## Factura Crédito (30 días)

Al seleccionar tipo **Factura Crédito**:

- El formulario muestra la **fecha de vencimiento** calculada automáticamente: fecha de hoy + 30 días
- El campo aparece en **azul** indicando el plazo correcto
- El vencimiento se guarda con la factura y aparece en la columna **"Vence"** de la tabla
- Las facturas vencidas se marcan en **rojo**

!!! note "Plazo fijo"
    El plazo de 30 días es fijo para todas las facturas crédito, independientemente de la condición de pago del cliente.

---

## Boleta Contado — cobro automático

Al seleccionar tipo **Boleta Contado**, aparece obligatoriamente el campo **"Forma de cobro"**:

| Forma de cobro | Campos adicionales |
|---------------|--------------------|
| **Efectivo** | — |
| **Transferencia** | — |
| **Cheque** | Fecha de vencimiento del cheque *(obligatorio)* |
| **Tarjeta** | — |

Al guardar una Boleta Contado, el sistema automáticamente:

1. **Emite la factura** (pasa de BORRADOR a EMITIDA)
2. **Crea un Recibo de Cobro** (estado APLICADO) por el total
3. **Descuenta el saldo** de la factura → queda en $0

No es necesario ir a Recibos de Cobro para registrar el pago.

!!! warning "Cheque: fecha de vencimiento"
    Si el cliente paga con cheque, debés ingresar la **fecha de vencimiento del cheque**. Este dato queda guardado en el recibo y es clave para el seguimiento de cobros.

---

## Emitir una factura manualmente

Para Facturas Crédito, Contrarembolso, Notas de Crédito/Débito creadas en BORRADOR:

1. En la tabla de facturas, buscá la factura en estado BORRADOR
2. Hacé clic en **Emitir**
3. El estado pasa a EMITIDA y aparece el saldo pendiente

---

## Saldo pendiente

Cada factura emitida tiene un campo **Saldo pendiente** que refleja el monto aún no cobrado. Al aplicar recibos de cobro, el saldo se descuenta automáticamente.

La tabla muestra:
- ✓ cobrada (en verde) si el saldo es $0
- Monto pendiente en rojo si tiene saldo

---

## Anular una factura

Solo facturas EMITIDA pueden anularse. La anulación revierte el saldo pendiente.

---

## API

```
GET    /api/v1/facturas                             → Listar facturas de la empresa
POST   /api/v1/facturas                             → Crear factura (requiere pedidoId)
POST   /api/v1/facturas/{id}/emitir                 → Emitir factura
POST   /api/v1/facturas/{id}/anular                 → Anular factura
GET    /api/v1/facturas/cliente/{id}/pendientes      → Facturas con saldo pendiente del cliente
```

### Ejemplo: crear Boleta Contado con cheque

```json
POST /api/v1/facturas
{
  "pedidoId": 5,
  "tipo": "BOLETA_CONTADO",
  "serie": "A",
  "medioPago": "CHEQUE",
  "medioPagoVencimiento": "2026-04-15",
  "items": [...]
}
```
