# Módulo: Pedidos de Venta

Los pedidos de venta confirman el compromiso de entrega con el cliente. Pueden crearse desde una cotización existente o en forma directa.

## Acceso

Menú lateral → **Ventas** → **Pedidos**

## Ciclo de vida

```
BORRADOR → CONFIRMADO → EN_PROCESO → FACTURADO
         ↘ CANCELADO    ↘ CANCELADO
```

| Estado | Descripción |
|--------|-------------|
| **BORRADOR** | Recién creado. Se puede cancelar. |
| **CONFIRMADO** | Confirmado para preparación. Listo para facturar. |
| **EN_PROCESO** | En preparación / armado. Listo para facturar. |
| **FACTURADO** | Se generó una factura. Estado final positivo. |
| **CANCELADO** | Cancelado. Estado final negativo. |

## Crear un pedido

### Opción A: Desde una cotización existente

Al seleccionar un cliente que tiene cotizaciones en estado ENVIADA o ACEPTADA, el sistema muestra una tabla con las cotizaciones disponibles. Al hacer clic en **"Usar esta cotización"**, los ítems se precargan automáticamente.

1. Seleccioná el cliente
2. Aparecen las cotizaciones disponibles del cliente
3. Hacé clic en **"Usar esta cotización"** — los ítems se cargan
4. Ajustá cantidades, precios o descuentos si es necesario
5. Completá: Vendedor, Fecha de entrega, Orden de Compra del cliente (opcional)
6. Guardá

!!! info "¿Qué pasa con la cotización?"
    Al guardar el pedido, la cotización de origen pasa automáticamente al estado **CONVERTIDA**.

### Opción B: Pedido libre (sin cotización previa)

1. Hacé clic en **Nuevo Pedido**
2. Seleccioná la moneda (UYU o USD)
3. Seleccioná el cliente — el descuento del cliente se aplica a los ítems
4. Cargá los ítems manualmente (igual que en cotizaciones)
5. Guardá

## Confirmar un pedido

Desde la tabla, hacé clic en **Confirmar** para pasar el pedido a estado CONFIRMADO. Esto habilita la facturación.

## Cancelar un pedido

Disponible en estados BORRADOR y CONFIRMADO. Una vez cancelado, el pedido no puede reactivarse.

## API

```
GET    /api/v1/pedidos                              → Listar pedidos de la empresa
POST   /api/v1/pedidos                              → Crear pedido
POST   /api/v1/pedidos/{id}/confirmar               → Confirmar pedido
POST   /api/v1/pedidos/{id}/cancelar                → Cancelar pedido
GET    /api/v1/pedidos/para-facturar?clienteId=     → Pedidos CONFIRMADO/EN_PROCESO del cliente
```
