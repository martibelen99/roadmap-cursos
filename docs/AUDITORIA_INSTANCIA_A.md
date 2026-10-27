# Auditoría de coherencia — SaberColectivo — Instancia A

Fecha: 27/10/2026. **Resultado: siete documentos redactados y coherentes con las decisiones confirmadas; entrega preparada para revisión con V-01 pendiente de instalación en B.** No hay implementación, pruebas de aplicación ni aprobación docente registradas. No quedan preguntas de producto abiertas para el alcance acordado; esta revisión no certifica un sistema ejecutable.

## 1. Alcance de la revisión

Fuentes: SPEC original en commit 2128507, siete plantillas en docs/plantillas, ejemplo UX-UI y README de referencia, decisiones confirmadas por el autor en la conversación. El historial original se conserva. Rama de trabajo: docs/instancia-a. El usuario autorizó su publicación y fusión con main después de esta auditoría; se conservará el historial sin reescribirlo.

Se inspeccionaron secciones numeradas contra cada plantilla, enlaces locales, fences Markdown, identificadores, correspondencia de API, alcance, estados, seguridad y plan. Se calcularon ratios de contraste mediante luminancia relativa sRGB. Se instaló únicamente pnpm 12.15.0 global en el prefijo NVM, fuera del repositorio, con autorización expresa; se ejecutó pnpm --version. No se instaló frontend/backend, no se ejecutaron contenedores ni se descargaron imágenes o generaron archivos de aplicación.

## 2. Cumplimiento por plantilla

| Documento | Requisitos revisados | Resultado |
|---|---|---|
| SPEC | 9 secciones; problema 2 párrafos; visión; alcance; 5 reglas clave; stack; restricciones; vacíos y origen; historia | Estructura completa. Host inventariado y objetivos exactos; instalación de dependencias del proyecto pendiente V-01 en B. |
| PRD | 10 secciones; 3 personas (mínimo 2); 9 historias P0 (mínimo 6 historias/4 P0), criterios verificables; RF/RNF/BR; exclusiones y preguntas | Cumple mínimos estructurales y trazabilidad; reglas D-09–D-11 incorporadas con criterios verificables. |
| TRD | 6 secciones; 11 decisiones (mínimo 3); capas; árbol; servicios; Git; errores; variables; logging; riesgos | Diseño técnico completo para base acordada; versiones reales del proyecto pendientes V-01. |
| UX-UI | 7 secciones; 5 principios; tokens y bloque CSS documental; 3 capas responsive; 8 componentes; estados; layout; accesibilidad; assets | Revisión documental realizada. Verificación de UI solo podrá hacerse en B. |
| APPFlow | 5 secciones; sitemap; 17 rutas/fichas incluyendo fallback; 6 flujos (mínimo 3); estados globales | Incluye gestión administrativa y advertencia de retirado; contratos conectados. |
| BackendSchema | 4 secciones; ER y 8 tablas; constraints/índices; ciclo; auth/catálogo/aportes/roadmap/categorías; decisiones | D-09–D-11 cerradas: duplicados, permisos y ciclo de retiro/republicación definidos. |
| Implementation Plan | 6 secciones; MVP; 6 módulos (mínimo 4); 22 tareas con hecho/estimación; registro; pruebas; riesgos | 81 h base + 25% reserva (101.25 h total); tareas no ejecutadas, gestión administrativa incluida. |
| README y .gitignore | Índice, estado real, revisión, stack, Git; secretos/salidas locales ignorados | Documentados; sin instrucciones de instalación ficticias ni declaración de entrega realizada. |

Las checklists de accesibilidad y tareas de B están deliberadamente sin marcar: describen verificaciones futuras, no son campos sin completar de una plantilla.

## 3. Matriz de trazabilidad de requisitos originales

