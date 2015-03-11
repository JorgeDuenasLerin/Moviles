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

Ejemplo completo
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: java

	
	public class MyGdxGame extends ApplicationAdapter {
		SpriteBatch batch;
		Rectangle rCubo;
		Rectangle rGota;
		Texture imagenCubo, imagenGota;
		OrthographicCamera camara;
		Array<Rectangle> gotas; 
		Vector3 posTocada;
		int ANCHURA=800;
		int ALTURA=480;
		Sound sonidoGota;
		long instanteUltimaGota=0;
		Music ruidoLluvia;
		Random generadorNumeros=new Random();
		
		public void generarGota(){
			int x=generadorNumeros.nextInt(800);
			int y=480;
			Rectangle rGota=new Rectangle();
			rGota.x=x;
			rGota.y=y;
			rGota.width=32;
			rGota.height=32;
			gotas.add(rGota);
		}
		public void create () {
			gotas=new Array<Rectangle>();
			sonidoGota=Gdx.audio.newSound(
					Gdx.files.internal("sonidogota.wav")
					);
			ruidoLluvia=Gdx.audio.newMusic(
					Gdx.files.internal("sonidolluvia.mp3")
					);
			rCubo=new Rectangle();
			rCubo.x=200;
			rCubo.y=20;
			rCubo.width=32;
			rCubo.height=32;
			rGota=new Rectangle();
			rGota.x=300;
			rGota.y=440;
			rGota.width=32;
			rGota.height=32;
			ruidoLluvia.play();
			ruidoLluvia.setLooping(true);
			batch = new SpriteBatch();
			imagenCubo=new Texture(
					Gdx.files.internal("imagencubo.png")
					);
			imagenGota=new Texture(
					Gdx.files.internal("imagengota.png")
					);
			camara=new OrthographicCamera();		
			camara.setToOrtho(false,ANCHURA,ALTURA);
			batch.setProjectionMatrix(camara.combined);
		}

		public void actualizarPosicionGotas(){
			Iterator<Rectangle> puntero=gotas.iterator();
			while (puntero.hasNext()){
				Rectangle elemento=puntero.next();
				elemento.y=elemento.y-
						(200*Gdx.graphics.getDeltaTime());
				batch.draw(imagenGota, 
						elemento.x, elemento.y);
				if (elemento.y<0){
					puntero.remove();
				}
				if (elemento.overlaps(rCubo)){
					sonidoGota.play();
					puntero.remove();
				}
			}
		}
		@Override
		public void render () {
			Gdx.gl.glClearColor(0.5f, 0.5f, 0.8f, 1);
			Gdx.gl.glClear(GL20.GL_COLOR_BUFFER_BIT);
			batch.begin();
			batch.draw(imagenCubo, rCubo.x, rCubo.y);
			actualizarPosicionGotas();
			batch.end();
			
			/* Si han pasado más de 500ms
			 * desde la generación de la última
			 * gota, generamos una nueva
			 * y nos apuntamos el instante actual
			 */
			long tiempoActual=TimeUtils.millis();
			long diferencia=tiempoActual-instanteUltimaGota;
			if ( diferencia>500 ){
				this.generarGota();
				this.instanteUltimaGota=TimeUtils.millis();
				
			}
			
			if (Gdx.input.isTouched()){
				posTocada=new Vector3();
				posTocada.x=Gdx.input.getX();
				posTocada.y=Gdx.input.getY();
				Vector3 posCorregida=
						camara.unproject(posTocada);
				rCubo.x=posCorregida.x;
				if (rCubo.x>(ANCHURA-32)){
					rCubo.x=(ANCHURA-32);
				}			
			}
		}
	}
	
Hojas de sprites
------------------------------------------------------

La carga de ficheros individuales es un proceso muy lento para el móvil/tablet, por lo que suele ser mucho más efectivo cargar un solo fichero con todos los gráficos y luego "trocearlo" en memoria. Este proceso implica usar objetos ``TextureRegion`` mas o menos de esta forma:

.. code-block:: java

	public class Laberinto extends ApplicationAdapter {
		SpriteBatch batch;
		Texture	ficheroHoja;
		TextureRegion hojaSprites;
		TextureRegion trozos[][];
		OrthographicCamera camara;
		@Override
		public void create () {
			camara=new OrthographicCamera();
			camara.setToOrtho(false, 800, 480);
			batch = new SpriteBatch();
			batch.setProjectionMatrix(camara.combined);
			ficheroHoja=new Texture(
					Gdx.files.internal("hojasprites.png")
			);
			hojaSprites=new TextureRegion();
			hojaSprites.setRegion(ficheroHoja);
			trozos=hojaSprites.split(32, 32);
		}
		public void dibujarFondoCesped(){
			for (int x=32; x<800-32; x=x+32){
				for (int y=32; y<480-32; y=y+32){
					batch.draw(trozos[2][6], x, y);
				}
			}
		}
		public void dibujarBordePiedra(){
			for (int x=0; x<800; x=x+32){
				batch.draw(trozos[3][6], x, 0);
			}
		}
		@Override
		public void render () {
			Gdx.gl.glClearColor(1, 0, 0, 1);
			Gdx.gl.glClear(GL20.GL_COLOR_BUFFER_BIT);
			batch.begin();
			dibujarBordePiedra();
			dibujarFondoCesped();
			batch.end();
		}
	}
	
Animaciones
------------

Usando la clase ``Animation`` podemos crear animaciones. La clase ``Animation`` necesita dos cosas para ser construida:

* El tiempo que pasará entre frame y frame.
* Un vector de elementos ``TextureRegion`` que la animación irá reproduciendo.

A continuación se muestra como crear las animaciones de un juego sencillo:

.. code-block:: java

	public void crearAnimaciones(){
		TextureRegion[] cuadrosImagenAba=
				new TextureRegion[3];
		cuadrosImagenAba[0]=trozos[0][0];
		cuadrosImagenAba[1]=trozos[0][1];
		cuadrosImagenAba[2]=trozos[0][2];
		animAba=new Animation(
			intervaloAnimaciones,cuadrosImagenAba);
		TextureRegion[] cuadrosImagenIzq=
				new TextureRegion[3];
		cuadrosImagenIzq[0]=trozos[1][0];
		cuadrosImagenIzq[1]=trozos[1][1];
		cuadrosImagenIzq[2]=trozos[1][2];
		animIzq=new Animation(	
			intervaloAnimaciones,cuadrosImagenIzq);
		TextureRegion[] cuadrosImagenDer=
				new TextureRegion[3];
		cuadrosImagenDer[0]=trozos[2][0];
		cuadrosImagenDer[1]=trozos[2][1];
		cuadrosImagenDer[2]=trozos[2][2];
		animDer=new Animation(
			intervaloAnimaciones,cuadrosImagenDer);
		TextureRegion[] cuadrosImagenArr=
				new TextureRegion[3];
		cuadrosImagenArr[0]=trozos[3][0];
		cuadrosImagenArr[1]=trozos[3][1];
		cuadrosImagenArr[2]=trozos[3][2];
		animArr=new Animation(
			intervaloAnimaciones,cuadrosImagenArr);
	}
	
	
Enemigos
------------------------------------------------------
Todos los enemigos de un juego suelen actuar de acuerdo a tres posibles comportamientos

* Movimientos cíclicos. Pueden ser más o menos complejos y/o fáciles de adivinar por el jugador.
* Enemigos perfectos, que persiguen al jugador a la perfección complicando muchísimo la dificultad del juego.
* Enemigos "lo bastante inteligentes", como para hacer el juego atractivo sin caer en la dificultad excesiva.

En este curso manejaremos enemigos cíclicos. Supongamos un enemigo que traza un rectángulo siguiendo un trazado derecha-arriba-izquierda-abajo.

El código siguiente ilustra como crear este movimiento:

.. code-block:: java

	public class Enemigo {
		public int direccion;
		public final int ARRIBA=0;
		public final int ABAJO=1;
		public final int IZQUIERDA=2;
		public final int DERECHA=3;
		public int x_actual, y_actual;
		private int x0, y0, x1, y1;
		public Enemigo(int x0, int y0, int x1, int y1){
			this.x0=x0; this.y0=y0;
			this.x1=x1; this.y1=y1;
			this.x_actual=x0;
			this.y_actual=y0;
			this.direccion=DERECHA;
		}
		public void avanzar(){
			if (this.direccion==DERECHA){
				this.x_actual+=1;
				if (this.x_actual==this.x1){
					this.direccion=ARRIBA;
				}
			}
			if (this.direccion==ARRIBA){
				this.y_actual+=1;
				if (this.y_actual==this.y1){
					this.direccion=IZQUIERDA;
				}
			}
			if (this.direccion==IZQUIERDA){
				this.x_actual-=1;
				if (this.x_actual==this.x0){
					this.direccion=ABAJO;
				}
			}
			if (this.direccion==ABAJO){
				this.y_actual-=1;
				if (this.y_actual==this.y0){
					this.direccion=DERECHA;
				}
			}
		}	
	}
	
