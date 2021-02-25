# Autenticación en el Front

Actualmente el stack de seguridad para aplicaciones en node js y otras tecnologías web está compuesto por

* JWT.
* OAuth 2.0: Estandar que permite implementar autorización \(no es lo mismo que autenticación\).
* OpenID Connect: Capa adicional de autenticación.

**Autenticación** es la acción de verificar la identidad de un usuario.

**Autorización** es la acción de otorgar permisos limitados de uso a los usuarios.

**Sesión**, es un registro de información que se comparte entre diferentes request de tipo http \(teniendo en cuenta que estas peticiones no tienene estado\), adicionalmente cuando hay un proceso de autenticación se relaciona la sesión con el usuario autenticado. Naturalmente para poder mantener este registro de datos entre peticiones http se necesita una base de datos en memoria preferiblemente, por ejemplo Redis.

### Sesiones



