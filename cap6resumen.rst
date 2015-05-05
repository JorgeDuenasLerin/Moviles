OpenGL: resumen
===================================================

Introducción
------------------------------------------------------

Hasta ahora se han visto muchas cosas básicas para empezar a crear una aplicación OpenGL. En este capítulo volveremos a repasar los puntos vistos hasta ahora para poder afianzar los conocimientos antes de avanzar:


El proceso para crear una aplicación consiste en lo siguiente

1. Crear una clase que herede de ``GLSurfaceView`` y añadirla al interfaz de nuestro proyecto
2. Se necesita una clase que implemente el interfaz ``Renderer`` que se ocupará del dibujado. 
3. Se necesitan al menos un vertex shader y un fragment shader.
4. Los vertex shader y los fragment shader hay que compilarlos. Usaremos una clase que nos ayude con esta tarea que implementaremos de una forma distinta.

La clase que hereda de ``GLSurfaceView``
------------------------------------------------------
El código se muestra a continuación:

.. code-block:: java
		
	public class Superficie extends GLSurfaceView {
		public Superficie(Context context, AttributeSet attrs) {
			super(context, attrs);
			/* 8 bits para R, G, B, 16 de profundidad y 0 de stencil*/
			this.setEGLConfigChooser(8, 8, 8, 8, 16, 0);
			this.setEGLContextClientVersion(2);
		}
	}	
	
La clase que implementa``Renderer``
------------------------------------------------------

Tiene como mínimo tres métodos:

* ``onSurfaceCreated`` que crea lo que necesitemos pero que podría ejecutarse varias veces.
* ``onSurfaceChanged`` que se ejecuta cuando se rota la pantalla.
* ``onDrawFrame`` que se ejecuta continuamente y se ocupa del dibujado de elementos.

Los shaders
------------------------------------------------------

Un vertex shader es un programa que se ejecuta para cada punto. Un fragment es un programa que une puntos para crear líneas o superficies. El vertex shader será así:

.. code-block:: c

	attribute vec4 posicion_vertice;
	void main(){
		gl_PointSize=10.0;
		gl_Position=posicion_vertice;
	}

Como cada punto tendrá una posición distinta, la posición es ``attribute``.

El fragment shader es así, como el color va a ser igual lo ponemos ``uniform``:

.. code-block:: c

	precision mediump float;
	uniform vec4 color;
	void main(){
		gl_FragColor=color;
	}	

Compilación de shaders
------------------------

Para todo shader hay que hacer lo siguiente:

1. Crearle un ``id`` con ``glCreateShader(tipoShader)``. El ``tipoShader`` puede ser ``GL_FRAGMENT_SHADER`` O ``GL_VERTEX_SHADER``
2. Pasar el código al interior de OpenGL con ``glShaderSource(id_shader, codigoShader)``. El ``id_shader`` es el ``id`` obtenido antes y ``codigoShader`` es un String con el código del shader.
3. Compilar cada shader con ``glCompileShader(id_shader)``.
4. Crear un programa dentro de OpenGL con ``glCreateProgram()``. Nos devolver un ``idPrograma``,
5. Meter dentro del programa ambos shader con ``glAttachShader(idPrograma, id_vertex_shader)`` y ``glAttachShader(idPrograma, id_fragment_shader)``.
6. Enlazar ambos programa con ``glLinkProgram(idPrograma)``
7. Comprobar que el programa está en un estado válido con ``glGetProgramiv(idPrograma, GL_VALIDATE_STATUS, vector, 0)`` que escribirá un codigo de error (si lo hay) en la posición 0 del vector de enteros llamado ``error``.



Ejecución del programa OpenGL
------------------------------------------------------
En los shaders que hemos puesto tenemos varios posibles valores:

* Hay un atributo ``posicion_vertice`` de tipo ``vec4`` en el vertex shader.
* Hay un ``uniform`` en el fragment shader también de tipo ``vec4``.

Nuestro vertex shader se ejecutará una vez para cada punto que queramos dibujar. El fragment shader se irá ejecutando automáticamente rellenando las superficies o líneas que unen los puntos.

Para poder ejecutar el programa, necesitamos pasar una lista de puntos a OpenGL. Lo que pasa es que no podemos pasar directamente un array de ``float`` ya que Java y OpenGL no usan el mismo sistema para almacenar números. Afortunadamente, Java ofrece una clase ``FloatBuffer`` que puede adaptar nuestros ``float`` al formato de OpenGL. El siguiente método muestra como se "traduce":

.. code-block:: java

	public FloatBuffer crearListaPuntos(float[] listaPuntos){
		final int BYTES_POR_FLOAT=4;
		int totalBytes=listaPuntos.length*BYTES_POR_FLOAT;
		ByteBuffer temp=ByteBuffer.allocateDirect(totalBytes);
		ByteOrder orden=ByteOrder.nativeOrder();
		temp.order(orden);
		return temp.asFloatBuffer();
	}
	
Si ahora queremos que OpenGL ejecute el vertex shader para cada punto de un ``FloatBuffer`` entonces necesitaremos saber la dirección de memoria donde está el atributo ``posicion_vertice``. Podemos averiguar dicha dirección con ``glGetAttribLocation(idPrograma, nombreAtributo)``, donde el ``nombreAtributo`` es un ``String`` con el contenido "posicion_vertice".

Una vez localizado el atributo "posicion_vertice", podemos enviar a esa dirección la lista de puntos para los cuales se debe ejecutar el vertex shader. Para ello se usa ``glVertexAttribPointer(direccion, longitud_vector, GL_FLOAT, false, 0, vector)`` y luego habilitar el vector de puntos con ``glEnableVertexAttribArray(direccion)``

Una vez pasado el vector con los datos ya se pueden ejecutar llamadas como ``glDrawArrays(GL_TRIANGLE_FAN, primera_pos, ultima_pos)`` para dibujar elementos en pantalla. No solamente podemos usar un vector de posiciones, sino también un vector de colores. Los puntos y los colores pueden almacenarse en el mismo vector o separados. Separar los datos es un poquito más fácil de programar y depurar, pero poner todo junto obtiene el máximo rendimiento.