Ejercicio: movimiento de enemigos
------------------------------------------------------

Crear enemigos que puedan moverse en L y en triángulo. Crear un juego en el que haya varios enemigos donde cada uno se mueva realizando distintos recorridos.

	
	
	
Movimiento del protagonista
------------------------------------------------------


La siguiente clase ilustra como puede moverse el protagonista de nuestro juego:

.. code-block:: java

	public class Protagonista {
		private float x_actual=0;
		private float y_actual=0;
		private float x_objetivo=0;
		private float y_objetivo=0;
		private float incr_x=0;
		private float incr_y=0;
		boolean enMovimiento=false;
		private int X_ES_EJE_MAYOR=0;
		private int Y_ES_EJE_MAYOR=1;
		public void moverseHacia(Vector3 pos){
			x_objetivo= pos.x;
			y_objetivo= pos.y;
			enMovimiento=true;
			calcularIncrementos();
		}
		public void calcularIncrementos(){
			float dif_x=(x_actual-x_objetivo);
			float dif_y=(y_actual-y_objetivo);
			float abs_dif_x=Math.abs(dif_x);
			float abs_dif_y=Math.abs(dif_y);
			if (abs_dif_x>abs_dif_y){
				if (dif_x>0){
					incr_x=1;
				} else {
					incr_x=-1;
				}
				incr_y=(y_actual-y_objetivo) 
						/ (x_actual-x_objetivo);
			} else {
				if (dif_y>0){
					incr_y=1;
				} else {
					incr_y=-1;
				}
				incr_x=(x_actual-x_objetivo) 
						/ (y_actual-y_objetivo);
			}
		}
		public int getX(){
			return (int) this.x_actual;
		}
		public int getY(){
			return (int) this.y_actual;
		}
		public void avanzar(){
			if (enMovimiento==false) return ;
			if ( (x_actual==x_objetivo) && 
					y_actual==y_objetivo) {
				enMovimiento=false;
				return ;
			}
			x_actual=x_actual+incr_x;
			if (x_actual>=x_objetivo){
				incr_x=0;
			}
			y_actual=y_actual+incr_y;
			if (y_actual>=y_objetivo){
				incr_y=0;
			}
		}	
	}

En realidad este protagonista "traza una curva", lo cual no es un movimiento muy correcto. Para poder mover correctamente el protagonista se necesitan varias cosas:

* Se necesita saber si el incr_x o el incr_y serán positivos o negativos.
* Se necesita calcular cuanto vale cada incremento **POR SEPARADO** si uno de ellos vale 1, el otro valdrá una fracción (como 0.33). Esto implica que si el muñeco avanza un paso en las x da solo un tercio de paso en las y.

Esta clase ilustra un movimiento mucho mejor.

.. code-block:: java

	public class Protagonista {
		private float x_actual=0;
		private float y_actual=0;
		private float x_objetivo=0;
		private float y_objetivo=0;
		private float incr_x=0;
		private float incr_y=0;
		boolean enMovimiento=false;
		private int X_ES_EJE_MAYOR=0;
		private int Y_ES_EJE_MAYOR=1;
		public void moverseHacia(Vector3 pos){
			x_objetivo= pos.x;
			y_objetivo= pos.y;
			enMovimiento=true;
			calcularIncrementos();
		}
		public void calcularIncrementos(){
			int signo_x=0,signo_y=0;
			if (x_objetivo<x_actual){
				signo_x=-1;
			} else {
				signo_x=1;
			}
			if (y_objetivo<y_actual){
				signo_y=-1;
			} else {
				signo_y=1;
			}
			double dif_x=Math.abs(x_actual-x_objetivo);
			double dif_y=Math.abs(y_actual-y_objetivo);
			if (dif_x>dif_y){
				incr_x=1*signo_x;
				incr_y=(float) ((dif_y/dif_x)*signo_y);
			} else {
				incr_y=1*signo_y;
				incr_x=(float) ((dif_x/dif_y)*signo_x);
			}
		}
		public int getX(){
			return (int) this.x_actual;
		}
		public int getY(){
			return (int) this.y_actual;
		}
		public void avanzar(){
			if (enMovimiento==false) return ;
			float dif_x=Math.abs(x_actual-x_objetivo);
			float dif_y=Math.abs(y_actual-y_objetivo);
			if ( (dif_x<=0.5) && (dif_y<=0.5) ){
				enMovimiento=false;
				return ;
			}
			x_actual=x_actual+incr_x;
			y_actual=y_actual+incr_y;
		}	
	}
	
