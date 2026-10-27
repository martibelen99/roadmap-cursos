# UX-UI — SaberColectivo

Guía de diseño, no interfaz implementada. Identidad definitiva confirmada: el conocimiento crece cuando se comparte. Fecha: 27/10/2026.

## 1. Principios

1. Base usable en 360 px, una columna y controles cómodos; ampliar agrega capacidad.
2. La información de gratuidad, edición y certificado debe poder compararse sin abrir el proveedor.
3. La jerarquía visual distingue explorar, aportar y planificar; una acción primaria por sección.
4. Vacío, espera, error y éxito se diseñan junto con el estado normal.
5. El color acompaña texto e iconos, nunca comunica por sí solo.

## 2. Design tokens

### 2.1 Paleta de colores

| Token | Valor | Uso |
|---|---|---|
| --color-primary | #1a73e8 | Azul Google en enlaces y acciones |
| --color-on-primary | #ffffff | Texto sobre primario |
| --color-surface | #ffffff | Cards, paneles y formularios |
| --color-background | #f8f9fa | Fondo de página |
| --color-text | #202124 | Texto principal |
| --color-text-muted | #5f6368 | Metadatos y ayudas |
| --color-border | #747775 | Límite perceptible de controles |
| --color-primary-hover | #1558b0 | Hover de primario |
| --color-primary-active | #174ea6 | Presionado y texto sobre chip claro |
| --color-primary-container | #e8f0fe | Selección suave |
| --color-success | #137333 | Éxito con etiqueta |
| --color-warning | #7a4e00 | Revisión pendiente con etiqueta |
| --color-error | #b3261e | Error y rechazo con etiqueta |
| --color-disabled / --color-on-disabled | #e3e3e3 / #444746 | Controles deshabilitados legibles |
| --color-overlay | #20212499 | Fondo de modal/drawer |

Ratios calculados localmente con luminancia relativa sRGB (WCAG), no estimados ni certificados mediante un sitio externo:

| Color | Contra surface | Contra background |
|---|---|---|
| primary | 4.51:1 | 4.27:1 |
| text | 16.10:1 | 15.27:1 |
| text-muted | 6.05:1 | 5.74:1 |
| border | 4.53:1 | 4.30:1 |
| success | 5.95:1 | 5.65:1 |
| warning | 7.20:1 | 6.83:1 |
| error | 6.54:1 | 6.20:1 |

Texto normal requiere ≥4.5:1; controles ≥3:1. Blanco sobre primary tiene el mismo ratio de la tabla contra surface. Primary solo se usa como texto sobre surface blanco (4.51:1). Sobre background gris usar primary-active #174ea6; primary sobre gris da 4.27:1 y no cumple AA para texto normal. No usar warning claro ni grises decorativos para texto. El diseño de los bordes no depende del foco para ser perceptible.

### 2.2 Tipografía

| Token | Familia | Tamaño / line-height | Uso |
|---|---|---|---|
| --font-family-base | system-ui, sans-serif | — | Sin descarga de fuente; disponible localmente |
| --text-sm | Heredada | 14px / 20px | Metadatos y ayudas |
| --text-base | Heredada | 16px / 24px | Cuerpo, inputs |
| --text-lg | Heredada | 20px / 28px | Secciones y cards |
| --text-xl | Heredada | 24px / 32px | Título de pantalla |
| --font-weight-normal / --font-weight-medium / --font-weight-bold | Heredada | 400 / 500 / 700 | Jerarquía |

### 2.3 Espaciado, radios, sombras, z-index

| Tokens | Valores | Uso |
|---|---|---|
| --space-1 a --space-8 | 4, 8, 12, 16, 24, 32, 40, 48px | Escala de separación |
| --radius-sm / md / lg | 8 / 12 / 24px | Inputs y botones / cards / modal |
| --shadow-sm | 0 1px 3px #20212426 | Card en reposo |
| --shadow-md | 0 4px 12px #20212433 | Card hover, drawer y modal |
| --z-header / drawer / modal / toast | 100 / 200 / 300 / 400 | Capas |
| --content-max / --target-min | 1200px / 44px | Contenedor y controles |
| --motion-fast | 150ms | Transiciones; desactivadas con reduced motion |

### 2.4 Bloque de variables listo para copiar

Este bloque es una especificación dentro del documento, no un archivo de aplicación.

