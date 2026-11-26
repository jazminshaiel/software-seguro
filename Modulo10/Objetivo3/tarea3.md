## Objetivo 3: XSS vs CSRF

Ambos ataques involucran al navegador de la víctima y ambos buscan afectar su cuenta, pero sus mecánicas son opuestas. Redacta una comparación técnica directa explicando la diferencia fundamental entre un Cross-Site Scripting (XSS) y un Cross-Site Request Forgery (CSRF).

---

El XSS ejecuta código en el navegador de la víctima; el CSRF no ejecuta código, solo fuerza al navegador a enviar una petición que el servidor (engañado por la cookie) cree legítima.

**XSS (Cross-Site Scripting)**
Es una vulnerabilidad de inyección. El atacante logra que código JavaScript malicioso se ejecute en el navegador de la víctima, dentro del contexto del sitio legítimo. El atacante explota la confianza que el usuario tiene en el sitio web. El código malicioso se inyecta en el propio sitio vulnerable y se ejecuta cuando la víctima lo visita. Su impacto principal es el robo de cookies (Session Hijacking), alteración visual del sitio (defacement) o robo de credenciales mediante formularios falsos.

**CSRF (Cross-Site Request Forgery)**
Es una vulnerabilidad de falsificación. El atacante obliga al navegador de la víctima a enviar una petición HTTP no deseada a un sitio donde tiene sesión activa, sin ejecutar código en ese sitio. El atacante explota la confianza que el servidor tiene en el navegador de la víctima (el servidor cree que la petición con la cookie es intencional). El código malicioso reside en un sitio externo controlado por el atacante y solo dispara una petición al sitio vulnerable. Su impacto principal es la ejecución de acciones no autorizadas en nombre de la víctima: transferencias, cambios de contraseña o creación de usuarios. 
