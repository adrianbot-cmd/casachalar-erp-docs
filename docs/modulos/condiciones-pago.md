# Módulo: Condiciones de Pago

Las condiciones de pago definen los plazos en los que se espera el pago de una venta o una compra. Se usan en clientes, proveedores, cotizaciones, pedidos y facturas.

## Acceso

Menú lateral → **Configuración** → **Condiciones de Pago**

## Permisos

| Operación | Roles permitidos |
|-----------|-----------------|
| Ver listado | Todos los usuarios autenticados |
| Crear / editar | ADMIN, SUPER_ADMIN |
| Dar de baja | SUPER_ADMIN |

## Condiciones predefinidas del sistema

El sistema incluye las siguientes condiciones de pago estándar:

| Código | Descripción | Días |
|--------|-------------|------|
| `CONTADO` | Contado | 0 |
| `30_DIAS` | 30 días | 30 |
| `60_DIAS` | 60 días | 60 |
| `90_DIAS` | 90 días | 90 |
| `120_DIAS` | 120 días | 120 |
| `180_DIAS` | 180 días | 180 |

!!! info "Condiciones predefinidas"
    Estas condiciones vienen cargadas de fábrica y no pueden eliminarse. Sí pueden desactivarse si la empresa no las utiliza.

## Crear una condición de pago personalizada

Si las condiciones estándar no cubren las necesidades, se pueden crear condiciones adicionales:

1. Ir a **Configuración** → **Condiciones de Pago**
2. Hacer clic en **Nueva Condición**
3. Completar:
   - **Código**: identificador único (ej: `45_DIAS`)
   - **Descripción**: texto visible en los formularios (ej: "45 días")
   - **Días**: cantidad de días de plazo (0 = contado)
4. Guardar

## Uso en el sistema

### En clientes y proveedores

Cada cliente y proveedor tiene una condición de pago por defecto. Al crear un comprobante para ese cliente/proveedor, la condición se pre-completa automáticamente pero puede modificarse en el comprobante.

### En comprobantes de venta

La condición de pago en una cotización, pedido o factura determina:
- La **fecha de vencimiento** del documento
- Los **días de crédito** otorgados al cliente

```
Fecha de vencimiento = Fecha del comprobante + Días de la condición
```

### En cashflow

Las facturas de venta y de compra se proyectan en el [Cashflow](cashflow.md) según su fecha de vencimiento, que se calcula a partir de la condición de pago.

## Modelo de datos

```
condiciones_pago
├── id
├── empresa_id
├── codigo         (ej: "CONTADO", "30_DIAS")
├── descripcion    (ej: "Contado", "30 días")
├── dias           (0 para contado)
└── activo
```

## API

```
GET    /api/v1/condiciones-pago           → Listar condiciones activas
GET    /api/v1/condiciones-pago/{id}      → Obtener condición por ID
POST   /api/v1/condiciones-pago           → Crear condición
PUT    /api/v1/condiciones-pago/{id}      → Actualizar condición
DELETE /api/v1/condiciones-pago/{id}      → Dar de baja
```
