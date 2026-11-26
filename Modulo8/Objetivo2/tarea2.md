## Objetivo 2: Automatización vs Explotación Manual (sqlmap)

Enumera y explica al menos dos escenarios técnicos del mundo real donde sqlmap fallaría estrepitosamente, pero un atacante manual (o un pentester experimentado manipulando la petición en Burp Suite) sí lograría explotar la vulnerabilidad.

---

**Escenario 1: Protecciones CSRF o CAPTCHAs dinámicos** 

Muchas aplicaciones web incluyen tokens CSRF o CAPTCHAs que cambian con cada petición para evitar ataques automatizados. sqlmap, al enviar la misma petición una y otra vez, utiliza un token que ya expiró, por lo que el servidor rechaza la petición (por ejemplo, con un error 403) antes de que la consulta SQL llegue a ejecutarse. La herramienta automatizada se confunde y asume que no hay inyección.

* Un pentester experimentado, en cambio, intercepta la petición en Burp Suite, extrae el token actualizado y lo reemplaza manualmente. 

**Escenario 2: Filtros WAF estrictos**

Los WAFs modernos (como Cloudflare o ModSecurity) bloquean las firmas de ataque más conocidas que utiliza sqlmap por defecto (como las palabras `UNION SELECT`, `SLEEP`, o el uso de comillas simples). Si el WAF está bien configurado, sqlmap recibirá cientos de errores 403 Forbidden y fallará.

* Un pentester experimentado, puede evadir el WAF jugando con la sintaxis de la consulta SQL, una técnica llamada "evasión de filtros". Por ejemplo, si el WAF bloquea espacios, el pentester usa comentarios `/**/`. 

**Escenario 3: APIs modernas (JSON/XML)**

sqlmap está optimizado para formularios HTML tradicionales (*application/x-www-form-urlencoded*). En las arquitecturas modernas, las APIs reciben datos en formato JSON (por ejemplo: `{"usuario": "admin", "id": 1}`). Por defecto, sqlmap no sabe cómo inyectar en este tipo de estructuras y puede pasar por alto la vulnerabilidad.

* Un pentester experimentado intercepta el JSON en Burp Suite, modifica el valor del parámetro manualmente (por ejemplo, `"id": "1' OR '1'='1"`) y observa la respuesta del servidor.
