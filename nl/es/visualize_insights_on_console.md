---

copyright:
  years: 2018
lastupdated: "2018-11-22"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}

# Visualización de conocimientos en la consola
{: #visualize_insights_on_console}

Desde la consola de análisis de MobileFirst puede ver y configurar los informes de Herramientas de análisis, gestionar alertas y visualizar registros de cliente. Inicie la consola de analíticas desde la consola de operaciones de Mobile Foundation pulsando en **Consola de analíticas** desde la navegación de la izquierda.

Se iniciará la consola de análisis y aparecerá el panel de control predeterminado. Si una aplicación cliente ya ha enviado registros y datos analíticos al servidor, se crearán y mostrarán los informes relevantes. Podrá revisar los datos de análisis recopilados desde el panel de control. Los datos de análisis pueden estar relacionados con bloqueos de aplicaciones, sesiones de aplicación y el tiempo de proceso del servidor. Además, podrá crear diagramas personalizados y gestionar alertas.

Además de una vista rápida de los análisis móviles, la función de análisis incluye la capacidad de realizar una búsqueda en bruto en los registros de cliente, los datos de bloqueo de los clientes capturados y cualquier dato adicional que proporcione de forma explícita a través de llamadas a funciones de API que proporcionan el servicio de Mobile Analytics.

## Supervisión de datos de app
{: #monitoring_app_data}

El análisis móvil proporciona funciones de supervisión y análisis para las aplicaciones móviles. Puede registrar registros de aplicación y supervisar datos con el SDK de cliente de Mobile Analytics. Los desarrolladores pueden controlar cuándo enviar estos datos al servicio de análisis móvil. Cuando los datos se entregan a Mobile Analytics, puede utilizar la consola de Mobile Analytics para obtener información de análisis sobre las aplicaciones móviles, los dispositivos y los registros de aplicación.

Información que se puede derivar:

**Métricas predefinidas**

Con las métricas predefinidas puede responder a preguntas como:
* ¿Cuántos usuarios nuevos tengo?
* ¿Cuántas personas utilizan mi aplicación de forma activa?
* ¿Con qué frecuencia utilizan mi aplicación?
* ¿A qué hora del día utilizan mi aplicación?
* ¿Qué modelos de dispositivo prefieren mis usuarios?
* ¿Cuándo debería anular el soporte para los sistemas operativos existentes?
* ¿Qué aplicaciones tienen problemas de rendimiento?

**Sucesos personalizados**

Mediante la adición de sucesos personalizados, puede responder a preguntas como:
* ¿Qué características se utilizan más y cuáles menos?
* ¿Desde dónde entran y salen los usuarios de mi app?
* ¿Qué actividades ven más los usuarios?
* ¿Completan los usuarios los flujos de trabajo de la app (por ejemplo, canalizaciones de conversión)?

## Informes que se pueden visualizar: Usuarios
{: #reports_visualized_users}

En este informe se muestra un gráfico que proporciona el número de usuarios activos que han utilizado la aplicación en un intervalo de fechas especificado. El informe también muestra el número de usuarios nuevos, que son usuarios exclusivos que utilizan la app por primera vez, en el intervalo de fechas especificado.
Los gráficos se pueden filtrar según el nombre de la app, el sistema operativo o las versiones del sistema operativo.

## Informes que se pueden visualizar: Sesiones
{: #reports_visualized_sessions}

En este informe aparece un gráfico que muestra las sesiones de app en el intervalo de fechas especificado. Una sesión se graba cuando una app se lleva al primer plano de un dispositivo. Los gráficos se pueden filtrar según el nombre de la app, el sistema operativo o las versiones del sistema operativo.

## Informes que se pueden visualizar: Solicitudes de red
{: #reports_visualized_network_requests}

Visualice datos de solicitud de red para las aplicaciones en la consola de Mobile Analytics.

Dispone de datos para las siguientes medidas:

**Tiempo de ida y vuelta** - define el período de tiempo, en milisegundos, que la aplicación tarda en realizar solicitudes de red.
**Recuento de solicitudes** - muestra la frecuencia con la que la app realiza solicitudes. Los datos también se muestran como promedio.
Los gráficos se pueden filtrar según el nombre de la app, el sistema operativo o las versiones del sistema operativo.

## Informes que se pueden visualizar: Bloqueos
{: #reports_visualized_crashes}

Puede ver información sobre los bloqueos de aplicaciones en la consola de Mobile Analytics para supervisar mejor y resolver los problemas de las aplicaciones.

En la página Bloqueos, la tabla **Visión general de bloqueos** muestra las columnas de datos siguientes:

**Aplicación**: nombre de aplicación<br/>
**Bloqueos**: número total de bloqueos de dicha aplicación<br/>
**Usos totales**: número total de veces que un usuario abre y cierra la aplicación<br/>
**Frecuencia de bloqueos**: porcentaje de bloqueos por uso<br/>
Puede ver rápidamente información sobre los bloqueos de aplicación en la tabla Bloqueos. El gráfico de barras Bloqueos muestra un histograma de bloqueos a lo largo del tiempo.<br/>

Puede visualizar los datos de bloqueos de dos formas:

1.  Visualizar frecuencia de bloqueos: frecuencia de bloqueos con el tiempo
2.  Visualizar bloqueos totales: bloqueos totales con el tiempo

## Informes que se pueden visualizar: Resolución de problemas
{: #reports_visualized_troubleshooting}

La página **Resolución de problemas** de la consola Mobile Analytics ofrece una vista detallada de los bloqueos de aplicación, utilizando la tabla **Resumen de bloqueos**.

La tabla **Resumen de bloqueos** se puede ordenar e incluye las siguientes columnas de datos:

* Bloqueos
* Dispositivos
* Último bloqueo
* Aplicación
* Sistema operativo
* Mensaje

Pulse el icono +, situado junto a cualquier entrada, para ver la tabla **Detalles de bloqueo**, que incluye las siguientes columnas:

* Hora del bloqueo
* Versión de aplicación
* Versión de sistema operativo
* Modelo de dispositivo
* ID de dispositivo
* Descarga: Enlace para descargar los registros que han provocado el bloqueo

Expanda cualquier entrada de la tabla **Detalles de bloqueo** para obtener información más detallada (por ejemplo, un seguimiento de pila).

Los datos de la tabla **Resumen de bloqueos** se rellenan consultando los registros de aplicaciones de nivel muy grave. Si la aplicación no recopila registros de aplicaciones muy graves, no habrá datos disponibles.
{: note}


## Informes que se pueden visualizar: Comentarios de usuarios
{: #reports_visualized_userfeedback}

Los comentarios de usuarios proporcionan un análisis de comentarios integrados en la app utilizando Mobile Analytics.
Con esta característica de Mobile Analytics:
* Los **usuarios y responsables de las pruebas** pueden registrar y enviar comentarios e informes de errores desde la app a medida que ejecutan y utilizan la aplicación.
* Los **propietarios de apps** reciben información detallada sobre la experiencia del usuario con la aplicación con el contexto de los comentarios de los usuarios.
* Los **desarrolladores** reciben contextos precisos de aplicación para diagnosticar y solucionar errores o deficiencias en la funcionalidad.

### Habilitación de comentarios de usuario
{: #enable_user_feedback}

Complete los pasos siguientes para habilitar la aplicación móvil y capturar comentarios de los usuarios.

#### Instrumentar la app
{: #instrument_app}

* Instrumente la app móvil para introducir la modalidad de comentarios. Llame a la API `Analytics.triggerFeedbackMode();` para invocar la modalidad de comentarios. <!--For more information, refer to the documentation [here](instrument_an_app.html)-->.
* La API se puede llamar en cualquier suceso de aplicación como, por ejemplo, botones, acciones de menú o gestos.

#### Recibir comentarios de los usuarios
{: #receive_feedback}

* Los usuarios y los encargados de las pruebas de la app pueden cambiar a la modalidad de comentarios activando la acción de la aplicación que se ha instrumentado para recibir comentarios.
* En la modalidad de comentarios es posible recopilar comentarios contextuales y capturas de pantalla que después se podrán enviar a Mobile Analytics.

#### Analizar los comentarios de los usuarios
{: #analyze_feedback}

* Mobile Analytics recibe y consolida los comentarios enviados desde aplicaciones móviles.
* Inicie sesión en la consola de Mobile Analytics y seleccione la opción **Comentarios de usuarios** para ver los comentarios.
* Un propietario de la aplicación puede revisar los comentarios, añadir comentarios adicionales y etiquetar los comentarios con un estado de revisión. Los comentarios podrían ser acciones planificadas, como enlaces a temas de Git creados para trabajar con la información o los comentarios podrían constituir la declaración de las razones por las que no es necesaria ninguna acción en la respuesta.
* El estado de revisión se puede utilizar para gestionar de forma eficiente los comentarios clasificándolos en una de las distintas opciones de estado de revisión.

