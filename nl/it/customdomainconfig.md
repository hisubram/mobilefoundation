---

copyright:
  years: 2016, 2018
lastupdated:  "2018-11-19"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock:  .codeblock}

# Configurazione del dominio personalizzato per il server Mobile Foundation
{: #configcustomdomain}

{{site.data.keyword.mobilefoundation_short}} crea un {{site.data.keyword.mfserver_short_notm}}, che è accessibile utilizzando un URL con i nomi di dominio basati sulla **Regione** {{site.data.keyword.Bluemix_notm}}. Puoi anche configurare il tuo dominio personalizzato.
{: shortdesc}

L'URL o la rotta sono creati con i nomi del dominio predefiniti basati sulla `Regione` {{site.data.keyword.Bluemix_notm}}.

  |Dominio |  Regione  |    
  |:----- | :----- |    
  |`mybluemix.net` | Stati Uniti Sud |    
  |`eu-gb.mybluemix.net` | Regno Unito  |
  |`au-syd.mybluemix.net` | Sydney  |   
  |`eu-de.mybluemix.net` | Francoforte |   
  {: caption="Tabella 1. Nomi dominio dell'applicazione basati sulla regione in {{site.data.keyword.Bluemix_notm}}" caption-side="top"}

Per poter utilizzare il tuo proprio dominio, dovrai configurare il dominio personalizzato completando la seguente procedura:

1.	Crea un'istanza {{site.data.keyword.mfserver_short_notm}} creando l'istanza del servizio {{site.data.keyword.mobilefoundation_short}} scegliendo uno dei piani supportati.

+ Aggiungi il dominio personalizzato che vuoi utilizzare alla tua `Organizzazione` {{site.data.keyword.Bluemix_notm}}. Per aggiungere il tuo dominio, vai a **Gestisci organizzazioni > DOMINI > AGGIUNGI DOMINIO**.

+ Configura una rotta in modo che il server utilizzi il tuo dominio personalizzato.

+ Vai al provider DNS del tuo dominio e aggiungi una voce CNAME, che instraderà il traffico dal tuo dominio alla rotta {{site.data.keyword.Bluemix_notm}} predefinita, in cui è in esecuzione il server.

+ Se vuoi configurare `https` per il dominio personalizzato, carica il certificato SSL per il tuo dominio in {{site.data.keyword.Bluemix_notm}}. Per farlo, vai a **Gestisci organizzazioni > DOMINI**, seleziona il dominio personalizzato per cui desideri configurare il certificato SSL e fai clic su **Carica certificato** per caricare il certificato SSL per il tuo dominio. Fai riferimento a [questo post ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://developer.ibm.com/bluemix/2014/09/28/ssl-certificates-bluemix-custom-domains/){: new_window}, per ulteriori informazioni.
