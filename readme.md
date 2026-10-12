# 📧 AI Email Reply Agent



Agente de correo electrónico desarrollado en Python que utiliza inteligencia artificial para redactar respuestas automáticas a correos recibidos en Gmail, mediante la API de OpenAI y LangChain.



El programa comprueba los correos no leídos, identifica los remitentes autorizados y genera respuestas profesionales adaptadas al contenido y al idioma del mensaje original.



Por seguridad, el agente puede guardar las respuestas como borradores en Gmail para que el usuario las revise antes de enviarlas.


---


## ✨ Características



- **Filtrado de remitentes:** solo procesa correos procedentes de las direcciones incluidas en `direcciones_email.txt`.

- **Redacción mediante IA:** utiliza OpenAI y LangChain para generar respuestas concisas, amables y profesionales.

- **Dos modos de funcionamiento:** guardar respuestas como borradores o enviarlas automáticamente.

- **Protección frente a respuestas inapropiadas:** identifica mensajes que requieren revisión humana, como correos relacionados con cuestiones legales, económicas, reclamaciones o situaciones urgentes.

- **Control de correos procesados:** registra los identificadores de los mensajes para evitar procesarlos repetidamente.

- **Conservación del estado de lectura:** consulta los mensajes sin marcarlos como leídos durante la búsqueda.

- **Filtrado de correos automáticos:** evita responder a listas de distribución, newsletters y determinados remitentes automáticos.

- **Configuración mediante variables de entorno:** permite mantener las credenciales fuera del código fuente.


---


## 🛠️ Tecnologías utilizadas



- Python 3.10 o superior.

