---

copyright:
  years: 2016, 2018
lastupdated:  "2018-03-05"

---

#	Utilización del plan Professional Per Device
{: #using_mobilefoundation_p5}

Con el plan Professional Per Device, los usuarios pueden crear, probar y ejecutar aplicaciones móviles en producción, independientemente del número de usuarios o dispositivos móviles. Este plan da soporte a grandes despliegues y alta disponibilidad.
Tras crear la instancia del servicio {{site.data.keyword.mobilefoundation_short}}: Professional Per Device, consulte el procedimiento siguiente para iniciarse en el uso del servicio.

## Requisitos previos
{: #prerequisites_p5}

Tenga en cuenta lo siguiente antes de configurar la instancia del servicio {{site.data.keyword.mobilefoundation_short}}: Professional Per Device.
* {{site.data.keyword.mobilefoundation_short}}: Professional Per Device solo se admite con planes de {{site.data.keyword.Db2_on_Cloud_short}} de {{site.data.keyword.Bluemix_notm}}.

* Deberá tener acceso a las credenciales de la instancia de servicio {{site.data.keyword.Db2_on_Cloud_short}} antes de poder configurar los valores de su instancia de servicio {{site.data.keyword.mobilefoundation_short}}.

> **Nota**: La instancia de servicio {{site.data.keyword.Db2_on_Cloud_short}} puede existir en cualquier `Espacio` dentro de la {{site.data.keyword.Bluemix_notm}} `Organización` o de cualquier otra `Organización` a la que tenga acceso. Asegúrese de que dispone de los permisos para acceder al `Espacio` donde existe la instancia del servicio {{site.data.keyword.Db2_on_Cloud_short}}.


## Adición de la conexión de base de datos
{: #configure_dashdb_p5}

###  Primeros pasos
{: #firststeps_p5}

Tras crear la instancia del servicio {{site.data.keyword.mobilefoundation_short}}: Professional Per Device, siga el procedimiento para iniciarse en el uso del servicio.

### Configuración de la conexión con la instancia de servicio Db2 on Cloud
{: #connect_dashdb_p5}

Una vez que se haya creado la instancia de servicio de {{site.data.keyword.mobilefoundation_short}}: Professional Per Device, podrá ver la página *Visión general*, donde deberá especificar la información de conexión de la instancia de servicio de {{site.data.keyword.Db2_on_Cloud_short}}, a la que se debe conectar la instancia de servicio de {{site.data.keyword.mobilefoundation_short}}.

También puede crear una nueva instancia de servicio de {{site.data.keyword.Db2_on_Cloud_short}}, si aún no tiene ninguna existente.

Siga estos pasos para crear una nueva instancia de servicio de Db2 on Cloud:

1. En la página *Visión general*, seleccione la sección **Crear nuevo servicio**.

+ Seleccione `Sí` en la opción **Configuración de alta disponibilidad**, si desea una instancia de servicio altamente disponible de {{site.data.keyword.Db2_on_Cloud_short}}.

+ Revise los detalles del plan y pulse **Crear**.

Se ha creado una nueva instancia de servicio de {{site.data.keyword.Db2_on_Cloud_short}}, que proporciona una instancia dedicada de {{site.data.keyword.Db2_on_Cloud_short}} con RAM de 8 GB y 2 vCPUs, y 500 GB de almacenamiento.

Siga estos pasos para conectarse a una instancia de servicio existente de {{site.data.keyword.Db2_on_Cloud_short}} o a la instancia de servicio de {{site.data.keyword.Db2_on_Cloud_short}} que acaba de crear:

1. Seleccione la `Organización` de {{site.data.keyword.Bluemix_notm}} donde existe la instancia del servicio {{site.data.keyword.Db2_on_Cloud_short}}.

+ Seleccione el `Espacio` de {{site.data.keyword.Bluemix_notm}} donde existe la instancia del servicio {{site.data.keyword.Db2_on_Cloud_short}}, en la lista de espacios disponibles en la `Organización` seleccionada.   
> **Nota:** Si no ve la `Organización` y el `Espacio` donde existe su instancia del servicio {{site.data.keyword.Db2_on_Cloud_short}} consulte si es miembro de la `Organización` y del `Espacio`. Se requiere el rol de *Desarrollador* para acceder a la organización y al espacio, ya que el servicio de {{site.data.keyword.mobilefoundation_short}} accede a las credenciales desde el servicio de {{site.data.keyword.Db2_on_Cloud_short}}.

+ Seleccione el `Nombre de servicio` y las `Credenciales` de {{site.data.keyword.Db2_on_Cloud_short}} para conectarse con la instancia  de servicio {{site.data.keyword.Db2_on_Cloud_short}} existente.

+  Pruebe la conexión a la instancia de servicio especificada de {{site.data.keyword.Db2_on_Cloud_short}}.

+  Pulse **Añadir**. Esta acción crea las tablas necesarias en la instancia de servicio de base de datos de {{site.data.keyword.Db2_on_Cloud_short}} configurada.

En varios segundos, puede acceder a la página `Visión general`, que le ofrece guías de aprendizaje y vídeos para ayudarle a aprender a utilizar el servicio {{site.data.keyword.mobilefoundation_short}}.

> **Nota**: no puede cambiar la instancia de servicio {{site.data.keyword.Db2_on_Cloud_short}} que está configurada para que la utilice la instancia del servicio {{site.data.keyword.mobilefoundation_short}}. No obstante, puede utilizar la misma instancia de servicio {{site.data.keyword.Db2_on_Cloud_short}} en varias instancias de servicio {{site.data.keyword.mobilefoundation_short}}, ya que cada instancia de servicio {{site.data.keyword.mobilefoundation_short}} crea su propio esquema en la instancia de servicio {{site.data.keyword.Db2_on_Cloud_short}} seleccionada.

## Inicio del servidor de MobileFirst
{: #start_mobilefoundation_p5}

* Para iniciar {{site.data.keyword.mfserver_short_notm}} con los valores predeterminados, pulse **Iniciar servidor básico**.

* Esta selección suministra un {{site.data.keyword.mfserver_long_notm}} con la configuración siguiente:
    -  2 nodos con 1 GB de memoria cada uno. Este tamaño es correcto para realizar actividades de desarrollo, de pruebas no muy exigentes y cargas de trabajo de producción a pequeña escala.

    -	El `nombre de usuario` y la `contraseña` se generan de forma automática. Tiene acceso a ellos cuando el servidor está en ejecución.

Se inicia el proceso de suministro del servidor. Este proceso dura unos 10 minutos y un mensaje indica el progreso de la operación. Una vez finalizado, se muestra un panel de control en el que se puede ver lo siguiente:

  -	El estado del servidor que se ejecuta (estado, tamaño).

  -	Se ha creado la ruta para el servidor. Utilice esta ruta en su aplicación móvil para conectarse a {{site.data.keyword.mfserver_short_notm}}.

  -	Su `nombre de usuario` y `contraseña` personal para acceder a la {{site.data.keyword.mfp_oc_short_notm}}. La `contraseña` está oculta. Pulse el icono **Mostrar contraseña** para visualizarla.

*	Pulse **Iniciar consola** para abrir la {{site.data.keyword.mfp_oc_short_notm}}.


Con la consola, puede gestionar sus apps móviles, adaptadores y dispositivos móviles, utilizar su servidor como programa de fondo móvil, enviar notificaciones push, etc.


##  Adición del servicio de Mobile Analytics
{: #adding_analytics_server_p5}

 Ahora puede supervisar su aplicación móvil en el servidor de {{site.data.keyword.mobilefirst}} añadiendo una instancia de servicio de Mobile Analytics a la instancia de servicio de {{site.data.keyword.mobilefoundation_short}}.

 <!--The Professional plan creates the Mobile Analytics service in a container group, the user can customize the configuration by selecting the number of container nodes in the container group.

 Users can also attach volumes to the containers to persist data. The volume once selected cannot be changed. 20 GB is the default file share space available to the user. If the user needs additional storage space to persist analytics data, he is required to buy additional file share and create a volume using this file share. He can then select this new volume while deploying the analytics server.

 For more information on adding volumes to {{site.data.keyword.containerlong}}, refer to [Storing persistent data in a volume by using the {{site.data.keyword.Bluemix_notm}} Dashboard ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://console.ng.bluemix.net/docs/containers/container_volumes_ov.html#container_volumes_ui.html){: new_window}.-->

* Pulse **Añadir Analytics** para crear y añadir una instancia de servicio de Mobile Analytics a la instancia de {{site.data.keyword.mobilefoundation_short}}.

<!--* You can choose the Mobile Analytics service configuration, the minimum supported configuration for the Analytics server is 2 nodes with 1 GB memory each, you can choose to create an Analytics server up to a maximum configuration of 32 nodes with 16 GB memory each.-->

Se inicia el proceso de suministro. Este proceso dura varios minutos y un indicador de progreso muestra el progreso de la operación.  

* Inicie la consola del servicio de Mobile Analytics desde {{site.data.keyword.mfp_oc_short_notm}}.

* El inicio de sesión único está habilitado entre {{site.data.keyword.mfserver_short_notm}} y el servicio de Mobile Analytics. El servicio de Mobile Analytics está configurado con las mismas claves de LTPA y credenciales de usuario que el servidor de {{site.data.keyword.mfserver_short_notm}}. Puede utilizar el mismo `nombre_usuario` y `contraseña` para iniciar sesión en la consola de Mobile Analytics como se solía iniciar sesión en {{site.data.keyword.mfp_oc_short_notm}}.

Para obtener más información sobre Mobile Analytics, puede consultar [MobileFirst Foundation Operational Analytics ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/analytics/){: new_window}.

> **Nota:** Al suprimir la instancia de servicio de {{site.data.keyword.mobilefoundation_short}}, se eliminará la instancia de servicio de Mobile Analytics.

##  Supresión del servicio de Mobile Analytics
{: #deleting_analytics_server_p5}

Ahora puede suprimir el servicio de Mobile Analytics que se ha añadido a la instancia de servicio de {{site.data.keyword.mobilefoundation_short}}, desde el panel de control de servicio de {{site.data.keyword.mobilefoundation_short}}.

* Pulse **Suprimir Analytics** para suprimir el servicio de Mobile Analytics que se ha añadido a la instancia de servicio de {{site.data.keyword.mobilefoundation_short}}.

 Al pulsar **Suprimir Analytics**, se suprime la instancia de servidor de análisis. El proceso de supresión de instancias de análisis tarda unos 10 minutos. Puede renovar la pantalla para ver el estado actualizado. Al suprimir la instancia de análisis, se vuelve a habilitar el botón **Añadir Analytics**. Si decide volver a añadir el servicio de Mobile Analytics, pulse este botón.

## Recreación del servidor de MobileFirst
{: #recreate_mobilefoundation_p5}

*	Pulse **Recrear** para volver a crear el servidor.

* Esta acción detiene el servidor existente y suprime los datos. Se crea una nueva instancia con una versión actualizada, si está disponible. Esta acción tarda unos minutos en completarse.

> **Nota**: los datos de su instancia de servidor anterior, incluida la información sobre apps y adaptadores, se conservan en la instancia de servicio de {{site.data.keyword.Db2_on_Cloud_short}} configurada. Estos datos se utilizan para volver a crear el servidor.

##	Ajuste de la configuración avanzada
{: #using_mfs_advanced_p5}

Utilice la opción **Iniciar servidor con configuración avanzada** en la página `Visión general` para crear el servidor con valores avanzados o personalizados. También puede actualizar los valores del servidor para personalizar su configuración desde el separador **Valores**. {{site.data.keyword.mobilefoundation_short}} le ofrece acceso a algunos valores avanzados.

*	Desde el separador **Topología**, puede seleccionar el tamaño del servidor y el número de instancias de servidor que necesita. El valor predeterminado del servidor es de 1 GB, que es suficiente para el desarrollo y la realización de pruebas no muy exigentes.
  - Seleccione el tamaño correcto para su servidor en función de sus necesidades.

  - **Instancias** muestra el número de instancias que se han creado.

      <!--- {{site.data.keyword.mobilefirst}} server farm can be created by configuring the number of nodes here. The minimum supported configuration is 2 nodes with 1 GB memory each and the maximum supported configuration is 32 nodes with 16 GB memory each.-->

Consulte la documentación de [{{site.data.keyword.mobilefoundation_long}} ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://www.ibm.com/support/knowledgecenter/SSHS8R_8.0.0/wl_welcome.html){: new_window}, para obtener más detalles.
