# Auditoría de accesibilidad, UX y diseño responsive

## 1. Resumen ejecutivo

El formulario presenta una base sólida desde la perspectiva de accesibilidad y semántica: usa `main`, `form`, etiquetas asociadas mediante `for`, `input` con validación nativa y mensajes de error con `aria-live`. La estructura es clara y entendible para lectores de pantalla, y la navegación con teclado está asegurada por el uso de controles nativos.

Se detectaron dos hallazgos relevantes de nivel medio y ninguno crítico o alto:

- Contraste insuficiente en mensajes de validación de error y éxito.
- Objetivos táctiles pequeños en inputs y botón para móviles.

No se detectaron errores de sintaxis en JavaScript y no existen imágenes que requieran `alt` en la implementación actual.

### Criterios que cumplen

- Estructura semántica: cumple.
- Jerarquía de encabezados: cumple (`h1` único y apropiado).
- Nombres accesibles: cumple, porque cada control tiene etiqueta asociada.
- Textos alternativos (`alt`): cumple, porque no hay elementos `img` ni `svg` decorativos que requieran alternativos.
- Navegación mediante teclado: cumple, porque los controles son nativos y enfocables.
- Foco visible: cumple en los inputs de texto, con `outline` visible en `:focus`.
- Uso correcto de botones y enlaces: cumple, porque hay un botón de envío y no hay enlaces engañosos ni ambiguos.
- Uso adecuado de ARIA: cumple, con `aria-live="polite"` en los mensajes de validación.
- Atributos `required` y validación nativa: cumple.
- Posibles problemas de overflow horizontal: no se detectan por código, principalmente por el uso de `box-sizing: border-box` y la ausencia de anchos fijos.

## 2. Hallazgos críticos, altos, medios y bajos

### Críticos

- Ninguno detectado con la evidencia disponible.

### Altos

- Ninguno detectado con la evidencia disponible.

### Medios

1. Contraste insuficiente en mensajes de error y éxito.
   - Impacto: afecta la legibilidad y no cumple WCAG 2.2 AA para texto normal.
   - Severidad: media.

2. Objetivos táctiles pequeños para móvil.
   - Impacto: inputs y botón pueden requerir más ancho/alto para una interacción cómoda en pantallas pequeñas.
   - Severidad: media.

### Bajos

- Ninguno detectado con base suficiente en el código y en las pruebas realizadas.

## 3. Evidencia concreta

### Hallazgo 1: contraste insuficiente en mensajes de error y éxito

- Archivo: `styles.css`
- Elementos afectados: `.mensaje-error`, `.mensaje-exito`
- Evidencia concreta:
  - `.mensaje-error { color: #d64545; font-size: .85rem; min-height: 1.1em; }`
  - `.mensaje-exito { color: #1f9d55; font-weight: 600; }`
- Cálculo de contraste con fondo blanco:
  - `#d64545` sobre `#ffffff`: 4.38:1
  - `#1f9d55` sobre `#ffffff`: 3.49:1
- Resultado:
  - El texto de error no cumple WCAG 2.2 AA para texto normal (mínimo 4.5:1).
  - El texto de éxito tampoco cumple WCAG 2.2 AA para texto normal.

### Hallazgo 2: objetivos táctiles pequeños para móvil

- Archivo: `styles.css`
- Elementos afectados: `input[type="text"], input[type="email"], input[type="tel"], input[type="password"]` y `button[type="submit"]`
- Evidencia concreta:
  - `padding: 8px;` en inputs.
  - `padding: 10px 16px;` en botón.
  - No se define `min-height: 44px` en los controles.
- Resultado:
  - En pantallas móviles, los controles pueden quedar por debajo del tamaño recomendado para un objetivo táctil cómodo (44x44 px).

## 4. Recomendación de corrección

### Hallazgo 1: contraste insuficiente

- Ajustar el color del texto de error y éxito para lograr un contraste mínimo de 4.5:1 sobre fondo blanco.
- Una solución práctica es usar tonos más oscuros para rojo y verde, por ejemplo tonos de rojo/verde con relación de contraste validada.
- Mantener el mismo significado semántico del estado, pero mejorando la legibilidad del mensaje para usuarios con baja visión.

### Hallazgo 2: objetivos táctiles pequeños

- Definir un `min-height` mínimo de 44px para inputs y el botón de envío.
- Aumentar el padding de los controles para facilitar la interacción en móvil.
- Revisar el espaciado horizontal y vertical del formulario para que sea cómodo en resoluciones de 320 px y 398 px.

## 5. Pruebas que deberían repetirse después de corregir los problemas

1. Verificar contraste de textos en todos los estados del formulario:
   - error,
   - éxito,
   - texto base,
   - foco y hover.
2. Reproducir la experiencia en navegador con estas resoluciones:
   - 320 px,
   - 398 px,
   - 768 px,
   - escritorio (resolución estándar).
3. Validar la navegación con teclado:
   - Tab desde el primer campo hasta el botón de envío,
   - comprobar visibilidad del foco,
   - asegurar que los mensajes de error se lean de forma clara con lector de pantalla.
4. Comprobar que no haya overflow horizontal en móvil y tablet.
5. Revisar interacción en dispositivos táctiles para confirmar que los controles cumplen tamaño y distancia recomendados.
6. Verificar que el flujo de validación se comporte igual si el usuario llena y corrige campos en varias ocasiones.
7. Validar la sintaxis de JavaScript nuevamente después de cualquier cambio:
   - `node --check app.js`

## Observaciones finales

- El formulario cumple con la base de accesibilidad general y no presenta errores de sintaxis evidentes.
- La principal oportunidad de mejora está en contraste de mensajes y tamaño táctil para móvil.
- No se detectaron problemas de `alt` ni de estructura semántica que requieran corrección inmediata.
