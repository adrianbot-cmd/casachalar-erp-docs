# Módulo: Usuarios

Gestión de usuarios del sistema ERP.

## Acceso

Menú lateral → **Usuarios**  
*(Solo visible para ADMIN y SUPER_ADMIN)*

## Permisos

| Operación | Roles permitidos |
|-----------|-----------------|
| Ver listado de usuarios | ADMIN, SUPER_ADMIN |
| Crear usuario | ADMIN, SUPER_ADMIN |
| Modificar usuario | ADMIN, SUPER_ADMIN |
| Desactivar usuario | Solo SUPER_ADMIN |

## Campos

| Campo | Obligatorio | Descripción |
|-------|-------------|-------------|
| Username | ✅ | Nombre de usuario para login |
| Password | ✅ (alta) | Contraseña (se guarda cifrada con BCrypt) |
| Nombre | ✅ | Nombre completo del usuario |
| Email | ✅ | Email institucional |
| Rol | ✅ | Rol asignado (ver tabla de roles) |
| Empresa | — | Empresa a la que pertenece (ADMIN asigna su empresa, SUPER_ADMIN puede asignar cualquiera) |
| Activo | — | Estado del usuario |

## Roles disponibles

| Rol | Descripción | Alcance |
|-----|-------------|---------|
| `SUPER_ADMIN` | Acceso total a todas las empresas | Multi-empresa |
| `ADMIN` | Administrador de su empresa | Por empresa |
| `DEPOSITO` | Gestión de artículos y stock | Por empresa |

!!! warning "Crear SUPER_ADMIN"
    Solo un `SUPER_ADMIN` puede crear otro `SUPER_ADMIN`. Los ADMIN no pueden asignar este rol.

## Aislamiento por empresa

- Un ADMIN solo ve y gestiona los usuarios de **su empresa**.
- Un SUPER_ADMIN ve los usuarios de **todas las empresas**.
- La desactivación de usuarios solo la puede hacer un SUPER_ADMIN.

## API

```
GET    /api/v1/usuarios           → Listar usuarios (filtrado por empresa para ADMIN)
GET    /api/v1/usuarios/{id}      → Obtener usuario por ID
POST   /api/v1/usuarios           → Crear usuario
PUT    /api/v1/usuarios/{id}      → Actualizar usuario
DELETE /api/v1/usuarios/{id}      → Desactivar usuario (solo SUPER_ADMIN)
```
