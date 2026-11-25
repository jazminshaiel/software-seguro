## Objetivo 4: El Punto Ciego de los Escáneres Automatizados

Mientras que herramientas como Nessus, Acunetix o el escáner activo de Burp Suite son excelentes encontrando fallas técnicas (como un XSS o configuraciones inseguras), suelen fracasar rotundamente al buscar fallas lógicas. Explica brevemente por qué la lógica de negocio es el mayor "punto ciego" de la automatización y por qué la auditoría manual es indispensable en esta etapa.

---

Los escáneres (Nessus, Acunetix, Burp) funcionan buscando patrones predefinidos (firmas): errores de sintaxis, inyecciones, cabeceras faltantes o directorios expuestos. No entienden el contexto de la aplicación ni el propósito de una transacción.

Para que un escáner detecte una falla lógica (como comprar un producto con cantidad negativa, saltarse un paso del carrito o usar un cupón múltiples veces), necesitaría saber cómo funciona el negocio. La automatización no puede deducir que "restar un saldo" o "reutilizar un cupón" es un error, simplemente ve una petición HTTP válida que el servidor procesa sin devolver errores técnicos.

La auditoría manual es muy importante ya que se analiza el flujo completo del negocio para comprender el valor de los datos y experimentar con escenarios fuera de lo común. La lógica de negocio es única para cada empresa y cambia constantemente, no hay una "firma" estándar que un escáner pueda buscar. 