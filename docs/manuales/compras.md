# Manual de Compras

Guía completa para operar el módulo de compras del ERP Casa Chalar: desde identificar una necesidad hasta registrar la factura del proveedor.

---

## El flujo de compras

```
Requerimiento → Cotización de Compra → Orden de Compra → Recepción → Factura Proveedor (CxP)
```

El flujo completo tiene sentido cuando hay que comparar proveedores o se necesita aprobación interna. Para compras directas y urgentes, podés crear la OC directamente sin requerimiento ni cotización.

| Escenario | Pasos |
|-----------|-------|
| Compra directa urgente | OC → Recepción → Factura Proveedor |
| Compra habitual con proveedor conocido | OC → Recepción → Factura Proveedor |
| Compra importante, comparando precios | Requerimiento → Cotizaciones → Adjudicar → OC (auto) → Recepción → Factura Proveedor |
| Insumo o servicio que requiere aprobación | Requerimiento → Aprobación → Cotizaciones → Adjudicar → OC (auto) → ... |

---

## 1. Requerimiento de Compra

Ir a **Compras → Requerimientos → Nuevo Requerimiento**

Un requerimiento es la solicitud interna: "necesitamos comprar esto". Sirve para dejar registro de quién pidió qué y, opcionalmente, pasar por aprobación antes de gastar.

### Crear un requerimiento

1. Elegí el **tipo**:

   | Tipo | Centro de costos |
   |------|-----------------|
   | **MERCADERIA** | Opcional |
   | **INSUMO** | Obligatorio |
   | **SERVICIO** | Obligatorio |
   | **BIEN_USO** (activo fijo) | Obligatorio |

2. Si el tipo lo requiere, completá el **Centro de costos**
3. Agregá los ítems: artículo, cantidad, unidad, observación
4. Guardá → queda en **BORRADOR**

### Enviar a aprobación

1. Revisá que todo esté correcto
2. Clic en **Enviar para aprobación**
3. Estado pasa a **PENDIENTE_APROBACION**
4. Un ADMIN lo aprueba o rechaza

### ¿Necesito aprobar siempre?

Solo si tu empresa lo requiere. Para **MERCADERIA** es frecuente saltearse este paso y crear la OC directamente.

---

## 2. Cotizaciones de Compra

Ir a **Compras → Cotizaciones → Nueva Cotización**

Cuando tenés un requerimiento **APROBADO** y querés comparar precios de varios proveedores.

### Crear una cotización por proveedor

1. Seleccioná el **Requerimiento** (debe estar APROBADO)
2. Seleccioná el **Proveedor** al que le pedís precio
3. Los ítems del requerimiento se precargan solos
4. Guardá → queda en **PENDIENTE**

Repetí este proceso para cada proveedor que quieras consultar. Cada uno tiene su propia cotización.

### Cuando el proveedor responde

1. Abrí la cotización del proveedor
2. Completá los precios recibidos
3. Clic en **Marcar como recibida** → estado pasa a **RECIBIDA**

### Adjudicar la mejor oferta

Una vez que tenés las respuestas, comparás y elegís:

1. Clic en **Adjudicar** en la cotización ganadora
2. El sistema automáticamente:
   - ✅ Genera la **Orden de Compra** con los datos del proveedor ganador
   - ✅ Descarta las otras cotizaciones
   - ✅ Marca el requerimiento como **PROCESADO**

---

## 3. Orden de Compra (OC)

Ir a **Compras → Órdenes de Compra**

La OC es el documento formal que le mandás al proveedor. Puede generarse automáticamente desde una cotización adjudicada o crearse manualmente.

### Crear una OC manualmente

1. Clic en **Nueva Orden de Compra**
2. Completar:
   - **Proveedor**
   - **Condición de pago**
   - **Fecha de necesidad** — cuándo la necesitás
   - **Tolerancia de recepción (%)** — margen de diferencia aceptable en cantidad (default: 5%)
   - **¿Es activo fijo?** — marcá si es un bien que se va a activar (equipo, mueble, etc.)
   - **Dirección de entrega** *(opcional)*
3. Agregar ítems: artículo, cantidad, precio, moneda, IVA
4. Guardar

El sistema asigna el número de OC automáticamente: `OC-2026-001`, `OC-2026-002`, etc.

### Estados de la OC