```css
:root {
  --color-primary: #1a73e8;
  --color-on-primary: #ffffff;
  --color-surface: #ffffff;
  --color-background: #f8f9fa;
  --color-text: #202124;
  --color-text-muted: #5f6368;
  --color-border: #747775;
  --color-primary-hover: #1558b0;
  --color-primary-active: #174ea6;
  --color-primary-container: #e8f0fe;
  --color-success: #137333;
  --color-warning: #7a4e00;
  --color-error: #b3261e;
  --color-disabled: #e3e3e3;
  --color-on-disabled: #444746;
  --color-overlay: #20212499;
  --font-family-base: system-ui, sans-serif;
  --text-sm: 0.875rem;
  --text-base: 1rem;
  --text-lg: 1.25rem;
  --text-xl: 1.5rem;
  --line-sm: 1.25rem;
  --line-base: 1.5rem;
  --line-lg: 1.75rem;
  --line-xl: 2rem;
  --font-weight-normal: 400;
  --font-weight-medium: 500;
  --font-weight-bold: 700;
  --space-1: 4px;
  --space-2: 8px;
  --space-3: 12px;
  --space-4: 16px;
  --space-5: 24px;
  --space-6: 32px;
  --space-7: 40px;
  --space-8: 48px;
  --radius-sm: 8px;
  --radius-md: 12px;
  --radius-lg: 24px;
  --shadow-sm: 0 1px 3px #20212426;
  --shadow-md: 0 4px 12px #20212433;
  --z-header: 100;
  --z-drawer: 200;
  --z-modal: 300;
  --z-toast: 400;
  --content-max: 1200px;
  --target-min: 44px;
  --motion-fast: 150ms;
}
```

## 3. Estrategia mobile-first con mejora progresiva

| Capa | Aplica | Qué define |
|---|---|---|
| Base sin media queries | Todos los tamaños | Una columna; header con nombre y menú sándwich; cards apiladas; formularios verticales; gutter 16px; ancho de controles 100% cuando corresponda; targets ≥44px. |
| @media (min-width: 768px) | Tablet y mayores | Catálogo en dos columnas; tablas de aportes/roadmap con encabezados y mismas acciones; formularios agrupables en dos columnas. |
| @media (min-width: 1024px) | Desktop | Catálogo en tres columnas, navegación horizontal reemplaza drawer; gutter 24px, max-width 1200px centrado. |

No usar max-width como condición de media query ni media queries para corregir una base rota. La propiedad max-width del contenedor sí es válida. `prefers-reduced-motion` es una preferencia de accesibilidad, no un breakpoint de layout; desactiva transiciones y shimmer. En 320px y zoom 200% no debe perderse contenido ni aparecer scroll horizontal de página.

## 4. Componentes (anatomía + estados)

### 4.1 Botón (primario / secundario / peligro)

Altura mínima target-min, padding space-3/space-5, radio sm, texto base de peso medio; icono opcional 20px con separación space-2. Primario fondo primary/on-primary; secundario surface con borde border y texto primary-active; peligro error con blanco.

Default según variante; hover usa primary-hover o fondo primary-container en secundario, peligro oscurece con filtro brightness(.9); active primary-active o brightness(.8) en peligro. Focus-visible: anillo 3px primary-active, offset 3px. Disabled: disabled/on-disabled, atributo disabled, sin activación. Loading: spinner más texto «Guardando…», aria-busy, deshabilitado; no pierde ancho ni nombre accesible. Error de acción deja botón disponible para reintentar y muestra mensaje persistente asociado a la zona.

### 4.2 Campo de formulario (input / select)

Label siempre visible, input de ≥44px, borde 1px border, radio sm, padding space-3; ayuda y error bajo campo en text-sm. Default vacío; focus con borde 2px primary y anillo visible sin desplazar contenido; filled conserva label; error con borde error, icono y texto asociado mediante aria-describedby y aria-invalid; disabled con colores disabled y explicación si impide continuar. Durante envío campos se bloquean temporalmente sin borrar valores. Select de categoría carga páginas y ofrece «Cargar más categorías»; error de carga permite reintentar, vacío explica que no hay categorías disponibles. Booleanos con radios Sí/No sin opción preseleccionada. Docentes como campo repetible con botones accesibles; remover exige nombre específico del docente.

### 4.3 Card de curso

Surface, radio md, shadow-sm, padding space-4, título text-lg, categoría y docentes text-sm; edición/versión y badges con texto para examen, cuestionarios, certificado y aval. Si certificado pago, etiqueta «Certificado opcional pago». Título enlaza a detalle; acción de roadmap separada, sin botones dentro de un enlace envolvente.

Default shadow-sm; hover shadow-md; focus anillo en enlace/acción; seleccionada muestra check y «En mi roadmap» sobre primary-container, sin depender del color. Loading usa skeleton §4.8; error de agregar mantiene card y ofrece reintento; acción deshabilitada mientras envía, éxito anuncia sin mover foco.

### 4.4 Lista/tabla paginada

Base en lista semántica de cards; desde 768px los listados densos usan tabla con caption y encabezados. Presentación alternativa no duplica controles en el árbol accesible. Paginador muestra página, total, anterior y siguiente, botones ≥44px y aria-current=page. page_size 20 por defecto; cambios de filtro reinician página 1. Antes/después deshabilitados en extremos; página fuera de rango se explica y ofrece volver a primera, sin bucle de redirección. Loading conserva espacio; empty diferencia sin datos de sin coincidencias; error persistente con Reintentar. Cambios anuncian resultado con aria-live polite y foco en título de resultados tras navegación explícita. Reordenar roadmap ofrece subir/bajar o mover antes de otro ítem incluso fuera de la página, nunca solo drag-and-drop. Ítem retirado conserva datos, posición y acciones de avance/quitar, con icono warning y texto «Este curso ya no está disponible en el catálogo comunitario»; no ofrece enlace de ficha pública. La administración usa badges con texto Publicado/Retirado, filtro de estado y las mismas reglas de paginación.

