# Módulo: Rubros, Familias y Categorías

El sistema organiza los artículos en una jerarquía de tres niveles: **Rubro → Familia → Categoría**. Esto permite clasificar el catálogo y filtrar artículos de forma precisa.

## Acceso

Menú lateral → **Catálogos** → **Rubros / Familias / Categorías**

## Jerarquía

```
Rubro
└── Familia
    └── Categoría
        └── Artículo
```

**Ejemplo real:**

```
Climatización
└── Split
    └── Split Inverter
        └── Split Inverter 18000 BTU Marca X
```

## Conceptos

### Rubro

El nivel más alto de clasificación. Agrupa grandes áreas del negocio.

- Ej: Climatización, Refrigeración, Repuestos, Herramientas

### Familia

Subdivisión de un Rubro. Agrupa líneas de producto dentro del rubro.

- Ej: dentro de "Climatización" → Split, Central, Techo, Fan Coil

### Categoría

El nivel más específico. Los artículos se asignan directamente a una Categoría.

- Ej: dentro de "Split" → Split Inverter, Split On/Off, Split Cassette

## Gestión

### Crear un Rubro

1. Ir a **Catálogos** → **Rubros**
2. Clic en **Nuevo Rubro**
3. Ingresar el nombre
4. Guardar

### Crear una Familia

1. Ir a **Catálogos** → **Familias**
2. Clic en **Nueva Familia**
3. Seleccionar el **Rubro** al que pertenece
4. Ingresar el nombre
5. Guardar

### Crear una Categoría

1. Ir a **Catálogos** → **Categorías**
2. Clic en **Nueva Categoría**
3. Seleccionar la **Familia** a la que pertenece
4. Ingresar el nombre
5. Guardar

!!! note "Asignación a artículos"
    Al crear o editar un artículo, el campo **Categoría** muestra todas las categorías disponibles. Seleccionando una categoría, el artículo queda automáticamente clasificado bajo la familia y el rubro correspondientes.

## Modelo de datos

```
rubros
├── id
├── empresa_id
├── nombre
└── activo

familias
├── id
├── rubro_id
├── nombre
└── activo

categorias_articulo
├── id
├── familia_id
├── nombre
└── activo
```

## Permisos

| Operación | Roles permitidos |
|-----------|-----------------|
| Ver rubros / familias / categorías | Todos los usuarios autenticados |
| Crear / editar | ADMIN, SUPER_ADMIN |

## API

```
GET  /api/v1/rubros                       → Listar rubros de la empresa
POST /api/v1/rubros                       → Crear rubro
PUT  /api/v1/rubros/{id}                  → Editar rubro

GET  /api/v1/familias                     → Listar familias (todas o filtradas por rubro)
GET  /api/v1/familias?rubroId={id}        → Familias de un rubro específico
POST /api/v1/familias                     → Crear familia
PUT  /api/v1/familias/{id}               → Editar familia

GET  /api/v1/categorias-articulo                    → Listar categorías
GET  /api/v1/categorias-articulo?familiaId={id}     → Categorías de una familia
POST /api/v1/categorias-articulo                    → Crear categoría
PUT  /api/v1/categorias-articulo/{id}              → Editar categoría
```
