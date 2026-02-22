# ADR-005: Módulo de Ventas — Comprobantes y Flujo Comercial

**Estado:** ✅ Implementado  
**Fecha:** 2026-02-21  
**Autores:** Adri (agente IA), Julio Giraudi

---

## Contexto

Se requería implementar el flujo comercial completo de ventas: desde la propuesta al cliente hasta el registro del cobro. El sistema legacy manejaba todos los tipos de documentos en una tabla `operacion` polimórfica. Se optó por una estructura normalizada por tipo de comprobante.

---

## Decisiones

### 1. Flujo de comprobantes

El flujo es secuencial con trazabilidad completa:

```
Cotización → Pedido → Factura → Recibo de Cobro
```

- Una factura **siempre** se origina en un pedido (requerimiento de Julio).
- Un recibo de cobro **siempre** se origina en facturas emitidas con saldo.
- Esto garantiza trazabilidad completa y evita facturas "huérfanas".

### 2. Ciclos de vida por estado

**Cotización:** `BORRADOR → ENVIADA → ACEPTADA/RECHAZADA/VENCIDA → CONVERTIDA`  
**Pedido:** `BORRADOR → CONFIRMADO → EN_PROCESO → FACTURADO / CANCELADO`  
**Factura:** `BORRADOR → EMITIDA → ANULADA`  
**Recibo:** `BORRADOR → APLICADO → ANULADO`

### 3. Multimoneda (UYU/USD)

- Los comprobantes pueden crearse en UYU o USD.
- El backend **siempre almacena en UYU** (`precio_uyu`). La conversión se aplica al guardar.
- Cada comprobante guarda `moneda` y `cotizacion_usd` para reproducir los valores originales.
- El tipo de cambio se consulta automáticamente del BCU (lunes a viernes). Si falla, se usa el último registro disponible o $50 por defecto.

### 4. Descuentos del cliente

- La tabla `clientes` tiene `descuento1_pct` y `descuento2_pct` (migrados de `TER_Desc1/2` del legacy).
- Al seleccionar un cliente en un comprobante, los descuentos se pre-completan en todos los ítems.
- Las tablas de ítems (`cotizacion_items`, `pedido_items`, `factura_items`) almacenan `descuento_1_pct` y `descuento_2_pct` por ítem, permitiendo ajustes individuales.

### 5. Tipos de factura

Se implementaron los tipos necesarios para el mercado uruguayo:

| Enum | Descripción |
|------|-------------|
| `FACTURA_CREDITO` | Factura con crédito |
| `BOLETA_CONTADO` | Boleta contado |
| `CONTRAREMBOLSO` | Pago contra entrega |
| `NOTA_CREDITO` | Nota de crédito |
| `NOTA_DEBITO` | Nota de débito |

### 6. Recibo multi-factura

Un único recibo puede aplicarse a múltiples facturas del mismo cliente con montos parciales por factura. Esto refleja la práctica comercial real (un cheque cancela varias facturas).

Al aplicar el recibo:
- Cada factura reduce su `saldo_pendiente` por el monto aplicado.
- Al anular el recibo, se revierte el `saldo_pendiente` en cada factura.

### 7. Vendedores

Los vendedores no eran un maestro en el nuevo ERP. Se creó la tabla `vendedores` con migración automática desde el sistema legacy (tabla `vendedor`, filtrando `Borrado=''` y nombres que no incluyen 'NO USAR').

### 8. Saldo de facturas

El campo `saldo_pendiente` en `facturas` se actualiza en tiempo real al aplicar/anular recibos. Se calcula como `total - suma(montos_aplicados)`.

### 9. Performance: endpoints livianos para selects

Para evitar cargar 1.76MB de artículos completos al abrir formularios:

- `GET /articulos/selector` → DTO mínimo (id, código, descripción, IVA). ~200KB.
- `GET /articulos/mejor-precio` → `Map<articuloId, precioUsd>`. ~50KB.

### 10. Formularios sin modal

Por requerimiento explícito del negocio: todos los formularios de ventas son **inline** (integrados en la página), no modales. Esto permite ver el detalle completo sin perder contexto.

---

## Alternativas consideradas

| Alternativa | Razón de descarte |
|-------------|-------------------|
| Tabla `operacion` polimórfica (como legacy) | Dificulta queries, tipos estrictos, validaciones |
| Factura libre sin pedido | Pierde trazabilidad requerida |
| Descuentos solo a nivel de comprobante | No refleja condiciones individuales por ítem |
| Precio almacenado en USD | Complejidad de conversión en reportes; UYU es la moneda base del negocio |

---

## Consecuencias

✅ Trazabilidad completa: toda factura tiene pedido, todo recibo tiene factura  
✅ Soporte multimoneda real sin pérdida de información  
✅ Descuentos heredables y ajustables por ítem  
✅ Performance aceptable en formularios con catálogo grande  
⚠️ La integración con CFE (factura electrónica DGI) queda para una fase futura
