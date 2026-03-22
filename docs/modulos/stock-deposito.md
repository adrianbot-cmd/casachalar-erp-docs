# Módulo: Stock por Depósito

El módulo de Stock por Depósito permite consultar y gestionar el inventario de cada artículo distribuido entre los distintos depósitos de la empresa. A diferencia del stock general (que es por sucursal), aquí se puede ver y ajustar el stock en cada depósito físico.

## Acceso

Menú lateral → **Stock** → **Por Depósito**

## Permisos

| Operación | Roles permitidos |
|-----------|-----------------|
| Consultar stock por depósito | Todos los usuarios autenticados |
| Ajustar stock manualmente | ADMIN, SUPER_ADMIN, DEPOSITO |

## Consultar stock por depósito

### Vista general

La pantalla principal muestra una tabla con el stock de todos los artículos en todos los depósitos:

| Columna | Descripción |
|---------|-------------|
| Artículo | Código y descripción del artículo |
| Depósito | Nombre del depósito |
| Cantidad | Stock disponible actual |
| Última actualización | Fecha y hora del último movimiento |

### Filtros disponibles

- **Por depósito**: ver solo un depósito específico
- **Por artículo**: buscar el stock de un artículo en todos los depósitos
- **Por categoría**: ver todos los artículos de una categoría en un depósito

!!! tip "Vista consolidada"
    Para ver el stock total de un artículo (suma de todos los depósitos), usar el módulo [Stock](stock.md) que muestra la consolidación.

## Modelo de datos

El stock por depósito se almacena en la tabla `stock_deposito`:

```
stock_deposito
├── empresa_id
├── deposito_id    → FK a depositos
├── articulo_id    → FK a articulos
└── cantidad       (stock actual en ese depósito)
```

Cada fila es única por la combinación `(empresa_id, deposito_id, articulo_id)`.

## Ajuste manual de stock

Se puede corregir el stock de un artículo en un depósito específico cuando hay diferencias detectadas fuera de un inventario formal.

### Cómo hacer un ajuste manual

1. Ir a **Stock** → **Por Depósito**
2. Buscar el artículo y depósito a ajustar
3. Hacer clic en **Ajustar**
4. Completar:
   - **Cantidad**: nueva cantidad real (no la diferencia, sino el total)
   - **Motivo**: descripción del motivo del ajuste
5. Confirmar

!!! warning "Ajuste vs. Inventario físico"
    El ajuste manual es para correcciones puntuales. Si necesitás hacer un conteo completo de un depósito o toda la empresa, utilizá el módulo [Inventario Físico](inventario-fisico.md) que tiene un flujo formal con auditoría.

!!! info "Registro de movimiento"
    Todo ajuste manual genera un movimiento de tipo `AJUSTE_MANUAL` en el historial, con el usuario y la fecha registrados.

## Movimientos que afectan el stock por depósito

El stock en `stock_deposito` se actualiza automáticamente por:

| Evento | Tipo de movimiento | Depósito afectado |
|--------|-------------------|-------------------|
| Confirmar recepción de compra | ENTRADA | Depósito de la recepción |
| Confirmar factura de venta | SALIDA | `deposito_stock_id` de la sucursal |
| Confirmar remito | SALIDA/ENTRADA | Depósito origen y destino |
| Cerrar inventario físico | INVENTARIO_AJUSTE | Depósito del inventario |
| Ajuste manual | AJUSTE_MANUAL | Depósito seleccionado |

## Relación con Sucursales

Cada sucursal tiene un campo `deposito_stock_id` que indica de cuál depósito se descuenta el stock al generar una factura de venta. Ver [Sucursales](sucursales.md).

## API

```
GET  /api/v1/stock-deposito                              → Stock de todos los depósitos
GET  /api/v1/stock-deposito?depositoId={id}              → Stock de un depósito
GET  /api/v1/stock-deposito?articuloId={id}              → Stock de un artículo en todos los depósitos
POST /api/v1/stock-deposito/ajuste                       → Registrar ajuste manual
```

### Ejemplo: consultar stock de un artículo

```
GET /api/v1/stock-deposito?articuloId=42
```

```json
[
  { "depositoId": 1, "depositoNombre": "Depósito Central", "cantidad": 150 },
  { "depositoId": 3, "depositoNombre": "ZF Montevideo", "cantidad": 80 }
]
```

### Ejemplo: ajuste manual

```json
POST /api/v1/stock-deposito/ajuste
{
  "depositoId": 1,
  "articuloId": 42,
  "cantidadNueva": 145,
  "motivo": "Corrección por conteo parcial"
}
```
