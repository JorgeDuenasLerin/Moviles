Programación de aplicaciones para dispositivos móviles
========================================================

Herramientas y fases de construcción.
------------------------------------------------------
La principal herramienta para programar aplicaciones Android es Eclipse aunque hoy en día está empezando a ser reemplazada por Android Studio.

En primer lugar se debe hacer un análisis de los permisos que deberá necesitar nuestra aplicación.

Todos los permisos que requiera nuestra aplicación se indican en el principal fichero del proyecto: ``AndroidManifest.xml``


El archivo ``AndroidManifest.xml`` es un archivo imprescindible en cualquier aplicación Android, debe tener siempre ese nombre y debe estar en el directorio raíz del proyecto. Este fichero sirve para lo siguiente:

* Identifica el paquete Java de la aplicación, que se usará como identificador único de la misma.

* Describe los componentes de la aplicación: actividades, servicios, etc...

* Determina qué procesos alojarán componentes de la aplicación.

* Declara los permisos que debe tener la aplicación para acceder al hardware o al software del sistema.

* Declara los permisos que otros componentes deben tener para interactuar con los componentes de nuestra aplicación.

* Identifica las clases ``Instrumentation`` que se usarán para monitorizar el rendimiento. Normalmente esto solo se hace mientras estamos en pruebas, después se elimina.

* Indica la versión mínima de Android que se necesita para ejecutar nuestra app.

* Indica las bibliotecas con las que enlaza nuestro programa.

Un ``AndroidManifest.xml`` tiene esta estructura:

.. code-block:: xml

	<?xml version="1.0" encoding="utf-8"?>

	<manifest>

		<uses-permission />
		<permission />
		<permission-tree />
		<permission-group />
		<instrumentation />
		<uses-sdk />
		<uses-configuration />  
		<uses-feature />  
		<supports-screens />  
		<compatible-screens />  
		<supports-gl-texture />  

		<application>

			<activity>
				<intent-filter>
					<action />
					<category />
					<data />
				</intent-filter>
				<meta-data />
			</activity>

			<activity-alias>
				<intent-filter> . . . </intent-filter>
				<meta-data />
			</activity-alias>

			<service>
				<intent-filter> . . . </intent-filter>
				<meta-data/>
			</service>

			<receiver>
				<intent-filter> . . . </intent-filter>
				<meta-data />
			</receiver>

			<provider>
				<grant-uri-permission />
				<meta-data />
				<path-permission />
			</provider>

			<uses-library />

		</application>

	</manifest>   
	
``<uses-permission>``	
------------------------------------------------------
Indica que la app necesita que se le conceda un cierto permiso para poder ser instalada y ejecutada. El permiso se indica en el atributo ``android:name`` con un valor como ``android.permission.CAMERA``.

También puede llevar un atributo ``android:maxSdkVersion`` con el que se indica la versión máxima de Android donde es necesario pedir el permiso. Se usa en los casos en los que un permiso deja de existir. Eclipse suele rellenar este valor con la misma versión que usamos para el desarrollo.

Un posible valor:

.. code-block:: xml

   <uses-permission
     android:name="android.permission.WRITE_EXTERNAL_STORAGE"
     android:maxSdkVersion="18" />

	 
	
	
Después de construir el ``AndroidManifest.xml``	se debería realizar un boceto de como va a ser el interfaz. Aunque se indique de esta forma, el ``AndroidManifest.xml`` puede volver a modificarse en el futuro. Hay aplicaciones que permiten elaborar el "wireframe" de nuestro interfaz, pero Eclipse también puede ayudar mucho en esta tarea.	
	
	

Interfaces de usuario. Clases asociadas.
------------------------------------------------------



Ejercicio
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Usando el diseñador de Eclipse construye un interfaz como el siguiente. Recuerda editar los ID de los controles (para poder tener en el código nombres más fáciles de recordar) y pon nuevos textos a los controles (para que la aplicación sea fácil de traducir)

.. figure:: imagenes/appPension.png
   :figwidth: 50%
   :align: center
   
   Interfaz de la aplicación
   
La aplicación calcula una pensión de una forma muy sencilla: si se ha cotizado durante el mínimo de años exigidos por la ley, se tiene una pensión equivalente al 90% del sueldo actual. Si no ha sido así se tiene una pensión del 75% del sueldo actual.

Aunque es una funcionalidad que todavía no se va a implementar, la app podrá enviar un SMS con el resultado (y un anuncio de nuestra empresa) a otro número. Esto implica hacer que la aplicación exija pedir ese permiso en el ``AndroidManifest.xml``.

Código Java
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

El código Java sería algo así (faltan unas líneas)

