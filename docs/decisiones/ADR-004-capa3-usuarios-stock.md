# ADR-004 — Capa 3: Usuarios, Stock e Integración de Sucursales

**Fecha:** 2026-02-20  
**Estado:** ✅ Implementado  
**Participantes:** Julio Giraudi, Adri (agente)

---

## Contexto

Con los maestros básicos (Artículos, Proveedores, Clientes) implementados, la Capa 3 agrega la gestión de usuarios del sistema y el control de inventario por sucursal.

---

## Módulo: Usuarios

### Decisiones

- Gestión completa de usuarios (alta, modificación, baja lógica)
- Solo `ADMIN` y `SUPER_ADMIN` pueden gestionar usuarios
- Solo `SUPER_ADMIN` puede desactivar usuarios (`DELETE /{id}`)
- Los usuarios están aislados por empresa (SUPER_ADMIN ve todos)
- Campos: `username`, `password` (BCrypt), `nombre`, `email`, `rol`, `empresa`, `activo`

### Roles definidos

| Rol | Descripción |
|-----|-------------|
| `SUPER_ADMIN` | Acceso total a todas las empresas |
| `ADMIN` | Administrador de su empresa |
| `DEPOSITO` | Puede gestionar artículos y stock |

---

## Módulo: Stock por Sucursal

### Decisiones

- El stock se trackea por **artículo + sucursal**
- Tabla `stock_articulo`: cantidad actual + costo unitario promedio
- Tabla `movimientos_stock`: auditoría completa de cada movimiento
- Los ajustes de stock se registran con tipo, cantidad, referencia y usuario que lo hizo
- Tipos de ajuste: `ENTRADA`, `SALIDA`, `AJUSTE`
- Solo `ADMIN`, `SUPER_ADMIN` y `DEPOSITO` pueden crear ajustes
- Todos los usuarios autenticados pueden ver el stock

### Modelo

```
movimientos_stock
├── sucursal_id
├── articulo_id
├── cantidad        (positiva = entrada, negativa = salida)
├── costo_unitario  (opcional)
├── referencia      (texto libre)
├── usuario_id      (quién hizo el movimiento)
└── fecha_hora
```

---

## Estado actual

| Entidad | Backend | Frontend | Observaciones |
|---------|---------|----------|---------------|
| Usuarios | ✅ CRUD | ✅ CRUD | Solo ADMIN/SUPER_ADMIN |
| Stock (vista) | ✅ | ✅ Vista | Por empresa y por sucursal |
| Ajuste de stock | ✅ | ✅ Form | Con movimiento auditado |
| Sucursales (lectura) | ✅ | — | Solo lectura por ahora |
| Roles (lectura) | ✅ | — | Usados en alta de usuarios |

---

## Pendiente

- CRUD de Sucursales (alta/modificación vía UI)
- Stock negativo: definir si se permite o no
- Depósitos: cuando Carlos defina los tipos (PROPIO, ZF, CONSIGNACION)
- Stock por depósito (subconjunto de la sucursal)
