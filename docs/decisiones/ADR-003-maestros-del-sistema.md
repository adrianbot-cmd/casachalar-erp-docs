# ADR-003 — Maestros del Sistema: Artículos, Clientes, Proveedores y Precios

**Fecha:** 2026-02-20  
**Estado:** En definición  
**Participantes:** Julio Giraudi, Adri (agente)  
**Pendiente:** Revisión y aprobación de Carlos Bendayan

---

## Contexto

Con la Capa 1 (fundación: empresas, sucursales, usuarios, tipo de cambio) en producción, el siguiente paso es construir los **maestros del sistema**: las tablas de referencia que todos los módulos operativos (Ventas, Compras, Inventarios) utilizan como base.

---

## Maestros definidos

### ✅ Ya implementados (Capa 1)

| Entidad | Tabla | Estado |
|---------|-------|--------|
| Empresas | `empresas` | ✅ En producción |
| Sucursales | `sucursales` | ✅ En producción |
| Artículos | `articulos` | ✅ Tabla creada, CRUD pendiente |
| Tipo de cambio | `tipo_cambio` | ✅ Automático via BCU |

### 🔲 A construir (Capa 2 — Maestros)

| Entidad | Tabla | Depende de | Estado |
|---------|-------|------------|--------|
| CRUD Artículos | `articulos` | — | ⚙️ En desarrollo |
| Depósitos | `depositos` | Carlos Bendayan (define tipos) | ⏳ Pendiente definición |
| Clientes | `clientes` | Carlos Bendayan | ⏳ Pendiente definición |
| Proveedores | `proveedores` | Carlos Bendayan | ⏳ Pendiente definición |
| Listas de precios | `listas_precios`, `precios_articulos` | Carlos Bendayan | ⏳ Pendiente definición |
| Stock por sucursal | `stock` | Depósitos + Artículos | ⏳ Pendiente |

---

## Decisiones sobre Artículos (ya definidas en ADR-002)

- Código interno por empresa (único por empresa)
- Código de barras Code 128
- Unidad de medida libre (UN, KG, MT, etc.)
- El artículo existe a nivel empresa; el **stock** existe a nivel sucursal/depósito

## Decisiones sobre Depósitos (pendiente Carlos)

Tipos conocidos (ADR-002):
- `PROPIO` — depósito propio de la empresa
- `ZF` — depósito en Zona Franca
- `CONSIGNACION` — mercadería en consignación

Preguntas abiertas:
- ¿Los depósitos son hijos de la sucursal o de la empresa?
- ¿Una sucursal puede tener múltiples depósitos?
- ¿El stock de ZF tiene tratamiento especial en el costo?

## Decisiones sobre Clientes (pendiente Carlos)

Preguntas abiertas:
- ¿Clientes y proveedores son la misma entidad (con un flag) o tablas separadas?
- ¿Se maneja cuenta corriente desde el inicio o solo para módulos posteriores?
- ¿Campos obligatorios? (RUT, razón social, dirección, etc.)
- ¿Clientes pueden ser de otras empresas (intercompany)?

## Decisiones sobre Listas de Precios (pendiente Carlos)

Preguntas abiertas:
- ¿Cuántas listas de precios hay actualmente?
- ¿El precio es en USD o UYU?
- ¿Descuentos por cliente o por lista?
- ¿Vigencia de precios (fecha desde/hasta)?

---

## Plan de desarrollo (propuesto)

### Sprint 1 (sin Carlos)
1. CRUD completo de Artículos (frontend + backend + API)

### Sprint 2 (con Carlos)
2. Depósitos + Stock inicial
3. Clientes y Proveedores
4. Listas de Precios

### Sprint 3
5. Módulo de Inventarios (movimientos de stock)
6. Módulo de Ventas (básico)