.. code-block:: java

	public void calcularPension(View controlPulsado){
			EditText control;
			control=(EditText) 
					this.findViewById(R.id.txtSueldoActual);
			
			Editable cadPension=control.getEditableText();
			if (cadPension.toString().equals("")) return ;
			Double sueldoActual;
			sueldoActual=
					Double.parseDouble(cadPension.toString());
			
			ToggleButton togMinimo;
			togMinimo=(ToggleButton)
					this.findViewById(R.id.togMinimo);
			Double pensionResultado;
			if (togMinimo.isChecked()){
				pensionResultado=sueldoActual*0.9;
			}
			else {
				pensionResultado=sueldoActual*0.75;
			}
			
			EditText txtPensionResultado;
			txtPensionResultado=(EditText) 
					this.findViewById(R.id.txtPensionResultado);
			txtPensionResultado.setText(pensionResultado.toString());
		}
	
   

Sobre el diseño de interfaces
------------------------------------------------------

Cuando se diseña un interfaz lo normal es ir insertando los controles en "layouts" que a su vez van dentro de otros. El objetivo es poder modificar un bloque de controles sin afectar a los demás.


Todo control Android puede manipularse de dos formas:

* Indicando su tamaño en los parámetros ``width`` y ``height``. Se podría indicar el tamaño en puntos (mala idea porque el control no se redimensiona automáticamente) pero también se pueden indicar otras dos posibilidades:
	
	* ``wrap_content``: significa más o menos "adáptate al mínimo posible".
	* ``match_parent``: "agrándate y adáptate al tamaño de tu contenedor padre".

* Indicando qué proporción ocupa con respecto a sus controles del mismo contenedor. Esto se hace modificando el atributo ``weight`` y poniendo luego el ``width`` o el ``height`` a ``0dp``.


Ejercicio
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Crear una aplicación que permita al usuario practicar el cálculo mental con operaciones sencillas (sumas y restas) con números pequeños (de 1 a 99). Cuando el usuario introduce un resultado se le dice si acierta o no y se genera una nueva operación al azar.

.. code-block:: java

	package com.ies.calculus;

	import java.util.Random;

	import android.support.v7.app.ActionBarActivity;
	import android.os.Bundle;
	import android.util.Log;
	import android.view.Menu;
	import android.view.MenuItem;
	import android.view.View;
	import android.widget.EditText;
	import android.widget.TextView;


	public class ActividadPrincipal extends ActionBarActivity {
		int num1;
		int num2;
		String operacion;
		@Override
		protected void onCreate(Bundle savedInstanceState) {
			super.onCreate(savedInstanceState);
			setContentView(R.layout.activity_actividad_principal);
			generarOperacion();
		}
		

		@Override
		public boolean onCreateOptionsMenu(Menu menu) {
			// Inflate the menu; this adds items to the action bar if it is present.
			getMenuInflater().inflate(R.menu.actividad_principal, menu);
			return true;
		}

		@Override
		public boolean onOptionsItemSelected(MenuItem item) {
			// Handle action bar item clicks here. The action bar will
			// automatically handle clicks on the Home/Up button, so long
			// as you specify a parent activity in AndroidManifest.xml.
			int id = item.getItemId();
			if (id == R.id.action_settings) {
				return true;
			}
			return super.onOptionsItemSelected(item);
		}
		
		private void escribirNumeroEnTextView(
				int num, int id){
			TextView tv=(TextView)
					this.findViewById(id);
			tv.setText(""+num);
		}
		private void generarOperacion(){
			Random generador=new Random();
			num1=generador.nextInt(100);
			num2=generador.nextInt(100);
			escribirNumeroEnTextView(
					num1, R.id.tvOperando1);
			escribirNumeroEnTextView(
					num2, R.id.tvOperando2);
			//Para generar la op. matemática
			//escogeremos un valor al azar de un vector
			String[] ops={"+", "-"};
			
			int posAzar=generador.nextInt(ops.length);
			operacion=ops[posAzar];
			TextView tvOperando=
					(TextView)this.findViewById(R.id.tvOperador);
			tvOperando.setText(operacion);    	
		}
		public void comprobar(View control){
			EditText txtResultado=(EditText)
					this.findViewById(R.id.txtResultado);
			String resultado=txtResultado.getText().toString();
			TextView tvMensajes=(TextView)
					this.findViewById(R.id.tvMensajes);
			if (resultado.equals("")){
				tvMensajes.setText("Resultado incorrecto");
				generarOperacion();
				return ;
			}
			int resultCalculado=0;
			switch (operacion.charAt(0)){
				case '+':{
					resultCalculado=num1+num2;
					break;
				}
				case '-':{
					resultCalculado=num1-num2;
					break;
				}
			}
			int resultadoIntroducido=
					Integer.parseInt(resultado);
			if (resultadoIntroducido==resultCalculado){
				tvMensajes.setText("¡Correcto!");
			} else {
				tvMensajes.setText("¡MAL!");
			}
			txtResultado.setText("");
			generarOperacion();
		}
	}

Resumen de los contenedores Android
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

En la imagen siguiente puede apreciarse la variedad de contenedores que ofrece Android:

.. figure:: imagenes/contenedores.png
   :figwidth: 50%  
   :align: center
   :alt: Contenedores Android
   
   Contenedores Android
   
   
Actividades
------------------------------------------------------

Una actividad es un programa diseñado no solo para llamar a otros programas sino que también puede ofrecer sus servicios en Android para que otros programas les llamen a ellos.

