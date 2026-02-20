# ADR-002 — Arquitectura de Negocio: Multiempresa, Moneda y Sucursales

**Fecha:** 2026-02-20  
**Estado:** Aceptado  
**Participantes:** Julio Giraudi, Carlos Bendayan, Adri (agente)

---

## Contexto

Casa Chalar opera con dos empresas jurídicas distintas que comparten operaciones comerciales. Antes de comenzar el desarrollo de los módulos funcionales, se necesitaba definir la arquitectura base del negocio: cómo se relacionan las empresas, cómo se maneja la moneda, y cómo se estructuran las sucursales.

---

## Decisiones

### 1. Multiempresa

- **2 empresas iniciales**, con operación independiente entre sí
- Cada empresa tiene su propia contabilidad, stock, caja y usuarios
- Se soporta **intercompany** (operaciones entre empresas):
  - **Stock:** desde el inicio (transferencias de mercadería entre empresas)
  - **Dinero (cuentas corrientes intercompany):** a futuro, en una segunda etapa

### 2. Seguridad y accesos

- **`super_admin`:** rol con acceso a todas las empresas (para administración del sistema)
- **Usuarios por empresa:** cada usuario pertenece a una empresa y tiene un rol dentro de ella
- No hay usuarios con acceso cruzado a múltiples empresas (salvo `super_admin`)

### 3. Moneda base

- **UYU (Peso Uruguayo)** como moneda base para ambas empresas
- Toda la contabilidad interna se registra en UYU

### 4. Multimoneda

- Se soporta operación en múltiples monedas (principalmente USD)
- Tabla `tipo_cambio` con tipo de cambio diario
- **Fuente primaria:** API pública del BCU (Banco Central del Uruguay)
- **Fallback:** ingreso manual si la API del BCU no está disponible
- Los precios de venta se publican en USD, pero la facturación se emite en UYU al tipo de cambio del día

### 5. Multipaís

- **Alcance inicial:** Uruguay únicamente
- **Arquitectura preparada** para incorporar otros países en el futuro (estructura de impuestos, monedas y documentos fiscal por país)

### 6. Sucursales

- Cada empresa puede tener **N sucursales**
- **Stock:** gestionado por sucursal (cada sucursal tiene su propio inventario)
- **Caja:** gestionada por sucursal
- **Lista de precios:** asignada por sucursal (una sucursal puede tener una lista de precios diferente a otra)

---

## Módulos iniciales definidos

| Módulo | Descripción |
|--------|-------------|
| **Inventarios** | Stock por sucursal, depósitos (PROPIO / ZF / CONSIGNACION), código de barras Code 128 |
| **Compras** | Órdenes locales + importaciones con Carpeta de Importación (costos por CBM y posición arancelaria NCM) |
| **Ventas** | Precios en USD, facturación en UYU al tipo de cambio BCU del día |
| **Tesorería** | Pendiente de definición detallada |

---

## Pendiente de definir

- Depósitos locales específicos (Carlos Bendayan lo aporta)
- Precio de valorización de stock en transferencias intercompany (¿costo? ¿precio de lista?)
- Definición completa del módulo Tesorería
- Definición completa del módulo Ventas

---

## Consecuencias

- La base de datos tendrá una tabla `empresas` y todas las entidades principales estarán asociadas a una empresa
- El esquema de seguridad requiere un campo `empresa_id` en la sesión del usuario
- La tabla `tipo_cambio` se cargará diariamente (cron job consultando BCU)
- Los documentos de venta almacenarán tanto el importe en USD como la cotización usada y el importe en UYU
