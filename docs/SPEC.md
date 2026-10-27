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

Catálogo público con búsqueda, filtro de categoría y paginación; registro, login, logout y recuperación completa de contraseña; aportes con metadatos y declaración de toma; revisión por administrador designado; rechazo motivado y corrección/reenvío por autor; un único roadmap privado por usuario con orden manual y estados pendiente/en curso/completado.

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

| Capa | Tecnología | Versión y evidencia |
|---|---|---|
| Frontend | React + Vite + pnpm | React 19 exigido; versiones exactas del proyecto pendientes de instalación autorizada en Instancia B. No hay frontend. |
| Backend | FastAPI + uv | FastAPI stable exigido (referencia de consigna: 0.158.2). Sin backend instalado; uv detectado, ejecución bloqueada por confinamiento de Snap. |
| Base de datos | PostgreSQL | 18.6 fijada por consigna; no se ha creado ni verificado un contenedor del proyecto. |
| Infra dev | Docker + Compose | Herramientas del host registradas en TRD §1; no equivalen a un entorno del proyecto validado. |

Pendiente obligatorio V-01: registrar versiones exactas resueltas y su compatibilidad antes de cerrar la entrega; no instalar durante esta instancia sin resolver la contradicción con la prohibición de inicializar la aplicación.

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
| T-01 | Búsqueda y duplicados | Solo categoría / texto y categoría; URL exacta / normalizada | Texto y categoría; unicidad por URL normalizada | Decisión técnica de diseño, TRD y BackendSchema |
| D-06–08 | Nombre, categorías y exclusiones | Nombre propuesto / elección propia; categorías amplias / solo tecnología; ampliar / acotar MVP | SaberColectivo; diez categorías amplias, una principal y sin subcategorías; exclusiones confirmadas | PRD §8–10 |

D-01 a D-08 fueron confirmadas expresamente por el autor el 27/10/2026. Las decisiones técnicas se identifican como propias del diseño, nunca como requisitos docentes.

## 9. Historia de la SPEC (autoregistro de cambios)

| Fecha | Cambio | Por qué |
|---|---|---|
| 21/10/2026 | Fecha de entrega indicada por la SPEC original | Dato de la fuente; no acredita fecha real del commit. |
| 27/10/2026 | Se verificó el commit original 2128507 | Preservar la consigna y trazabilidad. |
| 27/10/2026 | Refinamiento con D-01 a D-08, restricciones y pendientes | Incorporar SaberColectivo, diez categorías y alcance confirmado, sin inventar respuestas. |
