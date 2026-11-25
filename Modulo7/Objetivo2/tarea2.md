## Objetivo 2: Análisis de Manipulación Transaccional (Cantidades Negativas)

Analiza el siguiente escenario teórico: Un e-commerce permite agregar productos al carrito. Al interceptar la petición, cambias la cantidad del producto de 1 a -5. El servidor lo procesa y el total del carrito se vuelve un número negativo, lo que termina sumando saldo a tu cuenta en lugar de cobrarte. Explica técnicamente por qué ocurre esta falla y cómo debería el desarrollador validar estrictamente este campo del lado del servidor para evitarlo.

---

Esta falla ocurre cuando el servidor recibe el parámetro de cantidad (quantity) desde el navegador y lo procesa directamente en sus operaciones matemáticas sin realizar una validación previa.

La causa es la falta de validación del lado del servidor. El Frontend (HTML/JavaScript) puede restringir al usuario para que solo ingrese números positivos, pero como el atacante intercepta la petición, se salta esa validación. El backend confió ciegamente en un dato que provenía del cliente.

Para evitar esta vulnerabilidad, el desarrollador debe implementar las siguientes validaciones del lado del servidor...

* Validación de tipo de dato: Verificar que el parámetro recibido sea estrictamente un número entero (o decimal, si aplica), descartando strings, arreglos o valores nulos.

* Validación de rango: Comprobar que la cantidad sea mayor a cero (cantidad > 0) y establecer un límite máximo razonable (por ejemplo, según el stock disponible) para prevenir abusos.

* Sanitización de entrada: Limpiar el dato de cualquier carácter extraño antes de procesarlo.

* Validación final del total: Al calcular el carrito, el servidor debe verificar que el monto total sea siempre un número positivo. Si el total da menor o igual a cero, la transacción debe rechazarse automáticamente.