### 4.5 Menú sándwich

Botón 44px con aria-label «Abrir menú», aria-expanded y aria-controls. Panel surface, ancho min(320px, 90vw), shadow-md, overlay; enlaces de 48px, activo con aria-current y primary-container. Cerrado sin foco ni contenido accesible; abierto con foco inicial en primer enlace, fondo inerte y foco contenido en panel. Cierra con enlace, X, Escape o overlay; al cancelar devuelve foco al botón. Si cambia a desktop, cerrar panel y retirar bloqueo de foco; navegación horizontal conserva las mismas opciones. Estados de botones siguen §4.1.

### 4.6 Modal

Para confirmar quitar un ítem o retirar/republicar un curso administrado, no para todo el formulario. En retiro incluye label «Motivo del retiro», textarea de 1–2000 caracteres, ayuda y error asociado; no permite confirmar vacío. En republicación incluye verificación explícita de enlace activo, gratuidad y problema solucionado; sin confirmar no envía. Dialog con aria-modal, título asociado, surface, radio lg, padding space-5, ancho máximo 480px y margen space-4. Cerrado no accesible; abierto atrapa foco, inicia en Cancelar y vuelve al disparador al cerrar; Escape cancela. Enviando muestra loading, evita doble envío y cierre que oculte resultado incierto. Error dentro del modal permite reintentar; éxito cierra y coloca foco en vecino o título si lista vacía.

### 4.7 Toast (notificación)

Surface con texto text, borde por tipo, icono y frase explícita, shadow-md; inferior con margen 16px y ancho limitado al viewport menos 32px; capa toast. Éxito/info role=status aria-live=polite, 6 segundos y botón cerrar, pausa al hover/foco. Error persiste hasta cerrar; mensaje principal también queda en formulario/banner para no perderse. No roba foco ni cubre acción enfocada; no se usa un toast como única explicación de un fallo.

### 4.8 Skeleton y Empty state

Skeleton gris disabled, dimensiones de card/detalle; contenedor aria-busy y texto «Cargando», bloques decorativos aria-hidden. Animación opcional 150ms, desactivada con reduced motion. No simula contenido interactivo. Empty: icono decorativo, título text-lg, descripción y CTA: catálogo sin cursos → «Aportar curso» (login si visitante); filtros sin coincidencias → «Limpiar filtros»; roadmap vacío → «Explorar catálogo»; mis aportes → «Aportar curso»; cola vacía → «No hay aportes pendientes», sin CTA artificial.

## 5. Layout y navegación

Header sticky de 64px, surface, capa header; mobile nombre descriptivo + hamburguesa, desktop nombre + Catálogo/Mi roadmap/Mis aportes/Aportar y Revisar/Gestionar cursos solo administrador; acciones Ingresar/Registrarse o Salir. Enlace saltar al contenido como primer control. Contenedor y gutters según §3; formularios máximo 640px, texto legible sin bloques excesivamente anchos.

Pantallas: inicio orienta hacia catálogo; catálogo y detalle permiten descubrir; registro/login/recuperar/restablecer gestionan acceso; aportar y mis aportes acompañan envío/corrección; roadmap organiza avance; cola y detalle de revisión y gestión de publicados/retirados exclusivos del administrador. En edición administrativa no hay control de autor/lo_tomo, sí actor/fecha de última actualización y formulario de metadatos. Duplicado publicado ofrece botón «Ver curso»; en revisión solo mensaje, sin ID ni datos ajenos. Todas las rutas y APIs están en APPFlow. Cada pantalla tiene un h1 y estado de red; formularios muestran resumen de errores enlazado a campos y conservan datos salvo contraseñas/tokens.

## 6. Accesibilidad mínima (no negociable)

Diseñada aquí; pendiente de prueba de implementación, no se marca como cumplida en una app inexistente.

- [ ] Texto ≥4.5:1 y límites de controles ≥3:1; ratios documentados en §2.
- [ ] Focus-visible en todos los controles y recorrido completo por teclado.
- [ ] Drawer con ARIA, foco contenido y restauración; sin trampas al cambiar breakpoint.
- [ ] Reduced motion desactiva transiciones y skeleton animado.
- [ ] Iconos decorativos aria-hidden; iconos de acción con nombre accesible; imágenes informativas con alt.
- [ ] Errores asociados, resumen enfocable y avisos anunciados.
- [ ] Targets ≥44px; zoom 200%, 320px y 360px sin pérdida de contenido.
- [ ] Reordenar con teclado y controles explícitos; estados expresados también con texto.

## 7. Iconografía y assets

SVG inline de Material Symbols, tamaño 24px en navegación y 20px en acciones, currentColor. Selección pequeña y local durante B: menu, close, search, check, warning, school, arrow_upward y arrow_downward. Sin biblioteca ni fuente descargada en A. Mantener licencia y atribución correspondientes al incorporar los assets. No se requieren fotografías ni imágenes generadas; nombre textual SaberColectivo identifica el producto sin requerir un logo gráfico.
