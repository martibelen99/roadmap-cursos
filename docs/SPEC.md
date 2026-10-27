# SPEC — SaberColectivo

Estado: Instancia A, versión refinada del 27/10/2026. Nombre definitivo confirmado por el autor; identidad: el conocimiento crece cuando se comparte. La SPEC original permanece intacta en el commit `2128507` (`git show 2128507:docs/SPEC.md`). No se reescribió el historial.

## 1. Identidad del producto

- **Nombre propio:** SaberColectivo. Su identidad se basa en que el conocimiento crece cuando se comparte.
- **Una línea:** una comunidad de estudiantes descubre cursos gratuitos validados y organiza su aprendizaje en un roadmap personal privado.

## 2. Problema

Los estudiantes encuentran cursos distribuidos entre múltiples proveedores. Comparar requisitos, edición, docentes y condiciones del certificado demanda revisar páginas con información heterogénea.

Guardar enlaces por separado tampoco permite distinguir qué estudiar después, qué está en curso y qué ya se completó. El producto reúne un catálogo construido por la comunidad, con una revisión previa a la publicación, y un recorrido personal ordenable. Estas necesidades son hipótesis de diseño derivadas de la consigna, no resultados de entrevistas.

## 3. Visión

En doce meses, aspiramos a que una comunidad pueda mantener un catálogo útil y revisado de cursos de distintos proveedores y gestionar recorridos personales sin depender de una plataforma educativa única. Esta visión no compromete funciones adicionales al MVP.

## 4. Alcance

### Dentro

Catálogo público con búsqueda, filtro de categoría y paginación; registro, login, logout y recuperación completa de contraseña; aportes con metadatos y declaración de toma; revisión por administrador designado; rechazo motivado y corrección/reenvío por autor; detección de duplicados por URL normalizada; corrección, retiro y republicación administrativa conservando roadmaps; un único roadmap privado por usuario con orden manual y estados pendiente/en curso/completado.

### Fuera (out of scope explícito)

No se admite contenido completo pago ni pruebas gratuitas temporales. Se excluyen pagos dentro de la app, reseñas, valoraciones, comentarios, chat, recomendaciones automáticas, certificados propios y correos ajenos a recuperación (confirmado por el autor). Los certificados del proveedor sí se identifican. La Instancia A no incluye código de aplicación ni inicialización de dependencias.

## 5. Reglas de producto clave

| ID | Regla | Origen |
|---|---|---|
| BR-001 | Solo los aportes aprobados por un administrador se publican. | SPEC original + decisión del autor D-01 |
| BR-002 | El autor declara explícitamente si tomó el curso. | SPEC original |
| BR-003 | Cada usuario tiene un único roadmap privado, ordenable y con estados de avance. | D-03 |
| BR-004 | Todo rechazo tiene motivo y admite corrección y reenvío del autor. | D-04 |
| BR-005 | Contenido completo gratuito; certificado opcional pago permitido; pruebas temporales excluidas. | D-05 |

