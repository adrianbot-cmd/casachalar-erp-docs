# ADR-001: Selección del stack tecnológico

**Fecha:** 2026-02-20  
**Estado:** Aceptado  
**Decidido por:** Julio Giraudi, Adri (agente)

## Contexto

Casa Chalar necesita reemplazar su ERP actual. La nueva solución debe ser mantenible a largo plazo, robusta para transacciones comerciales y desarrollable por un agente de IA.

## Decisión

- **Backend:** Spring Boot 3.5 + Java 21
- **Frontend:** React + TypeScript + Vite
- **Base de datos:** PostgreSQL 16
- **Hosting:** GCP (Cloud SQL + Compute Engine)

## Justificación

- Spring Boot es el estándar para ERPs Java: manejo de transacciones ACID, Spring Security maduro, ecosistema enorme.
- PostgreSQL es ideal para datos relacionales complejos de ERP.
- React con Ant Design acelera el desarrollo de UIs de datos (tablas, formularios, reportes).
- GCP con e2-micro + db-f1-micro mantiene costos en ~$22/mes para 15 usuarios.

## Consecuencias

- El agente (Adri) escribe código Spring Boot / React / SQL.
- Los despliegues son automáticos via GitHub Actions.
- La documentación de API se genera automáticamente con OpenAPI/Swagger.
