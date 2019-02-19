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

#	Mobile Foundation CLI
{: #mobile_foundation_cli}

{{ site.data.keyword.mobilefoundation_short }} provides a Command Line Interface (CLI) tool for the developer **mfpdev**, to easily manage the {{site.data.keyword.mobilefoundation_short}} client and server artifacts.

All `mfpdev` commands can be run in interactive or direct mode. The parameters required for the command will be prompted for and some default values will be used in the interactive mode. In the direct mode, the parameters must be provided along with the command.


## Installing {{site.data.keyword.mobilefoundation_short}} CLI
{: #installing_mf_cli}

{{site.data.keyword.mobilefoundation_short}} is available as an NPM package in the [NPM registry ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://www.npmjs.com){: new_window}.

Ensure **node.js** and **npm** are installed to install NPM packages. Follow the installation instructions in [nodejs.org ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://nodejs.org/){: new_window} to install **node.js**. To confirm if **node.js** is installed properly, run the following command:
```
node -v
```
{: codeblock}

> **Note:** Minimum supported node.js version is **4.2.3**. Also, with the fast evolving node and npm packages, the {{site.data.keyword.mobilefoundation_short}} CLI may not be fully functional with all the available versions of node and npm including the latest versions. Ensure that node is on version **6.11.1** and npm version is **3.10.10**, for proper functioning of the CLI.

> For MobileFirst CLI interim fix versions *8.0.2018100112* and higher, you can use Node versions 8.x or 10.x.

To install the {{site.data.keyword.mobilefoundation_short}} CLI, run the following command:
```
npm install -g mfpdev-cli 
```
{: codeblock}

If the CLI  compressed file (*.zip*) was downloaded from the Download Center of the MobileFirst Operations Console, use the following command:
```
npm install -g <path-to-mfpdev-cli.tgz> 
```
{: codeblock}

To install the CLI without optional dependencies, add the `--no-optional` flag:
```
npm install -g --no-optional path-to-mfpdev-cli.tgz
```
{: codeblock}

While installing MobileFirst CLI using Node 8, you may see few of the following errors in the terminal window:
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

This error is due a [known bug in node-gyp ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://github.com/nodejs/node-gyp/issues/1547){: new_window}. These errors can be ignored as it doesn't affect the functioning of the MobileFirst CLI. This issue is applicable for `mfpdev-cli` interim fix level *8.0.2018100112* and higher. To overcome this error, use the `--no-optional` flag during installation.

To confirm that the CLI is installed correctly, run the following command:
```
mfpdev
```
{: codeblock}

The CLI help will be printed as output.

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

## List of {{site.data.keyword.mobilefoundation_short}} CLI commands
{: #list_cli_commands}

<table summary="{{site.data.keyword.mobilefoundation_short}} CLI commands that have links that bring you to more info for the command">
<caption>Table 1. {{site.data.keyword.mobilefoundation_short}} CLI commands</caption>
 <thead>
 <th colspan="6">General [mfpdev](#mfpdev) commands</th>
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

Sets your configuration preferences for preview browser type, preview timeout value, and server timeout value for the mfpdev command-line interface.

```
mfpdev config
```
{: codeblock}

Displays information about your environment, including operating system, memory consumption, node version, and command-line interface version. If the current directory is a Cordova application, information provided by the Cordova `cordova info` command is also displayed.

```
mfpdev info
```
{: codeblock}

Displays the version number of the {{site.data.keyword.mobilefoundation_short}} CLI currently in use.

```
mfpdev -v
```
{: codeblock}

Debug mode: Produces debug output.

```
mfpdev [-d|--debug]
```
{: codeblock}

Verbose debug mode: Produces verbose debug output.

```
mfpdev [-dd|--ddebug]
```
{: codeblock}

Suppresses use of color in command output.

```
mfpdev -no-color
```
{: codeblock}

### mfpdev help
{: #mfpdev_help}

Displays help for MobileFirst CLI (mfpdev) commands. With command name as an argument, it displays more specific help text for each command type or command. For example, `mfpdev help server add`.

```
mfpdev help <command name>
```
{: codeblock}


### mfpdev app
{: #mfpdev_app}

Registers your app with a MobileFirst Server.

```
mfpdev app register
```
{: codeblock}

To register an app to a non-default server and runtime use:

```
mfpdev app register <server> <runtime>
```
{: codeblock}

For Cordova Windows platform, the `-w <platform>` argument must be added to the command. The platform argument is a comma-separated list of the windows platforms to be registered. Valid values are windows, windows8 and windowsphone8.

```
mfpdev app register -w windows8
```
{: codeblock}

Enables you to specify the back-end server and runtime to use for your app. For Cordova apps, this command enables you to configure several more aspects such as the default language for system messages and whether to do a checksum security check. Other configuration parameters are included for Cordova apps.

```
mfpdev app config
```
{: codeblock}

Retrieves an existing app configuration from the server.

```
mfpdev app pull
```
{: codeblock}

Sends app configuration to the server.

```
mfpdev app push
```
{: codeblock}

Enables you to preview your Cordova app without requiring an actual device of the target platform type. You can view the preview in either the Mobile Browser Simulator or your web browser.

```
mfpdev app preview
```
{: codeblock}

Packages the application resources present in the www directory into a compressed (*.zip*) file that can be used for the direct update process.

```
mfpdev app webupdate
```
{: codeblock}

This command will package the updated web resources to a compressed (*.zip*) file and upload it to the default MobileFirst Server registered. The packaged web resources can be found at the `[cordova-project-root-folder]/mobilefirst/` folder.

To upload the web resources to different server instance, provide the server name and runtime as part of the command:

```
mfpdev app webupdate <server_name> <runtime>
```
{: codeblock}

You can use the –build parameter to generate the compressed (*.zip*) file with the packaged web resources without uploading it to a server.
```
mfpdev app webupdate --build
```
{: codeblock}

To upload a package that was previously built, use the –file parameter:
```
mfpdev app webupdate --file mobilefirst/com.ibm.test-android-1.0.0.zip
```
{: codeblock}

There's also the option to encrypt the content of package using the –encrypt parameter:
```
mfpdev app webupdate --encrypt
```
{: codeblock}


### mfpdev server
{: #mfpdev_server}

Displays information about the MobileFirst Server.

```
mfpdev server info
```
{: codeblock}

Adds a server definition to your environment.

```
mfpdev server add
```
{: codeblock}

Enables you to edit a server definition.

```
mfpdev server edit
```
{: codeblock}

To set a server as the default one, use:

```
mfpdev server edit <server_name> --setdefault
```
{: codeblock}

Removes a server definition from your environment.

```
mfpdev server remove
```
{: codeblock}

Opens the MobileFirst Operations Console.

```
mfpdev server console
```
{: codeblock}

To open the console of another server, provide the server name as a parameter for the command:

```
mfpdev server console <server-name>
```
{: codeblock}

Unregisters apps and removes adapters from the MobileFirst Server.

```
mfpdev server clean
```
{: codeblock}

### mfpdev adapter
{: #mfpdev_adapter}

Creates an adapter.

```
mfpdev adapter create
```
{: codeblock}

Builds an adapter.

```
mfpdev adapter build
```
{: codeblock}

Finds and builds all of the adapters in the current directory and its subdirectories.

```
mfpdev adapter build all
```
{: codeblock}

Deploys an adapter to the MobileFirst Server.

```
mfpdev adapter deploy
```
{: codeblock}

To deploy to a different server, use:
```
mfpdev adapter deploy <server_name>
```
{: codeblock}

Finds all of the adapters in the current directory and its subdirectories, and deploys them to the MobileFirst Server.

```
mfpdev adapter deploy all
```
{: codeblock}

Calls an adapter’s procedure on the MobileFirst Server.

```
mfpdev adapter call
```
{: codeblock}

Retrieves an existing adapter configuration from the server.
```
mfpdev adapter pull
```
{: codeblock}

Sends adapter configuration to the server.
```
mfpdev adapter push
```
{: codeblock}

## Updating and uninstalling {{site.data.keyword.mobilefoundation_short}} CLI
{: #update_uninstall_mf_cli}

To update the command line interface, run the command:

```
npm update -g mfpdev-cli
```
{: codeblock}

To uninstall the command line interface, run the command:

```
npm uninstall -g mfpdev-cli
```
{: codeblock}
