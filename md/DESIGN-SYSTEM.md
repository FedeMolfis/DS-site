# Design System — Documento Maestro

> Este documento es la fuente de verdad del design system. Está escrito para dos audiencias a la vez: personas (diseñadores, developers, PMs) y agentes de IA que necesiten leer, razonar o generar código a partir de él. Por eso la estructura es fija, predecible y evita ambigüedad.

## 0. Cómo leer este documento

- **Humanos**: leer de corrido, especialmente las secciones 1, 2 y 5.
- **IA / agentes**: cada token y cada componente tiene una estructura de datos fija (ver `tokens/*.json` y `component-template.md`). No infieras valores: si un dato no está en el JSON o en la doc del componente, no existe todavía.
- **Fuente de verdad**: Figma (Variables) → export a JSON → este repo. Nunca al revés. Si algo en código no coincide con Figma, gana Figma hasta que se sincronice.

---

## 1. Principios

1. **Una sola fuente de verdad**: los valores viven en Figma Variables. Todo lo demás (JSON, CSS, código) se genera o sincroniza a partir de eso.
2. **Jerarquía estricta**: `primitivo → semántico → componente`. Nunca al revés, nunca se saltea un nivel sin justificación documentada.
3. **Nombra por intención, no por apariencia**. `color.text.danger`, no `color.text.red`.
4. **Todo token tiene descripción**. Un valor sin descripción de propósito es información incompleta, tanto para una persona nueva en el equipo como para una IA.
5. **Nada se borra, se deprecia**. Los tokens tienen contrato con el código que los consume; eliminarlos rompe cosas silenciosamente.

---

## 2. Arquitectura de tokens (3 capas)

### 2.1 Primitivos (`primitive`)

Son la paleta cruda: valores sin significado de uso. Nunca se consumen directo en un componente ni en UI final.

```
color.blue.500        = #2563EB
color.gray.100         = #F3F4F6
spacing.4              = 16px
font-size.100          = 14px
radius.200              = 8px
```

**Regla de naming**: `{categoría}.{escala}` — escalas numéricas (50, 100, 200... 900) para permitir insertar valores intermedios a futuro sin romper naming.

### 2.2 Semánticos (`semantic`)

Le dan propósito a un primitivo. Siempre **referencian** un primitivo (nunca un valor hardcodeado nuevo).

```
color.background.primary   → {color.blue.500}
color.text.danger          → {color.red.600}
color.border.subtle        → {color.gray.200}
spacing.inset.md           → {spacing.4}
typography.action.md.font-size → {typography.size.sm}
```

**Nota sobre categorías por rol** (ej. `action`): cuando un conjunto de valores se repite entre varios componentes interactivos — tamaños de tipografía en Button, Input, Tabs; estados de color en cualquier control accionable — conviene agruparlo bajo un rol semántico compartido (`action.*`, `typography.action.*`) en vez de definirlo suelto dentro de un solo componente. Así se define una vez y todos los componentes alias contra lo mismo.

**Regla de naming**: `{categoría}.{rol}.{variante}` — el nombre describe *para qué se usa*, no de qué color es. Si mañana `color.text.danger` cambia de rojo a naranja, el nombre sigue siendo correcto.

### 2.3 Componente (`component`)

Tokens específicos de un componente puntual. Referencian preferentemente un semántico; si no existe un semántico aplicable, se referencia un primitivo (y se evalúa si vale la pena crear el semántico).

```
button.background.default   → {color.background.primary}
button.background.hover     → {color.background.primary.hover}
button.text.default         → {color.text.on-primary}
card.padding                → {spacing.inset.md}
```

**Regla de naming**: `{componente}.{propiedad}.{variante}.{estado?}` — el estado va al final (`default`, `hover`, `pressed`, `disabled`, `focus`).

### 2.4 Regla de dependencia (no negociable)

```
primitivo  →  semántico  →  componente
```

Un componente **nunca** referencia un primitivo directo si existe un semántico equivalente. Un semántico **nunca** trae un valor hardcodeado. Esto es lo que permite hacer un rebrand cambiando solo la capa semántica, sin tocar componentes.

---

## 3. Formato de exportación (a definir/implementar)

Como todavía no hay export, la recomendación es usar el estándar **W3C Design Tokens Community Group (DTCG)**, porque:

