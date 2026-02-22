# Manual de Usuario: Módulo Ventas

Esta guía explica cómo usar el módulo de Ventas del ERP Casa Chalar de principio a fin.

## El flujo completo

```
1. Cotización → 2. Pedido → 3. Factura → 4. Recibo de Cobro
```

Cada paso es opcional según la operatoria:
- Una venta al contado directa: **Pedido → Factura → Recibo**
- Una venta con propuesta previa: **Cotización → Pedido → Factura → Recibo**

---

## Paso 1: Cotización

### ¿Cuándo usar una cotización?

Cuando el cliente pide un presupuesto antes de confirmar la compra. La cotización no compromete stock ni genera saldo.

### Crear una cotización

1. Ir a **Ventas → Cotizaciones**
2. Hacer clic en **Nueva Cotización**
3. Elegir la **moneda** (UYU o USD) — el tipo de cambio del BCU se carga automáticamente
4. Seleccionar el **cliente** (los descuentos del cliente se aplican solos)
5. Seleccionar el **vendedor** responsable (opcional)
6. Indicar la **fecha de vencimiento** de la oferta (opcional)
7. Cargar los **ítems**:
   - Buscar el artículo por nombre o código
   - El precio y el IVA se completan solos
   - El descuento se pre-completa con el % del cliente
   - Ajustar cantidad, precio o descuento si es necesario
8. Agregar más ítems con **+ Agregar ítem**
9. Guardar → queda en estado **BORRADOR**

!!! tip "Tip: Ítems libres"
    Podés agregar ítems sin seleccionar un artículo del catálogo. Escribí directamente en el campo "Descripción / Concepto". Útil para servicios o conceptos especiales.

### Enviar la cotización al cliente

1. En la tabla de cotizaciones, buscar la cotización en estado BORRADOR
2. Hacer clic en **Enviar**
3. Estado pasa a **ENVIADA**

### ¿El cliente la aceptó?

- Hacer clic en **✓ Aceptar** → estado pasa a ACEPTADA
- Cuando se crea un pedido desde esta cotización → pasa a CONVERTIDA

### ¿El cliente la rechazó?

- Hacer clic en **✗** → estado pasa a RECHAZADA

---

## Paso 2: Pedido de Venta

### Crear un pedido desde una cotización

1. Ir a **Ventas → Pedidos**
2. Hacer clic en **Nuevo Pedido**
3. Seleccionar el **cliente**
4. Si el cliente tiene cotizaciones disponibles (ENVIADA o ACEPTADA), aparecen en una tabla
5. Hacer clic en **"Usar esta cotización"** → los ítems se precargan automáticamente
6. Ajustar si es necesario
7. Completar: vendedor, fecha de entrega, número de OC del cliente (opcional)
8. Guardar → queda en **BORRADOR**

### Crear un pedido sin cotización previa

1. Ir a **Ventas → Pedidos**
2. Hacer clic en **Nuevo Pedido**
3. Seleccionar cliente, moneda y cargar ítems manualmente
4. Guardar

### Confirmar el pedido

1. En la tabla, hacer clic en **Confirmar**
2. Estado pasa a **CONFIRMADO** — ya está listo para facturar

---

## Paso 3: Factura

### Crear una factura desde un pedido

!!! warning "Importante"
    Las facturas **siempre** se crean a partir de un pedido. No existe factura directa sin pedido.

1. Ir a **Ventas → Facturas**
2. Hacer clic en **Nueva Factura**
3. Seleccionar el **cliente**
4. Aparecen los pedidos CONFIRMADOS y EN_PROCESO del cliente
5. Hacer clic en **"Facturar este"** para seleccionar el pedido
6. Los ítems se precargan del pedido
7. Completar:
   - **Tipo de comprobante:** Factura Crédito / Boleta Contado / Contrarembolso / Nota Crédito / Nota Débito
   - **Serie:** letra de la serie (ej: A)
   - **Vendedor** (opcional)
   - **Observaciones** (opcional)
8. Hacer clic en **Crear Factura** → queda en **BORRADOR**

### Emitir la factura

Para que la factura genere saldo y pueda cobrarse:

1. En la tabla, buscar la factura en BORRADOR
2. Hacer clic en **Emitir**
3. Estado pasa a **EMITIDA** con su saldo pendiente

---

## Paso 4: Recibo de Cobro

### Registrar un cobro

1. Ir a **Ventas → Recibos Cobro**
2. Hacer clic en **Nuevo Recibo**
3. Seleccionar el **cliente**
4. Aparecen todas las facturas con saldo pendiente del cliente
5. Marcar las facturas que se van a cobrar (check o clic en la fila)
6. Para cobros parciales: ajustar el monto en el campo **"A cobrar"** de cada factura
7. Seleccionar el **medio de pago** (Efectivo / Transferencia / Cheque / Tarjeta)
8. Agregar observaciones si es necesario
9. Hacer clic en **Registrar Cobro** → queda en **BORRADOR**

### Aplicar el recibo

1. En la tabla, hacer clic en **Aplicar**
2. El saldo de cada factura cobrada se reduce automáticamente
3. Las facturas totalmente cobradas muestran "✓ Cobrada" en verde

---

## Monedas y tipo de cambio

### UYU vs USD

- Podés crear comprobantes en **UYU** (pesos uruguayos) o **USD** (dólares)
- El tipo de cambio del BCU se consulta automáticamente cada día hábil
- Si el BCU no responde, se usa el último valor disponible o $50 por defecto

### Convertir moneda en el formulario

Al cambiar de UYU a USD (o viceversa) **después de cargar ítems**, los precios se reconvierten automáticamente usando el TC del día. No perdés los precios cargados.

### Almacenamiento

El backend almacena todos los precios en UYU internamente. La moneda del comprobante controla cómo se muestran y calculan los totales.

---

## Descuentos

### Descuento del cliente (automático)

Cada cliente puede tener descuentos configurados en su ficha:
- **Descuento 1 %:** descuento de línea (sobre el precio de cada ítem)
- **Descuento 2 %:** descuento financiero adicional

Al seleccionar un cliente en cualquier comprobante, estos descuentos se aplican automáticamente a todos los ítems.

### Ajuste manual

Podés modificar el descuento de cada ítem individualmente en el formulario, sin perder el descuento base del cliente.

---

## Preguntas frecuentes

**¿Puedo crear una factura sin pedido?**  
No. Toda factura debe originarse en un pedido confirmado. Esto garantiza trazabilidad completa (cotización → pedido → factura → cobro).

**¿Puedo cobrar varias facturas en un solo recibo?**  
Sí. El recibo de cobro permite seleccionar múltiples facturas del mismo cliente.

**¿Puedo cobrar una factura en dos partes?**  
Sí. Al registrar el recibo, ajustá el campo "A cobrar" al monto parcial. El saldo restante queda disponible para el próximo recibo.

**¿El sistema avisa si la sesión expiró?**  
Sí. Cuando el token de sesión vence, el sistema redirige automáticamente a la pantalla de login.

**¿Los precios en USD se guardan en USD?**  
No. El backend almacena todo en UYU usando el TC del día. La moneda del comprobante es para visualización. Al convertir un comprobante de USD a UYU en pantalla, se usa el TC guardado al momento de la creación.
