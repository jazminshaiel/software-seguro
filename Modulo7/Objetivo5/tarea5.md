## Objetivo 5: El Espejismo del Cifrado en el Cliente (Frontend Encryption)

El equipo de desarrollo de una aplicación financiera implementa un mecanismo donde todo el cuerpo (body) de las peticiones POST y los parámetros sensibles se cifran mediante JavaScript en el navegador del usuario antes de ser enviados al backend. Los arquitectos del sistema afirman que, gracias a esto, es imposible que un atacante intercepte y manipule los valores utilizando un proxy como Burp Suite, ya que todo viaja encriptado. Analiza este escenario de arquitectura y explica detalladamente por qué esta afirmación es una falsa sensación de seguridad. ¿Dónde reside el fallo fundamental de diseño y cuáles serían los pasos tácticos que realizaría un atacante para evadir esta protección, leer los datos y enviar valores manipulados al servidor?

---

La afirmación de los arquitectos es incorrecta esto porque el fallo reside en que la clave criptográfica y el algoritmo de cifrado viajan y se ejecutan en el cliente (el navegador del usuario), que es un entorno controlado por el atacante. Si el navegador puede cifrar los datos, el atacante también tiene acceso a esa misma lógica y a la clave para descifrarlos o generar nuevos datos cifrados a su antojo. 

**Pasos:**

1. Interceptar el tráfico y ver el cuerpo cifrado. Como la clave está en el JavaScript, usamos el inspector y buscamos el archivo .js que contiene la lógica de cifrado para extraer la función y la clave secreta.

2. Aplicamos la misma función en una herramienta de scripting (Python, Node.js) para leer el texto plano original y comprender la estructura del JSON.

3. Modificamos los valores que queremos (por ejemplo, cambiar el monto de una transferencia de $100 a $10.000) y volvemos a cifrar el payload con la función y clave extraídas.

4. Luego reenviamos la petición POST con el nuevo cuerpo cifrado. El backend recibirá el dato, lo descifrará con su clave (asumiendo que está correcto) y procesará la transacción manipulada sin detectar anomalías.