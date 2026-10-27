# SaberColectivo

El conocimiento crece cuando se comparte. Webapp comunitaria para descubrir cursos gratuitos y construir un recorrido de aprendizaje personal.

*Estado:* Instancia A — documentación preparada para revisión docente, con V-01 identificado. La aprobación docente no está registrada. No hay aplicación implementada ni comandos de instalación ejecutables del proyecto.

SaberColectivo ofrece un catálogo público de cursos aportados por usuarios y revisados por un administrador designado. Cada usuario tiene un roadmap privado con orden manual y estados pendiente, en curso y completado. Administradores pueden aprobar sus aportes con trazabilidad, corregir metadatos, retirar cursos con motivo y republicarlos tras verificar solución; los roadmaps existentes conservan los cursos retirados con una advertencia. Se admite certificado opcional pago del proveedor; el contenido completo debe ser gratuito y no una prueba temporal.

## Documentación

| Documento | Propósito |
|---|---|
| [SPEC](docs/SPEC.md) | Consigna refinada, decisiones y origen |
| [PRD](docs/PRD.md) | Objetivos, reglas, historias y alcance |
| [TRD](docs/TRD.md) | Arquitectura, seguridad, versiones y flujo Git |
| [UX-UI](docs/UX-UI.md) | Tokens, componentes y accesibilidad |
| [APPFlow](docs/APPFlow.md) | Rutas, pantallas y flujos |
| [BackendSchema](docs/BackendSchema.md) | Entidades, estados y contratos API |
| [Implementation Plan](docs/IMPLEMENTATION_PLAN.md) | Dependencias, tareas, estimaciones y pruebas |
| [Auditoría](docs/AUDITORIA_INSTANCIA_A.md) | Cumplimiento, trazabilidad y pendientes de entrega |

Las plantillas de entrada son material de referencia y no integran los siete entregables. Se conservan en [docs/plantillas](docs/plantillas/). Son referencias de estructura: sus ejemplos no constituyen requisitos aprobados del producto.

## Alcance y categorías

Catálogo, búsqueda y filtro, registro/login/logout, recuperación de contraseña, aportes sin duplicados por URL normalizada, revisión y corrección/reenvío, gestión administrativa y roadmap privado. Diez categorías iniciales, una principal por curso, sin subcategorías; lista completa en PRD §9.

Fuera del MVP: pagos, reseñas, valoraciones, comentarios, chat, recomendaciones automáticas, certificados propios y correos ajenos a recuperación. Sí se informa si el proveedor ofrece certificado y si este es pago. También quedan fuera solicitudes de edición de usuarios e historial completo de valores por campo; la revisión independiente obligatoria se reserva para una evolución futura.

## Stack y ejecución

Stack exigido: React 19 + Vite + pnpm; FastAPI + uv; PostgreSQL 18.6; Docker + Compose. Entorno confirmado: Node 22.23.2, pnpm global 12.15.0, Python 3.12.3, uv 0.12.21, Docker 29.1.3 y Compose 2.40.3. Objetivos sin instalar: React 19.3.1, Vite 8.3.5, FastAPI 0.162.0 y PostgreSQL 18.6. Evidencia y pendientes en TRD §1. Las instrucciones `docker compose up --build` del plan son criterios para B: no hay Compose ni aplicación hoy. No inicializar dependencias ni implementar antes de confirmación explícita de aprobación docente.

## Cómo revisar

Leer SPEC → PRD → TRD/UX-UI → APPFlow/BackendSchema → Implementation Plan y auditoría. GitHub renderiza Markdown y Mermaid; alternativamente usar preview Markdown del editor. El original del profesor se consulta sin alterar historial:

```bash
git show 2128507:docs/SPEC.md
```

## Git y entrega

Primer commit preservado: 2128507. Documentación en docs/instancia-a; commits convencionales por documento o lote coherente. El flujo completo está en TRD §4. No se hace merge ni push sin autorización expresa; no se reescribe historial ni se usa force push. No incluir secretos locales en commits.

| Instancia | Estado |
|---|---|
| A — Documentación | Preparada y auditada para revisión docente; aprobación pendiente, V-01 registrado |
| B — MVP | No iniciada; requiere aprobación docente confirmada |
| C — Evolución/testing avanzado | No iniciada; no se infiere autorización por fechas de plantilla |

Trabajo práctico individual — Ingeniería en Sistemas de Información, Desarrollo de Software S34, UTN Facultad Regional La Plata. No se declara una licencia de publicación sin decisión del autor.
