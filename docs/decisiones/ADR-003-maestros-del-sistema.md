# ADR-003 — Maestros del Sistema: Artículos, Clientes, Proveedores

**Fecha:** 2026-02-20  
**Estado:** ✅ Implementado  
**Participantes:** Julio Giraudi, Adri (agente)

---

## Contexto

Con la Capa 1 (fundación: empresas, sucursales, usuarios, tipo de cambio) operativa, se construyeron los **maestros del sistema**: las tablas de referencia que todos los módulos operativos utilizan como base.

---

## Decisiones implementadas

### Artículos (`articulos`)

- Código interno único por empresa
- Código de barras Code 128 (campo `codigoBarra`, opcional)
- Descripción larga (300 chars) + descripción corta (100 chars)
- Unidad de medida libre: `UN`, `KG`, `MT`, etc. (default `UN`)
- Soft delete (baja lógica, `activo = false`)
- El artículo existe a nivel empresa; el **stock** existe a nivel sucursal

### Proveedores (`proveedores`)

- Razón social, RUT, teléfono, email, dirección
- Soft delete (`activo = false`)
- Aislados por empresa

### Clientes (`clientes`)

- Razón social, RUT, teléfono, email, dirección
- Soft delete (`activo = false`)
- Aislados por empresa

---

## Estado actual

| Entidad | Backend | Frontend | Observaciones |
|---------|---------|----------|---------------|
| Artículos | ✅ CRUD | ✅ CRUD | V5 migration |
| Proveedores | ✅ CRUD | ✅ CRUD | V7 migration |
| Clientes | ✅ CRUD | ✅ CRUD | V8 migration |

---

## Pendiente (próximas capas)

| Entidad | Estado | Referencia |
|---------|--------|------------|
| Depósitos (PROPIO, ZF, CONSIGNACION) | ⏳ Pendiente definición con Carlos | ADR futuro |
| Listas de precios | ⏳ Pendiente definición con Carlos | ADR futuro |
| Precios por artículo | ⏳ Pendiente | Depende de Listas de precios |
