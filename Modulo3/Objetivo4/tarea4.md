Resolver los laboratorios prácticos de la plataforma Software Seguro: Apagar la IA, El mejor secreto, Votacion y Votacion nueva version. Redactar un breve informe indicando los pasos que se llevaron a cabo para resolver los laboratorios. Formato de entrega: Enlace al archivo .md

# Objetivo 4: Ejecución del Laboratorios

## Laboratorio 1 - Apagar la IA

**Tipo de vulnerabilidad:** IDOR

**Pasos realizados**
1. Al ingresar al laboratorio, veo que me dan dos URLs, cuyos identificadores eran hashes MD5. Al desencriptarlos, me di cuenta que correspondían a los números 9912 y 9995 por lo que supuse que los archivos seguían un patrón secuencial numérico, pero el servidor solicitaba el hash MD5 de ese número en la URL (`/codes/{hash_MD5}/`).

3. Intercepté la petición `GET /codes/{hash}/` con Burp Suite y se envió al Intruder.

4. En la pestaña **Positions** del Intruder, se limpiaron las marcas automáticas y se seleccionó únicamente el valor del hash en la URL para agregarle los marcadores de inyección (`§`).

5. En la pestaña **Payloads**, se cargó un archivo de texto (`.txt`) que contenía una lista de hashes MD5 previamente generados a partir de un rango de números (desde el 9900 hasta el 12000).

6. Configuré la pestaña **Settings** del Intruder, específicamente en la sección **Grep - Match** con la expresión regular `\b\d{16}\b`, tildando la opción **Regex** y habilitando la casilla **Flag responses matching these expressions**.

7. Lancé el ataque con el botón **Start attack**. 

8. Al finalizar el ataque, Grep - Match detectó automáticamente una coincidencia en los resultados (marcando la petición). Al revisar la petición correspondiente al número de archivo **11520**, se encontró en su respuesta HTML el código de 16 dígitos: `5524663362514956`.

9. Finalmente, tomé ese número de 16 dígitos y obtuve su correspondiente hash MD5.

---

## Laboratorio 2 - El mejor secreto

**Tipo de vulnerabilidad:** Fuerza bruta

**Pasos realizados**
1. Al observar el video del jefe de estado, se identificó que se teclean 12 dígitos en un teclado numérico modificado. La secuencia de posiciones pulsadas es la siguiente:
6a - 2 - 6b - 6b - 9 - 6a - 9 - 9 - 9 - 9 - 5 - 6b

Dado que no conocemos la relación entre la tecla y el dígíto real, agrupé las posiciones en 5 variables distintas (A, B, C, D, E) basadas en las repeticiones:

* A (posiciones 1 y 6)

* B (posición 2)

* C (posiciones 3, 4 y 12)

* D (posiciones 5, 7, 8, 9 y 10)

* E (posición 11)

Sabiendo que cada tecla corresponde a un dígito distinto (y mutuamente excluyente), se calculó que existen 30.240 combinaciones posibles (10 * 9 * 8 * 7 * 6).

2. Se generó un archivo de texto (.txt) con las 30.240 combinaciones posibles, respetando la estructura de repeticiones descubierta en el video.

3. Para descifrar el archivo secreto.zip, se utilizó la herramienta John the Ripper. El archivo ZIP estaba protegido con cifrado AES (PBKDF2-SHA1), un método que no es soportado por librerías estándar de Python, pero sí por herramientas especializadas como John.


* Se extrajo el hash del archivo ZIP utilizando la utilidad zip2john:
```~/john/run/zip2john secreto.zip > hash_zip.txt```

* Se ejecutó el ataque de diccionario con John the Ripper, apuntando al archivo de hashes y usando el diccionario generado:
```~/john/run/john --wordlist=numeros12digitos.txt hash_zip.txt```

La herramienta logró descifrar el archivo exitosamente, encontrando la contraseña: 547795999937.

---

## Laboratorio 3 - Votación

**Tipo de vulnerabilidad:** Broken Access Control

**Pasos realizados**
1. Accedí al sitio de votación y emití un voto por la UTN (opción 1) para poder interceptar la petición `POST /votacion.ct1.php` utilizando Burp Suite.

2. Al analizar la respuesta del servidor, vi que se generaba una cookie de sesión (Set-Cookie) con una variable llamada `voto`, diseñada para evitar la votación múltiple desde un mismo navegador.

3. Envié la petición al Repeater. Al reenviarla sin modificaciones, el servidor denegó la acción indicando que ya se había votado.

4. Probé eliminando la cabecera `Cookie:` de la petición y agregué una nueva cabecera personalizada (`Header-Personalizado: 1`).

5. Al enviar la petición modificada con el Repeater, el servidor aceptó el voto como válido (200 OK), ignorando la restricción de la cookie original.

6. Entonces envié la petición al Intruder. Elegí como parámetro variable el valor de `Header-Personalizado: §1§`.

7. Desde Payloads lancé un ataque de tipo "Numbers" con un rango del 1 al 4000, permitiendo que el servidor reciba múltiples votos desde lo que parece ser un cliente distinto en cada petición.

8. Al completarse las peticiones y refrescar la página principal, la UTN superó los votos de Harvard.

---

## Laboratorio 4 - Votación nueva versión

**Tipo de vulnerabilidad:** Broken Access Control (Header Attack)

**Pasos realizados**
1. Accedí al laboratorio de la nueva versión del sistema de votación y voté por la UTN. Intercepté la petición `POST /votacion.ct1.php` con Burp Suite.

2. A diferencia de la versión anterior, al intentar reenviar la petición, el servidor devolvió un mensaje de error indicando que no se podía votar desde la misma dirección IP. El control de acceso ya no dependía de una cookie, sino de la IP del cliente.

3. Para evadir esta restricción, probé agregando una nueva cabecera a la petición desde el Repeater: `X-Forwarded-For: <IP_FALSA>`.

4. Al modificar manualmente el valor de la IP (por ejemplo, `X-Forwarded-For: 1.1.1.1` y luego `X-Forwarded-For: 2.2.2.2`), me di cuenta de que el servidor tomaba esta cabecera como la IP real del cliente, permitiendo registrar un nuevo voto en cada envío.

5. Entonces envié la petición al Intruder, marcando únicamente el último octeto de la IP como punto de inyección: `X-Forwarded-For: 1.1.1.§1§` y configurando en payloads un ataque de tipo Numbers, con un rango del 1 al 255.

6. Lancé el ataque y al refrescar la página de votación, la UTN ya había superado a Harvard gracias a los múltiples votos registrados.