# Librerias

Librerias de utilidad para la implementación del servidor XMPP

## [asyncio](https://docs.python.org/3/library/asyncio.html)

Es una librería para programación asíncrona en Python. Permite escribir código concurrente utilizando la sintazis _async_/_await_ (como en javascript). Requiere Python 3.7+. Dispone de las siguientes _APIs_:

* High Level API:
    * Corrutinas y Tareas: Existen 3 tipos de objetos _awaitables_: **Corutine** (se usa la sintaxis _async_/_await_), **Tasks** (se crean con **asyncio.create_task**) y **Futures** (se llaman con **asyncio.gather(func, callback)**). Para ejecutar la corutina usaremos **asyncio.run(corutina)**. También existe un **asyncio.sleep** asíncrono. Podemos ejecutar varias corutinas de forma concurrente con **asyncio.gather**. Para evitar que se cancele un corutina podemos llamarla con la función **shield**, sin embargo si queremos establecer un _timeout_ la llamaremos usando **asyncio.wait_for**. También podemos esperar a que una o varias corutinas acaben con **asyncio.wait**. Para ejecutar la corutina en un _thread_ la llamamos con **asyncio.to_thread**.
    * Streams: Permiten enviar y recibir datos de forma asincrona, sin necesidad de callbacks.
    * Primitivas de sincronización: Dispone de **Lock** (Mutex lock), **Event** (Notifica un evento a multiples tareas), **Condition** (Permite a una tarea esperar a un evento para obtener un recurso compartido), **Semaphore**, **BoundedSemaphore** (Es un semaforo que lanza una excepción si el contador interno supera el valor inicial). Estas primitivas no son _thread-safe_, ni disponen de _timeout_. Para indicar un _timeout_ se debe usar la función **asyncio.wait_for()**.
    * Subprocesos: Se puede llamar a un subproceso de forma asincrona usando la función **asyncio.create_subprocess_shell**.
    * Colas: No son _thread-safe_, se deben usar con el _async/await_. Tampoco tienne un _timeout_, para realizar las operaciones con _timeout_ debemos usar la función **asyncio.wait_for()**. Tiene definidos 3 tipos de colas: **Queue (FIFO)**, **PriorityQueue** y **LifoQueue**.

* Low LEvel API:
    * Event Loop: Ejecuta de forma asíncrona las tareas y callbacks, así como los subprocesos y las operaciones de E/S.
    * Futures: Son como las promesas de javascript.
    * Transports & Protocols: Utilidades para la implementación de protocolos de red o IPC.
    * Policies: Define el contexto del _Event Loop_.
    * Platform suppport: Aunque la librería es portable hay algunas limitaciones con algunos sistemas operativos; algunos métodos no están soportados en Windows. Tambien hay algunas limitaciones con el **SelectorEventLoop** en versiones anteriores a 10.8 de MacOS.

Buenas prácticas:
* El modo de depuración de **asyncio** puede ser útil para detectar errores y problemas de rendimiento, por lo que deberiamos tenerlo habilitado durante el desarrollo. 
* Hay que tener en cuenta que la mayoría de objetos de esta librería no son _thread-safe_. Esto no es un problema mientras no ejecutemos código fuera de la Tarea o la callback, ya que estas se ejecutan en el _thread_ principal del event loop. Sin embargo, si tuvieramos que llamar a una corutina en otro hilo, debemos usar la función **loop.call_soon_threadsafe(callback, *args)**.
* Si ejecutamos una tarea que haga un uso intensivo del CPU, el resto de tareas y operaciones se verá retrasado. Para evitar esto, podemos ejevutar la tarea en otro _thread_ utilizando **loop.run_in_executor**.
* Se puede ajustar el nivel de log de **asyncio** configurando el logging _logging.getLogger("asyncio").setLevel(logging.WARNING)_

## [aioxmpp](https://pypi.org/project/aioxmpp/)

Requieres al menos Python 3.4. Parece que es del lado del cliente, pero puede servirnos para probar el server y coger ideas.

Soporta los RFCs y numerosas extensiones XEPs:
* [RFC 4505 - Anonymous Simple Authentication and Security Layer (SASL) Mechanism](https://tools.ietf.org/html/rfc4505.html)
* [RFC 4616 - The PLAIN Simple Authentication and Security Layer (SASL) Mechanism](https://tools.ietf.org/html/rfc4616.html)
* [RFC 5802 - Salted Challenge Response Authentication Mechanism (SCRAM) SASL and GSS-API Mechanisms](https://tools.ietf.org/html/rfc5802.html)
* [RFC 6120 - Extensible Messaging and Presence Protocol (XMPP): Core](https://tools.ietf.org/html/rfc6120.html)
* [RFC 6121 - Extensible Messaging and Presence Protocol (XMPP): Instant Messaging and Presence](https://tools.ietf.org/html/rfc6121.html)
* [RFC 6122 - Extensible Messaging and Presence Protocol (XMPP): Address Format](https://tools.ietf.org/html/rfc6122.html)

## [slixmpp](https://pypi.org/project/slixmpp/)

