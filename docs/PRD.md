# PRD — Comunidad de cursos

Estado: requisitos redactados; cierre de identidad, categorías y exclusiones pendiente en §10. Fecha: 27/10/2026.

## 1. Resumen ejecutivo

Una comunidad aporta cursos cuyo contenido completo es gratuito. Un administrador revisa cada aporte antes de publicarlo en un catálogo de acceso público. Los usuarios registrados organizan cursos publicados en un único roadmap privado, con orden manual y estados de aprendizaje. Se admite certificado opcional pago y se excluyen pruebas temporales.

## 2. Objetivos y métricas de éxito

Son criterios de aceptación del MVP, no estadísticas de uso observadas ni metas comerciales inventadas.

| Objetivo | Cómo sabemos que lo logramos |
|---|---|
| Descubrimiento confiable | El 100% de los cursos visibles en el conjunto de prueba tiene aprobación registrada; ningún aporte pendiente/rechazado es público. |
| Planificación personal | Un usuario agrega dos cursos, los reordena y cambia sus estados; al volver a ingresar persisten, y otra cuenta no puede acceder. |
| Aportes corregibles | Un rechazo con motivo puede corregirse y reenviarse conservando el historial. |
| Recuperación de acceso | Solicitud, recepción del enlace, nueva contraseña e ingreso funcionan; reutilizar el enlace falla. |

## 3. Personas

Personas hipotéticas para diseñar y verificar requisitos, no investigación de campo.

### Persona 1 — Estudiante que planifica

- **Contexto:** estudia y tiene tiempo limitado; consulta desde celular y computadora.
- **Motivaciones:** encontrar contenido gratuito y priorizar qué estudiar.
- **Frustraciones:** enlaces dispersos, certificados confusos, pérdida del progreso.
- **Historias tipo:** «Quiero guardar lo que estudiaré después». «Necesito retomar donde quedé».

### Persona 2 — Estudiante que aporta

- **Contexto:** encuentra un curso o ya lo tomó y quiere compartirlo.
- **Motivaciones:** ayudar a la comunidad sin volver a cargar datos innecesariamente.
- **Frustraciones:** no saber por qué se rechazó su aporte ni cómo corregirlo.
- **Historias tipo:** «Quiero aclarar si lo tomé». «Quiero corregir lo que observó el administrador».

### Persona 3 — Administrador designado

- **Contexto:** revisa aportes en una cola pendiente.
- **Motivaciones:** mantener enlaces activos, gratuidad y metadatos fiables.
- **Frustraciones:** duplicados y revisiones sin información suficiente.
- **Historias tipo:** «Quiero revisar la página del proveedor». «Necesito dejar un motivo claro al rechazar».

## 4. User stories

P0 significa obligatorio para el MVP. Todos los criterios siguientes deben aprobarse; no se marcan como pruebas ejecutadas durante documentación.

| ID | Prioridad | Historia | Criterios de aceptación verificables |
|---|---|---|---|
| US-001 | P0 | Como visitante quiero explorar el catálogo para descubrir cursos gratuitos. | Sin sesión se muestran solo publicados; página inicial de 20; con 21 cursos hay segunda página; texto y categoría combinan filtros; sin coincidencias hay mensaje y acción para limpiar filtros; error permite reintentar. |
| US-002 | P0 | Como visitante quiero registrarme e ingresar para gestionar mi aprendizaje. | Registro válido crea cuenta de usuario; correo duplicado informa conflicto; credenciales válidas abren sesión; inválidas producen mensaje genérico; exceso de intentos informa espera; logout impide volver a consultar datos privados. |
| US-003 | P0 | Como usuario quiero recuperar mi contraseña para volver a acceder. | Solicitud muestra igual mensaje para correos existentes o desconocidos; llega enlace a cuenta existente; nueva contraseña válida permite ingresar; enlace vencido o reutilizado falla; sesiones anteriores quedan revocadas. |
| US-004 | P0 | Como usuario quiero aportar un curso para enriquecer el catálogo. | Exige todos los metadatos RF-004 y respuesta explícita sí/no sobre haberlo tomado; enviar crea en_revision; no aparece públicamente; errores conservan datos; URL duplicada informa conflicto; el autor consulta el estado. |
| US-005 | P0 | Como administrador quiero aprobar o rechazar aportes para asegurar su calidad. | Solo administrador accede; revisa enlace activo, contenido gratuito y metadatos; aprobar publica; rechazar sin motivo falla; autor ve motivo; revisión simultánea no sobrescribe una decisión previa. |
| US-006 | P0 | Como autor quiero corregir un rechazo para pedir otra revisión. | Solo autor puede editar su rechazado; guardar conserva estado y motivo anterior; reenviar pasa a en_revision; historial conserva rechazo y reenvío; no permite editar publicado ni en_revision. |
| US-007 | P0 | Como usuario quiero administrar mi roadmap para organizar mi aprendizaje. | Solo cursos publicados; no permite repetidos; nuevo ítem queda pendiente al final; se puede mover antes/después de un vecino incluso entre páginas; al cambiar estado u orden persiste; quitar exige confirmación; otro usuario no puede verlo ni modificarlo. |
| US-008 | P0 | Como visitante quiero consultar un curso para evaluar sus condiciones. | Detalle incluye docentes, edición, URL externa y condiciones de certificado/aval; no publicado devuelve no encontrado; enlace externo abre con advertencia contextual de proveedor externo. |

## 5. Requisitos funcionales