Esta clase Laberinto.java contiene el código principal del juego tal y como lo tenemos:

.. code-block:: java

	public class Laberinto extends ApplicationAdapter {
		SpriteBatch batch;
		Texture	ficheroHoja;
		TextureRegion hojaSprites;
		TextureRegion trozos[][];
		OrthographicCamera camara;
		Animation animAba, animArr, animIzq,animDer;
		float intervaloAnimaciones=0.1f;
		float tiempoTranscurrido;
		Enemigo enemigo;
		Protagonista protagonista;
		Vector3 posTocada, posFinal;
		@Override
		public void create () {
			protagonista=new Protagonista();
			posTocada=new Vector3();
			posFinal=new Vector3();
			camara=new OrthographicCamera();
			camara.setToOrtho(false, 800, 480);
			batch = new SpriteBatch();
			batch.setProjectionMatrix(camara.combined);
			ficheroHoja=new Texture(
					Gdx.files.internal("hojasprites.png")
			);
			hojaSprites=new TextureRegion();
			hojaSprites.setRegion(ficheroHoja);
			trozos=hojaSprites.split(32, 32);
			crearAnimaciones();
			crearEnemigos();
		}
		public void crearEnemigos(){
			enemigo=new Enemigo(100,100, 350,350);
		}
		public void crearAnimaciones(){
			TextureRegion[] cuadrosImagenAba=
					new TextureRegion[3];
			cuadrosImagenAba[0]=trozos[0][0];
			cuadrosImagenAba[1]=trozos[0][1];
			cuadrosImagenAba[2]=trozos[0][2];
			animAba=new Animation(
					intervaloAnimaciones,cuadrosImagenAba);
			TextureRegion[] cuadrosImagenIzq=
					new TextureRegion[3];
			cuadrosImagenIzq[0]=trozos[1][0];
			cuadrosImagenIzq[1]=trozos[1][1];
			cuadrosImagenIzq[2]=trozos[1][2];
			animIzq=new Animation(
					intervaloAnimaciones,cuadrosImagenIzq);
			TextureRegion[] cuadrosImagenDer=
					new TextureRegion[3];
			cuadrosImagenDer[0]=trozos[2][0];
			cuadrosImagenDer[1]=trozos[2][1];
			cuadrosImagenDer[2]=trozos[2][2];
			animDer=new Animation(
					intervaloAnimaciones,cuadrosImagenDer);
			TextureRegion[] cuadrosImagenArr=
					new TextureRegion[3];
			cuadrosImagenArr[0]=trozos[3][0];
			cuadrosImagenArr[1]=trozos[3][1];
			cuadrosImagenArr[2]=trozos[3][2];
			animArr=new Animation(
					intervaloAnimaciones,cuadrosImagenArr);
			
			
		}
		public void dibujarFondoCesped(){
			for (int x=32; x<800-32; x=x+32){
				for (int y=32; y<480-32; y=y+32){
					batch.draw(trozos[2][6], x, y);
				}
			}
		}
		public void dibujarBordePiedra(){
			for (int x=0; x<800; x=x+32){
				batch.draw(trozos[3][6], x, 0);
				batch.draw(trozos[3][6], x, 480-32);
			}
			for (int y=0; y<480;y=y+32){
				batch.draw(trozos[3][6],0,y); 
				batch.draw(trozos[3][6],800-32,y);
			}
		}
		
		public void dibujarHorizontal(TextureRegion img, 
				int x0, int y0, int xfinal, int yfinal, int incr_x){
			for (int x=x0; x<xfinal; x=x+incr_x){
				batch.draw(img, x, y0);
			}
		}
		public void dibujarVertical(TextureRegion img, 
				int x0, int y0, int xfinal, int yfinal, int incr_y){
			for (int y=y0; y<yfinal; y=y+incr_y){
				batch.draw(img, x0, y);
			}
		}
		public void dibujarEnemigos(float tiempo){
			enemigo.avanzar();
			TextureRegion cuadro=null;
			if (enemigo.direccion==enemigo.DERECHA){
				cuadro=animDer.getKeyFrame(tiempo, true);
			}
			if (enemigo.direccion==enemigo.IZQUIERDA){
				cuadro=animIzq.getKeyFrame(tiempo, true);
			}
			if (enemigo.direccion==enemigo.ABAJO){
				cuadro=animAba.getKeyFrame(tiempo, true);
			}
			if (enemigo.direccion==enemigo.ARRIBA){
				cuadro=animArr.getKeyFrame(tiempo, true);
			}
			batch.draw(cuadro, 
					enemigo.x_actual,enemigo.y_actual);
		}
		public void dibujarProtagonista(){
			protagonista.avanzar();
			batch.draw(
					trozos[0][4],
					protagonista.getX(),
					protagonista.getY() );
		}
		@Override
		public void render () {
			tiempoTranscurrido+=
					Gdx.graphics.getDeltaTime();
			Gdx.gl.glClearColor(1, 0, 0, 1);
			Gdx.gl.glClear(GL20.GL_COLOR_BUFFER_BIT);
			batch.begin();
			dibujarBordePiedra();
			dibujarFondoCesped();
			dibujarHorizontal(trozos[1][6], 64,64, 640,64, 32);
			dibujarVertical(trozos[1][6], 64,64, 64,320, 32);
			dibujarEnemigos(tiempoTranscurrido);
			dibujarProtagonista();
			batch.end();
			
			if (Gdx.input.isTouched()){
				posTocada=new Vector3();
				posTocada.x=Gdx.input.getX();
				posTocada.y=Gdx.input.getY();
				posFinal=camara.unproject(posTocada);
				protagonista.moverseHacia(posFinal);
			}		
		}
	}
		
