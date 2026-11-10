Investiga y explica con tus propias palabras qué es y cómo funciona el proceso de TLS Handshake (el apretón de manos TLS) que ocurre antes de que el navegador envíe la primera petición HTTP. 

¿Qué rol juegan los certificados digitales en este proceso y por qué se usan dos tipos de cifrado distintos (asimétrico y simétrico) en una misma conexión? 

**TLS** (Transport Layer Security) es un protocolo creado para proporcionar privacidad, autenticación e integridad en las comunicaciones entre navegador y servidor.

Antes de que se envíe la primera petición, se realiza el **TLS Handshake**, este proceso consta de:

1. El navegador y el servidor intercambian mensajes para negociar la versión de TLS y los algoritmos de cifrado que van a usar (*Client Hello / Server Hello*).
2. El servidor envía su certificado digital.
3. El navegador genera un secreto y lo cifra usando la llave pública del servidor. El servidor lo descifra con su llave privada.

Ambas partes, a partir de ese secreto, calculan la misma **Session Key** (clave de sesión) que se usará para el cifrado simétrico.

## Tipos de Cifrado

*   **Asimétrico:** El servidor tiene una llave pública y una privada. Cuando se encripta con la llave pública, solo la llave privada puede desencriptar. Es usado al principio, es lento y pesado.
*   **Simétrico:** Usa la misma llave para encriptar y desencriptar. Es usado para todo el tráfico HTTP posterior, es rápido y liviano.

Se usan ambos cifrados porque es más eficiente, usamos el cifrado asimétrico para transportar la llave del cifrado simétrico. Esto porque el cifrado asimétrico asegura el intercambio y simétrico la navegación rápida.

## Certificado Digital

Es un certificado firmado por una **Autoridad de Certificación (CA)** que le indica al navegador que la llave que el servidor proporciona es confiable y segura.