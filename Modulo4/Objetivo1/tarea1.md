## Objetivo 1: Estandarización de Vulnerabilidades (CVE y CWE)
Investiga y redacta un informe breve explicando qué es un CVE y qué es el catálogo CWE, marcando la diferencia entre ambos. Menciona quiénes son las autoridades que asignan los CVE (CNAs), dónde se consultan públicamente, y nombra al menos dos herramientas automatizadas de la industria que se utilizan para detectar vulnerabilidades con CVEs asignados.


### ¿Qué es un CVE? (Common Vulnerabilities and Exposures)
Es una lista pública de vulnerabilidades de seguridad específicas que han sido descubiertas y divulgadas. Funciona como un identificador único para cada fallo de seguridad real.

*   **¿Quiénes asignan los CVE?** Son asignados por las **CNAs (CVE Numbering Authorities)**, que son organizaciones autorizadas para asignar identificadores a vulnerabilidades dentro de su ámbito de cobertura. Actualmente existen más de 560 CNAs en todo el mundo, incluyendo gigantes tecnológicos como Microsoft, Google, Apple, Red Hat y GitHub.

*   **¿Dónde se consulta públicamente?** El catálogo oficial se encuentra en `cve.org`.

### ¿Qué es el CWE? (Common Weakness Enumeration)
El CWE es un catálogo comunitario de tipos de debilidades en software y hardware. Mientras que un CVE identifica una instancia específica de un fallo, el CWE identifica la causa raíz o el patrón de programación que provocó ese fallo. Se consulta públicamente en `cwe.mitre.org`.

### Diferencias
*   **CVE** - **La instancia específica**. Es un fallo concreto en un producto y versión específicos (Ej: "La versión 2.0 del software X tiene una falla de desbordamiento").

*   **CWE** - **La causa raíz**. Es la categoría del error de programación que causó esa falla (Ej: "Desbordamiento de búfer"). 


### Herramientas Automatizadas

1.  **Nessus (Tenable):** Es uno de los escáneres de vulnerabilidades más utilizados a nivel corporativo. Escanea redes y sistemas en busca de configuraciones débiles y software desactualizado, vinculando cada hallazgo con su CVE correspondiente.

2.  **OpenVAS (Greenbone):** Es una herramienta de escaneo de vulnerabilidades de código abierto. Funciona de manera similar a Nessus, realizando pruebas de seguridad y reportando fallos que tienen asignados identificadores CVE.

3.  **Snyk:** Es una herramienta enfocada en la seguridad del código y las dependencias (SCA). Detecta bibliotecas de código abierto vulnerables en los proyectos de desarrollo, alertando sobre CVEs conocidos en esas librerías.
