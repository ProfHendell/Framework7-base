# Requisitos

Para desarrollar usando este repositorio es necesario tener instalado lo siguiente:

+ Visual Studio Code
+ NodeJS
+ Framework7-CLI
+ Google Chrome
+ XAMPP con el proyecto Framework7-api configurado y listo
+ Cordova (Opcional para compilar APK)
+ Android Studio SDK (Opcional para compilar APK)

# Clonar proyecto

_Este repositorio es el proyecto base de una aplicación híbrida de Framework7 creada usando Framework7-cli, puedes encontrar mas información aquí: https://framework7.io/cli/_

_Toda la documentación necesaria es accesible desde la pagina oficial de Framework7, a la cual puedes acceder aquí: https://framework7.io/docs/_

+ Abre un símbolo del sistema (CMD) `CTRL + R` -> `cmd` -> `ENTER`
+ Entra a la carpeta que deseas copiar el proyecto, es recomendable usar el escritorio para tener fácil acceso a ellos; `cd Desktop` también puedes crear una carpeta para ordenar todo mejor usando el comando `mkdir Repositorios`
+ Escribe el comando `git clone https://github.com/ProfHendell/Framework7-base.git` Este creara una carpeta llamada `Framework7-base`

# Ejecutar el proyecto

Abre el proyecto usando Visual Studio Code, `File` -> `Open folder` -> `Framework7-base`

El árbol de archivos es el siguiente:

Dentro de la carpeta `src` encontraras todo el código de la aplicación es el que vamos a editar para adaptarla a nuestras necesidades.

`src/css` Contiene los estilos.
`src/js` Contiene archivos Javascript, como las rutas, módulos y demás lógica de la aplicación.

`src/pages` Contiene las plantillas de las distintas paginas que usa la aplicación, estas se definen dentro del archivo `src/js/rutes.js` para que el framework las identifique.

`src/app.f7.html` Contiene la estructura básica de la vista de la App.

`src/index.f7.html` Es la estructura básica de HTML de la App.

Ejecuta una terminal (puede ser la misma de Visual Studio Code) y dentro de la carpeta del proyecto ejecuta el siguiente comando:


> `npm run start`

Esto iniciara el servidor desde NodeJS el cual detectara los cambios realizados en la carpeta `src/` y recopilará el proyecto usando `webpack` de manera automática, este comando es posible que inicie una nueva pestaña de Google Chrome a no ser asi puedes ingresar desde tu navegador desde esta url 

> `http://localhost:8080/`

También es muy recomendable iniciar Chrome devtools lo podemos iniciar presionando la tecla `F12` o haciendo click derecho sobre la pagina y elegir la opción **inspeccionar** (Desde aquí puedes activar la vista movil)