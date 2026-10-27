# PRD — SaberColectivo

Estado: requisitos redactados con identidad, categorías y exclusiones confirmadas; decisiones operativas confirmadas y registradas en §10. Fecha: 27/10/2026.

## 1. Resumen ejecutivo

Una comunidad aporta cursos cuyo contenido completo es gratuito. Un administrador revisa cada aporte antes de publicarlo en un catálogo de acceso público. Los usuarios registrados organizan cursos publicados en un único roadmap privado, con orden manual y estados de aprendizaje. Se admite certificado opcional pago y se excluyen pruebas temporales.

## 2. Objetivos y métricas de éxito

Son criterios de aceptación del MVP, no estadísticas de uso observadas ni metas comerciales inventadas.

| Objetivo | Cómo sabemos que lo logramos |
|---|---|
| Descubrimiento confiable | El 100% de los cursos visibles en el conjunto de prueba tiene aprobación registrada; ningún aporte pendiente/rechazado ni curso retirado es público. |
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
| US-004 | P0 | Como usuario quiero aportar un curso para enriquecer el catálogo. | Exige todos los metadatos RF-004 y respuesta explícita sí/no sobre haberlo tomado; enviar crea en_revision; no aparece públicamente; errores conservan datos; variantes de URL que solo difieren en espacios extremos, fragmento o tracking conocido no crean otro registro; si está publicado, conflicto con enlace a ficha; si en revisión, aviso sin datos privados; URL con identificador distinto se admite; el autor consulta el estado. |
| US-005 | P0 | Como administrador quiero aprobar o rechazar aportes para asegurar su calidad. | Solo administrador accede; revisa enlace activo, contenido gratuito y metadatos; aprobar publica; rechazar sin motivo falla; autor ve motivo; revisión simultánea no sobrescribe una decisión previa; administrador puede aprobar su propio aporte pero siempre pasa primero por en_revision; backend registra autor, revisor y fecha/hora y deniega validar a usuario común. |
| US-006 | P0 | Como autor quiero corregir un rechazo para pedir otra revisión. | Solo autor puede editar su rechazado; guardar conserva estado y motivo anterior; reenviar pasa a en_revision; historial conserva rechazo y reenvío; el autor común no puede editar publicado ni en_revision; la gestión administrativa es un flujo separado. |
| US-007 | P0 | Como usuario quiero administrar mi roadmap para organizar mi aprendizaje. | Solo se agregan cursos publicados; retirados ya incorporados siguen visibles con advertencia, orden y estado conservados; no permite repetidos; nuevo ítem queda pendiente al final; se puede mover antes/después de un vecino incluso entre páginas; al cambiar estado u orden persiste; quitar exige confirmación; otro usuario no puede verlo ni modificarlo. |
| US-008 | P0 | Como visitante quiero consultar un curso para evaluar sus condiciones. | Detalle incluye docentes, edición, URL externa y condiciones de certificado/aval; no publicado devuelve no encontrado; enlace externo abre con advertencia contextual de proveedor externo. |
| US-009 | P0 | Como administrador quiero corregir, retirar y republicar cursos para mantener confiable el catálogo. | Corregir metadatos de publicado/retirado conserva ID y roadmaps, registra responsable/fecha y no requiere revisión nueva; cambio de URL respeta unicidad; retiro exige motivo y oculta catálogo/detalle público sin borrar items; usuarios existentes ven advertencia y no se permite alta nueva; republicar exige verificación del problema solucionado; usuario común recibe 403; no se guarda historial por campo. |

## 5. Requisitos funcionales

