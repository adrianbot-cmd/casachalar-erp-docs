# Módulo: Tipo de Cambio

El sistema obtiene automáticamente el tipo de cambio USD/UYU del Banco Central del Uruguay (BCU) para usarlo en facturación y valorización de stock.

## Funcionamiento

- Cada vez que se consulta el tipo de cambio del día, el sistema **primero busca en base de datos**.
- Si no existe, **consulta automáticamente la API del BCU** y lo persiste para futuros usos.
- En caso de que la API del BCU no esté disponible, se puede ingresar **manualmente**.

## Ingreso manual

Si el BCU no está disponible o el tipo de cambio del día necesita una corrección:

```
POST /api/v1/tipo-cambio/manual
  ?moneda=USD
  &fecha=2026-02-20
  &valor=43.50
```

Solo `ADMIN` y `SUPER_ADMIN` pueden ingresar valores manuales.

## API

```
GET  /api/v1/tipo-cambio/hoy          → Tipo de cambio USD de hoy
GET  /api/v1/tipo-cambio/{fecha}      → Tipo de cambio para una fecha (YYYY-MM-DD)
POST /api/v1/tipo-cambio/manual       → Ingresar valor manual (ADMIN/SUPER_ADMIN)
```

## Uso en el sistema

- **Ventas**: los precios se manejan en USD y se factura en UYU al tipo de cambio del día.
- **Stock**: el costo unitario se puede expresar en cualquier moneda; la conversión usa este servicio.
- **Importaciones**: los costos en USD de una Carpeta de Importación se convierten a UYU.

## Fuente de datos

API pública del Banco Central del Uruguay (BCU):  
`https://cotizaciones.bcu.gub.uy/wscotizaciones/ServiceCotizaciones.svc`
