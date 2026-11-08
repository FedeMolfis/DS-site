# Componente: Button

## Descripción

Elemento interactivo que dispara una acción. El sistema define 4 variantes con distinto nivel de énfasis visual: `primary`, `secondary`, `icon-secondary` (solo ícono) y `link` (texto, sin contenedor).

**Usar cuando**: hay una acción que el usuario puede ejecutar (enviar, guardar, confirmar, navegar).
**No usar cuando**: es solo navegación entre páginas con URL propia → usar un componente de link/anchor semántico en su lugar (no un botón estilizado como link).

## Anatomía

1. Contenedor (background + borde opcional según variante)
2. Label de texto (ausente en `icon-secondary`)
3. Ícono (opcional en `primary`/`secondary`/`link`; obligatorio en `icon-secondary`) — implementado en Figma como property `Icon` (instance-swap) de la librería **Simple Design System**. Default: `Check`. Cualquier otro ícono de esa librería es válido como alternativa. Ver sección 4.0 de `DESIGN-SYSTEM.md` para la convención completa.

## Variantes y properties

### Button · 9 variantes

| Property | Tipo | Valores |
|---|---|---|
| `Show Icon` | Booleano | — |
| `Label` | Texto | — |
| `Icon` | Instancia (swap) | — |
| `Style` | Variante | `Primary`, `Link`, `Secondary` |
| `Size` | Variante | `MD`, `SM`, `LG` |

### Button Icon-Secondary · 3 variantes

| Property | Tipo | Valores |
|---|---|---|
| `Icon` | Instancia (swap) | — |
| `Size` | Variante | `SM`, `MD`, `LG` |


## Tokens que consume

