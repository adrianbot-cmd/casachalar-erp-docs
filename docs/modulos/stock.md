# Módulo: Stock

Control de inventario por artículo. El módulo Stock proporciona la vista consolidada del inventario de la empresa, con herramientas para consultar niveles actuales, registrar ajustes manuales y revisar el historial de movimientos.

## Acceso

Menú lateral → **Stock**

## Permisos

| Operación | Roles permitidos |
|-----------|-----------------|
| Ver stock | Todos los usuarios autenticados |
| Ajustar stock manualmente | ADMIN, SUPER_ADMIN, DEPOSITO |
| Ver historial de movimientos | Todos los usuarios autenticados |

## Concepto: Stock por depósito

El stock se registra por la combinación **artículo + depósito**. Cada artículo puede tener stock en uno o varios depósitos simultáneamente. La tabla `stock_deposito` almacena la cantidad disponible en cada combinación.

Para ver el stock desglosado por depósito y hacer ajustes específicos por depósito, ver el módulo [Stock por Depósito](stock-deposito.md).

## Vista consolidada

La pantalla principal del módulo Stock muestra el stock **total** de cada artículo (suma de todos los depósitos de la empresa):

| Columna | Descripción |
|---------|-------------|
| Código | Código del artículo |
| Descripción | Descripción del artículo |
| Unidad | Unidad de medida |
| Stock total | Suma de todos los depósitos |
| Costo unitario | Costo promedio ponderado actual |
| Valor stock | Stock × Costo unitario |

### Filtros

- **Buscar por artículo**: código o descripción
- **Filtrar por categoría**: ver el stock de una línea de productos
- **Con stock / Sin stock**: ver solo artículos con existencias o solo los agotados

## Tipos de movimiento de stock

Todos los movimientos de stock quedan registrados en el historial con su tipo:

| Tipo | Descripción | Origen |
|------|-------------|--------|
| `ENTRADA_COMPRA` | Ingreso por recepción de una orden de compra | Automático — Recepciones |
| `SALIDA_VENTA` | Egreso por confirmación de factura de venta | Automático — Facturas |
| `SALIDA_REMITO` | Egreso del depósito de origen de un remito | Automático — Remitos |
| `ENTRADA_REMITO` | Ingreso al depósito de destino de un remito | Automático — Remitos |
| `AJUSTE_MANUAL` | Corrección manual ingresada por un operador | Manual |
| `INVENTARIO_AJUSTE` | Ajuste resultante del cierre de un inventario físico | Automático — Inventario Físico |
| `DEVOLUCION_CLIENTE` | Devolución de mercadería por un cliente | Manual |
| `DEVOLUCION_PROVEEDOR` | Devolución de mercadería a un proveedor | Manual |

!!! info "Trazabilidad completa"
    Cada movimiento registra: tipo, artículo, depósito, cantidad, costo unitario, referencia al comprobante origen (si aplica), usuario y fecha/hora. Los movimientos no se pueden eliminar.

## Realizar un ajuste manual de stock

Para corregir el stock de un artículo en un depósito puntual (fuera de un inventario formal):

1. Ir a **Stock**
2. Hacer clic en **Nuevo ajuste**
3. Completar:
   - **Depósito**: depósito donde se registra el movimiento
   - **Artículo**: artículo afectado (buscar por código o descripción)
   - **Tipo**: ENTRADA o SALIDA (o seleccionar la cantidad nueva directamente)
   - **Cantidad**: unidades a ajustar
   - **Costo unitario** (opcional): para actualizar la valorización
   - **Referencia**: descripción del motivo (ej: "Corrección por rotura", "Diferencia conteo")
4. Guardar

Cada ajuste queda registrado en el historial con el tipo `AJUSTE_MANUAL`.

!!! tip "Ajuste masivo"
    Para ajustar múltiples artículos a la vez (resultado de un conteo físico), usar el módulo [Inventario Físico](inventario-fisico.md) que tiene un flujo formal con comparación de diferencias.

## Historial de movimientos

El historial muestra todos los movimientos de stock de la empresa:

1. Ir a **Stock** → **Historial de movimientos**
2. Aplicar filtros según necesidad:
   - **Artículo**: ver el historial de un artículo específico
   - **Depósito**: ver los movimientos de un depósito
   - **Tipo de movimiento**: filtrar por tipo
   - **Rango de fechas**: movimientos en un período
   - **Usuario**: movimientos realizados por un usuario específico

### Columnas del historial

| Columna | Descripción |
|---------|-------------|
| Fecha/hora | Momento exacto del movimiento |
| Tipo | Tipo de movimiento |
| Artículo | Código y descripción |
| Depósito | Depósito afectado |
| Cantidad | Positiva = entrada, negativa = salida |
| Costo unitario | Costo en el momento del movimiento |
| Referencia | Número de comprobante origen o descripción |
| Usuario | Quién registró el movimiento |

## Modelo de datos

```
stock_deposito
├── empresa_id
├── deposito_id       → FK a depositos
├── articulo_id       → FK a articulos
└── cantidad          (stock actual — se actualiza con cada movimiento)

movimientos_stock
├── id
├── empresa_id
├── deposito_id
├── articulo_id
├── tipo              (ENTRADA_COMPRA / SALIDA_VENTA / AJUSTE_MANUAL / ...)
├── cantidad          (positiva = entrada, negativa = salida)
├── costo_unitario    (al momento del movimiento)
├── referencia        (texto o ID del comprobante origen)
├── comprobante_tipo  (FACTURA / ORDEN_COMPRA / REMITO / INVENTARIO / null)
├── comprobante_id    (FK al comprobante origen, si aplica)
├── usuario_id        → FK a usuarios
└── fecha_hora
```

## API

```
GET  /api/v1/stock                              → Stock consolidado de la empresa
GET  /api/v1/stock/deposito/{depositoId}        → Stock de un depósito
GET  /api/v1/stock/articulo/{articuloId}        → Stock de un artículo en todos los depósitos
GET  /api/v1/stock/movimientos                  → Historial de movimientos
POST /api/v1/stock/ajuste                       → Registrar ajuste manual
```

### Ejemplo: ajuste manual

```json
POST /api/v1/stock/ajuste
{
  "depositoId": 1,
  "articuloId": 42,
  "cantidad": 5,
  "costoUnitario": 15.50,
  "referencia": "Diferencia encontrada en conteo parcial"
}
```

```
cantidad positiva (+5) → ENTRADA al depósito
cantidad negativa (-3) → SALIDA del depósito
```

## Notas

- SUPER_ADMIN puede consultar stock de cualquier empresa con `X-Empresa-Id`.
- Los movimientos son permanentes y no pueden eliminarse (auditoría).
- El costo unitario se actualiza mediante **costo promedio ponderado** con cada entrada.
