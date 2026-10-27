# Auditoría de coherencia — SaberColectivo — Instancia A

Fecha: 27/10/2026. **Resultado: siete documentos redactados; entrega todavía no cerrada.** No hay implementación, pruebas de aplicación ni aprobación docente registradas. Esta revisión no sustituye las decisiones del autor ni certifica un sistema ejecutable.

## 1. Alcance de la revisión

Fuentes: SPEC original en commit 2128507, siete plantillas en docs/plantillas, ejemplo UX-UI y README de referencia, decisiones confirmadas por el autor en la conversación. El historial original se conserva. Rama de trabajo: docs/instancia-a; sin merge, push ni reescritura de historial.

Se inspeccionaron secciones numeradas contra cada plantilla, enlaces locales, fences Markdown, identificadores, correspondencia de API, alcance, estados, seguridad y plan. Se calcularon ratios de contraste mediante luminancia relativa sRGB. No se instaló frontend/backend, no se ejecutaron contenedores ni se generaron archivos de aplicación.

## 2. Cumplimiento por plantilla

| Documento | Requisitos revisados | Resultado |
|---|---|---|
| SPEC | 9 secciones; problema 2 párrafos; visión; alcance; 5 reglas clave; stack; restricciones; vacíos y origen; historia | Estructura completa. Versiones instaladas incompletas V-01; decisiones Q-04–06 por cerrar. |
| PRD | 10 secciones; 3 personas (mínimo 2); 8 historias P0 (mínimo 6 historias/4 P0), criterios verificables; RF/RNF/BR; exclusiones y preguntas | Cumple mínimos estructurales y trazabilidad; reglas operativas pendientes explícitas. |
| TRD | 6 secciones; 9 decisiones (mínimo 3); capas; árbol; servicios; Git; errores; variables; logging; riesgos | Diseño técnico completo para base acordada; versiones reales del proyecto pendientes V-01. |
| UX-UI | 7 secciones; 5 principios; tokens y bloque CSS documental; 3 capas responsive; 8 componentes; estados; layout; accesibilidad; assets | Revisión documental realizada. Verificación de UI solo podrá hacerse en B. |
| APPFlow | 5 secciones; sitemap; 15 rutas/fichas incluyendo fallback; 5 flujos (mínimo 3); estados globales | Contratos base conectados; Q-06 puede agregar ruta y flujo. |
| BackendSchema | 4 secciones; ER y 8 tablas; constraints/índices; ciclo; auth/catálogo/aportes/roadmap/categorías; decisiones | Contratos y transacciones definidos; Q-04–06 abiertos. |
| Implementation Plan | 6 secciones; MVP; 6 módulos (mínimo 4); 20 tareas con hecho/estimación; registro; pruebas; riesgos | 73 h base + 25% reserva (91.25 h total); tareas no ejecutadas. Cambiar si Q-06 amplía alcance. |
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
| React/Vite/pnpm, FastAPI/uv, PostgreSQL 18.6, Docker/Compose | SPEC §6 | TRD §1/3 | V-01 y M0; instalado no demostrado |
| Mobile-first/mejora progresiva | RNF-002 | UX-UI §3, base sin media queries | M0.4, M5.2 |
| Paginación de todos los listados/tablas | RNF-001 | TD-002; cursos, categorías, aportes, cola, eventos, roadmap | API páginas y M5.2 |
| Menú sándwich mobile | RNF-002 | UX-UI §4.5; ARIA/foco/cierre | M0.4 y revisión teclado |
| Screaming Architecture | RNF-006 | TRD §3.2: identidad/catalogo/aportes/roadmap | M0.2 revisión árbol |
| Inspiración Google con valores concretos | RNF-005 | UX-UI tokens/estados y contraste calculado | M0.4, M5.2 |

## 4. Coherencia transversal y hallazgos resueltos

