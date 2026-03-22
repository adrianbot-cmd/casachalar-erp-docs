# Primeros pasos

## Acceder al sistema

1. Abrí el navegador (Chrome o Firefox recomendado)
2. Ingresá a la URL del sistema:
   - **Producción:** https://one-erp.casachalar.com
3. Ingresá tu **usuario** y **contraseña**
4. Hacé clic en **Ingresar**

!!! note "Primera vez"
    Si es tu primera vez, pedí las credenciales a Julio Giraudi o Carlos Bendayan.

---

## Navegación

El sistema tiene un **menú lateral izquierdo** con acceso a todos los módulos:

```
Dashboard
├── Artículos
├── Catálogos
│   ├── Rubros / Familias / Categorías
│   └── Marcas
├── Listas de Precio
├── Clientes
├── Proveedores
├── Vendedores
├── Stock
│   ├── Vista general (consolidada)
│   ├── Por Depósito
│   └── Inventario Físico
├── Almacenes
│   ├── Depósitos
│   ├── Sucursales
│   └── Remitos
├── Cajas
├── Usuarios
├── Ventas
│   ├── Cotizaciones
│   ├── Pedidos
│   ├── Facturas
│   └── Recibos Cobro
├── Compras
│   ├── Requerimientos
│   ├── Cotizaciones de Compra
│   ├── Órdenes de Compra
│   ├── Recepciones
│   └── Cuentas por Pagar
├── Tesorería
│   ├── Bancos
│   ├── Cheques
│   └── Cashflow
└── Configuración
    ├── Condiciones de Pago
    └── Unidades de Medida
```

---

## Módulos disponibles

### 📦 Artículos
El catálogo de productos y repuestos de la empresa.
- Ver, crear y modificar artículos
- Asignar categoría, marca, proveedor habitual e IVA
- Gestión bimonetaria (precios en UYU y USD)
- Ver [manual de Artículos](../modulos/articulos.md)

### 🗂️ Catálogos
Clasificación jerárquica del catálogo: Rubro → Familia → Categoría.
- Ver [Rubros, Familias y Categorías](../modulos/categorias.md)

### 💲 Listas de Precio
Gestión de listas con márgenes y precios especiales por cliente.
- Ver [manual de Listas de Precio](../modulos/listas-precio.md)

### 👥 Clientes
Directorio de clientes con condición de pago, lista de precios y descuentos automáticos.
- Ver, crear y modificar clientes
- Ver [manual de Clientes](../modulos/clientes.md)

### 🚚 Proveedores
Directorio de proveedores nacionales e importadores.
- Ver, crear y modificar proveedores
- Ver [manual de Proveedores](../modulos/proveedores.md)

### 🧑‍💼 Vendedores
Maestro de vendedores para seguimiento de comprobantes y comisiones.
- Ver [manual de Vendedores](../modulos/vendedores.md)

### 📊 Stock
Inventario en tiempo real por artículo y depósito.

| Sub-módulo | Para qué sirve |
|-----------|---------------|
| **Vista general** | Stock consolidado por artículo (suma todos los depósitos) |
| **Por Depósito** | Stock desglosado por depósito. Ajustes manuales. |
| **Inventario Físico** | Proceso formal de conteo y ajuste por lote |

- Ver [manual de Stock](../modulos/stock.md)
- Ver [Stock por Depósito](../modulos/stock-deposito.md)
- Ver [Inventario Físico](../modulos/inventario-fisico.md)

### 🏭 Almacenes
Gestión de depósitos y transferencias de mercadería.

| Sub-módulo | Para qué sirve |
|-----------|---------------|
| **Depósitos** | ABM de depósitos propios, zonas francas y consignación |
| **Sucursales** | ABM de sucursales con depósito de stock asignado |
| **Remitos** | Transferencias de stock entre depósitos |

- Ver [manual de Depósitos](../modulos/depositos.md)
- Ver [manual de Sucursales](../modulos/sucursales.md)
- Ver [manual de Remitos](../modulos/remitos.md)

### 💵 Cajas
Control de movimientos de efectivo por sucursal.
- Apertura y cierre de sesión de caja
- Movimientos de ingreso, egreso, cobros y pagos
- Ver [manual de Cajas](../modulos/cajas.md)

### 🛒 Ventas
El módulo completo de ventas. Ver el [Manual de Ventas](ventas.md) para la guía detallada.

| Sub-módulo | Para qué sirve |
|-----------|---------------|
| **Cotizaciones** | Presupuestos para clientes (paso 1, opcional) |
| **Pedidos** | Confirmación de la venta (paso 2) |
| **Facturas** | Documento fiscal de la venta (paso 3) |
| **Recibos Cobro** | Registro del pago del cliente (paso 4) |

### 🛍️ Compras
Ciclo completo de compras. Ver el [Manual de Compras](compras.md) para la guía detallada.

| Sub-módulo | Para qué sirve |
|-----------|---------------|
| **Requerimientos** | Solicitud interna de compra (con aprobación opcional) |
| **Cotizaciones de Compra** | Pedido de precio a proveedores y comparación |
| **Órdenes de Compra** | Documento formal de compra al proveedor |
| **Recepciones** | Registro de la mercadería que llega |
| **Cuentas por Pagar** | Facturas de proveedor y saldos pendientes |

### 💰 Tesorería
Gestión de cheques y proyección de caja. Ver el [Manual de Tesorería](tesoreria.md) para la guía detallada.

| Sub-módulo | Para qué sirve |
|-----------|---------------|
| **Bancos** | Maestro de instituciones bancarias |
| **Cheques** | Seguimiento de cheques propios y de terceros |
| **Cashflow** | Proyección de ingresos y egresos por fecha |

### ⚙️ Configuración *(solo ADMIN)*
Tablas maestras del sistema.

| Sub-módulo | Para qué sirve |
|-----------|---------------|
| **Condiciones de Pago** | CONTADO / 30 días / 60 días / etc. |
| **Unidades de Medida** | UN, KG, MT, LT, etc. |

- Ver [Condiciones de Pago](../modulos/condiciones-pago.md)
- Ver [Unidades de Medida](../modulos/unidades-medida.md)

### 👤 Usuarios *(solo ADMIN)*
Gestión de usuarios del sistema y sus roles.
- Ver [manual de Usuarios](../modulos/usuarios.md)

---

## Tipo de cambio

El sistema obtiene automáticamente el tipo de cambio USD/UYU del **Banco Central del Uruguay (BCU)** cada día hábil. Se usa en todos los comprobantes en dólares y en la conversión de precios de artículos.

Si el BCU no está disponible, se usa el último valor registrado.

Ver [Tipo de Cambio](../modulos/tipo-cambio.md).

---

## Multiempresa

Casa Chalar opera con **dos empresas** en el mismo sistema. Cada empresa tiene su propio catálogo de artículos, clientes, proveedores, stock y comprobantes.

Al iniciar sesión, el sistema te posiciona en la empresa asignada a tu usuario. Los usuarios con rol SUPER_ADMIN pueden cambiar de empresa desde el menú de configuración.

---

## Cerrar sesión

Hacé clic en tu nombre de usuario en la esquina superior del sistema. La sesión expira automáticamente después de un período de inactividad.

---

## Soporte

- **Tecnología:** Julio Giraudi — julio.giraudi@casachalar.com
- **Sistema:** Adri (IA) — adrian.bot@casachalar.com
- **Funcional:** Carlos Bendayan
