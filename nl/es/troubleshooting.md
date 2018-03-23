---

copyright:
  years: 2018
lastupdated: "2018-02-12"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}

# Resolución de problemas
{: #troubleshooting}

Para aislar y resolver problemas con los productos de {{site.data.keyword.IBM_notm}}, puede utilizar la información de resolución de problemas. Esta información contiene instrucciones para utilizar los recursos de determinación de problemas que se proporcionan con los productos de {{site.data.keyword.IBM_notm}}, incluido {{site.data.keyword.mobilefoundation_short}}.
{: shortdesc}

## Técnicas para la resolución de problemas
{: #ts_overview}

La *resolución de problemas* consiste en un enfoque sistemático para solucionar un problema. El objetivo de la resolución de problemas es determinar por qué algo no funciona como estaba previsto y cómo resolver el problema. Algunas técnicas comunes pueden ayudar con la tarea de resolución de problemas.

El primer paso del proceso de resolución de problemas es describir el problema completamente. Las descripciones de problemas le ayudan a usted y al representante del servicio de asistencia técnica de {{site.data.keyword.IBM_notm}} a saber por dónde empezar para encontrar la causa del problema. En este paso, debe plantearse algunas cuestiones básicas:

- ¿Cuáles son los síntomas del problema?
- ¿Dónde se produce el problema?
- ¿Cuándo se produce el problema?
- ¿Bajo qué circunstancias se produce el problema?
- ¿Se puede reproducir el problema?

Las respuestas a estas preguntas suelen llevar a una buena descripción del problema, lo que puede llevar, a su vez, a
resolverlo.

### ¿Cuáles son los síntomas del problema?

Cuando se empieza a describir un problema, la pregunta más obvia es: "¿Cuál es el problema?" Esta pregunta puede resultar demasiado directa, sin embargo, puede subdividirse en varias preguntas más focalizadas que crean una imagen más descriptiva del problema. Estas preguntas pueden incluir:

- ¿Quién o qué notifica el problema?
- ¿Qué códigos y mensajes de error se emiten?
- ¿De qué manera falla el sistema? Por ejemplo, ¿se produce un bucle, el sistema se cuelga, se produce una colisión, una disminución del rendimiento o un resultado incorrecto?

### ¿Dónde se produce el problema?

Determinar dónde se origina el problema no siempre es fácil, pero es uno de los pasos más importantes para la resolución de un problema. Pueden existir muchas capas de tecnología entre los componentes de informe y error. Redes, discos y controladores son sólo algunos de los componentes a tener en cuenta cuando se investiga un problema.

Las siguientes preguntas le ayudarán a determinar dónde se produce el problema para
aislar la capa del problema:

- ¿El problema es específico de una plataforma o sistema operativo o, por el contrario, es común en varias plataformas o sistemas operativos?
- ¿Se da soporte al entorno y la configuración actuales?
- ¿El problema afecta a todos los usuarios?
- (Para instalaciones en varios sitios). ¿Todos los sitios tienen este problema?

Si una capa
informa del problema, el problema no tiene por qué haberse generado necesariamente en esa
capa. Parte del proceso de identificación del lugar donde se origina un problema consiste en comprender el entorno en el que se produce. Tómese un tiempo para describir por completo el entorno del problema, incluidos el sistema operativo y la versión y toda la información de software, hardware y versiones. Confirme que está trabajando en un entorno con una configuración soportada; muchos
problemas pueden rastrearse hasta niveles incompatibles de software que no están
concebidos para funcionar juntos o no se han probado a fondo conjuntamente.

### ¿Cuándo se produce el problema?

Desarrolle una línea temporal detallada de sucesos que lleven hasta el error, especialmente en los casos de una única aparición. Puede desarrollar con mayor facilidad una cronología si recorre el trabajo de forma retrospectiva: empiece en el momento en que se ha informado acerca del error (de un modo tan preciso como sea posible, incluso llegando a los milisegundos) y trabaje hacia adelante a través de la información y los registros disponibles. Por lo general, sólo suele ser necesario llegar hasta el primer suceso sospechoso que encuentra en un registro de diagnóstico.

Para desarrollar una línea temporal detallada de los sucesos, responda estas preguntas:

- ¿Se produce el problema sólo en ciertos momentos del día o de la noche?
- ¿Con qué frecuencia se produce el problema?
- ¿Qué secuencia de sucesos antecede al momento en que se informa del problema?
- ¿El problema se produce tras un cambio de entorno, como al actualizar
o instalar software o hardware?

La respuesta a este tipo de preguntas puede proporcionar un marco de referencia en el que investigar el problema.

### ¿Bajo qué circunstancias se produce el problema?

Es importante saber
qué sistemas y aplicaciones están en ejecución cuando se produce el problema
para resolverlo. Estas preguntas sobre el entorno le ayudarán a identificar la causa raíz del problema:

- ¿El problema siempre se produce al realizar la misma tarea?
- ¿Tiene que darse una secuencia de sucesos determinada para que se produzca el problema?
- ¿Hay otras aplicaciones que den error al mismo tiempo?

Responder a estos tipos de preguntas puede ayudarle a describir el entorno en el que se produce el problema y a correlacionar las dependencias. Recuerde que aunque varios problemas ocurran más o menos a la vez, no tienen por qué estar relacionados.

### ¿Se puede reproducir el problema?

Desde el punto de vista de la resolución de problemas, el problema ideal es el que se puede reproducir. Por lo general, cuando se puede reproducir un problema, se dispone de un conjunto más grande de herramientas o procedimientos que facilitan la investigación. Por lo tanto, los problemas que puede reproducir suelen ser más fáciles de depurar y resolver.

Sin embargo, los problemas que se pueden reproducir pueden tener una desventaja: si el problema tiene un impacto significativo en la empresa, no será deseable que vuelva a producirse. Si es posible, vuelva a crear el problema en un entorno de prueba o desarrollo, que generalmente ofrece más flexibilidad y control durante la investigación.

- ¿Se puede recrear el problema en un sistema de prueba?
- ¿Hay varios usuarios o aplicaciones que encuentren el mismo tipo de problema?
- ¿El problema puede reproducirse ejecutando un único mandato, un conjunto de mandatos o una aplicación específica?


##  Limitaciones conocidas
{: #knownlimitations_mfp}

* La IU del servicio {{site.data.keyword.mobilefoundation_short}} no utiliza el patrón específico del entorno local seleccionado por el usuario para mostrar números.

## Obtención de ayuda y soporte para Mobile Foundation
{: #getting_help_mobilefoundation}

Si tiene problemas o preguntas sobre el uso de {{site.data.keyword.mobilefoundation_short}}, puede obtener ayuda buscando información o formulando preguntas en un foro. También puede abrir una incidencia de soporte.

Al utilizar los foros para formular una pregunta, etiquete la pregunta de forma que la puedan ver los equipos de desarrollo de IBM {{site.data.keyword.Bluemix_notm}}.

Si tiene preguntas técnicas sobre el desarrollo o el despliegue de una app con {{site.data.keyword.mobilefoundation_short}}, publíquelas en [Stack Overflow ![icono de enlace externo](../../icons/launch-glyph.svg "icono de enlace externo")](http://stackoverflow.com/search?q=ibm-mobilefirst+bluemix){:new_window} y etiquete las preguntas con `bluemix` e `ibm-mobilefirst`.

Para formular preguntas sobre el servicio y obtener instrucciones de iniciación, utilice el foro [IBM developerWorks dW Answers ![icono de enlace externo](../../icons/launch-glyph.svg "icono de enlace externo")](https://developer.ibm.com/answers/topics/mobilefirst/?smartspace=bluemix){:new_window}. Incluya las etiquetas `bluemix` y `mobilefirst`.

Para obtener información sobre cómo abrir una incidencia de soporte de IBM, o sobre los niveles de soporte y la gravedad de las incidencias, consulte [Cómo obtener soporte ![icono de enlace externo](../../icons/launch-glyph.svg "icono de enlace externo")](https://console.bluemix.net/docs/get-support/getstarttssup.html#typesofsupport  ){: new_window}.