El objetivo básico es comprender la forma de comunicar actividades en Android.

Actividad receptora de información
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Una actividad típica debería estar preparada para recibir parámetros de una forma similar a esta:

.. code-block:: java

	public static String parametroNombre=
			"com.ies.actividades1.nombrePersona";
	protected void onCreate(Bundle savedInstanceState) {
		super.onCreate(savedInstanceState);
		this.setContentView(R.layout.actividad_mostrar_nombres);
		
		Intent intentPasado=this.getIntent();
		String nombrePasado=
				intentPasado.getStringExtra(
			ActividadMostrarNombres.parametroNombre
		);
		
		TextView txtNombreAMostrar;
		txtNombreAMostrar=
				(TextView) this.findViewById(R.id.txtNombreMostrado);
		txtNombreAMostrar.setText(nombrePasado);
	}	

Actividad llamadora
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Una actividad que desee invocar a otro necesitará "pasar parámetros" de una forma similar a esta:

.. code-block:: java

	public void pasarNombre(View control){
    	EditText txtNombre;
    	txtNombre=(EditText) findViewById(R.id.txtNombre);
    	String nombre=txtNombre.getText().toString();
    	Intent iMostrarNombre;
    	//Indicamos quien es el llamador e
    	//indicamos
    	iMostrarNombre=new Intent(
    			this, ActividadMostrarNombres.class);
    	iMostrarNombre.putExtra
    		(ActividadMostrarNombres.parametroNombre
    				, nombre);
    	//Se lanza el intent
    	this.startActivity(iMostrarNombre);
    	
    }		

Construcción de actividades
------------------------------------------------------

Para crear una actividad desde cero necesitamos hacer dos cosas

1. Crear el interfaz XML (Eclipse puede que no añada un ``id`` a dicho interfaz, si no lo ha hecho añadirlo a mano)
2. Crear una clase Java que herede de ``Activity``. Dicha clase Java necesita que añadamos algo: el ``onCreate`` contendrá ahora el código que procesa el ``Intent`` que nos pasen y también  usaremos ``setContentView`` para cargar un fichero de interfaz o *layout*.
	
	
Dentro de la actividad suele ser buena política definir los nombres de los parámetros utilizando como prefijo el nombre del paquete:

.. code-block:: java

	public class 
		ActividadCalculadora extends Activity {

		public static String nombreNum1=
				"com.ies.actividades2.num1";
		public static String nombreNum2=
				"com.ies.actividades2.num2";
		public static String nombreOp=
				"com.ies.actividades2.op";
		@Override
		protected void onCreate(Bundle savedInstanceState) {
			// TODO Auto-generated method stub
			super.onCreate(savedInstanceState);
			
		}
	}


	
Ejemplo: llamadas entre actividades
------------------------------------------------------

Supongamos que deseamos tener una actividad que acepta recibir dos números y un operando. Tras la recepción se efectuará la operación matemática y se mostrará el resultado en un interfaz distinto de la actividad llamadora.

.. figure:: imagenes/actividadcalculadora.png
   :figwidth: 50%
   :align: center
   
   Aplicación con dos actividades

   
   


Actividad calculadora
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Esta actividad carga el interfaz XML y despues procesa el ``Intent`` para determinar qué operación debe ejecutar.

También define el nombre de los parámetros en **constantes** que tanto el llamador como ella pueden usar (y así evitar el cortar y pegar).


.. code-block:: java

	public class ActividadCalculadora extends Activity {

		public static String nombreNum1=
				"com.ies.actividades2.num1";
		public static String nombreNum2=
				"com.ies.actividades2.num2";
		public static String nombreOp=
				"com.ies.actividades2.op";
		@Override
		protected void onCreate(Bundle savedInstanceState) {
			// TODO Auto-generated method stub
			super.onCreate(savedInstanceState);
			this.setContentView(R.layout.actividad_secundaria);
			Intent intento=this.getIntent();
			float num1=
					intento.getFloatExtra(
							ActividadCalculadora.nombreNum1
							, 
							0.0f);
			float num2=intento.getFloatExtra(
					ActividadCalculadora.nombreNum2
					, 
					0.0f);
			String op=
					intento.getStringExtra(
							ActividadCalculadora.nombreOp);
			
			float resultado=this.calcular(num1, op, num2);
			
			String cadResultado=
					num1+op+num2+"="+resultado;	
			TextView tvResultado;
			tvResultado=(TextView) findViewById(R.id.tvResultado);
			tvResultado.setText(cadResultado);
		}
		public float calcular(float n1, String op, float n2){
			float resultado=0.0f;
			
			if (op.equals("+")){
				return n1+n2;
			}
			if (op.equals("-")){
				return n1-n2;
			}
			return resultado;
		}
	}	

