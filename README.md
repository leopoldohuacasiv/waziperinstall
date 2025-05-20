# Instalación WAZIPER WHATSAPP

Waziper es un software de marketing de WhatsApp basado en scripts PHP. Es una solución ideal para empresas, software como servicio (SaaS) y especialistas en marketing digital. Permite enviar mensajes masivos, respuestas automáticas, chatbot, envío de archivos y exportación de contactos.

Este documento proporciona los pasos detallados en Español para instalar Waziper de forma correcta desde entorno VPS. Al no existir información en Español esta guia es de gran ayuda para implementarlo

---

## 1. Configurar DNS desde el proveedor de compra
- Crear un registro DNS tipo "A" que apunte al IP del servidor VPS.
- Crear un subdominio llamado "API" y direccionarlo al IP del servidor VPS.

## 2. Acceder al servidor VPS vía SSH
```sh
ssh root@999.999.999
```
- Ingresar la contraseña y ejecutar la instalación de aapanel:
```sh
wget -O install.sh http://www.aapanel.com/script/install-ubuntu_6.0_en.sh && sudo bash install.sh aapanel
```
- Instalar los siguientes paquetes:
  - PHP 7.4
  - MySQL 5.7.44
  - Node.js Manager 2.2
  - Redis 7.2.4
  - phpMyAdmin 4.9
  - Nginx 1.2.4

## 3. Configuración de Dominio en aapanel
- Ir a **Website** > **ADD SITE**
- Completar los datos:
  - Nombre de dominio
  - Descripción
  - Base de datos: **MySQL**
  - Configuración de base de datos (nombre + contraseña)
  - Versión de PHP: **7.4**

## 4. Activar certificado SSL
- Para cada sitio web creado:
  - Ir a **Configuración** > **SSL** > **Let's Encrypt**
  - Seleccionar el dominio y presionar **APPLY**
  - Confirmar en **APPLY AND OPEN**

## 5. Instalación de WAZIPER
- Acceder a la carpeta **Document root** del sitio web principal.
- Eliminar todos los archivos y carpetas existentes.
- Subir el instalador comprimido de Waziper y descomprimirlo.
- Ubicar el archivo `install.zip`, moverlo a la carpeta principal y descomprimirlo.

## 6. Instalación de Base de Datos
- Ir a **Databases** > **phpMyAdmin** > **Public access**
- Acceder con las credenciales de la base de datos.
- Seleccionar la base de datos y realizar la importación desde `database.sql`.

## 7. Configuración del archivo .env
- Editar el archivo `.env` en la carpeta principal y actualizar:
```env
database.default.database = nombredetubasededatos
database.default.username = nombredeusuario
database.default.password = contraseñabasedatos
```

## 8. Configuración de PHP 7.4
- Ir a **APP STORE** > **PHP 7.4** > **Configuración**
- Eliminar `putenv` en **DISABLED FUNCTIONS**

## 9. Configuración de Node.js
- Instalar la versión `v20.18.0` desde **Node.js version manager**
- Seleccionar la versión en **Command line version**

## 10. Instalación del WASERVER
- Crear la carpeta `/www/waserver` y mover `waserver.zip` a esta ubicación.
- Descomprimir `waserver.zip`.
- Editar `config.js` con los siguientes valores:
```js
frontend: "https://tudominioprincipal.com"
redis: "redis://:@127.0.0.1:6379"
user: nombredeusariobasedatos
password: contraseñabasedatos
database: nombrededatos
```
## 11. Configuración del sitio web API
- En **Website**, acceder a la configuración del sitio `api.tudominio.com`
- Ir a **Reverse proxy** y agregar:
  - Proxy name: `Waserver`
  - Target URL: `http://127.0.0.1:9000`

## 12. Redireccionamiento al dominio principal
- En **Website**, acceder a **URL REWRITE** e insertar:
```nginx
location / {
    try_files $uri $uri/ /index.php;
}
```

## 13. Inicio de sesión en la web principal
- Acceder al dominio principal e iniciar sesión:
  - Usuario: `demoapp`
  - Contraseña: `123456`
- Ir a **Settings** > **Social Network Settings** > **WhatsApp**
- Deshabilitar "Enable Login" y configurar `whatsapp server url` con `api.tudominio.com`

## 14. Configuración desde la terminal SSH
```sh
cd /www/waserver
pm2 start app.js
pm2 save
pm2 startup
pm2 save
```

## 15. QR de WhatsApp
- Ir a **Account Manager** y agregar una cuenta.
- Si no se genera el QR:
  - Borrar el registro en `sp_whatsapp_sessions`.
  - Reiniciar el servidor y volver a intentarlo.

### Fin de la instalación

## Dato Extra

Para descargar el código fuente accede al grupo de whatsapp: [Sistemas con WhatsApp](https://chat.whatsapp.com/HR9PZZLqsRHAP8ZA8s0H5G)

Y escribe la palabra clave en el chat del grupo "waziperglite" para que el BOT te comparta el link de descarga

