---

copyright:
  years: 2018
lastupdated:  "2018-05-11"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:tip: .tip}
{:pre: .pre}

#	Direct Update en aplicaciones de Cordova
{: #direct_update_cordova_apps}

Los recursos web (JavaScript, HTML, CSS o archivos de imagen) en aplicaciones de Cordova se pueden actualizar *instantáneamente (over-the-air, OTA)* con Direct Update. Mediante la característica Direct Update, las empresas pueden asegurarse de que los usuarios finales utilicen la versión más reciente de sus apps.
Para actualizar una aplicación, los recursos web actualizados de la aplicación necesitan empaquetarse y cargarse al servidor de MobileFirst, utilizando la CLI de MobileFirst o desplegando un archivo de archivado generado. Direct Update se activará entonces automáticamente. Se aplicará entonces en cada solicitud de usuario a un recurso protegido.

Se da soporte a Direct Update en las plataformas Cordova iOS y Cordova Android.

A efectos de desarrollo y pruebas, los desarrolladores habitualmente utilizarán Direct Update simplemente subiendo un archivador al servidor de desarrollo. Aunque este proceso es fácil de implementar, no es seguro. Para esta fase, se utilizará un par de claves RSA internas extraídas de un certificado firmado automáticamente de MobileFirst incorporado.

Sin embargo, para la fase de pruebas de preproducción o de producción, se recomienda implementar Direct Update seguro antes de publicar la aplicación en la tienda de apps. Una versión de Direct Update segura necesita una pareja de claves RSA extraídas de un certificado de servidor firmado por una autoridad de certificación (CA) real.

* Tenga cuidado de no modificar la configuración del almacén de claves una vez que se publique la aplicación. Las actualizaciones descargadas no pueden autenticarse antes de volver a configurar la aplicación con una nueva clave pública y de volver a publicar la aplicación. Si no realiza los dos pasos anteriores, Direct Update fallará en el cliente.
* Direct Update actualiza solo los recursos web de la aplicación. Si desea actualizar recursos nativos, se debe enviar una nueva versión de la aplicación a las respectivas tiendas de apps.
* Cuando se utiliza la característica Direct Update y está habilitada la característica de suma de comprobación de recursos web, se establece una nueva base de suma de comprobación con cada Direct Update.
* Si el servidor de MobileFirst se ha actualizado utilizando un fixpack, seguirá sirviendo a las actualizaciones directas correctamente. Sin embargo, si se sube un archivador de Direct Update compilado recientemente (archivo .zip), puede detener las actualizaciones en los clientes antiguos. La razón es que el archivado contiene la versión del plug-in de `cordova-plugin-mfp`. Antes de servir dicho archivador a un cliente móvil, el servidor compara la versión del cliente con la versión del plugin. Si ambas versiones son lo suficientemente cercanas (los tres dígitos más significativos son los mismos), Direct Update funcionará de forma normal. De lo contrario, el servidor de MobileFirst omitirá la actualización de forma silenciosa. Una solución para la discordancia de versiones es descargar el `cordova-plugin-mfp` con la misma versión que la del proyecto de Cordova original y volver a generar el archivado de Direct Update.
* En condiciones óptimas, un único servidor de MobileFirst puede enviar datos a clientes a la velocidad de 250 MB por segundo. Si son necesarias velocidades más elevadas, considere la posibilidad de utilizar un clúster o un servicio de una CDN.
{: tip}

## ¿Cómo funciona Direct Update?
{: #working_direct_update}

Los recursos web de la aplicación inicialmente de empaquetan con la aplicación para asegurar primero una disponibilidad fuera de línea. Posteriormente, la aplicación busca actualizaciones en cada solicitud del servidor de MobileFirst.

>**Nota:** Una vez que se realice una Direct Update, se comprobará de nuevo tras 60 minutos.

![Diagrama de cómo funciona la actualización directa](images/internal_function.jpg)

Después de una actualización de Direct Update, la aplicación deja de utilizar los recursos web empaquetados de forma previa. En su lugar, utiliza los recursos web descargados desde el recinto de pruebas de la aplicación. Si la memoria caché de aplicaciones del dispositivo se borra, se utilizarán de nuevo los recursos web empaquetados originales.

>Una actualización de Direct Update se aplica únicamente a una versión específica. En otras palabras, las actualizaciones generadas para una aplicación con la versión 2.0 no se pueden aplicar a otra versión de la misma aplicación.

## Creación y despliegue de recursos web actualizados
{: #creating_deploying_updates}

Los recursos web actualizados necesitan empaquetarse y cargarse en el servidor de MobileFirst.

1. Abra una ventana de línea de mandatos y vaya a la raíz del proyecto de Cordova.
2. Ejecute el mandato:
  ```
  mfpdev app webupdate
  ```
  {: pre}
El mandato `mfpdev app webupdate` empaqueta los recursos web actualizados en un archivo `.zip` y los cargará al servidor de MobileFirst predeterminado ejecutándose en la estación de trabajo del desarrollador. Los recursos web empaquetados se pueden encontrar en la carpeta `[cordova-project-root-folder]/mobilefirst/`.

**Pasos alternativos:**

* Cree el archivo `.zip` y cárguelo en un servidor de MobileFirst distinto: `mfpdev app webupdate [server-name] [runtime-name]`.
  Por ejemplo:
  ```
  mfpdev app webupdate myQAServer MyBankApps
  ```
  {: pre}

* Cargue un archivo `.zip` generado de forma previa: `mfpdev app webupdate [server-name] [runtime-name] --file [path-to-packaged-web-resources]`.
  Por ejemplo:
  ```
  mfpdev app webupdate myQAServer MyBankApps --file mobilefirst/ios/com.mfp.myBankApp-1.0.1.zip
  ```
  {: pre}

* Cargue manualmente recursos web empaquetados en el servidor de MobileFirst:
  1. Compile el archivo .zip sin cargarlo:
      ```
      mfpdev app webupdate --build
      ```
      {: pre}
  2. Cargue la consola de operaciones de MobileFirst y pulse la entrada de aplicación.
  3. Pulse en **Subir archivo de recursos web** para subir los recursos web empaquetados.    
      ![Cargar el archivo .zip de Direct Update desde la consola](images/upload-direct-update-package.png)

Ejecute el mandato `mfpdev help app webupdate` para obtener más información.
{: tip}

## Experiencia del usuario
{: #user_experience}

Una vez que se reciba una Direct Update, se mostrará un diálogo de forma predeterminada, y se le pedirá permiso al usuario para iniciar el proceso de actualización. Después de que el usuario lo apruebe, se visualiza un diálogo con una barra de progreso y los recursos web se descargan. La aplicación se recarga de forma automática cuando la actualización finaliza.

![Ejemplo de actualización directa](images/direct-update-flow.png)

## Personalización de la interfaz de usuario de Direct Update
{: #customize_du_ui}

La IU de Direct Update presentada al usuario final se puede personalizar.
Añada el siguiente código dentro de la función `wlCommonInit()` en **index.js**:
```JavaScript
wl_DirectUpdateChallengeHandler.handleDirectUpdate = function(directUpdateData, directUpdateContext) {
    // Implement custom Direct Update logic
};
```
{: codeblock}

*directUpdateData* es un objeto JSON que contiene la propiedad downloadSize que representa el tamaño de archivo (en bytes) del paquete de actualización que se descargará desde el servidor de MobileFirst.
*directUpdateContext* es un objeto JavaScript que expone las funciones .start() y .stop(), lo que inicia y detiene el flujo de Direct Update.

Si los recursos web son más nuevos en el MobileFirst Server que en la aplicación, los datos de solicitud de Direct Update se añaden a la respuesta del servidor. Cuando el marco del lado del cliente de MobileFirst detecta esta solicitud de actualización directa, se invoca la función `wl_directUpdateChallengeHandler.handleDirectUpdate`.

La función proporciona un diseño predeterminado de Direct Update: Un diálogo de mensaje predeterminado que se muestra cuando hay disponible una Direct Update y una pantalla de progreso predeterminada que se muestra cuando se inicia el proceso de actualización directa. Puede implementar el comportamiento personalizado de la interfaz de usuario de Direct Update o personalizar el recuadro de diálogo de Direct Update sustituyendo esta función e implementando su propia lógica.

En el código de ejemplo siguiente, una función `handleDirectUpdate` implementa un mensaje personalizado en el diálogo de Direct Update. Añada este código al archivo `www/js/index.js` del proyecto de Cordova.
Ejemplos adicionales para una interfaz de usuario (IU) de Direct Update personalizada:
* Un diálogo creado utilizando un marco de JavaScript de terceros (como Dojo o jQuery Mobile, Ionic, etc.)
* Interfaz de usuario nativa completa mediante la ejecución de un plugin de Cordova
* Un HTML alternativo que se presenta al usuario con opciones,
y así sucesivamente.

```JavaScript
wl_directUpdateChallengeHandler.handleDirectUpdate = function(directUpdateData, directUpdateContext) {        
    navigator.notification.confirm(  // Creates a dialog.
        'Custom dialog body text',
        // Handle dialog buttons.
          directUpdateContext.start();
        },
        'Custom dialog title text',
        ['Update']
    );
};
```
{: codeblock}

El proceso de Direct Update se inicia ejecutando el método `directUpdateContext.start()` siempre que el usuario pulsa el botón de diálogo. Se muestra la pantalla de progreso predeterminada, que se asemeja a la de versiones anteriores del servidor de MobileFirst.

Este método da soporte a los siguientes tipos de invocación:
* Cuando no se especifican parámetros, el servidor de MobileFirst utiliza la pantalla de progreso predeterminada.
* Cuando se proporciona una función de escucha como, por ejemplo, `directUpdateContext.start(directUpdateCustomListener)`, el proceso de Direct Update se ejecuta en un segundo plano mientras envía sucesos de ciclo de vida al escucha. El escucha personalizado debe implementar los siguientes métodos:

```JavaScript
var  directUpdateCustomListener  = {
    onStart : function ( totalSize ){ },
    onProgress : function ( status , totalSize , completedSize ){ },
    onFinish : function ( status ){ }
};
```
{: codeblock}

Los métodos del escucha se inician durante el proceso de actualización directa de acuerdo con las siguientes reglas:
* Se llama a `onStart` con el parámetro `totalSize` que contiene el tamaño del archivo de actualización.
* Se llama varias veces a `onProgress` con el estado `DOWNLOAD_IN_PROGRESS`, `totalSize` y `completedSize` (volumen de lo descargado hasta el momento).
* Se llama a `onProgress` con el estado `UNZIP_IN_PROGRESS`.
* Se llama a `onFinish` con uno de los siguientes códigos de estado final:

| Código de estado | Descripción |
|:------------|:------------|
| `SUCCESS` | La actualización directa finalizó sin errores. |
| `CANCELED` | Se canceló la actualización directa (por ejemplo, debido a que se llamó al método `stop()`). |
| `FAILURE_NETWORK_PROBLEM` | Se produjo un problema con una conexión de red durante la actualización. |
| `FAILURE_DOWNLOADING` | El archivo no se ha descargado completamente. |
| `FAILURE_NOT_ENOUGH_SPACE` | No hay espacio suficiente en el dispositivo para descargar y desempaquetar el archivo de actualización. |
| `FAILURE_UNZIPPING` | Se ha producido un problema desempaquetar el archivo de actualización. |
| `FAILURE_ALREADY_IN_PROGRESS` | Se ha llamado al método de inicio mientras ya se estaba ejecutando una actualización directa. |
| `FAILURE_INTEGRITY` | No se ha podido verificar el archivo de actualización. |
| `FAILURE_UNKNOWN` | Error interno inesperado. |

Si implementa un escucha de actualización directa personalizado, debe asegurarse de que la app se recarga cuando se complete el proceso de dicha actualización directa y se haya llamado al método `onFinish()`. También se debe llamar a `wl_directUpdateChalengeHandler.submitFailure()` si el proceso de actualización directa no se completa de forma satisfactoria.

En el siguiente ejemplo se muestra una implementación de un escucha de actualización directa personalizado:
```JavaScript
var directUpdateCustomListener = {
  onStart: function(totalSize){
    //show custom progress dialog
  },
  onProgress: function(status,totalSize,completedSize){
    //update custom progress dialog
  },
  onFinish: function(status){

    if (status == 'SUCCESS'){
      //show success message
      WL.Client.reloadApp();
    }
    else {
      //show custom error message

      //submitFailure must be called is case of error
      wl_directUpdateChallengeHandler.submitFailure();
    }
  }
};

wl_directUpdateChallengeHandler.handleDirectUpdate = function(directUpdateData, directUpdateContext){

  WL.SimpleDialog.show('Update Avalible', 'Press update button to download version 2.0', [{
    text : 'update',
    handler : function() {
      directUpdateContext.start(directUpdateCustomListener);
    }
  }]);
};
```
{: codeblock}

### Escenario: Ejecución de actualizaciones directas sin una interfaz de usuario
{: #scenario-running-ui-less-direct-updates }
{{site.data.keyword.mobilefoundation_short}} da soporte a las actualizaciones directas sin una interfaz de usuario cuando la aplicación se encuentra en un segundo plano.

Para ejecutar las actualizaciones directas sin interfaz de usuario, implemente `directUpdateCustomListener`. Proporcione implementaciones de función vacías para los métodos `onStart` y `onProgress`. Las implementaciones vacías harán que el proceso de la actualización directa se ejecute en un segundo plano.

Para completar el proceso de la actualización directa, se debe recargar la aplicación. Las siguientes opciones están disponibles:
* El método `onFinish` también puede estar vacío. En este caso, la actualización directa se aplicará después de que se haya reiniciado la aplicación.
* Se puede implementar un diálogo personalizado que informe o solicite al usuario el reiniciar la aplicación. (Véase el ejemplo siguiente).
* El método `onFinish` puede forzar la recarga de la aplicación llamando a `WL.Client.reloadApp()`.

A continuación se muestra una implementación de ejemplo de `directUpdateCustomListener`:

```JavaScript
var directUpdateCustomListener = {
  onStart: function(totalSize){
  },
  onProgress: function(status,totalSize,completeSize){
  },
  onFinish: function(status){
    WL.SimpleDialog.show('New Update Available', 'Press reload button to update to new version', [ {
      text : WL.ClientMessages.reload,
      handler : WL.Client.reloadApp
    }]);
  }
};
```
{: codeblock}

Implemente la función `wl_directUpdateChallengeHandler.handleDirectUpdate`. Pase la implementación de `directUpdateCustomListener` que haya creado como un parámetro para la función. Asegúrese de que se llama a `directUpdateContext.start(directUpdateCustomListener`). A continuación se muestra la implementación de `wl_directUpdateChallengeHandler.handleDirectUpdate` de ejemplo:

```JavaScript
wl_directUpdateChallengeHandler.handleDirectUpdate = function(directUpdateData, directUpdateContext){

  directUpdateContext.start(directUpdateCustomListener);
};
```
{: codeblock}

**Nota:** Cuando la aplicación se envía a un segundo plano, se suspende el proceso de actualización directa.

### Escenario: Cómo manejar una anomalía de una actualización directa
{: #scenario-handling-a-direct-update-failure }
Este escenario muestra cómo manejar una anomalía en una actualización directa que puede ser originada, por ejemplo, por una pérdida de conectividad. En este escenario, el usuario deja de poder utilizar la app incluso en la modalidad de fuera de línea. Se visualiza un diálogo ofreciendo al usuario la opción de intentarlo de nuevo.

Cree una variable global para almacenar el contexto de la actualización directa de forma que lo puede utilizar más tarde cuando el proceso de actualización directa falle. Por ejemplo:

```JavaScript
var savedDirectUpdateContext;
```
{: codeblock}

Implemente un manejador de desafío de actualización directa. Aquí se guarda el contexto de la actualización directa. Por ejemplo:

```JavaScript
wl_directUpdateChallengeHandler.handleDirectUpdate = function(directUpdateData, directUpdateContext){

  savedDirectUpdateContext = directUpdateContext; // save direct update context

  var downloadSizeInMB = (directUpdateData.downloadSize / 1048576).toFixed(1).replace(".", WL.App.getDecimalSeparator());
  var directUpdateMsg = WL.Utils.formatString(WL.ClientMessages.directUpdateNotificationMessage, downloadSizeInMB);

  WL.SimpleDialog.show(WL.ClientMessages.directUpdateNotificationTitle, directUpdateMsg, [{
    text : WL.ClientMessages.update,
    handler : function() {
      directUpdateContext.start(directUpdateCustomListener);
    }
  }]);
};
```
{: codeblock}

Cree una función que inicie el proceso de actualización directa utilizando el contexto de la actualización directa. Por ejemplo:

```JavaScript
restartDirectUpdate = function () {
  savedDirectUpdateContext.start(directUpdateCustomListener); // use saved direct update context to restart direct update
};
```
{: codeblock}

Implemente `directUpdateCustomListener`. Añada una comprobación de estado en el método `onFinish`. Si el estado se inicia con `FAILURE`, abra un modal solo de diálogo con la opción **Intentar de nuevo**. Por ejemplo:

```JavaScript
var directUpdateCustomListener = {
  onStart: function(totalSize){
    alert('onStart: totalSize = ' + totalSize + 'Byte');
  },
  onProgress: function(status,totalSize,completeSize){
    alert('onProgress: status = ' + status + ' completeSize = ' + completeSize + 'Byte');
  },
  onFinish: function(status){
    alert('onFinish: status = ' + status);
    var pos = status.indexOf("FAILURE");
    if (pos > -1) {
      WL.SimpleDialog.show('Update Failed', 'Press try again button', [ {
        text : "Try Again",
        handler : restartDirectUpdate // restart direct update
      }]);
    }
  }
};
```
{: codeblock}

Cuando el usuario pulsa el botón **Try Again**, la aplicación reinicia el proceso de actualización directa.

## Actualizaciones directas completas y delta
{: #delta-and-full-direct-update }
Las actualizaciones directas de tipo delta (actualizaciones de diferencias) permiten que una aplicación descargue solo los archivos que han cambiado desde la última actualización en lugar de descargar todos los recursos web de la aplicación. Esto reduce el tiempo de descarga, conserva el ancho de banda y mejora la experiencia global del usuario.

> <span class="glyphicon glyphicon-exclamation-sign" aria-hidden="true"></span> **Importante:** Una **actualización delta** solo es posible si los recursos web de la aplicación del cliente están una versión por debajo de versión de la aplicación actualmente desplegada en el servidor. Las aplicaciones de cliente que están más de una versión por detrás de la aplicación desplegada actualmente (es decir, la aplicación se desplegó en el servidor al menos dos veces desde que la aplicación de cliente se actualizó), recibirán una **actualización completa** (es decir, se descargarán y actualizarán todos los recursos web).


## Aplicación de ejemplo
{: #sample-application }
[Pulse para descargar el proyecto de Cordova de ![icono de enlace externo](../../icons/launch-glyph.svg "icono de enlace externo")](https://github.com/MobileFirst-Platform-Developer-Center/CustomDirectUpdate/tree/release80).  

### Uso de ejemplo
{: #sample-usage }
Siga el archivo README.md del ejemplo para obtener instrucciones.

## Soporte de CDN
{: #cdn_support}

Puede configurar que se sirvan solicitudes de Direct Update desde una CDN (red de entrega de contenidos) en lugar de desde el servidor de MobileFirst.

### Ventajas de utilizar una CDN
{: #advantages-of-using-a-cdn }
Utilizar una CDN en lugar del servidor de MobileFirst para servir solicitudes de Direct Update tiene las siguientes ventajas:

* Elimina las sobrecargas de red del servidor de MobileFirst.
* Aumenta las tasas de transferencia superiores al límite de 250 MB/segundo al servir solicitudes desde un servidor de MobileFirst.
* Asegura una experiencia de Direct Update más uniforme a todos los usuarios independientemente de su ubicación geográfica.

### Requisitos generales
{: #general-requirements }
Para dar servicio a las solicitudes de Direct Update desde una CDN, asegúrese de que la configuración cumple las siguientes condiciones:

* La CDN debe ser un proxy inverso frente al servidor de MobileFirst (o frente a otro proxy inverso, si es necesario).
* Al crear la aplicación desde el entorno de desarrollo, configure el servidor de destino en el host y el puerto de la CDN en lugar del host y del puerto del servidor de MobileFirst. Por ejemplo, al ejecutar el mandato de la CLI de MobileFirst `mfpdev server add`, proporcione el host y el puerto de la CDN.
* En el panel de administración de la CDN, debe marcar las siguientes URL de Direct Update para almacenarlas en la memoria caché para asegurarse de que la CDN pase todas las solicitudes al servidor de MobileFirst, excepto para las solicitudes de Direct Update. Para las solicitudes de Direct Update, la CDN determina si obtuvo el contenido. Si lo ha obtenido, lo devolverá sin ir al servidor de MobileFirst; si no lo ha obtenido, irá al servidor de MobileFirst, obtendrá el archivado de Direct Update (archivo .zip) y lo almacenará para las siguientes solicitudes para dicho URL específico. Para aplicaciones compiladas con la v8.0 de {{site.data.keyword.mobilefoundation_short}}, el URL de Direct Update es: `PROTOCOLO://DOMINIO:PUERTO/VÍA_CONTEXTO/api/directupdate/VERSIÓN/SUMA_COMPROBACIÓN/TIPO`.
El prefijo `PROTOCOLO://DOMINIO:PUERTO/VÍA_CONTEXTO` es el mismo para todas las solicitudes de tiempo de ejecución. Por ejemplo: `http://my.cdn.com:9080/mfp/api/directupdate/0.0.1/742914155/full?appId=com.ibm.DirectUpdateTestApp&clientPlatform=android`

En el ejemplo, hay parámetros de solicitud adicionales que también son parte de la solicitud.

* La CDN debe permitir almacenar en caché los parámetros de solicitud. Dos archivadores de Direct Update podrían diferir únicamente por los parámetros de la solicitud.
* La CDN debe dar soporte a TTL en la respuesta de Direct Update. El soporte es necesario para permitir varias actualizaciones directas para la misma versión.
* La CDN no puede cambiar ni eliminar las cabeceras HTTP que se utilizan en el protocolo de cliente/servidor.

### Ejemplo de configuración de CDN
{: #example-cdn-configuration }
Este ejemplo se basa en la utilización de la configuración de la CDN de Akamai que almacena en caché el archivador de Direct Update. Las tareas siguientes las completa el administrador de red, el administrador de MobileFirst y el administrador de Akamai:

#### Administrador de red
{: #network-administrator }
Cree otro dominio en el DNS para el servidor de MobileFirst. Por ejemplo, si el dominio del servidor es `yourcompany.com`, necesitará crear un dominio adicional como, por ejemplo, `cdn.yourcompany.com`.
En el DNS para el nuevo dominio `cdn.yourcompany.com`, establecerá un `CNAME` al nombre de dominio que Akamai proporcione. Por ejemplo, `yourcompany.com.akamai.net`.

#### Administrador de MobileFirst
{: #mobilefirst-administrator }
Establezca el nuevo dominio `cdn.yourcompany.com` como un URL del servidor de MobileFirst para las aplicaciones de MobileFirst. Por ejemplo, para la tarea del creador de Ant, la propiedad será:
```xml
<property name="wl.server" value="http://cdn.yourcompany.com/${contextPath}/"/>
```
{: codeblock}

#### Administrador de Akamai
{: #akamai-administrator }
1. Debería abrir el gestor de propiedades de Akamai y establecer la propiedad de **nombre de host** con el valor del nuevo dominio.

    ![Establezca el nombre de host de la propiedad en el valor del nuevo dominio](images/direct_update_cdn_3.jpg)

2. En el separador Regla predeterminada, configure el host y el puerto del servidor de MobileFirst originales, y establezca el valor **Custom Forward Host Header** al dominio recién creado.

    ![Establezca el valor Custom Forward Host Header al dominio recién creado](images/direct_update_cdn_4.jpg)

3. En la lista de **Opción de almacenamiento en caché**, seleccionar **No almacenar**.

    ![En la lista de Opción de almacenamiento en caché, seleccione No almacenar](images/direct_update_cdn_5.jpg)

4. En el separador **Configuración de contenido estático**, configurar el criterio coincidente de acuerdo con el URL de Direct Update de la aplicación. Por ejemplo, debería crear una condición que indique `Si la vía de acceso coincide con un URL_de_direct_update`.

    ![Configure los criterios de coincidencia según el URL de Direct Update de la aplicación](images/direct_update_cdn_6.jpg)

5. Configuración del comportamiento de clave de caché para utilizar todos los parámetros de solicitud en la clave de caché (es necesario hacerlo para almacenar en caché archivadores de Direct Update diferentes para distintas aplicaciones o versiones). Por ejemplo, desde la lista **Comportamiento**, seleccione `Incluir todos los parámetros (conservar el orden de la solicitud)`.

    ![Configure el comportamiento de la clave de memoria caché para utilizar todos los parámetros de solicitud en la clave de la memoria caché](images/direct_update_cdn_8.jpg)

6. Debería establecer valores similares a los siguientes para configurar el comportamiento del almacenamiento en caché para el URL de Direct Update y para configurar TTL.

      ![Establezca valores para configurar el comportamiento del almacenamiento en memoria caché](images/direct_update_cdn_7.jpg)

| Campo | Valor |
|:------|:------|
| Opción de almacenamiento en caché | Caché |
| Forzar la reevaluación de objetos obsoletos | Servir obsoleto si no es posible validar |
| Edad máxima | 3 minutos |


## Direct Update seguro
{: #secure-dc }

Inhabilitado de forma predeterminada, Direct Update seguro impide a un atacante de terceros alterar los recursos web que se transmiten desde el servidor de MobileFirst (o desde una CDN (Content Delivery Network, red de entrega de contenido)) a la aplicación cliente.

**Para habilitar la autenticación de Direct Update:**  
Mediante la herramienta de su elección, extraiga la clave pública del almacén de claves del servidor de MobileFirst y conviértala a base64.  
El valor generado se debería utilizar entonces tal como se indica a continuación:

1. Abra una ventana de **línea de mandatos** y vaya a la raíz del proyecto de Cordova.
2. Ejecute el mandato `mfpdev app config` y seleccione la opción **Clave pública de autenticidad de Direct Update**.
3. Proporcione la clave pública y confirme.

Cualquier entrega futura de Direct Update a aplicaciones de cliente estarán protegidas mediante la autenticidad de Direct Update.

Para asegurarse de que Direct Update funcione, debe desplegarse un archivo de almacén de claves definido por el usuario en el servidor de MobileFirst y debe incluirse una copia de la clave pública coincidente en la aplicación de cliente desplegada.

En este tema se describe cómo vincular una clave pública a nuevas aplicaciones de cliente y a aplicaciones de cliente existentes que se hayan actualizado. Para obtener más información sobre cómo configurar el almacén de claves del servidor de MobileFirst, consulte [Configuración del almacén de claves del servidor de MobileFirst ![icono de enlace externo](../../icons/launch-glyph.svg "icono de enlace externo")](http://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/authentication-and-security/configuring-the-mobilefirst-server-keystore/){: new_window}

El servidor incorpora un almacén de claves que sirve para probar Direct Update de forma segura en las fases de desarrollo.

>**Nota:** Después de vincular la clave pública con la aplicación de cliente y de recompilarla, no necesita subirla de nuevo a {{ site.data.keys.mf_server }}. Sin embargo, si con anterioridad publicó la aplicación en el mercado, sin la clave pública, necesitará publicarla de nuevo.

Para fines de desarrollo, se proporciona la siguiente clave pública ficticia predeterminada con el servidor de MobileFirst:

```xml
-----BEGIN PUBLIC KEY-----
MIIDPjCCAiagAwIBAgIEUD3/bjANBgkqhkiG9w0BAQsFADBgMQswCQYDVQQGEwJJTDELMAkGA1UECBMCSUwxETA
PBgNVBAcTCFNoZWZheWltMQwwCgYDVQQKEwNJQk0xEjAQBgNVBAsTCVdvcmtsaWdodDEPMA0GA1UEAxMGV0wgRG
V2MCAXDTEyMDgyOTExMzkyNloYDzQ3NTAwNzI3MTEzOTI2WjBgMQswCQYDVQQGEwJJTDELMAkGA1UECBMCSUwxE
TAPBgNVBAcTCFNoZWZheWltMQwwCgYDVQQKEwNJQk0xEjAQBgNVBAsTCVdvcmtsaWdodDEPMA0GA1UEAxMGV0wg
RGV2MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAzQN3vEB2/of7KAvuvyoIt0T7cjaSTjnOBm0N3+q
zx++dh92KpNJXj/a3o4YbwJXkJ7jU8ykjCYvjXRf0hme+HGhiIVwxJo54iqh76skDS5m7DaseFdndZUJ4p7NFVw
I5ixA36ZArSZ/Pn/ej56/RRjBeRI7AEGXUSGojBUPA6J6DYkwaXQRew9l+Q1kj4dTigyKL5Os0vNFaQyYu+bT2E
vnOixQ0DXm94IqmHZamZKbZLrWcOEfuAsSjKYOdMSM9jkCiHaKcj7fpEZhUxRRs7joKs1Ri4ihs6JeUvMEiG4gK
l9V3FP/Huy0pfkL0F8xMHgaQ4c/lxS/s3PV0OEg+7wIDAQABMA0GCSqGSIb3DQEBCwUAA4IBAQAgEhhqRl2Rgkt
MJeqOCRcT3uyr4XDK3hmuhEaE0nOvLHi61PoLKnDUNryWUicK/W+tUP9jkN5xRckdzG6TJ/HPySmZ7Adr6QRFu+
xcIMY+/S8j4PHLXBjoqgtUMhkt7S2/thN/VA6mwZpw4Ol0Pa2hyT2TkhQoYYkRwYCk9pxmuBCoH/eCWpSxquNny
RwrY25x0YzccXUaMI8L3/3hzq3mW40YIMiEdpiD5HqjUDpzN1funHNQdsxEIMYsWmGAwOdV5slFzyrH+ErUYUFA
pdGIdLtkrhzbqHFwXE0v3dt+lnLf21wRPIqYHaEu+EB/A4dLO6hm+IjBeu/No7H7TBFm
-----END PUBLIC KEY-----
```
{: codeblock}

**Importante:** No utilice la clave pública para fines de producción.

### Generación y despliegue del almacén de claves
{: #generating-and-deploying-the-keystore }
Hay muchas herramientas disponibles para generar certificados y extraer las claves públicas de un almacén de claves. En el siguiente ejemplo se muestran los procedimientos con el programa de utilidad keytool del JDK y openSSL.

1. Extraiga la clave pública del archivo de almacén de claves que se ha desplegado en {{site.data.keys.mf_server}}.  
   >**Nota:** La clave pública debe estar codificada en Base64.

   Por ejemplo, supongamos que el nombre de alias es `mfp-server` y que el archivo del almacén de claves es **keystore.jks**.  
   Para generar un certificado, emita el siguiente mandato:

   ```bash
   keytool -export -alias mfp-server -file certfile.cert
   -keystore keystore.jks -storepass keypassword
   ```
   {: codeblock}

   Se genera un archivo de certificado.  
   Emita el siguiente mandato para extraer la clave pública:

   ```bash
   openssl x509 -inform der -in certfile.cert -pubkey -noout
   ```
   {: codeblock}

   > **Nota:** Con únicamente la herramienta de claves no es posible extraer claves públicas en formato Base64.

2. Siga uno de los siguientes procedimientos:
    * Copie el texto resultante, sin los marcadores `BEGIN PUBLIC KEY` y `END PUBLIC KEY` en el archivo de propiedades mfpclient de la aplicación, inmediatamente después de `wlSecureDirectUpdatePublicKey`.
    * Desde el indicador de mandatos, emita el siguiente mandato:
`mfpdev app config direct_update_authenticity_public_key <public_key>`

    Para `<public_key>`, pegue el texto que se obtiene del Paso 1, sin los marcadores `BEGIN PUBLIC KEY` ni `END PUBLIC KEY`.

3. Ejecute el mandato cordova build para guardar la clave pública en la aplicación.
