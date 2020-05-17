# **Changelog V2.0**

La versión 2.0 incluye la integración con Controladores Fiscales Epson TM900/TM220, la posibilidad de enviar automáticamente los archivos de auditoría a la AFIP, logs de transacciones para el usuario, actualizaciones automáticas y considerables simplificaciones para los implementadores del producto.

## **Integración Controladores Fiscales EPSON TM900 / TM-220**

- Se puede seleccionar el modelo del controlador fiscal. Si el modelo seleccionado es distinto al que se detecta durante la prueba de conexión, se mostrará un aviso.
- Se puede conectar por puerto COM o USB. 
- Para las conexiones por USB, el puerto se detecta solo por lo que no hace falta ingresar un número.
- Es posible testear la conexión desde el configurador.
- El resultado final será un archivo ZIP que contendrá los archivos de auditoría F8010, F8011 Y F8012.
- Se tiene en cuenta la posible situación de que no se esté tratando de hacer una bajada de la auditoría de manera concurrente; este caso es detectado por el servicio y se procederá a hacer la descarga por todo el período pendiente.

## **Envío de informes F8011 Y F8012 a AFIP**

- Es posible conocer el estado de lo servicios AFIP de producción para informar declaraciones juradas. 
- Es posible verificar si el certificado configurado permitirá autenticar correctamente contra el servicio de declaraciones juradas. 
- Es posible verificar si el certificado configurado permitirá autenticar correctamente contra el serivicio de facturación electrónica. Notar que esta funcionalidad no es relevante para la ejecución del servicio. Es una funcionalidad meramente destinada al troubleshooting de los implementadores ante la posible eventualidad de que el certificado parezca no funcionar correctamente. 
- Es posible configurar al servicio para que reintente la subida de archivos de auditoría sin informar.
- Es posible reintentar la subida de archivos de auditoría pendientes manualmente.
- Sólo se informan los archivos F8011 Y F8012.
- Se puede definir que la solución utilice el Cuit presente en los archivos de auditoría. Este cuit es el que se encuentra configurado en el controlador fiscal.
- Si algún PEM falla durante el envío a AFIP, el archivo ZIP no se indetificará como subido. Esto logrará que se vuelva a intentar la subida.
- El servicio verifica contra el archivo transaccional de PEMs ("RegistroAuditoriaTransccionesPEMCF.csv") si el PEM se ha informado previamente. Si se encuentra el pem y el mismo tiene un número de transacción, se salteará el proceso. Esto es debido a la posibilidad de que un formulario se haya subido correctamente pero el segundo no y por lo tanto el archivo ZIP sería identificado como pendiente de informar, a pesar de que ya se ha informado al menos un PEM correctamente.

## **Log Transaccional**

- Existen dos archivos "RegistroAuditoriaTransccionesZipCF.csv" y "RegistroAuditoriaTransccionesPEMCF.csv". El primero registra lo que sucede con los archivos ZIP. El segundo registra lo que sucede con los archivos PEM individuales. Por el momento, sólo los archivos PEM se suben a AFIP, por lo que el segundo archivo tendrá los códigos únicos.
- Los archivos ahora además de tener un número único, terminan con la letra "F" o "T" para simbolizar si se informaron a AFIP. Una vez el archivo informado satisfactoriamente, se modificará la última letra a "T" y se escrbirá en el Registro de Transacciones la fecha y el código transaccional enviado por AFIP.
- Crea la carpeta si no existe
- Crea el csv si no existe
- Registra en el log a medida q se avanza
- Archivos ahora tienen un identificador unico	
- Validacion y captura de los archivos csv abiertos. En el caso de que alguno de los archivos se encuentre abierto por el usuario, el servicio chequeará cada un minuto si el usuario cerró el archivo. En caso positivo, seguirá el proceso.

## **Actualizaciones automáticas**

- Al iniciar el configurador, se chequea la versión y de haber actualizaciones pendientes, se pregunta si se desea actualizar.
- Es posible actualizar el configurador y servicio presionando un botón.
- Permite configurar servidor y credenciales para descargar la nueva versión.
- Permite activar actualizaciones automáticas que se ejecutarán desde el servicio. De este modo, cuando publicamos una nueva versión, todos los clientes van comenzar una actualización sin necesidad de la intervención de implementadores. [50% terminado.]

## **Información y Funcionalides extras agregadas**

