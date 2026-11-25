## Objetivo 3: Peligros del "Directory Listing"
Investiga y explica técnicamente qué ocurre cuando un atacante navega a un directorio que no contiene un archivo de índice (como index.html o index.php) y esta directiva está activa. 

Describe qué tipo de archivos sensibles podrían quedar expuestos al público y cómo se mitiga.

* Directory Listing: Es una vulnerabilidad que deriva de una mala configuración a nivel de servidor web (como Apache o Nginx). 

Normalmente, cuando un usuario navega a un directorio, el servidor busca un archivo de índice por defecto (como `index.html` o `index.php`) para renderizar la página. Sin embargo, si este archivo no existe y la directiva de listado está habilitada, el servidor web, en lugar de devolver un error `404 Not Found` (que sería lo correcto y seguro), expone al atacante un índice autogenerado con el árbol de archivos y carpetas alojados en ese directorio, permitiendo ver archivos que no debería ver o el mapeo de la estructura interna del sitio.

Si un atacante accede a un directorio con el Directory Listing habilitado, puede descubrir y descargar archivos críticos que no están destinados a ser públicos, como por ejemplo:

1. Archivos de respaldo (backup)
2. Archivos de configuración
3. Bases de datos
4. Código fuente
5. Documentos internos

Para solucionar este problema, la mejor práctica es la **denegación por defecto**. Se debe configurar el servidor web para que, ante la ausencia de un archivo de índice, no muestre absolutamente nada (devolviendo un código HTTP 403 o 404). 