Actividad llamadora
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: java

	public class ActividadPrincipal extends ActionBarActivity {

		@Override
		protected void onCreate(Bundle savedInstanceState) {
			super.onCreate(savedInstanceState);
			setContentView(R.layout.activity_actividad_principal);
		}


		@Override
		public boolean onCreateOptionsMenu(Menu menu) {
			// Inflate the menu; this adds items to the action bar if it is present.
			getMenuInflater().inflate(R.menu.actividad_principal, menu);
			return true;
		}

		@Override
		public boolean onOptionsItemSelected(MenuItem item) {
			// Handle action bar item clicks here. The action bar will
			// automatically handle clicks on the Home/Up button, so long
			// as you specify a parent activity in AndroidManifest.xml.
			int id = item.getItemId();
			if (id == R.id.action_settings) {
				return true;
			}
			return super.onOptionsItemSelected(item);
		}
		
		
		
		/* Dado un id de recurso este método nos
		 * devuelve el texto que hay dentro
		 */
		public float getNumero(int id){
			EditText control;
			control=(EditText) findViewById(id);
			String cadena=control.getText().toString();
			float f=Float.parseFloat(cadena);
			return f;
		}
		public void lanzarActCalculadora(
				float f1, float f2, String op
				){
			Intent intento=new Intent(this, ActividadCalculadora.class);
			intento.putExtra(
					ActividadCalculadora.nombreNum1,
					f1);
			intento.putExtra(
					ActividadCalculadora.nombreNum2,
					f2);
			intento.putExtra(
					ActividadCalculadora.nombreOp,
					op);
			this.startActivity(intento);
		}
		public void calcular(View control){
			RadioButton rbSuma;
			rbSuma=(RadioButton) findViewById(R.id.radSuma);
			if (rbSuma.isChecked()){
				float f1=this.getNumero(R.id.txtNum1);
				float f2=this.getNumero(R.id.txtNum2);
				lanzarActCalculadora(f1,f2,"+");
			}
		}   
	}
	
Modificación del ``AndroidManifest.xml``	
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Se debe añadir esta actividad en el ``AndroidManifest.xml``

.. code-block:: xml

		<activity 
			android:name=".ActividadCalculadora">        
        </activity>

		
Ejercicio
------------------------------------------------------

Crear una aplicación con dos actividades donde una de ellas permita introducir un texto y un número y la otra reciba ambos valores. La segunda truncará los *n* primeros caracteres de la cadena y los mostrará en pantalla.

.. figure:: imagenes/acttruncado.png
   :figwidth: 50%
   :align: center
   
   Ejemplo de funcionamiento del truncado
   
Actividad inicial
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: java

	public class ActPeticionTexto extends ActionBarActivity {

		@Override
		protected void onCreate(Bundle savedInstanceState) {
			super.onCreate(savedInstanceState);
			setContentView(R.layout.activity_act_peticion_texto);
		}


		@Override
		public boolean onCreateOptionsMenu(Menu menu) {
			// Inflate the menu; this adds items to the action bar if it is present.
			getMenuInflater().inflate(R.menu.act_peticion_texto, menu);
			return true;
		}

		@Override
		public boolean onOptionsItemSelected(MenuItem item) {
			// Handle action bar item clicks here. The action bar will
			// automatically handle clicks on the Home/Up button, so long
			// as you specify a parent activity in AndroidManifest.xml.
			int id = item.getItemId();
			if (id == R.id.action_settings) {
				return true;
			}
			return super.onOptionsItemSelected(item);
		}
		private String getCadena(int id){
			EditText controlTexto=
					(EditText) this.findViewById(id);
			return controlTexto.getText().toString();
		}
		
		public void truncar(View control){
			Intent intento=new Intent(this,ActividadTruncadora.class);
			String textoEscrito=
					getCadena(R.id.txtTexto);
			String textoNumCaracteres=
					getCadena(R.id.txtNumero);
			int numCaracteres=Integer.parseInt(
					textoNumCaracteres);
			intento.putExtra(
					ActividadTruncadora.nombreCadena, 
					textoEscrito);
			intento.putExtra(
					ActividadTruncadora.nombreNumCaracteres, 
					numCaracteres);
			this.startActivity(intento);
		}
	}
	

Actividad truncadora
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: java

	public class ActividadTruncadora extends Activity {
		public static String nombreCadena=
				"com.ies.truncado.nombreCadena";
		public static String nombreNumCaracteres=
				"com.ies.truncado.nombreNumCaracteres";
		@Override
		protected void onCreate(Bundle savedInstanceState) {
			// TODO Auto-generated method stub
			super.onCreate(savedInstanceState);
			this.setContentView(R.layout.act_truncado);
			Intent intRecibido=this.getIntent();
			String cad=intRecibido.getStringExtra(
					ActividadTruncadora.nombreCadena
					);
			int numCaracteres=	intRecibido.getIntExtra(
				ActividadTruncadora.nombreNumCaracteres,
				0);
			
			String cadTruncada=this.truncar(cad, numCaracteres);
			Log.d("Truncado", "Resultado:"+cadTruncada);
			TextView tvTextoTruncado=
					(TextView) this.findViewById(R.id.tvResultado);
			tvTextoTruncado.setText(cadTruncada);
		}
		
		/* Recorta los num primeros caracteres*/
		private String truncar (String cad, int num){
			/* Si el usuario intenta poner
			 * un valor más grande que la propia
			 * longitud de la cadena, reducimos el numero
			 */
			if (num>cad.length()){
				num=cad.length();
			}
			return cad.substring(0, num);	
		}
	}
	
