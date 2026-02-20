# Módulo: Stock

Control de inventario por artículo y sucursal.

## Acceso

Menú lateral → **Stock**

## Permisos

| Operación | Roles permitidos |
|-----------|-----------------|
| Ver stock | Todos los usuarios autenticados |
| Ajustar stock (entrada/salida/corrección) | ADMIN, SUPER_ADMIN, DEPOSITO |

## Conceptos

### Stock por sucursal

El stock se trackea por la combinación **artículo + sucursal**. Cada movimiento queda registrado en el historial de movimientos para auditoría completa.

### Tipos de ajuste

| Tipo | Descripción |
|------|-------------|
| `ENTRADA` | Ingreso de mercadería (compra, devolución de cliente, etc.) |
| `SALIDA` | Egreso de mercadería (venta, devolución a proveedor, etc.) |
| `AJUSTE` | Corrección de inventario (resultado de conteo físico, etc.) |

## Realizar un ajuste de stock

1. Ir a **Stock** en el menú lateral
2. Hacer clic en **Nuevo ajuste**
3. Completar:
   - **Sucursal**: sucursal donde se realiza el movimiento
   - **Artículo**: artículo afectado
   - **Cantidad**: positiva = entrada, negativa = salida
   - **Costo unitario** (opcional): para valorización
   - **Referencia**: descripción del motivo del ajuste
4. Guardar

Cada ajuste queda registrado con el usuario que lo realizó y la fecha/hora.

## Modelo de datos

```
stock_articulo
├── sucursal_id
├── articulo_id
├── cantidad          (stock actual)
└── costo_unitario    (costo promedio ponderado)

movimientos_stock
├── sucursal_id
├── articulo_id
├── cantidad          (positiva = entrada, negativa = salida)
├── costo_unitario    (al momento del movimiento)
├── referencia        (texto libre)
├── usuario_id        (quién realizó el movimiento)
└── fecha_hora
```

## API

```
GET  /api/v1/stock                        → Stock de toda la empresa (todas las sucursales)
GET  /api/v1/stock/sucursal/{sucursalId}  → Stock de una sucursal específica
POST /api/v1/stock/ajuste                 → Registrar un ajuste de stock
```

### Ejemplo: ajuste de stock

```json
POST /api/v1/stock/ajuste
{
  "sucursalId": 1,
  "articuloId": 42,
  "cantidad": 100,
  "costoUnitario": 15.50,
  "referencia": "Recepción OC #2026-001"
}
```

## Notas

- SUPER_ADMIN puede consultar stock de cualquier empresa con `X-Empresa-Id`.
- Los movimientos quedan permanentemente en la base de datos (no se borran).
