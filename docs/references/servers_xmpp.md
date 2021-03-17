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

## Isode M-Link

## jackal

## Metronome IM

## MongooseIM

## Openfire

## Prosody IM