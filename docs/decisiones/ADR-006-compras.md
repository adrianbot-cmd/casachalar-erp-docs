# ADR-006: Módulo de Compras

**Estado:** ✅ Implementado  
**Fecha:** 2026-02-24  
**Autores:** Adri (agente IA), Julio Giraudi, Carlos Bendayan

---

## Contexto

Se requería implementar el ciclo completo de compras: desde la detección de una necesidad interna hasta el pago al proveedor. El sistema legacy no tenía módulo de compras estructurado — las compras se registraban directamente como entradas de stock sin trazabilidad del proceso.

El objetivo fue construir un módulo que refleje la práctica real del negocio: requerimiento interno → cotización a proveedores → adjudicación → recepción → pago.

---

## Decisiones

### 1. Flujo en 4 etapas

Se estructuró el módulo en cuatro entidades principales encadenadas:

```
Requerimiento → Cotización de Compra → Orden de Compra → Recepción → Factura Proveedor (CxP)
```

Este diseño garantiza trazabilidad completa: toda OC tiene origen (manual o adjudicación), toda recepción tiene OC, toda factura de proveedor tiene recepción.

### 2. Requerimientos opcionales, no mandatorios

El flujo estándar usa requerimientos, pero las OC también se pueden crear directamente (sin requerimiento previo). Esto permite flexibilidad para compras urgentes o recurrentes donde el proceso formal de aprobación es innecesario.

### 3. Aprobación por rol, no por flujo de trabajo configurable

La aprobación de requerimientos se delega a cualquier usuario con rol **ADMIN** o **SUPER_ADMIN**, sin configurar flujos de aprobación complejos. Suficiente para el tamaño actual del negocio.

### 4. Cotizaciones N:1 con requerimiento

Un requerimiento puede tener N cotizaciones (una por proveedor). Esto permite comparar precios de múltiples proveedores antes de decidir.

### 5. Adjudicación atómica

Al adjudicar una cotización, las siguientes acciones ocurren en una única transacción de base de datos:
- La cotización seleccionada → `ADJUDICADA`
- Las demás cotizaciones del requerimiento → `RECHAZADA`
- Se genera la OC automáticamente con los datos de la cotización
- El requerimiento → `PROCESADO`

Esto evita estados inconsistentes (ej: dos cotizaciones adjudicadas para el mismo requerimiento).

### 6. 3-way match en facturas de proveedor

Al registrar una factura de proveedor, el sistema valida:
1. **Cantidades**: lo facturado no puede superar lo recibido
2. **Precios**: el precio facturado no puede superar el precio de la OC
3. **Condición de pago**: debe coincidir con la acordada en la OC

Si alguna validación falla, se rechaza el registro con un mensaje descriptivo. El objetivo es evitar pagos de más o pagos por mercadería no recibida.

**Alternativa descartada:** validación laxa (solo advertencias, sin bloqueo). Se descartó porque genera riesgo de pagar facturas incorrectas.

### 7. Costo promedio ponderado al recibir

Al registrar una recepción, el costo unitario del artículo se actualiza con la fórmula de costo promedio ponderado:

```
nuevo_costo = (stock_actual × costo_actual + cantidad_recibida × costo_recepcion) / (stock_actual + cantidad_recibida)
```

Esto refleja el costo real de reposición y sirve de base para la valorización del inventario.

**Alternativa descartada:** FIFO. Más preciso contablemente pero significativamente más complejo de implementar y de explicar al equipo operativo.

### 8. Centro de costos obligatorio para no-mercadería

Para requerimientos de tipo INSUMO, SERVICIO o BIEN_USO, el centro de costos es obligatorio. Para MERCADERIA es opcional porque el costo va al costo de ventas, no a un centro específico.

### 9. Tolerancia de recepción configurable por OC

Cada OC tiene un campo `tolerancia_recepcion_pct` (default 5%). Permite recibir hasta un 5% más o menos de lo pedido sin que se requiera autorización manual. Apropiado para proveedores que entregan por peso o con variaciones normales de cantidad.

### 10. Número de OC auto-generado

El número de OC sigue el formato `OC-YYYY-NNN` generado automáticamente por empresa. Legible, sin colisiones, facilita referencias en comunicaciones con proveedores.

---

## Alternativas consideradas

| Alternativa | Razón de descarte |
|-------------|-------------------|
| Compras sin requerimiento (directo a OC siempre) | No refleja el proceso real; pierde trazabilidad de necesidades |
| Flujo de aprobación configurable (workflows) | Complejidad innecesaria para el tamaño actual del negocio |
| FIFO en costo de stock | Más preciso pero mucho más complejo de implementar y auditar |
| 3-way match como advertencia (no bloqueo) | Riesgo real de pagar facturas incorrectas |
| Cotización 1:1 con requerimiento | No permite comparar proveedores fácilmente |

---

## Consecuencias

✅ Trazabilidad completa: de la necesidad al pago  
✅ Control de precios y cantidades antes de pagar (3-way match)  
✅ Costo de inventario actualizado automáticamente al recibir  
✅ Flexibilidad: OC con o sin requerimiento previo  
✅ Consistencia garantizada en adjudicación (transacción atómica)  
⚠️ El pago efectivo de facturas de proveedor (integración con Tesorería) queda pendiente  
⚠️ La autorización automática por exceso de tolerancia queda para versión futura  
⚠️ Sin integración con e-procurement o portales de proveedor (fuera del scope MVP)
