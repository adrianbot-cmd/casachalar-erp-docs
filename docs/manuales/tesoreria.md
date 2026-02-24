# Manual de Tesorería

Guía para operar el módulo de Tesorería del ERP Casa Chalar: bancos, cheques y proyección de caja.

---

## ¿Qué incluye Tesorería?

| Sub-módulo | Para qué sirve |
|-----------|----------------|
| **Bancos** | Maestro de instituciones bancarias |
| **Cheques** | Registro y seguimiento de cheques propios y de terceros |
| **Cashflow** | Proyección de ingresos y egresos futuros |

---

## Bancos

Ir a **Tesorería → Bancos**

Los principales bancos uruguayos ya vienen precargados (BROU, Itaú, Santander, etc.). Solo tenés que agregar un banco si usás uno que no está en la lista.

### Agregar un banco

1. Clic en **Nuevo Banco**
2. Ingresá el nombre y, si lo sabés, el código BCU
3. Guardar

Los bancos se usan al registrar cheques.

---

## Cheques

Ir a **Tesorería → Cheques**

### Tipos de cheque que manejamos

| Origen | Qué es | Ejemplo |
|--------|--------|---------|
| **Terceros** | Cheques que recibimos de clientes | Un cliente nos paga con un cheque |
| **Propios** | Cheques que emitimos nosotros para pagar | Le damos un cheque a un proveedor |

| Modalidad | Qué significa |
|-----------|--------------|
| **Común** | Se cobra el mismo día que se emite |
| **Diferido** | Tiene fecha de cobro futura (posfechado) |

---

### Cómo entra un cheque al sistema

**Opción A — Automático (lo más común)**  
Al registrar un **Recibo de Cobro** con forma de pago *Cheque*, el sistema crea el cheque automáticamente y lo deja en estado **EN_CARTERA**.

**Opción B — Manual**  
Si necesitás registrarlo por fuera de un recibo:

1. Ir a **Tesorería → Cheques → Nuevo Cheque**
2. Completar: número, banco, tipo (COMUN/DIFERIDO), monto, moneda, fechas, librador
3. Guardar → queda en **EN_CARTERA**

---

### Estados del cheque y qué hacer con cada uno

Un cheque de terceros recorre estos estados:

```
EN_CARTERA → DEPOSITADO   (lo mandamos al banco)
           → DESCONTADO   (el banco nos adelantó el efectivo)
           → ENDOSADO     (se lo pasamos a un proveedor como pago)
           → RECHAZADO    (el banco lo rechazó)
```

Para cambiar el estado, buscá el cheque en la lista y usá el menú de acciones:

| Acción | Cuándo usarla |
|--------|--------------|
| **Depositar** | Cuando enviás el cheque al banco para acreditarlo |
| **Descontar** | Cuando el banco te da el efectivo antes del vencimiento (con comisión) |
| **Endosar** | Cuando le transferís el cheque a un proveedor como forma de pago |
| **Rechazar** | Cuando el banco informa que el cheque fue rechazado |

---

### Cheques diferidos — seguimiento

Los cheques diferidos que tenés **EN_CARTERA** aparecen en el **Cashflow** agrupados por su fecha de cobro. Así sabés exactamente cuándo vas a recibir ese dinero.

---

### Nota importante al cobrar con cheque

Al registrar una **Boleta Contado** pagada con cheque:

- Solo se acepta cheque **COMUN** (al día)
- La fecha del cheque debe ser igual a la fecha del recibo

Si el cliente paga con cheque diferido, registralo como **Factura Crédito** + **Recibo de Cobro** con cheque.

---

## Cashflow

Ir a **Tesorería → Cashflow**

El cashflow muestra una proyección de cuánto dinero va a entrar y salir por fecha, basado en:

- ✅ **Ingresos**: cheques de terceros EN_CARTERA (por su fecha de cobro)
- ✅ **Egresos**: facturas de proveedor pendientes (por su fecha de vencimiento)

### Cómo leer el cashflow

| Columna | Descripción |
|---------|-------------|
| **Fecha** | Fecha del movimiento proyectado |
| **Ingresos** | Lo que esperamos cobrar ese día |
| **Egresos** | Lo que tenemos que pagar ese día |
| **Saldo acumulado** | Liquidez acumulada desde hoy |

!!! warning "Saldo en rojo"
    Si el saldo acumulado aparece en rojo para alguna fecha, significa que los pagos proyectados superan los cobros. Conviene revisar si hay cheques diferidos que depositar antes o reprogramar pagos a proveedores.

### ¿Qué NO incluye todavía el cashflow?

- Movimientos de caja (apertura/cierre)
- Transferencias bancarias
- Pagos registrados manualmente

Estas funcionalidades se agregan en próximas versiones.

---

## Preguntas frecuentes

**¿Puedo editar un cheque después de registrarlo?**  
Solo mientras está en estado EN_CARTERA. Una vez que cambió de estado (depositado, descontado, etc.), ya no se puede editar.

**¿Un cheque rechazado recupera el saldo de la factura que pagaba?**  
Hoy no es automático. Si un cheque rebota, tenés que anular el recibo de cobro manualmente para que el saldo de la factura se restaure.

**¿El cashflow trabaja en UYU o USD?**  
Muestra los montos en UYU. Los cheques en USD se convierten usando el tipo de cambio del día.

**¿Puedo ver el historial de movimientos de un cheque?**  
Sí. Desde el detalle de cualquier cheque, podés ver todos los cambios de estado con fecha y usuario.

**¿Quién puede cambiar el estado de un cheque?**  
Usuarios con rol ADMIN, SUPER_ADMIN o TESORERIA.
