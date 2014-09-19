.. Apuntes de móviles documentation master file, created by
   sphinx-quickstart on Thu Sep 18 23:23:41 2014.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

Prog. multimedia y de dispositivos móviles.
==============================================

Índice:

.. toctree::
   :maxdepth: 2
   
Análisis de tecnologías para aplicaciones en dispositivos móviles:
=======================================================================

Puntos iniciales
-----------------


* En primer lugar, en http://10.8.0.253 se puede encontrar un servidor.

* Los apuntes también se podrán encontrar a diario en http://oscarmaestre.github.io

* Existen fotocopias con la programación, criterios, etc... en la mesa del profesor. En cualquier caso, están colgadas en la página del centro http://www.iesmaestredecalatrava.es (buscar el apartado "Presentaciones"

* Si se desea acceder a algún fichero individual de los apuntes puede hacerse en las página siguiente

	* https://github.com/OscarMaestre/Moviles
	* https://github.com/OscarMaestre/ServiciosYProcesos


El desarrollo para la telefonía móvil es un campo que se encuentra en plena expansión. El número de teléfonos no deja de crecer y las necesidades de programación de los mismos tampoco. En ese sentido existen diversas plataformas de desarrollo a tener en cuenta al empezar a programar.

* Android: es el más numeroso de lejos. La mayor parte del mercado usa esta plataforma. El hecho de que Google ofrezca *completamente gratis* el sistema operativo para los fabricantes y el entorno para los programadores lo ha hecho crecer hasta desbancar a su competidor. Google solo se ocupa de la venta de apps y ese es su nicho de beneficios.
* iOS: Utiliza una filosofía completamente distinta que es controlar todo el proceso desde el desarrollo hasta la distribución de aplicaciones. Ese control se hace por medio del pago de licencias y de la unicidad de la distribución.
* Windows Phone: es el sistema de Microsoft que ha lanzado más o menos recientemente y que aún está por ver si consigue una cuota de mercado significativa o no.
* FirefoxOS: se centra principalmente en mercados emergentes (con el objetivo a largo plazo de ganar cuota de mercado) su filosofía es la misma del software libre.
* Bada: Es la plataforma de Samsung creada exclusivamente para sus teléfonos. Es muy poco usada fuera de Corea del Sur.
* Symbian: surge de un antiguo sistema creado para las PDA, está prácticamente descatalogado en el desarrollo para telefonía móvil.
* Tizen: una plataforma relativamente nueva. Al igual que Android es gratis y además tiene el respaldo de Intel.
* Jolla: surge en los países nórdicos con una filosofía similar al software libre pero con la salvedad de que, de momento, solo se ejecuta en sus teléfonos (que son de gama alta)

En cuanto a la tecnología hay diferencias sustanciales entre ellas:


* Android: pensado principalmente para ser programado en Java (aunque se puede llegar a usar C++ con un kit aparte).
* iOS: usa Objective-C que lo separa mucho del resto de plataformas. El entorno exige el pago de una licencia, el SO exige una licencia y el poner aplicaciones a la venta exige otra.
* Windows Phone: usa Visual Studio que es una herramienta muy potente y usa la plataforma .NET.
* FirefoxOS: usa HTML y Javascript.
* Symbian y otros también permiten el uso de HTML y JS.
* Tizen permite dos opciones: C++ o HTML/JS
* Jolla/Sailfish: usa C++.
* Bada usa C++.

En este curso se usara Android con Java como lenguaje de desarrollo.
















Limitaciones: desconexión, seguridad, memoria, consumo batería, almacenamiento.
-------------------------------------------------------------------------------


Sistemas operativos para dispositivos móviles. Características.
-------------------------------------------------------------------------------

Entornos integrados de trabajo.
------------------------------------------------------

Módulos para el desarrollo de aplicaciones móviles.
------------------------------------------------------

Emuladores.
------------------------------------------------------

Configuraciones. Tipos y características. Dispositivos soportados.
-------------------------------------------------------------------------------

Perfiles. Características. Arquitectura y requerimientos. Dispositivos soportados.
----------------------------------------------------------------------------------

Ciclo de vida de una aplicación
------------------------------------------------------

Descubrimiento
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Instalación
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Ejecución
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Actualización
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Borrado.
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Modificación de aplicaciones existentes.
------------------------------------------------------

