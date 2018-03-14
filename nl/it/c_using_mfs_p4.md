---

copyright:
  years: 2016, 2018
lastupdated:  "2018-02-14"

---

#	Utilizzo del piano Professional Per Capacity
{: #using_mobilefoundation_p4}

Con il piano Professional Per Capacity gli utenti possono creare qualsiasi numero di applicazioni mobili con più sistemi operativi mobili.
Dopo che hai creato l'istanza del servizio {{site.data.keyword.mobilefoundation_short}}: Professional Per Capacity, leggi la seguente procedura introduttiva al servizio.

## Prerequisiti
{: #prerequisites_p4}

Tieni conto di quanto segue, prima di configurare l'istanza del servizio {{site.data.keyword.mobilefoundation_short}}: Professional Per Capacity
* {{site.data.keyword.mobilefoundation_short}}: Professional Per Capacity è supportato solo con i piani {{site.data.keyword.Db2_on_Cloud_short}} {{site.data.keyword.Bluemix_notm}}.

* Devi avere accesso alle credenziali dell'istanza del servizio {{site.data.keyword.Db2_on_Cloud_short}} prima di poter configurare le impostazioni della tua istanza del servizio {{site.data.keyword.mobilefoundation_short}}.

> **Nota**: l'istanza del servizio {{site.data.keyword.Db2_on_Cloud_short}} può esistere in qualsiasi `Spazio` nella tua `Organizzazione` {{site.data.keyword.Bluemix_notm}} o in qualsiasi altra `Organizzazione` a cui hai accesso. Assicurati di disporre delle autorizzazioni per accedere allo `Spazio` dove è presente l'istanza del servizio {{site.data.keyword.Db2_on_Cloud_short}}.


## Aggiunta della connessione al database
{: #configure_dashdb_p4}

###  Prime operazioni
{: #firststeps_p4}

Dopo che hai creato l'istanza del servizio {{site.data.keyword.mobilefoundation_short}}: Professional Per Capacity, completa la seguente procedura per iniziare a utilizzarla.

### Impostazione della connessione all'istanza del servizio Db2 on Cloud
{: #connect_dashdb_p4}

Dopo che l'istanza del servizio {{site.data.keyword.mobilefoundation_short}}: Professional Per Capacity è stata creata, vedrai la pagina *Panoramica*
dove dovrai specificare le informazioni di connessione per l'istanza del servizio {{site.data.keyword.Db2_on_Cloud_short}},
a cui dovrebbe collegarsi l'istanza del servizio {{site.data.keyword.mobilefoundation_short}}. 

Puoi anche creare una nuova istanza del servizio {{site.data.keyword.Db2_on_Cloud_short}}, se non ne hai una già esistente.

Utilizza la seguente procedura per creare una nuova istanza del servizio Db2 on Cloud:

1. Nella pagina *Panoramica* seleziona la sezione **Crea nuovo servizio**.

+ Seleziona `Sì` per l'opzione **Configurazione alta disponibilità **,
se desideri l'alta disponibilità per l'istanza del servizio {{site.data.keyword.Db2_on_Cloud_short}}.

+ Controlla i dettagli del piano e fai clic su **Crea**.

Viene creata una nuova istanza del servizio {{site.data.keyword.Db2_on_Cloud_short}} che fornisce un'istanza
{{site.data.keyword.Db2_on_Cloud_short}} dedicata con 8GB di RAM e 2 vCPU e 500 GB di archiviazione. 

Utilizza la seguente procedura per stabilire una connessione a un'istanza del servizio {{site.data.keyword.Db2_on_Cloud_short}} esistente o all'istanza del servizio {{site.data.keyword.Db2_on_Cloud_short}} che hai appena creato:

1. Seleziona l'`Organizzazione` {{site.data.keyword.Bluemix_notm}} dove è presente l'istanza del servizio {{site.data.keyword.Db2_on_Cloud_short}}.

+ Seleziona lo `Spazio`  {{site.data.keyword.Bluemix_notm}} in cui è presente l'istanza del servizio {{site.data.keyword.Db2_on_Cloud_short}}, dall'elenco di spazi disponibili nell'`Organizzazione` selezionata.   
> **Nota:** se non vedi elencati l'`Organizzazione` e lo `Spazio` in cui è presente l'istanza del servizio {{site.data.keyword.Db2_on_Cloud_short}} controlla di essere un membro di tali `Organizzazione` e `Spazio`. Devi avere l'accesso al ruolo di *Sviluppatore* per l'organizzazione e lo spazio, in modo che il servizio {{site.data.keyword.mobilefoundation_short}} acceda alle credenziali dal servizio {{site.data.keyword.Db2_on_Cloud_short}}.

+ Seleziona il `Nome servizio` e le `Credenziali` {{site.data.keyword.Db2_on_Cloud_short}} per connetterti all'istanza del servizio {{site.data.keyword.Db2_on_Cloud_short}} esistente.

+  Verifica la connessione all'istanza del servizio {{site.data.keyword.Db2_on_Cloud_short}} specificata.

+  Fai clic su **Aggiungi**. Questa azione crea le tabelle richieste nell'istanza del servizio database {{site.data.keyword.Db2_on_Cloud_short}} configurato.

Dopo pochi secondi, puoi accedere alla pagina `Panoramica` che ti fornisce le esercitazioni e i video per aiutarti a iniziare a lavorare con il servizio  {{site.data.keyword.mobilefoundation_short}}.

> **Nota**: non puoi modificare l'istanza del servizio {{site.data.keyword.Db2_on_Cloud_short}} configurata per essere utilizzata dalla tua istanza del servizio {{site.data.keyword.mobilefoundation_short}}. Tuttavia, puoi utilizzare la stessa istanza del servizio {{site.data.keyword.Db2_on_Cloud_short}} tra più istanze del servizio {{site.data.keyword.mobilefoundation_short}}, poiché ogni istanza di {{site.data.keyword.mobilefoundation_short}} crea il proprio schema nell'istanza del servizio  {{site.data.keyword.Db2_on_Cloud_short}} selezionata.

## Avvio del server MobileFirst
{: #start_mobilefoundation_p4}

* Per avviare {{site.data.keyword.mfserver_short_notm}}, con le impostazioni predefinite, fai clic su **Avvia server di base**.

* Questa selezione fornisce un {{site.data.keyword.mfserver_long_notm}} con le seguenti impostazioni:
    -  2 nodi con 1 GB di memoria ognuno. Questa dimensione è buona per lo sviluppo, per delle attività moderate di test e i carichi di lavoro di produzione su piccola scala.

    -	Il `nome utente` e la `password` ti vengono generati automaticamente. Disporrai dell'accesso ad essi quando il server è avviato e in esecuzione.

Viene avviato il processo di provisioning del tuo server. Questo processo impiega circa 10 minuti e una finestra di messaggio
indica l'avanzamento di questa operazione. Al suo completamento, viene visualizzato un dashboard
dove puoi vedere:

  -	Lo stato del tuo server in esecuzione (stato, dimensione).

  -	La rotta del server viene creata per te. Utilizza questa rotta nella tua applicazione mobile per collegarti a {{site.data.keyword.mfserver_short_notm}}.

  -	I tuoi `nomeutente` e `password` personali per accedere a {{site.data.keyword.mfp_oc_short_notm}}. La `password` è nascosta. Fai clic sull'icona **Mostra password** per visualizzarla.

*	Fai clic su **Avvia console** per aprire {{site.data.keyword.mfp_oc_short_notm}}.


<!--This console runs inside the container.--> Con la console, puoi gestire le tue applicazioni mobili, gli adattatori e i tuoi dispositivi mobili, utilizzare il tuo server come un backend mobile, inviare notifiche di push e altro ancora.

##  Aggiunta del servizio Mobile Analytics
{: #adding_analytics_server_p4}

 Puoi ora monitorare la tua applicazione mobile sul server {{site.data.keyword.mobilefirst}} aggiungendo un'istanza del servizio Mobile Analytics all'istanza {{site.data.keyword.mobilefoundation_short}}.

 <!--The Professional plan creates the Mobile Analytics service in a container group, the user can customize the configuration by selecting the number of container nodes in the container group.

 Users can also attach volumes to the containers to persist data. The volume once selected cannot be changed. 20 GB is the default file share space available to the user. If the user needs additional storage space to persist analytics data, he is required to buy additional file share and create a volume using this file share. He can then select this new volume while deploying the analytics server.

 For more information on adding volumes to {{site.data.keyword.containerlong}}, refer to [Storing persistent data in a volume by using the {{site.data.keyword.Bluemix_notm}} Dashboard ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://console.ng.bluemix.net/docs/containers/container_volumes_ov.html#container_volumes_ui.html){: new_window}.-->

* Fai clic su **Aggiungi Analytics** per creare e aggiungere un'istanza del servizio Mobile Analytics all'istanza {{site.data.keyword.mobilefoundation_short}}.

<!--* You can choose the Mobile Analytics service configuration, the minimum supported configuration for the Analytics server is 2 nodes with 1 GB memory each, you can choose to create an Analytics server up to a maximum configuration of 32 nodes with 16 GB memory each.-->

Viene avviato il processo di provisioning. Questo processo dura alcuni minuti e un indicatore di avanzamento visualizza l'avanzamento di questa operazione.   

* Avvia la console del servizio Mobile Analytics da {{site.data.keyword.mfp_oc_short_notm}}.

* SSO (single sign-on) è abilitato tra {{site.data.keyword.mfserver_short_notm}} e il servizio Mobile Analytics. Il servizio Mobile Analytics è configurato con le stesse credenziali utente e chiavi LTPA del server {{site.data.keyword.mfserver_short_notm}}. Puoi utilizzare gli stessi `nomeutente` e `password` per accedere alla console di Mobile Analytics utilizzati per accedere a {{site.data.keyword.mfp_oc_short_notm}}.

Per ulteriori informazioni su Mobile Analytics, puoi fare riferimento a [MobileFirst Foundation Operational Analytics ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/analytics/){: new_window}.

> **Nota:** l'eliminazione dell'istanza del servizio {{site.data.keyword.mobilefoundation_short}} rimuove l'istanza del servizio Mobile Analytics.

##  Eliminazione del servizio Mobile Analytics
{: #deleting_analytics_server_p4}

Puoi ora eliminare il servizio Mobile Analytics che è stato aggiunto all'istanza del servizio {{site.data.keyword.mobilefoundation_short}},
dal dashboard del servizio {{site.data.keyword.mobilefoundation_short}}. 

* Fai clic su **Elimina Analytics** per eliminare il servizio Mobile Analytics che è stato aggiunto all'istanza del servizio {{site.data.keyword.mobilefoundation_short}}.

 Facendo clic su **Elimina Analytics** elimini l'istanza del server di analisi. Il processo di eliminazione dell'istanza di analisi dura circa 10 minuti. Puoi aggiornare la schermata per visualizzare lo stato di aggiornamento. L'eliminazione dell'istanza di analisi riabilita il pulsante **Aggiungi Analytics**. Se scegli di aggiungere nuovamente il servizio Mobile Analytics, puoi fare clic sul questo pulsante.

## Ricreazione del server MobileFirst
{: #recreate_mobilefoundation_p4}

*	Fai clic su **Ricrea** per ricreare il server.

* Questa azione arresta il tuo server esistente e elimina i dati. Viene creata una nuova istanza del server con una versione aggiornata, se disponibile. Il completamento di questa azione richiede
alcuni minuti.

> **Nota**: i dati dalla tua istanza del server precedente, comprese le informazioni sulle applicazioni e sugli adattatori, sono memorizzati in modo persistente nell'istanza del servizio {{site.data.keyword.Db2_on_Cloud_short}} configurata. Questi dati saranno utilizzati per ricreare il tuo server.

##	Impostazione della configurazione avanzata
{: #using_mfs_advanced_p4}

Utilizza **Avvia server con la configurazione avanzata** dalla pagina `Panoramica` per creare il server con le impostazioni avanzate o personalizzate. Puoi inoltre
aggiornare le impostazioni del server per personalizzare la tua configurazione server facendo clic sulla scheda **Impostazioni**. {{site.data.keyword.mobilefoundation_short}} ti dà l'accesso ad alcune impostazioni avanzate.

*	Dalla scheda **Topologia**, puoi selezionare la dimensione del server e il numero di istanze del server di cui hai bisogno. Il server da 1 GB predefinito è sufficiente per lo sviluppo e test semplici.
  - Seleziona la dimensione corretta per il tuo server sulla base delle tue necessità.

  - **Istanze** visualizza il numero di istanze create. 

      <!--- {{site.data.keyword.mobilefirst}} server farm can be created by configuring the number of nodes here. The minimum supported configuration is 2 nodes with 1 GB memory each and the maximum supported configuration is 32 nodes with 16 GB memory each.-->

Consulta la [documentazione {{site.data.keyword.mobilefoundation_long}} ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://www.ibm.com/support/knowledgecenter/SSHS8R_8.0.0/wl_welcome.html){: new_window}, per ulteriori dettagli.
