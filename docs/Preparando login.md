# Editando el menu lateral

Framework7 tiene dos menús de los que tomaremos ventaja para la colocar nuestros botones, en este caso vamos a crear dos paginas:

+ Login/Iniciar sesión, Nos permitirá iniciar sesión con nuestras credenciales que comprobaremos con nuestra API.
+ Registrar Haciendo uso del API vamos a crear usuarios con sus respectivas contraseñas que permitirán el uso completo del sistema.

# Crear la pagina de Registro

Crear un archivo de plantilla dentro de la carpeta `src/pages`

# Archivo `registrar.f7.html`

```html
<template>
    <div class="page" data-name="register-page">
        <div class="navbar">
            <div class="navbar-bg"></div>
            <div class="navbar-inner sliding">
                <div class="left">
                    <a href="#" class="link back">
                        <i class="icon icon-back"></i>
                    </a>
                </div>
                <div class="title">Registrar usuario</div>
            </div>
        </div>
        <div class="page-content">
            <div class="block-title">Registrar en el sistema</div>
            <div class="list no-hairlines-md">
                <ul>
                    <li>
                        <div class="item-content item-input">
                            <div class="item-inner">
                                <div class="item-title item-label">Correo</div>
                                <div class="item-input-wrap">
                                    <input type="email" placeholder="Ingresa tu correo electrónico" />
                                </div>
                            </div>
                        </div>
                    </li>
                    <li>
                        <div class="item-content item-input">
                            <div class="item-inner">
                                <div class="item-title item-label">Contraseña</div>
                                <div class="item-input-wrap">
                                    <input type="password" placeholder="Contraseña valida" />
                                </div>
                            </div>
                        </div>
                    </li>
                    <li>
                        <div class="item-content item-input">
                            <div class="item-inner">
                                <div class="item-title item-label">Repetir contraseña</div>
                                <div class="item-input-wrap">
                                    <input type="password" placeholder="Contraseña anterior" />
                                </div>
                            </div>
                        </div>
                    </li>
                </ul>
            </div>
            <p class="row">
                <a href="#" class="col button">Registrar</a>
            </p>
        </div>
    </div>
</template>
<script>
    export default () => {
        return $render;
    };
</script>
```

Este es la estructura básica usando una plantilla de Framework7, se crean las barras de navegación y un area para el formulario, en este caso hacemos uso de dos campos, uno de tipo email para el correo y otro de tipo password para la contraseña, esta por motivos de seguridad se le pide al usuario que la ingrese dos veces.

Con la pagina creada es momento de agregarla al archivo `routes.js` que esta ubicado en `src/js` para que sea accesible para el sistema.

Al inicio del archivo `routes.js` importamos la pagina usando el siguiente código:

```javascript
import RegistrarPage from '../pages/registrar.f7.html';
```

Ahora se tiene que agregar al `array` de `routes` que esta creado usando la siguiente estructura:

```javascript
  {
    path: '/registrar-page/',
    component: RegistrarPage,
  },
```

_Ten en cuenta que esta entrada es preferible que este en el primer y ultimo elemento del arreglo, es decir, luego del componente `/` y antes del componente `(.*)` y recuerda la coma que separa los elementos del arreglo_


Con esta entrada le estamos especificando al sistema que cuando se detecte una URL con la ruta `/registrar-page/` (que puede ser invocada desde un `href` o desde Javascript) se cargue el componente y se muestre.

# Editar el archivo `app.f7.html` ubicado en la carpeta `src/`

Bajamos en el archivo hasta ubicar el apartado:

```html
<div class="block-title">Control Main View</div>
```

Esta sección carga las paginas en la misma vista del app sin crear sub-ventanas.


Podremos ver un elemento `div` con clase `list`, esta es donde agregaremos nuestra pagina; se agrega lo siguiente al final de la lista:

```html
<li>
    <a href="/registrar-page/" data-view=".view-main" class="panel-close">Registrar usuario</a>
</li>
```

Una ves realizado esto tendremos que ver que dentro de nuestro menu izquierdo tenemos un nuevo elemento con el nombre de **Registrar usuario** con el cual podremos interactuar, este nos mostrara el formulario que acabamos de crear.

