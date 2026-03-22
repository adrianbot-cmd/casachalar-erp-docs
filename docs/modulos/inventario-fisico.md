# Módulo: Inventario Físico

El inventario físico es el proceso de contar manualmente las existencias reales en un depósito o sucursal y comparar con el stock registrado en el sistema. Al cerrar el inventario, el sistema aplica automáticamente los ajustes necesarios para corregir las diferencias.

## Acceso

Menú lateral → **Stock** → **Inventario Físico**

## Permisos

| Operación | Roles permitidos |
|-----------|-----------------|
| Ver inventarios | Todos los usuarios autenticados |
| Crear inventario | ADMIN, SUPER_ADMIN, DEPOSITO |
| Cargar conteo (editar cantidades) | ADMIN, SUPER_ADMIN, DEPOSITO |
| Cerrar inventario (aplica ajustes) | ADMIN, SUPER_ADMIN |
| Anular inventario | SUPER_ADMIN |

## Alcance del inventario

Al crear un inventario físico, se define el **alcance**: qué porción del stock se va a contar.

| Alcance | Descripción |
|---------|-------------|
| **Depósito** | Se cuentan todos los artículos de un depósito específico |
| **Sucursal** | Se cuentan todos los depósitos de una sucursal |
| **Empresa** | Se cuentan todos los depósitos de la empresa |

!!! tip "Recomendación"
    Para inventarios parciales o periódicos, usar alcance **Depósito**. Para el inventario anual, usar alcance **Empresa**.

## Flujo del inventario físico

```
Crear inventario → Cargar conteo → Revisar diferencias → Cerrar (aplica ajustes)
```

### 1. Crear el inventario

1. Ir a **Stock** → **Inventario Físico**
2. Hacer clic en **Nuevo Inventario**
3. Seleccionar:
   - **Alcance**: Depósito, Sucursal o Empresa
   - **Depósito** / **Sucursal** (según el alcance elegido)
   - **Fecha** del inventario
   - **Descripción** (opcional, ej: "Inventario anual 2025")
4. Confirmar creación

!!! info "Carga automática de cantidades del sistema"
    Al crear el inventario, el sistema carga automáticamente la **cantidad del sistema** (`cantidad_sistema`) para cada artículo en el alcance seleccionado. Esto refleja el stock registrado en ese momento.

!!! warning "Stock en movimiento"
    Se recomienda evitar movimientos de stock (ventas, recepciones, remitos) mientras el inventario está abierto para minimizar diferencias artificiales. En la práctica, para inventarios de depósito se puede coordinar con el equipo.

### 2. Cargar el conteo físico

Esta es la etapa de trabajo en el depósito. Para cada artículo:

1. Contar físicamente las unidades
2. Ir al inventario en el sistema
3. Buscar el artículo
4. Ingresar la **cantidad contada** (`cantidad_contada`)
5. Guardar

!!! tip "Trabajo en equipo"
    Múltiples usuarios con rol DEPOSITO pueden cargar el conteo simultáneamente, siempre que no editen el mismo artículo al mismo tiempo.

#### Filtros para la carga de conteo

| Filtro | Descripción |
|--------|-------------|
| Sin contar | Artículos con `cantidad_contada` vacía |
| Con diferencia | Artículos donde `cantidad_contada ≠ cantidad_sistema` |
| Sin diferencia | Artículos donde los valores coinciden |

### 3. Revisar diferencias

Antes de cerrar, revisar los artículos con diferencia:

| Campo | Descripción |
|-------|-------------|
| Artículo | Código y descripción |
| Cantidad sistema | Stock registrado al iniciar el inventario |
| Cantidad contada | Lo que se contó físicamente |
| Diferencia | `cantidad_contada - cantidad_sistema` (positiva = sobrante, negativa = faltante) |

!!! warning "Diferencias grandes"
    Si alguna diferencia parece anormalmente grande, verificar si hay un error de conteo antes de cerrar. Una vez cerrado el inventario, los ajustes se aplican automáticamente y no se pueden revertir individualmente.

### 4. Cerrar el inventario

Al cerrar, el sistema aplica los ajustes de stock:

1. Abrir el inventario
2. Verificar que todas las líneas relevantes tienen `cantidad_contada` cargada
3. Hacer clic en **Cerrar inventario**
4. Confirmar la acción

**Resultado del cierre:**
- Para cada artículo con diferencia (`cantidad_contada ≠ cantidad_sistema`), se genera un movimiento de tipo `INVENTARIO_AJUSTE` en `stock_deposito`
- El stock en el sistema queda igualado a la `cantidad_contada`
- El inventario pasa al estado `CERRADO`

#### Ejemplo de ajustes aplicados

| Artículo | Sistema | Contado | Diferencia | Movimiento |
|---------|---------|---------|------------|------------|
| COMPRESOR-X50 | 12 | 10 | -2 | INVENTARIO_AJUSTE: -2 |
| FILTRO-G3 | 50 | 50 | 0 | (ninguno) |
| GAS-R410 | 8 | 11 | +3 | INVENTARIO_AJUSTE: +3 |

!!! info "Artículos sin contar"
    Si un artículo no tiene `cantidad_contada` al cerrar, se interpreta como que **no fue contado** y no se genera ajuste para ese artículo.

## Estados del inventario

| Estado | Descripción |
|--------|-------------|
| `ABIERTO` | En proceso. Se puede editar el conteo. |
| `CERRADO` | Finalizado. Ajustes aplicados. Solo lectura. |
| `ANULADO` | Cancelado sin aplicar ajustes. Solo SUPER_ADMIN. |

## Historial y auditoría

El inventario queda registrado permanentemente con:
- Fecha de creación y cierre
- Usuario que lo creó y quien lo cerró
- Listado completo de artículos con `cantidad_sistema` y `cantidad_contada`
- Referencia a los movimientos `INVENTARIO_AJUSTE` generados

## Modelo de datos

```
inventarios_fisicos
├── id
├── empresa_id
├── descripcion
├── alcance           (DEPOSITO / SUCURSAL / EMPRESA)
├── deposito_id       (null si alcance = EMPRESA)
├── sucursal_id       (null si alcance = EMPRESA)
├── estado            (ABIERTO / CERRADO / ANULADO)
├── fecha
├── fecha_cierre
├── usuario_creacion_id
└── usuario_cierre_id

inventario_items
├── id
├── inventario_id
├── deposito_id
├── articulo_id
├── cantidad_sistema   (snapshot al momento de crear)
├── cantidad_contada   (lo ingresado por el usuario)
└── diferencia         (cantidad_contada - cantidad_sistema, calculado)
```

## API

```
GET    /api/v1/inventarios                      → Listar inventarios de la empresa
GET    /api/v1/inventarios/{id}                 → Obtener inventario por ID
POST   /api/v1/inventarios                      → Crear inventario
GET    /api/v1/inventarios/{id}/items           → Listar artículos del inventario
PUT    /api/v1/inventarios/{id}/items/{itemId}  → Actualizar cantidad contada
POST   /api/v1/inventarios/{id}/cerrar          → Cerrar inventario (aplica ajustes)
POST   /api/v1/inventarios/{id}/anular          → Anular inventario
```
