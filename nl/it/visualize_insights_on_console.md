---

copyright:
  years: 2018
lastupdated: "2018-11-22"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}

# Visualizza le informazioni approfondite sulla console
{: #visualize_insights_on_console}

Dalla MobileFirst Analytics Console, puoi visualizzare e configurare i report di analisi approfondita, gestire gli avvisi e visualizzare i log del client. Avvia la Analytics Console dalla Mobile Foundation Operations Console facendo clic su **Analytics Console** dalla navigazione a sinistra.

La Analytics Console viene avviata e viene visualizzato il dashboard predefinito. Se un'applicazione client ha già inviato i log e i dati di analisi al server, i report pertinenti vengono creati e visualizzati. Dal dashboard, puoi riesaminare i dati di analisi raccolti. Questi dati di analisi possono essere correlati ad arresti anomali delle applicazioni, alle sessioni delle applicazioni e al tempo di elaborazione del server. Puoi inoltre creare dei grafici personalizzati e gestire gli avvisi.

Oltre a una visualizzazione immediata della tua analisi delle applicazioni mobili, la funzione di analisi include la funzionalità per eseguire una ricerca semplice (raw) sui log del client, sui dati relativi agli arresti anomali del client acquisiti e qualsiasi dato supplementare da te esplicitamente fornito tramite le chiamate delle funzioni API del client che passano dati in Mobile Analytics.

## Monitoraggio dei dati delle applicazioni
{: #monitoring_app_data}

Mobile Analytics fornisce il monitoraggio e l'analisi per le tue applicazioni mobili. Puoi registrare i log delle applicazioni e monitorare i dati con l'SDK client Mobile Analytics. Gli sviluppatori possono controllare quando inviare questi dati al servizio Mobile Analytics. Quando i dati vengono forniti a Mobile Analytics, puoi utilizzare la console Mobile Analytics per ottenere delle informazioni approfondite di analisi sui tuoi log dell'applicazione, sui tuoi dispositivi e sulle tue applicazioni mobili.

Alcune delle informazioni approfondite che è possibile derivare:

**Metriche predefinite**

Con le metriche predefinite rispondi a domande quali:
* Quanti nuovi utenti ho?
* Quante persone stanno attivamente utilizzando la mia applicazione?
* Con che frequenza le persone stanno utilizzando la mia applicazione?
* A che ora del giorno le persone stanno utilizzando la mia applicazione?
* Quali modelli di dispositivo preferiscono i miei utenti?
* Quando devo abbandonare il supporto per i sistemi operativi legacy?
* Per quali applicazioni si stanno riscontrando dei problemi delle prestazioni?

**Eventi personalizzati**

Aggiungendo i tuoi eventi personalizzati, puoi rispondere a domande quali:
* Quali funzioni sono utilizzate di più e quali di meno?
* Dove si trovano gli utenti che accedono ed escono dalla mia applicazione?
* Quali attività gli utenti stanno visualizzando maggiormente?
* Gli utenti stanno completando i flussi di lavoro nell'applicazione (ad esempio, i funnel di conversione)?

## Report che possono essere visualizzati: Users
{: #reports_visualized_users}

Questo report visualizza un grafico che mostra il numero di utenti attivi che hanno utilizzato l'applicazione entro uno specifico intervallo di date. Il report mostra anche il numero di nuovi utenti che sono utenti univoci che stanno utilizzando l'applicazione per la prima volta per l'intervallo di date specificato.
I grafici possono essere filtrati in base al nome dell'applicazione, al sistema operativo o alle versioni del sistema operativo.

## Report che possono essere visualizzati: Sessions
{: #reports_visualized_sessions}

Questo report visualizza un grafico che mostra le sessioni dell'applicazione per l'intervallo di date specificato. Viene registrata una sessione quando un'applicazione viene portata in primo piano in un dispositivo. I grafici possono essere filtrati in base al nome dell'applicazione, al sistema operativo o alle versioni del sistema operativo.

## Report che possono essere visualizzati: Network Requests
{: #reports_visualized_network_requests}

Visualizza i dati delle richieste di rete per le tue applicazioni nella console Mobile Analytics.

I dati sono disponibili per le seguenti misurazioni:

**Round Trip Time** - definisce il lasso di tempo, misurato in millisecondi, necessario perché la tua applicazione effettui le richieste di rete.
**Request Count** - visualizza quanto spesso un'applicazione effettua delle richieste di rete. I dati sono anche visualizzati come una media.
I grafici possono essere filtrati in base al nome dell'applicazione, al sistema operativo o alle versioni del sistema operativo.

## Report che possono essere visualizzati: Crashes
{: #reports_visualized_crashes}

Puoi visualizzare le informazioni sui tuoi arresti anomali dell'applicazione nella console Mobile Analytics per meglio monitorare e risolvere i problemi delle tue applicazioni.

Nella pagina Crashes, la tabella **Crash Overview** mostra le seguenti colonne di dati:

**Application**: il nome dell'applicazione<br/>
**Crashes**: numero totale di arresti anomali per tale applicazione<br/>
**Total Uses**: numero totale di volte per cui un utente apre e chiude tale applicazione<br/>
**Crash Rate**: percentuale di arresti anomali per ogni utilizzo<br/>
Puoi rapidamente visualizzare le informazioni sui tuoi arresti anomali dell'applicazione nella tabella Crashes. Il grafico a barre Crashes visualizza un istogramma degli arresti anomali nel tempo.<br/>

Puoi visualizzare i dati sugli arresti anomali in due modi:

1.  Display crash rate: frequenza degli arresti anomali nel tempo
2.  Display total crashes: numero totale di arresti anomali nel tempo

## Report che possono essere visualizzati: Troubleshooting
{: #reports_visualized_troubleshooting}

La pagina **Troubleshooting** nella console Mobile Analytics offre una vista dettagliata dei tuoi arresti anomali dell'applicazione utilizzando la tabella **Crash Summary**.

La tabella **Crash Summary** è ordinabile e include le seguenti colonne di dati:

* Crashes
* Devices
* Last Crash
* Application
* OS
* Message

Fai clic sull'icona + accanto a qualsiasi voce per visualizzare la tabella **Crash Details**, che include le seguenti colonne: 

* Time Crashed
* Application Version
* OS Version
* Device Model
* Device ID
* Download: link per scaricare i log che hanno portato all'arresto anomalo

Espandi qualsiasi voce nella tabella **Crash Details** per ottenere ulteriori dettagli, compresa una traccia di stack. 

I dati per la tabella **Crash Summary** vengono popolati eseguendo query dei log dell'applicazione del livello errore irreversibile. Se l'applicazione non raccoglie i log delle applicazioni degli errori irreversibili, non è disponibile alcun dato.
{: note}


## Report che possono essere visualizzati: User Feedback
{: #reports_visualized_userfeedback}

User Feedback fornisce l'analisi del feedback all'interno dell'applicazione utilizzando Mobile Analytics.
Con questa funzione di Mobile Analytics -
* **Gli utenti e i tester** possono registrare e inviare feedback e report dei bug dall'applicazione mentre eseguono e usano l'applicazione.
* **I proprietari dell'applicazione** ottengono una comprensione più approfondita dell'esperienza utente dell'applicazione con questo feedback dell'utente dal ricco contesto.
* **Gli sviluppatori**, tuttavia, ricevono dei contesti dell'applicazione accurati per diagnosticare e correggere i bug o le lacune delle funzioni.

### Abilitazione del feedback dell'utente
{: #enable_user_feedback}

Completa la seguente procedura per abilitare la tua applicazione mobile ad acquisire il feedback dell'utente.

#### Dota di strumentazione la tua applicazione
{: #instrument_app}

* Dota di strumentazione la tua applicazione mobile per entrare in modalità di feedback. Richiama l'API `Analytics.triggerFeedbackMode();` per richiamare la modalità di feedback. <!--For more information, refer to the documentation [here](instrument_an_app.html)-->.
* L'API può essere richiamata su qualsiasi evento dell'applicazione quali pulsanti, azioni del menu o gesti.

#### Ricevi il feedback degli utenti
{: #receive_feedback}

* Gli utenti e i tester della tua applicazione possono passare alla modalità di feedback attivando l'azione dell'applicazione dotata della strumentazione per il feedback.
* Dalla modalità di feedback, è possibile raccogliere e inviare a Mobile Analytics un feedback contestuale completo con un'acquisizione dello schermo.

#### Analizza il feedback dell'utente
{: #analyze_feedback}

* Mobile Analytics riceve e consolida il feedback contestuale completo inviato dalle applicazioni mobili.
* Accedi alla console Mobile Analytics e seleziona l'opzione **Feedback utente** per visualizzare il feedback.
* Un proprietario di applicazione può esaminare il feedback, aggiungere commenti e apporre una tag al feedback con uno stato di revisione. I commenti possono di norma essere azioni pianificate quali dei collegamenti a elementi di lavoro Git, che sono creati per lavorare sul feedback, oppure i commenti possono essere delle dichiarazioni che giustificano perché non è necessaria alcuna azione sul feedback.
* Lo stato di revisione può essere utilizzato per gestire efficientemente il feedback eseguendone la categorizzazione sotto una delle diverse opzioni di stato di revisione.