Resta conectar este elemento gráfico o de GUI con el API.

# Comprendiendo los endpoint

Un endpoint es básicamente una petición en forma de url que nosotros realizamos a una API que funciona como una interfaz o medio de comunicación entre la base de datos y una aplicación, su función principal es la de manejar datos, pueden ser ediciones, ingresos, borrados o ediciones de las mismas, ademas podemos usarlas para realizar trabajos que no pueden ser posibles para el cliente. 

En nuestro caso tenemos un endpoint que nos permite registrar un usuario en la siguiente url:

> `http://localhost/Framework7-api/crear.php?correo=foo@bar.com&password=foo`

Para hacerla funcional tenemos que realizar una petición AJAX a esta.

Se implementara desde Javascript; al final de la plantilla `registrar.f7.html` tenemos un apartado llamado `<script>` aquí es donde pondremos la llamada del registro.

Primero tenemos que agregar algunas variables que usaremos dentro del script, notaras un apartado con esta estructura

> `export default ()`

Al inicio del código, lo cambiaremos por esta:

```javascript
export default (props, { $, $f7, $f7router, $update })
```

Esto lo que nos permite es usar métodos jquery (selectores), métodos del framework (peticiones ajax), entre otras cosas puedes encontrar mas información dentro de la documentación de Framework7

Luego de esto es vamos a crear una variable donde guardaremos la url a la que estaremos haciendo las peticiones:

```javascript
const baseUrl = "http://localhost/Framework7-api/";
```

En caso de que tengas el proyecto en otro servidor (como en un compartido o en otra carpeta) puedes hacer el cambio aquí.

Agregaremos la siguiente función debajo de este:

```javascript
        const registrarUsuario = () => {
            let correo = $('#correo').val();
            let password = $('#password').val();
            let password2 = $('#password2').val();

            if (correo.length >= 3 && password === password2) {
                let url = baseUrl + 'crear.php';
                $f7.request.json(url, {
                    correo: correo,
                    password: password
                })
                    .then(function (res) {
                        console.log(res.data);

                        let data = res.data;
                        if (data.ingreso) {
                            $f7.dialog.alert('Usuario registrado correctamente');
                            $f7router.back();
                        }
                    })
                    .catch(function (err) {
                        console.log(err.xhr);
                        console.log(err.status);
                        console.log(err.message);
                        $f7.dialog.alert('No se pudo establecer conexión con el API');
                    });
            }
            else {
                $f7.dialog.alert('Formulario incorrecto');
            }
        }
```

Esta función sera invocada dentro del evento click del botón registrar, lo que hace es tomar los valores del formulario los guarda en variables, los comprueba y hace la petición al archivo `crear.php` dentro del api. En caso de que todo este correcto se genera una alerta y se regresa a la pagina anterior, si hay un error en el api se mandan mensajes de error a la consola y se avisa al usuario por una alerta y en caso de que el correo tenga menos de 3 caracteres o que las dos contraseñas no sean igual se le notifica al usuario que tiene incorrecto el formulario.

Ahora queda editar 3 cosas dentro de la plantilla existente, dar ID a los `input` del formulario e invocar la función al hacer click al botón.

```html
<input id="correo" type="email" placeholder="Ingresa tu correo electrónico" />
```
Se agrega el atributo `id="correo"`

```html
<input id="password" type="password" placeholder="Contraseña valida" />
```

```html
<input id="password2" type="password" placeholder="Contraseña anterior" />
```

_Recuerda ir por cada uno de ellos agregando su atributo ID_

Ahora al botón de registro lo cambiaríamos por esto:

```html
<a href="#" class="col button" @click=${registrarUsuario}>Registrar</a>
```

La sintaxis es: `@click=${<nombre_del_método>}`

# NOTA PARA EL API

Ademas de los cambios para este proyecto es necesario editar el archivo `crear.php` dentro del proyecto de `Framework7-api` para que permita la conexion desde `Framework7-base`, dentro del archivo `crear.php` agregamos este encabezado:

```php
header('Access-Control-Allow-Origin: *');
```

Se puede agregar abajo del `require_once`