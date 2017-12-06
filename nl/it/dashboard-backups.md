---

copyright:
  years: 2017
lastupdated: "2017-10-16"
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Backup
{: #backups}

Puoi creare e scaricare i backup dalla scheda _Backups_ della pagina *Manage* del tuo dashboard del servizio. Sono disponibili sia i backup manuali che pianificati.

I backup vengono creati eseguendo il backup dei nodi del database Scylla. I backup Scylla vengono presi utilizzando il programma di utilità dell'istantanea Scylla, eseguendo il backup di tutti i file di dati sul disco archiviati nella directory dei dati. L'istantanea può essere eseguita mentre i database sono online.

## Visualizzazione dei backup esistenti

I backup giornalieri del tuo database vengono automaticamente pianificati. Per visualizzare i tuoi backup esistenti:

1. Passa alla pagina _Manage_ del tuo dashboard del servizio.
2. Fai clic su **Backups** nelle schede per aprire la pagina _Backups_. Viene visualizzato un elenco di backup disponibili, con i backup più recenti all'inizio dell'elenco: 

  ![Backup disponibili](./images/janusgraph-backups-show.png "Un elenco di backup disponibili, incluso un backup in attesa")

Fai clic sulla riga corrispondente per espandere le opzioni per ogni backup disponibile.
  ![Opzioni di backup](./images/janusgraph-backups-options.png "Opzioni di backup.") 

## Creazione di un backup manuale

Come per i backup pianificati puoi creare un backup manualmente. Per creare un backup manuale, segui le istruzioni per visualizzare i backup esistenti, quindi fai clic su **Back up now** sopra l'elenco dei backup disponibili. Viene visualizzato un messaggio per farti sapere che è stato avviato un backup ed è stato aggiunto un backup 'in sospeso' all'elenco dei backup disponibili.

## Ripristino di un backup
Per ripristinare un backup in una nuova istanza, segui le istruzioni per visualizzare i backup esistenti, quindi fai clic sulla riga corrispondente per espandere le opzioni del backup che desideri scaricare. Fai clic sul pulsante **Restore**. Viene visualizzato un messaggio per farti sapere che è stato avviato un ripristino. La nuova istanza del servizio sarà automaticamente denominata "janusgraph-restore-[timestamp]" e visualizzata nel tuo dashboard dopo l'avvio del provisioning.