| ID | Requisito | Historia |
|---|---|---|
| RF-001 | Registro, login y logout; roles usuario y administrador; registro nunca permite elegir administrador. | US-002 |
| RF-002 | Recuperación completa por enlace temporal de un solo uso. | US-003 |
| RF-003 | Catálogo público y detalle; búsqueda por título/tema y filtro de categoría, sin distinguir mayúsculas; listados paginados. | US-001, US-008 |
| RF-004 | Aporte: URL, categoría de lista, tema, título, versión, año de edición, uno o más docentes; respuestas explícitas a cuestionarios, examen, certificado, aval y si lo tomó; organización si hay aval; indicación de si el certificado opcional es pago, sin solicitar monto. | US-004 |
| RF-005 | Cola de revisión exclusiva del administrador; aprobar/rechazar con registro de actor, fecha y motivo. | US-005 |
| RF-006 | Mis aportes paginados, detalle propio, corrección y reenvío de rechazados. | US-004, US-006 |
| RF-007 | Un roadmap privado con alta, baja, orden manual y estado por curso; listado paginado. | US-007 |
| RF-008 | Categorías seleccionables desde lista administrada mediante datos iniciales; no texto libre. Diez categorías iniciales confirmadas, ampliables sin cambio arquitectónico y sin subcategorías en MVP; lista en §9. | US-001, US-004 |
| RF-009 | Gestión administrativa paginada de publicados/retirados; corrección de metadatos, retiro motivado y republicación tras verificar solución; preservar ID y referencias. | US-009 |
| RF-010 | Detectar duplicados por URL normalizada y diferenciar respuesta pública/revisión/oculta sin datos privados; conservar el registro ante nuevas ediciones con la misma URL. | US-004, US-009 |

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
| BR-007 | URL normalizada única en todo estado; duplicado publicado ofrece ficha, en revisión avisa sin datos privados. Normalizar espacios extremos, fragmento y tracking conocido sin borrar parámetros identificadores. Ediciones en URL distintas son cursos independientes. | D-09 (Q-04 resuelta) |
| BR-008 | Autor corrige solo su rechazado; administrador corrige metadatos de publicado/retirado sin nueva revisión. Actualizar edición, año o URL no cambia ID ni elimina relaciones. | D-04, D-09 y D-11 |
| BR-009 | Un curso solo puede estar una vez en el roadmap de cada usuario; retirar no elimina el curso comunitario. | Diseño derivado de D-03. |
| BR-010 | Administrador puede aprobar su propio aporte en MVP; todo aporte pasa por en_revision, backend verifica rol y registra autor, revisor, fecha/hora y motivo al rechazar. Usuario común nunca valida. | D-10 (Q-05 resuelta) |
| BR-011 | Retirar requiere administrador y motivo; no borra curso ni items; oculto públicamente, visible con advertencia en roadmaps existentes y prohibido para nuevas incorporaciones. | D-11 |
| BR-012 | Administrador puede republicar un retirado tras verificar solución; conserva ID y relaciones, registra actor y fecha. | D-11 |
| BR-013 | Modificaciones administrativas registran responsable y fecha, sin historial completo de valores por campo; motivos y decisiones permanecen trazables. | D-11 |

## 8. Fuera de alcance (out of scope)

Confirmado: cursos de contenido pago y pruebas gratuitas temporales. No se aloja ni se reproduce contenido educativo; se enlaza al proveedor.

Confirmado: pagos dentro de la app, comentarios, reseñas, valoraciones, chat, recomendaciones automáticas, certificados propios y correos distintos de recuperación. Los certificados emitidos por proveedores se identifican; estas extensiones no forman parte del plan del MVP. Solicitudes de edición por usuarios, historial de versiones por campo y revisión independiente obligatoria quedan fuera del MVP; revisión independiente podrá incorporarse con un equipo de moderación futuro. Tampoco se planifica app nativa ni múltiples roadmaps: la solución acordada es web con un roadmap por usuario.

## 9. Supuestos y decisiones sobre la SPEC de clase

[Registro de origen en SPEC §8](SPEC.md#8-vacíos-de-la-spec-de-clase-y-decisiones-tomadas). D-01–D-11 son respuestas del autor. Personas y detalles técnicos de búsqueda/normalización son decisiones de diseño; la unicidad y las políticas administrativas están confirmadas por el autor. La validación de enlace es manual por administrador: no se promete crawler ni validación automática de gratuidad. Las métricas de §2 son objetivos de verificación, no datos observados.

Categorías iniciales confirmadas: Tecnología y Programación; Negocios y Administración; Ciencias y Matemáticas; Idiomas; Arte y Diseño; Humanidades y Ciencias Sociales; Oficios y Formación Técnica; Comunicación y Marketing; Desarrollo Personal y Profesional; Salud y Bienestar. Cada curso tiene una categoría principal. Las categorías son datos ampliables, sin subcategorías en MVP.

## 10. Preguntas abiertas

| ID | Pregunta | Impacto | Estado / recomendación |
|---|---|---|---|
| Q-01 | Nombre propio | Identidad | Resuelta: SaberColectivo, conocimiento que crece al compartirlo. |
| Q-02 | Categorías iniciales | Seed y formulario | Resuelta: diez categorías de §9, una principal por curso, sin subcategorías. |
| Q-03 | Exclusiones | Alcance | Resuelta: exclusiones de §8 confirmadas. |
| Q-04 | Duplicados y ediciones con misma URL | Unicidad y edición | Resuelta D-09: URL normalizada única; conservar ID y roadmaps; respuestas por visibilidad. |
| Q-05 | Administrador aprueba aporte propio | Permisos y operación | Resuelta D-10: permitido en MVP con autorización backend y trazabilidad; independencia futura fuera de MVP. |
| Q-06 | Corrección/retiro de publicados | Ciclo de vida, API y plan | Resuelta D-11: corrección, retiro motivado y republicación por administrador; conservar relaciones. |

No quedan decisiones de producto abiertas para el alcance actual. Versiones y aprobación docente son pendientes técnicos/de entrega, no decisiones de producto; seguimiento en auditoría.
