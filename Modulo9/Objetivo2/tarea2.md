## Objetivo 2: Anatomía del DOM XSS

A diferencia del XSS Almacenado y Reflejado, el DOM-Based XSS es indetectable para muchos sistemas de prevención de intrusos (IPS) y firewalls (WAF). Investiga y explica por qué ocurre esto (Pista: analiza cómo manejan los navegadores el fragmento # en una URL y qué sucede cuando el código JavaScript procesa entradas como location.hash de forma insegura).

---

El XSS basado en DOM (Document Object Model) es una vulnerabilidad que se ejecuta exclusivamente en el navegador del cliente y nunca llega a ser procesada por el servidor. La clave de esta invisibilidad está en cómo los navegadores manejan el fragmento de la URL (el símbolo `#`).

* Cuando el atacante envía un enlace con un payload en el hash (ej. `https://sitio.com/perfil#<img src=1 onerror=alert(1)>`), el navegador realiza la petición HTTP únicamente a `https://sitio.com/perfil`. Todo el texto después del `#` es ignorado y no se envía al backend.

* Como el payload malicioso nunca llega al servidor, el Firewall de Aplicaciones Web (WAF) y los sistemas de prevención de intrusos (IPS) no tienen nada que inspeccionar. No pueden bloquear lo que no ven.

* El servidor devuelve un HTML seguro. Sin embargo, el código JavaScript de la propia página toma el valor del hash (Source) y lo inyecta directamente en el DOM (Sink). Por ejemplo, usando `document.getElementById("saludo").innerHTML = "Hola " + window.location.hash.substring(1);`. Al usar `innerHTML`, el navegador interpreta el texto como HTML y ejecuta el script malicioso.
