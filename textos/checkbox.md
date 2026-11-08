---
componente: Checkbox
estado: aprobado
escrito_por: copilot
revisado_por: Fede
nodos_figma: [34:78]
huella_figma: a6291322
---

## Descripción

Control de selección binaria dentro de un grupo de opciones, con cambio pendiente de confirmación (a diferencia de Switch, que aplica al instante).

**Usar cuando**: el usuario puede seleccionar una o varias opciones de una lista, y el cambio se confirma después (ej. con un botón "Guardar" o "Enviar").
**No usar cuando**: la acción es binaria e inmediata sin paso de confirmación (usar Switch) o las opciones son mutuamente excluyentes dentro de un mismo grupo (usar Radio).

## Anatomía

1. Box (cuadrado redondeado — vacío si `Checked=false`, relleno con ícono de check si `Checked=true`)
2. Columna de texto: Label (obligatorio) + Helper text (opcional, togglable)

## Accesibilidad

- **Contraste**: `label` (#0F172A) sobre blanco ≈17.9:1. Ícono blanco sobre `background/selected` (#4F46E5) ≈6.5:1 — mismo par que ya se verificó en `Button.primary`.
- **✅ Bug corregido — foco invisible en `checked+focus`**: el primer intento aplicó `border/focus` como stroke directo sobre el box, pero `border/focus` y `background/selected` son el mismo color (`#4F46E5`) — el anillo se fundía con el relleno y quedaba imperceptible. Se corrigió envolviendo el box en un frame `FocusRing` separado (offset de 3px, stroke 2px), siguiendo el patrón estándar de anillo de foco desacoplado del elemento en vez de pegado a su borde. Verificado visualmente que ahora se distingue con claridad.
- **⚠️ Limitación documentada, no bloqueante — bajo contraste en `checked+disabled`**: `icon/disabled` (#94A3B8) sobre `action/primary/disabled` (#9CA3AF) da un contraste real de **≈1:1** — el check es casi imperceptible por color. WCAG exime a controles deshabilitados de este criterio (no transmiten información activa), así que es aceptable, pero es una decisión consciente a confirmar con diseño, no un descuido. Si se quiere mejorar legibilidad ahí, haría falta un token dedicado (ej. `Checkbox.icon.disabled-on-selected` con más contraste) — no se creó porque no hay pedido explícito de resolverlo.
- **Rol y teclado**: implementar con `<input type="checkbox">` nativo (no `div` + `role`); Space alterna el estado; el `label` debe estar asociado (`<label for>` o envolvente), no ser solo texto visual.
- **Grupos de checkboxes**: cuando se usan varios relacionados, agrupar con `<fieldset>` + `<legend>` para que el lector de pantalla anuncie el contexto del grupo.
- **Target táctil**: el box (16×16) está muy por debajo de 44×44 — **resuelto vía hitbox expandida** (regla 5.1 del doc maestro), igual que Switch: el `<label>`/wrapper clickeable debe garantizar el área mínima real, no el box en sí.

## Ejemplos de uso

**Correcto**: una lista de checkboxes para "Selecciona tus intereses" dentro de un formulario con botón "Guardar" al final — el cambio no se aplica hasta confirmar.

**Incorrecto**: usar Checkbox para una preferencia que se aplica al tocar, sin paso de confirmación — ahí Switch comunica mejor la inmediatez.

## Notas de implementación

- ✅ Ícono (`Check`, librería Simple Design System) bindeado correctamente al vector interno desde el arranque — se aplicó la lección de Button (regla 6.9 del doc maestro) sin repetir el bug.
- ✅ Foco corregido con anillo separado (offset), no pegado al borde del box — ver Accesibilidad.
- **Decisión de diseño**: `Checked=true, Hover` no tiene tratamiento visual distinto de `Checked=true, Default` — no hay token de color dedicado para ese cruce en el export. Aceptable porque el cambio de cursor ya comunica la interactividad; si se quiere un feedback visual adicional, es un token a sumar más adelante (ej. `Checkbox.background.selected-hover`).
- Componente listo para pasar a implementación en código con esta doc como contrato.
