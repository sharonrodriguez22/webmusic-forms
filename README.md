# webmusic-forms

Encuesta de investigación de mercado para **WebMusic**, la tienda online de instrumentos musicales. Es el TP 1.5 de Prácticas Profesionalizantes I (PPI), IFTS N.º 18.

La encuesta sirve para entender quién compra instrumentos, cómo compra hoy y qué le impide comprar online. Con esas respuestas se decide qué construir primero en la tienda ([webmusic-ecommerce](https://sharonrodriguez22.github.io/webmusic-ecommerce/)).

## Qué hay en el repo

Todo está en un solo archivo, [index.html](index.html), con el HTML, el CSS y el JavaScript juntos. No tiene dependencias ni build: solo carga las fuentes Playfair Display y DM Sans desde Google Fonts.

## Cómo funciona

- **El cuestionario está definido como datos.** El array `SECCIONES` es la única fuente de verdad: de ahí salen los controles, el orden en pantalla, los saltos lógicos y los nombres de columna en la planilla. Para agregar, quitar o cambiar una pregunta se edita solo ese array.
- **Saltos lógicos.** Una pregunta con `cuando: { campo, es, no }` queda oculta hasta que se responde la pregunta de la que depende. Por ejemplo, si la persona responde que no toca y compra para otra, no se le pregunta su perfil musical ni su instrumento.
- **Progreso.** La barra del encabezado y el contador "X de Y preguntas" cuentan solo las preguntas visibles y obligatorias.
- **Validación.** Al enviar, si falta alguna respuesta, el formulario lleva el foco a la primera pregunta sin completar.
- **Envío.** Las respuestas se mandan por `POST` en JSON a un webhook de n8n (constante `ENDPOINT`). Ese flujo valida los datos y agrega una fila a la pestaña "Respuestas" de la planilla de Google Sheets. Siempre se envían todas las columnas: las preguntas salteadas van como `""` para que cada fila tenga la misma forma. La fecha y hora la agrega el servidor, no el navegador.
- **Diseño.** Usa los mismos tokens de color y tipografía que la tienda, así la encuesta se ve como parte del mismo producto. Tiene tema claro y oscuro (sigue la preferencia del sistema o el atributo `data-theme`), se adapta a celulares y respeta `prefers-reduced-motion`.

### Campos de cada pregunta

| Clave | Significado |
|---|---|
| `n` | Nombre del campo y de la columna en la planilla |
| `l` | Texto de la pregunta |
| `t` | Tipo: `select` (por defecto), `texto` o `texto-largo` |
| `o` | Opciones, para los `select` |
| `pista` | Texto de ayuda debajo de la pregunta |
| `opcional` | `true`: no es obligatoria y no cuenta en el progreso |
| `cuando` | Salto lógico: `{ campo, es: [...], no: true? }` |

Los bloques con `escala` se muestran como una grilla de 1 a 5 con botones de radio.

## Secciones de la encuesta

| | Sección | Qué pregunta |
|---|---|---|
| A | Para empezar | Si toca algún instrumento y si compra para sí o para regalar |
| B | Tu perfil | Nombre, edad, perfil musical, instrumento principal |
| C | Cómo comprás hoy | Si ya compró online, frecuencia de compra, qué compra más |
| D | Qué te frena y qué necesitás | Frenos para comprar online, atención humana, seguridad de datos, importancia de cada factor (1 a 5), valor de cada funcionalidad (1 a 5), medio de pago, información que necesita antes de comprar |
| E | Para cerrar | Facilidad de su última compra online, promociones, canal preferido, comentario libre |

En total son 31 columnas. Solo se pide el nombre; no se piden teléfono, email ni datos de pago.

## Cómo probarla

Abrí `index.html` en el navegador. No hace falta un servidor local.

Ojo: al enviar, la respuesta llega a la planilla real. Para probar sin cargar datos, cambiá `ENDPOINT` por la URL de un webhook de prueba.

---

© 2026 WebMusic Solutions · Prototipo académico, TP1 PPI, IFTS N.º 18
