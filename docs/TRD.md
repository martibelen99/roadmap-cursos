# TRD — SaberColectivo

Diseño para Instancia B. No se ha instalado ni generado la aplicación. Fecha: 27/10/2026.

## 1. Stack y versiones exactas

| Capa | Elección | Versión / evidencia | Por qué |
|---|---|---|---|
| Frontend | React + Vite + pnpm | React 19 requerido; parches de React y versiones Vite/pnpm no resueltos, sin package.json ni instalación | Stack docente. |
| Backend | FastAPI + uv | FastAPI stable requerido; 0.158.2 es referencia de la SPEC, no instalación verificada. uv encontrado en /snap/bin/uv, su ejecución falló por confinamiento Snap. | Stack docente. |
| ORM / migraciones / driver | SQLAlchemy + Alembic + psycopg | Versiones exactas pendientes de resolución compatible en B | SQL explícito para locks/transacciones; migraciones revisables. |
| BD | PostgreSQL | 18.6 exigido; sin contenedor ni servidor del proyecto verificado | Stack docente. |
| Infra dev | Docker + Compose | Host: Docker 29.1.3, build 29.1.3-0ubuntu3~24.04.2; Compose 2.40.3+ds1-0ubuntu1~24.04.1 | Salidas reales del 27/10/2026. |
| Runtime host | Node / Python | Node v22.23.2; Python 3.12.3 | Inventario; no garantiza compatibilidad con paquetes futuros. |
| Seguridad | pwdlib con Argon2id | Versión de librería pendiente; parámetros TD-004 | Hash más fuerte permitido por SPEC. |
| Correo dev | Capturador SMTP local Mailpit | Versión de imagen pendiente | Demostrar recuperación completa sin enviar correo real durante desarrollo. |

V-01 bloquea el cierre formal del requisito «versiones exactas instaladas». No se presentan ejemplos ni versiones del host como dependencias instaladas del proyecto. Antes de B, resolver versiones stable compatibles, fijar tags/digests e inventario y registrar comandos/evidencia. Si el docente exige instalaciones ya en A, consultar cómo cumplirlo sin violar la prohibición actual de inicializar dependencias. No se propone una imagen latest flotante como entrega reproducible.

## 2. Decisiones técnicas (mínimo 3)

| ID | Decisión | Alternativas | Elegida | Por qué |
|---|---|---|---|---|
| TD-001 | Autenticación | JWT / sesiones servidor / OAuth externo | Sesión opaca en PostgreSQL, cookie HttpOnly | Revocación sencilla al salir o recuperar contraseña; sin refresh tokens. |
| TD-002 | Paginación | Cursor / offset con total | page desde 1, page_size 20 por defecto, máximo 100; offset + total | UI numerada y conjunto moderado; toda colección usa el mismo contrato. |
| TD-003 | Errores | Envelope propio / Problem Details | Envelope error.code/message/fields/request_id | Mensajes de campos y estados coherentes; sin detalles internos. |
| TD-004 | Hash de contraseña | bcrypt / Argon2id | Argon2id, m=65536 KiB, t=3, p=4, salt aleatoria por hash | Más fuerte permitido por SPEC; consume memoria y evita el límite de entrada de bcrypt. Calibrar en B sin bajar estos parámetros sin justificación. |
| TD-005 | Persistencia | SQLModel / SQLAlchemy / SQL manual | SQLAlchemy síncrono + psycopg + Alembic | Transacciones y constraints explícitos; endpoints de BD síncronos, sin bloquear event loop con IO síncrona en async. |
| TD-006 | Verificar URL activa | Crawler del servidor / revisión manual | Sintaxis en API; revisión humana en navegador antes de aprobar | Evita un subsistema de scraping y SSRF; el backend nunca descarga URLs aportadas. |
| TD-007 | Orden de roadmap | Array de IDs / ranking fraccional / posición entera | Posición entera, mover antes de otro ítem o al final bajo lock de usuario | Funciona entre páginas sin descargar todo el roadmap; unicidad diferible. |
| TD-008 | Rate limiting | Memoria de proceso / Redis / PostgreSQL | Contadores atómicos persistidos en PostgreSQL | Un servicio menos; no se evade cambiando de worker o reiniciando API. |
| TD-009 | Búsqueda y URL única | Motor externo / SQL; limpieza amplia / conservadora | ILIKE título/tema; normalización de BackendSchema §1.2 y UNIQUE en todos los estados | D-09; lista cerrada de tracking, conservar parámetros identificadores y rutas hash. |
| TD-010 | Autorización administrativa | Restricción solo UI / verificación backend; revisión independiente / propia | Verificar administrador en cada caso de uso y admitir aprobación propia | D-10; operación individual viable, con autor/revisor/fecha y eventos. |
| TD-011 | Correcciones, retiro y republicación | Borrado físico / nueva revisión / gestión directa | Metadatos actuales + actor/fecha; estado retirado; eventos mínimos, sin snapshots por campo | D-11; conserva FKs y roadmap, sin sistema de solicitudes de edición. |

