# Intro

Actualmente el stack de seguridad para aplicaciones en node js y otras tecnologías web está compuesto por

* JWT.
* OAuth 2.0: Estandar que permite implementar autorización \(no es lo mismo que autenticación\).
* OpenID Connect: Capa adicional de autenticación.

**Autenticación** es la acción de verificar la identidad de un usuario.

**Autorización** es la acción de otorgar permisos limitados de uso a los usuarios.

**Sesión**, es un registro de información que se comparte entre diferentes request de tipo http \(teniendo en cuenta que estas peticiones no tienene estado\), adicionalmente cuando hay un proceso de autenticación se relaciona la sesión con el usuario autenticado. Naturalmente para poder mantener este registro de datos entre peticiones http se necesita una base de datos en memoria preferiblemente, por ejemplo Redis.

### Anatomía JWT

Un token es un estandar del facto que se utiliza como un mecanismo de autenticación para peticiones http, se compone principalmente de 3 partes separadas por un punto.

* **Header:** Contiene el tipo de token, la mayoría de las veces se especifíca como JWT y también en esta parte se describe el tipo de algoritmo de encriptación. Los algoritmos de encriptación pueden ser síncronos o asíncronos, los primeros son algoritmos con los que se encripta y desencripta con la misma llave; los segundos utilizan llave pública y llave privada.
* **Payload**: aquí viene toda la información específica del usuario, como por ejemplos los permisos que tiene el usuario logueado.
* **Firma**: Codificación del header y el payload. Esta codificación se realiza mediante el algortimo expresado en la sección header más un secret.

### Autenticación tradicional vs Autenticación con JWT

#### Tradicional

* Se crea una sesión en el servidor y se manda la información de la sesión al navegador en forma de cookie. A partir de allí, todas las peticiones http llevarán la cookie para poder relacionar la petición con la sesión en el servidor.
* Problemas del enfoque
  * Las SPAs se ven afectadas ya que su ciclo de vida es mayormente en el cliente, por lo tanto no se le notifica ni se da por enterada en cuanto a cambios en la sesión en el backend.
  * REST APIs están pensadas para no tener estado, por ende para no tener sesiones.
  * El control de acceso siempre requiere una revisión en la base de datos, lo que amplia los tiempos de latencia.
  * Mayor consumo de memoria, más usuarios con sesión mayor uso de memoria.

#### JWT

* No se crea una sesión, sólo se firma un token una vez el usuario se ha autenticado.
* Todos los request que realice el cliente una vez recibido el token deben llevar dicho token.
* Ya no hace falta que una SPA trabaje con el backend para saber si el usuario está autenticado. Al tener el token es prueba de que se autenticó en el algún momento del pasado, dependiendo de la expiración del token.
* Se reduce el consumo de memoria del lado del backend ya que simplemente hace falta determinar que el token fue firmado correctamente y está vigente.
* Problemas
  * Es decodificable, por lo tanto debemos evitar agregar información sensible en él. Esto puede incluir contraseñas, keys, nùmeros de cuenta, etc.
  * Si no se establece un tiempo de expiración, podría ser utilizado por un largo período sin ninguna caducidad y constituiría un riesgo de seguridad.
  * No se debe usar el localStorage ni sessionStorage para guardar los tokens, se debe usar memoria o cookie.

### Firmando un JWT

```javascript
//index.js
const jwt = require('jsonwebtoken');

const [, , option, secret, nameOrToken ] = process.argv;

if ( !option || !secret || !nameOrToken) {
    return console.log("Missing arguments");

}

function signToken(payload, secret) {
    return jwt.sign (payload, secret);
}

function verifyToken(token, secret) {
    return jwt.verify(token, secret);
}

if(option == 'sign') {
    console.log(signToken({ sub: nameOrToken}, secret ));
} else if ( option == 'verify') {
    console.log(verifyToken(nameOrToken, secret));
} else {
    console.log('Option needs to be "sign" or "verify"');
} 
```

### Cookie

Las cookies son básicamente registros de datos que permiten mejorar la experiencia de usuario. Entre las cookies podemos mencionar las siguientes, según sus tipos:

* Cookie Session: Son cookies de corta vida, en otras palabras expiran en un corto tiempo. Se remueven cuando se cierra el tab o el navegador.
* Cookies persistentes: Guardan datos por un mayor tiempo y generalmente se usan para capturar información de la experiencia del usuario en la aplicación para mejorar su experiencia al usar el sitio.
* Secured Cookis: guardan la información de manera cifrada para evitar que terceros malintencionados usen esa data. Suelen usarse en https.

Es importante siempre avisarle al usuario acerca del uso de cookies. Es necesario le consentimiento del usuario.

