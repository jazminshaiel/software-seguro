## Objetivo 2: SSRF y el Peligro en la Nube (AWS/GCP)

Investiga y explica qué sucede si un atacante encuentra un SSRF en una aplicación alojada en Amazon Web Services (AWS) y fuerza al servidor a hacer una petición GET a la dirección mágica 169.254.169.254. ¿Qué tipo de información altamente sensible espera extraer el atacante?

---

La IP `169.254.169.254` es el Servicio de Metadatos de Instancia de AWS. Es un servicio interno no enrutable al que las instancias en la nube (como AWS) consultan para obtener información de configuración y credenciales. 

Si la aplicación vulnerable en AWS permite al atacante enviar una petición a `http://169.254.169.254/latest/meta-data/`, el servidor web (al estar dentro de la red de AWS) responderá con información crítica. El atacante no necesita hacer nada especial, simplemente usa el SSRF para que el servidor "se pregunte a sí mismo" por sus datos.


1. Credenciales de IAM (AWS Identity and Access Management): Las instancias EC2 tienen un rol de IAM asociado. Los metadatos exponen las credenciales temporales (Access Key ID, Secret Access Key y Session Token) en la ruta `http://169.254.169.254/latest/meta-data/iam/security-credentials/[nombre-del-rol]`. 

2. User Data: En la ruta `http://169.254.169.254/latest/user-data`, a menudo se almacenan scripts de arranque (bash scripts) que contienen contraseñas de bases de datos, claves de API de terceros, o información de configuración.

3. Metadatos de Red y Entorno: Direcciones IP privadas, subredes, grupos de seguridad (que permiten al atacante mapear la red interna).

El atacante logra comprometer no solo la aplicación web, sino toda la infraestructura de la cuenta en la nube. Con las credenciales de IAM obtenidas, puede escalar privilegios (si el rol tiene permisos excesivos), moverse lateralmente entre servicios y causar una fuga masiva de datos.