### Seguridad operativa diseñada

Sesión de 32 bytes aleatorios codificados base64url; guardar solo SHA-256 del secreto. Caduca a las 12 h sin renovación deslizante. Cookie `session`, Path=/, HttpOnly, SameSite=Lax; Secure=true con HTTPS. En dev localhost HTTP, Secure=false explícito; no trasladar esa configuración a despliegue. Login rota sesión y logout revoca antes de borrar cookie. GET /auth/me devuelve el usuario y un CSRF derivado mediante HMAC-SHA256 del token de sesión y APP_SECRET; no persistirlo en localStorage. Toda mutación autenticada exige `X-CSRF-Token` válido. Toda mutación, incluidas registro/login/recuperación, exige Origin igual a APP_ORIGIN; fallas 403. El frontend utiliza proxy del mismo origen, sin CORS wildcard con credenciales. El HMAC CSRF no es el hash almacenado de la sesión.

Contraseña: 12 a 128 caracteres, sin truncar ni recortar espacios, sin reglas arbitrarias de composición. Email trim + minúsculas, máximo 254 caracteres; nombre 1–100. Registro no acepta rol. Errores de login iguales para usuario ausente o contraseña incorrecta; ejecutar verificación de hash ficticio en caso ausente. Secretos, cookies y tokens no van en logs ni URL de API.

Rate limiting: ventanas fijas de 15 min, límites combinados por IP y por identidad de email normalizada (clave HMAC, sin email claro): login 20/IP y 5/email, registro 10/IP, recuperación 10/IP y 3/email; reset 10/IP. Cada intento cuenta y supera umbral → 429 con Retry-After hasta siguiente ventana. Para recuperación, agotamiento por email devuelve el mismo 202 sin enviar nuevo correo, para no revelar existencia; agotamiento IP devuelve 429 para todas las cuentas. INSERT/UPSERT atómico en PostgreSQL, limpieza de ventanas vencidas; IP directa de conexión en dev, headers de proxy solo si proxy confiable configurado. Validar concurrencia en B.

Recuperación: token aleatorio de 32 bytes, almacenar solo SHA-256, caduca a los 30 min, uso único. Solicitudes con formato válido responden 202 genérico exista o no cuenta. Para cuenta existente crear token invalidando anteriores y enviar por SMTP mediante puerto de correo; si envío falla, eliminar token nuevo y registrar incidente sin secreto. La API mantiene mensaje genérico; UI explica pedir un enlace nuevo si no llega. Enlace construido desde APP_ORIGIN fijo: `/restablecer#token=...`; el cliente lee fragmento, lo retira del historial y envía token en body al confirmar. Reset válido cambia hash, consume token e invalida todas las sesiones y otros tokens en una transacción bloqueando usuario; no inicia sesión automáticamente. Dos resets concurrentes no pueden consumir el mismo token. Mailpit permite recorrer el enlace en dev; proveedor real se decide al desplegar, fuera de esta instancia.

Validación administrativa: URL activa se comprueba abriendo la página del proveedor, confirmando que el contenido completo sigue disponible sin pago ni prueba temporal. 404/410 o contenido inaccesible impiden aprobar; si no se puede verificar, dejar en revisión o rechazar con explicación, nunca aprobar por suposición. No hay cron de revalidación prometido.

## 3. Arquitectura

### 3.1 Patrón y capas

```mermaid
flowchart LR
    HTTP[Router HTTP] --> UC[Casos de uso]
    UC --> RULE[Reglas puras]
    UC --> PORT[Puertos de repositorio y correo]
    ADAPTER[Adaptadores SQL y SMTP] -. implementan .-> PORT
    ADAPTER --> DB[(PostgreSQL)]
```

