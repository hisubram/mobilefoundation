---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-10"

keywords: mobile foundation faq, updates to mobile foundation, custom domain

subcollection:  mobilefoundation
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:faq: data-hd-content-type='faq'}
{:note: .note}

# Perguntas Mais Freqüentes
{: #mfp-faq}

Esta Pergunta mais frequente fornece respostas às perguntas comuns sobre o serviço {{site.data.keyword.mobilefoundation_long}}.
{: shortdesc}

## Como eu sei quando as atualizações para o serviço do {{site.data.keyword.mobilefoundation_short}} são disponibilizadas?
{: #maintupdates_mf}
{: faq}

O {{site.data.keyword.mobilefoundation_short}} cria um {{site.data.keyword.mfserver_short_notm}}. As atualizações no servidor do {{site.data.keyword.mobilefoundation_short}} são notificadas aos usuários. Uma notificação é mostrada no painel da instância de serviço. O usuário pode escolher aplicar a atualização ao {{site.data.keyword.mobilefoundation_short}} durante uma janela de manutenção que é decidida pelo usuário. É possível escolher atualizar o servidor {{site.data.keyword.mobilefoundation_short}} quando for conveniente para você.

A atualização de serviço do {{site.data.keyword.mobilefoundation_short}} é disponibilizada quando um dos componentes a seguir é atualizado.

* {{site.data.keyword.mfserver_short_notm}}.
* Versão do Liberty subjacente.
* Versão do Java Developer Kit subjacente.

## Como eu aplico as atualizações no serviço do {{site.data.keyword.mobilefoundation_short}}?
{: #apply_update_mf}
{: faq}

A atualização para o {{site.data.keyword.mobilefoundation_short}} pode ser aplicada ao clicar em **Recriar**.
Ao aplicar a atualização, a versão do servidor, conforme visto no {{site.data.keyword.mfp_oc_short_notm}}, é modificada para indicar a versão de atualização do servidor.

* Os usuários não podem aplicar suas próprias correções e atualizações à sua instância de serviço do {{site.data.keyword.mobilefoundation_short}}.
* Consulte [Recriando o servidor no plano Professional Per Device](/docs/services/mobilefoundation?topic=mobilefoundation-using_mobilefoundation_p5#recreate_mobilefoundation_p5) e [Recriando o servidor no plano Professional 1 Application](/docs/services/mobilefoundation?topic=mobilefoundation-using_mobilefoundation_p2#recreate_mobilefoundation_p2) para entender a diferença no comportamento entre os planos quando se clica em **Recriar**.
{: note}

## Como eu configuro o domínio customizado para a minha instância de servidor do {{site.data.keyword.mobilefoundation_short}}?
{: #configcustomdomain}
{: faq}

{{site.data.keyword.mobilefoundation_short}} provisões um {{site.data.keyword.mfserver_short_notm}}, que é acessível usando uma URL com os nomes de domínio com base na {{site.data.keyword.Bluemix_notm}} **Região**. Também é possível configurar seu próprio domínio customizado.

A URL ou rota é criada com os nomes de domínio padrão baseados na `Região` do {{site.data.keyword.Bluemix_notm}}.

  |Domínio |  Região  |    
  |:----- | :----- |    
  |`mybluemix.net` | Sul dos EUA |    
  |`eu-gb.mybluemix.net` | Reino Unido  |
  |`au-syd.mybluemix.net` | Sydney  |   
  |`eu-de.mybluemix.net` | Frankfurt |   
  {: caption="Tabela 1. Nomes de domínio de aplicativo baseados na Região no {{site.data.keyword.Bluemix_notm}}" caption-side="top"}

Para usar seu próprio domínio, é necessário configurar o domínio customizado executando as etapas a seguir:

1.	Crie uma instância {{site.data.keyword.mfserver_short_notm}} criando a instância de serviço {{site.data.keyword.mobilefoundation_short}} escolhendo um dos planos suportados.

+ Inclua o domínio customizado que você gostaria de usar na `Organization` do {{site.data.keyword.Bluemix_notm}}. Acesse **Gerenciar organizações > DOMÍNIOS > INCLUIR DOMÍNIO** para incluir seu próprio domínio.

+ Configure uma rota para o servidor usar seu domínio customizado.

+ Acesse o provedor DNS para seu domínio e inclua uma entrada CNAME, que roteia o tráfego de seu domínio para a rota padrão do {{site.data.keyword.Bluemix_notm}}, em que o servidor está em execução.

+ Se quiser configurar `https` para seu domínio customizado, faça upload do certificado SSL para seu domínio no {{site.data.keyword.Bluemix_notm}}. Para fazer upload do certificado SSL, acesse **Gerenciar organizações > DOMÍNIOS**, selecione o domínio customizado para o qual você deseja configurar o certificado SSL, clique em **Fazer upload do certificado** para fazer upload do certificado SSL para seu domínio. Para obter mais informações, consulte [este post ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://developer.ibm.com/bluemix/2014/09/28/ssl-certificates-bluemix-custom-domains/){: new_window}.
