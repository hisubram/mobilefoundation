---

copyright:
  years: 2016, 2018
lastupdated:  "2018-02-14"

---

#	Utilizzo del piano Developer
{: #using_mobilefoundation_p1}

Dopo aver creato l'istanza del servizio {{site.data.keyword.mobilefoundation_short}} utilizzando il piano Developer, accedi alla pagina della panoramica in {{site.data.keyword.Bluemix_notm}}. In questa pagina, ti vengono forniti i video e le esercitazioni per aiutarti a iniziare a utilizzare il servizio.

## Avvio del server MobileFirst
{: #start_mobilefoundation_p1}
* Per avviare {{site.data.keyword.mfserver_short_notm}} con le impostazioni predefinite, fai clic su **Avvia server di base**.

  Questa selezione fornisce un {{site.data.keyword.mfserver_long_notm}} con le seguenti impostazioni:
  *	1 GB di memoria. Questa dimensione è sufficiente per lo sviluppo, per delle attività leggere di test e i carichi di lavoro di produzione su piccola scala.

  *	Il `nome utente` e la `password` ti vengono generati
automaticamente. Disporrai dell'accesso ad essi quando il server è avviato e in esecuzione.

  Viene avviato il processo di provisioning. Questo processo impiega circa 10 minuti e una finestra di messaggio
indica l'avanzamento di questa operazione. Al suo completamento, viene visualizzato un dashboard
dove puoi vedere:
    *	Lo stato del tuo server in esecuzione (stato, dimensione).

    *	La rotta del server creata per te. Utilizza questa rotta nella tua applicazione mobile per collegarti a {{site.data.keyword.mfserver_short_notm}}.

    *	I tuoi `nomeutente` e `password` personali per accedere a {{site.data.keyword.mfp_oc_short_notm}}. La `password` è nascosta. Fai clic sull'icona **Mostra password** per visualizzarla.

*	Fai clic su **Avvia console** per avviare {{site.data.keyword.mfp_oc_short_notm}}.

Con la console, puoi gestire le tue applicazioni mobili e i tuoi dispositivi mobili, utilizzare il tuo server come un backend mobile, inviare notifiche di push e altro ancora.

##  Aggiunta del servizio Mobile Analytics 
{: #adding_analytics_server_dev}

 Puoi ora monitorare la tua applicazione mobile sul server {{site.data.keyword.mobilefirst}} aggiungendo un servizio Mobile Analytics all'istanza del servizio {{site.data.keyword.mobilefoundation_short}}. Il piano Developer crea il servizio Mobile Analytics in un gruppo di contenitori con un solo nodo con 1 GB di memoria. 

* Fai clic su **Aggiungi Analytics** per aggiungere il servizio Mobile Analytics all'istanza del servizio {{site.data.keyword.mobilefoundation_short}}. 

  Viene avviato il processo di provisioning. Questo processo impiega circa 10 minuti e una finestra di messaggio
indica l'avanzamento di questa operazione.  

* Avvia la console del servizio Mobile Analytics da {{site.data.keyword.mfp_oc_short_notm}}. 

* SSO (single sign-on) è abilitato tra {{site.data.keyword.mfserver_short_notm}} e il servizio Mobile Analytics. Il servizio Mobile Analytics è configurato con le stesse credenziali utente e chiavi LTPA di {{site.data.keyword.mfserver_short_notm}}. Puoi utilizzare gli stessi `nomeutente` e `password` per accedere alla console di Mobile Analytics utilizzati per accedere a {{site.data.keyword.mfp_oc_short_notm}}.

Per ulteriori informazioni su Mobile Analytics, puoi fare riferimento a [MobileFirst Foundation Operational Analytics ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/analytics/){: new_window}.

> **Nota:** l'eliminazione dell'istanza del servizio {{site.data.keyword.mobilefoundation_short}} o il ricreare {{site.data.keyword.mfserver_short_notm}} rimuove l'istanza del servizio Mobile Analytics.

##  Eliminazione del servizio Mobile Analytics 
{: #deleting_analytics_server_dev}

Puoi ora eliminare il servizio Mobile Analytics che è stato aggiunto all'istanza del servizio {{site.data.keyword.mobilefoundation_short}},
dal dashboard del servizio {{site.data.keyword.mobilefoundation_short}}. 

* Fai clic su **Elimina Analytics** per eliminare il servizio Mobile Analytics che è stato aggiunto all'istanza del servizio {{site.data.keyword.mobilefoundation_short}}.

 Facendo clic su **Elimina Analytics** elimini l'istanza del server di analisi. Il processo di eliminazione dell'istanza di analisi dura circa 10 minuti. Puoi aggiornare la schermata per visualizzare lo stato di aggiornamento. L'eliminazione dell'istanza di analisi riabilita il pulsante **Aggiungi Analytics**. Se scegli di aggiungere nuovamente il servizio Mobile Analytics, puoi fare clic sul questo pulsante.


## Ricreazione del server MobileFirst
{: #recreate_mobilefoundation_p1}

*	Fai clic su **Ricrea** per ricreare il server.

* Questa azione arresta il tuo server esistente e elimina i dati. Tutti i dati nel tuo server mobile vengono persi. Viene creata una nuova istanza del server con una versione aggiornata, se disponibile. Il completamento di questa azione richiede
alcuni minuti.

##	Impostazione della configurazione avanzata
{: #using_mfs_advanced_p1}

Utilizza **Avvia server con la configurazione avanzata** dalla pagina `Panoramica` per creare il server con le impostazioni avanzate o personalizzate. Puoi inoltre
aggiornare le impostazioni del server per personalizzare la tua configurazione server facendo clic sulla scheda
**Configurazione**. {{site.data.keyword.mobilefoundation_short}} ti dà l'accesso ad alcune impostazioni avanzate.

*	Dalla scheda **Topologia**, puoi selezionare la dimensione del server e il numero di istanze di cui hai bisogno. Il server da 1 GB predefinito è sufficiente per lo sviluppo e una moderata attività di test.

  - Seleziona la dimensione corretta per il tuo server sulla base delle tue necessità.

* **Nodi** visualizza il numero di nodi creati. Questo campo non è modificabile in {{site.data.keyword.mobilefoundation_short}}: Developer. Il numero di nodi in <!--in your {{site.data.keyword.IBM_notm}} container group--> è predefinito su **1** nel piano Developer.

Fai riferimento alla documentazione di [{{site.data.keyword.mobilefoundation_long}} ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://www.ibm.com/support/knowledgecenter/SSHS8R_8.0.0/wl_welcome.html){: new_window} per ulteriori dettagli.
