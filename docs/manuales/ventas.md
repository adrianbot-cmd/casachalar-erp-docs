# Manual de Ventas

Guía completa para operar el módulo de ventas del ERP Casa Chalar.

## El flujo de ventas

```
(Cotización) → Pedido → Factura → Recibo de Cobro
```

Cada etapa es opcional según la operatoria del día:

| Escenario | Pasos |
|-----------|-------|
| Venta al contado sin presupuesto | Pedido → Boleta Contado *(cobro automático)* |
| Venta con presupuesto previo | Cotización → Pedido → Factura → Recibo |
| Cuenta corriente | Pedido → Factura Crédito → Recibo (cuando paga) |

---

## 1. Cotización (presupuesto)

Ir a **Ventas → Cotizaciones → Nueva Cotización**

### Cómo crear una cotización

1. **Moneda**: elegí UYU o USD. El tipo de cambio del BCU se carga solo
2. **Cliente**: al seleccionarlo, sus descuentos se aplican automáticamente a los ítems
3. **Vendedor**: quién atiende la venta (opcional)
4. **Fecha de vencimiento**: cuántos días es válida la oferta (opcional)
5. **Ítems**: buscá por código o descripción
   - El precio y el IVA se completan solos desde el catálogo
   - El descuento se pre-completa con el % del cliente
   - Todo es editable

**Guardar** → queda en **BORRADOR**

### Estados de la cotización

| Estado | Qué significa | Qué podés hacer |
|--------|--------------|-----------------|
| **BORRADOR** | Todavía en preparación | Editar, Enviar |
| **ENVIADA** | Ya fue enviada al cliente | Aceptar, Rechazar |
| **ACEPTADA** | Cliente confirmó la oferta | Convertir en pedido |
| **RECHAZADA** | Cliente la rechazó | — |
| **CONVERTIDA** | Ya se hizo el pedido | Ver detalle |

### Convertir cotización en pedido

1. En **Pedidos**, crear un nuevo pedido
2. Seleccionar el cliente — aparecen sus cotizaciones ENVIADA/ACEPTADA
3. Hacer clic en **"Usar esta cotización"**
4. Los ítems (con precios, descuentos y vendedor) se precargan solos
5. La cotización pasa automáticamente a **CONVERTIDA**

---

## 2. Pedido de Venta

Ir a **Ventas → Pedidos → Nuevo Pedido**

### Crear un pedido

**Opción A — Desde cotización existente:**
1. Seleccioná el cliente
2. Aparecen sus cotizaciones disponibles
3. Hacé clic en **"Usar esta cotización"**
4. Ajustá si es necesario y guardá

**Opción B — Pedido libre:**
1. Seleccioná cliente y moneda
2. Cargá los ítems manualmente
3. Guardá

### Confirmar el pedido

Para poder facturar, el pedido debe estar **CONFIRMADO**:

1. En la tabla, hacer clic en **Confirmar**
2. Estado pasa a CONFIRMADO → listo para facturar

---

## 3. Factura

Ir a **Ventas → Facturas → Nueva Factura**

### Crear una factura

1. Seleccioná el cliente
2. Aparecen sus pedidos CONFIRMADOS
3. Hacé clic en **"Facturar este"**
4. Elegí el **tipo de comprobante**:

### Tipos de comprobante y sus diferencias

| Tipo | Vencimiento | Cobro |
|------|-------------|-------|
| **Factura Crédito** | 30 días (automático) | Manual — crear Recibo de Cobro cuando paga |
| **Boleta Contado** | Mismo día | **Automático** — se cobra al crear la factura |
| **Contrarembolso** | Mismo día | Manual |
| **Nota de Crédito** | Mismo día | — |
| **Nota de Débito** | Mismo día | — |

### Boleta Contado — el cobro es automático

Al elegir Boleta Contado, aparece el campo **"Forma de cobro"** (obligatorio):

