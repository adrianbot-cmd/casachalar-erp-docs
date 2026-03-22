# Módulo: Proveedores

Directorio de proveedores de Casa Chalar: fabricantes nacionales, importadores y proveedores de servicios. Los proveedores se usan en el ciclo de compras (requerimientos, órdenes de compra, recepciones, cuentas por pagar) y como referencia en el catálogo de artículos.

## Acceso

Menú lateral → **Proveedores**

## Permisos

| Operación | Roles permitidos |
|-----------|-----------------|
| Ver listado | Todos los usuarios autenticados |
| Crear proveedor | ADMIN, SUPER_ADMIN |
| Modificar proveedor | ADMIN, SUPER_ADMIN |
| Dar de baja | ADMIN, SUPER_ADMIN |

## Campos

### Datos básicos

| Campo | Obligatorio | Descripción |
|-------|-------------|-------------|
| Razón social | ✅ | Nombre comercial o razón social del proveedor |
| Nombre de fantasía | — | Nombre comercial abreviado o alias |
| Tipo | — | Nacional o Importador |
| RUT | — | RUT uruguayo (o identificación fiscal del país de origen para importadores) |
| Email | — | Email principal de contacto |
| Teléfono | — | Teléfono principal |

### Ubicación

| Campo | Descripción |
|-------|-------------|
| Dirección | Dirección fiscal o de depósito |
| Ciudad | Ciudad del proveedor |
| Departamento | Departamento uruguayo (para nacionales) |
| País | País de origen (default: Uruguay) |

### Condiciones comerciales

| Campo | Descripción |
|-------|-------------|
| **Condición de pago** | Plazo habitual de pago: CONTADO / 30_DIAS / 60_DIAS / etc. Ver [Condiciones de Pago](condiciones-pago.md) |
| **Moneda habitual** | Moneda en que factura el proveedor: UYU o USD |
| **Descuento habitual %** | Descuento que el proveedor otorga habitualmente sobre sus precios de lista |

### Contacto principal

| Campo | Descripción |
|-------|-------------|
| Nombre de contacto | Persona de referencia en el proveedor (ej: "Ventas", "Administración") |
| Cargo | Cargo del contacto (ej: "Vendedor", "Gerente de Ventas") |
| Email del contacto | Email directo del contacto (puede diferir del email general) |
| Teléfono del contacto | Teléfono directo |

### Datos bancarios (para pagos)

| Campo | Descripción |
|-------|-------------|
| Banco | Banco del proveedor |
| Cuenta bancaria | Número de cuenta para transferencias |
| IBAN / SWIFT | Para proveedores del exterior |

## Crear un proveedor

1. Ir a **Proveedores**
2. Hacer clic en **Nuevo Proveedor**
3. Completar razón social (obligatorio) y los datos adicionales disponibles
4. Guardar

!!! tip "Datos mínimos"
    No es necesario tener todos los datos para crear el proveedor. Se puede crear con razón social y completar el resto luego. Sin embargo, para generar órdenes de compra se recomienda tener al menos el email para enviar los documentos.

## Búsqueda y filtros

En el listado se puede filtrar por:
- **Texto**: busca en razón social, nombre de fantasía y RUT
- **Tipo**: Nacional / Importador
- **Estado**: activos / inactivos

## Dar de baja un proveedor

La baja es **lógica** (soft delete). El proveedor queda inactivo y no aparece en los selectores de nuevos comprobantes, pero el historial de compras se conserva.

!!! warning "Proveedor con deuda pendiente"
    Verificar que no queden facturas de proveedor pendientes de pago antes de dar de baja.

## Relación con otros módulos

| Módulo | Relación |
|--------|----------|
| **Artículos** | Un artículo puede tener un proveedor habitual asignado |
| **Requerimientos** | Al crear un requerimiento de compra, se sugiere el proveedor habitual del artículo |
| **Órdenes de Compra** | Las OC se emiten a un proveedor específico |
| **Cuentas por Pagar** | Las facturas de proveedor registran la deuda con el proveedor |
| **Tesorería** | Los pagos a proveedores se registran desde tesorería o cajas |

## Modelo de datos

```
proveedores
├── id
├── empresa_id
├── razon_social
├── nombre_fantasia
├── tipo                  (NACIONAL / IMPORTADOR)
├── rut
├── email
├── telefono
├── direccion
├── ciudad
├── departamento
├── pais
├── condicion_pago        (CONTADO / 30_DIAS / ...)
├── moneda_habitual       (UYU / USD)
├── descuento_habitual_pct
├── contacto_nombre
├── contacto_cargo
├── contacto_email
├── contacto_telefono
├── banco
├── cuenta_bancaria
└── activo
```

## API

```
GET    /api/v1/proveedores           → Listar proveedores activos de la empresa
GET    /api/v1/proveedores/{id}      → Obtener proveedor por ID
POST   /api/v1/proveedores           → Crear proveedor
PUT    /api/v1/proveedores/{id}      → Actualizar proveedor
DELETE /api/v1/proveedores/{id}      → Dar de baja (soft delete)
```

SUPER_ADMIN puede usar `X-Empresa-Id: {id}` para operar sobre otra empresa.

### Ejemplo: crear proveedor

```json
POST /api/v1/proveedores
{
  "razonSocial": "Embraco Argentina S.A.",
  "tipo": "IMPORTADOR",
  "pais": "Argentina",
  "condicionPago": "30_DIAS",
  "monedaHabitual": "USD",
  "contactoNombre": "Martín López",
  "contactoEmail": "mlopez@embraco.com"
}
```
