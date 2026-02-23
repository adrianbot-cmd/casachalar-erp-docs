# Primeros pasos

## Acceder al sistema

1. Abrí el navegador (Chrome o Firefox recomendado)
2. Ingresá a la URL del sistema:
   - **Producción:** https://one-erp.casachalar.com
   - **Desarrollo:** http://34.151.246.79
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
├── Categorías
├── Marcas
├── Listas de Precio
├── Clientes
├── Proveedores
├── Stock
├── Usuarios
└── Ventas
    ├── Cotizaciones
    ├── Pedidos
    ├── Facturas
    └── Recibos Cobro
```

---

## Módulos disponibles

### 📦 Artículos
El catálogo de productos y repuestos de la empresa.
- Ver, crear y modificar artículos
- Asignar categoría, marca y proveedor habitual
- Ver [manual de Artículos](../modulos/articulos.md)

### 👥 Clientes
Directorio de clientes con condición de pago y lista de precios.
- Ver, crear y modificar clientes
- Ver [manual de Clientes](../modulos/clientes.md)

### 🚚 Proveedores
Directorio de proveedores nacionales e importadores.
- Ver, crear y modificar proveedores
- Ver [manual de Proveedores](../modulos/proveedores.md)

### 📊 Stock
Inventario en tiempo real por sucursal.
- Consultar stock actual
- Registrar entradas, salidas y ajustes
- Ver [manual de Stock](../modulos/stock.md)

### 🛒 Ventas
El módulo completo de ventas. Ver el [Manual de Ventas](ventas.md) para la guía detallada.

| Sub-módulo | Para qué sirve |
|-----------|---------------|
| **Cotizaciones** | Presupuestos para clientes (paso 1, opcional) |
| **Pedidos** | Confirmación de la venta (paso 2) |
| **Facturas** | Documento fiscal de la venta (paso 3) |
| **Recibos Cobro** | Registro del pago del cliente (paso 4) |

### 👤 Usuarios *(solo ADMIN)*
Gestión de usuarios del sistema y sus roles.
- Ver [manual de Usuarios](../modulos/usuarios.md)

---

## Tipo de cambio

El sistema obtiene automáticamente el tipo de cambio USD/UYU del **Banco Central del Uruguay (BCU)** cada día hábil. Se usa en todos los comprobantes en dólares.

Si el BCU no está disponible, se usa el último valor registrado.

---

## Cerrar sesión

Hacé clic en tu nombre de usuario en la esquina superior del sistema. La sesión expira automáticamente después de un período de inactividad.

---

## Soporte

- **Tecnología:** Julio Giraudi — julio.giraudi@casachalar.com
- **Sistema:** Adri (IA) — adrian.bot@casachalar.com
- **Funcional:** Carlos Bendayan