| Fuente / requisito | Producto | Diseño / contrato | Verificación planificada |
|---|---|---|---|
| Roadmap personal | BR-003/009, US-007 | roadmap_items, APPFlow roadmap, mover entre páginas | M4 y E2E flujo 2 |
| Aporte: URL, categoría, tema, título, versión, año, docentes | RF-004, US-004 | CursoInput, cursos, formulario aporte | M3.1 y M3.4 |
| Cuestionarios/examen/certificado/aval | RF-004 | Booleanos explícitos y constraints condicionales; badges | M3.1 y M2.2 |
| Declaración si lo tomó | BR-002 | lo_tomo obligatorio; privado en aporte, independiente del avance | M3.1 y reglas unitarias |
| URL activa y validación antes de catálogo | BR-001/005 | TD-006; revisión manual; estado publicado | M3.2, flujo 3 |
| Catálogo comunitario e inclusión personal | US-001/008/007 | GET cursos, POST roadmap | M2 y M4 |
| Registro y login | US-002, RF-001 | auth, sesiones, usuarios | M1.1–1.2 |
| Recuperación solicitud → enlace/token → nueva contraseña | US-003 | SMTP, reset_tokens, dos pantallas, consumo transaccional | M1.4 y E2E flujo 5 |
| Rate limiting al menos login | RNF-004 | TD-008, rate_limit_buckets, 429/Retry-After | M1.3 con concurrencia/reinicio |
| bcrypt mínimo o más fuerte justificado | RNF-003 | TD-004 Argon2id con parámetros explícitos | M1.1 y pruebas de hash |
| React/Vite/pnpm, FastAPI/uv, PostgreSQL 18.6, Docker/Compose | SPEC §6 | TRD §1/3 | V-01 y M0; herramientas del host confirmadas, dependencias del proyecto pendientes B |
| Mobile-first/mejora progresiva | RNF-002 | UX-UI §3, base sin media queries | M0.4, M5.2 |
| Paginación de todos los listados/tablas | RNF-001 | TD-002; cursos, categorías, aportes, cola, eventos, roadmap y gestión administrativa | API páginas y M5.2 |
| Menú sándwich mobile | RNF-002 | UX-UI §4.5; ARIA/foco/cierre | M0.4 y revisión teclado |
| Screaming Architecture | RNF-006 | TRD §3.2: identidad/catalogo/aportes/roadmap | M0.2 revisión árbol |
| Inspiración Google con valores concretos | RNF-005 | UX-UI tokens/estados y contraste calculado | M0.4, M5.2 |

## 4. Coherencia transversal y hallazgos resueltos

- Nombre SaberColectivo e identidad «el conocimiento crece cuando se comparte» propagados a documentos y README.
- Diez categorías exactas en PRD y BackendSchema, tabla ampliable, una principal por curso, sin subcategorías. Selector y endpoint paginados incluso con diez registros iniciales.
- Administración designada, sin añadir rol moderador distinto; hereda capacidades de usuario, con aprobación propia permitida en MVP, en_revision obligatorio, verificación backend y actor/fecha trazables. Revisión independiente futura excluida del MVP.
- Estados de curso en_revision/rechazado/publicado/retirado iguales en producto, esquema, flujos y plan. No se copió borrador del ejemplo como requisito. Rechazo preserva motivo/eventos; corrección y reenvío son pasos distintos. Gestión administrativa conserva ID/relaciones, permite retiro motivado y republicación tras verificar, sin volver a en_revision ni guardar historial completo por campo.
- Roadmap único privado; alta pendiente al final; tres estados; orden con transacciones y movimientos entre páginas, sin exigir cargar todos los IDs.
- 27 pares método/path definidos en BackendSchema y 27 referencias explícitas distintas en APPFlow. Ninguna referencia de APPFlow sin contrato. /health es operación de infraestructura definida en TRD, no pantalla del producto.
- Recuperación documentada desde solicitud hasta nuevo login y revocación de sesiones; no se limita a una pantalla de solicitud. Se distingue envío SMTP de visualización del token.
- Azul #1a73e8 contra blanco: 4.51:1; sobre fondo #f8f9fa: 4.27:1. Se corrigió la regla: texto azul primario solo sobre blanco, variante #174ea6 sobre gris. Bordes #747775 superan 3:1 en ambas superficies. Cálculo local, no comprobación manual ficticia en WebAIM.
- Ejemplos JSON no son datos reales ni evidencia de instalación; fechas del historial de la SPEC no inventan fecha de creación del primer commit.
- Exclusiones confirmadas no generan tareas del MVP. Gestión de cursos publicados confirmada en D-11 tiene historia US-009, APIs, pantallas y tareas; sin solicitudes de edición ni historial por campo.

- Duplicados: URL normalizada única en todos los estados, índice atómico y normalización común en alta/corrección; tracking de lista cerrada, parámetros identificadores conservados y fragmentos de rutas protegidos. Duplicado publicado ofrece ficha pública; en revisión informa sin datos privados; rechazado/retirado no revela ID/estado.
- Retirados no aparecen en listado/detalle públicos ni admiten nuevas altas; roadmap existente consulta relación sin filtro de publicado y muestra advertencia con metadatos actuales. Se mantienen estado/posición y se serializa alta concurrente con retiro.
- Eventos mínimos registran acciones/responsables/fechas/motivos y transiciones, sin snapshots completos de campos. Campos de última revisión, actualización y retiro en curso respaldan la trazabilidad, incluso si autor y administrador son la misma cuenta.
- Inventario confirmado: Git 2.43.0, Node 22.23.2, pnpm 12.15.0 global, Python 3.12.3, uv 0.12.21, Docker cliente/servidor 29.1.3, Compose 2.40.3+ds1-0ubuntu1~24.04.1; Ubuntu 24.04.2 LTS. pnpm está en el prefijo NVM de Node, sin dependencias del proyecto.
- Objetivos exactos sin instalar: React 19.3.1, Vite 8.3.5, FastAPI 0.162.0 y PostgreSQL 18.6. Node satisface engines publicados de pnpm/Vite; registros accesibles no equivalen a build o instalación probados. No se cambian objetivos silenciosamente en B.