Los routers validan entrada y traducen errores. Reglas puras sin HTTP ni BD. Los casos de uso coordinan puertos y unidad de trabajo; no contienen SQL. Repositorios implementan persistencia sin decidir reglas de negocio. Esta separación aclara el término «services sin BD» de la plantilla: las reglas no acceden a infraestructura; la orquestación solicita operaciones mediante interfaces inyectadas. Nunca hacer commit por repositorio si el caso de uso necesita varias escrituras atómicas.

### 3.2 Estructura de carpetas (a crear exclusivamente en B)

```text
backend/
  pyproject.toml
  Dockerfile
  app/
    main.py
    config.py
    identidad/   # router, schemas, rules, use_cases, ports, repository
    catalogo/    # mismas responsabilidades
    aportes/     # revisión e historial pertenecen a aportes
    roadmap/     # orden y estados pertenecen a roadmap
    infraestructura/ # BD, correo, rate limiting, unidad de trabajo
  migrations/
  tests/
frontend/
  package.json
  Dockerfile
  src/
    main.jsx
    app/         # router, shell, sesión
    features/
      identidad/
      catalogo/
      aportes/
      roadmap/
    shared/      # componentes accesibles, cliente HTTP
    styles/      # tokens UX-UI
  tests/
docker-compose.yml
docs/
```

Son rutas de diseño, no archivos creados en A.

### 3.3 Servicios Docker (entorno de desarrollo)

| Servicio | Imagen/base prevista | Puerto host → contenedor | Rol |
|---|---|---|---|
| postgres | postgres:18.6 (digest pendiente V-01) | 5432 → 5432 solo localhost | Persistencia con volumen y healthcheck |
| backend | Python 3.12 slim; parche/digest pendiente | 8000 → 8000 localhost | FastAPI y migraciones antes de servir |
| frontend | Node 22; parche/digest pendiente | 5173 → 5173 localhost | Vite con proxy /api hacia backend |
| mailpit | Mailpit, tag pendiente V-01 | 8025 → 8025 localhost; SMTP 1025 solo red interna | Bandeja de recuperación dev |

```mermaid
flowchart LR
    B[Navegador :5173] --> FE[Vite]
    FE -->|/api| BE[FastAPI :8000]
    BE --> PG[(PostgreSQL)]
    BE -->|SMTP :1025| MAIL[Mailpit]
    B -->|Bandeja :8025| MAIL
```

En B `docker compose up --build` debe arrancar desde repositorio limpio, aplicar migraciones una vez y esperar BD saludable; seed idempotente de categorías confirmadas y administrador obtenido de secretos locales. Sin contraseñas de prueba en Git. API `GET /health` fuera de /api/v1 devuelve 200 si BD responde o 503 si no; no revela configuración. No se afirma que esos servicios existan hoy.

## 4. Convenciones

- Commits Conventional Commits, específicos: docs, feat, fix, test, refactor, chore, docker.
- Git: main conserva primer commit original 2128507; documentación en docs/instancia-a. Un documento o lote coherente por commit. B solo tras aprobación docente explícita, ramas feat/identidad, feat/catalogo, feat/aportes, feat/roadmap y test/integracion según tarea. Inspeccionar diff, validar y commitear; PR contra main con resumen y pruebas. El autor revisa en este TP individual; aprobación docente habilita B. Merge y push requieren autorización expresa del usuario. Sin rebase destructivo de historial publicado ni force push. Plantillas de entrada se versionan sin alterar sus ejemplos.
- Errores comunes: 401 sin sesión, 403 rol/CSRF/Origin, 404 recurso no visible, 409 conflicto, 422 campos inválidos, 429 límite, 500 genérico. Forma única:

```json
{"error":{"code":"validation_error","message":"Revisá los campos indicados","fields":{"titulo":"Campo obligatorio"},"request_id":"identificador-de-traza"}}
```

`fields` puede ser vacío; request_id se genera en servidor, sin secretos. Solo para duplicado ya publicado se agrega `error.resource={curso_id,ficha_url}` con datos públicos; duplicado en revisión responde `course_in_review` sin ID ni autor, y rechazado/retirado recibe `duplicate_url` genérico. Los demás errores no incluyen resource.

