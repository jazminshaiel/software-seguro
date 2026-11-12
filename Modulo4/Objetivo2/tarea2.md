## Objetivo 2: Análisis de la Tríada CIA

La seguridad de la información se sostiene sobre tres pilares fundamentales: Confidencialidad, Integridad y Disponibilidad (CIA). Explica con tus propias palabras cada uno de estos pilares. Luego, da un ejemplo concreto de una vulnerabilidad web (o un tipo de ataque) que comprometa principal y directamente a cada pilar por separado. 

## Pilares
La seguridad de la información se sostiene sobre tres pilares fundamentales conocidos como la Tríada CIA:

*   **Confidencialidad (Confidentiality):** Garantiza que la información solo sea accedida y leída por las partes autorizadas.

*   **Integridad (Integrity):** Asegura que los elementos del sistema (datos, código, configuraciones) solo puedan ser modificados por partes autorizadas. 

*   **Disponibilidad (Availability):** Asegura que los sistemas, las redes y los datos estén accesibles y funcionales en el momento en que las partes autorizadas los necesiten para realizar sus tareas.

## Ejemplos concretos

#### Ataque a la Confidencialidad: IDOR (Insecure Direct Object Reference)
Es una vulnerabilidad de control de acceso donde una aplicación expone una referencia directa a un objeto interno (como un ID de usuario o de archivo). Si un atacante cambia ese ID, puede acceder a los datos de otro usuario sin autorización.

*   **Ejemplo:** Un usuario común (ID 1) cambia el parámetro en la URL a `GET /api/usuarios/2` y el servidor le devuelve los datos personales, correo y contraseña (hasheada) del usuario ID 2. El ataque rompe la **Confidencialidad** porque se produce una fuga de información sensible hacia un usuario que no tiene permisos para verla.

#### Ataque a la Integridad: Inyección SQL (SQLi)
Ocurre cuando un atacante logra inyectar datos maliciosos o modificar parámetros (como un JSON) para alterar la base de datos de la aplicación.
*   **Ejemplo:** En un formulario de perfil, el usuario envía un JSON para actualizar su foto, pero inyecta un campo no permitido: `{"foto": "nueva.jpg", "rol": "admin"}`. Si el servidor acepta ese campo y modifica el rol, se alteró la lógica del sistema. El ataque rompe la **Integridad** porque se modificó el estado de un registro (el rol del usuario) sin estar autorizado para hacerlo.

#### Ataque a la Disponibilidad: Denegación de Servicio (DoS) o Rate Limit bypass
Consiste en saturar los recursos de un servidor (memoria, procesamiento, red) para que deje de responder a los usuarios legítimos.
*   **Ejemplo:** Un atacante automatiza el envío de miles de peticiones por segundo a un endpoint de login utilizando un script. El servidor consume toda su memoria RAM intentando procesar las solicitudes falsas, se cae y los usuarios legítimos no pueden acceder al sitio. El ataque rompe la **Disponibilidad** porque el sistema deja de estar operativo para quienes tienen permiso de usarlo.