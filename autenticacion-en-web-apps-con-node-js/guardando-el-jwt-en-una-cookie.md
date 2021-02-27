# Guardando el JWT en una Cookie

Como sabemos, uno de los mecanismos de autenticación más comunes para aplicaciones webs desacopladas, APIs, etc. es mediante un JSON Web Token.

Cuando estamos desarrollando una SPA puede resultarnos muy fácil utilizar el localStorage o el sessionStorage para persistir nuestro token en el lado del cliente. Aunque esto puede ser fácil, tiene un costo de seguridad considerable. Por lo tanto, un mecanismo más seguro para persistir nuestro token de autorización, es guardarlo en una cookie. Veamos cómo podemos hacer esto en un app con backend en Express.

Lo primero que debemos hacer es instalar el middleware de express cookie-parser.

Adicionalmente, instalaremos los módulos básicos para nuestro servidor web.

```bash
npm i express express-jwt jsonwebtoken cors cookie-parser --save
```

Este middleware inyecta las cookies provenientes en el request desde el cliente a nuestro objeto req. Este middleware facilita la lectura de las cookies al procesar solicitudes a nuestros endpoints.

Posteriormente podemos crear nuestra cookie una vez el token ha sido firmado y la cookie contendrá dicho token.

```javascript
// server.js

const express = require('express');
const jwt = require('express-jwt');
const jsonwebtoken = require('jsonwebtoken');
const cors = require('cors');
const jwtSecret = process.env.SECRET;
const app = express();

app.use(cors());
app.use(cookieParser());
app.use(
  jwt(
  { 
    secret: jwtSecret, 
    algorithms: ['HS256'],
    getToken: req => req.cookies.token
  }));

app.post('/login', (req, res) => {
    let authenticatedUser;
    // implementación del mecanismo de autenticación. buscar las credenciales en la base de datos u otro método, etc.
    // al finalizar el mecanismo anterior, pasamos a asignarle un valor a authenticatedUser
      const token = jsonwebtoken.sign(authenticatedUser, jwtSecret);
      
      res.cookie('token', token, { httpOnly: true });
      
      res.json({ success: 'welcome' });
});

app.listen(3000);
```

la línea 19 es la clave en este punto. En esta instrucción estamos logrando 2 objetivos:

1. Estamos creando un header en la respuesta que se enviará al cliente. Este header es _**Set-Cookie**_ y contendrá el token generado. A partir de aquí, todos los requests http provenientes del cliente, tendrán el token en las cookies y servirá para validar a nuestro usuario.
2. La bandera _**httpOnly**_ servirá para prevenir el acceso a esta cookie mediante javascript, adicionalmente sólo podrá ser usada en el protocolo http al mismo origin del que provino, mismo servidor e incluso mismo puerto. _****_

### ¿cómo validamos el token una vez comencemos a recibir request desde el cliente autenticado?

Es muy sencillo, hagamos un zoom a nuestro código

```javascript
app.use(
  jwt(
  { 
    secret: jwtSecret, 
    algorithms: ['HS256'],
    getToken: req => req.cookies.token
  }));
```

Como notamos, para que el middleware express-jwt , valide el token alojado en las cookies, es necesario una función corta que le avise al middleware dónde debe ir a buscar el token, en este caso en el objeto req en el atributo cookies, subatributo token \(recordemos que cookie-parser se encarga de hacer la inyección de las cookies en el objeto req\).

De esta manera podemos utilizar nuestros JWT alojándolos en Cookies.

