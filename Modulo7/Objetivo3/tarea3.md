## Objetivo 3: Anatomía de una Condición de Carrera (Race Condition)

Las vulnerabilidades de tiempo de procesamiento (Race Conditions / TOCTOU) son críticas en sistemas financieros o de inventario. Investiga y explica con tus palabras cómo un atacante podría utilizar herramientas (como el Turbo Intruder de Burp Suite) para enviar 50 peticiones simultáneas y lograr canjear un código de descuento de un solo uso múltiples veces antes de que el servidor actualice la base de datos.

---

Una Condición de Carrera es una vulnerabilidad que ocurre cuando un sistema realiza dos operaciones lógicas críticas de forma concurrente sin la debida sincronización: primero verifica un estado (Check) y luego ejecuta una acción basada en ese estado (Use). Si el tiempo entre la verificación y la ejecución es lo suficientemente amplio, un atacante puede enviar múltiples peticiones en paralelo para explotar esta ventana de tiempo.

Para explotar esta vulnerabilidad se utiliza Burp Suite con la extensión Turbo Intruder, que permite enviar peticiones en paralelo (concurrentes) mediante scripts personalizados.

1. El atacante posee un código de descuento válido para un solo uso.

2. Intercepta la petición HTTP de canje y la envía a Turbo Intruder.

3. Configura el script para que envíe 50 peticiones simultáneas con el mismo código de cupón.

4. El servidor recibe las 50 peticiones al mismo tiempo. Debido a la falta de bloqueos, los 50 hilos del servidor verifican en la base de datos que el cupón aún no ha sido usado, y todos obtienen una respuesta positiva.

5. El servidor procesa los 50 descuentos y recién después actualiza la base de datos marcando el cupón como "utilizado".

Así es como el atacante canjeó exitosamente un cupón de un solo uso unas 50 veces, generando una pérdida financiera directa para la empresa.
