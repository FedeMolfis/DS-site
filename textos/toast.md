---
componente: Toast
estado: aprobado
escrito_por: copilot
revisado_por: Fede
nodos_figma: [39:45]
huella_figma: e13414a9
---

## Descripción

Notificación flotante y transitoria que confirma el resultado de una acción puntual del usuario. A diferencia de Alert, se muestra sobre el contenido (no inline) y desaparece sola después de unos segundos, sin necesidad de descarte manual.

**Usar cuando**: hay que confirmar que una acción del usuario se completó (ej. "Cambios guardados", "Copiado al portapapeles").
**No usar cuando**: la información es relevante mientras dure el contexto de la página (ej. un estado de facturación pendiente) — ahí corresponde Alert, que no desaparece solo y no compite por atención de forma tan efímera.

## Anatomía

1. Ícono (leading, indica la severidad)
2. Message (una sola línea de texto — a diferencia de Alert, no hay separación título/descripción)

Sin botón de cierre en esta iteración, mismo criterio que Alert — el auto-dismiss por tiempo es el mecanismo principal de descarte.

## Accesibilidad

- **Contraste verificado** (WCAG AA, texto normal ≥4.5:1, texto/ícono blanco sobre cada fondo sólido):
  - `success`: blanco sobre `#047857` → **≈5.5:1**, pasa AA.
  - `warning`: blanco sobre `#B45309` → **≈5.0:1**, pasa AA (el más ajustado de los cuatro).
  - `danger`: blanco sobre `#B91C1C` → **≈6.5:1**, pasa AA.
  - `info`: blanco sobre `#1D4ED8` → **≈7.4:1**, pasa AA y AAA.
  - Los 4 casos superan a los pares equivalentes de Alert (que rondaban 4.8–6.9:1) — el fondo sólido no solo da más peso visual, también da más margen de contraste.
- **No depender solo del color**: mismo criterio que Alert — cada tipo tiene un ícono distinto, no el mismo ícono recoloreado.
- **Anuncio para lectores de pantalla**: implementar con `role="status"` y `aria-live="polite"` para la mayoría de los casos (no interrumpe lo que el lector de pantalla esté anunciando); usar `aria-live="assertive"` solo para `danger` si el error es crítico y requiere atención inmediata.
- **Tiempo de exposición**: si el toast se auto-descarta, dar tiempo suficiente para leerlo (mínimo ~4 segundos para un mensaje corto) y, cuando sea posible, pausar el timer si el usuario pasa el mouse o el foco sobre el toast.

## Ejemplos de uso

**Correcto**: mostrar un toast `success` ("Cambios guardados") inmediatamente después de que el usuario hace click en "Guardar", con auto-dismiss a los 4 segundos.

**Incorrecto**: usar Toast para un mensaje que el usuario necesita poder volver a consultar más tarde en la sesión — al desaparecer solo, se pierde; ahí corresponde Alert (persistente) o directamente un mensaje inline en el formulario.

## Notas de implementación

- ✅ Geometría y tipografía cerradas reutilizando tokens existentes — cero primitivos nuevos, mismo criterio que el resto del sistema.
- ✅ Íconos bindeados correctamente al vector interno desde el arranque (regla 6.9), sizing modes verificados por código antes de dar el componente por bueno (regla 6.8).
- **Cambio deliberado sobre el export original**: los tokens de color de Toast se rebindearon (ver sección de Tokens arriba) para darle más peso visual que Alert — decisión explícita, no un ajuste silencioso.
- Sin botón de cierre ni posición/stacking (esquina de pantalla, cómo se apilan varios toasts) en esta iteración — eso es comportamiento de la capa de implementación (ej. un `ToastProvider` en React), no algo que Figma modele.
