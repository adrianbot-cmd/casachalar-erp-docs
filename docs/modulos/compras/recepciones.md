# Módulo: Recepciones de Mercadería

Las recepciones registran la entrada física de mercadería contra una Orden de Compra. Al guardar una recepción, el stock se actualiza automáticamente y la OC refleja el avance.

## Acceso

Menú lateral → **Compras** → **Recepciones**

## ¿Cuándo registrar una recepción?

Cada vez que llega mercadería del proveedor, ya sea la totalidad de lo pedido o una entrega parcial.

## Registrar una recepción

1. Ir a **Compras** → **Recepciones**
2. Clic en **Nueva Recepción**
3. Seleccionar la **Orden de Compra** (debe estar en estado ABIERTA o PARCIALMENTE_RECIBIDA)
4. Completar:
   - **Número de remito del proveedor** *(opcional pero recomendado para auditoría)*
   - **Fecha de recepción**
   - **Observaciones** *(opcional)*
5. Por cada ítem de la OC, ingresar:
   - **Cantidad recibida** — puede ser menor a la pendiente (recepción parcial)
   - **Costo unitario** — precio real de la factura del proveedor
6. Guardar

## ¿Qué sucede al guardar?

Al registrar una recepción, el sistema automáticamente:

1. **Actualiza el stock** — registra un movimiento tipo `ENTRADA` en `movimientos_stock`
2. **Actualiza el costo promedio ponderado** del artículo en la sucursal
3. **Actualiza la OC**:
   - Si quedan ítems pendientes → estado `PARCIALMENTE_RECIBIDA`
   - Si todo fue recibido (dentro de la tolerancia) → estado `COMPLETADA`

!!! info "Costo promedio ponderado"
    El costo unitario que ingresás en la recepción se usa para actualizar el costo promedio del artículo:
    ```
    nuevo_costo = (stock_actual × costo_actual + cantidad_recibida × costo_nuevo) / (stock_actual + cantidad_recibida)
    ```

## Recepción parcial

Es totalmente válido recibir solo una parte de la OC:

- La OC pasa a `PARCIALMENTE_RECIBIDA`
- Podés registrar múltiples recepciones contra la misma OC hasta completarla
- El historial muestra todas las recepciones con sus fechas y cantidades

!!! warning "Tolerancia de recepción"
    Si la cantidad recibida supera lo pedido más la tolerancia configurada en la OC, el sistema muestra una advertencia. En la versión actual se permite continuar; la autorización automática se implementará en una versión futura.

## Historial de recepciones

Desde el detalle de una OC podés ver todas las recepciones asociadas:
- Fecha, número de remito, cantidades por ítem, costo registrado

## Modelo de datos

```
recepciones_compra
├── id
├── oc_id
├── fecha
├── numero_remito_proveedor
└── observaciones

recepciones_compra_items
├── id
├── recepcion_id
├── articulo_id
├── cantidad_recibida
└── costo_unitario
```

## Permisos

| Operación | Roles permitidos |
|-----------|-----------------|
| Ver recepciones | Todos los usuarios autenticados |
| Registrar recepción | ADMIN, SUPER_ADMIN, DEPOSITO, COMPRAS |

## API

```
GET  /api/v1/recepciones-compra                   → Listar recepciones
GET  /api/v1/recepciones-compra?ocId={id}         → Recepciones de una OC específica
POST /api/v1/recepciones-compra                   → Registrar recepción
```

### Ejemplo: registrar recepción parcial

```json
POST /api/v1/recepciones-compra
{
  "ocId": 7,
  "fecha": "2026-02-24",
  "numeroRemitoProveedor": "R-0045678",
  "observaciones": "Faltan 5 unidades, confirman entrega semana que viene",
  "items": [
    {
      "articuloId": 234,
      "cantidadRecibida": 15,
      "costoUnitario": 448.50
    }
  ]
}
```
