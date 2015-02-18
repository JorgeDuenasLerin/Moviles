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