Ampliación: baldosas y obstáculos
---------------------------------

En nuestro juego necesitamos tener obstáculos que impidan al jugador moverse y baldosas, las cuales al ser pisadas desaparecen. El objetivo del protagonista es pisar todas las baldosas para avanzar en el juego.

Clase Laberinto
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: java

	public class Laberinto extends ApplicationAdapter {
		private final int ANCHO=800;
		private final int ALTO=480;
		SpriteBatch batch;
		Texture	ficheroHoja;
		TextureRegion hojaSprites;
		TextureRegion trozos[][];
		Escenario escenario;
		OrthographicCamera camara;
		Animation animAba, animArr, animIzq,animDer;
		float intervaloAnimaciones=0.1f;
		float tiempoTranscurrido;
		Enemigo enemigo;
		Protagonista protagonista;
		Vector3 posTocada, posFinal;
		@Override
		public void create () {
			protagonista=new Protagonista(32,32);
			posTocada=new Vector3();
			posFinal=new Vector3();
			camara=new OrthographicCamera();
			camara.setToOrtho(false, 800, 480);
			batch = new SpriteBatch();
			batch.setProjectionMatrix(camara.combined);
			ficheroHoja=new Texture(
					Gdx.files.internal("hojasprites.png")
			);
			hojaSprites=new TextureRegion();
			hojaSprites.setRegion(ficheroHoja);
			trozos=hojaSprites.split(32, 32);
			crearAnimaciones();
			crearEnemigos();
			crearEscenario();
		}
		public void crearEscenario(){
			escenario=new Escenario();
			/* Se añade el cesped*/
			for (int x=0; x<ANCHO; x=x+32){
				for (int y=0;y<ALTO; y=y+32){
					Rectangle r=new Rectangle();
					r.x=x;
					r.y=y;
					r.height=32;
					r.width=32;
					TextureRegion cesped=trozos[2][6];
					escenario.addDecorado(r, cesped);
				}
			}
			for (int x=100;x<300; x=x+32){
				Rectangle r=new Rectangle();
				r.x=x;
				r.y=150;
				r.height=32;
				r.width=32;
				TextureRegion seto=trozos[1][6];
				escenario.addObstaculo(r, seto);
			}
			Rectangle rBaldosa1=new Rectangle();
			rBaldosa1.x=150;
			rBaldosa1.y=220;
			rBaldosa1.width=32;
			rBaldosa1.height=32;
			escenario.addBaldosa(rBaldosa1, trozos[3][6]);
			Rectangle rBaldosa2=new Rectangle();
			rBaldosa2.x=450;
			rBaldosa2.y=220;
			rBaldosa2.width=32;
			rBaldosa2.height=32;
			escenario.addBaldosa(rBaldosa2, trozos[3][6]);
			
			
		}
		public void crearEnemigos(){
			enemigo=new Enemigo(100,100, 350,350, 32,32);
		}
		public void crearAnimaciones(){
			TextureRegion[] cuadrosImagenAba=
					new TextureRegion[3];
			cuadrosImagenAba[0]=trozos[0][0];
			cuadrosImagenAba[1]=trozos[0][1];
			cuadrosImagenAba[2]=trozos[0][2];
			animAba=new Animation(
					intervaloAnimaciones,cuadrosImagenAba);
			TextureRegion[] cuadrosImagenIzq=
					new TextureRegion[3];
			cuadrosImagenIzq[0]=trozos[1][0];
			cuadrosImagenIzq[1]=trozos[1][1];
			cuadrosImagenIzq[2]=trozos[1][2];
			animIzq=new Animation(
					intervaloAnimaciones,cuadrosImagenIzq);
			TextureRegion[] cuadrosImagenDer=
					new TextureRegion[3];
			cuadrosImagenDer[0]=trozos[2][0];
			cuadrosImagenDer[1]=trozos[2][1];
			cuadrosImagenDer[2]=trozos[2][2];
			animDer=new Animation(
					intervaloAnimaciones,cuadrosImagenDer);
			TextureRegion[] cuadrosImagenArr=
					new TextureRegion[3];
			cuadrosImagenArr[0]=trozos[3][0];
			cuadrosImagenArr[1]=trozos[3][1];
			cuadrosImagenArr[2]=trozos[3][2];
			animArr=new Animation(
					intervaloAnimaciones,cuadrosImagenArr);
			
			
		}
		public void dibujarEnemigos(float tiempo){
			enemigo.avanzar();
			TextureRegion cuadro=null;
			if (enemigo.direccion==enemigo.DERECHA){
				cuadro=animDer.getKeyFrame(tiempo, true);
			}
			if (enemigo.direccion==enemigo.IZQUIERDA){
				cuadro=animIzq.getKeyFrame(tiempo, true);
			}
			if (enemigo.direccion==enemigo.ABAJO){
				cuadro=animAba.getKeyFrame(tiempo, true);
			}
			if (enemigo.direccion==enemigo.ARRIBA){
				cuadro=animArr.getKeyFrame(tiempo, true);
			}
			batch.draw(cuadro, 
					enemigo.x_actual,enemigo.y_actual);
		}
		public void dibujarProtagonista(){
			protagonista.avanzar(escenario);
			batch.draw(
					trozos[0][4],
					protagonista.getX(),
					protagonista.getY() );
			Rectangle rEnemigo=enemigo.getRectangulo();
			if (protagonista.colisionaConEnemigo(rEnemigo)){
				protagonista.moverInicio();
			}
			Rectangle rProtagonista;
			rProtagonista=protagonista.getRectangulo();
			int restantes=escenario.pisaBaldosa(rProtagonista);
			if (restantes==0){
				BitmapFont fuentePorDefecto;
				fuentePorDefecto=new BitmapFont();
				fuentePorDefecto.scale(3);
				String mensaje="Enhorabuena!";
				fuentePorDefecto.draw(
						batch, mensaje, ANCHO/2-100, ALTO/2);
				protagonista.parar();
			}
		}
		@Override
		public void render () {
			tiempoTranscurrido+=
					Gdx.graphics.getDeltaTime();
			Gdx.gl.glClearColor(1, 0, 0, 1);
			Gdx.gl.glClear(GL20.GL_COLOR_BUFFER_BIT);
			batch.begin();
			escenario.dibujar(batch);
			dibujarEnemigos(tiempoTranscurrido);
			dibujarProtagonista();
			batch.end();
			
			if (Gdx.input.isTouched()){
				posTocada=new Vector3();
				posTocada.x=Gdx.input.getX();
				posTocada.y=Gdx.input.getY();
				posFinal=camara.unproject(posTocada);
				protagonista.moverseHacia(posFinal);
			}	
		}
	}
	
