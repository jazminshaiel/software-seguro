## Objetivo 1: El Arte de la Inyección Ciega (Blind SQLi)

Investiga y explica con tus propias palabras la diferencia técnica entre una inyección Boolean-Based Blind (basada en booleanos/verdadero-falso) y una inyección Time-Based Blind (basada en tiempo). ¿Cómo logra un atacante extraer el nombre de una base de datos letra por letra cuando la aplicación solo responde con una pantalla genérica de "Cargando..."?

---

### Blind SQLi
La aplicación no muestra datos de la base ni mensajes de error en pantalla. Parece invulnerable a simple vista, pero el atacante deduce la información haciendo preguntas a ciegas.

* Basada en booleanos: El atacante le hace preguntas de Verdadero o Falso al servidor (como ser *AND 1=1* y *AND 1=2*). Si la inyección es verdadera, la página se comporta de una manera (por ejemplo, muestra algo nuevo o cambia el tamaño de respuesta"). Si la inyección es falsa, se comporta de otra forma (por ejemplo, la página queda en blanco, se rompe o muestra un error genérico).

* Basada en tiempo: Como la página siempre responde igual visualmente (sea verdadero o falso), el atacante le ordena a la base de datos que "se duerma" unos segundos (usando funciones como *SLEEP(5)*) solo si la condición se cumple, por lo que si la respuesta tarda en llegar, el atacante confirma que la inyección es verdadera y si llega instantáneamente, fue falsa.

Para extraer el nombre de una base de datos cuando la aplicación solo responde con una pantalla de "Cargando...", el atacante combina la fuerza bruta con la Búsqueda Binaria. El proceso es el siguiente:

1. El atacante usa funciones SQL para extraer un caracter en una posición específica (SUBSTRING) y para convertirlo a su valor numérico (ASCII).

2. El atacante le pregunta al servidor algo como: "¿El valor ASCII del primer carácter del nombre de la base de datos es mayor a 100?".

3. Si el servidor (por booleano o por tiempo) responde "Verdadero", el atacante sabe que la letra está entre los valores ASCII 101 y 127. Repite este proceso, achicando el rango a la mitad en cada intento. Con un máximo de 7 preguntas, el atacante puede determinar el número exacto (y por ende, la letra).

4. Una vez que descubre la primera letra, incrementa el índice de la función SUBSTRING (posición 2, 3, etc.) y repite el proceso. Automatizando estas peticiones, el atacante reconstruye el nombre completo de la base de datos, tabla por tabla.