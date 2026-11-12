## Objetivo 3: Análisis de Automatización y Fuerza Bruta

Investiga y explica con tus propias palabras la diferencia fundamental entre un ataque de "Fuerza Bruta" tradicional y un ataque de "Diccionario" aplicado a un formulario de login. Además, menciona al menos un mecanismo de defensa que un desarrollador podría implementar para mitigar estas automatizaciones.

*   **Fuerza Bruta:** Consiste en probar todas las combinaciones posibles de caracteres sin seguir una lógica o patrón específico. Es un proceso extremadamente lento y costoso computacionalmente.

*   **Ataque de Diccionario:** En lugar de probar combinaciones al azar, se usa una lista predefinida (el "diccionario") con contraseñas que las personas suelen usar en la vida real o vienen predeterminadas y no se cambian (por ejemplo, usuario: admin / contraseña: admin). Este método es mucho más rápido y eficiente porque explota el error humano, probando primero las contraseñas más comunes. 

Se pueden implementar varias medidas para mitigar los ataques automatizados:
1.  **Límite de Peticiones (Rate Limiting):** Es la defensa fundamental ya que limita la cantidad de peticiones que un cliente puede hacer en un tiempo determinado (por ejemplo, un máximo de 10 peticiones cada 60 segundos). 

2.  **CAPTCHA Obligatorio:** Tras algunos intentos fallidos (por ejemplo, después de 3), el sistema exige resolver un CAPTCHA. Esto frena en seco a los scripts automatizados, ya que deben ser resueltos por un humano (aunque hoy en día se pueden pagar servicios para resolverlos).

3.  **Bloqueo por IP + Cuenta (La combinación perfecta):** Es la medida más recomendada ya que bloquea una cuenta específica solo si los intentos provienen de la misma IP atacante. Esto debido a que si solo bloqueamos la IP, todos los usuarios de esa red quedarían sin acceso y si solo bloqueamos la cuenta, un atacante podría bloquear la cuenta de un usuario legítimo. Al combinar ambos, el atacante se queda afuera, pero el dueño real de la cuenta puede entrar desde otra red.
