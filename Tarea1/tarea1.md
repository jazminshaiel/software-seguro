# Objetivo 1: Mapeo de Arquitectura Lógica

## Diagrama de Flujo
![Diagrama](diagrama.png)

## Descripción del Flujo

1. El usuario ingresa sus datos en el formulario (Frontend).
2. El navegador envía una **petición HTTP** al servidor. Aquí actúa **HTTPS**: los datos viajan cifrados.
3. El servidor (Backend) recibe la petición y consulta la Base de Datos.
4. La Base de Datos responde con el resultado.
5. El servidor envía una **respuesta HTTP** al navegador. De nuevo, **HTTPS** cifra el viaje.
6. El usuario ve el resultado en pantalla.