- **Efectivo** — sin campos adicionales
- **Transferencia** — sin campos adicionales  
- **Cheque** — aparece campo **"Venc. cheque"** *(obligatorio)*. Ingresá la fecha del cheque
- **Tarjeta** — sin campos adicionales

Al guardar la Boleta Contado, el sistema automáticamente:
- Emite la factura
- Crea el recibo de cobro (APLICADO)
- Deja el saldo en $0

**No necesitás ir a Recibos de Cobro.**

### Factura Crédito — vencimiento a 30 días

El formulario muestra la fecha de vencimiento calculada (hoy + 30 días) en azul. Es solo visual, no se puede cambiar. Las facturas vencidas se marcan en rojo en la tabla.

Para cobrar una Factura Crédito cuando el cliente paga, ir a **Ventas → Recibos Cobro**.

---

## 4. Recibo de Cobro

Ir a **Ventas → Recibos Cobro → Nuevo Recibo**

### Para qué sirve

Registra el pago de **Facturas Crédito** y otros comprobantes que no tienen cobro automático.

### Crear un recibo

1. Seleccioná el cliente
2. Aparecen sus facturas con saldo pendiente
3. Marcá las facturas que vas a cobrar (podés marcar varias)
4. Para **cobros parciales**: editá el campo "A cobrar" con el monto efectivo
5. Elegí el **medio de pago**:
   - Si elegís **Cheque**: ingresá la fecha de vencimiento del cheque *(obligatorio)*
6. Agregá observaciones si querés (número de transferencia, banco, etc.)
7. Clic en **Registrar Cobro**

### Aplicar el recibo

El recibo recién creado queda en **BORRADOR**. Para que descuente el saldo de las facturas:

1. En la tabla, clic en **Aplicar**
2. Los saldos se actualizan automáticamente

---

## Monedas (UYU / USD)

### Cómo funciona

Podés crear cualquier comprobante en **pesos (UYU)** o **dólares (USD)**.

- El tipo de cambio del BCU se consulta automáticamente
- Si el BCU no responde, se usa el último valor disponible
- Al elegir USD, los precios del catálogo se muestran en dólares

### Cambiar la moneda después de cargar ítems

Si cambias la moneda con ítems ya cargados, el sistema reconvierte los precios usando el TC del día. **No perdés los ítems.**

### Almacenamiento interno

El backend siempre guarda los precios en UYU. La moneda del comprobante determina cómo se muestran los totales en pantalla y en los recibos.

---

## Descuentos del cliente

Cada cliente puede tener configurado hasta dos porcentajes de descuento en su ficha:

- **Descuento 1 %**: descuento de línea, se aplica sobre el precio unitario
- **Descuento 2 %**: descuento financiero adicional, se aplica sobre el subtotal ya descontado

Al seleccionar el cliente en cualquier comprobante, estos descuentos se pre-completan en todos los ítems. Podés modificarlos ítem por ítem si es necesario.

---

## Preguntas frecuentes

**¿Puedo hacer una factura sin pedido?**  
No. Toda factura requiere un pedido confirmado. Esto garantiza trazabilidad de cotización → pedido → factura → cobro.

**¿Puedo cobrar varias facturas en un recibo?**  
Sí. El recibo permite seleccionar múltiples facturas del mismo cliente.

**¿Puedo hacer un cobro parcial?**  
Sí. Al crear el recibo, editá el campo "A cobrar" con el monto parcial. El resto queda como saldo pendiente.

**¿Qué pasa si una Boleta Contado ya tiene el cobro automático y quiero anularla?**  
Primero anulá el Recibo de Cobro asociado (esto restaura el saldo de la factura), luego anulá la factura.

**¿Las facturas vencidas se notifican?**  
Hoy se marcan en rojo en la tabla. En el futuro habrá alertas y reportes de facturas vencidas.

**¿El sistema guarda el cheque por separado?**  
La fecha de vencimiento del cheque queda registrada en el Recibo de Cobro, visible en el detalle. Un futuro módulo de Tesorería permitirá hacer seguimiento de cheques en cartera.
