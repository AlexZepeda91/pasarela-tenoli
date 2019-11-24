
## Instalación de Pasarela Tenoli

  Esta son las instrucciones para instalar la pasarela de Tenoli en su institución. Esta es la plataforma distribuida de itnercambio de información del Gobierno de El Salvador. Puede [conocer más sobre Tenoli en esta página](http://tenoli.gobiernoelectronico.gob.sv/).
  
**Requisitos:** 
* Ubuntu Bionic LTS / Debian 10 o superior. 
* Al menos 2GB Ram y 3GB de disco duro.  
* Dos direcciones IP: una para acceso a la red interna y otra para acceso a Internet.
* La IP de acceso público debe tener disponibles los siguientes puertos TCP:  
	 - Ingreso: 5500, 5577, 9011, 9999 
	- Egreso: 5500, 5577, 4001, 2080, 80, 443 
* La IP de interna debe tener disponibles los siguientes puertos TCP:  
Ingreso: 80,443

El equipo puede ser una maquina virtual o un servidor físico dedicado.  

**1. Instalar**

1. Antes de iniciar debe obtener los archivos de instalacioón, puede hacerlo con los siguientes comandos:
```sh
~# cd /opt/
~# wget https://github.com/egobsv/pasarela-tenoli/archive/master.zip
~# unzip master.zip;mv pasarela-tenoli-master tenoli;
~# cd /opt/tenoli; chmod +x instalar.sh
```
2. Descarque los paquetes DEB y guardelos en su servidor usando los siguientes comandos:
```sh
~# mkdir /opt/tenoli/debs;
~# cd /opt/tenoli/debs;
~# wget -r -nH -np --cut-dirs=1 http://tenoli.gobiernoelectronico.gob.sv/paquetes/deb/;
```
3. Modifique las direcciones IP y define el usuario y contraseña de administrador de la pasarela dentro del archivo ss-respuestas.txt. La instalación creara el usaurio automaticamente, una vez realizados los cambios ejecute el script de instalación:
```sh
~# cd /opt/tenoli/;
~# nano ss-respuestas.txt;
~# ./instalar.sh
```

**2. Inicializar Pasarela**

Conéctese a la pasarela desde el navegador https://[DIRECCION IP]:4000/

Use las credenciales que creó en el paso anterior, el sistema ingresa y pide el ancla de configuración inicial.

El ancla de  configuración inicial contiene los parámetros de inicio definidos en el servidor central.  Esta información, guardada en un archivo XML conocido como ‘ancla de configuración’, está [disponible aquí.](https://github.com/egobsv/pasarela-tenoli/blob/master/ancla-configuracion.xml)  
  
Descargue, guarde este archivo en su máquina y luego regrese a la página de configuración de su pasarela para que pueda importar el ancla de configuración inicial.

* Ingresar el código presupuestario asignado por el Ministerio de Hacienda a su institución, por ejemplo, para Ministerio de Economía el código es 4100.

* Ingresar el identificador de su pasarela, este valor sera parte del certificado electrónico que identificará a su pasarela. Se recomienda usar el [formato ETSI para la identificación de entidades](https://www.etsi.org/deliver/etsi_en/319400_319499/31941201/01.01.01_60/en_31941201v010101p.pdf) que incluye [XX-código país][XXX-fuente del código][####-código]-[##-correlativo de pasarela insitucional] ejemplo: MIHSV4100-01. 

* Ingrese el número PIN de acceso para proteger los certificados del servidor. Este PIN será requerido para administrar los certificados de su pasarela.

* Presionar 'Continuar', para guardar los cambios y entrar por primera vez.  
  
* Una vez dentro, desde el Menú Principal, seleccionar 'Parámetros del Sistema', agregue el servicio de sellado de tiempo.

* En la parte superior de la pantalla tendrá una aviso que le indica que necesita ingresar su número PIN, presiónelo e ingrese el código que ingreso en la pantalla inicial. Edite el archivo '/etc/xroad/autologin' en su servidor con su nuevo PIN para que evitar ingresarlo manulamente.
  
**3. Registrar Pasarela**

Para terminar, es necesario registrar nuestra pasarela para que pueda unirse a la red Tenoli.  
Este registro se hace a través de certificados de Firma Electrónica Simple  y es aprobado desde la Autoridad Certificadora de SETEPLAN.  
Para iniciar este registro ingrese a la sección de 'Llaves y certificados' y genere las solicitudes de registro siguientes usando los datos de su institución.

* Certificado de Autorización - Este certificado será utilizado por las instituciones miembro de la red Tenoli para identificar a su pasarela. Para crearlo debe presionar el botón 'Generar Llave', luego el botón 'Generar CSR', y en el recuadro a continuación:
		Seleccione la autoridad "certifiacdora raíz" y Formato CSR "PEM" antes de presionar el botón OK para avanzar  		      la siguiente paso.
		En el segundo recuadro, ingrese el nombre de su institución y el nombre de dominio que usara su pasarela, 		  ej: tenoli.minec.gob.sv 

* Certificado de Firma - Este certificado será utilizado por su pasarela para firmar mensajes. Para crearlo debe presionar el botón 'Generar Llave', luego el botón 'Generar CSR', y en el recuadro a continuación:
		Seleccione la autoridad "certifiacdora raíz" y Formato CSR "PEM" antes de presionar el botón OK para avanzar  		      la siguiente paso. En el segundo recuadro, ingrese el nombre de su institución.


El sistema genera y descarga automáticamente las peticiones de certificados a su maquina, estas deberán ser enviadas a l correo dquijada @ presidencia.gob.sv. Una vez procesadas las solicitudes, se entregarán los certificados para que pueda finalizar el registro.

**4. Finalizar Registro**

Usando los certificados que recibió, ingrese a su pasarela, seleccione la opcion 'Llaves y certificados', presione el botón 'importar certificado' y luego importe sus certificados de Autorización y Firma.  
  
Con el certificado de Identidad, una vez importado, deberá realizarse el registro de su pasarela. Seleccione el certificado Identidad, y presione el botón 'Activar' y luego 'Registrar'. El estado del certificado cambia a 'registro en progreso'. 

Una vez la administración central de Tenoli autorice el registro, el estado cambiara a 'registrado'  
  
Es importante verificar que la página de diagnóstico, desde el menú principal, no muestre errores. Si aparece algún  error (indicador rojo) revise el Firewall de su red, es probable que se este boqueando alguna conexión.  
  
Con esto queda activada nuestra pasarela dentro de la red Tenoli. El siguiente paso es [agregar servicios](http://tenoli.gobiernoelectronico.gob.sv/servicios-rest.html) para consumir o compartir datos con otras instituciones.

## Licencia

Este trabajo esta cubierto dentro de la estrategia de desarrollo de servicios de Gobierno Electrónico del Gobierno de El Salvador y como tal es una obra de valor público sujeto a los lineamientos de la Política de Datos Abiertos y la licencia [CC-BY-SA](https://creativecommons.org/licenses/by-sa/3.0/deed.es).  
