---
uid: web-api/overview/security/working-with-ssl-in-web-api
title: Arbeiten mit SSL in der Web-API | Microsoft-Dokumentation
author: MikeWasson
description: Zeigt die Verwendung von SSL mit ASP.net-Web-API, einschließlich der Verwendung von SSL-Client Zertifikaten.
ms.author: riande
ms.date: 02/22/2019
ms.assetid: 97f6164f-59cf-45c0-b820-e4aa29b45396
msc.legacyurl: /web-api/overview/security/working-with-ssl-in-web-api
msc.type: authoredcontent
ms.openlocfilehash: 31589b3713b1f1a9b98d12906bfef81f8bf5e3f9
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78484413"
---
# <a name="working-with-ssl-in-web-api"></a>Arbeiten mit SSL in der Web-API

von [Mike Wasson](https://github.com/MikeWasson)

Mehrere allgemeine Authentifizierungs Schemas sind über einfaches http nicht sicher. Insbesondere senden die einfache Authentifizierung und Formularauthentifizierung unverschlüsselte Anmeldeinformationen. Um sicher zu sein, *müssen* diese Authentifizierungs Schemas SSL verwenden. Außerdem können SSL-Client Zertifikate verwendet werden, um Clients zu authentifizieren.

## <a name="enabling-ssl-on-the-server"></a>Aktivieren von SSL auf dem Server

Einrichten von SSL in IIS 7 oder höher:

- Erstellen oder rufen Sie ein Zertifikat ab. Zum Testen können Sie ein selbst signiertes Zertifikat erstellen.
- Fügen Sie eine HTTPS-Bindung hinzu.

Weitere Informationen finden [Sie unter Einrichten von SSL auf IIS 7](https://www.iis.net/learn/manage/configuring-security/how-to-set-up-ssl-on-iis).

Für lokale Tests können Sie SSL in IIS Express aus Visual Studio aktivieren. Legen Sie in der Eigenschaftenfenster **SSL aktiviert** auf **true**fest. Notieren Sie sich den Wert der **SSL-URL**. Verwenden Sie diese URL zum Testen von HTTPS-Verbindungen.

![](working-with-ssl-in-web-api/_static/image1.png)

### <a name="enforcing-ssl-in-a-web-api-controller"></a>Erzwingen von SSL in einem Web-API-Controller

Wenn Sie sowohl über eine HTTPS-als auch über eine HTTP-Bindung verfügen, können Clients weiterhin http für den Zugriff auf die Website verwenden. Möglicherweise können einige Ressourcen über HTTP verfügbar sein, während für andere Ressourcen SSL erforderlich ist. Verwenden Sie in diesem Fall einen Aktionsfilter, um SSL für die geschützten Ressourcen anzufordern. Der folgende Code zeigt einen Web-API-Authentifizierungsfilter mit Überprüfung auf SSL:

[!code-csharp[Main](working-with-ssl-in-web-api/samples/sample1.cs)]

Fügen Sie diesen Filter allen Web-API-Aktionen hinzu, die SSL erfordern:

[!code-csharp[Main](working-with-ssl-in-web-api/samples/sample2.cs)]

## <a name="ssl-client-certificates"></a>SSL-Client Zertifikate

SSL ermöglicht die Authentifizierung mithilfe von Public Key-Infrastruktur Zertifikaten. Der Server muss ein Zertifikat bereitstellen, mit dem der Server beim Client authentifiziert wird. Es ist weniger üblich, dass der Client ein Zertifikat für den Server bereitstellt, aber dies ist eine Option zum Authentifizieren von Clients. Zum Verwenden von Client Zertifikaten mit SSL benötigen Sie eine Möglichkeit, signierte Zertifikate an Ihre Benutzer zu verteilen. Bei vielen Anwendungs Typen ist dies keine gute Benutzerumgebung, aber in einigen Umgebungen (z. b. Enterprise) ist dies möglicherweise möglich.

| Vorteile | Nachteile |
| --- | --- |
| -Die Zertifikat Anmelde Informationen sind stärker als Benutzername/Kennwort. -SSL bietet einen umfassenden sicheren Kanal mit Authentifizierung, Nachrichten Integrität und Nachrichten Verschlüsselung. | -Sie müssen PKI-Zertifikate abrufen und verwalten. -Die Client Plattform muss SSL-Client Zertifikate unterstützen. |

Zum Konfigurieren von IIS für die Annahme von Client Zertifikaten öffnen Sie IIS-Manager, und führen Sie die folgenden Schritte aus:

1. Klicken Sie in der Strukturansicht auf den Knoten Standort.
2. Doppelklicken Sie im mittleren Bereich auf das Feature **SSL-Einstellungen** .
3. Wählen Sie unter **Client Zertifikate**eine der folgenden Optionen aus: 

    - **Akzeptieren**: IIS akzeptiert ein Zertifikat vom Client, erfordert aber kein Zertifikat.
    - **Erforderlich**: ein Client Zertifikat erforderlich. (Um diese Option zu aktivieren, müssen Sie auch "SSL erforderlich" auswählen).

Sie können diese Optionen auch in der Datei "applicationHost. config" festlegen:

[!code-xml[Main](working-with-ssl-in-web-api/samples/sample3.xml)]

Das Flag " **sslaushandatecert** " bedeutet, dass IIS ein Zertifikat vom Client annimmt, aber keinen erfordert (entspricht der Option "Accept" im IIS-Manager). Wenn Sie ein Zertifikat anfordern möchten, legen Sie das **SSLRequireCert** -Flag fest. Zum Testen können Sie diese Optionen auch in IIS Express auf dem lokalen ApplicationHost festlegen. Config-Datei, die sich unter "documents\iisexpress\config" befindet.

### <a name="creating-a-client-certificate-for-testing"></a>Erstellen eines Client Zertifikats zum Testen

Zu Testzwecken können Sie [Makecert. exe](/windows/desktop/SecCrypto/makecert) verwenden, um ein Client Zertifikat zu erstellen. Erstellen Sie zunächst eine Test Stamm Zertifizierungsstelle:

[!code-console[Main](working-with-ssl-in-web-api/samples/sample4.cmd)]

Mit Makecert werden Sie aufgefordert, ein Kennwort für den privaten Schlüssel einzugeben.

Fügen Sie als nächstes das Zertifikat dem Speicher "Vertrauenswürdige Stamm Zertifizierungsstellen" des Test Servers wie folgt hinzu:

1. Öffnen Sie MMC.
2. Wählen Sie unter **Datei**die Option **Snap-in hinzufügen/entfernen**aus.
3. Wählen Sie **Computer Konto**aus.
4. Wählen Sie **lokaler Computer** aus, und beenden Sie den Assistenten.
5. Erweitern Sie im Navigationsbereich den Knoten "Vertrauenswürdige Stamm Zertifizierungsstellen".
6. Zeigen Sie im Menü **Aktion** auf **alle Aufgaben**, und klicken Sie dann auf **importieren** , um den Zertifikat Import-Assistenten zu starten.
7. Navigieren Sie zur Zertifikatsdatei TempCA. cer.
8. Klicken Sie auf **Öffnen**und dann auf **weiter** , und schließen Sie den Assistenten ab. (Sie werden aufgefordert, das Kennwort erneut einzugeben.)

Erstellen Sie jetzt ein Client Zertifikat, das vom ersten Zertifikat signiert wurde:

[!code-console[Main](working-with-ssl-in-web-api/samples/sample5.cmd)]

### <a name="using-client-certificates-in-web-api"></a>Verwenden von Client Zertifikaten in der Web-API

Auf der Serverseite können Sie das Client Zertifikat abrufen, indem Sie [getclientcertificate](https://msdn.microsoft.com/library/system.net.http.httprequestmessageextensions.getclientcertificate.aspx) für die Anforderungs Nachricht aufrufen. Die-Methode gibt NULL zurück, wenn kein Client Zertifikat vorhanden ist. Andernfalls wird eine **X509Certificate2** -Instanz zurückgegeben. Verwenden Sie dieses Objekt, um Informationen aus dem Zertifikat zu erhalten, z. b. den Aussteller und den Betreff. Anschließend können Sie diese Informationen für die Authentifizierung und/oder Autorisierung verwenden.

[!code-csharp[Main](working-with-ssl-in-web-api/samples/sample6.cs)]
