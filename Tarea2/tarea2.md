# Objetivo 2: Ejecución de Laboratorios

## Laboratorio 1: Turnero

**Tipo de vulnerabilidad:** IDOR (Insecure Direct Object Reference)

**Pasos realizados:**
1. Al ingresar con el usuario `luis` e inspeccionar la pestaña Red (Network), observé que la aplicación consume la API `/api/{user_id}/appointments/` para listar los turnos. Anoté que el `user_id` de luis es `1`.

2. Descubrí que el endpoint GET no validaba correctamente la sesión. Al modificar el `{user_id}` en la URL (por ejemplo, probando con el ID 2), el servidor devolvía una lista vacía (el usuario existía, pero no tenía turnos).

3. Tras probar manualmente con diferentes IDs, encontré que el usuario `xdalvik` tenía el `user_id: 101`. Al acceder a `/api/101/appointments/`, el servidor filtró la información y mostró todos sus turnos, incluyendo sus IDs (10, 11, 12, 13...).

4. Intercepté la petición de cancelación (`DELETE /api/appointments/{id}`). Usé la función "Editar y reenviar" para cambiar el ID del turno por los IDs correspondientes a xdalvik (10, 11, 12...). El servidor respondió con `200 OK` a cada petición, eliminando los turnos sin verificar la propiedad del recurso.

---

## Laboratorio 2: Ventas

**Tipo de vulnerabilidad:** Information Disclosure - IDOR

**Pasos realizados:**
1. Identifiqué que el endpoint `/ventas` requería un parámetro `id`. Las pruebas manuales devolvían respuestas `403 Forbidden` (recurso existente pero protegido) y `404 Not Found` (recurso inexistente), alternando de una forma que impedía un análisis directo.

2. Desarrollé un script en Python (con ayuda de IA) utilizando las librerías `requests`, `time` y `hashlib`. El script iteraba sobre un rango de IDs del 1 al 3500 (rango aproximado obtenido en las pruebas manuales) en la URL `/ventas?id=X`.

3. Agregué una pausa de 0.1 segundos (`time.sleep`) entre peticiones para no saturar el servidor y evitar errores de Timeout.

4. Implementé una condición de parada: si el script detectaba 10 respuestas `404` consecutivas, asumía que había llegado al final de los datos y detenía el bucle.

5. El script finalizó con éxito tras encontrar **1641 respuestas 403 (Forbidden)**.

6. Se calculó el hash MD5 del número `1641` utilizando la función `hashlib.md5` integrada en el mismo script, el cual fue validado por el juez del laboratorio.

---

## Laboratorio 3: Presupuesto

**Tipo de vulnerabilidad:** Mass Assignment

**Pasos realizados:**
1. Inspeccioné la petición `GET /api/gastos/` y vi que el servidor devolvía un JSON con todos los gastos, incluyendo sus IDs, montos, categorías y el campo `revisado: false`.

2. Hice clic en el botón "Revisar" de un gasto y capturé la petición `POST /api/gastos/{id}/editar/`. El cuerpo enviaba `{"revisado": true}`. Al no haber validación estricta, el servidor aceptaba la inyección de otros campos (Mass Assignment).

3. Utilizando la herramienta "Editar y reenviar" del Inspector de Firefox, fui modificando cada gasto uno por uno. Inyecté en el JSON los campos "monto" (con los valores recalculados matemáticamente para cumplir los promedios) y "revisado": true.

4. Al refrescar la página, el resumen mostró los promedios exactos solicitados y todos los gastos aparecieron con el estado "Sí" en la columna Revisado.

---


## Laboratorio 4: Gran Rifa 2019

**Tipo de vulnerabilidad:** Mass Assignment

**Pasos realizados:**
1. Ingresé a la plataforma con las credenciales provistas (`guido` / `RIFA_2019`).

2. Abrí el Inspector de Firefox (F12) en la pestaña "Red" (Network) para observar el tráfico.

3. Identifiqué que la tabla se llenaba mediante una petición `GET /api/numeros/`. Al analizar la respuesta JSON, descubrí la estructura de datos:

   ```json
   {"id": 4, "numero": 454, "comprador": "John Backus", "esta_pago": false}

[Campo clave] esta_pago: false

4. Hice clic en el botón "Editar" de la fila de John Backus y guardé los cambios.

5. Capturé la petición generada: POST /api/numeros/4/editar/.

6. En la pestaña "Solicitud" ví que el cuerpo (body) enviado por el formulario era:

    ```json
    {"comprador": "John Backus"}

7. Hice clic derecho sobre la petición POST en la pestaña Red y seleccioné "Editar y reenviar".

8. Modifiqué manualmente el cuerpo JSON, agregando el campo que había descubierto en el paso 3 y que el formulario normal no enviaba:

    ```json
    {
        "comprador": "John Backus",
        "esta_pago": true
    }

9. Envié la petición modificada. El servidor respondió con un código 200 (OK).

10. Al recargar la página, el sistema validó la modificación en la base de datos y mostró que el campo estaba como pagado.

---