| ID | Requisito | Historia |
|---|---|---|
| RF-001 | Registro, login y logout; roles usuario y administrador; registro nunca permite elegir administrador. | US-002 |
| RF-002 | Recuperación completa por enlace temporal de un solo uso. | US-003 |
| RF-003 | Catálogo público y detalle; búsqueda por título/tema y filtro de categoría, sin distinguir mayúsculas; listados paginados. | US-001, US-008 |
| RF-004 | Aporte: URL, categoría de lista, tema, título, versión, año de edición, uno o más docentes; respuestas explícitas a cuestionarios, examen, certificado, aval y si lo tomó; organización si hay aval; costo del certificado si lo ofrece. | US-004 |
| RF-005 | Cola de revisión exclusiva del administrador; aprobar/rechazar con registro de actor, fecha y motivo. | US-005 |
| RF-006 | Mis aportes paginados, detalle propio, corrección y reenvío de rechazados. | US-004, US-006 |
| RF-007 | Un roadmap privado con alta, baja, orden manual y estado por curso; listado paginado. | US-007 |
| RF-008 | Categorías seleccionables desde lista administrada mediante datos iniciales; no texto libre. Lista inicial pendiente Q-02. | US-001, US-004 |

## 6. Requisitos no funcionales

| ID | Requisito | Origen / comprobación |
|---|---|---|
| RNF-001 | Todos los listados y tablas paginados. | SPEC; incluye categorías, aportes, cola e historial. |
| RNF-002 | Mobile-first, mejora progresiva y menú sándwich. | SPEC; revisión base sin media queries y mejoras min-width. |
| RNF-003 | Hash de contraseñas bcrypt como mínimo o algoritmo más fuerte justificado. | SPEC; TRD adopta Argon2id. |
| RNF-004 | Rate limiting al menos en login. | SPEC; umbrales y prueba 429 en TRD. |
| RNF-005 | Estética Google/Material con valores concretos. | SPEC; tokens y componentes UX-UI. |
| RNF-006 | Arquitectura expresada por dominios. | SPEC; TRD §3.2. |
| RNF-007 | Navegación por teclado, foco visible, mensajes asociados y contraste AA. | Requisito de plantilla UX-UI. |
| RNF-008 | API no expone roadmaps ajenos, hashes ni tokens secretos. | Diseño técnico; pruebas de permisos. |

## 7. Reglas de negocio

| ID | Regla | Fuente |
|---|---|---|
| BR-001 | Publicación solo tras aprobación del administrador designado. | SPEC + D-01 |
| BR-002 | Declarar si tomó el curso es obligatorio; sí y no son respuestas válidas, no se infiere por checkbox ausente. | SPEC |
| BR-003 | Un único roadmap privado por usuario; orden manual; estados pendiente/en_curso/completado. | D-03 |
| BR-004 | Rechazo con motivo obligatorio; autor puede corregir y reenviar. | D-04 |
| BR-005 | Contenido completo gratuito, certificado opcional pago permitido y pruebas temporales excluidas. | D-05 |
| BR-006 | Catálogo y detalle publicados accesibles sin cuenta. | D-02 |
| BR-007 | Un curso por URL normalizada, incluso mientras está en revisión; versiones en la misma URL se resuelven mediante corrección antes de publicar, no creando duplicados. | Decisión de diseño para evitar duplicados; no requisito docente. |
| BR-008 | Solo el autor modifica su aporte rechazado; ni administrador ni autor editan un curso publicado en el MVP diseñado. | Diseño mínimo de permisos; extensiones sujetas a Q-03. |
| BR-009 | Un curso solo puede estar una vez en el roadmap de cada usuario; retirar no elimina el curso comunitario. | Diseño derivado de D-03. |
| BR-010 | Administrador mantiene capacidades de usuario; no aprueba su propio aporte (debe revisarlo otro administrador). | Diseño de integridad; requiere segunda cuenta administradora si el administrador aporta. |

## 8. Fuera de alcance (out of scope)

Confirmado: cursos de contenido pago y pruebas gratuitas temporales. No se aloja ni se reproduce contenido educativo; se enlaza al proveedor.

Propuesto para Q-03: pagos dentro de la app, comentarios, reseñas, chat, recomendaciones automáticas, certificados propios y correos distintos de recuperación. No están comprometidos en el MVP. Tampoco se planifica app nativa ni múltiples roadmaps: la solución acordada es web con un roadmap por usuario.

## 9. Supuestos y decisiones sobre la SPEC de clase

[Registro de origen en SPEC §8](SPEC.md#8-vacíos-de-la-spec-de-clase-y-decisiones-tomadas). D-01–D-05 son respuestas del autor. Personas, búsqueda, unicidad de URL y permisos mínimos son decisiones de diseño documentadas. La validación de enlace es manual por administrador: no se promete crawler ni validación automática de gratuidad. Las métricas de §2 son objetivos de verificación, no datos observados.

## 10. Preguntas abiertas

| ID | Pregunta | Impacto | Estado / recomendación |
|---|---|---|---|
| Q-01 | Nombre propio del producto | Identidad y títulos | Consultada; recomendación «Ruta Abierta»; título descriptivo temporal. |
| Q-02 | Categorías iniciales | Seed y opciones del formulario | Consultada; propuesta amplia: Tecnología, Negocios, Ciencias, Idiomas, Arte y Humanidades, Oficios. No aprobada aún. |
| Q-03 | Exclusiones adicionales del MVP | Alcance y estimaciones | Consultada; excluir extensiones enumeradas en §8. Sin confirmación no se incorporan ni se declaran rechazadas definitivamente. |

Versiones y aprobación docente son pendientes técnicos/de entrega, no decisiones de producto; seguimiento en auditoría.