## 5. Pendientes antes de cerrar la entrega

| ID | Pendiente | Quién / acción | Impacto |
|---|---|---|---|
| V-01 | Versiones exactas instaladas del proyecto y compatibilidad integral | Instalar y verificar React/Vite/FastAPI/PostgreSQL en B después de aprobación; fijar locks/digests y resolver librerías auxiliares; cotejar stable de Docker/Compose | Objetivos/host documentados; requisito de instalación de la aplicación aún pendiente, sin atribuir exención al docente |
| A-01 | Revisión docente | Usuario confirma aprobación cuando ocurra | B permanece sin autorización |
| G-01 | Publicación autorizada por el usuario tras auditoría | Subir rama, abrir PR y fusionar preservando commits; comprobar documentos en main | Autorización registrada; ejecución y verificación remota se informan al terminar |

Q-01–Q-06 están resueltas: nombre, categorías, exclusiones, duplicados, aprobación propia y gestión administrativa. No se vuelven a preguntar. El usuario autorizó instalar pnpm global y reservar instalaciones del proyecto para B; la publicación documental y el merge están autorizados tras auditoría, pero no hay autorización para iniciar B.

## 6. Preparación de entrega y límites de evidencia

Decisiones D-09–D-11 propagadas y auditoría focalizada repetida. Documentos listos para revisión con V-01 identificado; el usuario deberá presentar este pendiente al docente si se exige acreditar instalaciones antes de aprobar A. No se afirma cumplimiento total ni exención concedida. V-01 se resuelve con evidencia real durante B autorizada, no con números de ejemplo. Revisar diff contra main y commits convencionales y presentar resultado al usuario antes de cualquier publicación o merge; la autorización de publicación y merge ya está registrada en esta etapa. V-01 es una observación conocida de la entrega para solicitar aprobación, no una instalación realizada ni una exención concedida por el profesor.

Comprobaciones documentales realizadas: secuencia de secciones principales idéntica a las siete plantillas; enlaces locales y fences; IDs únicos (9 US, 10 RF, 8 RNF, 13 BR); endpoints y referencias; git diff --check. Las herramientas de revisión documental no comprueban renderizado Mermaid en GitHub ni comportamiento de una aplicación aún inexistente. No se afirma que la Instancia A esté aprobada ni que se haya enviado un correo al profesor.

## 7. Auditoría final previa a publicación

| Control | Resultado |
|---|---|
| Siete documentos | Secciones obligatorias y coherencia revisadas; sin campos de plantilla pendientes. V-01 explícito corresponde a instalación futura, no a un campo olvidado. |
| Requisitos y contratos | 9 historias P0, 10 RF, 8 RNF y 13 BR; 27 contratos coinciden con APPFlow; 17 fichas/rutas y 22 tareas (81 h base). |
| README | SaberColectivo, descripción e índice con enlaces relativos válidos a los siete documentos y auditoría. |
| Historial | Primer commit raíz 2128507e94984070c4e28eed5ee115507e971050 conservado; sin amend/rebase/force push. Mensajes de commits auditados con Conventional Commits. |
| Archivos | Solo Markdown y .gitignore; sin código de aplicación, dependencias, lockfiles, entornos, Dockerfiles ni Compose. |
| Credenciales y datos personales | No se detectaron claves privadas, tokens GitHub/AWS ni credenciales reales en archivos versionados. Email documental es no-reply@example.test, ficticio. Se generalizó ruta local del TRD. El historial conserva nombre/correo de autor Git ya utilizados y publicados desde el primer commit; no se reescriben ni se confunden con credenciales. |
| Plantillas | Se conservan como fuente de auditoría, claramente diferenciadas de entregables; la consigna no exige que sean exclusivamente locales. Sus campos de ejemplo no se contabilizan como faltantes en documentos finales. Quitarlas hoy tampoco las eliminaría del historial que se preserva. |
| Observaciones | V-01: instalar/verificar aplicación en B tras aprobación; cotejo de stable y librerías auxiliares en ese momento. A-01: aprobación docente pendiente. No se declara cumplimiento de instalación ni aprobación por el solo hecho de publicar. |
| Publicación | Autorizada por usuario; flujo PR y merge preservando historial en TRD §4. URL, PR, hash final y comprobación remota se informarán luego de ejecutarlos. |

Huella de la SPEC del primer commit: blob Git `ffb2ccc2a6d3b863e1390013116702ad11887e62`; SHA-256 de sus bytes `ccb9f1fc25317bbfbc9b4c3c2b27b9c8fcfb13cd267f6a631585e07e3528bc11`. La SPEC refinada actual es una evolución posterior, no reemplaza ni altera el objeto original.
