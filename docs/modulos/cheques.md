# Módulo: Cheques

El módulo de cheques permite gestionar los cheques recibidos de clientes (cheques de terceros) y los cheques emitidos para pagar a proveedores (cheques propios).

## Acceso

Menú lateral → **Tesorería** → **Cheques**

## Tipos de cheque

### Por origen

| Origen | Descripción | Uso típico |
|--------|-------------|-----------|
| **TERCEROS** | Cheque recibido de un cliente como pago | Se registra al cobrar una factura |
| **PROPIO** | Cheque emitido por la empresa para pagar | Se registra al pagar a un proveedor |

### Por modalidad

| Modalidad | Descripción |
|-----------|-------------|
| **COMUN** | La fecha de pago es igual a la fecha de emisión (al día) |
| **DIFERIDO** | La fecha de pago es posterior a la de emisión (posfechado) |

## Ciclo de vida

### Cheques de TERCEROS

```
EN_CARTERA → DEPOSITADO    (se depositó en el banco)
           → DESCONTADO    (el banco adelantó el efectivo)
           → ENDOSADO      (se transfirió a un proveedor como pago)
           → RECHAZADO     (el banco lo rechazó / rebotó)
```

### Cheques PROPIOS

```
EMITIDO → DEPOSITADO    (el proveedor lo depositó / cobró)
        → RECHAZADO     (rebotó por fondos insuficientes u otro motivo)
```

## Estados

| Estado | Descripción |
|--------|-------------|
| **EN_CARTERA** | Cheque en poder de la empresa, pendiente de acción |
| **DEPOSITADO** | Depositado en banco |
| **DESCONTADO** | El banco adelantó el monto (menos un costo financiero) |
| **ENDOSADO** | Transferido a un tercero (proveedor) como pago |
| **RECHAZADO** | El cheque fue rechazado por el banco |

## Registrar un cheque

Los cheques de terceros se registran automáticamente al crear un **Recibo de Cobro** con forma de pago **Cheque**.

Para registrar un cheque manualmente:

1. Ir a **Tesorería** → **Cheques**
2. Clic en **Nuevo Cheque**
3. Completar:
   - **Número**: número del cheque
   - **Banco**: banco librador
   - **Tipo**: COMUN o DIFERIDO
   - **Origen**: TERCEROS o PROPIO
   - **Monto** y **Moneda** (UYU / USD)
   - **Fecha de emisión**
   - **Fecha de pago** *(obligatoria para DIFERIDO)*
   - **Librador**: nombre de quien emitió el cheque
   - **Beneficiario**: a quién va dirigido
   - **Observaciones** *(opcional)*
4. Guardar — el cheque queda en estado **EN_CARTERA**

!!! warning "Cheques en Boleta Contado"
    Al cobrar una Boleta Contado con cheque, solo se acepta cheque **COMUN** y la **fecha de pago debe ser igual a la fecha del recibo**. Cheques diferidos no están habilitados para cobros al contado.

## Cambiar el estado de un cheque

Desde la lista de cheques, usando el menú de acciones:

- **Depositar** → pasa a DEPOSITADO (indicar banco destino)
- **Descontar** → pasa a DESCONTADO (registra el costo financiero)
- **Endosar** → pasa a ENDOSADO (indicar a quién)
- **Rechazar** → pasa a RECHAZADO (indicar motivo)

## Cashflow

Los cheques de terceros en estado **EN_CARTERA** aparecen en el **Cashflow** agrupados por su `fecha_pago`. Esto permite proyectar cuándo se va a recibir el efectivo.

## Modelo de datos

```
cheques
├── id
├── empresa_id
├── numero
├── banco_id
├── tipo              (COMUN / DIFERIDO)
├── origen            (PROPIO / TERCEROS)
├── estado            (EN_CARTERA / DEPOSITADO / DESCONTADO / RECHAZADO / ENDOSADO)
├── monto
├── moneda            (UYU / USD)
├── fecha_emision
├── fecha_pago
├── librador
├── beneficiario
└── observaciones
```

## Permisos

| Operación | Roles permitidos |
|-----------|-----------------|
| Ver cheques | Todos los usuarios autenticados |
| Crear / editar cheques | ADMIN, SUPER_ADMIN, TESORERIA |
| Cambiar estado | ADMIN, SUPER_ADMIN, TESORERIA |

## API

```
GET  /api/v1/cheques                     → Listar cheques de la empresa
GET  /api/v1/cheques?estado=EN_CARTERA   → Filtrar por estado
POST /api/v1/cheques                     → Registrar cheque
PUT  /api/v1/cheques/{id}/estado         → Cambiar estado del cheque
```

### Ejemplo: registrar cheque diferido de terceros

```json
POST /api/v1/cheques
{
  "numero": "0012345",
  "bancoId": 1,
  "tipo": "DIFERIDO",
  "origen": "TERCEROS",
  "monto": 15000.00,
  "moneda": "UYU",
  "fechaEmision": "2026-02-24",
  "fechaPago": "2026-03-24",
  "librador": "Distribuidora Del Sur SA",
  "beneficiario": "Casa Chalar SA"
}
```
