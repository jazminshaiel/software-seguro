## Objetivo 1: Anatomía de un Ataque CSRF (Cross-Site Request Forgery)

Explica el flujo exacto de un ataque CSRF exitoso. Detalla por qué ocurre este fallo de seguridad (haciendo énfasis en el comportamiento automático de los navegadores con las cookies) y cómo el atributo de cookie SameSite (y sus valores Lax y Strict) ayuda a mitigar radicalmente este vector.

---

El Cross-Site Request Forgery (CSRF) es una vulnerabilidad que explota la confianza que tiene un servidor en el navegador de la víctima. El flujo del ataque se compone de los siguientes pasos:

1. La víctima visita una página del atacante (a través correo o mensaje) mientras mantiene su sesión abierta en la aplicación objetivo. La página maliciosa contiene un código oculto que dispara una petición HTTP hacia *banco.com* (por ejemplo, una transferencia de dinero).
2. El navegador de la víctima procesa el código y emite la petición al servidor. En este punto, el navegador adjunta automáticamente la cookie de sesión válida de la víctima, sin que ella intervenga.
3. El servidor de *banco.com* recibe la petición, valida la cookie de sesión (que es legítima) y ejecuta la acción (transferir el dinero) sin poder distinguir si la acción fue intencional o no.

El fallo está en que el servidor se basa exclusivamente en la cookie de sesión para autenticar la acción. El protocolo HTTP es *stateless* (sin estado), por lo que el navegador se ve obligado a enviar la cookie en cada petición para mantener la sesión. La Same-Origin Policy (SOP) impide que el script de *evil.com* lea la respuesta de *banco.com*, pero no impide que el navegador envíe la petición. El servidor no tiene forma de distinguir si el usuario hizo clic en "Transferir" de forma consciente o si el formulario se envió solo desde otra pestaña.

**Mitigación:** El atributo `SameSite` en las cookies le indica al navegador bajo qué condiciones debe enviar la cookie en peticiones que provienen de otros sitios (cross-site).

*   **SameSite=Strict:** La cookie nunca se envía en peticiones cross-site. Si la víctima está en `evil.com` y se dispara una petición a *banco.com*, el navegador no adjuntará la cookie. El ataque fracasa por completo. Tiene el impacto de que si la víctima sigue un enlace desde un correo hacia *banco.com*, tendrá que loguearse de nuevo.

*   **SameSite=Lax:** La cookie se envía solo en navegaciones top-level con métodos seguros (GET). No se envía en peticiones POST, `fetch`, iframes o carga de imágenes cross-site. Esto bloquea los ataques CSRF más comunes (que usan POST o cargan recursos ocultos), manteniendo la sesión activa si el usuario navega hacia el sitio.