| Variable | Ejemplo no secreto | Obligatoria | Uso |
|---|---|---|---|
| DATABASE_URL | postgresql+psycopg://usuario:CLAVE_LOCAL@postgres:5432/comunidad | Sí | Conexión, nunca valor real en Git |
| APP_ORIGIN | http://localhost:5173 | Sí | Origin permitido y enlaces de recuperación |
| APP_SECRET | generar secreto aleatorio de al menos 32 bytes | Sí | CSRF y claves de rate limit |
| COOKIE_SECURE | false en dev, true con HTTPS | Sí | Seguridad cookie |
| SMTP_HOST / SMTP_PORT | mailpit / 1025 | Sí | Correo dev |
| SMTP_FROM | no-reply@example.test | Sí | Remitente |
| SMTP_USER / SMTP_PASSWORD | ausentes en Mailpit | No en dev | Autenticación de proveedor real |
| ADMIN_EMAIL / ADMIN_PASSWORD | solo en .env local | Sí en bootstrap inicial | Seed administrador; hash Argon2id, no registrar password |
| LOG_LEVEL | INFO | No | Nivel de logging |

Logging estructurado con request_id, método, ruta de plantilla, status, duración y actor interno cuando corresponda. No body de auth, cookies, email, token ni URL con fragmentos. Manejo centralizado traduce excepciones esperadas; rollback antes de responder. Stack trace solo en logs protegidos para errores inesperados. Auditoría de moderación y gestión en eventos_aporte (acción, actor, fecha, transición y motivo), sin snapshots completos de metadatos. cursos conserva autor_id, actualizado_por_id/actualizado_en y revisado_por_id/revisado_en; retiro agrega responsable/fecha/motivo. No depender de logs como única evidencia.

## 5. Requisitos no funcionales técnicos

| Restricción | Cómo se cumple | Verificación prevista |
|---|---|---|
| Paginación total | TD-002, también categorías e historial | Pruebas página 1/2/vacía/fuera de rango |
| Mobile-first y menú | UX-UI §3 y §4.5 | 360, 768 y 1024 px, teclado |
| Screaming Architecture | §3.2, dominios en raíz de app | Revisión árbol en B |
| Recuperación | Token temporal, SMTP, consumo transaccional | E2E con Mailpit y reutilización fallida |
| Hash | TD-004 | Hash comienza por argon2id; contraseña no persiste en claro |
| Rate limiting | TD-008 | 429, Retry-After, reinicio y dos workers |
| Privacidad | Filtros de propietario en repositorio y autorización en caso de uso | Dos usuarios, acceso directo a ID ajeno |

## 6. Riesgos técnicos

| Riesgo | Probabilidad | Impacto | Mitigación |
|---|---|---|---|
| Dependencias y versiones sin resolver | Alta | Bloquea instalación reproducible | V-01 y prueba de compatibilidad al habilitar B |
| Correo no recibido | Media | Usuario sin recuperación | Mailpit dev, errores controlados, reintento por usuario |
| Enlace cambia después de aprobado | Media | Catálogo desactualizado | Administrador corrige o retira/republica; sin crawler ni revalidación automática periódica en MVP |
| Orden concurrente o revisión doble | Media | Datos inconsistentes | Locks por usuario/curso, versión optimista y constraints |
| Argon2 satura recursos | Media | Login lento | Limitar intentos antes de hash, medir parámetros en B |

Fuentes consultadas el 27/10/2026: [FastAPI: hashing con Argon2](https://fastapi.tiangolo.com/tutorial/security/oauth2-jwt/), [OWASP: recuperación](https://cheatsheetseries.owasp.org/cheatsheets/Forgot_Password_Cheat_Sheet.html), [OWASP: sesiones](https://cheatsheetseries.owasp.org/cheatsheets/Session_Management_Cheat_Sheet.html). Las duraciones, umbrales y arquitectura elegidos son decisiones de este proyecto, no cifras impuestas por esas fuentes.

Normalización: usar parser estructurado y validación explícita; [urllib.parse](https://docs.python.org/3/library/urllib.parse.html) no valida por sí solo entradas hostiles. [RFC 3986](https://www.rfc-editor.org/rfc/rfc3986.html) fundamenta separar esquema, autoridad, path, query y fragmento; la lista concreta de tracking y las excepciones conservadoras son política propia de SaberColectivo, no requisitos de la RFC. BackendSchema §1.2 es la fuente única del algoritmo.
