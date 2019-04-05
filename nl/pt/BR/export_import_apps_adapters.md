---

copyright:
  years: 2018
lastupdated:  "2018-08-09"

keywords: export apps, adapters export

subcollection:  mobilefoundation
---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:tip: .tip}
{:pre: .pre}

# Exportar e importar aplicativos e adaptadores
{: #export_import_apps_adapters}

No {{site.data.keyword.mfp_oc_short_notm}}, sob determinadas condições, é possível exportar um aplicativo ou uma de suas versões e, posteriormente, importá-lo para um servidor diferente. Também é possível exportar e reimportar adaptadores. Use esse recurso para propósitos de reutilização ou backup ou para migrar entre as instâncias do {{site.data.keyword.mfserver_short_notm}}.

Se você tiver concedido a função de administrador **mfpadmin** e a função de implementador **mfpdeployer**, será possível exportar uma versão ou todas as versões de um aplicativo. O aplicativo ou a versão é exportada como um arquivo compactado `.zip`, que salva o *ID do aplicativo*, *descritores*, *dados de autenticidade* e *recursos da web*. É possível importar posteriormente o archive para reimplementar o aplicativo ou a versão para o mesmo ou para um servidor diferente.

Considere cuidadosamente o seu caso de uso:
* O arquivo de exportação inclui os dados de autenticidade do aplicativo. Esses dados são específicos para a construção de um app móvel. O app móvel inclui a URL do servidor. Portanto, se você deseja usar outro servidor, deve-se reconstruir o app. A transferência somente dos arquivos de app exportados não funcionaria.
* Alguns artefatos podem variar de um servidor para outro. As credenciais de push são diferentes, dependendo se você trabalha em um ambiente de desenvolvimento ou de produção.
* A configuração de tempo de execução do aplicativo (que contém o estado ativo ou desativado e os perfis de log) pode ser transferida em alguns casos, mas não em todos.
* A transferência de recursos da web pode não fazer sentido em alguns casos, por exemplo, quando você reconstrói o app para usar um novo servidor.
{: tip}

##  Pré-requisito
{: #prereq}

Ativar o {{site.data.keyword.mfp_oc_short_notm}}.

##  Procedimento
{: #procedure}

1.  Na barra lateral de navegação, selecione um aplicativo ou uma versão de aplicativo ou um adaptador.

2.  Selecione **Ações > Exportar aplicativo** ou **Exportar versão** ou **Exportar adaptador**.
     Você será solicitado a salvar o archive `.zip` que encapsula os recursos exportados. O aspecto da caixa de diálogo depende de seu navegador e a pasta de destino depende de suas configurações do navegador.

3.   Salve o archive.
      O nome de archive inclui o nome e a versão do aplicativo ou do adaptador, por exemplo, `export_applications_com.sample.zip`.

4.   Para reutilizar um archive de exportação existente, selecione **Ações > Importar aplicativo** ou **Importar versão** ou **Importar adaptador**, navegue para o archive e clique em **Implementar**.
      O quadro do console principal exibe os detalhes do aplicativo ou adaptador importado.

##    Resultados
{: #results}

Se você importar para o mesmo servidor, o aplicativo ou versão não será necessariamente restaurado, pois ele foi exportado. Ou seja, a reimplementação no tempo de importação não remove as modificações subsequentes. Em vez disso, se alguns recursos de aplicativo forem modificados entre o tempo de exportação e a reimplementação no tempo de importação, somente os recursos que estão incluídos no archive exportado serão reimplementados em seu estado original.
<br/>
Por exemplo, se você exportar um aplicativo sem dados de autenticidade, em seguida fizer upload de dados de autenticidade e, em seguida, importar o archive inicial, os dados de autenticidade não serão apagados.
