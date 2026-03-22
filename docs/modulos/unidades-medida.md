# Módulo: Unidades de Medida

Las unidades de medida se asignan a los artículos del catálogo para indicar en qué unidad se comercializa cada producto (pieza, kilogramo, metro, litro, hora de servicio, etc.).

## Acceso

Menú lateral → **Configuración** → **Unidades de Medida**

## Permisos

| Operación | Roles permitidos |
|-----------|-----------------|
| Ver listado | Todos los usuarios autenticados |
| Crear / editar | ADMIN, SUPER_ADMIN |
| Dar de baja | SUPER_ADMIN |

## Unidades predefinidas del sistema

| Código | Descripción |
|--------|-------------|
| `UN` | Unidad (pieza) |
| `KG` | Kilogramo |
| `MT` | Metro |
| `LT` | Litro |
| `HS` | Hora de servicio |
| `MT2` | Metro cuadrado |
| `MT3` | Metro cúbico |
| `GR` | Gramo |
| `CM` | Centímetro |
| `PAR` | Par |
| `JGO` | Juego |
| `KIT` | Kit |

!!! info "Unidad por defecto"
    Si no se asigna unidad al crear un artículo, el sistema utiliza `UN` (Unidad) como valor por defecto.

## Crear una unidad de medida

Si las unidades predefinidas no cubren las necesidades del negocio, se pueden agregar nuevas:

1. Ir a **Configuración** → **Unidades de Medida**
2. Hacer clic en **Nueva Unidad**
3. Completar:
   - **Código**: identificador corto (máx. 10 caracteres, ej: `ML`)
   - **Descripción**: nombre completo (ej: "Mililitro")
4. Guardar

!!! warning "Código único"
    El código de la unidad de medida debe ser único dentro de la empresa. No se pueden repetir códigos.

## Uso en artículos

La unidad de medida se asigna en el [Módulo de Artículos](articulos.md) y aparece visible en:

- Listado de artículos
- Renglones de cotizaciones, pedidos y facturas
- Órdenes de compra y recepciones
- Reportes de stock

## Modelo de datos

```
unidades_medida
├── id
├── empresa_id
├── codigo       (ej: "UN", "KG", "MT")
├── descripcion  (ej: "Unidad", "Kilogramo", "Metro")
└── activo
```

## API

```
GET    /api/v1/unidades-medida           → Listar unidades activas
GET    /api/v1/unidades-medida/{id}      → Obtener unidad por ID
POST   /api/v1/unidades-medida           → Crear unidad
PUT    /api/v1/unidades-medida/{id}      → Actualizar unidad
DELETE /api/v1/unidades-medida/{id}      → Dar de baja
```
