---

copyright:
  years: 2018, 2019
lastupdated:  "2018-11-19"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:tip: .tip}
{:pre: .pre}

#	CLI de {{ site.data.keyword.mobilefoundation_short }}
{: #using_mobilefoundation_cli}

{{ site.data.keyword.mobilefoundation_short }} proporciona una herramienta de interfaz de línea de mandatos (CLI) a desarrolladores, **mfpdev**, para gestionar fácilmente los artefactos del cliente y del servidor de {{site.data.keyword.mobilefoundation_short}}.

Todos los mandatos `mfpdev` pueden ejecutarse en modalidad directa o interactiva. En la modalidad interactiva, se solicitarán los parámetros necesarios para el mandato y se utilizarán algunos valores predeterminados. En la modalidad directa, se deben proporcionar los parámetros con el mandato.


## Instalación de la CLI de {{site.data.keyword.mobilefoundation_short}}
{: #installing_mf_cli}

{{site.data.keyword.mobilefoundation_short}} está disponible como un paquete de NPM en el [registro NPM ![icono de enlace externo](../../icons/launch-glyph.svg "icono de enlace externo")](https://www.npmjs.com){: new_window}.

Asegúrese de que **node.js** y **npm** estén instalados para instalar paquetes NPM. Siga las instrucciones de instalación en [nodejs.org ![icono de enlace externo](../../icons/launch-glyph.svg "icono de enlace externo")](https://nodejs.org/){: new_window} para instalar **node.js**. Para confirmar que **node.js** esté instalado correctamente, ejecute el mandato siguiente:
```
node -v
```
{: codeblock}

> **Nota:** La versión de node.js mínima soportada es **4.2.3**. Además, con los paquetes de nodos y de npm de rápida evolución, la CLI de {{site.data.keyword.mobilefoundation_short}} podría no ser completamente funcional con todas las versiones disponibles del nodo y de npm, incluidas las versiones más recientes. Asegúrese de que el nodo se encuentra en la versión **6.11.1** y que la versión de npm es **3.10.10**, para el funcionamiento adecuado de la CLI.

> Para las versiones de arreglos temporales de la CLI de MobileFirst *8.0.2018100112* y superiores, puede utilizar las versiones de Node 8.x o 10.x.

Para instalar la CLI de {{site.data.keyword.mobilefoundation_short}}, ejecute el mandato siguiente:
```
npm install -g mfpdev-cli 
```
{: codeblock}

Si el archivo comprimido (*.zip*) de la CLI se ha descargado desde el Centro de descargas de la consola de operaciones de MobileFirst, utilice el mandato siguiente:
```
npm install -g <path-to-mfpdev-cli.tgz> 
```
{: codeblock}

Para instalar la CLI sin dependencias opcionales, añada el distintivo `--no-optional`:
```
npm install -g --no-optional path-to-mfpdev-cli.tgz
```
{: codeblock}

Al instalar la CLI de MobileFirst utilizando Node 8, es posible que vea algunos de los siguientes errores en la ventana de terminal:
```
> node-gyp rebuild

gyp ERR! clean error
gyp ERR! stack Error: EACCES: permission denied, rmdir 'build'
gyp ERR! System Darwin 18.0.0
gyp ERR! command "/usr/local/bin/node" "/usr/local/lib/node_modules/npm/node_modules/node-gyp/bin/node-gyp.js" "rebuild"
gyp ERR! cwd /usr/local/lib/node_modules/mfpdev-cli/node_modules/bufferutil
gyp ERR! node -v v8.12.0
gyp ERR! node-gyp -v v3.8.0
gyp ERR! not ok

> utf-8-validate@1.2.2 install /usr/local/lib/node_modules/mfpdev-cli/node_modules/utf-8-validate
> node-gyp rebuild

gyp ERR! clean error
gyp ERR! stack Error: EACCES: permission denied, rmdir 'build'
gyp ERR! System Darwin 18.0.0
gyp ERR! command "/usr/local/bin/node" "/usr/local/lib/node_modules/npm/node_modules/node-gyp/bin/node-gyp.js" "rebuild"
gyp ERR! cwd /usr/local/lib/node_modules/mfpdev-cli/node_modules/utf-8-validate
gyp ERR! node -v v8.12.0
gyp ERR! node-gyp -v v3.8.0
gyp ERR! not ok

> fsevents@1.2.4 install /usr/local/lib/node_modules/mfpdev-cli/node_modules/fsevents
> node install
```

Este error es debido a un [error conocido en node-gyp ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/nodejs/node-gyp/issues/1547){: new_window}. Estos errores se pueden ignorar puesto que no afectan al funcionamiento de la CLI de MobileFirst. El problema se puede aplicar al nivel de arreglo temporal de `mfpdev-cli` *8.0.2018100112* y superior. Para evitar este error, utilice el distintivo `--no-optional` durante la instalación.

Para confirmar que la CLI esté instalada correctamente, ejecute el mandato siguiente:
```
mfpdev
```
{: codeblock}

La ayuda de la CLI se imprimirá como una salida.

```
 NAME
     IBM MobileFirst Foundation Command Line Interface (CLI).

 SYNOPSIS
     mfpdev <command> [options]

 DESCRIPTION
     The IBM MobileFirst Foundation Command Line Interface (CLI) is a command-line
     for developing MobileFirst applications. The command-line can be used by itself, or in conjunction  with the IBM MobileFirst Foundation Operations Console. Some functions are available from  the command-line only and not the console.

     For more information and a step-by-step example of using the CLI, see the IBM Knowledge Center for your version of IBM MobileFirst Foundation at
     https://www.ibm.com/support/knowledgecenter.
     ...
     ...
     ...
```
{: screen}

## Lista de mandatos de CLI de {{site.data.keyword.mobilefoundation_short}}
{: #list_cli_commands}

<table summary="Mandatos de CLI de {{site.data.keyword.mobilefoundation_short}} que tienen enlaces que proporcionan más información para el mandato">
<caption>Tabla 1. Mandatos de CLI de {{site.data.keyword.mobilefoundation_short}}</caption>
 <thead>
 <th colspan="6">Mandatos [mfpdev](#mfpdev) generales</th>
 </thead>
 <tbody>
 <tr>
 <td>[app](#mfpdev_app)</td>
 <td>[server](#mfpdev_server)</td>
 <td>[adapter](#mfpdev_adapter)</td>
 <td>[help](#mfpdev_help)</td>
 </tr>
   </tbody>
 </table>

### mfpdev
{: #mfpdev}

Establece las preferencias de configuración para el tipo de navegador de vista previa, el valor del tiempo de espera de vista previa y el valor de tiempo de espera de servidor para la interfaz de línea de mandatos mfpdev.

```
mfpdev config
```
{: codeblock}

Visualiza información sobre el entorno, incluido el sistema operativo, el consumo de memoria, la versión del nodo y la versión de la interfaz de línea de mandatos. Si el directorio actual es una aplicación de Cordova, también se mostrará la información proporcionada por el mandato `cordova info` de Cordova.

```
mfpdev info
```
{: codeblock}

Muestra el número de versión de la CLI de {{site.data.keyword.mobilefoundation_short}} actualmente en uso.

```
mfpdev -v
```
{: codeblock}

Modalidad de depuración: Genera la salida de depuración.

```
mfpdev [-d|--debug]
```
{: codeblock}

Modalidad de depuración detallada: Genera la salida de depuración detallada.

```
mfpdev [-dd|--ddebug]
```
{: codeblock}

Suprime la utilización del color en la salida del mandato.

```
mfpdev -no-color
```
{: codeblock}

### mfpdev help
{: #mfpdev_help}

Muestra ayuda para los mandatos de CLI de MobileFirst (mfpdev). Con el nombre de mandato como un argumento, muestra texto de ayuda más específico para cada mandato o tipo de mandato. Por ejemplo, `mfpdev help server add`.

```
mfpdev help <command name>
```
{: codeblock}


### mfpdev app
{: #mfpdev_app}

Registra la app con un servidor de MobileFirst.

```
mfpdev app register
```
{: codeblock}

Para registrar una app en un servidor y en un tiempo de ejecución que no sean los predeterminados, utilice:

```
mfpdev app register <server> <runtime>
```
{: codeblock}

Con la plataforma Cordova Windows, se debe añadir el argumento `-w <platform>` al mandato. El argumento de la plataforma es una lista separada por comas de las plataformas de Windows que se registrarán. Los valores válidos son windows, windows8 y windowsphone8.

```
mfpdev app register -w windows8
```
{: codeblock}

Permite especificar el tiempo de ejecución y el servidor de fondo para que los utilice su app. Para las apps de Cordova, este mandato permite configurar varios aspectos adicionales como, por ejemplo, el idioma predeterminado para los mensajes del sistema o si es necesario realizar una suma de comprobación. Se incluyen otros parámetros de configuración para apps de Cordova.

```
mfpdev app config
```
{: codeblock}

Recupera una configuración de app existente desde el servidor.

```
mfpdev app pull
```
{: codeblock}

Envía la configuración de app al servidor.

```
mfpdev app push
```
{: codeblock}

Habilita obtener una vista previa de su app de Cordova sin que sea necesario el dispositivo real en el tipo de plataforma de destino. Puede ver la vista previa en el simulador del navegador móvil o en el navegador web.

```
mfpdev app preview
```
{: codeblock}

Empaqueta los recursos de la aplicación presentes en el directorio www en un archivo comprimido (*.zip*) que se puede utilizar para el proceso de Direct Update.

```
mfpdev app webupdate
```
{: codeblock}

Este mandato empaquetará los recursos web actualizados en un archivo comprimido (*.zip*) y los cargará al servidor de MobileFirst predeterminado registrado. Los recursos web empaquetados se pueden encontrar en la carpeta `[cordova-project-root-folder]/mobilefirst/`.

Para cargar los recursos web a distintas instancias de servidor, proporcione el nombre de servidor y el tiempo de ejecución como parte del mandato:

```
mfpdev app webupdate <server_name> <runtime>
```
{: codeblock}

Puede utilizar el parámetro –build para generar el archivo comprimido (*.zip*) con los recursos web empaquetados sin cargarlos en un servidor.
```
mfpdev app webupdate --build
```
{: codeblock}

Para cargar un paquete creado anteriormente, utilice el parámetro –file:
```
mfpdev app webupdate --file mobilefirst/com.ibm.test-android-1.0.0.zip
```
{: codeblock}

También existe la opción de cifrar el contenido del paquete utilizando el parámetro –encrypt:
```
mfpdev app webupdate --encrypt
```
{: codeblock}


### mfpdev server
{: #mfpdev_server}

Muestra información sobre el servidor de MobileFirst.

```
mfpdev server info
```
{: codeblock}

Añade una definición de servidor en el entorno.

```
mfpdev server add
```
{: codeblock}

Habilita la edición de definiciones de servidor.

```
mfpdev server edit
```
{: codeblock}

Para establecer el servidor como el predeterminado, utilice:

```
mfpdev server edit <server_name> --setdefault
```
{: codeblock}

Elimina una definición de servidor del entorno.

```
mfpdev server remove
```
{: codeblock}

Abre la consola de operaciones de MobileFirst.

```
mfpdev server console
```
{: codeblock}

Para abrir la consola de otro servidor, proporcione el nombre de servidor como un parámetro para el mandato:

```
mfpdev server console <server-name>
```
{: codeblock}

Elimina el registro de apps y elimina adaptadores del servidor de MobileFirst.

```
mfpdev server clean
```
{: codeblock}

### mfpdev adapter
{: #mfpdev_adapter}

Crea un adaptador.

```
mfpdev adapter create
```
{: codeblock}

Compila un adaptador.

```
mfpdev adapter build
```
{: codeblock}

Encuentra y compila todos los adaptadores en el directorio actual y sus subdirectorios.

```
mfpdev adapter build all
```
{: codeblock}

Despliega un adaptador en el servidor de MobileFirst.

```
mfpdev adapter deploy
```
{: codeblock}

Para desplegarlo en un servidor diferente, utilice:
```
mfpdev adapter deploy <server_name>
```
{: codeblock}

Busca todos los adaptadores en el directorio actual y sus subdirectorios, y los despliega en el servidor de MobileFirst.

```
mfpdev adapter deploy all
```
{: codeblock}

Llama el procedimiento de adaptador en el servidor de MobileFirst.

```
mfpdev adapter call
```
{: codeblock}

Recupera una configuración de adaptador existente desde el servidor.
```
mfpdev adapter pull
```
{: codeblock}

Envía la configuración de adaptador al servidor.
```
mfpdev adapter push
```
{: codeblock}

## Actualización y desinstalación de la CLI de {{site.data.keyword.mobilefoundation_short}}
{: #update_uninstall_mf_cli}

Ejecute el siguiente mandato para actualizar la interfaz de línea de mandatos:

```
npm update -g mfpdev-cli
```
{: codeblock}

Ejecute el siguiente mandato para desinstalar la interfaz de línea de mandatos:

```
npm uninstall -g mfpdev-cli
```
{: codeblock}
