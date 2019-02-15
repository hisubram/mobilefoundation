---

copyright:
  years: 2018, 2019
lastupdated:  "2018-11-16"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:faq: data-hd-content-type='faq'}

# Perguntas Mais Freqüentes

Esta Pergunta mais frequente fornece respostas às perguntas comuns sobre o serviço {{site.data.keyword.mobilefoundation_long}}.
{: shortdesc}

## Como eu sei quando as atualizações para o serviço do {{site.data.keyword.mobilefoundation_short}} são disponibilizadas?
{: #maintupdates_mf}
{: faq}

O {{site.data.keyword.mobilefoundation_short}} cria um {{site.data.keyword.mfserver_short_notm}}. As atualizações no servidor do {{site.data.keyword.mobilefoundation_short}} são notificadas aos usuários. Uma notificação será mostrada no painel da instância de serviço. O usuário pode escolher aplicar a atualização ao {{site.data.keyword.mobilefoundation_short}} durante uma janela de manutenção que é decidida pelo usuário. É possível escolher atualizar o servidor {{site.data.keyword.mobilefoundation_short}} quando for conveniente para você.

A atualização de serviço do {{site.data.keyword.mobilefoundation_short}} ficará disponível quando um dos componentes a seguir for atualizado.

* {{site.data.keyword.mfserver_short_notm}}.
* Versão do Liberty subjacente.
* Versão do Java Developer Kit subjacente.

## Como eu aplico as atualizações no serviço do {{site.data.keyword.mobilefoundation_short}}?
{: #apply_update_mf}
{: faq}

A atualização para o {{site.data.keyword.mobilefoundation_short}} pode ser aplicada ao clicar em **Recriar**.
Ao aplicar esta atualização, a versão do servidor, conforme visto no {{site.data.keyword.mfp_oc_short_notm}}, será modificada para indicar a versão da atualização do servidor.

> **Nota:**
>  * Os usuários não serão capazes de aplicar suas próprias correções e atualizações em sua instância de serviço do {{site.data.keyword.mobilefoundation_short}}.
>  * Consulte [Recriando o servidor no plano Professional Per Device](c_using_mfs_p5.html#recreate_mobilefoundation_p5) e [Recriando o servidor no plano Professional 1 Application](c_using_mfs_p2.html#recreate_mobilefoundation_p2) para entender a diferença no comportamento entre os planos quando **Recriar** é clicado.
>

## Como eu configuro o domínio customizado para a minha instância de servidor do {{site.data.keyword.mobilefoundation_short}}?
{: #configcustomdomain}
{: faq}

O {{site.data.keyword.mobilefoundation_short}} provisiona um {{site.data.keyword.mfserver_short_notm}}, que é acessível usando uma URL com os nomes de domínio baseados na **Região** do {{site.data.keyword.Bluemix_notm}}. Também é possível configurar seu próprio domínio customizado.

A URL ou rota é criada com os nomes de domínio padrão baseados na `Região` do {{site.data.keyword.Bluemix_notm}}.

  |Domínio |  Região  |    
  |:----- | :----- |    
  |`mybluemix.net` | Sul dos EUA |    
  |`eu-gb.mybluemix.net` | Reino Unido  |
  |`au-syd.mybluemix.net` | Sydney  |   
  |`eu-de.mybluemix.net` | Frankfurt |   
  {: caption="Tabela 1. Nomes de domínio de aplicativo baseados na Região no {{site.data.keyword.Bluemix_notm}}" caption-side="top"}

Para usar seu próprio domínio, será necessário configurar o domínio customizado executando as etapas a seguir:

1.	Crie uma instância {{site.data.keyword.mfserver_short_notm}} criando a instância de serviço {{site.data.keyword.mobilefoundation_short}} escolhendo um dos planos suportados.

+ Inclua o domínio customizado que você gostaria de usar na `Organization` do {{site.data.keyword.Bluemix_notm}}. Acesse **Gerenciar organizações > DOMÍNIOS > INCLUIR DOMÍNIO** para incluir seu próprio domínio.

+ Configure uma rota para o servidor usar seu domínio customizado.

+ Acesse o provedor DNS de seu domínio e incluir uma entrada CNAME, que roteará o tráfego de seu domínio para a rota padrão do {{site.data.keyword.Bluemix_notm}}, em que o servidor está em execução.

+ Se quiser configurar `https` para seu domínio customizado, faça upload do certificado SSL para seu domínio no {{site.data.keyword.Bluemix_notm}}. Para fazer isso, acesse **Gerenciar organizações > DOMÍNIOS**, selecione o domínio customizado para o qual você deseja configurar o certificado SSL, clique em **Fazer upload de certificado** para fazer upload do certificado SSL para seu domínio. Consulte [este post ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://developer.ibm.com/bluemix/2014/09/28/ssl-certificates-bluemix-custom-domains/){: new_window} para obter mais informações.