| Estado | Qué significa |
|--------|--------------|
| **ABIERTA** | OC emitida, esperando mercadería |
| **PARCIALMENTE_RECIBIDA** | Llegó parte, queda saldo pendiente |
| **COMPLETADA** | Se recibió todo (dentro del margen de tolerancia) |
| **ANULADA** | Cancelada |

---

## 4. Recepción de Mercadería

Ir a **Compras → Recepciones → Nueva Recepción**

Registrá cada vez que llega mercadería del proveedor, aunque sea parcial.

### Registrar una recepción

1. Seleccioná la **OC** correspondiente (debe estar ABIERTA o PARCIALMENTE_RECIBIDA)
2. Completar:
   - **Número de remito del proveedor** *(recomendado para auditoría)*
   - **Fecha de recepción**
3. Por cada artículo, ingresá:
   - **Cantidad recibida** — solo lo que llegó físicamente hoy
   - **Costo unitario** — precio real de la factura
4. Guardar

### ¿Qué pasa al guardar?

- ✅ El **stock se actualiza** automáticamente (entrada)
- ✅ El **costo promedio** del artículo se recalcula
- ✅ La **OC se actualiza**: pasa a PARCIALMENTE_RECIBIDA o COMPLETADA según corresponda

### Recepciones parciales

Si el proveedor entrega en partes, registrás una recepción por cada entrega. La OC queda en PARCIALMENTE_RECIBIDA hasta que se complete.

!!! info "Tolerancia"
    Si recibís un 5% más o menos de lo pedido, la OC se marca igual como COMPLETADA. Este porcentaje es configurable por OC.

---

## 5. Factura del Proveedor (Cuentas por Pagar)

Ir a **Compras → Cuentas por Pagar → Nueva Factura Proveedor**

Registrá la factura del proveedor **después** de haber recibido la mercadería. El sistema verifica que lo facturado coincida con lo recibido y lo acordado.

### Registrar la factura

1. Seleccioná la **OC** (debe tener al menos una recepción registrada)
2. Completar:
   - **Número de factura** del proveedor
   - **Fecha** de la factura
   - **Condición de pago** (debe coincidir con la OC)
   - **Moneda**
3. Revisá los ítems (se precargan desde la recepción)
4. Guardar

### Validaciones automáticas (3-way match)

El sistema valida que:

| Qué verifica | Regla |
|-------------|-------|
| **Cantidad** | No podés facturar más de lo que recibiste |
| **Precio** | No podés pagar más del precio acordado en la OC |
| **Condición de pago** | Debe coincidir con la OC |

Si algo no coincide, el sistema te avisa con un mensaje descriptivo.

### Dashboard de Cuentas por Pagar

La pantalla principal muestra:
- **Total pendiente de pago**
- **Total vencido** (en rojo — requiere atención urgente)
- **Próximos vencimientos** ordenados por fecha

!!! warning "Pago de facturas"
    El registro del pago efectivo a proveedores (desde Tesorería) está en desarrollo. Por ahora, el estado de la factura se puede actualizar manualmente.

---

## Preguntas frecuentes

**¿Puedo crear una OC sin requerimiento previo?**  
Sí. El requerimiento es opcional. Podés ir directamente a crear la OC cuando la compra es directa o urgente.

**¿Puedo recibir más de lo que pedí en la OC?**  
Sí, dentro del porcentaje de tolerancia configurado en la OC (default 5%). Si superás ese margen, el sistema te muestra una advertencia pero hoy te deja continuar.

**¿Puedo registrar múltiples recepciones contra la misma OC?**  
Sí. Cada vez que llega mercadería registrás una recepción. La OC acumula todas las recepciones hasta completarse.

**¿El stock se actualiza solo o tengo que hacerlo manualmente?**  
Solo. Al guardar una recepción, el stock se actualiza automáticamente.

**¿Qué pasa si la factura del proveedor tiene precio distinto al de la OC?**  
Si el precio es mayor, el sistema lo rechaza (3-way match). Tenés que pedir una nota de crédito al proveedor o actualizar la OC con el precio correcto antes de registrar la factura.

**¿Puedo anular una OC?**  
Solo si no tiene recepciones registradas. Una vez que llegó aunque sea un artículo, la OC no se puede anular.

**¿Quién puede aprobar requerimientos?**  
Solo usuarios con rol ADMIN o SUPER_ADMIN.

**¿Cómo sé qué facturas de proveedor están por vencer?**  
Ir a **Compras → Cuentas por Pagar**. El dashboard muestra los próximos vencimientos ordenados por fecha. Las vencidas aparecen en rojo.