- Se pueden visualzar los logs del servicio desde el configurador.
- Al hacer doble click sobre un log, se abrirá el mismo.
- En el visualizador de logs solo se muestran los archivos .txt que comienzan con la palabra "logService"
- Las pantallas de notificaciones son propias ahora y no las estandard de windows.
- Se muestra el último código único descargado por el servicio. 
- Se muestra el último rango de fechas descargado.
- Se muestra el último código AFIP en caso de estar conectado al webservice.
- Si hay archivos pendientes de subir, se puede definir la aplicación para que los intente subir nuevamente. Por defecto esto se encuentra activado.
- El log del servicio ahora muestra los errores de una manera más clara para su fácil deteción.
- Se agregaron tooltips a casi todos los elementos del configurador. Posicionando el mouse sobre un elemento mostrará el Tooltip.
- En controlador fiscal, se mostrarán sólo las opciones posibiles para cada tipo de controador. Ej: Para "Epson", no se podrá seleccionar "HTTP".
- Es posible forzar la recreación del archivo de configuración.
- Se puede agregar contraseña al configurador para evitar el uso posible de los usuarios. Sólo se pide contraseña en el caso que haya una configurada.
- Se muestra una pestaña de confirmación para ciertas funciones.
- La ventana inicial que recuerda que la herramienta se debe ejecutar con permisos de administrador ahora sólo muestra el mensaje si la herramienta no se está ejecutando como administrador.
- No se puede desinstalar el fiscal controller a no ser que el servicio se encuentre parado.
- En varias pestañas se muestra el logo de Stec. 
- El logo de la aplicación de Stec tiene fondo transparente en la barra de tareas y en Quitar Programas.
- Arreglos menores en la GUI, como el estatus del servicio mostrandose completo o validaciones de qué elementos deben estar activos.

## **Detalle GUI del Fiscal Controller V2.0**

- Ventana Principal 
  - Configuración
    - Se reordenó la pestaña.
    - Ahora no se puede seleccionar la carpeta para almacenar el log a no ser que el checkbox "Habilitar Log" se encuentra activado.
  - Servicio
    - Se quitó la ruta del archivo de configuración del servicio. Ahora es una detección automática.
    - Se agregó un botón para abrir la carpeta donde se encuentran los archivos de auditoría.
    - Se agregó un botón para abrir el archivo transaccional ZIP.
    - Se agregó un botón para abrir el archivo transaccional PEM.
    - Se agregó un visor de los archivos log del servicio. Haciendo doble click se puede abrir el log deseado.
    - Se agregó un botón para refrescar el visor de logs. El visor se carga durante el inicio del configurador.
    - Se muestra el último código único descargado por el servicio. 
    - Se muestra el último rango de fechas descargado.
    - Se muestra el último código AFIP en caso de estar conectado al webservice.
  - Controlador Fiscal
    - Se agregó la posibilidad de seleccionar "Epson".
    - Se agregó la posibilidad de seleccionar el modelo para controladores fiscales Epson
    - Al seleccionar "Epson" o "Hasar", se mostrará la información pernitente a cada controlador.
    - Para controladores Epson, se incluyó un botón para testear la conexión con el controlador.
    - Se muestra un aviso si la impresora Epson detectada es distinta a la de la configuración.
  - WebService AFIP (nueva pestaña)
    - Se puede ingresar la ruta del certificado pfx para comunicarse con AFIP.
    - Se puede ingresar el Cuit.
    - Permite definir si se reintentará subir aquellas auditorías que no se lograron subir automáticamente la primera vez.
    - Permite definir si utilizar el valor del cuit que figura en los archivos de auditoría.
    - Permite verificar el estado de los servidores de AFIP correspondientes.
    - Permite verificar si la configuración es correcta para autenticar con AFIP y si el certificado permite informar declaraciones juradas.
  - Actualizaciones Automáticas (nueva pestaña)
    - Permite configurar para permitir que el servicio actualice automáticamente.
    - Permite ingresar servidor y credenciales descargar la actualización.
    - Botón que verifica versiones e inicia la descarga de actualizaciones.
  - Avanzado (nueva pestaña)
    - Se agregó un botón para regenerar el archivo de configuración. 
    - Se agregó un botón para verificar si el certificado que se encuentra definido en la pestaña "WebService AFIP" permite utilizarse para informar facturas electrónicas.
    - Botón para verficar si existe auditoría pendiente por enviar.
    - Se puede ingresar una contraseña para el configurador.
- Ventana de Password (nueva ventana)
  - Se visualiza el logo de Stec.
  - Se puede ingresar la contraseña.

## **Bug Fixes**

- Si faltaban datos en el archivo de configuracion, era posible que la fecha del día sea incorrectamente generada. Ej:  20051 en vez de 200501
- En el archivo de configuración, el fabricante se mostraba incorrectamente por lo que podía llegar a causar un eventual error. Nunca sucedió ya que sólo se utilizaba Hasar.
- En arquitecturas x86, el configurador no mostraba correctamente la licencia y podía generar confusión y un eventual error de licenciamiento.
- Algunos archivos podían no copiarse correctamente durante la instalación.



