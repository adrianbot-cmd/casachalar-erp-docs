# Módulo: Remitos

Los remitos permiten transferir mercadería entre depósitos de la empresa. Cada remito registra la salida de artículos de un depósito de origen y la entrada en un depósito de destino, con trazabilidad completa y control de stock.

## Acceso

Menú lateral → **Almacenes** → **Remitos**

## Permisos

| Operación | Roles permitidos |
|-----------|-----------------|
| Ver remitos | Todos los usuarios autenticados |
| Crear remito (BORRADOR) | ADMIN, SUPER_ADMIN, DEPOSITO |
| Solicitar aprobación | ADMIN, SUPER_ADMIN, DEPOSITO |
| Aprobar remito | ADMIN, SUPER_ADMIN |
| Confirmar remito (mueve stock) | ADMIN, SUPER_ADMIN, DEPOSITO |
| Anular remito | ADMIN, SUPER_ADMIN |

## Numeración

Cada remito recibe un número único con el formato:

```
REM-{empresaId}-{secuencia 6 dígitos}
```

Ejemplo: `REM-1-000042`, `REM-2-000001`

La secuencia es **independiente por empresa** y se incrementa automáticamente.

## Estados del remito

```
BORRADOR → PENDIENTE_APROBACION → APROBADO → CONFIRMADO
                                           ↘ ANULADO
```

| Estado | Descripción | ¿Mueve stock? |
|--------|-------------|---------------|
| `BORRADOR` | Remito en preparación, se puede editar | No |
| `PENDIENTE_APROBACION` | Esperando aprobación de un superior | No |
| `APROBADO` | Aprobado, listo para confirmar | No |
| `CONFIRMADO` | Ejecutado: stock movido de origen a destino | **Sí** |
| `ANULADO` | Cancelado. Si estaba CONFIRMADO, el stock se revierte | Revierte |

!!! warning "Anular un remito CONFIRMADO"
    Si se anula un remito que ya fue confirmado, el sistema **revierte automáticamente el movimiento de stock**: suma de vuelta en origen y resta en destino. Esta operación no se puede deshacer.

## Flujo paso a paso

### 1. Crear un remito (BORRADOR)

1. Ir a **Almacenes** → **Remitos**
2. Hacer clic en **Nuevo Remito**
3. Completar el encabezado:
   - **Depósito origen**: de dónde sale la mercadería
   - **Depósito destino**: a dónde llega la mercadería
   - **Fecha**: fecha del remito
   - **Observaciones** (opcional)
4. Agregar renglones (artículos):
   - Buscar artículo por código o descripción
   - Ingresar la cantidad a transferir
5. Guardar como **BORRADOR**

!!! tip "Validación de stock"
    Al confirmar (no al crear), el sistema valida que haya stock suficiente en el depósito de origen. Si falta stock, la confirmación se rechaza con detalle de los artículos con faltante.

### 2. Solicitar aprobación

Cuando el remito está listo para ser revisado:

1. Abrir el remito en estado BORRADOR
2. Hacer clic en **Solicitar aprobación**
3. El remito pasa a `PENDIENTE_APROBACION`

### 3. Aprobar el remito

Un usuario con rol ADMIN o SUPER_ADMIN revisa el remito:

1. Ir a la lista de remitos en `PENDIENTE_APROBACION`
2. Abrir el remito
3. Revisar los artículos y cantidades
4. Hacer clic en **Aprobar**
5. El remito pasa a `APROBADO`

### 4. Confirmar el remito

Al confirmar, el sistema **mueve efectivamente el stock**:

1. Abrir el remito en estado APROBADO
2. Verificar que los artículos y cantidades son correctos
3. Hacer clic en **Confirmar**
4. El sistema valida el stock disponible en el depósito de origen
5. Si hay stock suficiente → el remito pasa a `CONFIRMADO` y se mueven los artículos
6. Si no hay stock suficiente → se muestra un error con detalle por artículo

### 5. Anular un remito

Si es necesario cancelar el remito:

1. Abrir el remito en cualquier estado (excepto ANULADO)
2. Hacer clic en **Anular**
3. Ingresar el motivo de anulación
4. Confirmar

!!! warning "Anulación de CONFIRMADO"
    Si el remito ya fue confirmado, la anulación revierte todos los movimientos de stock. Verificar las implicancias antes de proceder.

## Renglones del remito

| Campo | Descripción |
|-------|-------------|
| Artículo | Artículo a transferir (código + descripción) |
| Cantidad | Unidades a transferir |
| Unidad de medida | Unidad del artículo |
| Stock disponible en origen | Cantidad actual antes de confirmar |

## Historial de cambios de estado

Cada cambio de estado queda registrado con:
- Estado anterior y nuevo
- Usuario que realizó el cambio
- Fecha y hora
- Observación (para aprobaciones y anulaciones)

## Casos de uso típicos

| Caso | Descripción |
|------|-------------|
| **Reabastecimiento** | Transferir stock del depósito central a una sucursal |
| **Consignación** | Enviar mercadería a un depósito de consignación en un cliente |
| **Devolución interna** | Devolver mercadería de una sucursal al depósito central |
| **Reequilibrio** | Balancear stock entre dos sucursales con depósitos propios |

## Modelo de datos

```
remitos
├── id
├── empresa_id
├── numero              (REM-{empresaId}-{seq})
├── deposito_origen_id  → FK a depositos
├── deposito_destino_id → FK a depositos
├── estado              (BORRADOR/PENDIENTE_APROBACION/APROBADO/CONFIRMADO/ANULADO)
├── fecha
├── observaciones
├── usuario_creacion_id
├── usuario_aprobacion_id
├── usuario_confirmacion_id
├── fecha_confirmacion
└── motivo_anulacion

remito_items
├── id
├── remito_id
├── articulo_id
├── cantidad
└── unidad_medida
```

## API

```
GET    /api/v1/remitos                    → Listar remitos de la empresa
GET    /api/v1/remitos/{id}               → Obtener remito por ID
POST   /api/v1/remitos                    → Crear remito (BORRADOR)
PUT    /api/v1/remitos/{id}               → Editar remito en BORRADOR
POST   /api/v1/remitos/{id}/solicitar     → Solicitar aprobación
POST   /api/v1/remitos/{id}/aprobar       → Aprobar remito
POST   /api/v1/remitos/{id}/confirmar     → Confirmar (mueve stock)
POST   /api/v1/remitos/{id}/anular        → Anular remito
```