- Es JSON tipado (`$type`, `$value`, `$description`), no un formato propietario.
- Lo entienden herramientas como Tokens Studio, Style Dictionary, y cualquier pipeline de CI que quieras armar después.
- Es el formato más fácil de parsear para una IA: estructura predecible, sin necesidad de inferir nada.

### 3.1 Camino sugerido

1. Instalar el plugin **Tokens Studio for Figma** y conectarlo a las Variables ya creadas (o migrar las Variables actuales a Tokens Studio si preferís seguir editando ahí).
2. Configurar el export en 3 archivos separados, reflejando las 3 capas:
   - `tokens/primitive.json`
   - `tokens/semantic.json`
   - `tokens/component.json`
3. Cada token exportado debe incluir `$description`. Esto no es opcional: es lo que convierte un JSON de valores en un JSON legible por una IA sin contexto adicional.
4. (Opcional, cuando haya código) usar **Style Dictionary** para transformar estos JSON en CSS variables, TS, etc.

Ver `tokens/tokens.example.json` en este mismo paquete para un ejemplo concreto de las 3 capas con sintaxis de referencia (`{color.blue.500}`).

---

## 4. Documentación de componentes

### 4.0 Sistema de íconos

Fuente única de íconos: librería de Figma **Simple Design System** (community, licencia CC BY 4.0). Ningún componente crea íconos custom por fuera de esta librería — evita inconsistencias de grosor de trazo, grid y aspect ratio entre componentes.

- Cada ícono es un componente individual (no un sprite ni un set con variantes de tamaño) — el grid nativo suele ser 20×20px.
- Para usarlo en otro tamaño (16px, 24px), se **escala la instancia de forma uniforme** (mismo valor en ancho y alto) — nunca se estira de forma no proporcional. Esto es lo que garantiza el 1:1 real; un ícono con proporciones distintas en ancho/alto es señal de que se aplicó mal, no de que la librería esté rota.
- Los componentes que llevan ícono exponen una property `Icon` de tipo **instance-swap** (no una property fija) para poder elegir cuál usar sin romper el binding a los demás tokens (color, tamaño, etc. siguen atados a las Variables del componente, no al ícono elegido).
- Ícono por defecto documentado por componente cuando aplica; cualquier otro ícono de la misma librería es válido como alternativa vía la property.

Cada componente se documenta con la **misma estructura exacta** (ver `component-template.md`). La consistencia estructural es lo que le permite a una IA leer cualquier componente sin tener que "aprender" un formato nuevo cada vez.

Campos obligatorios por componente:

