# MAMBOFRAME

**MAMBOFRAME** es una base *open source* para crear proyectos de backend profesionales y escalables. Su filosofía es similar a la de **[Django](https://www.djangoproject.com/)**: empezar rápido, con una estructura coherente y con funcionalidades críticas listas desde el minuto uno.

En pocas palabras, **MAMBOFRAME** es una plantilla para construir **APIs seguras y de alto rendimiento** con **FastAPI**, incluyendo de serie gestión de usuarios y autenticación mediante cookies *HTTP Only*.


---

## Índice

* [Características](#caracter%C3%ADsticas)
* [Requisitos](#requisitos)
* [Instalación](#instalaci%C3%B3n)
* [Creación de un nuevo proyecto](#creaci%C3%B3n-de-un-nuevo-proyecto)
  * [Estructura inicial de archivos](#estructura-inicial-de-archivos)
* [Arrancar el proyecto](#arrancar-el-proyecto)
* [Configuración de variables de entorno (](#configuraci%C3%B3n-de-variables-de-entorno-env)`[.env](#configuraci%C3%B3n-de-variables-de-entorno-env)`[)](#configuraci%C3%B3n-de-variables-de-entorno-env)
* [Problemas comunes](#problemas-comunes)
* [Estado del proyecto y soporte](#estado-del-proyecto-y-soporte)


---

## Características

* ✅ **API basada en FastAPI** ya configurada y lista para usar.
* ✅ **Gestión de usuarios y autenticación** utilizando cookies *HTTP Only* para maximizar la seguridad.
* ✅ **Sistema de login vía Google Oauth2**: Intregramos una solución sencilla, segura y estándar para comenzar a usar **Google** como vía de login principal (o secundaria).
* ✅ **Arquitectura de archivos escalable**, pensada para crecer sin perder orden ni mantenibilidad.
* ✅ **Licencia libre**, que permite usar, modificar y distribuir el proyecto sin restricciones.


---

## Requisitos

Antes de instalar **MAMBOFRAME**, necesitas tener:


1. **PostgreSQL**
2. **Python >= 3.10**

> Puede funcionar en versiones anteriores de Python, pero **no se ha probado** de forma oficial.


---

## Instalación

> Se recomienda **fuertemente** trabajar dentro de un entorno virtual (`venv`) para mantener aisladas las dependencias del proyecto.

Con tu entorno virtual activado, instala **MAMBOFRAME** desde `pip`:

```bash
python -m pip install mamboframe
```

Esto instalará la última versión oficial disponible del paquete.


---

## Creación de un nuevo proyecto

Antes de crear tu proyecto, es útil entender la estructura de archivos que propone **MAMBOFRAME** para mantener una buena organización y escalabilidad.

Supongamos que tu proyecto se va a llamar **Skoob** y que estás trabajando en la carpeta:

```bash
/root/MyProjectFolderToUpload/
```

### Estructura inicial de archivos

Tras inicializar un proyecto, la estructura típica será algo como:

```text
MyProjectFolderToUpload/          # Carpeta creada por ti
├─ mambo_config.json              # Configuración generada por MAMBOFRAME
├─ .env                           # Variables de entorno (se crea al arrancar por primera vez)
├─ .gitignore
└─ Skoob/                         # Carpeta del proyecto (plantilla generada)
   ├─ main.py                     # Punto de entrada principal del paquete
   └─ ...                         # Otros módulos/archivos de la plantilla
```

Para crear el proyecto desde una carpeta vacía (por ejemplo `/root/MyProjectFolderToUpload/`) con tu entorno virtual en `/root/MyProjectFolderToUpload/.venv`, ejecuta:

```bash

mamboframe --init {PROJECT_NAME}
```

Donde `{PROJECT_NAME}` es opcional y define el nombre del proyecto (MAMBOFRAME usará ese nombre para crear la carpeta).

Por ejemplo:

```bash
mamboframe --init BookApp
```

Esto generará la carpeta `BookApp` dentro de `MyProjectFolderToUpload`, quedando algo así:

```text
MyProjectFolderToUpload/
├─ mambo_config.json              # Configuración generada por MAMBOFRAME
└─ BookApp/                       # Proyecto creado
   ├─ main.py
   └─ ...
```

Y con esto, tu proyecto base estará creado.


---

## Arrancar el proyecto

Para iniciar tu proyecto MAMBOFRAME, utiliza el comando:

```bash
mamboframe --start
```

En el **primer arranque**, es normal que aparezca un error indicando que falta el archivo `.env`. Este archivo contiene las **variables de entorno** que controlan aspectos sensibles de la aplicación (conexiones a la base de datos, claves secretas, API keys externas, etc.).

Al primer intento de arranque, se generará automáticamente un archivo `.env` en la raíz del proyecto. Deberás editarlo antes de volver a intentar arrancar el servidor.


---

## Configuración de variables de entorno (`.env`)

Al abrir el archivo `.env` recién creado, verás algo similar a:

```bash
DB_NAME=mambo                     #! Required.
DB_USER=postgres                  #! Required.
DB_PASSWORD=dbpassword            #! Required.
DB_HOST=localhost

DB_PORT=5432                      #! Required.
JWT_SECRET_KEY=supersecretkey     #! Required.
GOOGLE_SECRET_KEY=xxxxxxxxxxxxxxxxxxxxxxxxx

GOOGLE_CLIENT_ID=xxxxxxxxxxxxxxxxxxxxxxxxxxxxx.apps.googleusercontent.com

GOOGLE_GEMINI_API_KEY=xxxxxxxxxxxxxxxxxxxxxxxxxxxx

GOOGLE_REDIRECT_CALLBACK_URI=http://localhost:3030/users/auth/google/callback

GOOGLE_REDIRECT_FRONTEND_URI=http://localhost:3000

API_URL=http://127.0.0.1:3030     #! Required.

# ===== admins por defecto =====
ADMIN1_NAME=FerBackend0!!2        #! Required.
ADMIN1_EMAIL=ferdhaban@gmail.com  #! Required.
ADMIN1_PASSWORD=Sup3r:-admin!     #! Required.
```

* Las variables marcadas con `#! Required.` **deben** configurarse correctamente para que la aplicación funcione.
* Algunas variables (como las de Google OAuth2 o Gemini) solo son necesarias si vas a utilizar esos módulos concretos.

> Las variables opcionales puedes configurarlas únicamente si las necesitas, pero **no las elimines del archivo** `**.env**`.
> Si no las usas, déjalas con valores por defecto o ficticios.

Una vez que hayas ajustado estas variables (especialmente las credenciales de la base de datos y la clave JWT), vuelve a ejecutar:

```bash
mamboframe --start
```

Si todo está correctamente configurado, tu API se iniciará por defecto en el puerto `3030`.

La configuración del puerto la puedes ajustar en el archivo `api.py` del módulo `api` dentro de tu proyecto. Por ejemplo:

```text

MyProjectFolderToUpload/
├─ mambo_config.json
├─ .env
└─ BookApp/
   ├─ main.py 
   ├─ api/
   │  ├─ ...
   │  ├─ api.py          # <----- Archivo donde puedes configurar el puerto
   │  ├─ ...
   └─ ...
```

> En futuras versiones se añadirá soporte para configurar estos parámetros de forma más sencilla (por ejemplo, directamente desde `mambo_config.json` o variables de entorno adicionales).


---

## Problemas comunes

* **La aplicación no arranca o se cierra inmediatamente**
  * Revisa el archivo `.env` y confirma que:
    * La base de datos PostgreSQL está en ejecución.
    * `DB_NAME`, `DB_USER`, `DB_PASSWORD`, `DB_HOST` y `DB_PORT` son correctos.
    * `JWT_SECRET_KEY` tiene un valor seguro y no está vacío.
* **Error de conexión a la base de datos**
  * Verifica que puedes conectarte a tu instancia de PostgreSQL con exactamente las mismas credenciales que has indicado en el `.env`.
* **Problemas con Google OAuth2 / Gemini**
  * Confirma que tus claves (`GOOGLE_CLIENT_ID`, `GOOGLE_SECRET_KEY`, `GOOGLE_GEMINI_API_KEY`, etc.) son correctas y que las URLs de callback (`GOOGLE_REDIRECT_CALLBACK_URI`) coinciden con lo configurado en la consola de Google.


---

## Estado del proyecto y soporte

Este proyecto se encuentra en un estado de desarrollo **temprano**, por lo que es posible que existan:

* Bugs
* Patrones mejorables
* Falta de opciones de configuración o accesibilidad

La idea es ir refinando y ampliando **MAMBOFRAME** conforme surjan nuevas necesidades reales en proyectos de producción.

Si necesitas soporte directo, puedes contactarme vía **Discord**:

> [Servidor de Discord](https://discord.gg/bTh46UCsmq)

Cualquier *issue*, sugerencia de mejora o *pull request* es más que bienvenida.