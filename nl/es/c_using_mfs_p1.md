---

copyright:
  years: 2016, 2018
lastupdated:  "2018-02-14"

---

#	Utilización del plan Developer
{: #using_mobilefoundation_p1}

Después de crear la instancia de servicio de {{site.data.keyword.mobilefoundation_short}} mediante el plan Developer, acceda a la página Visión general en {{site.data.keyword.Bluemix_notm}}. En esta página, se proporcionan guías de aprendizaje y vídeos para ayudarle a aprender a utilizar el servicio.

## Inicio del servidor de MobileFirst
{: #start_mobilefoundation_p1}
* Para iniciar {{site.data.keyword.mfserver_short_notm}} con los valores predeterminados, pulse **Iniciar servidor básico**.

  Esta selección suministra un {{site.data.keyword.mfserver_long_notm}} con la configuración siguiente:
  *	1 GB de memoria. Este tamaño es suficiente para realizar actividades de desarrollo, de pruebas no muy exigentes y cargas de trabajo de producción a pequeña escala.

  *	El `nombre de usuario` y la `contraseña` se generan de forma automática. Tiene acceso a ellos cuando el servidor está en ejecución.

  Se inicia el proceso de suministro. Este proceso dura unos 10 minutos y un mensaje indica el progreso de la operación. Una vez finalizado, se muestra un panel de control en el que se puede ver lo siguiente:
    *	El estado del servidor que se ejecuta (estado, tamaño).

    *	La ruta creada para el servidor. Utilice esta ruta en su aplicación móvil para conectarse a {{site.data.keyword.mfserver_short_notm}}.

    *	Su `nombre de usuario` y `contraseña` personal para acceder a la {{site.data.keyword.mfp_oc_short_notm}}. La `contraseña` está oculta. Pulse el icono **Mostrar contraseña** para visualizarla.

*	Pulse **Iniciar consola** para iniciar la {{site.data.keyword.mfp_oc_short_notm}}.

Con la consola, puede gestionar sus apps móviles y dispositivos móviles, utilizar su servidor como programa de fondo móvil, enviar notificaciones push, etc.

##  Adición del servicio de Mobile Analytics
{: #adding_analytics_server_dev}

 Ahora puede supervisar su aplicación móvil en el servidor de {{site.data.keyword.mobilefirst}} añadiendo un servicio de Mobile Analytics a la instancia de servicio de {{site.data.keyword.mobilefoundation_short}}. El plan Developer crea el servicio de Mobile Analytics en un grupo de contenedores con un nodo único que tiene 1 GB de memoria.

* Pulse **Añadir Analytics** para añadir el servicio de Mobile Analytics a la instancia de servicio de {{site.data.keyword.mobilefoundation_short}}.

  Se inicia el proceso de suministro. Este proceso dura unos 10 minutos y un mensaje indica el progreso de la operación.  

* Inicie la consola del servicio de Mobile Analytics desde {{site.data.keyword.mfp_oc_short_notm}}.

* El inicio de sesión único está habilitado entre {{site.data.keyword.mfserver_short_notm}} y el servicio de Mobile Analytics. El servicio de Mobile Analytics está configurado con las mismas claves de LTPA y credenciales de usuario que {{site.data.keyword.mfserver_short_notm}}.Puede utilizar el mismo `nombre_usuario` y `contraseña` para iniciar sesión en la consola de Mobile Analytics como se solía iniciar sesión en {{site.data.keyword.mfp_oc_short_notm}}.

Para obtener más información sobre Mobile Analytics, puede consultar [MobileFirst Foundation Operational Analytics ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/analytics/){: new_window}.

> **Nota:** Al suprimir la instancia de servicio de {{site.data.keyword.mobilefoundation_short}} o volver a crear el {{site.data.keyword.mfserver_short_notm}}, se eliminará la instancia de servicio de Mobile Analytics.

##  Supresión del servicio de Mobile Analytics
{: #deleting_analytics_server_dev}

Ahora puede suprimir el servicio de Mobile Analytics que se ha añadido a la instancia de servicio de {{site.data.keyword.mobilefoundation_short}}, desde el panel de control de servicio de {{site.data.keyword.mobilefoundation_short}}.

* Pulse **Suprimir Analytics** para suprimir el servicio de Mobile Analytics que se ha añadido a la instancia de servicio de {{site.data.keyword.mobilefoundation_short}}.

 Al pulsar **Suprimir Analytics**, se suprime la instancia de servidor de análisis. El proceso de supresión de instancias de análisis tarda unos 10 minutos. Puede renovar la pantalla para ver el estado actualizado. Al suprimir la instancia de análisis, se vuelve a habilitar el botón **Añadir Analytics**. Si decide volver a añadir el servicio de Mobile Analytics, pulse este botón.


## Recreación del servidor de MobileFirst
{: #recreate_mobilefoundation_p1}

*	Pulse **Recrear** para volver a crear el servidor.

* Esta acción detiene el servidor existente y suprime los datos. Todos los datos del servidor móvil se pierden. Se crea una nueva instancia con una versión actualizada, si está disponible. Esta acción tarda unos minutos en completarse.

##	Ajuste de la configuración avanzada
{: #using_mfs_advanced_p1}

Utilice la opción **Iniciar servidor con configuración avanzada** en la página `Visión general` para crear el servidor con valores avanzados o personalizados. También puede actualizar los valores del servidor para personalizar su configuración desde el separador **Configuración**. {{site.data.keyword.mobilefoundation_short}} le ofrece acceso a algunos valores avanzados.

*	Desde el separador **Topología**, puede seleccionar el tamaño del servidor y el número de instancias que necesita. El valor predeterminado del servidor es de 1 GB, que es suficiente para el desarrollo y la realización de pruebas no muy exigentes.

  - Seleccione el tamaño correcto para su servidor en función de sus necesidades.

* **Nodos** muestra el número de nodos que se han creado. Este campo no es editable en {{site.data.keyword.mobilefoundation_short}}: Developer. El número de nodos <!--in your {{site.data.keyword.IBM_notm}} container group--> toma el valor predeterminado de **1** en el plan Developer.

Consulte la documentación de [{{site.data.keyword.mobilefoundation_long}} ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://www.ibm.com/support/knowledgecenter/SSHS8R_8.0.0/wl_welcome.html){: new_window} para obtener más detalles.
