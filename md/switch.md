# Componente: Switch

## Descripción

Control de encendido/apagado inmediato (on/off) para una única opción independiente. A diferencia de Checkbox, el cambio se aplica al instante, sin necesidad de un botón de guardar aparte.

**Usar cuando**: la acción es binaria e inmediata (activar notificaciones, modo oscuro, una preferencia que toma efecto al tocar).
**No usar cuando**: hay que seleccionar varias opciones de una lista (usar Checkbox) o el cambio requiere confirmación explícita antes de aplicarse.

## Anatomía

1. Track (contenedor tipo píldora)
2. Thumb (círculo que se desplaza dentro del track)
3. Columna de texto: Label (obligatorio) + Helper text (opcional, togglable)

No tiene ícono — a diferencia de Button, no se consideró necesario para este control.

## Variantes y properties

### Switch · 4 variantes

| Property | Tipo | Valores |
|---|---|---|
| `Label` | Texto | — |
| `Helper` | Texto | — |
| `Show Helper` | Booleano | — |
| `State` | Variante | `Off`, `On` |
| `Disabled` | Variante | `false`, `true` |


## Tokens que consume

| Token | Apunta a | Primitivo | Valor | En el canvas |
|---|---|---|---|---|
| `Switch/track-off` | `background/secondary` | `Color/Neutral/50` | `#F3F4F6` | En el canvas |
| `Switch/track-on` | `action/primary/default` | `Color/Brand/500` | `#4F46E5` | En el canvas |
| `Switch/thumb` | `background/primary` | `Color/Neutral/00` | `#FFFFFF` | En el canvas |
| `Switch/thumb-border` | `border/default` | `Color/Neutral/300` | `#9CA3AF` | En el canvas |
| `Switch/label` | `text/primary` | `Color/Neutral/900` | `#0F172A` | En el canvas |
| `Switch/helper` | `text/secondary` | `Color/Neutral/700` | `#334155` | En el canvas |
| `Switch/track/width` — 40px. = thumb*2 + inset*2, reutiliza Space/2XXL. | `Space/2XXL` | — | `40px` | En el canvas |
| `Switch/track/height` — 24px. = thumb + inset*2, reutiliza Space/XL. | `Space/XL` | — | `24px` | En el canvas |
| `Switch/track/radius` — Pill, reutiliza radius/Full. | `radius/Full` | — | `9999 (pill)` | En el canvas |
| `Switch/thumb/size` — 16px, reutiliza Space/M. | `Space/M` | — | `16px` | En el canvas |
| `Switch/thumb/inset` — 4px de separación entre thumb y borde del track, reutiliza Space/XXS. | `Space/XXS` | — | `4px` | En el canvas |
| `Switch/track/focus` — Anillo de foco, reutiliza action/focus/default (mismo token que Button). | `action/focus/default` | `Color/Brand/500` | `#4F46E5` | Sin uso en el canvas |

## Hallazgos de la auditoría

⚠️ **1 token sin uso en el canvas:** `Switch/track/focus`.

✅ **11 de 12 tokens** se usan en el canvas, directo o a través de su alias.

⚠️ **6 de 12 tokens tienen descripción** en Figma (50%). El principio 4 pide el 100%.

## Accesibilidad

- **Contraste**: `track-on` (#4F46E5) con thumb blanco tiene contraste suficiente para distinguir el estado visualmente; `label` (#0F172A) sobre fondo blanco ≈17.9:1, `helper` (#334155) sobre blanco también pasa AA cómodo.
- **Disabled y contraste**: al aplicarse como opacidad global (40%) y no como recoloreo, el contraste baja proporcionalmente en el estado disabled — aceptable porque WCAG exime a controles deshabilitados de este criterio (no transmiten información activa), mismo criterio que documentamos en Button.
- **Foco visible — pendiente de implementación en código**: el token `Switch.track.focus` existe y está listo (reutiliza `action/focus/default`, el mismo semántico que Button), pero **no se modeló como variante persistente en Figma** — intentarlo así requeriría duplicar la matriz completa (State × Disabled × Focused = 8 variantes) solo para mostrar un anillo que en código es un simple `:focus-visible`. Se documenta acá para que la implementación lo aplique como `outline` de 2px alrededor del track, igual que se resolvió para el botón `link`.
- **Rol y teclado**: implementar con `<input type="checkbox" role="switch">` o `role="switch"` explícito sobre un elemento accesible; Space/Enter alterna el estado; el `label` debe estar asociado semánticamente (via `<label>` o `aria-labelledby`), no ser solo texto visual al lado.
- **Target táctil**: el track visual (40×24) queda por debajo del mínimo de 44×44 — **resuelto vía hitbox expandida** (regla 5.1 del doc maestro), no agrandando el track. En implementación, el `<label>`/wrapper clickeable que envuelve track+texto debe garantizar 44×44 mínimo de área interactiva real, aunque el track se siga viendo en su tamaño actual.

## Ejemplos de uso

**Correcto**: una fila de configuración "Notificaciones por email" con su Switch a la derecha, aplicando el cambio inmediatamente al tocar.

**Incorrecto**: usar Switch dentro de un formulario que se envía con un botón "Guardar" aparte — ahí el cambio no es inmediato, así que Checkbox comunica mejor la semántica de "selección pendiente de confirmar".

## Notas de implementación

- ✅ Los 6 tokens de color venían ya definidos en el export original; se sumaron 6 tokens nuevos (geometría + foco) reutilizando primitivos existentes, sin inventar valores.
- **Decisión de diseño**: Disabled se resuelve con opacidad (40%, fija en el componente), no con tokens de color por estado — mantiene el componente simple y evita duplicar 6 tokens de color solo para la variante deshabilitada. Si el equipo de diseño prefiere colores dedicados para disabled más adelante, es un cambio aditivo, no rompe nada de lo ya construido.
- **Decisión de diseño**: un solo tamaño — no se replicó el patrón `SM/MD/LG` de Button porque no hay un caso de uso identificado que lo pida. Fácil de sumar después si aparece.
- Componente listo para pasar a implementación en código, con la salvedad del foco (aplicar como `:focus-visible` + `outline`, no viene de una variante de Figma).