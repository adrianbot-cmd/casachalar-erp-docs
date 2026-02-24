# Módulo: Requerimientos de Compra

Un requerimiento es la solicitud interna de compra de un artículo o servicio. Inicia el flujo del módulo de compras y puede requerir aprobación antes de proceder.

## Acceso

Menú lateral → **Compras** → **Requerimientos**

## Flujo de estados

```
BORRADOR → PENDIENTE_APROBACION → APROBADO → PROCESADO
                                ↘ RECHAZADO
```

| Estado | Descripción |
|--------|-------------|
| **BORRADOR** | Requerimiento en preparación, aún no enviado |
| **PENDIENTE_APROBACION** | Enviado para aprobación, esperando respuesta |
| **APROBADO** | Aprobado por un ADMIN. Listo para cotizar/comprar |
| **RECHAZADO** | Rechazado. No se procederá con la compra |
| **PROCESADO** | Se adjudicó una cotización y se generó la Orden de Compra |

## Tipos de requerimiento

| Tipo | Descripción | Centro de costos |
|------|-------------|-----------------|
| **MERCADERIA** | Compra de stock para reventa | Opcional |
| **INSUMO** | Materiales de uso interno (papelería, limpieza, etc.) | **Obligatorio** |
| **SERVICIO** | Contratación de servicio externo | **Obligatorio** |
| **BIEN_USO** | Activo fijo (equipamiento, mobiliario, etc.) | **Obligatorio** |

!!! warning "Centro de costos"
    Para los tipos INSUMO, SERVICIO y BIEN_USO, el campo **Centro de costos** es obligatorio. El sistema no permite enviar a aprobación sin completarlo.

## Crear un requerimiento

1. Ir a **Compras** → **Requerimientos**
2. Clic en **Nuevo Requerimiento**
3. Completar:
   - **Tipo**: MERCADERIA, INSUMO, SERVICIO o BIEN_USO
   - **Centro de costos** *(obligatorio si no es MERCADERIA)*
   - **Observaciones** *(opcional)*
4. Agregar los ítems:
   - **Artículo** (selector del catálogo)
   - **Cantidad**
   - **Unidad de medida**
   - **Observación del ítem** *(opcional)*
5. Guardar como **BORRADOR**

## Enviar a aprobación

Desde el requerimiento en BORRADOR:

1. Clic en **Enviar para aprobación**
2. El estado pasa a **PENDIENTE_APROBACION**
3. Los usuarios con rol ADMIN o SUPER_ADMIN verán el requerimiento pendiente

## Aprobar o rechazar

Un ADMIN puede:

- **Aprobar**: el requerimiento pasa a APROBADO y queda listo para cotizar
- **Rechazar**: especificar el motivo; el requerimiento pasa a RECHAZADO

!!! info "¿Quién puede aprobar?"
    Solo los usuarios con rol **ADMIN** o **SUPER_ADMIN** pueden aprobar requerimientos.

## ¿Qué pasa después?

Una vez **APROBADO**, el requerimiento puede incluirse en una **Cotización de Compra** para pedir precio a proveedores. Al adjudicar la cotización ganadora, el requerimiento pasa automáticamente a **PROCESADO**.

## Permisos

| Operación | Roles permitidos |
|-----------|-----------------|
| Ver requerimientos | Todos los usuarios autenticados |
| Crear / editar BORRADOR | Todos los usuarios autenticados |
| Enviar a aprobación | Todos los usuarios autenticados |
| Aprobar / Rechazar | ADMIN, SUPER_ADMIN |

## API

```
GET  /api/v1/requerimientos                              → Listar requerimientos
POST /api/v1/requerimientos                              → Crear requerimiento
PUT  /api/v1/requerimientos/{id}                         → Editar requerimiento (solo BORRADOR)
PUT  /api/v1/requerimientos/{id}/enviar-aprobacion       → Enviar a aprobación
PUT  /api/v1/requerimientos/{id}/aprobar                 → Aprobar (ADMIN)
PUT  /api/v1/requerimientos/{id}/rechazar                → Rechazar (ADMIN)
```

### Ejemplo: crear requerimiento

```json
POST /api/v1/requerimientos
{
  "tipo": "MERCADERIA",
  "observaciones": "Stock crítico de split 12000 BTU",
  "items": [
    {
      "articuloId": 234,
      "cantidad": 20,
      "unidadMedidaId": 1,
      "observacion": "Modelo inverter preferentemente"
    }
  ]
}
```