Ejercicio 
------------------------------------------------------

Crear una aplicación que permita simular un juego de apuestas a la ruleta.

El usuario puede apostar de 1 a 100 euros y parte con un saldo inicial ficticio de 1000 euros.

La ruleta tiene 37 números (del uno al 36 más el 0, que será un caso especial) y el usuario puede apostar de dos formas:

* Puede apostar a par o impar: si apuesta por ejemplo 2 euros a "Par" y sale por ejemplo, el 18, ganará un 50% más, es decir los dos euros se multiplican por 0'5 y ganará un euro. Si pierde, pierde los dos euros.

* Puede apostar a que el número está en la primera docena (del 1 al 12) en la segunda docena (del 13 al 24) o en la tercera docena (del 25 al 36). Si por ejemplo apostamos 3 euros a la primera docena y sale por ejemplo el 7 multiplicamos por 0,66 los 3 euros y obtendremos 2 euros de beneficio. Si perdemos perdemos los 3 euros que apostamos.

* El 0 significa que la banca gana. No  importa si la apuesta se hizo a "Par" o a "Primera docena". Perderemos todo lo que apostamos.


		
Bases de datos y almacenamiento.
------------------------------------------------------

Android ofrece 5 posibilidades a la hora de almacenar datos:

* Preferencias.
* Almacenamiento interno.
* Almacenamiento externo.
* Bases de datos SQLite.
* Almacenamiento en la nube.

Preferencias compartidas
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Dentro de las preferencias se puede almacenar cualquier tipo de datos básicos: ``String``, ``int``, ``floats``, ``ints`` y ``longs``. Dentro de nuestra actividad podemos usar dos tipos de preferencias

* Preferencias compartidas: lo usaremos cuando queramos manejar muchos ficheros de preferencias, debiendo indicar siempre un nombre de fichero.
* Preferencias únicas. Si solo queremos un fichero de preferencias para la actividad no tendremos que indicar ningún nombre de fichero.

Para escribir valores usaremos el objeto ``SharedPreferences`` de esta forma


