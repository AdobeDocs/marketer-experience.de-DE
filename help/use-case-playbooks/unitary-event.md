---
title: Unitäres Ereignis
description: Dies ist eine Anleitungsseite für die Simulation des Typs [!UICONTROL Unitäres Ereignis] der Journey-Validierung.
source-git-commit: 0a52611530513fc036ef56999fde36a32ca0f482
workflow-type: ht
source-wordcount: '749'
ht-degree: 100%

---


# Unitäres Ereignis

## Zu befolgende Schritte {#steps-to-follow}

>[!CONTEXTUALHELP]
>id="marketerexp_sampledata_unitaryevent"
>title="Wie wird sie verwendet?"
>abstract="Bitte folgen Sie dem Link für weitere Informationen"

>[!IMPORTANT]
>
>Diese Anweisungen können sich je nach **[!UICONTROL Playbook]** ändern, daher sollten Sie immer den Abschnitt „Beispieldaten“ im jeweiligen **[!UICONTROL Playbook]** lesen.

## Voraussetzung

* Sie müssen die Postman-Software installiert haben
* Verwenden Sie das Playbook, um die Instanzen-Assets wie **[!UICONTROL Journey]**, **[!UICONTROL Schemata]**, **[!UICONTROL Segmente]**, **[!UICONTROL Nachrichten]** usw. zu erstellen.

Die erstellten Assets werden auf der Seite `Bill Of Material` angezeigt.

![Seite „Stückliste“](../assets/bom-page.png)

## Vorbereiten von Postman mit erforderlicher Sammlung

1. Besuchen Sie die Anwendung **[!UICONTROL Anwendungsfall-Playbook]**.
1. Klicken Sie auf die jeweilige **[!UICONTROL Playbook]**-Karte, um zur **[!UICONTROL Playbook]**-Detailseite zu gelangen.
1. Besuchen Sie die Seite **[!UICONTROL Stückliste]** und finden Sie den Abschnitt **[!UICONTROL Beispieldaten]**.
1. Laden Sie die `postman.json` herunter, indem Sie auf die entsprechenden Schaltflächen in der Benutzeroberfläche klicken.
1. Importieren Sie `postman.json` in die **[!DNL Postman Software]**.
1. Erstellen Sie eine eigene Postman-Umgebung für diese Validierung (z. B. `Adobe <PLAYBOOK_NAME>`).

## Abrufen des IMS-Tokens

>[!NOTE]
>
>Bei allen Umgebungsvariablen wird zwischen Groß- und Kleinschreibung unterschieden, verwenden Sie also bitte immer den genauen Variablennamen.

