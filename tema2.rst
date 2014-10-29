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
	* ``match_parent``: "agrándate y adápte al tamaño de tu contenedor padre".

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

.. figure:: ..imagenes/contenedores.png
   :figwidth: 50%  
   :align: center
   :alt: Contenedores Android
   
   Contenedores Android
   
   
   





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