- Nombre SaberColectivo e identidad «el conocimiento crece cuando se comparte» propagados a documentos y README.
- Diez categorías exactas en PRD y BackendSchema, tabla ampliable, una principal por curso, sin subcategorías. Selector y endpoint paginados incluso con diez registros iniciales.
- Administración designada, sin añadir rol moderador distinto; hereda capacidades de usuario, con política de revisión propia aún consultada.
- Estados de curso base en_revision/rechazado/publicado iguales en producto, esquema, flujos y plan. No se copió borrador del ejemplo como requisito. Rechazo preserva motivo/eventos; corrección y reenvío son pasos distintos.
- Roadmap único privado; alta pendiente al final; tres estados; orden con transacciones y movimientos entre páginas, sin exigir cargar todos los IDs.
- 22 pares método/path definidos en BackendSchema y 22 referencias explícitas distintas en APPFlow. Ninguna referencia de APPFlow sin contrato. /health es operación de infraestructura definida en TRD, no pantalla del producto.
- Recuperación documentada desde solicitud hasta nuevo login y revocación de sesiones; no se limita a una pantalla de solicitud. Se distingue envío SMTP de visualización del token.
- Azul #1a73e8 contra blanco: 4.51:1; sobre fondo #f8f9fa: 4.27:1. Se corrigió la regla: texto azul primario solo sobre blanco, variante #174ea6 sobre gris. Bordes #747775 superan 3:1 en ambas superficies. Cálculo local, no comprobación manual ficticia en WebAIM.
- Ejemplos JSON no son datos reales ni evidencia de instalación; fechas del historial de la SPEC no inventan fecha de creación del primer commit.
- Exclusiones confirmadas no generan tareas del MVP. Gestión de cursos publicados espera Q-06 y no se confunde con comentarios, chat u otras extensiones excluidas.

## 5. Pendientes antes de cerrar la entrega

| ID | Pendiente | Quién / acción | Impacto |
|---|---|---|---|
| Q-04 | Un único registro por URL o permitir varias ediciones | Autor: responder pregunta agrupada; recomendado URL única normalizada | Unicidad, errores y reglas de aporte |
| Q-05 | Administrador puede aprobar su aporte | Autor: recomendado permitir en MVP individual con auditoría | Permisos y número mínimo de administradores |
| Q-06 | Corregir/retirar publicados | Autor: recomendada gestión mínima administrativa conservando roadmap | Estado adicional, rutas, endpoints y estimación si se acepta |
| V-01 | Versiones exactas realmente resueltas e instaladas | Resolver con docente/autor cuándo verificar sin inicializar código/dependencias en A | Requisito obligatorio de SPEC/TRD todavía no cumplido |
| A-01 | Revisión docente | Usuario confirma aprobación cuando ocurra | B permanece sin autorización |
| G-01 | Publicación de la entrega | Usuario autoriza push/merge cuando revise resultado concreto | No se publica ni fusiona por autorización de edición local |

Q-01, Q-02 y Q-03 están resueltas: nombre, categorías y exclusiones adicionales confirmados. No se vuelven a preguntar.

## 6. Preparación de entrega y límites de evidencia

Después de responder Q-04–06: ajustar SPEC/PRD/contratos/pantallas/plan, repetir auditoría focalizada y actualizar este resultado. V-01 requiere evidencia real; no se soluciona rellenando números de ejemplo. Luego revisar diff contra main y commits convencionales, confirmar ausencia de código/secretos y presentar resultado al usuario antes de pedir autorización final para publicación o merge.

Comprobaciones documentales realizadas: secuencia de secciones principales idéntica a las siete plantillas; enlaces locales y fences; IDs únicos (8 US, 8 RF, 8 RNF, 10 BR); endpoints y referencias; git diff --check. Las herramientas de revisión documental no comprueban renderizado Mermaid en GitHub ni comportamiento de una aplicación aún inexistente. No se afirma que la Instancia A esté aprobada o enviada.
