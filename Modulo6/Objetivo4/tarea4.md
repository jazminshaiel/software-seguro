## Objetivo 4: Análisis del Ataque de Algoritmo "None"

Una de las vulnerabilidades más críticas en la implementación de JWT es aceptar el algoritmo None. Investiga y explica técnicamente por qué un backend vulnerable a esto permite a un atacante falsificar cualquier identidad sin necesidad de conocer la clave secreta del servidor.

El algoritmo "None" es una opción que existe en el estándar de JWT para crear tokens "no asegurados", es decir, sin firma. Está pensado para casos muy específicos donde la seguridad se maneja por otra capa (como una red interna ultra segura).

Cuando un backend está mal configurado y recibe un token cuyo Header indica "alg": "none", el servidor interpreta que debe omitir la verificación de la firma y confía ciegamente en el contenido del token. 

Un atacante puede tomar un token válido (propio con privilegios bajos), decodificarlo, modificar el Header para que indique "alg": "none", alterar el Payload a su antojo (cambiando "is_superadmin": true) y, finalmente, eliminar la firma. Si el servidor es vulnerable, aceptará el token modificado como legítimo sin siquiera comprobar la firma.

Es muy peligroso ya que rompe por completo el pilar de la autenticación. La firma es el único mecanismo que garantiza que el token fue emitido por el servidor legítimo y que no fue manipulado. Al omitir su verificación, el atacante logra falsificar cualquier identidad y escalar privilegios sin necesidad de conocer la clave secreta del servidor.