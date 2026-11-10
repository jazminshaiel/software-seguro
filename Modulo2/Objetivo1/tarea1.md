# Objetivo 1: Puertos y Servicios Esenciales

Antes de interactuar con un servidor, es fundamental realizar un reconocimiento de su superficie de exposición. Los puertos son las "puertas de entrada" a los diferentes servicios que se ejecutan en un sistema. A continuación, se detallan los servicios por defecto de los puertos fundamentales:

*   **Puerto 20 (FTP - Datos):** File Transfer Protocol. Se utiliza exclusivamente para la transferencia de datos en el protocolo FTP.

*   **Puerto 21 (FTP - Control):** File Transfer Protocol. Es el canal de control, utilizado para enviar comandos y credenciales de autenticación. Es considerado inseguro porque transmite todo en texto plano.

*   **Puerto 22 (SSH):** Secure Shell. Proporciona una terminal remota cifrada para la administración segura de servidores. Es el reemplazo seguro de Telnet.

*   **Puerto 23 (Telnet):** Protocolo de terminal remota. Transmite la información (incluyendo contraseñas) en texto plano, por lo que se considera obsoleto y un riesgo de seguridad.

*   **Puerto 25 (SMTP):** Simple Mail Transfer Protocol. Utilizado para el envío y enrutamiento de correos electrónicos entre servidores de correo.

*   **Puerto 53 (DNS):** Domain Name System. Se encarga de resolver nombres de dominio (ej. google.com) a direcciones IP para que los navegadores puedan encontrar los sitios web.

*   **Puerto 80 (HTTP):** Hypertext Transfer Protocol. Protocolo base para la navegación web. No cuenta con cifrado, por lo que la información viaja expuesta.

*   **Puerto 110 (POP3):** Post Office Protocol v3. Permite a los clientes de correo electrónico (como Outlook) descargar los mensajes desde el servidor al dispositivo local.

*   **Puerto 443 (HTTPS):** HTTP Secure. Es el protocolo HTTP pero cifrado mediante TLS/SSL. Es el estándar actual para la navegación web segura.

*   **Puerto 3306 (MySQL):** Es el puerto por defecto del sistema de gestión de bases de datos MySQL/MariaDB. Por motivos de seguridad, nunca debería estar expuesto directamente a internet.
