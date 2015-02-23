Análisis de motores de juegos:
=======================================

Introducción: la biblioteca Gdx
------------------------------------------------------------------------------
En la actualidad existen diversos frameworks y bibliotecas para crear juegos, como por ejemplo

* AndEngine
* Unity
* DirectX
* OpenGL

En este curso se usará la biblioteca **LibGDX** para desarrollar pequeños juegos para plataformas Android.

Animación 2D y 3D.
------------------------------------------------------------------------------

El concepto de animación es bastante sencillo, ya que la idea base se reduce a mostrar un gráfico en pantalla, borrarlo y volver a dibujarlo en una posición ligeramente distinta. Si se hace a la velocidad suficiente se logra transmitir la sensación de movimiento. A estos gráficos animados se les denomina *sprites* . Normalmente la animación implica varias cosas

* Se debe tener presente el tiempo: normalmente un gráfico animado evoluciona a lo largo del tiempo.
* Una cosa es el gráfico mostrado en pantalla y otro el rectángulo que lo encierra. Normalmente, los motores gráficos separan dichos conceptos y nos ofrecen una abstracción del tipo "Rectángulo" que incorpora método como "chocaCon(otroRectángulo)" que nos permiten *detectar colisiones*.
* Puede ser necesario almacenar muchos rectángulos que representen muchos objetos a mostrar en pantalla. Se debe utilizar una clase que permita recorrer rápidamente todos los posibles elementos a dibujar así como eliminarlos sin suponer una gran penalización de tiempo. La clase Java ``Array`` nos permitirá hacer esto. La clase ``Array`` se recorre mediante objetos ``Iterator``-
* Un juego puede ejecutarse en dispositivos de distinta resolución. Sería útil disponer de un "mundo virtual" que tenga un tamaño que nos convenga y que luego la biblioteca se encargue de recalcular las coordenadas (LibGdx lo permite.)

Arquitectura del juego. Componentes.
------------------------------------------------------------------------------

Creación del proyecto
~~~~~~~~~~~~~~~~~~~~~~

En LibGDX se crea el proyecto inicial mediante la herramienta ``gdx-setup.jar``. Este programa crea un proyecto vacío de tipo Gradle que despues podremos importar en Eclipse. Cabe destacar que LibGDX permite crear juegos multiplataforma, aunque aquí se trabajará solo con proyecto Android.

Recursos
~~~~~~~~~
Todos los recursos del juego (animaciones, sonidos), deben almacenarse en el directorio ``assets`` del proyecto. Despues los gráficos se crearán mediante un constructor y los sonidos/músicas con un un método

.. code-block:: java

   /* Carga de un gráfico */
   Texture graficos=new Texture(Gdx.files.internal("sprites.png"));
   /* Carga de un sonido o música */
   Sound sonidoGota= Gdx.audio.newSound(Gdx.files.internal("gota.wav"));
   Music musicaFondo=Gdx.audio.newMusic(Gdx.files.internal("musica.mp3"));

Camara
~~~~~~
La cámara permite establecer un "tamaño de mundo virtual", que es muy cómodo de manejar, ya que con ayuda de otra clase llamada ``SpriteBatch`` permite auto-adaptar el tamaño de los gráficos a la resolución real del dispositivo. La cámara usada en LibGDX se pone en marcha así:

.. code-block:: java

   sonidoGota= Gdx.audio.newSound(Gdx.files.internal("gota.wav"));
   ...
   camara=new OrthographicCamera();
   /*La y positiva no apunta hacia abajo
   y el "mundo" mide 800x480*/
   camara.setToOrtho(false, 800, 480);

Sonidos y música
~~~~~~~~~~~~~~~~
Un sonido puede hacerse escuchar con el método ``play()``. Una música puede ponerse en segundo plano sonando todo el tiempo con el método ``setLooping(true)`` y ejecutando despues el método ``play``.

Interacción con el usuario
~~~~~~~~~~~~~~~~~~~~~~~~~~~
Cuando el usuario toca la pantalla se tiene que procesar la posición donde toca, ya que si se está usando un "mundo virtual", las coordenadas tienen que "volver a convertirse a coordenadas del mundo real". Un método típico es actuar así:

.. code-block:: java

   if (Gdx.input.isTouched()){
      /* Traduciendo a coordenadas "reales" */
       posicion=new Vector3();
       posicion.set(Gdx.input.getX(), Gdx.input.getY(), 0);
       camara.unproject(posicion);
       
    }

Motores de juegos: Tipos y utilización.
------------------------------------------------------------------------------

Áreas de especialización, librerías utilizadas y lenguajes de programación.
------------------------------------------------------------------------------

Componentes de un motor de juegos.
------------------------------------------------------------------------------

Librerías que proporcionan las funciones básicas de un Motor 2D/3D.
------------------------------------------------------------------------------

APIs gráficos 3D.
------------------------------------------------------------------------------

Estudio de juegos existentes.
------------------------------------------------------------------------------

Aplicación de modificaciones sobre juegos existentes
------------------------------------------------------------------------------
