## Objetivo 3: La Única Mitigación Definitiva (Prepared Statements)

Investiga y explica técnicamente cómo funciona una Consulta Parametrizada (Prepared Statement) a nivel de arquitectura, y por qué esta técnica neutraliza por completo cualquier intento de inyección, sin importar qué caracteres envíe el usuario.

---

Los filtros basados en listas negras (bloquear `SELECT`, `UNION` o escapar comillas) fallan porque el atacante siempre encuentra variantes sintácticas (codificaciones, comentarios, mayúsculas) para evadir el filtro. Además, escapar comillas manualmente es propenso a errores y difícil de mantener.

**Consulta Parametrizada (Prepared Statement)**
La clave está en la separación del código y los datos a nivel de arquitectura, mediante el "Pre-compilado":

1.  **Preparación (Prepare):** El desarrollador define la estructura de la consulta SQL con marcadores de posición (por ejemplo: `SELECT * FROM usuarios WHERE id = ?`). La base de datos compila esta plantilla y "congela" el plan de ejecución y la estructura de la consulta.

2.  **Vinculación (Bind):** El servidor web envía el valor del usuario por un canal separado. La base de datos lo trata estrictamente como **datos**, no como código ejecutable.

3.  **Ejecución (Execute):** La base de datos inserta el dato en el marcador, pero como ya conoce la estructura (fue compilada en el paso 1), el dato nunca puede alterar la lógica de la consulta.

Esto neutraliza cualquier inyección porque el motor de la base de datos ya sabe cuál es la estructura del comando antes de recibir la entrada del usuario. Si el atacante ingresa `1' OR '1'='1`, la base de datos no interpreta la comilla como un cierre de string, sino que la trata como un texto literal (la cadena de caracteres completa `1' OR '1'='1`). La consulta final siempre se ejecuta con la estructura original, evitando que los datos maliciosos modifiquen la lógica SQL, independientemente de los caracteres enviados.