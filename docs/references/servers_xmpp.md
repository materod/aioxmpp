# Servidores XMPP

Listado de servidores XMPP disponibles en https://xmpp.org/software/servers.html.

## [AstraChat](https://www.astrachat.com/)

No parece que este el codigo fuente disponible.

## [ejabberd](https://www.ejabberd.im/)

**Código fuente**: https://github.com/processone/ejabberd

**Documentación**: https://docs.ejabberd.im/developer/guide/

**Lenguaje**: Earlang

**Plataformas**: Windows, Linux, FreeBSD, NetBSD.

**Características**:
* Docker packaging.
* Internalización.
* Fully XMPP-compliant.
* Modular (permite extensiones).
* Seguridad SASL, STARTTLS.
* Soporta las siguientes bases de datos: Mnesia, MySQL, PostgreSQL, ODBC, Microsoft SQL Server.
* Autenticación: PAM, LDAP, ODBC, Interna o Externa con un script.
* Soporta virtual hosting.
* Compresion XML.
* Soporta ipv6.
* Dispone de API rest para comunicarse con clientes que no usan el protocolo XMPP (por ejemplo smartwatchs).

### Arquitectura

El **Core** del servidor se compone de las siguientes capas:

* **Network Layer**: Se encarga de las conexiones entrantes. Esta compuesto por 3 modulos: **ejabberd_listener**, **ejabberd_receiver** y **ejabberd_socket**. Una vez el **ejabberd_listener** acepta una conexión, se lanza una instanacia del **ejabberd_receiver** que se encarga del socket, realizando las siguientes operaciones:
    - Acelera la conexión utilizando _shapers_ con el módulo **shaper**.
    - Realiza el descifrado TLS con la libreria **fast_tls**.
    - Descomprime el stream usando la librería **ezlib**.
    - Parsea el XML usando la librería **fast_xml**.
    
    El módulo **ejabberd_socket** hace lo mismo que el receiver pero en orden inverso. El objeto **xmlel** obtenido de parsear el xml se pasa a la siguiente capa.

* **Stream Layer**: Esta capa se compone de los modulos **xmpp_stream_in** y **xmpp_stream_out**. Se crea una instancia del **xmpp_stream_in** por cada instancia del **ejabberd_receiver**, que le pasa el objeto **xmlel** parseado. Con este objeto el módulo **xmpp_stream_in** realiza las siguientes operaciones:
    - Codifica/Decodifica los paquetes usando la librería **xmpp**.
    - Realiza la negociación de los streams XMPP entrantes.
    - Realiza la negociación STARTTKS si es necesario.
    - Realiza la negociación de la compresión si es necesario.
    - Realiza la autenticación SASL.

    El módulo **xmpp_stream_out** realiza las mismas operaciones para los streams salientes.

* **Routing Layer**: Se compone de un **ejabberd_router** que se encarga de enrutar las _stanzas_. Las _stanzas_ se gestionan con distintos módulos segun la ruta a la que vayan dirigidas:
    - Ruta local: Cuando las _stanzas_ estan destinadas al propio servidor. Se gestiona con el módulo **ejabberd_local**.
    - Session manager: Cuando las _stanzas_ estan destinadas a usuarios locales del servidor. Se gestionan con el módulo **ejabberd_sm**.
    - Ruta registrada: Cuando las _stanzas_ estan destinadas a otros servidores que estan registrados en una _look up table_. El propio módulo **ejabberd_router** permite registrar las estas rutas en la base de datos configurada.
    - Resto de rutas: Cuando las _stanzas_ no estan destinadas otros servidores que no estan registrados. El módulo **ejabberd_s2s** gestiona este proceso.

Este servidor permite añadir nueva funcionalidad de varias formas:

* **IQ Handlers**: Permiten procesar _iq stanzas_.

* **Hooks**: Permiten procesar eventos arbitrarios, modificando el comportamiento de ejabberd.

* **Modulos**: El servidor es modular, por lo que se pueden implementar nuevos módulos (con o sin estado).

## [Isode M-Link](https://isode.com/products/m-link.html)

No esta disponible el codigo fuente

## [jackal](https://github.com/ortuman/jackal)

**Código fuente**: https://github.com/ortuman/jackal

**Lenguaje**: Go

**Plataformas**: OS X, Linux

