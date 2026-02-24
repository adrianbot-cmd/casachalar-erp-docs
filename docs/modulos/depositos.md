# Módulo: Depósitos

Los depósitos son los lugares físicos donde se almacena la mercadería. Pueden ser depósitos propios, zonas francas o mercadería en consignación en manos de un cliente.

## Acceso

Menú lateral → **Almacenes** → **Depósitos**

## Tipos de depósito

| Tipo | Descripción |
|------|-------------|
| **PROPIO** | Depósito propio de la empresa. Almacenamiento interno habitual. |
| **ZF** | Zona Franca. Mercadería bajo régimen aduanero especial. |
| **CONSIGNACION** | Mercadería enviada a un cliente para su venta en consignación. Requiere cliente vinculado. |

!!! info "Consignación"
    Los depósitos de tipo CONSIGNACION están vinculados a un cliente específico. La mercadería está físicamente fuera de la empresa pero sigue siendo de su propiedad hasta la venta efectiva. El flujo de movimiento de stock a un depósito en consignación se gestiona mediante **Remitos** (módulo en desarrollo).

## Modelo de datos

```
depositos
├── id
├── empresa_id
├── nombre
├── tipo              (PROPIO / ZF / CONSIGNACION)
├── cliente_id        (solo obligatorio para tipo CONSIGNACION)
└── activo
```

## Crear un depósito

1. Ir a **Almacenes** → **Depósitos**
2. Hacer clic en **Nuevo Depósito**
3. Completar:
   - **Nombre**: identificador del depósito (ej: "Depósito Central", "ZF Montevideo")
   - **Tipo**: PROPIO, ZF o CONSIGNACION
   - **Cliente** *(solo para CONSIGNACION)*: cliente al que se le envía la mercadería
4. Guardar

!!! warning "Tipo CONSIGNACION sin cliente"
    Si seleccionás el tipo CONSIGNACION, el campo Cliente es obligatorio. El sistema no te permite guardar sin él.

## Permisos

| Operación | Roles permitidos |
|-----------|-----------------|
| Ver depósitos | Todos los usuarios autenticados |
| Crear / editar depósitos | ADMIN, SUPER_ADMIN |

## Estado del módulo

| Funcionalidad | Estado |
|---------------|--------|
| ABM de depósitos | ✅ Disponible |
| Transferencia de stock entre depósitos (Remitos) | ⏳ Pendiente |
| Stock por depósito | ⏳ Pendiente (hoy el stock es por sucursal) |

## API

```
GET  /api/v1/depositos              → Listar depósitos de la empresa
POST /api/v1/depositos              → Crear depósito
PUT  /api/v1/depositos/{id}         → Editar depósito
```

### Ejemplo: crear depósito de consignación

```json
POST /api/v1/depositos
{
  "nombre": "Consignación - Repuestera Norte",
  "tipo": "CONSIGNACION",
  "clienteId": 142
}
```
