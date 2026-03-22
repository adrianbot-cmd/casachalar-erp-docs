# Módulo: Artículos

El catálogo de artículos contiene todos los productos y repuestos que Casa Chalar comercializa. Incluye información comercial (precios, listas), logística (unidad de medida, proveedor habitual) y fiscal (tasa de IVA).

## Acceso

Menú lateral → **Artículos**

## Permisos

| Operación | Roles permitidos |
|-----------|-----------------|
| Ver listado | Todos los usuarios autenticados |
| Crear artículo | ADMIN, SUPER_ADMIN, DEPOSITO |
| Modificar artículo | ADMIN, SUPER_ADMIN, DEPOSITO |
| Dar de baja (soft delete) | ADMIN, SUPER_ADMIN |

## Campos

### Identificación

| Campo | Obligatorio | Descripción |
|-------|-------------|-------------|
| Código | ✅ | Código interno del artículo (único por empresa). Ej: `COMP-X50`, `FILTRO-G3` |
| Código de barras | — | Código de barras Code 128 o EAN-13 para lectura con lector óptico |
| Descripción | ✅ | Descripción completa del artículo (hasta 300 caracteres) |
| Descripción corta | — | Nombre corto para listados y comprobantes (hasta 100 caracteres) |

### Clasificación

| Campo | Obligatorio | Descripción |
|-------|-------------|-------------|
| Categoría | — | Categoría del catálogo (Rubro → Familia → Categoría). Ver [Categorías](categorias.md) |
| Marca | — | Marca o fabricante del artículo |
| Unidad de medida | — | Unidad en la que se comercializa. Ver [Unidades de Medida](unidades-medida.md). Default: `UN` |

### Proveedor y compras

| Campo | Obligatorio | Descripción |
|-------|-------------|-------------|
| Proveedor habitual | — | Proveedor principal para las compras de este artículo. Ver [Proveedores](proveedores.md) |
| Código del proveedor | — | Código con el que el proveedor identifica el artículo (para órdenes de compra) |

### Precios y costos

El sistema maneja precios en **dos monedas** (UYU y USD). El precio en dólares es la referencia principal; el precio en pesos se calcula automáticamente usando el [Tipo de Cambio BCU](tipo-cambio.md).

| Campo | Descripción |
|-------|-------------|
| **Costo (USD)** | Costo de adquisición en dólares. Se actualiza automáticamente al recibir mercadería (costo promedio ponderado) |
| **Costo (UYU)** | Equivalente en pesos al tipo de cambio vigente |
| **Precio de venta (USD)** | Precio base de venta en dólares |
| **Precio de venta (UYU)** | Equivalente en pesos al tipo de cambio vigente |

!!! info "Sistema bimonetario"
    Casa Chalar opera con dos monedas: **pesos uruguayos (UYU)** y **dólares americanos (USD)**. Los artículos tienen precios en ambas monedas. La conversión se hace automáticamente con el tipo de cambio del Banco Central del Uruguay (BCU), actualizado cada día hábil. Ver [Tipo de Cambio](tipo-cambio.md).

!!! tip "Listas de precio"
    Los precios que se muestran en los comprobantes de venta no provienen directamente del artículo, sino de las [Listas de Precio](listas-precio.md). El precio del artículo es el precio base sobre el que se calculan los márgenes de cada lista.

### Configuración fiscal

| Campo | Obligatorio | Descripción |
|-------|-------------|-------------|
| **Tasa de IVA** | ✅ | Porcentaje de IVA aplicable al artículo. Valores habituales: `22%` (tasa básica), `10%` (tasa mínima), `0%` (exento) |

!!! warning "IVA en Uruguay"
    En Uruguay existen tres tasas de IVA: 22% (básica), 10% (mínima, para alimentos básicos y medicamentos), y 0% (exento). Los repuestos de climatización y refrigeración generalmente aplican la tasa básica del 22%. Configurar correctamente la tasa es fundamental para la liquidación fiscal.

## Formulario de creación/edición

1. Ir a **Artículos**
2. Hacer clic en **Nuevo Artículo** (o en el ícono de edición de un artículo existente)
3. Completar los campos del formulario
4. Guardar

## Búsqueda y filtros

En el listado de artículos se puede filtrar por:

- **Texto**: busca en código, código de barras, descripción y descripción corta
- **Categoría**: filtra por categoría del catálogo
- **Marca**: filtra por marca
- **Proveedor**: filtra por proveedor habitual
- **Estado**: activos / inactivos

## Dar de baja un artículo

La baja es **lógica** (soft delete): el artículo queda inactivo pero no se elimina de la base de datos. Los comprobantes históricos que referencian ese artículo mantienen su información completa.

!!! warning "Artículos con stock"
    Si un artículo tiene stock disponible en algún depósito, se recomienda regularizar el stock antes de darlo de baja.

## Relación con otros módulos

| Módulo | Relación |
|--------|----------|
| **Stock** | Cada artículo tiene stock por depósito. Ver [Stock por Depósito](stock-deposito.md) |
| **Listas de precio** | Los precios de venta se definen en las listas. Ver [Listas de Precio](listas-precio.md) |
| **Cotizaciones / Pedidos / Facturas** | Los artículos son los renglones de los comprobantes de venta |
| **Órdenes de compra** | El proveedor habitual se sugiere automáticamente en los requerimientos |

## Modelo de datos

```
articulos
├── id
├── empresa_id
├── codigo                (único por empresa)
├── codigo_barras
├── descripcion
├── descripcion_corta
├── unidad_medida         (UN, KG, MT, LT, HS...)
├── categoria_id          → FK a categorias
├── marca                 (texto libre o FK a marcas)
├── proveedor_habitual_id → FK a proveedores
├── codigo_proveedor
├── tasa_iva_pct          (0, 10, 22)
├── costo_usd
├── costo_uyu
├── precio_usd
├── precio_uyu
└── activo
```

## API

```
GET    /api/v1/articulos           → Listar artículos activos de la empresa
GET    /api/v1/articulos/{id}      → Obtener artículo por ID
POST   /api/v1/articulos           → Crear artículo
PUT    /api/v1/articulos/{id}      → Actualizar artículo
DELETE /api/v1/articulos/{id}      → Dar de baja (soft delete)
```

Todos los endpoints requieren header `Authorization: Bearer {token}`.  
SUPER_ADMIN puede usar `X-Empresa-Id: {id}` para operar sobre otra empresa.

### Ejemplo: crear artículo

```json
POST /api/v1/articulos
{
  "codigo": "COMP-X50",
  "descripcion": "Compresor Embraco 1/5HP R134a",
  "descripcionCorta": "Compresor Embraco 1/5HP",
  "unidadMedida": "UN",
  "categoriaId": 5,
  "marca": "Embraco",
  "proveedorHabitualId": 12,
  "tasaIvaPct": 22,
  "costoUsd": 45.00,
  "precioUsd": 75.00
}
```
