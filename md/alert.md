# Componente: Alert

## Descripción

Mensaje persistente e inline (no flotante) que comunica el resultado de una acción o el estado de una sección de la página. A diferencia de Toast, no desaparece solo — permanece hasta que el usuario lo descarta o cambia el contexto que lo generó.

**Usar cuando**: hay información de estado relevante para el contexto actual de la página (ej. "Tu suscripción vence en 3 días" arriba de una sección de facturación).
**No usar cuando**: es una confirmación breve de una acción puntual del usuario (ej. "Guardado con éxito") — ahí Toast comunica mejor porque no requiere descarte manual y no compite por espacio permanente en el layout.

## Anatomía

1. Ícono (leading, indica la severidad — obligatorio, no opcional como en Button)
2. Columna de texto: Title (obligatorio) + Description (opcional, togglable)

Sin botón de cierre en esta iteración — se puede sumar como slot opcional más adelante si aparece el caso de uso (alert descartable por el usuario).

## Variantes y properties

### Alert · 4 variantes

| Property | Tipo | Valores |
|---|---|---|
| `Title` | Texto | — |
| `Description` | Texto | — |
| `Show Description` | Booleano | — |
| `Type` | Variante | `Success`, `Warning`, `Danger`, `Info` |


## Tokens que consume

| Token | Apunta a | Primitivo | Valor | En el canvas |
|---|---|---|---|---|
| `Alert/title` | `text/primary` | `Color/Neutral/900` | `#0F172A` | En el canvas |
| `Alert/description` | `text/secondary` | `Color/Neutral/700` | `#334155` | En el canvas |
| `Alert/background/success` | `Status/success/background` | `Color/UI/success/50` | `#ECFDF5` | En el canvas |
| `Alert/background/warning` | `Status/warning/background` | `Color/UI/warning/50` | `#FFFBEB` | En el canvas |
| `Alert/background/danger` | `Status/danger/background` | `Color/UI/danger/50` | `#FEF2F2` | En el canvas |
| `Alert/background/info` | `Status/info/background` | `Color/UI/information/50` | `#EFF6FF` | En el canvas |
| `Alert/border/success` | `Status/success/content` | `Color/UI/success/700` | `#047857` | En el canvas |
| `Alert/border/warning` | `Status/warning/content` | `Color/UI/warning/700` | `#B45309` | En el canvas |
| `Alert/border/danger` | `Status/danger/content` | `Color/UI/danger/700` | `#B91C1C` | En el canvas |
| `Alert/border/info` | `Status/info/content` | `Color/UI/information/700` | `#1D4ED8` | En el canvas |
| `Alert/icon/success` | `Status/success/content` | `Color/UI/success/700` | `#047857` | En el canvas |
| `Alert/icon/warning` | `Status/warning/content` | `Color/UI/warning/700` | `#B45309` | En el canvas |
| `Alert/icon/danger` | `Status/danger/content` | `Color/UI/danger/700` | `#B91C1C` | En el canvas |
| `Alert/icon/info` | `Status/info/content` | `Color/UI/information/700` | `#1D4ED8` | En el canvas |
| `Alert/radius` — Reutiliza radius/action (mismo semántico que Button/Input). | `radius/action` | `radius/M` | `8px` | En el canvas |
| `Alert/padding` — 16px, simétrico en los 4 lados (contenedor tipo card, no hay runway de texto que justifique asimetría). Reutiliza Space/M. | `Space/M` | — | `16px` | En el canvas |
| `Alert/gap-icon-text` — 8px entre ícono y columna de texto. Reutiliza Space/XS. | `Space/XS` | — | `8px` | En el canvas |
| `Alert/gap-title-description` — 4px entre título y descripción. Reutiliza Space/XXS. | `Space/XXS` | — | `4px` | En el canvas |
| `Alert/icon/size` — 20px, reutiliza typography/action/MD/icon-size — mismo tamaño de ícono "medio" que ya usa Button. | `typography/action/MD/icon-size` | `Typography/line-height/sm` | `20px` | Sin uso en el canvas |

## Hallazgos de la auditoría

⚠️ **1 token sin uso en el canvas:** `Alert/icon/size`.

✅ **18 de 19 tokens** se usan en el canvas, directo o a través de su alias.

⚠️ **5 de 19 tokens tienen descripción** en Figma (26%). El principio 4 pide el 100%.

## Accesibilidad

- **Contraste verificado** (WCAG AA, texto normal ≥4.5:1, calculado sobre los pares reales):
  - `Title`/`Description` (`#0F172A`/gris) sobre cualquiera de los 4 fondos tintados → contraste muy alto en los 4 casos, los fondos son casi blancos.
  - Ícono+borde `success`: `#047857` sobre `#ECFDF5` → **≈5.2:1**, pasa AA.
  - Ícono+borde `warning`: `#B45309` sobre `#FFFBEB` → **≈4.84:1**, pasa AA con el margen más ajustado de las 4 — si el primitivo de warning cambia, revisar de nuevo.
  - Ícono+borde `danger`: `#B91C1C` sobre `#FEF2F2` → **≈5.92:1**, pasa AA.
  - Ícono+borde `info`: `#1D4ED8` sobre `#EFF6FF` → **≈6.85:1**, pasa AA.
- **No depender solo del color**: cada variante tiene un ícono distinto por tipo (no el mismo ícono recoloreado) — el `Check` de success es visualmente distinto del triángulo de warning, así que la información no depende únicamente del color de fondo/borde.
- **Rol y semántica**: implementar con `role="alert"` cuando el contenido aparece dinámicamente (ej. tras una acción del usuario) para que se anuncie automáticamente; `role="status"` si es informativo y no urgente. No usar `role="alert"` para contenido que ya está en el DOM al cargar la página — solo para inserciones dinámicas.
- **Ícono decorativo**: como el tipo ya se comunica por texto (Title/Description) y color/forma, el ícono debe llevar `aria-hidden="true"` en implementación — es reforzante, no la única fuente de información.

## Ejemplos de uso

**Correcto**: un alert `warning` arriba de un formulario de checkout ("Tu método de pago vence pronto") que permanece visible mientras el problema no se resuelva.

**Incorrecto**: usar Alert para confirmar "Cambios guardados" tras cada click en un botón — eso genera fatiga visual porque no desaparece solo; ahí corresponde Toast.

## Notas de implementación

- ✅ Ícono bindeado correctamente al vector interno desde el arranque en las 4 variantes (regla 6.9 del doc maestro aplicada sin repetir el bug de Button).
- ✅ Sizing modes verificados por código antes de dar el componente por bueno (lección de Input, regla 6.8) — `AUTO`/`HUG` reales, no `FIXED` atascado.
- Sin botón de descarte en esta iteración — evaluar si hace falta cuando aparezca un caso de uso concreto de alert descartable.
- `type` no tiene default — a diferencia de Badge (`neutral` por default), un Alert sin tipo definido no tiene mucho sentido de uso, así que se deja como prop requerida.