---
componente: Button
estado: aprobado
escrito_por: copilot
revisado_por: Fede
nodos_figma: [5:26, 5:33]
huella_figma: fa994da7
---

## Descripción

Elemento interactivo que dispara una acción. El sistema define 4 variantes con distinto nivel de énfasis visual: `primary`, `secondary`, `icon-secondary` (solo ícono) y `link` (texto, sin contenedor).

**Usar cuando**: hay una acción que el usuario puede ejecutar (enviar, guardar, confirmar, navegar).
**No usar cuando**: es solo navegación entre páginas con URL propia → usar un componente de link/anchor semántico en su lugar (no un botón estilizado como link).

## Anatomía

1. Contenedor (background + borde opcional según variante)
2. Label de texto (ausente en `icon-secondary`)
3. Ícono (opcional en `primary`/`secondary`/`link`; obligatorio en `icon-secondary`) — implementado en Figma como property `Icon` (instance-swap) de la librería **Simple Design System**. Default: `Check`. Cualquier otro ícono de esa librería es válido como alternativa. Ver sección 4.0 de `DESIGN-SYSTEM.md` para la convención completa.

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