| Token | Apunta a | Primitivo | Valor | En el canvas |
|---|---|---|---|---|
| `Button/radius` | `radius/action` | `radius/M` | `8px` | En el canvas |
| `Button/SM/padding-x` | `Space/S` | — | `12px` | En el canvas |
| `Button/SM/padding-y` | `Space/XXS` | — | `4px` | En el canvas |
| `Button/SM/font-size` | `typography/action/SM/font-size` | `Typography/size/XSM` | `12px` | Vía Text Style |
| `Button/SM/line-height` | `typography/action/SM/line-height` | `Typography/line-height/xsm` | `16px` | Vía Text Style |
| `Button/SM/font-weight` | `typography/action/SM/font-weight` | `Typography/weight/medium` | `Medium` | Vía Text Style |
| `Button/SM/icon-size` | `typography/action/SM/icon-size` | `Typography/line-height/xsm` | `16px` | Sin uso en el canvas |
| `Button/MD/padding-x` | `Space/M` | — | `16px` | En el canvas |
| `Button/MD/padding-y` | `Space/XS` | — | `8px` | En el canvas |
| `Button/MD/font-size` | `typography/action/MD/font-size` | `Typography/size/SM` | `14px` | Vía Text Style |
| `Button/MD/line-height` | `typography/action/MD/line-height` | `Typography/line-height/sm` | `20px` | Vía Text Style |
| `Button/MD/font-weight` | `typography/action/MD/font-weight` | `Typography/weight/medium` | `Medium` | Vía Text Style |
| `Button/MD/icon-size` | `typography/action/MD/icon-size` | `Typography/line-height/sm` | `20px` | Sin uso en el canvas |
| `Button/LG/padding-x` | `Space/L` | — | `20px` | En el canvas |
| `Button/LG/padding-y` | `Space/S` | — | `12px` | En el canvas |
| `Button/LG/font-size` | `typography/action/LG/font-size` | `Typography/size/MD` | `16px` | Vía Text Style |
| `Button/LG/line-height` | `typography/action/LG/line-height` | `Typography/line-height/md` | `24px` | Vía Text Style |
| `Button/LG/font-weight` | `typography/action/LG/font-weight` | `Typography/weight/medium` | `Medium` | Vía Text Style |
| `Button/LG/icon-size` | `typography/action/LG/icon-size` | `Typography/line-height/md` | `24px` | Sin uso en el canvas |
| `Button/border/focus` | `action/focus/default` | `Color/Brand/500` | `#4F46E5` | Sin uso en el canvas |
| `Button/primary/text` | `text/inverse` | `Color/Neutral/00` | `#FFFFFF` | En el canvas |
| `Button/primary/icon` | `icon/inverse` | `Color/Neutral/00` | `#FFFFFF` | En el canvas |
| `Button/primary/background/default` | `action/primary/default` | `Color/Brand/500` | `#4F46E5` | En el canvas |
| `Button/primary/background/hover` | `action/primary/hover` | `Color/Brand/600` | `#4338CA` | Sin uso en el canvas |
| `Button/primary/background/pressed` | `action/primary/pressed` | `Color/Brand/700` | `#3730A3` | Sin uso en el canvas |
| `Button/primary/background/disabled` | `action/primary/disabled` | `Color/Neutral/300` | `#9CA3AF` | Sin uso en el canvas |
| `Button/secondary/text` | `text/primary` | `Color/Neutral/900` | `#0F172A` | En el canvas |
| `Button/secondary/icon` | `icon/primary` | `Color/Neutral/900` | `#0F172A` | En el canvas |
| `Button/secondary/border/default` | `border/default` | `Color/Neutral/300` | `#9CA3AF` | En el canvas |
| `Button/secondary/border/hover` | `border/strong` | `Color/Neutral/500` | `#64748B` | Sin uso en el canvas |
| `Button/secondary/border/pressed` | `border/brand` | `Color/Brand/500` | `#4F46E5` | Sin uso en el canvas |
| `Button/secondary/background/default` | `background/primary` | `Color/Neutral/00` | `#FFFFFF` | En el canvas |
| `Button/secondary/background/hover` | `background/secondary` | `Color/Neutral/50` | `#F3F4F6` | Sin uso en el canvas |
| `Button/secondary/background/pressed` | `background/tertiary` | `Color/Neutral/100` | `#E5E7EB` | Sin uso en el canvas |
| `Button/secondary/background/disabled` | `action/secondary/disabled` | `Color/Neutral/300` | `#9CA3AF` | Sin uso en el canvas |
| `Button/icon-secondary/icon/default` | `icon/primary` | `Color/Neutral/900` | `#0F172A` | En el canvas |
| `Button/icon-secondary/icon/disabled` | `icon/disabled` | `Color/Neutral/400` | `#94A3B8` | Sin uso en el canvas |
| `Button/icon-secondary/border/default` | `border/default` | `Color/Neutral/300` | `#9CA3AF` | En el canvas |
| `Button/icon-secondary/border/hover` | `border/strong` | `Color/Neutral/500` | `#64748B` | Sin uso en el canvas |
| `Button/icon-secondary/border/focus` | `border/brand` | `Color/Brand/500` | `#4F46E5` | Sin uso en el canvas |
| `Button/icon-secondary/border/disabled` | `border/subtle` | `Color/Neutral/200` | `#D1D5DB` | Sin uso en el canvas |
| `Button/icon-secondary/background/default` | `background/primary` | `Color/Neutral/00` | `#FFFFFF` | En el canvas |
| `Button/icon-secondary/background/hover` | `background/secondary` | `Color/Neutral/50` | `#F3F4F6` | Sin uso en el canvas |
| `Button/icon-secondary/background/pressed` | `background/tertiary` | `Color/Neutral/100` | `#E5E7EB` | Sin uso en el canvas |
| `Button/icon-secondary/background/disabled` | `background/secondary` | `Color/Neutral/50` | `#F3F4F6` | Sin uso en el canvas |
| `Button/link/text/default` | `text/brand/default` | `Color/Brand/500` | `#4F46E5` | En el canvas |
| `Button/link/text/hover` | `text/brand/hover` | `Color/Brand/600` | `#4338CA` | Sin uso en el canvas |
| `Button/link/text/pressed` | `text/brand/pressed` | `Color/Brand/700` | `#3730A3` | Sin uso en el canvas |
| `Button/link/text/disabled` | `text/disabled` | `Color/Neutral/400` | `#94A3B8` | Sin uso en el canvas |
| `Button/link/icon/default` | `action/primary/default` | `Color/Brand/500` | `#4F46E5` | En el canvas |
| `Button/link/icon/hover` | `action/primary/hover` | `Color/Brand/600` | `#4338CA` | Sin uso en el canvas |
| `Button/link/icon/pressed` | `action/primary/pressed` | `Color/Brand/700` | `#3730A3` | Sin uso en el canvas |
| `Button/link/icon/disabled` | `action/primary/disabled` | `Color/Neutral/300` | `#9CA3AF` | Sin uso en el canvas |
| `Button/icon-secondary/SM/padding` — Padding simétrico (4 lados) para el botón solo-ícono, para garantizar contenedor cuadrado. Distinto de Button/*/padding-x que es asimétrico (pensado para label de texto). | `Space/XXS` | — | `4px` | En el canvas |
| `Button/icon-secondary/MD/padding` — Padding simétrico (4 lados) para el botón solo-ícono, para garantizar contenedor cuadrado. Distinto de Button/*/padding-x que es asimétrico (pensado para label de texto). | `Space/XS` | — | `8px` | En el canvas |
| `Button/icon-secondary/LG/padding` — Padding simétrico (4 lados) para el botón solo-ícono, para garantizar contenedor cuadrado. Distinto de Button/*/padding-x que es asimétrico (pensado para label de texto). | `Space/S` | — | `12px` | En el canvas |

## Hallazgos de la auditoría

⚠️ **El component set no modela estados.** Hay 22 tokens de hover, pressed, disabled o focus definidos, pero ninguna variante los usa. En código se aplican como estados del navegador; en Figma no se pueden previsualizar.

⚠️ **3 tokens sin uso en el canvas:** `Button/SM/icon-size`, `Button/MD/icon-size`, `Button/LG/icon-size`.

✅ **31 de 56 tokens** se usan en el canvas, directo o a través de su alias.

⚠️ **3 de 56 tokens tienen descripción** en Figma (5%). El principio 4 pide el 100%.

## Accesibilidad

- **Rol**: usar elemento nativo `<button>` (no `div` + `role="button"`), así el navegador resuelve foco y activación por teclado gratis.
- **Contraste verificado** (WCAG AA, texto normal ≥4.5:1):
  - `primary` texto blanco sobre `#4F46E5` (default) → **≈6.5:1**, pasa AA.
  - `secondary` texto `#0F172A` sobre `#FFFFFF` → **≈17.9:1**, pasa AA y AAA.
  - `primary` disabled: blanco sobre `#9CA3AF` → **≈2.5:1**, no pasa AA. WCAG exime a controles deshabilitados de este criterio (no transmiten información activa), así que es aceptable, pero conviene confirmarlo como decisión consciente y no accidental.
- **✅ Bug corregido — color del ícono no estaba atado a los tokens**: el ícono real (instancia de `Check`, librería Simple Design System) tenía su stroke bindeado a la variable de color **interna de la librería** (`#1E1E1E`, gris fijo), no a `Button.{variant}.icon`. Pasaba desapercibido a simple vista pero rompía el contrato: en `primary`, daba un ícono oscuro sobre fondo índigo con contraste real de **≈2.57:1** — muy por debajo del mínimo de 3:1 para elementos gráficos (WCAG 1.4.11). Se corrigió bindeando el stroke del vector interno del ícono (no el nodo instancia, el vector hijo) a `Button.{variant}.icon` en las 12 variantes (9 de Button + 3 de Icon-Secondary). Como el ícono ahora comparte color con el texto de cada variante, el contraste verificado arriba para texto aplica igual al ícono.
- **Foco visible**: resuelto para `primary`, `secondary` e `icon-secondary` vía `Button.border.focus`. Para `link`, que no tiene contenedor con borde, la recomendación es implementar el foco como `outline` de CSS (no `border`) reutilizando el mismo color `action/focus/default` (`#4F46E5`) — no necesita un token nuevo, ese semántico ya existe y se puede consumir directo en el CSS del link sin pasar por un token de componente dedicado.
- **`icon-secondary` sin label visible**: requiere `aria-label` obligatorio en implementación; no hay forma de derivarlo de los tokens, es responsabilidad de quien instancia el componente.
- **Teclado**: con `<button>` nativo, Enter y Space activan por defecto. No requiere JS adicional.

## Ejemplos de uso

**Correcto**: un botón `primary` por vista/formulario para la acción principal ("Guardar cambios"), acompañado de un `secondary` para la acción alternativa ("Cancelar").

**Incorrecto**: dos o más botones `primary` compitiendo por atención en la misma pantalla — diluye la jerarquía visual que la variante está diseñada para comunicar.

## Notas de implementación

- ✅ Namespace unificado bajo `Button.*` (color, radius, tamaños, tipografía y focus conviven en un solo árbol). Casing consistente en todo el componente.
- ✅ Los 4 gaps originales (radius, spacing, focus, tipografía) cerrados con Variables reales, valores exactos a lo propuesto en cada iteración.
- ✅ `icon-secondary` ahora usa padding simétrico dedicado (`Button.icon-secondary.{SM|MD|LG}.padding`), no el padding asimétrico compartido con las variantes de texto — garantiza contenedor cuadrado 1:1. Ver tabla de tokens de `icon-secondary` arriba.
- ✅ Sistema de íconos resuelto: librería **Simple Design System**, íconos como instancias reales (no placeholders), escalado uniforme por tamaño, y **color bindeado al token correcto por variante** (bug corregido — venían con el color interno fijo de la librería, no atado a nuestros tokens; ver Accesibilidad).
- Único punto abierto, no bloqueante: si existe o hace falta una variante `destructive`/`danger` (los primitivos de rojo existen en el sistema pero ningún token de `Button` los referencia todavía).
- `Button` está listo para pasar a implementación en código con esta doc como contrato.