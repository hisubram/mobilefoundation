---

copyright:
  years: 2018, 2019
lastupdated: "2018-12-21"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:note: .note}
{:tip: .tip}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}

# Direct Update
{: #direct_update}

Os recursos da web (JavaScript, HTML, CSS ou arquivos de imagem) em aplicativos Cordova ou Ionic podem ser atualizados over-the-air (OTA) com **Direct Update** sem a necessidade de os usuários terem que passar pela atualização do App Store ou Play Store. Usando o recurso Direct Update, as empresas podem assegurar que os usuários finais usem a versão mais recente de seus apps. Para atualizar um aplicativo, os recursos da web atualizados do aplicativo precisam ser empacotados e transferidos por upload para a sua instância do Mobile Foundation usando a CLI do Mobile Foundation ou implementando um archive gerado. Direct Update é ativado automaticamente. Ele é, então, cumprido em cada solicitação do usuário para um recurso protegido.

![Diagrama de como o Direct Update funciona](images/internal_function.jpg)

O Direct Update é suportado nas plataformas Cordova iOS e Cordova Android.
{: note}

Para a fase de teste de produção ou pré-produção em tempo real, é recomendável implementar o Direct Update seguro antes de publicar seu aplicativo na loja de apps. A atualização direta segura requer um par de chaves RSA extraído de um certificado do servidor assinado de CA real.

1. Assegure-se de não modificar a configuração do keystore após a publicação do aplicativo. As atualizações transferidas por download não podem ser autenticadas antes de reconfigurar o aplicativo com uma nova chave pública e publicar novamente o aplicativo. Se você não executar as duas etapas acima, a atualização direta falhará no cliente.
2. A atualização direta atualiza somente os recursos da web do aplicativo. Para atualizar recursos nativos, uma nova versão do aplicativo deve ser submetida à respectiva loja de aplicativos.
3. Quando o recurso de atualização direta é usado e o recurso de soma de recursos da web está ativado, uma nova base de soma de verificação é estabelecida com cada atualização direta.
4. Se o servidor Mobile Foundation foi submetido a upgrade, ele continuará a entregar as atualizações diretas corretamente. No entanto, se um archive da atualização direta recentemente construído (arquivo .zip) for transferido por upload, ele poderá parar as atualizações para os clientes mais antigos. A razão é que o archive contém a versão do plug-in `cordova-plugin-mfp`. Antes de atender esse archive para um cliente móvel, o servidor compara a versão do cliente com a versão do plug-in. Se ambas as versões forem próximas o suficiente (o que significa que os três dígitos mais significativos são idênticos), a atualização direta ocorrerá normalmente. Caso contrário, o servidor Mobile Foundation ignorará silenciosamente a atualização. Uma solução para a incompatibilidade de versão é fazer download do `cordova-plugin-mfp` com a mesma versão que aquela do seu projeto Cordova original e gerar novamente o archive da atualização direta.
{: tip}

Após uma atualização direta, o aplicativo não utiliza mais os recursos da web predefinidos. Em vez disso, ele usa os recursos da web transferidos por download do ambiente de simulação do aplicativo. Se o cache do aplicativo no dispositivo for limpo, os recursos da web empacotados originais serão usados novamente.

O Direct Update de recursos da web se aplica somente a uma versão específica do app. Por exemplo, as atualizações geradas para um aplicativo versão 2.0 não serão entregues para a versão 2.1 do mesmo app.
{: note}

## Criando e implementando recursos da web atualizados
{: #creating_deploying_updates}

Depois de fazer mudanças em seus recursos da web, é possível empacotar e fazer upload deles para a sua instância do Mobile Foundation usando a CLI do Mobile Foundation.

1.  O Direct Update atualiza somente para os recursos da web do aplicativo. Para atualizar recursos nativos, uma nova versão do aplicativo deve ser submetida à respectiva loja de aplicativos.
2. O serviço Mobile Foundation parará a atualização direta para os clientes se a parte nativa do app for significativamente diferente daquela transferida por upload para o serviço Mobile Foundation. Isso pode ocorrer quando o *cordova-plugin-mfp* é atualizado com uma versão mais recente do plug-in. Antes de entregar o archive para um cliente móvel, o servidor compara a versão do cliente com a versão do plug-in. Se ambas as versões estiverem próximas o suficiente (significando que os três dígitos mais significativos no identificador de versão são idênticos), o Direct Update ocorrerá normalmente. Caso contrário, o servidor Mobile Foundation ignorará silenciosamente a atualização. Uma solução para a incompatibilidade de versão é fazer download do *cordova-plugin-mfp* com a mesma versão que aquela do seu projeto Cordova original e gerar novamente o archive da atualização direta.
{: tip}

1. Abra uma janela de linha de comandos e navegue para a raiz do projeto Cordova.
2. Execute o comando:
  ```bash
  Webupdate app mfpdev
  ```
  O comando `mfpdev app webupdate` empacota os recursos da web atualizados em um arquivo `.zip` e faz upload dele para o servidor Mobile Foundation padrão em execução na estação de trabalho do desenvolvedor. Os recursos da web empacotados podem ser localizados na pasta `[cordova-project-root-folder]/mobilefirst/`.

Para obter etapas alternativas para atualizar o conteúdo da web em seu app, veja [aqui](update_web_content_in_app_alternate_steps.html).

## Configuração avançada do Direct Update
{: #advanced_direct_update_config}

Para tópicos avançados na configuração de Direct Update, veja [aqui](update_web_content_in_app_advanced.html).
