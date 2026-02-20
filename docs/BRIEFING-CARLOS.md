# Briefing para Carlos Bendayan — Estado del Proyecto ERP

**Fecha:** 20 de febrero de 2026  
**Preparado por:** Adri (agente de desarrollo) + Julio Giraudi

---

## ¿Qué es este proyecto?

Estamos construyendo un **ERP propio para Casa Chalar** para reemplazar el sistema actual. El objetivo es tener un sistema moderno, hecho a medida, que se adapte exactamente a cómo trabaja el negocio.

---

## El equipo

| Persona | Rol |
|---------|-----|
| **Julio Giraudi** | Responsable de tecnología e infraestructura |
| **Carlos Bendayan** | Responsable funcional — sabe cómo debe funcionar el software |
| **Adri** (IA) | Agente de desarrollo — escribe el código, gestiona la infra, documenta |

**Adri** es un agente de inteligencia artificial que actúa como desarrollador del equipo. Se comunica por Slack (este canal), por Telegram y por email (adrian.bot@casachalar.com).

---

## Lo que ya está listo (infraestructura)

No necesitás entender los detalles técnicos, pero es útil saber que ya existe:

- ✅ **Repositorio de código** privado en GitHub
- ✅ **Dos servidores en la nube** (Google Cloud, Brasil):
  - Servidor de **desarrollo** — para probar funcionalidades antes de que lleguen a los usuarios
  - Servidor de **producción** — el que usarán los 15 usuarios de Casa Chalar
- ✅ **Base de datos** PostgreSQL configurada
- ✅ **Sistema de deploy automático** — cuando el código se aprueba, se sube automáticamente al servidor
- ✅ **Revisión automática de código** (CodeRabbit) — una IA revisa cada cambio antes de que llegue a producción
- ✅ **Sitio de documentación:** https://adrianbot-cmd.github.io/casachalar-erp-docs/

**Costo mensual de infraestructura: ~$22 USD/mes** (presupuesto seteado en $50 USD con alertas)

---

## La tecnología (para contexto, no hace falta entenderlo en detalle)

- **Backend (lógica del sistema):** Java + Spring Boot
- **Frontend (lo que ven los usuarios):** React (aplicación web moderna)
- **Base de datos:** PostgreSQL

---

## Cómo trabajamos

### El ciclo de desarrollo

```
Carlos define QUÉ debe hacer una funcionalidad
         ↓
Adri escribe el código
         ↓
Un sistema automático revisa que el código sea correcto
         ↓
Julio o Carlos aprueban el cambio
         ↓
El sistema se actualiza automáticamente
```

### Cómo comunicarse con Adri

- **Slack** — en este canal (#erp-casachalar). Simplemente escribí lo que necesitás.
- **Email** — adrian.bot@casachalar.com

Adri recuerda el contexto del proyecto entre conversaciones. Podés preguntarle sobre el estado de cualquier funcionalidad, pedir que explique algo, o pedirle que documente decisiones.

---

## Lo que necesitamos de Carlos

La parte más importante del proyecto depende de vos: **definir cómo debe funcionar cada módulo del ERP**.

### Módulos a definir (propuesta inicial)

1. **Stock e Inventario** — productos, movimientos, depósitos, stock mínimo
2. **Ventas** — presupuestos, pedidos, facturas, remitos
3. **Compras e Importaciones** — órdenes de compra, seguimiento de importaciones
4. **Clientes y Proveedores** — directorio, cuentas corrientes
5. **Finanzas** — cajas, movimientos, reportes
6. **Reportes** — análisis de ventas, stock valorizado, etc.

### Para cada módulo necesitamos que definas:

- ¿Qué información maneja? (campos, datos)
- ¿Qué puede hacer el usuario? (crear, editar, buscar, imprimir...)
- ¿Qué reglas de negocio aplican? (ej: "no se puede facturar sin stock disponible")
- ¿Quién puede hacer qué? (permisos por rol de usuario)

**No hace falta que sea formal.** Podés explicarlo conversando en Slack, como si le explicaras el negocio a alguien nuevo. Adri toma nota, hace preguntas, y lo convierte en funcionalidades.

---

## Próximos pasos

1. **Reunión kickoff con Carlos** — presentación del proyecto y primera definición de módulos
2. **Definir el primer módulo a desarrollar** — probablemente Stock o Ventas
3. **Adri empieza a desarrollar** mientras seguimos definiendo los demás módulos

---

## Preguntas frecuentes

**¿Cuándo va a estar listo el sistema?**
Depende de cuántos módulos se desarrollen y con qué nivel de detalle. El enfoque es entregar funcionalidades completas de a una, empezando por las más críticas.

**¿Puedo ver el sistema mientras se desarrolla?**
Sí — el servidor de desarrollo (`http://34.151.246.79`) muestra el estado actual. A medida que Adri desarrolla funcionalidades, aparecen ahí para que las revisen antes de ir a producción.

**¿Qué pasa si algo no funciona como esperaba?**
Se reporta en Slack, Adri lo corrige, y pasa por el proceso de aprobación antes de volver a producción. El sistema siempre tiene un "backup" del estado anterior.