1. Bitte folgen Sie der Dokumentation [Authentifizieren und Aufrufen von Experience Platform-APIs](https://experienceleague.adobe.com/docs/experience-platform/landing/platform-apis/api-authentication.html?lang=de), um das Zugriffs-Token zu generieren.
1. Speichern Sie den Wert des Zugriffs-Tokens in einer Umgebungsvariablen namens `ACCESS_TOKEN`.
1. Speichern Sie andere authentifizierungsbezogene Werte wie `API_KEY`, `IMS_ORG` und `SANDBOX_NAME` in Umgebungsvariablen.

>[!IMPORTANT]
>
>Bevor Sie eine API von Postman ausführen, müssen Sie sicherstellen, dass alle erforderlichen Umgebungsvariablen hinzugefügt wurden.

## Veröffentlichen der vom Playbook erstellten Journey

Es gibt zwei Möglichkeiten, die Journey zu veröffentlichen. Sie können beliebig zwischen ihnen wählen:

1. **Verwenden der AJO-Benutzeroberfläche** – Klicken Sie auf den Journey-Link auf der `Bill Of Material Page`. Dadurch werden Sie zur Journey-Seite umgeleitet, wo Sie auf die Schaltfläche **[!UICONTROL Veröffentlichen]** klicken können, damit die Journey veröffentlicht wird.

   ![Journey-Objekt](../assets/journey-object.png)

1. **Verwenden der Postman-API**

   1. Lösen Sie eine **[!DNL Publish Journey]**-Anfrage über **[!DNL Journey Publish]** > **[!DNL Queue journey publish job]** aus.
   1. Die Veröffentlichung einer Journey kann einige Zeit in Anspruch nehmen. Um den Status zu überprüfen, führen Sie die API „Journey-Veröffentlichungsstatus überprüfen“ aus, bis `response.status` `SUCCESS` ist. Warten Sie 10–15 Sekunden, wenn die Veröffentlichung der Journey einige Zeit dauert.

   >[!NOTE]
   >
   >Bei allen Umgebungsvariablen wird zwischen Groß- und Kleinschreibung unterschieden, verwenden Sie also bitte immer den genauen Variablennamen.

## Aufnehmen des Kundenprofils

>[!TIP]
>
>Sie können dieselbe E-Mail-Adresse wiederverwenden, indem Sie `+<variable>` in Ihrer E-Mail-Adresse anhängen. So kann z. B. `usertest@email.com` als `usertest+v1@email.com` oder `usertest+24jul@email.com` wiederverwendet werden. Dies kann hilfreich sein, wenn jedes Mal ein neues Profil, jedoch mit derselben E-Mail-ID, verwendet werden soll.

1. Erstmalige Benutzerinnen und Benutzer müssen das **[!DNL customer dataset]** und die **[!DNL HTTP Streaming Inlet Connection]** erstellen.
1. Wenn Sie bereits das **[!DNL customer dataset]** und die **[!DNL HTTP Streaming Inlet Connection]** erstellt haben, fahren Sie bitte mit dem Schritt `5` fort.
1. Lösen Sie **[!DNL Customer Profile Ingestion]** > **[!DNL Create Customer Profile InletId]** > **[!DNL Create Dataset]** aus, um das **[!DNL customer dataset]** zu erstellen. Dadurch wird eine `CustomerProfile_dataset_id` in den Postman-Umgebungsvariablen gespeichert.
1. Erstellen Sie die **[!DNL HTTP Streaming Inlet Connection]** und verwenden Sie die Postman-APIs unter **[!DNL Customer Profile Ingestion > Create Customer Profile InletId]**.

   1. `CustomerProfile_dataset_id` muss in den Postman-Umgebungsvariablen vorhanden sein. Falls dies nicht der Fall ist, lesen Sie Schritt `3`.
   1. Lösen Sie **[!DNL `CREATE Base Connection`]** aus, um [!DNL create base connection] durchzuführen.
   1. Lösen Sie **[!DNL `CREATE Source Connection`]** aus, um [!DNL create source connection] durchzuführen.
   1. Lösen Sie **[!DNL `CREATE Target Connection`]** aus, um [!DNL create target connection] durchzuführen.
   1. Lösen Sie **[!DNL `CREATE Dataflow`]** aus, um [!DNL create dataflow] durchzuführen.
   1. Lösen Sie **[!DNL `GET Base Connection`]** aus – dies speichert automatisch die `CustomerProfile_inlet_id` in den Postman-Umgebungsvariablen.

1. Bei diesem Schritt müssen Sie `CustomerProfile_dataset_id` und `CustomerProfile_inlet_id` in den Postman-Umgebungsvariablen haben. Falls dies nicht der Fall ist, lesen Sie bitte den Schritt `3` bzw. `4`.
1. Um eine Kundin oder einen Kunden aufzunehmen, muss die Benutzerin bzw. der Benutzer `customer_country_code`, `customer_mobile_no`, `customer_first_name`, `customer_last_name` und `email` in den Postman-Umgebungsvariablen speichern.

   1. `customer_country_code` ist die Landesvorwahl der Handynummer, z. B. `91` oder `1`
   1. `customer_mobile_no` ist die Handynummer, z. B. `9987654321`
   1. `customer_first_name` ist der Vorname der Benutzerin bzw. des Benutzers
   1. `customer_last_name` ist der Nachname der Benutzerin bzw. des Benutzers
   1. `email` ist die E-Mail-Adresse der Benutzerin bzw. des Benutzers. Es ist wichtig, eine eindeutige E-Mail-Adresse zu verwenden, damit ein neues Profil aufgenommen werden kann.

1. Aktualisieren Sie die Postman-Anfrage **[!DNL Customer Ingestion]** > **[!DNL Customer Streaming Ingestion]**, um den bevorzugten Kanal der Kundin bzw. des Kunden zu ändern. Standardmäßig ist [!DNL `email`] in der Anfrage konfiguriert.

   ```js
   "consents": {
       "marketing": {
           "preferred": "email",
           "email": {
               "val": "y"
           },
           "push": {
               "val": "n"
           },
           "sms": {
               "val": "n"
           }
       }
   }
   ```

1. Ändern Sie den bevorzugten Kanal in `sms` oder `push` und setzen Sie den jeweiligen Kanalwert auf `y`, aber auf `n` zu anderen Werten, z. B.

   ```js
   "consents": {
       "marketing": {
           "preferred": "sms",
           "email": {
               "val": "n"
           },
           "push": {
               "val": "n"
           },
           "sms": {
               "val": "y"
           }
       }
   }
   ```

1. Lösen Sie schließlich **[!DNL `Customer Profile Ingestion > Customer Profile Streaming Ingestion`]** aus, um das Kundenprofil aufzunehmen.

## Aufnahmeereignis

1. Erstmalige Benutzerinnen bzw. Benutzer müssen das **[!DNL event dataset]** und die **[!DNL HTTP Streaming Inlet Connection for events]** erstellen
1. Wenn Sie bereits das **[!DNL event dataset]** und die **[!DNL HTTP Streaming Inlet Connection for events]** erstellt haben, fahren Sie bitte mit dem Schritt `5` fort.
1. Lösen Sie **[!DNL `Schemas Data Ingestion > AEP Demo Schema Ingestion > Create AEP Demo Schema InletId > Create Dataset`]** aus, um das **[!DNL event dataset]** zu erstellen. Dadurch wird eine `AEPDemoSchema_dataset_id` in den Postman-Umgebungsvariablen gespeichert.
1. Erstellen Sie eine **[!DNL HTTP Streaming Inlet Connection for events]** und verwenden Sie die Postman-APIs unter **[!DNL Schemas Data Ingestion]** > **[!DNL AEP Demo Schema Ingestion]** > **[!DNL Create AEP Demo Schema InletId]**.

   1. `AEPDemoSchema_dataset_id` muss in den Postman-Umgebungsvariablen vorhanden sein. Falls dies nicht der Fall ist, lesen Sie Schritt `3`
   1. Lösen Sie **[!DNL `CREATE Base Connection`]** aus, um [!DNL create base connection] durchzuführen
   1. Lösen Sie **[!DNL `CREATE Source Connection`]** aus, um [!DNL create source connection] durchzuführen
   1. Lösen Sie **[!DNL `CREATE Target Connection`]** aus, um [!DNL create target connection] durchzuführen
   1. Lösen Sie **[!DNL `CREATE Dataflow`]** aus, um [!DNL create dataflow] durchzuführen
   1. Lösen Sie **[!DNL `GET Base Connection`]** aus – dies speichert automatisch `AEPDemoSchema_inlet_id` in den Postman-Umgebungsvariablen

1. Bei diesem Schritt müssen Sie `AEPDemoSchema_dataset_id` und `AEPDemoSchema_inlet_id` in den Postman-Umgebungsvariablen haben. Falls dies nicht der Fall ist, lesen Sie bitte den Schritt `3` bzw. `4` 
1. Um ein Ereignis aufzunehmen, muss die Benutzerin bzw. der Benutzer die Zeitvariable `timestamp` im Anfragetext von **[!DNL Schemas Data Ingestion]** > **[!DNL AEP Demo Schema Ingestion]** > **[!DNL AEP Demo Schema Streaming Ingestion]** in Postman ändern.

   1. `timestamp` ist die Zeit des Auftretens des Ereignisses. Verwenden Sie den aktuellen Zeitstempel, z. B. `2023-07-21T16:37:52+05:30`, und passen Sie die Zeitzone nach Ihren Bedürfnissen an.

1. Lösen Sie **[!DNL Schemas Data Ingestion > AEP Demo Schema Ingestion > AEP Demo Schema Streaming Ingestion]** aus, um das Ereignis aufzunehmen, damit die Journey ausgelöst werden kann

## Abschließende Validierung

Sie müssen eine Nachricht auf dem von Ihnen gewählten bevorzugten Kanal erhalten, der in **[!DNL Ingest the Customer Profile]** im Schritt `8` verwendet wird.

* `SMS`, wenn der bevorzugte Kanal `sms` für `customer_country_code` und `customer_mobile_no` ist
* `Email` wenn der bevorzugte Kanal `email` für `email` ist

Alternativ können Sie auch `Journey Report` überprüfen. Klicken Sie dazu auf `Journey Object` auf der `Bill of Materials page`, und Sie werden zur `Journey Details page` weitergeleitet.

Für jede veröffentlichte Journey muss die Benutzerin bzw. der Benutzer eine Schaltfläche **[!UICONTROL Bericht anzeigen]** erhalten
![Seite „Journey-Bericht“](../assets/journey-report-page.png)


## Bereinigen

Bitte lassen Sie nicht mehrere Instanzen von `Journey` gleichzeitig laufen, sondern stoppen Sie die Journey, wenn sie nur zur Validierung dient, sobald die Validierung abgeschlossen ist.
