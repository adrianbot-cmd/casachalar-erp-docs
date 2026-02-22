# Módulo: Listas de Precio

Las listas de precio agrupan los precios de venta por categoría de cliente o canal. Cada artículo puede tener un precio diferente en cada lista.

## Acceso

Menú lateral → **Listas de Precio**

## Listas disponibles (migradas del sistema legacy)

| Lista | Descripción | Artículos |
|-------|-------------|-----------|
| Lista 1 — Mayorista | Precio mayorista | 1 artículo |
| Lista 2 — Precio de Lista | Precio de catálogo | ~4.000 artículos |
| Lista 3 — Precio Mecánico | Precio para talleres | ~4.000 artículos |
| Lista 4 — Costo | Precio de costo | ~2.000 artículos |

## Asignación a clientes

Cada cliente puede tener una lista de precios asignada en su ficha. Al crear un comprobante, el sistema usa los precios de esa lista. Si el cliente no tiene lista asignada, usa la mejor lista disponible.

## Ver precios de un artículo

En la pantalla de **Listas de Precio**, hacé clic en **Ver precios** de cualquier lista para ver un drawer con todos los artículos y sus precios en esa lista.

## API

```
GET    /api/v1/listas-precio                        → Listar listas de precio de la empresa
POST   /api/v1/listas-precio                        → Crear lista de precio
PUT    /api/v1/listas-precio/{id}                   → Actualizar lista de precio

GET    /api/v1/articulos/mejor-precio               → Map articuloId → mejor precio USD
GET    /api/v1/articulos/selector                   → Listado liviano para selects (id, código, descripción, IVA)
```

## Notas técnicas

- Los precios se almacenan en **USD** internamente.
- Al crear un comprobante en UYU, los precios se convierten usando el tipo de cambio del día.
- El endpoint `/articulos/mejor-precio` devuelve el precio de la **lista con ID más bajo** para cada artículo (prioridad de lista configurada por ID ascendente).