.. code-block:: java

    public class Actividad1 extends Activity{
        private static String ficheroPrefs="misPrefs.prf";
        private MODO_FICHERO=MODE_PRIVATE
        protected void onCreate(Bundle estado){
            SharedPreferences prefs=getSharedPrefs(
                    ficheroPrefs, MODO_FICHERO);
            SharedPreferences.Editor editor=
                    prefs.edit();
            editor.putString("nombreUsuario", "pepe");
            /* ¡NO HAY QUE OLVIDAR EL COMMIT!*/
            editor.commit();
        }

Un fichero se puede crear de varias maneras:

* ``MODE_PRIVATE``
* ``MODE_WORLD_READABLE``
* ``MODE_WORLD_WRITABLE``
* ``MODE_MULTI_PROCESS``: Lo usaremos cuando queramos indicar que muchos ficheros van a cambiar a la vez el fichero en forma ``MODE_PRIVATE | MODE_MULTI_PROCESS``.


Por ejemplo, supongamos una aplicación que desea guardar un texto como el nombre de usuario que está almacenado en un control ``EditText``. El código para almacenar sería algo así:

.. code-block:: java


	String ficheroPrefs="nombre_usuario";
	String claveUltimoUsuario="ultimo_usuario";
	
	public void guardar(View control){
    	EditText txtNombre=
    			(EditText) findViewById(R.id.txtNombre);
    	String cadena=
    			txtNombre.getText().toString();
    	SharedPreferences gestorPrefs;
    	
    	
    	gestorPrefs=this.getSharedPreferences(
    			ficheroPrefs, MODE_PRIVATE);
    	
    	SharedPreferences.Editor editor;
    	editor=gestorPrefs.edit();
    	
    	editor.putString(claveUltimoUsuario, 
    			cadena);
    	
    	/* Si no hay commit, no se cierra
    	 * la transacción->No se almacenará
    	 */
    	editor.commit();
    	Log.d("Almacen:", "Cadena almacenada");
    }
    
Almacenamiento interno
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Los ficheros creados aquí son privados a nuestra aplicación. Ni siquiera el usuario puede acceder a ellos (salvo en caso de teléfonos *rooteados* )

Para almacenar haremos algo como esto:

.. code-block:: java

    String fichero  =   "saludo.txt";
    String mensaje  =   "Hola mundo";
    FileOutputStream fos=openFileOutput(fichero, MODE_PRIVATE);
    fos.write(mensaje.getBytes());
    fos.close();
    
Almacenamiento externo
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Implica solicitar permisos como ``READ_EXTERNAL_STORAGE`` o ``WRITE_EXTERNAL_STORAGE``.

El almacenamiento puede estar o no disponible, se debería comprobar con algo como:

.. code-block:: java

	String estado =
		Environment.getExternalStorageState();
	if (Environment.MEDIA_MOUNTED.equals(state)) {
		/* Podemos escribir y además leer*/
		return SE_PUEDE_ESCRIBIR;
	}
	if (Environment.MEDIA_MOUNTED.equals(estado) ||
	Environment.MEDIA_MOUNTED_READ_ONLY.equals(state)) {
		return SOLO_SE_PUEDE_LEER;
	}
	/* Si llegamos aquí no se puede hacer nada*/
	return NO_SE_PUEDE_HACER_NADA;
	
Como vemos, la clave está en la clase ``Environment`` que nos ofrece diversos métodos y constantes para acceder a directorios de la tarjeta.

.. figure:: imagenes/directorios.png
   :figwidth: 50%
   :align: center
   
   Directorios estándar
   
Por ejemplo, el código siguiente ilustra como conseguir crear un subdirectorio en el directorio estándar de imágenes:

.. code-block:: java

	String miDir="mis_imgs";
	File file = 
		new File ( 
		Environment.getExternalStoragePublicDirector
			(Environment.DIRECTORY_PICTURES), 
			miDir
	);
	if (!file.mkdirs()) {
        Log.e("Error", "No se pudo crear "+miDir);
	}

   
Ejercicio
------------------------------------------------------

En los juegos de apuestas, todo jugador siempre desea saber el punto en el que debió dar marcha atrás, sin embargo, no siempre es fácil recordar cual fue.

Para facilitar esto se desea modificar el programa de simulación de la ruleta para que se vaya almacenando todo el historial de apuestas en un fichero llamado ``historial.txt``.

En dicho historial deberíamos ir viendo el saldo, el tipo de apuesta que hizo el usuario (si fue a par, si fue a la segunda docena...), el número que salió al apostar y el estado en que quedó el saldo. Estas operaciones deben almacenarse cada vez que el usuario hace una apuesta del tipo que sea.


Bases de datos
------------------------------------------------------

En Android es perfectamente posible utilizar bases de datos relacionales con prácticamente todas sus características: tablas, claves primarias y ajenas, consultas, etc... El corazón de este sistema es `SQLite <http://sqlite.org>` un gestor de bases de datos pensado para dispositivos reducidos y con versiones para prácticamente todas las plataformas.

Para operar con bases de datos la documentación oficial de Google aconseja utilizar *clases contrato*. En dichas clases se almacenarán los nombres de las tablas, campos y demás, con el fin de facilitar el mantenimiento. Aunque no es obligatorio es muy aconsejable implementar el interfaz ``BaseColumns``. De hecho hay muchos casos en los que Android espera clases que implementen dicho interfaz.

Supongamos que deseamos almacenar información técnica sobre modelos de automóvil y los costes asociados de un seguro: supongamos que hay coches de muchas marcas y modelos, y que para cada uno de ellos se puede contratar un seguro de uno de estos tipos (aunque se desea poder tener más tipos de seguro en el futuro):

* Seguro obligatorio.
* Seguro lunas+incendio sin franquicia.
* Seguro lunas+incendio con franquicia.
* Seguro todo riesgo.

No todos los seguros se ofrecen para todos los coches y de hecho podría haber coches para los cuales la compañía no ofrece ningún seguro. No todos los tipos de seguro tienen el mismo coste para todos los coches. Todo seguro tiene una validez medida en días y siempre es la misma para un cierto tipo de seguro. Por ejemplo, todos los seguros obligatorios tienen una validez de 365 días y todos los "todo riesgo" de 90. Los seguros del tipo "lunas+incendio" tienen todos una validez de 180 días.

Toda marca tiene un nombre y un código único, todo modelo tiene un nombre único y una cilindrada. Todo seguro tiene un un código único y una descripción.

.. figure:: imagenes/diagrama-bd-automoviles.png
   :figwidth: 50%  
   :align: center
   :alt:  Diagrama E/R de la BD Automóviles
   
   
Con esto el SQL que necesitaríamos por ejemplo para la tabla ``Marcas`` sería algo así:

.. code-block:: sql

    create table marcas
    (
        id integer primary key,
        nombre varchar(40)
    );
    
    insert into marca values (1, "Ford");
    insert into marca values (2, "Renault");

Y la clase contrato Java asociada a esta entidad sería:

.. code-block:: java

    public class MarcasContrato implements BaseColumns {
            public static final String 
                    NOMBRE_TABLA="marcas";
            public static final String 
                    NOMBRE_COL_ID="id";
            public static final String 
                    NOMBRE_COL_NOMBRE="nombre";
    }
    
    
    
Bases de datos SQLite
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Supongamos que deseamos crear una base de datos sobre seguros de coches. Un primer elemento necesario sería una tabla donde se almacenen las marcas (cada una llevará un ID).

.. code-block:: sql

	create table marcas (
		id	integer primary key,
		marca	char(30)
	);
	
	insert into marcas values (1, 'Ford');
	insert into marcas values (2, 'Renault');

Para manejar la creación y procesado de esta base de datos Android ofrece la clase ``SQLiteOpenHelper`` de la cual se puede heredar de esta manera:

.. code-block:: java

    public class BD extends SQLiteOpenHelper {
    
            private String sqlCreacion=
                "create table marcas(id integer primary key," + 
                    "nombre varchar(40));\n" ;
            private String insert1="insert into marca values (1, \"Ford\")";
            private String insert2="insert into marca values (2, \"Renault\");";
            public BD(Context context, String name,
                CursorFactory factory, int version) {
                    super(context, name, factory, version);
            }
    
            @Override
            public void onCreate(SQLiteDatabase db) {
                    db.execSQL(sqlCreacion);
                    db.execSQL(insert1);
                    db.execSQL(insert2);
    
            }
    }

Y podemos crear un objeto de la clase BD simplemente instanciándolo

.. WARNING::

   Cuando se hacen pruebas en el simulador es posible que el fichero de base de datos no aparezca hasta que no   intentemos leer o escribir datos de él. Aparte de eso, el fichero suele estar en el directorio ``/data/data/<paquete>`` *pero las ubicaciones pueden cambiar*.

Datos y cursores
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Un objeto del tipo ``SQLiteOpenHelper`` nos puede devolver un objeto ``SQLiteDatabase`` que tiene los métodos necesarios para acceder a cursores, hacer consultas y recorrer los datos. El código siguiente muestra un ejemplo:

.. code-block:: java

    BD gestorBD=new BD(this, "seguros.db", null, 1);
    SQLiteDatabase bd=gestorBD.getReadableDatabase();
    Cursor cursor=bd.rawQuery("select id, nombre from marcas", null);
    cursor.moveToFirst();
    int posID=cursor.getColumnIndex(MarcasContrato.NOMBRE_COL_ID);
    int posNombreMarca=
        cursor.getColumnIndex(MarcasContrato.NOMBRE_COL_NOMBRE);
    while (!cursor.isAfterLast()){
        String numero=cursor.getString(posID);
        String marca=cursor.getString(posNombreMarca);
        Log.d("Marca:", numero+":"+marca);
        cursor.moveToNext();
    }
    cursor.close();
    
	
Ejercicio: ampliación de la BD
------------------------------------------------------
Ampliar la base de datos para que exista una tabla "Modelos" que incluya un par de modelos de cada marca (tiene que haber claves ajenas).

* Marca: Ford, Modelo: Focus
* Marca: Ford, Modelo: Mondeo
* Marca: Renault, Modelo: Megane
* Marca: Renault, Modelo: Kangoo

Hacer un programa que recupere todos los modelos de coche junto con sus marcas y los muestre en pantalla.

Solución a la ampliación de la BD
------------------------------------------------------

En primer lugar, habría que crear el SQL que permita tener la segunda tabla con la clave ajena:

.. code-block:: sql

    create table marcas
    (
        id integer primary key,
        nombre varchar(40)
    );
    
    insert into marcas values (1, "Ford");
    insert into marcas values (2, "Renault");
    
    create table modelos
    (
        id_modelo integer primary key,
        id_marca integer, 
        nombre varchar(40),
        foreign key (id_marca) references marcas (id)
    );
    insert into modelos values(1, 1, 'Focus');
    insert into modelos values(2, 1, 'Mondeo');
    insert into modelos values(3, 2, 'Megane');
    insert into modelos values(4, 2, 'Kangoo');
    
En lugar de insertar todo el código SQL **se puede crear el archivo de base de datos en un ordenador** e insertarlo en el proyecto despues, por desgracia Android no ofrece un soporte cómodo para hacer esto, ya que una vez instalada la app tenemos que copiar el fichero de base de datos al terminal para despues abrirlo.





Creación dinámica del interfaz
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Un problema fundamental en este ejercicio es que no sabemos a priori cuantos controles poner en la aplicación: **el interfaz se tiene que crear dinámicamente**

Supongamos que simplemente deseamos crear un ListView en el que se muestren simplemente los modelos de coche. Se pueden utilizar un par de clases útiles para conseguir lo que queramos de la forma siguiente:

* Metemos los nombres en un vector de Strings.
* Crearemos un fichero de layout para indicar como se mostrará cada modelo. Este layout debe tener un TextView (Android encontrará automáticamente el TextView y en él insertará cada nombre de modelo).
* Usamos la clase ``ArrayAdapter``, que "convierte" cada elemento de nuestro *array* en un elemento del ListView. Le indicaremos el fichero de layout que debe crearse *para cada modelo individual*.
* Indicamos al ListView que use ese ``ArrayAdapter``.


Para conseguir que la clase BD nos devuelve un vector de Strings le podemos añadir el siguiente método:

.. code-block:: java

    public String[] getArrayModelos(){
    	String[] vectorResultado;
    	SQLiteDatabase bd=this.getReadableDatabase();
        Cursor cursor=bd.rawQuery("select nombre from modelos", null);
        vectorResultado=new String[cursor.getCount()];
        cursor.moveToFirst();
        int pos=0;
        while (!cursor.isAfterLast()){
        	vectorResultado[pos]=cursor.getString(0);
        	Log.d("D",  cursor.getString(0));
        	cursor.moveToNext();
        	pos++;
        }
    	return vectorResultado;
    }
    
Y para que ahora la Actividad cree el interfaz dinámicamente podemos hacer algo como esto:

.. code-block:: java

    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_actividad_principal);
        
        /* Recuperamos los modelos*/
        String[] modelosCoche=gestorBD.getArrayModelos();
        /* Cada modelo de coche se insertará en el fichero
         * de layout que tiene un textview donde
         * se pondrá el nombre del modelo */
        ArrayAdapter<String> adaptador=
        		new ArrayAdapter(this, 
        				R.layout.modelo, R.id.tvNombreModelo, 
        				modelosCoche);
        /* El Listview de nuestro interfaz cargará
         * los datos a partir de ese adaptador */
        ListView lvModelos=(ListView) this.findViewById(R.id.lvModelos);
        lvModelos.setAdapter(adaptador);
    }    

