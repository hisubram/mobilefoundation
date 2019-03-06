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

#	CLI do Mobile Foundation
{: #mobile_foundation_cli}

{{ site.data.keyword.mobilefoundation_short }} fornece uma ferramenta de Interface da Linha de Comandos (CLI) para o desenvolvedor **mfpdev**, para gerenciar facilmente os artefatos do cliente e do servidor do {{site.data.keyword.mobilefoundation_short}}.

Todos os comandos `mfpdev` podem ser executados no modo interativo ou direto. Os parâmetros necessários para o comando serão solicitados e alguns valores padrão serão usados no modo interativo. No modo direto, os parâmetros devem ser fornecidos junto com o comando.


## Instalando o {{site.data.keyword.mobilefoundation_short}} CLI
{: #installing_mf_cli}

O {{site.data.keyword.mobilefoundation_short}} está disponível como um pacote de NPM no [registro de NPM ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://www.npmjs.com){: new_window}.

Assegure-se de que **node.js** e **npm** estejam instalados para instalar pacotes NPM. Siga as instruções de instalação em [nodejs.org ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://nodejs.org/){: new_window} para instalar o **node.js**. Para confirmar se o **node.js** está instalado corretamente, execute o comando a seguir:
```
-v nó
```
{: codeblock}

> **Nota:** a versão de node.js mínima suportada é **4.2.3**. Além disso, com a rápida evolução do nó e dos pacotes npm, a CLI do {{site.data.keyword.mobilefoundation_short}} pode não ser totalmente funcional com todas as versões disponíveis de nó e npm, incluindo as versões mais recentes. Certifique-se de que o nó esteja na versão **6.11.1** e que a versão de npm seja **3.10.10** para o funcionamento adequado da CLI.

> Para a correção temporária da CLI do MobileFirst versões *8.0.2018100112* e superior, é possível usar o Node versões 8.x ou 10.x.

Para instalar a CLI do {{site.data.keyword.mobilefoundation_short}}, execute o seguinte comando:
```
-g instala npm mfpdev-cli 
```
{: codeblock}

Se o arquivo compactado da CLI (*.zip*) foi transferido por download por meio do Centro de Download do MobileFirst Operations Console, use o comando a seguir:
```
npm install -g <path-to-mfpdev-cli.tgz> 
```
{: codeblock}

Para instalar a CLI sem dependências opcionais, inclua a sinalização `--no-optional`:
```
-g instala npm -- sem caminho opcional-to-mfpdev-cli.tgz
```
{: codeblock}

Ao instalar a CLI do MobileFirst usando o Node 8, você pode ver alguns dos erros a seguir na janela do terminal:
```
> node-gyp rebuild

gyp ERR! clean error gyp ERR! stack Error: EACCES: permission denied, rmdir 'build'
gyp ERR! System Darwin 18.0.0
gyp ERR! command "/usr/local/bin/node" "/usr/local/lib/node_modules/npm/node_modules/node-gyp/bin/node-gyp.js" "rebuild"
gyp ERR! cwd /usr/local/lib/node_modules/mfpdev-cli/node_modules/bufferutil
gyp ERR! node -v v8.12.0
gyp ERR! node-gyp -v v3.8.0
gyp ERR! not ok 

> utf-8-validate@1.2.2 install /usr/local/lib/node_modules/mfpdev-cli/node_modules/utf-8-validate
> node-gyp rebuild

gyp ERR! clean error gyp ERR! stack Error: EACCES: permission denied, rmdir 'build'
gyp ERR! System Darwin 18.0.0
gyp ERR! command "/usr/local/bin/node" "/usr/local/lib/node_modules/npm/node_modules/node-gyp/bin/node-gyp.js" "rebuild"
gyp ERR! cwd /usr/local/lib/node_modules/mfpdev-cli/node_modules/utf-8-validate
gyp ERR! node -v v8.12.0
gyp ERR! node-gyp -v v3.8.0
gyp ERR! not ok 

> fsevents@1.2.4 install /usr/local/lib/node_modules/mfpdev-cli/node_modules/fsevents
> node install
```

Esse erro é devido a um [erro conhecido no nó-gyp ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/nodejs/node-gyp/issues/1547){: new_window}. Esses erros podem ser ignorados já que isso não afeta o funcionamento da CLI do MobileFirst. Esse problema é aplicável para o nível de correção temporária `mfpdev-cli` *8.0.2018100112* e superior. Para superar esse erro, use a sinalização `--no-optional` durante a instalação.

Para confirmar que a CLI está instalada corretamente, execute o seguinte comando:
```
Mfpdev
```
{: codeblock}

A ajuda da CLI será impressa como saída.

```
 NOME
     Interface da linha de comandos (CLI) do IBM MobileFirst Foundation.

 SINOPSE
     mfpdev <command> [opções]

 DESCRIÇÃO
     A interface da linha de comandos (CLI) do IBM MobileFirst Foundation é uma linha de comandos para desenvolver aplicativos MobileFirst. A linha de comandos pode ser usada sozinha ou em conjunto com o IBM MobileFirst Foundation Operations Console. Algumas funções estão
disponíveis somente da linha de comandos e não do console.

     Para obter mais informações e um exemplo passo a passo do uso da CLI, consulte o IBM Knowledge Center para a sua versão do IBM
MobileFirst Foundation em https://www.ibm.com/support/knowledgecenter.
     ...
     ...
     ...
```
{: screen}

## Lista de {{site.data.keyword.mobilefoundation_short}} comandos da CLI
{: #list_cli_commands}

<table summary="{{site.data.keyword.mobilefoundation_short}} Comandos da CLI que possuem links que trazem mais informações para o comando">
<caption>Tabela 1. {{site.data.keyword.mobilefoundation_short}} Comandos da CLI</caption>
 <thead>
 <th colspan="6">Geral [mfpdev](#mfpdev) os comandos</th>
 </thead>
 <tbody>
 <tr>
 <td>[app](#mfpdev_app)</td>
 <td>[servidor](#mfpdev_server)</td>
 <td>[adaptador](#mfpdev_adapter)</td>
 <td>[Ajuda](#mfpdev_help)</td>
 </tr>
   </tbody>
 </table>

### Mfpdev
{: #mfpdev}

Define as suas preferências de configuração para o tipo de navegador de visualização, o valor de tempo limite de visualização e
o valor de tempo limite do servidor para a interface da linha de comandos mfpdev.

```
Mfpdev configuração
```
{: codeblock}

Exibe informações sobre o seu ambiente, incluindo o sistema operacional, o consumo de memória, a versão do nó e a versão da
interface da linha de comandos. Se o diretório atual for um aplicativo Cordova, informações fornecidas pelo comando do Cordova
`cordova info` também serão exibidas.

```
Informações mfpdev
```
{: codeblock}

Exibe o número da versão da CLI do {{site.data.keyword.mobilefoundation_short}} atualmente em uso.

```
-v mfpdev
```
{: codeblock}

Modo de Depuração: Produz a saída de depuração.

```
mfpdev [-d|--debug]
```
{: codeblock}

Modo de depuração detalhado: produz saída de depuração detalhada.

```
Mfpdev [ -dd | --ddebug ]
```
{: codeblock}

Suprime o uso de cor na saída de comando.

```
Cor-Não mfpdev
```
{: codeblock}

### Mfpdev ajuda
{: #mfpdev_help}

Exibe ajuda para comandos da CLI (mfpdev) do MobileFirst. Com o nome do comando como um argumento, isso exibe texto de ajuda mais específico para cada tipo de comando ou comando. Por exemplo, `mfpdev helpserver add`.

```
mfpdev help <command name>
```
{: codeblock}


### Aplicativo mfpdev
{: #mfpdev_app}

Registra o seu aplicativo com um servidor MobileFirst.

```
Registro do aplicativo mfpdev
```
{: codeblock}

Para registrar um app em um servidor não padrão e no tempo de execução, use:

```
mfpdev app register <server> <runtime>
```
{: codeblock}

Para a plataforma Windows Cordova, o argumento `-w <platform>` deve ser incluído no comando. O argumento da plataforma é uma lista separada por vírgula das plataformas Windows a serem registradas. Os valores válidos são Windows, windows8 e
windowsphone8.

```
Mfpdev aplicativo registrar -w windows8
```
{: codeblock}

Permite especificar o servidor de backend e o tempo de execução a serem usados para o seu aplicativo. Para apps Cordova, esse comando permite configurar vários aspectos adicionais, como o idioma padrão para mensagens do sistema e se deve fazer uma verificação de segurança de soma de verificação. Outros parâmetros de configuração são incluídos para aplicativos Cordova.

```
Configapp mfpdev
```
{: codeblock}

Recupera uma configuração do aplicativo existente do servidor.

```
Puxa app mfpdev
```
{: codeblock}

Envia a configuração do aplicativo para o servidor.

```
Comando app mfpdev
```
{: codeblock}

Permite visualizar o aplicativo Cordova sem a necessidade de um dispositivo real do tipo da plataforma de destino. É possível visualizar a visualização no simulador de navegador móvel ou em seu navegador da web.

```
mfpdev app preview
```
{: codeblock}

Empacota os recursos de aplicativo presentes no diretório www em um arquivo compactado (*.zip*) que pode ser usado para o processo de atualização direta.

```
Webupdate app mfpdev
```
{: codeblock}

Esse comando empacotará os recursos da web atualizados para um arquivo compactado (*.zip*) e fará upload dele para o MobileFirst Server padrão registrado. Os recursos da web empacotados podem ser localizados na pasta `[cordova-project-root-folder]/mobilefirst/`.

Para fazer upload dos recursos da web para a instância do servidor diferente, forneça o nome do servidor e o tempo de
execução como parte do comando:

```
mfpdev app webupdate <server_name> <runtime>
```
{: codeblock}

É possível usar o parâmetro -build para gerar o arquivo compactado (*.zip*) com os recursos da web empacotados sem fazer upload dele para um servidor.
```
Webupdate app mfpdev -- compilação
```
{: codeblock}

Para fazer upload de um pacote que foi construído anteriormente, use o parâmetro -file:
```
Webupdate aplicativo mfpdev -- arquivo mobilefirst/com.ibm.test-android-1.0.0.zip
```
{: codeblock}

Há também a opção de criptografar o conteúdo do pacote usando o parâmetro -encrypt:
```
Webupdate aplicativo mfpdev -- encrypt
```
{: codeblock}


### Mfpdev do servidor
{: #mfpdev_server}

Exibe informações sobre o servidor MobileFirst.

```
Informações do servidor mfpdev
```
{: codeblock}

Inclui uma definição do servidor em seu ambiente.

```
Servidor mfpdev add
```
{: codeblock}

Permite editar uma definição de servidor.

```
Mfpdev do servidor de edição
```
{: codeblock}

Para configurar um servidor como o único padrão, use:

```
Server_name> < editar servidor mfpdev -- setdefault
```
{: codeblock}

Remove uma definição de servidor do seu ambiente.

```
Remover servidor mfpdev
```
{: codeblock}

Abre o Console de Operações MobileFirst.

```
Console do servidor mfpdev
```
{: codeblock}

Para abrir o console de outro servidor, forneça o nome do servidor como um parâmetro para o comando:

```
mfpdev server console <server-name>
```
{: codeblock}

Cancela o registro de aplicativos e remove os adaptadores do servidor MobileFirst.

```
Mfpdev do servidor limpo
```
{: codeblock}

### Adaptador mfpdev
{: #mfpdev_adapter}

Cria um adaptador.

```
Criar adaptador mfpdev
```
{: codeblock}

Constrói um adaptador.

```
Mfpdev do adaptador de compilação
```
{: codeblock}

Localiza e constrói todos os adaptadores no diretório atual e seus subdiretórios.

```
Mfpdev adaptador construção todos
```
{: codeblock}

Implementa um adaptador para o servidor MobileFirst.

```
Implementação do adaptador mfpdev
```
{: codeblock}

Para implementar em um servidor diferente, use:
```
mfpdev adapter deploy <server_name>
```
{: codeblock}

Localiza todos os adaptadores no diretório atual e seus subdiretórios e os implementa no servidor MobileFirst.

```
mfpdev adapter deploy all
```
{: codeblock}

Chama o procedimento de um adaptador no servidor MobileFirst.

```
Mfpdev chamada do adaptador
```
{: codeblock}

Recupera uma configuração de adaptador existente do servidor.
```
Puxe o adaptador mfpdev
```
{: codeblock}

Envia a configuração do adaptador para o servidor.
```
Comando de adaptador mfpdev
```
{: codeblock}

## Atualizando e desinstalando a CLI do {{site.data.keyword.mobilefoundation_short}}
{: #update_uninstall_mf_cli}

Para atualizar a interface da linha de comandos, execute o comando:

```
-g npm mfpdev-cli de atualização
```
{: codeblock}

Para desinstalar a interface da linha de comandos, execute o comando:

```
Mfpdev-cli -g npm desinstalação
```
{: codeblock}
