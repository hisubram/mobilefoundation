---

copyright:
  years: 2016, 2018
lastupdated:  "2018-11-19"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock:  .codeblock}

# Configurando domínio customizado para o servidor do Mobile Foundation
{: #configcustomdomain}

O {{site.data.keyword.mobilefoundation_short}} cria um {{site.data.keyword.mfserver_short_notm}}, que é acessível usando uma URL com os nomes de domínio baseados na **Região** do {{site.data.keyword.Bluemix_notm}}. Também é possível configurar seu próprio domínio customizado.
{: shortdesc}

A URL ou rota é criada com os nomes de domínio padrão baseados na `Região` do {{site.data.keyword.Bluemix_notm}}.

  |Domínio |  Região  |    
  |:----- | :----- |    
  |`mybluemix.net` | Sul dos EUA |    
  |`eu-gb.mybluemix.net` | Reino Unido  |
  |`au-syd.mybluemix.net` | Sydney  |   
  |`eu-de.mybluemix.net` | Frankfurt |   
  {: caption="Tabela 1. Nomes de domínio de aplicativo baseados na Região no {{site.data.keyword.Bluemix_notm}}" caption-side="top"}

Para poder usar seu próprio domínio, será necessário configurar o domínio customizado executando as etapas a seguir:

1.	Crie uma instância {{site.data.keyword.mfserver_short_notm}} criando a instância de serviço {{site.data.keyword.mobilefoundation_short}} escolhendo um dos planos suportados.

+ Inclua o domínio customizado que você gostaria de usar na `Organization` do {{site.data.keyword.Bluemix_notm}}. Acesse **Gerenciar organizações > DOMÍNIOS > INCLUIR DOMÍNIO** para incluir seu próprio domínio.

+ Configure uma rota para o servidor usar seu domínio customizado.

+ Acesse o provedor DNS de seu domínio e incluir uma entrada CNAME, que roteará o tráfego de seu domínio para a rota padrão do {{site.data.keyword.Bluemix_notm}} em que o servidor está em execução.

+ Se quiser configurar `https` para seu domínio customizado, faça upload do certificado SSL para seu domínio no {{site.data.keyword.Bluemix_notm}}. Para fazer isso, acesse **Gerenciar organizações > DOMÍNIOS**, selecione o domínio customizado para o qual você deseja configurar o certificado SSL, clique em **Fazer upload de certificado** para fazer upload do certificado SSL para seu domínio. Consulte [este post ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://developer.ibm.com/bluemix/2014/09/28/ssl-certificates-bluemix-custom-domains/){: new_window} para obter mais informações.