Clase Protagonista
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: java

	public class Protagonista {
		private float x_actual=0;
		private float y_actual=0;
		private float x_objetivo=0;
		private float y_objetivo=0;
		private float incr_x=0;
		private float incr_y=0;
		boolean enMovimiento=false;
		private Rectangle rProtagonista=new Rectangle();
		public Protagonista(int ancho, int alto){
			rProtagonista.height=alto;
			rProtagonista.width=ancho;
			rProtagonista.x=x_actual;
			rProtagonista.y=y_actual;
		}
		
		public Rectangle getRectangulo(){
			return rProtagonista;
		}
		public void parar(){
			this.enMovimiento=false;
		}
		public void moverInicio(){
			this.enMovimiento=false;
			this.x_actual=0;
			this.y_actual=0;
		}
		public void moverseHacia(Vector3 pos){
			x_objetivo= pos.x;
			y_objetivo= pos.y;
			enMovimiento=true;
			calcularIncrementos();
			
		}
		public boolean colisionaConEnemigo(Rectangle r){
			if (rProtagonista.overlaps(r)){
				return true;
			}
			return false;
		}
		public void calcularIncrementos(){
			int signo_x=0,signo_y=0;
			if (x_objetivo<x_actual){
				signo_x=-1;
			} else {
				signo_x=1;
			}
			if (y_objetivo<y_actual){
				signo_y=-1;
			} else {
				signo_y=1;
			}
			double dif_x=Math.abs(x_actual-x_objetivo);
			double dif_y=Math.abs(y_actual-y_objetivo);
			if (dif_x>dif_y){
				incr_x=1*signo_x;
				incr_y=(float) ((dif_y/dif_x)*signo_y);
			} else {
				incr_y=1*signo_y;
				incr_x=(float) ((dif_x/dif_y)*signo_x);
			}
		}
		public int getX(){
			return (int) this.x_actual;
		}
		public int getY(){
			return (int) this.y_actual;
		}
		public void avanzar(Escenario escenario){
			if (enMovimiento==false) return ;
			float dif_x=Math.abs(x_actual-x_objetivo);
			float dif_y=Math.abs(y_actual-y_objetivo);
			if ( (dif_x<=0.5) && (dif_y<=0.5) ){
				enMovimiento=false;
				return ;
			}
			x_actual=x_actual+incr_x;
			y_actual=y_actual+incr_y;
			rProtagonista.x=x_actual;
			rProtagonista.y=y_actual;
			if (escenario.colisionaConObstaculo(rProtagonista)){
				x_actual=x_actual-incr_x;
				y_actual=y_actual-incr_y;
				rProtagonista.x=x_actual;
				rProtagonista.y=y_actual;
				this.enMovimiento=false;
			}		
		}	
	}
	