Reglas completas: [PRD §7](PRD.md#7-reglas-de-negocio).

## 6. Stack registrado (versiones exactas con las que se va a desarrollar)

| Capa / herramienta | Instalado y verificado | Objetivo exacto | Pendiente |
|---|---|---|---|
| Frontend | Node 22.23.2; pnpm **12.15.0** global; npm auxiliar 10.9.8 | React **19.3.1**, Vite **8.3.5**, pnpm **12.15.0** | React/Vite no instalados; instalación y build en B tras aprobación |
| Backend | Python **3.12.3**; uv **0.12.21**, ejecución confirmada | FastAPI **0.162.0** | Instalar/resolver dependencias y verificar en B; 0.158.2 es referencia original, no versión instalada |
| Base de datos | Manifiesto remoto disponible para linux/amd64; sin instancia del proyecto | PostgreSQL **18.6** | Descargar y ejecutar/verificar en B, no durante A |
| Infra dev | Docker cliente/servidor **29.1.3**; Compose **2.40.3+ds1-0ubuntu1~24.04.1** | Host actual como base; PostgreSQL 18.6 obligatorio | Cotejar stable y tags/digests antes de B; no se ejecutó Compose ni se descargaron imágenes |
| Control de versiones / SO | Git **2.43.0**; Ubuntu **24.04.2 LTS**, x86_64 | Entorno local registrado | Sin cambio de SO ni herramientas adicionales |

Node 22.23.2 satisface engines de pnpm 12.15.0 (>=18) y Vite 8.3.5 (^20.19.0 o >=22.12.0), según metadatos consultados el 27/10/2026. Es compatibilidad declarada, no prueba de la aplicación. Inventario definitivo del host, comandos, fuentes y distinción instalado/objetivo/pendiente: [TRD §1](TRD.md#1-stack-y-versiones-exactas).

**V-01:** React, FastAPI y PostgreSQL —y Vite como dependencia frontend— se instalarán y verificarán durante la Instancia B, solo después de aprobación docente confirmada por el usuario. La preparación de A solo instaló pnpm global fuera del repositorio. El requisito de versiones exactas instaladas de la aplicación sigue pendiente de esa ejecución; no se rellena con versiones objetivo como si ya fueran instalaciones reales.

## 7. Restricciones técnicas heredadas de la SPEC de clase

| Restricción | Cumplimiento diseñado |
|---|---|
| Mobile-first con mejora progresiva | Base de una columna sin media queries; mejoras solo con min-width. UX-UI §3. |
| Todos los listados y tablas paginados | page/page_size, total y orden determinista; incluye categorías y moderación. TRD TD-002. |
| Menú sándwich mobile | Panel con foco gestionado y controles ARIA. UX-UI §4.5. |
| Screaming Architecture | Dominios identidad, catálogo, aportes y roadmap. TRD §3.2. |
| Estética Google/Material | Colores, tipografía, espaciado, sombras, radios y estados concretos. UX-UI §2–4. |
| Autenticación segura | Recuperación completa, rate limiting de login y hash Argon2id justificado en TRD. |

## 8. Vacíos de la SPEC de clase y decisiones tomadas

| ID | Vacío | Alternativas consideradas | Decisión | Desarrollo |
|---|---|---|---|---|
| D-01 | Quién valida | Administrador / comunidad / automático | Administrador designado | PRD BR-001 |
| D-02 | Visibilidad del catálogo | Público / requiere cuenta | Público | PRD US-001 |
| D-03 | Qué significa roadmap | Guardar enlaces / ordenar y avance / múltiples planes | Uno privado, orden manual y tres estados | PRD BR-003 |
| D-04 | Qué pasa tras rechazo | Definitivo / corregir y reenviar / edita administrador | Motivo obligatorio y reenvío por autor | PRD BR-004 |
| D-05 | Qué significa gratuito | Todo gratis / contenido gratis con certificado pago / pruebas temporales | Contenido completo gratis, certificado pago opcional, sin pruebas | PRD BR-005 |
| T-01 | Implementación de búsqueda y normalización | Motor externo / SQL; reglas amplias / lista conservadora | Texto y categoría; normalización acotada en BackendSchema §1.2 | Diseño técnico de D-09 y RF-003 |
| D-06–08 | Nombre, categorías y exclusiones | Nombre propuesto / elección propia; categorías amplias / solo tecnología; ampliar / acotar MVP | SaberColectivo; diez categorías amplias, una principal y sin subcategorías; exclusiones confirmadas | PRD §8–10 |
| D-09 | URL duplicada y cambios de edición | URL única / varias ediciones por URL / revisión manual de duplicados | URL normalizada única; misma URL conserva ID, URL distinta permite otro curso; respuesta según visibilidad | PRD BR-007/008, US-004 |
| D-10 | Administrador revisa su aporte | Permitir con trazabilidad / exigir otro administrador | Permitido en MVP; en_revision obligatorio y autorización en backend | PRD BR-010, TRD TD-010 |
| D-11 | Gestión de publicados | Diferir / gestión mínima / nueva revisión por cambio | Editar metadatos, retirar con motivo, republicar tras verificar; sin borrar roadmaps ni historial por campo | PRD BR-011–013, US-009 |

D-01 a D-11 fueron confirmadas expresamente por el autor el 27/10/2026. Las decisiones técnicas se identifican como propias del diseño, nunca como requisitos docentes.

## 9. Historia de la SPEC (autoregistro de cambios)

| Fecha | Cambio | Por qué |
|---|---|---|
| 21/10/2026 | Fecha de entrega indicada por la SPEC original | Dato de la fuente; no acredita fecha real del commit. |
| 27/10/2026 | Se verificó el commit original 2128507 | Preservar la consigna y trazabilidad. |
| 27/10/2026 | Refinamiento con D-01 a D-08, restricciones y pendientes | Incorporar SaberColectivo, diez categorías y alcance confirmado, sin inventar respuestas. |
| 27/10/2026 | Incorporadas D-09 a D-11: duplicados y gestión administrativa | Cerrar decisiones operativas confirmadas por el autor. |
| 27/10/2026 | Inventario real y pnpm global 12.15.0; objetivos React 19.3.1, Vite 8.3.5 y FastAPI 0.162.0 | Preparar entorno sin inicializar aplicación; instalaciones del proyecto y verificación quedan para B aprobada. |
