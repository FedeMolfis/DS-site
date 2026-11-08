---
componente: Input
estado: aprobado
escrito_por: copilot
revisado_por: Fede
nodos_figma: [24:52]
huella_figma: ed630cd1
---

## Descripción

Campo de texto de una sola línea para captura de datos del usuario.

**Usar cuando**: se necesita texto libre (nombre, email, búsqueda, cualquier valor no acotado a opciones predefinidas).
**No usar cuando**: las opciones son un conjunto cerrado y pequeño (usar Checkbox/Radio) o es una acción inmediata sin captura de datos (usar Switch/Button).

## Anatomía

1. Label (arriba del campo)
2. Field (contenedor: borde + fondo + texto tipeado o placeholder)
3. Helper text (debajo del campo — mensaje neutro, de error o de éxito, según el estado)

Sin ícono — no se identificó un caso de uso que lo requiera en esta iteración (a diferencia de Button). Se puede sumar como slot leading/trailing más adelante si aparece la necesidad (ej. ícono de búsqueda, ícono de mostrar/ocultar contraseña).

## Accesibilidad

- **Contraste**: `text/primary` (#0F172A) sobre `background/primary` (#FFFFFF) ≈17.9:1. `border/default` (#9CA3AF) sobre blanco es un borde, no texto — no aplica el mismo umbral, pero es lo bastante oscuro para distinguirse de un fondo blanco.
- **No depender solo del color para error/success**: el helper text cambia de contenido (no solo de color) en `Error`/`Success` — el color refuerza, no es el único portador de información.
- **Foco visible**: reforzado con doble señal (color + grosor de borde 1px→2px), más allá de lo mínimo.
- **Label asociado**: en implementación, usar `<label for>` o envolver el input — el label visual en Figma no reemplaza la asociación semántica real.
- **Mensajes de error anunciados**: en implementación, el helper text en estado `Error` debería tener `aria-describedby` apuntando al input, y considerar `role="alert"` si el error aparece dinámicamente tras submit.

## Ejemplos de uso

**Correcto**: un campo "Email" con helper text fijo ("Nunca lo compartimos") que cambia a `Error` con mensaje específico ("Formato de email inválido") solo tras intentar enviar el formulario.

**Incorrecto**: mostrar `Error` mientras el usuario todavía está escribiendo el primer caracter — valida on-blur o on-submit, no on-keystroke, para no penalizar a alguien que todavía no terminó de tipear.

## Notas de implementación

- **Ancho**: en Figma el componente está a 280px fijo — es una referencia de lienzo, no un token ni una restricción real (regla 5.1 del doc maestro). En implementación, el ancho por defecto es fluido (`100%` del contenedor), salvo que un caso puntual necesite un `max-width` explícito.
- ✅ 6 tokens de geometría/tipografía del value creados, reutilizando primitivos existentes (cero valores nuevos inventados).
- ✅ Text Style `Body/SM` promovido a compartido (Switch + Input) — aplica la regla 6.7 del doc maestro en su primer caso real de uso.
- ✅ Text Style `Body/MD` creado para el texto de valor del campo — los 3 textos del componente (Label, Value, Helper) quedan consolidados en Text Styles, ninguno con binding manual por nodo.
- Sin ícono en esta iteración — evaluar si hace falta un slot leading/trailing cuando aparezca un caso de uso concreto (búsqueda, contraseña).
- `Error`/`Success` modelados como estados de `State` independientes de hover/focus — en código, estos se combinan (ej. un input en foco Y en error), no son mutuamente excluyentes como en las variantes de Figma.
