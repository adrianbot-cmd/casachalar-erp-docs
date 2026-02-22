# Módulo: Cotizaciones

Las cotizaciones son el primer paso del flujo de ventas. Permiten presentar una oferta formal al cliente antes de confirmar el pedido.

## Acceso

Menú lateral → **Ventas** → **Cotizaciones**

## Ciclo de vida

```
BORRADOR → ENVIADA → ACEPTADA → CONVERTIDA (al crear pedido)
                  ↘ RECHAZADA
        ↘ VENCIDA (cuando supera fecha de vencimiento)
```

| Estado | Descripción |
|--------|-------------|
| **BORRADOR** | Recién creada, aún no enviada. Se puede editar. |
| **ENVIADA** | Enviada al cliente para su aprobación. |
| **ACEPTADA** | El cliente la aprobó. Lista para convertir en pedido. |
| **RECHAZADA** | El cliente la rechazó. |
| **VENCIDA** | Superó la fecha de vencimiento. |
| **CONVERTIDA** | Se generó un pedido a partir de ella. |

## Crear una cotización

1. Hacé clic en **Nueva Cotización**
2. **Seleccioná la moneda** del comprobante (UYU o USD)
   - El tipo de cambio del día se carga automáticamente desde el BCU
   - Si el cliente opera en dólares, elegí USD — los precios se muestran y almacenan en USD
3. **Seleccioná el cliente** — los descuentos del cliente se pre-completan en los ítems
4. Completá: Vendedor (opcional), Fecha de vencimiento (opcional), Observaciones
5. **Cargá los ítems:**
   - Buscá el artículo por código o descripción
   - La descripción se completa automáticamente
   - El precio se toma de la lista de precios asignada al cliente (o la mejor lista disponible)
   - El IVA se completa según el artículo (0%, 10% o 22%)
   - El descuento se pre-completa con el Descuento 1% del cliente
6. Podés ajustar precio, cantidad y descuento ítem por ítem
7. Guardá la cotización (queda en estado BORRADOR)

!!! tip "Agregar ítems sin artículo"
    Podés dejar el campo "Artículo" vacío e ingresar solo una descripción libre. Útil para servicios o conceptos que no están en el catálogo.

## Cambios de estado

Desde la tabla de cotizaciones:

- **Editar** — solo disponible en estado BORRADOR
- **Enviar** — pasa a ENVIADA (ya no se puede editar)
- **✓ Aceptar** — pasa a ACEPTADA (desde ENVIADA)
- **✗ Rechazar** — pasa a RECHAZADA (desde ENVIADA)

## Ver detalle

Hacé clic en **Ver** para abrir el drawer lateral con el detalle completo de la cotización e ítems.

## Selección de moneda

- Al cambiar la moneda **después de haber cargado ítems**, los precios se reconvierten automáticamente usando el TC del día
- El backend siempre almacena los precios en UYU internamente; la moneda del comprobante es solo para visualización y cálculo

## API

```
GET    /api/v1/cotizaciones                          → Listar cotizaciones de la empresa
POST   /api/v1/cotizaciones                          → Crear cotización
PUT    /api/v1/cotizaciones/{id}                     → Actualizar (solo BORRADOR)
POST   /api/v1/cotizaciones/{id}/estado?estado=...   → Cambiar estado
GET    /api/v1/cotizaciones/para-pedido?clienteId=   → Cotizaciones disponibles para convertir a pedido
```
