# Módulo: Cotizaciones

Las cotizaciones son el primer paso del flujo de ventas. Permiten presentar una oferta formal al cliente **sin comprometer stock ni generar saldo**.

## Acceso

Menú lateral → **Ventas** → **Cotizaciones**

---

## Ciclo de vida

```
BORRADOR → ENVIADA → ACEPTADA → CONVERTIDA
                   ↘ RECHAZADA
```

| Estado | Descripción | Editable |
|--------|-------------|----------|
| **BORRADOR** | Recién creada. Todavía en preparación. | ✅ Sí |
| **ENVIADA** | Enviada al cliente para su aprobación. | ❌ No |
| **ACEPTADA** | El cliente confirmó la oferta. | ❌ No |
| **RECHAZADA** | El cliente la rechazó. | ❌ No |
| **CONVERTIDA** | Se generó un pedido a partir de ella. | ❌ No |

---

## Crear una cotización

1. Hacé clic en **Nueva Cotización**
2. **Moneda**: elegí UYU o USD
   - El tipo de cambio del BCU se carga automáticamente
   - Si cambiás la moneda con ítems ya cargados, los precios se reconvierten
3. **Cliente**: al seleccionarlo, se pre-completan los descuentos del cliente en los ítems
4. **Vendedor** (opcional): quién atiende la venta
5. **Fecha de vencimiento** (opcional): hasta cuándo es válida la oferta
6. **Observaciones** (opcional)
7. **Ítems**:
   - Buscá el artículo por código o descripción
   - El precio se toma de la lista de precios del cliente (o la mejor disponible)
   - El IVA se completa según el artículo (0%, 10% o 22%)
   - El descuento se pre-completa con el % del cliente
   - Ajustá cantidad, precio y descuento si es necesario
8. Guardá → queda en **BORRADOR**

!!! tip "Ítems libres (sin artículo del catálogo)"
    Podés dejar el campo "Artículo" vacío y escribir directamente en "Descripción / Concepto". Útil para servicios u otros conceptos que no están en el catálogo.

---

## Acciones disponibles por estado

| Acción | Disponible en | Resultado |
|--------|--------------|-----------|
| **Editar** | BORRADOR | Abre el formulario de edición |
| **Enviar** | BORRADOR | Estado → ENVIADA |
| **✓ Aceptar** | ENVIADA | Estado → ACEPTADA |
| **✗ Rechazar** | ENVIADA | Estado → RECHAZADA |
| **Ver** | Todos | Abre el drawer con detalle completo |

---

## Convertir en pedido

Desde **Ventas → Pedidos**:

1. Crear un nuevo pedido
2. Seleccionar el cliente — aparecen sus cotizaciones ENVIADA/ACEPTADA
3. Hacer clic en **"Usar esta cotización"**
4. Los ítems, precios, descuentos y vendedor se precargan automáticamente
5. La cotización pasa a **CONVERTIDA**

---

## Ver el detalle

El botón **Ver** abre un drawer con:
- Datos del cliente, fecha, vencimiento, moneda
- Tabla de ítems con precios, descuentos e IVA
- Totales: subtotal sin IVA, IVA, total

---

## API

```
GET    /api/v1/cotizaciones                          → Listar cotizaciones de la empresa
POST   /api/v1/cotizaciones                          → Crear cotización
PUT    /api/v1/cotizaciones/{id}                     → Actualizar (solo BORRADOR)
POST   /api/v1/cotizaciones/{id}/estado?estado=...   → Cambiar estado (ENVIADA/ACEPTADA/RECHAZADA)
GET    /api/v1/cotizaciones/para-pedido?clienteId=   → Cotizaciones disponibles para convertir a pedido
```
