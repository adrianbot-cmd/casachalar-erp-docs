# ERP Casa Chalar

Bienvenido al sistema ERP de **Casa Chalar** — repuestos e insumos para climatización y refrigeración, con más de 60 años en el mercado uruguayo.

## ¿Qué es este sistema?

El ERP de Casa Chalar es la plataforma central para gestionar las operaciones del negocio:

- 📦 **Stock e inventario** de repuestos y productos
- 🛒 **Ventas** — cotizaciones, pedidos y facturación
- 💰 **Cobros** — recibos de cobro multi-factura
- 👥 **Clientes y proveedores**
- 💱 **Tipo de cambio** BCU (USD/UYU automático)
- 📊 **Reportes** y análisis

## Acceso al sistema

| Ambiente | URL | Uso |
|----------|-----|-----|
| Producción | https://one-erp.casachalar.com | Operación diaria |
| Desarrollo | http://34.151.246.79 | Pruebas y validación |

## Estado de desarrollo

### ✅ Módulos implementados

| Módulo | Backend | Frontend | Estado |
|--------|---------|----------|--------|
| Autenticación JWT | ✅ | ✅ Login | ✅ Completo |
| Empresas | ✅ | — | ✅ Completo |
| Sucursales | ✅ | — | ✅ Completo |
| Tipo de cambio (BCU) | ✅ auto + manual | ✅ | ✅ Completo |
| Artículos | ✅ CRUD | ✅ CRUD | ✅ Completo |
| Categorías de artículo | ✅ CRUD | ✅ CRUD | ✅ Completo |
| Marcas | ✅ CRUD | ✅ CRUD | ✅ Completo |
| Listas de precio | ✅ CRUD | ✅ CRUD + drawer precios | ✅ Completo |
| Proveedores | ✅ CRUD | ✅ CRUD | ✅ Completo |
| Clientes | ✅ CRUD + descuentos | ✅ CRUD inline | ✅ Completo |
| Usuarios | ✅ CRUD | ✅ CRUD | ✅ Completo |
| Stock por sucursal | ✅ + movimientos | ✅ Vista | ✅ Completo |
| Vendedores | ✅ (migrados del legacy) | — | ✅ Completo |
| **Cotizaciones** | ✅ CRUD + estados | ✅ Formulario inline | ✅ Completo |
| **Pedidos de Venta** | ✅ CRUD + estados | ✅ Desde cotización | ✅ Completo |
| **Facturas** | ✅ CRUD + tipos | ✅ Desde pedido | ✅ Completo |
| **Recibos de Cobro** | ✅ Multi-factura | ✅ Flujo guiado | ✅ Completo |

### ⏳ Próximos módulos

| Módulo | Estado |
|--------|--------|
| Compras locales (órdenes a proveedores) | Pendiente |
| Importaciones (Carpeta de Importación, costos NCM/CBM) | Pendiente |
| Tesorería (caja, cuentas corrientes) | Pendiente |
| Depósitos (PROPIO, ZF, CONSIGNACIÓN) | Pendiente definición |
| Intercompany (transferencias entre empresas) | Pendiente |

## Flujo de ventas

```
Cliente  →  Cotización  →  Pedido  →  Factura  →  Recibo de Cobro
                ↕              ↕
          (BORRADOR→       (BORRADOR→
           ENVIADA→         CONFIRMADO→
           ACEPTADA→        EN_PROCESO→
           CONVERTIDA)      FACTURADO)
```

## Contacto y soporte

- **Tecnología:** Julio Giraudi — julio.giraudi@casachalar.com
- **Sistema:** Adri (agente IA) — adrian.bot@casachalar.com
- **Funcional:** Carlos Bendayan