**Características**:
* Customizable
* SSL/TLS reforzado.
* Compresión de stream.
* Conexión a base de datos (PostgreSQL)
* Clustering.
* Metricas con [prometeus](https://prometheus.io/)
* Extensible.

No hay mucha documentación y no parece tan bien estructurado como ejabberd. Una de las cosas que critican de este servidor es que no soporta XEP-0198, que sirve para no perder mensajes cuando se interrumpe la conexión. El problema es que una vez esta implementado el servidor es mas dificil implementarlo que si se hace con esto en mente desde el principio. En cambio ejabberd si lo soporta.

## [Metronome IM](https://metronome.im/)

**Código fuente**: https://github.com/maranda/metronome

**Lenguaje**: Lua

**Plataformas**: Linux

**Características**:

Es un fork de Prosody, sin plugins de la comunidad un poco mas liviano.

## [MongooseIM](https://www.erlang-solutions.com/capabilities/mongooseim/)

**Código fuente**: https://github.com/esl/MongooseIM

**Documentación**: https://esl.github.io/MongooseDocs/latest/

**Lenguaje**: Earlang

**Plataformas**: Linux, Mac OSX

**Características**:

* Tolerancia a fallos.
* Clustering. Escalable.
* Acepta conexiones de cliente sobre:
    * TCP (with TLS/STARTTLS) -> XMPP (RFC 6120)
    * REST API 
    * WEBSockets (RFC 7395)  
    * BOSH (XEP-0124, XEP-0206)
* Metricas y monitorización.
* Persistencia con base de datos Mnesia, MySQL, PostgreSQL, Riak KV.
* Modular

Parece ser un fork de ejabberd, por lo que la arquitectura es basicamente la misma.

Nota: Gestion de GDPR -> https://esl.github.io/MongooseDocs/latest/operation-and-maintenance/gdpr-considerations/

## [Openfire](https://www.igniterealtime.org/projects/openfire/)

**Código fuente**: https://github.com/igniterealtime/Openfire

**Documentación**: https://www.igniterealtime.org/projects/openfire/documentation.jsp

**Lenguaje**: Java

**Plataformas**: Linux / macOS / Solaris / Windows

**Características**:

* Soporta WebSocket -> RFC 7395.
* Soporta streams bidireccionales -> XEP 0124
* Soporta BOSH -> XEP 0206
* Sistema de plugins

**Arquitectura**:

Se parece a ejabberd (puede que este basado en este), sin embargo parece que no esta tan estructurado.

* org.jivesoftware.openfire.auth -> Autenticación.
* org.jivesoftware.openfire.handler -> Implementación de IQ.
* org.jivesoftware.openfire.net -> Gestión de mensajes de red: Socket / Stanzas / Streams.
* org.jivesoftware.openfire.nio -> Gestión de conexiones. 
* org.jivesoftware.openfire.server -> Gestión de comunicación entre servidores.
* org.jivesoftware.openfire.session -> Gestión de sesiones.
* org.jivesoftware.openfire.transport -> Enruta los mesajes.

## [Prosody IM](https://prosody.im/)

**Código fuente**: https://hg.prosody.im/trunk (unofficial https://github.com/bjc/prosody)

**Documentación**: https://prosody.im/doc/developers

**Lenguaje**: Lua

**Plataformas**: Linux / macOS / BSD

**Arquitectura**:

* Core: Modulos con la funcionalidad principal del servidor
    * Gestión TLS y certificados.
    * Gestión de configuración.
    * Virtual Hosts
    * Logging
    * Gestión de modulos/plugins.
    * TCP listeners
    * Gestión de usuarios (lista de contactos)
    * Conexiones servidor a servidor.
    * Sesiones de usuario.
    * Enrutamiento de stanzas.
    * Reports y estadisticas.
    * API para gestion de usuarios.
* Net: 
    * Resolucion DNS
    * Gestión de conexiones.
    * Cliente/Servidor HTTP.
    * Manejo de socket.
    * Manejo de websockets.
* Util: Utilidades para tareas comunes y estructuras de datos.
* Plugins: Implementacion de los XEP y otros módulos propios de Prosody.


## Servidores en Python

### [XMPPD](https://github.com/xmpppy/xmppd)

No se actualiza desde 2009, que a su vez es un fork del de Alexey Nezhdanov de 2004.

### [python-xmpp-server](https://github.com/thisismedium/python-xmpp-server)

También sigue sin actualizarse desde 2012.

### [Wokkel](https://wokkel.ik.nu/)

Parece que es el unico "servidor" de Python que se sigue manteniendo en la actualidad. Parece que no tiene toda la parte de servidor implementada (parace que tienen implementado algo de conexion servidor a servidor, pero no del cliente ¿?) y que esta basado en el RFC 3920.