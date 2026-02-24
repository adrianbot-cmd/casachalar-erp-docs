# Módulo: Cashflow

El cashflow muestra la proyección de ingresos y egresos futuros, permitiendo anticipar la liquidez disponible por fecha.

## Acceso

Menú lateral → **Tesorería** → **Cashflow**

## ¿Qué muestra?

El cashflow consolida dos fuentes de información:

### Ingresos proyectados

- **Cheques de terceros** en estado **EN_CARTERA**, agrupados por su `fecha_pago`
  - Representan el dinero que vamos a recibir cuando se cobren los cheques

### Egresos proyectados

- **Facturas de proveedor** en estado PENDIENTE, agrupadas por su `fecha_vencimiento`
  - Representan las obligaciones de pago próximas
- **Cheques propios** emitidos, por su `fecha_pago`
  - Representan los cheques diferidos que emitimos y que aún no fueron cobrados

## Vista del cashflow

La pantalla muestra una tabla con las siguientes columnas:

| Columna | Descripción |
|---------|-------------|
| **Fecha** | Fecha del movimiento proyectado |
| **Ingresos** | Total de cobros esperados ese día |
| **Egresos** | Total de pagos esperados ese día |
| **Saldo del día** | Ingresos − Egresos del día |
| **Saldo acumulado** | Suma acumulada desde el inicio del período |

!!! info "Saldo acumulado negativo"
    Si el saldo acumulado se muestra en rojo, indica que los egresos proyectados superan los ingresos para ese período — señal de que podría faltar liquidez.

## Limitaciones actuales

Este módulo está en su versión inicial. Las siguientes funcionalidades están **pendientes de implementación**:

| Funcionalidad | Estado |
|---------------|--------|
| Cheques de terceros EN_CARTERA | ✅ Incluido |
| Facturas de proveedor vencibles | ✅ Incluido |
| Cheques propios emitidos | ✅ Incluido |
| Movimientos de caja (apertura/cierre) | ⏳ Pendiente |
| Transferencias bancarias | ⏳ Pendiente |
| Filtro por rango de fechas | ⏳ Pendiente |
| Exportación a Excel | ⏳ Pendiente |

## API

```
GET /api/{empresaId}/cashflow      → Proyección de cashflow de la empresa
```

### Respuesta (estructura simplificada)

```json
[
  {
    "fecha": "2026-02-25",
    "ingresos": 45000.00,
    "egresos": 12000.00,
    "saldoDia": 33000.00,
    "saldoAcumulado": 33000.00,
    "detalle": {
      "chequesACobrar": [...],
      "facturasPorVencer": [...],
      "chequesADebitar": [...]
    }
  }
]
```