Clase Enemigo
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: java

	public class Enemigo {
		public int direccion;
		public final int ARRIBA=0;
		public final int ABAJO=1;
		public final int IZQUIERDA=2;
		public final int DERECHA=3;
		public int x_actual, y_actual;
		private int x0, y0, x1, y1;
		private Rectangle rEnemigo;
		public Enemigo(int x0, int y0, int x1, int y1, 
				int ancho, int alto){
			rEnemigo=new Rectangle();
			rEnemigo.height=alto;
			rEnemigo.width=ancho;
			rEnemigo.x=x_actual;
			rEnemigo.y=y_actual;
			this.x0=x0; this.y0=y0;
			this.x1=x1; this.y1=y1;
			this.x_actual=x0;
			this.y_actual=y0;
			this.direccion=DERECHA;
		}
		public void avanzar(){
			if (this.direccion==DERECHA){
				this.x_actual+=4;
				if (this.x_actual>=this.x1){
					this.direccion=ARRIBA;
				}
			}
			if (this.direccion==ARRIBA){
				this.y_actual+=1;
				if (this.y_actual>=this.y1){
					this.direccion=IZQUIERDA;
				}
			}
			if (this.direccion==IZQUIERDA){
				this.x_actual-=4;
				if (this.x_actual<=this.x0){
					this.direccion=ABAJO;
				}
			}
			if (this.direccion==ABAJO){
				this.y_actual-=1;
				if (this.y_actual<=this.y0){
					this.direccion=DERECHA;
				}
			}	
			rEnemigo.x=x_actual;
			rEnemigo.y=y_actual;
		}
		public Rectangle getRectangulo(){
			return rEnemigo;
		}
	}
	
	