- [LangChain](https://www.langchain.com/).

- [OpenAI API](https://platform.openai.com/).

- [Pydantic](https://docs.pydantic.dev/).

- `python-dotenv` para cargar variables de entorno.

- IMAP y SMTP para recibir y gestionar correos de Gmail.


---


## 📋 Requisitos previos



Antes de ejecutar el proyecto necesitarás:



1. Tener Python instalado.

2. Disponer de una cuenta de Gmail.

3. Configurar una contraseña de aplicación de Google para acceder al correo mediante IMAP y SMTP.

4. Disponer de una clave API de OpenAI con acceso y facturación configurados según el uso previsto.

5. Tener conexión a Internet.



**Importante:** una suscripción a ChatGPT no incluye necesariamente crédito para utilizar la API de OpenAI. El consumo de la API se factura de forma independiente.


---


## 🚀 Instalación



### 1. Clonar el repositorio



Abre una terminal y ejecuta:



```bash

git clone https://github.com/juan1639/agent_reply_emails.git

cd agent_reply_emails

```



### 2. Crear un entorno virtual



Se recomienda utilizar un entorno virtual para mantener aisladas las dependencias del proyecto.



En Windows:



```powershell

python -m venv .venv

.venv\\Scripts\\activate

```



En Linux o macOS:



```bash

python3 -m venv .venv

source .venv/bin/activate

```



### 3. Instalar las dependencias



Con el entorno virtual activado, ejecuta:



```bash

python -m pip install --upgrade pip

pip install langchain-openai python-dotenv pydantic

```


---



## ⚙️ Configuración



El proyecto necesita un archivo `.env` con las credenciales de acceso al correo y a OpenAI.



### 1. Crear el archivo `.env`



Crea un archivo llamado `.env` en la raíz del repositorio, junto al script principal, con el siguiente contenido:



```dotenv

EMAIL_USER=tu_correo@gmail.com

EMAIL_PASS=tu_contrasena_de_aplicacion

OPENAI_API_KEY=tu_clave_api_de_openai



Modo de respuesta: borrador o enviar

MODO_RESPUESTA=borrador

```



Sustituye los valores de ejemplo por tus propias credenciales.



No compartas este archivo ni publiques sus credenciales en GitHub.



### 2. Configurar Gmail



El programa utiliza los siguientes servidores:



- **IMAP:** `imap.gmail.com`, para consultar los correos recibidos.

- **SMTP:** `smtp.gmail.com`, puerto `465`, para enviar respuestas.



Para Gmail, normalmente tendrás que activar la verificación en dos pasos y generar una contraseña de aplicación desde la configuración de seguridad de tu cuenta de Google.



Utiliza esa contraseña de aplicación en `EMAIL_PASS`, no tu contraseña habitual de Gmail.



Consulta la documentación oficial de Google:



- [Contraseñas de aplicación](https://support.google.com/accounts/answer/185833)

- [Configuración de Gmail para otros clientes de correo](https://support.google.com/mail/answer/7126229)



### 3. Configurar los remitentes autorizados



El archivo `direcciones_email.txt` contiene las direcciones de correo a las que el agente prestará atención.



Incluye una dirección por línea:



```text

Remitentes autorizados



cliente1@example.com

cliente2@example.com

proveedor@example.com

```



Las líneas vacías y las que comienzan por `#` se ignoran.



**Importante:** utiliza direcciones de ejemplo en el repositorio público. Si necesitas mantener una lista privada de remitentes, no publiques sus direcciones personales en GitHub.


---


## ▶️ Ejecución



Una vez completada la configuración, ejecuta el programa desde la raíz del repositorio:



```bash

python agent_reply_mail_v6.py

```



El programa validará la configuración, se conectará a Gmail, buscará mensajes pendientes de los remitentes autorizados y generará las respuestas que correspondan.



La ejecución es puntual: al terminar de procesar los mensajes encontrados, el programa finaliza. Para comprobar el correo periódicamente, puedes programar su ejecución mediante el Programador de tareas de Windows o una herramienta equivalente.



## 📝 Modos de respuesta



El comportamiento se controla mediante la variable `MODO_RESPUESTA` del archivo `.env`.



### Modo borrador (recomendado)



```dotenv

MODO\_RESPUESTA=borrador

```



El agente redacta la respuesta y la guarda en la carpeta de borradores de Gmail.



El correo original permanece sin leer y puedes revisar, editar y enviar manualmente el borrador.



Este es el modo predeterminado y el recomendado para empezar a utilizar el proyecto.



### Modo envío automático



```dotenv

MODO\_RESPUESTA=enviar

```



El agente envía directamente la respuesta generada al remitente original.



Después de enviar correctamente la respuesta, marca el correo original como leído.



Utiliza este modo únicamente después de comprobar el comportamiento del agente con mensajes reales y de confianza. La revisión humana sigue siendo recomendable.


---


## 🔄 ¿Cómo funciona?



El flujo de ejecución es el siguiente:



1. Carga la configuración del archivo `.env`.

2. Lee las direcciones autorizadas de `direcciones_email.txt`.

3. Consulta el archivo `procesados.txt` para identificar mensajes tratados anteriormente.

4. Se conecta a Gmail mediante IMAP y busca correos no leídos de los remitentes autorizados.

5. Comprueba la dirección exacta del remitente y filtra determinados mensajes automáticos.

6. Comprueba la autenticación DMARC del mensaje cuando está activada la opción correspondiente.

7. Envía el contenido del correo al modelo de OpenAI a través de LangChain para generar una respuesta estructurada.

8. Si el mensaje requiere intervención humana, no genera una respuesta automática y deja el correo sin leer.

9. Si la respuesta es adecuada, la guarda como borrador o la envía, según la configuración.

10. Registra el mensaje procesado para evitar repetir innecesariamente el trabajo.



El programa utiliza el modelo `gpt-4o-mini` y limita el texto del cuerpo del correo a 4.000 caracteres para controlar la cantidad de contenido enviado al modelo.


---


## 📁 Estructura del repositorio



```text

agent\_reply\_emails/

│

├── agent\_reply\_mail\_v6.py   # Código principal del agente

├── direcciones\_email.txt      # Lista de remitentes autorizados

├── .gitignore                  # Reglas para excluir archivos de Git

├── .env                        # Credenciales locales (no publicar)

├── procesados.txt              # Registro local de mensajes tratados

└── README.md                   # Documentación del proyecto

```



Los archivos `.env` y `procesados.txt` son archivos locales de configuración y ejecución. El primero contiene credenciales y el segundo almacena los identificadores de los mensajes procesados. No es necesario distribuirlos con el código fuente.


---


## 🔐 Seguridad y buenas prácticas



- No publiques tu clave API de OpenAI ni las credenciales de Gmail.

- No incluyas datos personales o direcciones privadas en archivos públicos.

- Mantén `MODO_RESPUESTA=borrador` hasta haber probado suficientemente el sistema.

- Revisa las respuestas generadas por la IA antes de enviarlas.

- Los filtros de remitentes y autenticación reducen ciertos riesgos, pero no garantizan que todos los mensajes sean legítimos.

- Los mensajes que requieren revisión humana pueden contener información sensible. Comprueba que el uso de la API se ajusta a tus necesidades de privacidad.

- No ejecutes el programa simultáneamente desde varias instancias sobre la misma cuenta sin revisar antes cómo se gestionan los mensajes procesados.


---


## 🐛 Solución de problemas



**Faltan variables de entorno**



Comprueba que el archivo `.env` está en la ubicación correcta y que contiene `EMAIL_USER`, `EMAIL_PASS` y `OPENAI_API_KEY`.



**Error de autenticación de Gmail**



Comprueba que la verificación en dos pasos está activada, que utilizas una contraseña de aplicación válida y que las credenciales corresponden a la cuenta correcta.



**No se encuentran correos**



Verifica que el mensaje está sin leer, que el remitente aparece exactamente en `direcciones_email.txt` y que el correo cumple los filtros de seguridad del programa.



**El agente no genera una respuesta**



El sistema puede decidir que un mensaje requiere revisión humana. Consulta la salida de la terminal para conocer el motivo.



**Error al llamar a OpenAI**



Comprueba la validez de la clave API, los permisos de la cuenta, la disponibilidad del modelo y la configuración de facturación.


---


## 📄 Licencia



Consulta los términos de uso del repositorio antes de reutilizar o distribuir el código. Si deseas permitir formalmente su reutilización por terceros, considera añadir un archivo `LICENSE` con la licencia que elijas.



## 👤 Autor



Desarrollado por [juan1639](https://github.com/juan1639).



Repositorio: [agent_reply_emails](https://github.com/juan1639/agent\_reply\_emails)



