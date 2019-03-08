---

copyright:
  years: 2019
lastupdated: "2018-12-21"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}
{:java: .ph data-hd-programlang='java'}
{:ruby: .ph data-hd-programlang='ruby'}
{:c#: .ph data-hd-programlang='c#'}
{:objectc: .ph data-hd-programlang='Objective C'}
{:python: .ph data-hd-programlang='python'}
{:javascript: .ph data-hd-programlang='javascript'}
{:php: .ph data-hd-programlang='PHP'}
{:swift: .ph data-hd-programlang='swift'}

# API de SDK de cliente Javascript
{: #javascript_client_sdk_api}

## API para Cordova/aplicaciones web (Javascript)

En la tabla siguiente se muestran las funciones que puede llevar a cabo en las aplicaciones de JavaScript y el método API correspondiente.

| Función | Descripción |
|----------|-------------|
| `WL.Client`, `WL.App` | Inicialización y recarga de una aplicación, globalización de textos de aplicación | 
| `WLAuthorizationManager` | Obtener el ID de cliente y la cabecera de autorización |
| `WL.Logger` | Impresión de mensajes de registro en el registro del entorno |
| `WL.NativePage` | Cambio de la pantalla basada en web que se visualiza con una página escrita de forma nativa |
| `WLResourceRequest` | Enviar solicitudes a recursos protegidos y no protegidos | 
| `WL.JSONStore` | API del lado del cliente que proporciona un sistema de almacenamiento ligero y orientado a documentos | 

## Información adicional
{: #additional-information }
### El objeto Opciones
{: #the-optional-object }
El objeto `opciones` contiene propiedades que son comunes en todos los métodos. Se utiliza en todas las llamadas asíncronas a {{ site.data.keys.mf_server }}.

A veces, lo mejoran propiedades que solo se aplican a métodos específicos. Estas propiedades adicionales se detallan como parte de la descripción de métodos específicos.

Las propiedades comunes del objeto opciones son las siguientes:

```javascript
options = {
    onSuccess: successHandler(response),
    onFailure: failureHandlder(response),
    invocationContext: invocation-context
};
```
{: code}
El significado de cada propiedad es el siguiente:

| Propiedad | Descripción |
|----------|-------------|
| `onSuccess` | Opcional. La función que se debe invocar al finalizar correctamente la llamada asíncrona. La sintaxis de la función `onSuccess` es: `success-handler-function(response)` donde `response` es un objeto que contiene al menos la siguiente propiedad: {::nomarkdown}<ul><li><b>invocationContext</b> - El objeto <code>invocationContext</code> que se ha pasado originalmente al objeto {{ site.data.keys.mf_server }} en el objeto <code>options</code> o <code>undefined</code> si no se ha pasado ningún objeto <code>invocationContext</code>.</li><li><b>status</b> - El estado de la respuesta HTTP</li></ul>{:/} **Nota:** En los métodos para los que el objeto `response` contiene propiedades adicionales, dichas propiedades se detallan como parte de la descripción del método específico. |
| `onFailure` | Opcional. La función que se invocará cuando la llamada asíncrona falle. Estos errores incluyen errores del lado del servidor y errores del lado del cliente que se han producido durante las llamadas asíncronas como, por ejemplo, una anomalía de conexión del servidor o llamadas de tiempo de espera excedido. **Nota:** No se llama a la función para los errores del lado del cliente que detienen la ejecución lanzando una excepción. La sintaxis de la función onFailure es: `failure-handler-function(response)` donde `response` es un objeto que contiene las propiedades siguientes: {::nomarkdown}<ul><li><b>invocationContext</b> - El objeto <code>invocationContext</code> que se ha pasado originalmente al objeto {{ site.data.keys.mf_server }} en el objeto <code>options</code> o <code>undefined</code> si no se ha pasado ningún objeto <code>invocationContext</code>.</li><li><b>errorCode</b> - Una serie de código de error. Todos los códigos de error que se pueden devolver se definen como constantes en el objeto <code>WL.ErrorCode</code> del archivo <b>worklight.js</b>.</li><li><b>errorMsg</b> - Un mensaje de error que proporciona {{ site.data.keys.mf_server }}. Este mensaje es para uso exclusivo del desarrollador y no debe mostrarse al usuario. No se traducirá al idioma del usuario.</li><li><b>status</b> - El estado de la respuesta HTTP</li></li></ul>{:/} **Nota:** En los métodos para los que el objeto `response` contiene propiedades adicionales, dichas propiedades se detallan como parte de la descripción del método específico. |
| `invocationContext` | Opcional. Un objeto que se devuelve a los controladores de error y acierto. El objeto `invocationContext` se utiliza para conservar el contexto del servicio de llamada asíncrono tras la devolución desde el servicio. Por ejemplo, el método `invokeProcedure` puede llamarse sucesivamente utilizando el mismo controlador de aciertos. El controlador de aciertos debe poder identificar qué llamada a invokeProcedure se está controlando. Una solución es implementar el objeto `invocationContext` como entero e incrementar el valor en uno para cada llamada de `invokeProcedure`. Cuando invoca al controlador de acierto, la infraestructura de {{ site.data.keys.product_adj }} lo pasa al objeto `invocationContext` del objeto opciones asociado con el método `invokeProcedure`. El valor del objeto `invocationContext` se puede utilizar para identificar la llamada a `invokeProcedure` con la que están asociados los resultados que se controlan. | 

## El objeto WL.ClientMessages
{: #the-wlclientmessages-object }
Puede ver una lista de los mensajes del sistema almacenados en el objeto `WL.ClientMessages` y habilitar la conversión de dichos mensajes del sistema.

Debe sustituir los mensajes del sistema en un nivel de JavaScript global porque algunas partes del código se ejecutan solo cuando la aplicación se haya inicializado correctamente.
{: note}

El objeto `WL.ClientMessages` es un objeto que almacena los mensajes del sistema que están definidos en el archivo **worklight/messages/messages.json**. El archivo se encuentra en la carpeta de entorno de los proyectos que ha generado con {{ site.data.keys.product }}. Para habilitar la conversión de un mensaje del sistema, debe alterar temporalmente el valor del mensaje en el objeto `WL.ClientMessages`, como se indica en el ejemplo de código siguiente:

```javascript
WL.ClientMessages.invalidUsernamePassword="The custom user name and password are not valid";
```
{: code}


* [JavaScript API Reference](http://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/api/client-side-api/javascript/client/#javascript-api-reference)
* [Referencia de API de Objective-C (para Cordova)](http://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/api/client-side-api/javascript/client/#objective-c-api-reference-for-cordova)
{: note}