Respondiendo al evento click
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Un problema que ocurre es que aunque alguien haga click en algún modelo no ocurre nada. Para conseguir que la actividad procese el evento podemos hacer que la actividad implemente el interfaz ``AdapterView.OnItemClickListener``.

.. WARNING::

   Al hacer esto debemos asegurarnos de poner a ``false`` el atributo ``Focusable`` de los controles. En concreto deberemos ir al fichero de layout que sirve de plantilla para cada control y si por ejemplo hay un ``EditText`` (que podría apoderarse del evento click) modificar su ``Focusable`` como hemos dicho. Se debe hacer esto para todos los controles.

.. code-block:: java

    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_actividad_principal);
        
        /* Recuperamos los modelos*/
        String[] modelosCoche=gestorBD.getArrayModelos();
        /* Cada modelo de coche se insertará en el fichero
         * de layout que tiene un textview donde
         * se pondrá el nombre del modelo */
        ArrayAdapter<String> adaptador=
        		new ArrayAdapter(this, 
        				R.layout.modelo, R.id.tvNombreModelo, 
        				modelosCoche);
        /* El Listview de nuestro interfaz cargará
         * los datos a partir de ese adaptador */
        ListView lvModelos=(ListView) this.findViewById(R.id.lvModelos);
        lvModelos.setAdapter(adaptador);
        /* Activar la gestión de eventos*/
        lvModelos.setOnItemClickListener(this);
    }
    public void onItemClick(AdapterView<?> padre, View control, int posicion,
			long id) {
	Toast.makeText(this, "Click en "+posicion, Toast.LENGTH_SHORT).show();		
    }

