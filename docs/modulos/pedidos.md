# Módulo: Pedidos de Venta

Los pedidos de venta confirman el compromiso de entrega con el cliente. Pueden crearse desde una cotización o directamente.

## Acceso

Menú lateral → **Ventas** → **Pedidos**

---

## Ciclo de vida

```
BORRADOR → CONFIRMADO → EN_PROCESO → FACTURADO
         ↘ CANCELADO    ↘ CANCELADO
```

| Estado | Descripción |
|--------|-------------|
| **BORRADOR** | Recién creado. Se puede editar y cancelar. |
| **CONFIRMADO** | Confirmado para preparación. Listo para facturar. |
| **EN_PROCESO** | En preparación o armado. También puede facturarse. |
| **FACTURADO** | Se generó una factura. Estado final. |
| **CANCELADO** | Cancelado. No puede reactivarse. |

---

## Crear un pedido

### Desde una cotización existente

1. Hacé clic en **Nuevo Pedido**
2. Seleccioná el cliente
3. Aparecen sus cotizaciones ENVIADA/ACEPTADA disponibles
4. Hacé clic en **"Usar esta cotización"**
   - Los ítems, precios, descuentos y **vendedor** se precargan automáticamente
   - La cotización pasa a estado **CONVERTIDA**
5. Ajustá si es necesario
6. Completá: fecha de entrega, número de OC del cliente (opcional), observaciones
7. Guardá → queda en **BORRADOR**

!!! info "Edición de pedidos"
    Los pedidos en BORRADOR pueden editarse. Una vez CONFIRMADO, no se pueden modificar los ítems.

### Sin cotización previa

1. Hacé clic en **Nuevo Pedido**
2. Seleccioná la moneda (UYU o USD)
3. Seleccioná el cliente — los descuentos se pre-completan en los ítems
4. Seleccioná el vendedor (opcional)
5. Cargá los ítems manualmente
6. Guardá

---

## Confirmar el pedido

Para habilitar la facturación:

1. En la tabla, hacé clic en **Confirmar**
2. Estado pasa a **CONFIRMADO**

---

## Cancelar un pedido

Disponible en estados BORRADOR y CONFIRMADO. Una vez cancelado, el pedido no puede reactivarse.

---

## Facturar un pedido

Una vez CONFIRMADO (o EN_PROCESO), el pedido aparece disponible en **Ventas → Facturas** al seleccionar el cliente. Ver [Manual de Facturas](facturas.md).

---

## Herencia del vendedor

Al cargar una cotización en el formulario de pedido, el **vendedor de la cotización** se auto-completa en el campo vendedor del pedido. Podés cambiarlo si es necesario.

---

## API

```
GET    /api/v1/pedidos                              → Listar pedidos de la empresa
POST   /api/v1/pedidos                              → Crear pedido
PUT    /api/v1/pedidos/{id}                         → Actualizar (solo BORRADOR)
POST   /api/v1/pedidos/{id}/confirmar               → Confirmar pedido
POST   /api/v1/pedidos/{id}/cancelar                → Cancelar pedido
GET    /api/v1/pedidos/para-facturar?clienteId=     → Pedidos CONFIRMADO/EN_PROCESO del cliente
```
