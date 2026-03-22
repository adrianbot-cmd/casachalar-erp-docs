# Módulo: Cajas

Las cajas registran todos los movimientos de dinero en efectivo de cada sucursal. Permiten controlar el saldo disponible, los ingresos y egresos del día, y los cobros y pagos realizados en efectivo.

## Acceso

Menú lateral → **Cajas**

## Permisos

| Operación | Roles permitidos |
|-----------|-----------------|
| Ver cajas y movimientos | ADMIN, SUPER_ADMIN, VENDEDOR |
| Abrir / Cerrar sesión | ADMIN, SUPER_ADMIN, VENDEDOR |
| Registrar movimientos manuales | ADMIN, SUPER_ADMIN |
| Crear / editar cajas | ADMIN, SUPER_ADMIN |

## Concepto: Caja Principal

Al crear una sucursal, el sistema genera automáticamente una **"Caja Principal"** asociada a esa sucursal. No es necesario crearla manualmente.

Si la empresa necesita múltiples cajas por sucursal (ej: caja 1 y caja 2), se pueden agregar desde la configuración de cajas.

## Sesiones de caja

Una **sesión** representa el período de operación de una caja desde su apertura hasta su cierre. El flujo es:

```
APERTURA de sesión  →  Operaciones del día  →  CIERRE de sesión
(con saldo inicial)        (ingresos/egresos)     (con saldo final)
```

### Abrir sesión

1. Ir a **Cajas**
2. Seleccionar la caja a abrir
3. Hacer clic en **Abrir sesión**
4. Ingresar el **saldo inicial** (dinero en efectivo al comenzar el turno)
5. Confirmar

!!! tip "Saldo inicial"
    El saldo inicial suele ser el sobrante de la sesión anterior o el fondo fijo asignado a la caja. Ingresarlo correctamente es importante para cuadrar al cierre.

### Cerrar sesión

1. Ir a **Cajas** → sesión activa
2. Hacer clic en **Cerrar sesión**
3. Ingresar el **saldo final** (dinero contado en efectivo al cierre)
4. Revisar el resumen de movimientos del período
5. Confirmar cierre

!!! warning "Diferencia al cierre"
    El sistema calcula automáticamente la diferencia entre el saldo teórico (saldo inicial + ingresos - egresos) y el saldo final declarado. Si hay diferencia, queda registrada en el historial.

## Tipos de movimiento

Todos los movimientos tienen un tipo que indica su origen:

| Tipo | Descripción | ¿Automático? |
|------|-------------|--------------|
| `INGRESO` | Entrada de dinero manual (ej: adelanto de cliente, depósito recibido) | No — manual |
| `EGRESO` | Salida de dinero manual (ej: pago de gastos chicos, rendiciones) | No — manual |
| `COBRO_FACTURA` | Cobro en efectivo de una factura de venta | Sí — generado al cobrar |
| `PAGO_PROVEEDOR` | Pago en efectivo a un proveedor | Sí — generado al pagar |

!!! info "Movimientos automáticos"
    Los movimientos de tipo `COBRO_FACTURA` y `PAGO_PROVEEDOR` se generan automáticamente cuando se registra un cobro o pago en efectivo desde los módulos de ventas o compras. No es necesario registrarlos manualmente.

## Registrar un movimiento manual

Para ingresar o egresar dinero manualmente (fuera de cobros/pagos de comprobantes):

1. Ir a la sesión activa de la caja
2. Hacer clic en **Nuevo movimiento**
3. Seleccionar el tipo: `INGRESO` o `EGRESO`
4. Ingresar el **monto**
5. Escribir una **descripción** del motivo
6. Confirmar

!!! warning "Sesión activa requerida"
    Solo se pueden registrar movimientos mientras la caja tenga una sesión abierta. Si la caja está cerrada, primero hay que abrirla.

## Consultar movimientos

En el listado de movimientos de una sesión se muestra:

| Columna | Descripción |
|---------|-------------|
| Fecha/hora | Momento exacto del movimiento |
| Tipo | INGRESO / EGRESO / COBRO_FACTURA / PAGO_PROVEEDOR |
| Descripción | Detalle del movimiento |
| Monto | Importe (positivo = ingreso, negativo = egreso) |
| Saldo acumulado | Saldo de caja después de ese movimiento |
| Usuario | Quién registró el movimiento |

## Saldo de caja

El saldo actual de la caja es:

```
saldo_inicial + Σ ingresos - Σ egresos
```

Donde los `COBRO_FACTURA` suman al saldo y los `PAGO_PROVEEDOR` lo reducen.

## Modelo de datos

```
cajas
├── id
├── empresa_id
├── sucursal_id
├── nombre          (ej: "Caja Principal")
└── activo

sesiones_caja
├── id
├── caja_id
├── usuario_apertura_id
├── usuario_cierre_id
├── saldo_inicial
├── saldo_final
├── fecha_apertura
├── fecha_cierre
└── estado           (ABIERTA / CERRADA)

movimientos_caja
├── id
├── sesion_caja_id
├── tipo             (INGRESO / EGRESO / COBRO_FACTURA / PAGO_PROVEEDOR)
├── monto
├── descripcion
├── referencia_id    (ID de la factura o pago origen, si aplica)
├── usuario_id
└── fecha_hora
```

## Notas

- **Una sola sesión activa por caja**: no se puede abrir una segunda sesión si ya hay una abierta.
- Los movimientos no se pueden eliminar. Si se cometió un error, se registra un movimiento compensatorio.
- SUPER_ADMIN puede consultar cajas de cualquier empresa con `X-Empresa-Id`.

## API

```
GET  /api/v1/cajas                           → Listar cajas de la empresa
GET  /api/v1/cajas/{id}/sesion-activa        → Obtener sesión activa de una caja
POST /api/v1/cajas/{id}/abrir                → Abrir sesión
POST /api/v1/cajas/{id}/cerrar               → Cerrar sesión
GET  /api/v1/cajas/{id}/movimientos          → Listar movimientos de la sesión activa
POST /api/v1/cajas/{id}/movimientos          → Registrar movimiento manual
```
