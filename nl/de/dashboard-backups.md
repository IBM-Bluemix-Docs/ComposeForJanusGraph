---

copyright:
  years: 2017,2018
lastupdated: "2017-10-16"
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Sicherungen
{: #backups}

Sie können Sicherungen erstellen und über die Registerkarte _Sicherungen_ der Seite _Verwalten_ Ihres Service-Dashboards herunterladen. Dabei sind tägliche, wöchentliche und monatliche Sicherungen sowie Sicherungen nach Bedarf verfügbar. Die Aufbewahrungszeit richtet sich nach folgendem Zeitplan:

Sicherungstyp|Aufbewahrungszeitplan
----------|-----------
Täglich|Tägliche Sicherungen werden 7 Tage aufbewahrt
Wöchentlich|Wöchentliche Sicherungen werden 4 Wochen aufbewahrt
Monatlich|Monatliche Sicherungen werden 3 Monate aufbewahrt
Bei Bedarf|Es wird eine bei Bedarf erstellte Sicherung aufbewahrt. Dabei ist die aufbewahrte Sicherung immer die letzte bei Bedarf erstellte Sicherung.
{: caption="Tabelle 1. Aufbewahrungszeitplan für Sicherungen" caption-side="top"}

Sicherungszeitpläne und Aufbewahrungsrichtlinien sind festgelegt. Falls Sie mehr Sicherungen benötigen als die, die im Aufbewahrungszeitplan vorgesehen sind, sollten Sie Sicherungen herunterladen und Sicherungsarchive gemäß Ihren Geschäftsanforderungen aufbewahren.

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