Utilización del entorno de ejecución del administrador de aplicaciones.
-------------------------------------------------------------------------------

Programación de aplicaciones para dispositivos móviles
========================================================

Herramientas y fases de construcción.
------------------------------------------------------

Interfaces de usuario. Clases asociadas.
------------------------------------------------------
Servicios en dispositivos móviles.
------------------------------------------------------
Proveedores de contenido.
------------------------------------------------------
Gestión de recursos y notificaciones.
------------------------------------------------------
Contexto gráfico. Imágenes.
------------------------------------------------------
Eventos del teclado.
------------------------------------------------------
Técnicas de animación y sonido.
------------------------------------------------------
Descubrimiento de servicios.
------------------------------------------------------
Bases de datos y almacenamiento.
------------------------------------------------------
Persistencia.
------------------------------------------------------
Modelo de hilos.
------------------------------------------------------

Comunicaciones: clases asociadas. Tipos de conexiones.
------------------------------------------------------
Gestión de la comunicación inalámbrica.
------------------------------------------------------
Seguridad y permisos.
------------------------------------------------------
Envío y recepción de mensajes texto.
------------------------------------------------------
Envío y recepción de mensajería multimedia. Sincronización de contenido.
-------------------------------------------------------------------------------
Manejo de conexiones HTTP y HTTPS.
------------------------------------------------------
Empaquetado y despliegue de aplicaciones para dispositivos móviles.
-------------------------------------------------------------------------------
Centros de distribución de aplicaciones.
------------------------------------------------------
Documentación de aplicaciones de dispositivos móviles.
------------------------------------------------------


Utilización de librerías multimedia integradas
===================================================

Conceptos sobre aplicaciones multimedia.
------------------------------------------------------

Arquitectura del API utilizado.
------------------------------------------------------

Fuentes de datos multimedia. Clases.
------------------------------------------------------

Datos basados en el tiempo.
------------------------------------------------------

Procesamiento de objetos multimedia. Clases. Estados, métodos y eventos.
-------------------------------------------------------------------------------

Reproducción de objetos multimedia. Clases. Estados, métodos y eventos.
-------------------------------------------------------------------------------

Depuración y documentación de los programas.
------------------------------------------------------

Análisis de motores de juegos
===================================================

Animación 2D y 3D.
------------------------------------------------------

Arquitectura del juego. Componentes.
------------------------------------------------------

Motores de juegos: Tipos y utilización.
------------------------------------------------------

Áreas de especialización, librerías utilizadas y lenguajes de programación
--------------------------------------------------------------------------

Componentes de un motor de juegos.
------------------------------------------------------

Librerías que proporcionan las funciones básicas de un Motor 2D/3D.
--------------------------------------------------------------------------


APIs gráficos 3D.
------------------------------------------------------

Estudio de juegos existentes.
------------------------------------------------------

Aplicación de modificaciones sobre juegos existentes.
------------------------------------------------------

Desarrollo de juegos 2D y 3D
===================================================


Entornos de desarrollo para juegos.
------------------------------------------------------

Integración del motor de juegos en entornos de desarrollo.
--------------------------------------------------------------

Conceptos avanzados de programación 3D.
------------------------------------------------------

Fases de desarrollo:
------------------------------------------------------

Propiedades de los objetos: luz, texturas, reflejos, sombras.
---------------------------------------------------------------------

Aplicación de las funciones del motor gráfico. Renderización.
----------------------------------------------------------------

Aplicación de las funciones del grafo de escena. 
------------------------------------------------------


Tipos de nodos y su utilización.
---------------------------------------------------------------------------------

Asociación de sonidos a los eventos del juego.
------------------------------------------------------

Análisis de ejecución. Optimización del código.
------------------------------------------------------

Documentación de la fase de diseño y de desarrollo de los juegos.
------------------------------------------------------

Sistemas basados en localización
===================================================


Tecnologías de localización (GPS, A-GPS,...).
------------------------------------------------------

Servicios de localización, mapas y geocodificación.
------------------------------------------------------

Emuladores para simular las ubicaciones.
------------------------------------------------------

Mecanismos para visualizar la información geolocalizada.
------------------------------------------------------
