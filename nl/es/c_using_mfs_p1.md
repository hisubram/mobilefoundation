---

copyright:
  years: 2016, 2018
lastupdated:  "2018-03-07"

---

#	Utilización del plan Developer
{: #using_mobilefoundation_p1}

Después de crear la instancia de servicio de {{site.data.keyword.mobilefoundation_short}} utilizando el plan Developer, acceda a la página Visión general en {{site.data.keyword.Bluemix_notm}}. En esta página, se le proporcionarán guías de aprendizaje y vídeos para ayudarle a empezar con el servicio.

## Cómo trabajar con el servidor de MobileFirst
{: #start_mobilefoundation_p1}
* Puede acceder instantáneamente al servidor de MobileFirst y trabajar con él.

  Esta selección suministra un {{site.data.keyword.mfserver_long_notm}} con la configuración siguiente:
  *	1 GB de memoria. Este tamaño es suficiente para realizar actividades de desarrollo, de pruebas no muy exigentes y cargas de trabajo de producción a pequeña escala.

  * Para acceder al servidor de MobileFirst mediante la CLI, necesitará las credenciales, que están disponibles al pulsar **Credenciales de servicio** en el panel de navegación izquierdo de la consola de IBM Cloud.

<!--  The process of provisioning starts. This process takes about 10 minutes, and a message window indicates the progress of this operation. When complete a dashboard is displayed where you can see:
    *	The status of your server that is running (state, size).

    *	The server route created for you. Use this route in your mobile application to connect to the {{site.data.keyword.mfserver_short_notm}}.

    *	Your personal `username` and `password` to access the {{site.data.keyword.mfp_oc_short_notm}}. The `password` is hidden. Click **Show Password** icon to visualize it.

*	Click **Launch Console** to launch the {{site.data.keyword.mfp_oc_short_notm}}.-->

Ahora puede gestionar sus apps móviles y dispositivos móviles, utilizar su servidor como programa de fondo móvil, enviar notificaciones push, etc.

## Servicio Mobile Analytics
{: #adding_analytics_server_dev}

El servidor de Mobile Analytics está incluido y preconfigurado con la instancia de servicio del plan Mobile Foundation: Developer.

<!-- You can now monitor your mobile application on {{site.data.keyword.mobilefirst}} server by adding a Mobile Analytics service to the {{site.data.keyword.mobilefoundation_short}} service instance. Developer plan creates the Mobile Analytics service in a container group with a single node having 1 GB memory.

* Click **Add Analytics** to add the Mobile Analytics service to the {{site.data.keyword.mobilefoundation_short}} service instance.

  The process of provisioning starts. This process takes about 10 minutes, and a message window indicates the progress of this operation.  -->

* Inicie la consola del servicio de Mobile Analytics desde {{site.data.keyword.mfp_oc_short_notm}}.

* El inicio de sesión único está habilitado entre {{site.data.keyword.mfserver_short_notm}} y el servicio de Mobile Analytics. El servicio de Mobile Analytics está configurado con las mismas claves de LTPA y credenciales de usuario que {{site.data.keyword.mfserver_short_notm}}. Puede utilizar el mismo `nombre_usuario` y `contraseña` para iniciar sesión en la consola de Mobile Analytics como se solía iniciar sesión en {{site.data.keyword.mfp_oc_short_notm}}.

Para obtener más información sobre Mobile Analytics, puede consultar [MobileFirst Foundation Operational Analytics ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/analytics/){: new_window}.

> **Nota:** Al suprimir la instancia de servicio de {{site.data.keyword.mobilefoundation_short}}, se eliminará la instancia de servicio de Mobile Analytics.

<!--##  Deleting Mobile Analytics service
{: #deleting_analytics_server_dev}

You can now delete the Mobile Analytics service that was added to the {{site.data.keyword.mobilefoundation_short}} service instance, from the {{site.data.keyword.mobilefoundation_short}} service dashboard.

* Click **Delete Analytics** to delete the  Mobile Analytics service that was added to the {{site.data.keyword.mobilefoundation_short}} service instance.

 Clicking **Delete Analytics** deletes the analytics server instance. The process of deleting analytics instance takes about 10 minutes. You can refresh the screen to view the updated status. Deletion of analytics instance reenables the **Add Analytics** button. If you choose to add the Mobile Analytics service again, you can click this button.


## Re-creating the MobileFirst server
{: #recreate_mobilefoundation_p1}

*	Click **Recreate** to re-create the server.

* This action stops your existing server and deletes the data. All the data in your mobile server is lost. A new server instance is created with an updated version, if available. This action takes a few minutes to complete.

##	Setting up advanced configuration
{: #using_mfs_advanced_p1}

Use the **Start Server with Advanced Configuration** from the `Overview` page to create the server with advanced or custom settings. You can also update the server settings to customize your server configuration by clicking the **Configuration** tab. {{site.data.keyword.mobilefoundation_short}} gives you access to some advanced settings.

*	From the **Topology** tab, you can select the server size and the number of instances you need. The default 1 GB server is enough for development and moderate testing.

  - Select the correct size for your server based on your need.

* **Nodes** displays the number of nodes that are created. This field is not editable in {{site.data.keyword.mobilefoundation_short}}: Developer. The number of nodes is defaulted to **1** in the Developer plan.-->

Consulte la documentación de [{{site.data.keyword.mobilefoundation_long}} ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://www.ibm.com/support/knowledgecenter/SSHS8R_8.0.0/wl_welcome.html){: new_window} para obtener más detalles.
