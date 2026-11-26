## Objetivo 3: Defensa en Profundidad (Encoding y CSP)

Explica cómo un arquitecto de seguridad puede mitigar definitivamente el riesgo de XSS implementando dos capas de defensa: 1) Output Encoding (Codificación de Salida) a nivel de código, y 2) Una política de Content Security Policy (CSP) a nivel de cabeceras HTTP.

---

Para mitigar el riesgo de Cross-Site Scripting (XSS) de forma definitiva, la estrategia correcta es la **Defensa en Profundidad**, implementando dos capas independientes: 

### Capa 1: Output Encoding (Codificación de Salida)
Es la mitigación principal a nivel de código. Consiste en transformar los caracteres especiales (como `<`, `>`, `"`, `'`, `&`) en sus entidades HTML correspondientes (ej. `&lt;`, `&gt;`, `&quot;`) antes de que el servidor envíe la respuesta al navegador. 

* **Contexto HTML:** Se usa codificación de entidades HTML para que el navegador trate el texto como datos y no como etiquetas.
*   **Contexto de Atributo:** Se codifican las comillas para evitar escapar del atributo (`value="..."`).
 *   **Contexto JavaScript:** Se escapan caracteres como `\`, `'`, `"`, y se evita inyectar dentro de bloques `<script>`.
*   **Contexto de URL (CSS):** Se escapan los caracteres especiales de la URL.

El navegador recibe `<script>` pero lo lee como la cadena literal `&lt;script&gt;`. El código malicioso nunca se ejecuta porque el navegador no lo interpreta como código, sino como texto plano.

### Capa 2: Content Security Policy (CSP)
Es una cabecera HTTP (`Content-Security-Policy`) que le indica al navegador desde qué orígenes (dominios) se pueden cargar recursos (scripts, imágenes, estilos). El servidor envía la cabecera con reglas estrictas. Por ejemplo:

*   `default-src 'self'`: Solo permite cargar recursos del mismo dominio.
*   `script-src 'self' https://apis.trusted.com`: Solo permite scripts del dominio propio y de un dominio de confianza explícito.
*   `script-src 'unsafe-inline'`: Permite ejecutar scripts inline (bloques `<script>` dentro del HTML). 

Si un atacante logra inyectar un script malicioso (por una falla en la codificación), el navegador consulta la CSP. Si el script intenta cargarse desde un dominio no autorizado o ejecutarse inline, el navegador lo bloquea automáticamente y muestra un error en la consola.