- Descripción y propósito (cuándo usarlo / cuándo no)
- Anatomía (partes que lo componen)
- Variantes y props
- Estados (`default`, `hover`, `pressed`, `disabled`, `loading`, `focus`, `error`)
- Tokens que consume (lista explícita, no "los de siempre")
- Accesibilidad específica del componente
- Ejemplos de uso correcto / incorrecto (do/don't)

---

## 5. Accesibilidad (reglas transversales)

Estas reglas aplican a **todo** el sistema, además de lo que cada componente documente puntualmente:

- **Contraste**: mínimo WCAG AA — 4.5:1 para texto normal, 3:1 para texto grande (≥18px o ≥14px bold) y para elementos gráficos/UI.
- **Foco visible**: todo elemento interactivo tiene un estado `focus` con token propio (`{componente}.border.focus` o similar). Nunca se remueve el outline sin reemplazo.
- **Target táctil mínimo**: 44×44px para elementos interactivos en interfaces táctiles. **Se resuelve con hitbox expandida, no agrandando el componente visual** — el tamaño que se ve en Figma/diseño no cambia; en implementación, el área clickeable real (padding invisible, `::before`/pseudo-elemento, o el contenedor clickeable) se extiende hasta 44×44 aunque el elemento visual sea más chico (ej. el track de Switch, 40×24 visual, con hitbox de 44×44 en el `<label>` o wrapper que lo contiene).
- **No depender solo del color**: todo estado que se comunica con color (error, éxito, warning) debe tener un segundo indicador (ícono, texto, patrón).
- **Reduced motion**: toda animación debe tener una alternativa o desactivarse respetando `prefers-reduced-motion`.
- **Semántica**: cada componente documenta qué rol/ARIA le corresponde cuando se implemente en código.

### 5.1 Alcance de plataforma: web responsive (incluye mobile web)

No hay componentes ni tokens separados para mobile — es **un solo set de componentes** que responde. Esto tiene consecuencias concretas a tener en cuenta en cada componente nuevo:

- **Hover es progresivo, no un requisito**: el estado `hover` existe para dispositivos con puntero (mouse/trackpad). En touch no hay hover real — el navegador lo simula de forma inconsistente al primer tap. Ningún componente puede depender de `hover` para comunicar información necesaria; siempre tiene que haber un estado accesible sin él (esto ya lo cubre la regla "no depender solo del color", pero acá aplica también a la sola presencia del estado).
- **Anchos fijos en Figma son de referencia, no un token**: cuando un componente en Figma tiene un ancho fijo (ej. Input a 280px), es una decisión de lienzo para poder visualizarlo, no un valor a respetar en implementación. Salvo que el token documente explícitamente un `max-width`, el comportamiento por defecto en código es ancho fluido (`100%` del contenedor).
- **Target táctil, siempre 44×44 mínimo vía hitbox** (ver regla arriba) — aplica en mobile web igual que en una app nativa; no es exclusivo de contextos "táctiles" en sentido estricto, un trackpad/mouse en una laptop táctil también se beneficia.

---

## 6. Contribución y gobernanza

### 6.1 Cuándo crear un token nuevo

- **Primitivo nuevo**: solo si el valor no existe en la escala actual (ej: un nuevo color de marca). Requiere aprobación de diseño.
- **Semántico nuevo**: cuando un rol de uso se repite en 2+ lugares y no hay uno existente que lo cubra. Antes de crear uno, buscar si alguno existente aplica.
- **Token de componente**: se crea junto con el componente, y siempre debe poder trazarse a un semántico.

### 6.2 Versionado

- Semver a nivel del paquete de tokens: `MAJOR.MINOR.PATCH`.
  - `MAJOR`: se elimina o cambia el significado de un token existente.
  - `MINOR`: se agregan tokens nuevos.
  - `PATCH`: se ajusta un valor sin cambiar el nombre/contrato.

### 6.3 Deprecación

Nunca se borra un token en uso. Se marca:

```json
"button.background.legacy": {
  "$value": "{color.blue.500}",
  "$deprecated": "Usar button.background.default. Se elimina en v3.0.0"
}
```

### 6.4 Revisión

Todo token o componente nuevo pasa por: propuesta → revisión de diseño → revisión de accesibilidad → merge. (Ajustar este flujo al proceso real del equipo cuando exista.)

### 6.5 El nombre de la colección de Figma es parte del contrato

Cada alias entre capas (`semántico → primitivo`, `componente → semántico`) se guarda en el export con el **nombre exacto** de la colección de origen (`targetVariableSetName`), no solo el ID de la variable — y el ID no es portable entre archivos de Figma. Esto significa que:

- **Renombrar una colección en Figma rompe en silencio** todos los alias que apunten a ella desde otras colecciones. El error no aparece en el momento de renombrar, sino después, al reexportar o reimportar.
- Al migrar tokens a un archivo nuevo de Figma (o restaurar un export), la colección de destino tiene que llamarse **exactamente igual** (mayúsculas, espacios, todo) que en el archivo de origen — un import nativo de Variables puede fallar el 100% de los tokens de una capa si el nombre no coincide carácter por carácter, aunque los valores y la estructura sean correctos.
- Antes de renombrar cualquier colección existente, buscar qué otras colecciones la referencian como alias y actualizar en conjunto.

### 6.6 Padding compartido no siempre es reutilizable: texto+ícono vs. solo-ícono

Un padding pensado para un control con label de texto suele ser **asimétrico a propósito** (más espacio horizontal que vertical, para darle lugar al texto). Ese mismo padding aplicado a una variante **solo-ícono** rompe la forma cuadrada esperada — el contenedor sale rectangular en vez de 1:1.

Regla: cualquier variante solo-ícono (icon-only button, futuros icon-only de otros componentes) necesita su **propio token de padding simétrico** (mismo valor en las 4 direcciones), no reutilizar el padding-x/padding-y del componente base. Buena noticia: casi nunca hace falta un valor nuevo — alcanza con alias el mismo primitivo que ya usa el `padding-y` (el eje vertical, más chico) del componente base, aplicado a los 4 lados.

### 6.7 Tipografía se consolida en Text Styles, no en el nodo

Cuando un mismo trío `font-size` / `line-height` / `font-weight` se aplica a texto en más de un lugar (label de Button, futuro Input, futuro Tabs), no se bindea variable por variable en cada nodo de texto — se crea un **Text Style de Figma** (uno por escalón de la escala compartida, ej. `Action/SM`, `Action/MD`, `Action/LG`), bindeado a las Variables semánticas correspondientes, y los nodos de texto consumen el Text Style.

Por qué importa: bindear las 3 variables directo en cada nodo funciona, pero cambiar la tipografía o el weight del sistema entero implica tocar nodo por nodo — exactamente el problema que los tokens están para evitar. Un Text Style es el nivel de indirección que falta entre "la Variable cambió" y "todos los textos que la usan se actualizan solos".

Regla práctica: si vas a bindear tipografía en más de un componente con la misma escala semántica (ej. `typography.action.*`), primero creá el Text Style; no repitas el binding nodo por nodo.

### 6.8 Orden de operaciones: `resize()` pisa el sizing mode "hug"

Al construir con auto-layout, llamar `resize(ancho, alto)` **después** de fijar `primaryAxisSizingMode`/`counterAxisSizingMode` en `'AUTO'` (hug content) lo revierte silenciosamente a `'FIXED'` — sin error, sin warning. El frame queda con un alto/ancho literal en vez de ajustarse al contenido, aunque visualmente pueda no notarse hasta que se edita el contenido o se inspecciona el nodo.

Regla: si un eje debe quedar en "hug content", el `resize()` (si hace falta) va **antes** de fijar ese eje en `AUTO`, nunca después. Más seguro todavía: evitar `resize()` del todo en el eje que va a ser hug, y dejar que el auto-layout calcule el tamaño solo a partir de los hijos ya agregados.

### 6.9 Íconos de librerías externas traen su propio color — hay que sobrescribirlo explícitamente

Un ícono importado de una librería (ej. Simple Design System) no es un vector suelto: es un componente con su propia estructura interna, y el color vive en un nodo hijo (`Vector`/`Icon`) bindeado a **la variable de color interna de esa librería**, no a las nuestras. Insertar la instancia y solo ajustar tamaño/posición dejar el color de la librería puesto — visualmente puede pasar desapercibido si por casualidad se acerca al color esperado, pero no está atado al token del componente y rompe apenas cambie la variante (ej. mismo ícono oscuro en un botón `primary` con fondo de color, en vez de heredar el color de texto de esa variante).

Regla: después de crear la instancia del ícono, hay que **encontrar el nodo vectorial interno** (no la instancia raíz) y sobrescribir explícitamente su `fill` o `stroke` (según cómo esté construido el ícono — muchas librerías de íconos usan `stroke`, no `fill`) con el token de color del componente que lo contiene. No alcanza con bindear la instancia raíz: el color vive un nivel más adentro.

---

## 7. Por qué este documento es "AI-ready"

- **Naming predecible** (`{categoría}.{rol}.{variante}.{estado}`) → una IA puede inferir la estructura de un token que nunca vio.
- **Jerarquía explícita y con referencias** (`{color.blue.500}`) → no hay que adivinar de dónde sale un valor.
- **JSON tipado con descripción obligatoria** → cada token se explica a sí mismo.
- **Plantilla fija de componentes** → un agente puede generar código o QA de accesibilidad componente por componente sin reinterpretar formato.
- **Manifest único** (recomendado, ver más abajo) → punto de entrada claro para cualquier agente que llegue al repo por primera vez.

### Manifest sugerido (`manifest.json`, a crear cuando haya archivos reales)

```json
{
  "name": "nombre-del-design-system",
  "version": "0.1.0",
  "tokens": {
    "primitive": "tokens/primitive.json",
    "semantic": "tokens/semantic.json",
    "component": "tokens/component.json"
  },
  "components": "components/",
  "docs": "DESIGN-SYSTEM.md"
}
```

---

## 8. Próximos pasos sugeridos

1. Elegir herramienta de export (Tokens Studio recomendado) y conectar con las Variables ya creadas en Figma.
2. Exportar los 3 archivos JSON siguiendo `tokens.example.json` como referencia de formato.
3. Documentar 1-2 componentes con `component-template.md` como piloto, para validar que la plantilla cubre los casos reales.
4. Crear `manifest.json` una vez que existan los archivos reales.
5. (Más adelante) Style Dictionary u otro transformer si se necesita generar CSS/TS desde los JSON.