Esto se muestra en el emulador de esta forma

.. figure:: imagenes/ListViewDinamico.png
   :figwidth: 50%  
   :align: center
   :alt: Contenedores Android
   

Creación dinámica de controles avanzada
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

El ``ArrayAdapter`` es útil pero solo sirve cuando manejamos cadenas, lo cual es un dato simple. Sin embargo, si queremos representar algo más complejo (por ejemplo, el nombre de modelo y el código de modelo) se necesita una clase más avanzada, que además pueda recuperar datos directamente de un ``Cursor``: la clase ``SimpleCursorAdapter``

Esta clase espera que pasemos cuatro cosas:
* La clase padre: normalmente ``this``
* Un vector con los nombres de las columnas que vamos a extraer de la consulta.
* Un vector con los ``id`` de recursos de controles donde se va a meter el valor de cada columna.
* Flags que puedan modificar el comportamiento de la clase (normalmente usaremos 0).

El problema principal es que esta clase **espera que nuestro cursor tenga algún campo llamado _id** que actúe como identificador así que tendremos que reescribir nuestro SQL y pasarlo de esto
.. code-block:: sql

    select id_modelo, nombre from modelos
    
a esto


.. code-block:: sql

    select id_modelo as _id, nombre from modelos

Así que ahora un método que podría rellenar controles dinámicamente sería este:

.. code-block:: java

    public void rellenarControles(){
    	SQLiteDatabase bd=gestorBD.getReadableDatabase();
    	Cursor cursor=bd.rawQuery("select id_modelo as _id, nombre from modelos",null);
    	String[] nombresColumnas={
    			BaseColumns._ID, 
    			ModelosContrato.NOMBRE_COL_NOMBRE
    	};
    	int[] idTextViews={
    			R.id.tvIdModelo, 
    			R.id.tvModelo
    	};
    	SimpleCursorAdapter adaptador=
    			new SimpleCursorAdapter(this, 
    					R.layout.modelos_avanzados,cursor,
    					nombresColumnas, idTextViews,0);
    	ListView lvModelos=(ListView) this.findViewById(R.id.lvModelos);
        lvModelos.setAdapter(adaptador);
        lvModelos.setOnItemClickListener(this);
    }
    
    
.. WARNING::

   El código para cargar datos o crear controles puede ser lento y podría llegar a bloquear el interfaz. Es recomendable delegar todo el código que pueda ser lento a una ``AsyncTask``

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