Clase Escenario
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: java

	public class Escenario {
		Array<ElementoDecorado> obstaculos;
		Array<ElementoDecorado> decorado;
		Array<ElementoDecorado> baldosas;
		public Escenario(){
			obstaculos=new Array<ElementoDecorado>();
			decorado=new Array<ElementoDecorado>();
			baldosas=new Array<ElementoDecorado>();
		}
		public void addBaldosa(Rectangle r, 
				TextureRegion dibujo){
			ElementoDecorado ed=
					new ElementoDecorado(r,dibujo, false);
			decorado.add(ed);
			baldosas.add(ed);
		}
		public void addObstaculo
			(Rectangle r, TextureRegion dibujo){
			ElementoDecorado ed=
					new ElementoDecorado(r, dibujo, true);
			obstaculos.add(ed);
			decorado.add(ed);
		}
		public void addDecorado(Rectangle r,
				TextureRegion dibujo){
			ElementoDecorado ed=
					new ElementoDecorado(r, dibujo, false);
			decorado.add(ed);
		}
		public void dibujar(Batch batch){
			TextureRegion dibujo;
			Rectangle rectangulo;
			for (int i=0; i<decorado.size; i++){
				rectangulo=decorado.get(i).getRectangulo();
				dibujo=decorado.get(i).getDibujo();
				batch.draw(
						dibujo, rectangulo.x, rectangulo.y);
			}
		}
		public boolean colisionaConObstaculo
					(Rectangle prota)
		{
			Rectangle rectangulo;
			for (int i=0; i<obstaculos.size; i++){
				rectangulo=obstaculos.get(i).getRectangulo();
				if (prota.overlaps(rectangulo)) return true;
			}
			return false;
		}
		/* Destruye las baldosas pisadas y nos dice
		 * cuantas quedan */
		public int pisaBaldosa(Rectangle prota){
			Rectangle rectangulo;
			for (int i=0;i<baldosas.size;i++){
				ElementoDecorado baldosa=baldosas.get(i);
				rectangulo=baldosas.get(i).getRectangulo();
				if (prota.overlaps(rectangulo)){
					decorado.removeValue(baldosa, false);
					baldosas.removeIndex(i);
				}
			}
			return baldosas.size;
		}
	}	

Clase ElementoDecorado
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: java

	public class Escenario {
		Array<ElementoDecorado> obstaculos;
		Array<ElementoDecorado> decorado;
		Array<ElementoDecorado> baldosas;
		public Escenario(){
			obstaculos=new Array<ElementoDecorado>();
			decorado=new Array<ElementoDecorado>();
			baldosas=new Array<ElementoDecorado>();
		}
		public void addBaldosa(Rectangle r, 
				TextureRegion dibujo){
			ElementoDecorado ed=
					new ElementoDecorado(r,dibujo, false);
			decorado.add(ed);
			baldosas.add(ed);
		}
		public void addObstaculo
			(Rectangle r, TextureRegion dibujo){
			ElementoDecorado ed=
					new ElementoDecorado(r, dibujo, true);
			obstaculos.add(ed);
			decorado.add(ed);
		}
		public void addDecorado(Rectangle r,
				TextureRegion dibujo){
			ElementoDecorado ed=
					new ElementoDecorado(r, dibujo, false);
			decorado.add(ed);
		}
		public void dibujar(Batch batch){
			TextureRegion dibujo;
			Rectangle rectangulo;
			for (int i=0; i<decorado.size; i++){
				rectangulo=decorado.get(i).getRectangulo();
				dibujo=decorado.get(i).getDibujo();
				batch.draw(
						dibujo, rectangulo.x, rectangulo.y);
			}
		}
		public boolean colisionaConObstaculo
					(Rectangle prota)
		{
			Rectangle rectangulo;
			for (int i=0; i<obstaculos.size; i++){
				rectangulo=obstaculos.get(i).getRectangulo();
				if (prota.overlaps(rectangulo)) return true;
			}
			return false;
		}
		/* Destruye las baldosas pisadas y nos dice
		 * cuantas quedan */
		public int pisaBaldosa(Rectangle prota){
			Rectangle rectangulo;
			for (int i=0;i<baldosas.size;i++){
				ElementoDecorado baldosa=baldosas.get(i);
				rectangulo=baldosas.get(i).getRectangulo();
				if (prota.overlaps(rectangulo)){
					decorado.removeValue(baldosa, false);
					baldosas.removeIndex(i);
				}
			}
			return baldosas.size;
		}
	}	