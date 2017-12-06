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

# Sicherungen
{: #backups}

Sie können Sicherungen erstellen und über die Registerkarte _Sicherungen_ der Seite *Verwalten* Ihres Service-Dashboards herunterladen. Es sind sowohl geplante als auch manuelle Sicherungen verfügbar.

Sicherungen werden erstellt, indem die Scylla-Datenbankknoten gesichert werden. Scylla-Sicherungen werden mithilfe des Scylla-Dienstprogramms für Snapshots erstellt, wobei alle On-Disk-Datendateien im Datenverzeichnis gespeichert werden. Der Snapshot kann aktiv sein, während Ihre Datenbanken online sind.

## Vorhandene Sicherungen anzeigen

Tägliche Sicherungen Ihrer Datenbank werden automatisch geplant. Gehen Sie wie folgt vor, um Ihre vorhandenen Sicherungen anzuzeigen:

1. Navigieren Sie zu der Seite _Verwalten_ Ihres Service-Dashboards.
2. Klicken Sie auf die Registerkarte **Sicherungen**, um die Seite _Sicherungen_ zu öffnen. Es wird eine Liste der verfügbaren Sicherungen angezeigt. Dabei befinden sich die neuesten Sicherungen in der Liste ganz oben:

  ![Verfügbare Sicherungen](./images/janusgraph-backups-show.png "Liste der verfügbaren Sicherungen, einschließlich der anstehenden Sicherung")

Klicken Sie in eine Zeile, um die Optionen für die entsprechende verfügbare Sicherung zu erweitern. ![Sicherungsoptionen](./images/janusgraph-backups-options.png "Optionen für eine Sicherung.") 

## Manuelle Sicherung erstellen

Neben geplanten Sicherungen können Sie manuelle Sicherungen erstellen. Führen Sie zum Erstellen einer manuellen Sicherung die Schritte zum Anzeigen der vorhandenen Sicherungen aus. Klicken Sie dann über der Liste der vorhandenen Sicherungen auf **Jetzt sichern**. Es wird eine Nachricht darüber angezeigt, dass eine Sicherung eingeleitet wurde, und zur Liste der verfügbaren Sicherungen wird eine 'anstehende' Sicherung hinzugefügt.

## Sicherung wiederherstellen
Führen Sie zum Wiederherstellen einer Sicherung in eine neue Serviceinstanz die Schritte zum Anzeigen der vorhandenen Sicherungen aus. Klicken Sie dann in die entsprechende Zeile, um die Optionen für die Sicherung zu erweitern, die Sie herunterladen wollen. Klicken Sie auf die Schaltfläche **Wiederherstellen**. Es wird eine Nachricht darüber angezeigt, dass eine Wiederherstellung eingeleitet wurde. Die neue Serviceinstanz erhält automatisch den Namen "janusgraph-restore-[timestamp]" und wird beim Start der Bereitstellung in Ihrem Dashboard angezeigt.
