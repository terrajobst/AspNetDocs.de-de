---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/unstructured-blob-storage
title: Unstrukturierte BLOB Storage (entwickeln realer Cloud-apps mit Azure) | Microsoft-Dokumentation
author: MikeWasson
description: Die Building Real World Cloud Apps mit Azure-e-Book basiert auf einer Präsentation von Scott Guthrie entwickelt wurde. Es wird erläutert, 13 Muster und Vorgehensweisen, die er können...
ms.author: riande
ms.date: 03/30/2015
ms.assetid: 9f05ccb1-2004-4661-ad8b-c370e6c09c8e
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/unstructured-blob-storage
msc.type: authoredcontent
ms.openlocfilehash: f48b2be755b84dff9b2672bd348c73107602c6dd
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/19/2020
ms.locfileid: "77456790"
---
# <a name="unstructured-blob-storage-building-real-world-cloud-apps-with-azure"></a>Unstrukturierte BLOB Storage (entwickeln realer Cloud-apps mit Azure)

von [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra)

[Herunterladen des IT-Projekts](https://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) oder [herunterladen des E-Books](https://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> Das e-Book zur Entwicklung **realer Cloud-apps mit Azure** basiert auf einer Präsentation von Scott Guthrie. Es wird erläutert, 13 Muster und Methoden, die Ihnen helfen können, werden erfolgreiche Entwicklung von Web-apps für die Cloud. Informationen zum e-Book finden Sie [im ersten Kapitel](introduction.md).

Im vorherigen Kapitel haben wir uns mit den Partitionierungs Schemas beschäftigt und erläutert, wie die Korrektur der IT-App Images im Azure Storage BLOB Dienst und andere Aufgaben Daten in Azure SQL-Datenbank speichert. In diesem Kapitel wird der BLOB-Dienst ausführlicher beschrieben, und es wird gezeigt, wie er in Korrektur von IT-Projekt Code implementiert wird.

## <a name="what-is-blob-storage"></a>Was ist Blobspeicher?

Der Azure Storage BLOB-Dienst bietet eine Möglichkeit zum Speichern von Dateien in der Cloud. Der BLOB-Dienst bietet eine Reihe von Vorteilen gegenüber der Speicherung von Dateien in einem lokalen Netzwerkdatei System:

- Es ist hochgradig skalierbar. In einem einzelnen Speicherkonto können [Hunderte von Terabyte](https://msdn.microsoft.com/library/windowsazure/dn249410.aspx)gespeichert werden, und Sie können über mehrere Speicher Konten verfügen. Einige der größten Azure-Kunden speichern Hunderte von Peer Tabellen. Microsoft SkyDrive verwendet BLOB Storage.
- Es ist dauerhaft. Alle Dateien, die Sie im BLOB-Dienst speichern, werden automatisch gesichert.
- Sie bietet hohe Verfügbarkeit. Die [SLA für Speicher verspricht eine](https://go.microsoft.com/fwlink/p/?linkid=159705&amp;clcid=0x409) Betriebszeit von 99,9% oder 99,99%, je nachdem, welche georedundanz Sie auswählen.
- Dabei handelt es sich um eine Platform-as-a-Service-Funktion (Platform-as-a-Service, PAS) von Azure, was bedeutet, dass Sie nur Dateien speichern und abrufen, nur für die tatsächliche Menge an Speicherplatz bezahlen, die Sie verwenden, und Azure übernimmt automatisch die Einrichtung und Verwaltung aller VMS und Laufwerke, die für die Leistungs.
- Sie können auf den BLOB-Dienst zugreifen, indem Sie eine Rest-API oder eine Programmiersprachen-API verwenden. Sdert sind für .net, Java, Ruby und andere verfügbar.
- Wenn Sie eine Datei im BLOB-Dienst speichern, können Sie Sie auf einfache Weise über das Internet öffentlich verfügbar machen.
- Sie können Dateien im BLOB-Dienst schützen, sodass nur autorisierte Benutzer darauf zugreifen können, oder Sie können temporäre Zugriffs Token bereitstellen, die Sie nur für einen begrenzten Zeitraum verfügbar machen.

Jedes Mal, wenn Sie eine APP für Azure entwickeln und viele Daten speichern möchten, die in einer lokalen Umgebung in Dateien (z. b. Bilder, Videos, PDF-Dateien, Kalkulations Tabellen usw.) gespeichert werden, sollten Sie den BLOB-Dienst in Erwägung gezogen.

## <a name="creating-a-storage-account"></a>Erstellen eines Speicher Kontos

Für den Einstieg in den BLOB-Dienst erstellen Sie ein Speicherkonto in Azure. Klicken Sie im Portal auf **neu** -- **Data Services** -- **Speicher** -- **schneller**Fassung, und geben Sie dann eine URL und einen Speicherort für den Daten Center ein. Der Speicherort des Rechenzentrums sollte mit Ihrer Web-App identisch sein.

![Erstellen eines Speicherzugriffs](unstructured-blob-storage/_static/image1.png)

Sie wählen die primäre Region aus, in der der Inhalt gespeichert werden soll. Wenn Sie die [georeplikationsoption](https://blogs.msdn.com/b/windowsazurestorage/archive/2013/12/11/introducing-read-access-geo-replicated-storage-ra-grs-for-windows-azure-storage.aspx#_Geo_Redundant_Storage) auswählen, erstellt Azure Replikate aller Ihrer Daten in einem anderen Rechenzentrum in einer anderen Region des Landes. Wenn Sie z. b. das Rechenzentrum "USA, Westen" auswählen, wird es beim Speichern einer Datei in das Daten Center "USA, Westen" kopiert, aber Azure kopiert es im Hintergrund auch in eines der anderen Rechenzentren in den USA. Wenn ein Notfall in einer Region des Landes auftritt, sind Ihre Daten immer noch sicher.

Azure repliziert Daten nicht über Grenzen hinweg: Wenn sich Ihr primärer Standort in der USA befindet, werden die Dateien nur in einer anderen Region in den USA repliziert. Wenn Ihr primärer Standort in Australien ist, werden die Dateien nur in einem anderen Rechenzentrum in Australien repliziert.

Selbstverständlich können Sie auch ein Speicherkonto erstellen, indem Sie wie zuvor gesehen Befehle aus einem Skript ausführen. Hier ist ein Windows PowerShell-Befehl zum Erstellen eines Speicher Kontos:

[!code-powershell[Main](unstructured-blob-storage/samples/sample1.ps1)]

Sobald Sie über ein Speicherkonto verfügen, können Sie sofort mit dem Speichern von Dateien im BLOB-Dienst beginnen.

## <a name="using-blob-storage-in-the-fix-it-app"></a>Verwenden von BLOB Storage in der Korrektur der IT-App

Die Korrektur der IT-App ermöglicht Ihnen das Hochladen von Fotos.

![Erstellen einer Korrektur](unstructured-blob-storage/_static/image2.png)

Wenn Sie auf **Create the fxit**klicken, lädt die Anwendung die angegebene Bilddatei hoch und speichert Sie im BLOB-Dienst.

### <a name="set-up-the-blob-container"></a>Einrichten des BLOB-Containers

Um eine Datei im BLOB-Dienst speichern zu können, benötigen Sie einen *Container* , in dem Sie gespeichert werden. Ein BLOB-Dienst Container entspricht einem Dateisystem Ordner. Mit den Umgebungs Erstellungs Skripts, die wir im [Kapitel Automatisieren von alles](automate-everything.md) überprüft haben, wird das Speicherkonto erstellt, aber es wird kein Container erstellt. Der Zweck der `CreateAndConfigure`-Methode der `PhotoService`-Klasse besteht darin, einen Container zu erstellen, wenn er nicht bereits vorhanden ist. Diese Methode wird von der `Application_Start`-Methode in " *Global. asax*" aufgerufen.

[!code-csharp[Main](unstructured-blob-storage/samples/sample2.cs)]

Der Speicherkonto Name und der Zugriffsschlüssel werden in der `appSettings`-Sammlung der Datei " *Web. config* " gespeichert, und der Code in der `StorageUtils.StorageAccount`-Methode verwendet diese Werte, um eine Verbindungs Zeichenfolge zu erstellen und eine Verbindung herzustellen:

[!code-csharp[Main](unstructured-blob-storage/samples/sample3.cs)]

Die `CreateAndConfigureAsync`-Methode erstellt dann ein-Objekt, das den BLOB-Dienst darstellt, und ein-Objekt, das einen Container (Ordner) mit dem Namen "Images" im BLOB-Dienst darstellt:

[!code-csharp[Main](unstructured-blob-storage/samples/sample4.cs)]

Wenn ein Container mit dem Namen "Images" noch nicht vorhanden ist, was wahr ist, wenn Sie die APP zum ersten Mal für ein neues Speicherkonto ausführen, erstellt der Code den Container und legt die Berechtigungen fest, um ihn öffentlich zu machen. (Standardmäßig sind neue BLOB-Container Privat und können nur für Benutzer zugänglich sein, die über die Berechtigung zum Zugreifen auf Ihr Speicherkonto verfügen.)

[!code-csharp[Main](unstructured-blob-storage/samples/sample5.cs)]

### <a name="store-the-uploaded-photo-in-blob-storage"></a>Speichern des hochgeladenen Fotos im BLOB-Speicher

Um die Bilddatei hochzuladen und zu speichern, verwendet die APP eine `IPhotoService`-Schnittstelle und eine Implementierung der-Schnittstelle in der `PhotoService`-Klasse. Die *Photoservice.cs* -Datei enthält den gesamten Code in der Fix-it-APP, die mit dem BLOB-Dienst kommuniziert.

Die folgende MVC-Controller Methode wird aufgerufen, wenn der Benutzer auf **Create the fxit**klickt. In diesem Code verweist `photoService` auf eine Instanz der `PhotoService`-Klasse, und `fixittask` verweist auf eine Instanz der `FixItTask` Entitäts Klasse, die Daten für eine neue Aufgabe speichert.

[!code-csharp[Main](unstructured-blob-storage/samples/sample6.cs?highlight=8)]

Die `UploadPhotoAsync`-Methode in der `PhotoService`-Klasse speichert die hochgeladene Datei im BLOB-Dienst und gibt eine URL zurück, die auf das neue BLOB verweist.

[!code-csharp[Main](unstructured-blob-storage/samples/sample7.cs)]

Wie bei der `CreateAndConfigure`-Methode stellt der Code eine Verbindung mit dem Speicherkonto her und erstellt ein Objekt, das den BLOB-Container "Images" darstellt. es wird jedoch davon ausgegangen, dass der Container bereits vorhanden ist.

Anschließend wird ein eindeutiger Bezeichner für das hoch zuladende Bild erstellt, indem ein neuer GUID-Wert mit der Dateierweiterung verkettet wird:

[!code-csharp[Main](unstructured-blob-storage/samples/sample8.cs)]

Der Code verwendet dann das blobcontainerobjekt und den neuen eindeutigen Bezeichner zum Erstellen eines BLOB-Objekts, legt ein Attribut für das Objekt fest, das angibt, welche Art von Datei es ist, und verwendet dann das BLOB-Objekt, um die Datei im BLOB-Speicher zu speichern.

[!code-csharp[Main](unstructured-blob-storage/samples/sample9.cs)]

Schließlich wird eine URL abgerufen, die auf das BLOB verweist. Diese URL wird in der-Datenbank gespeichert und kann zur Behebung von IT-Webseiten verwendet werden, um das hochgeladene Bild anzuzeigen.

[!code-csharp[Main](unstructured-blob-storage/samples/sample10.cs)]

Diese URL wird in der Datenbank als eine der Spalten der Tabelle "fixttask" gespeichert.

[!code-csharp[Main](unstructured-blob-storage/samples/sample11.cs?highlight=10)]

Wenn nur die URL in der Datenbank und Images im BLOB-Speicher vorliegen, hält die Korrektur der IT-APP die Datenbank gering, skalierbar und kostengünstig an, während die Images gespeichert werden, wo Speicherkosten günstig sind und Terabyte oder Peer-Tabellen verarbeiten können. Ein Speicherkonto kann Hunderte von Terabyte an Korrektur-Fotos speichern, und Sie zahlen nur für das, was Sie nutzen. Sie können also mit einer kleinen Zahlung von 9 Cent für das erste Gigabyte beginnen und weitere Abbilder für die pro-GB-Anzahl hinzufügen.

### <a name="display-the-uploaded-file"></a>Anzeigen der hochgeladenen Datei

Die Korrektur der IT-Anwendung zeigt die hochgeladene Bilddatei an, wenn Details für eine Aufgabe angezeigt werden.

![Details zur IT-Aufgabe mit Foto korrigieren](unstructured-blob-storage/_static/image3.png)

Um das Bild anzuzeigen, muss die gesamte MVC-Ansicht den `PhotoUrl` Wert in den an den Browser gesendeten HTML-Code einschließen. Der Webserver und die Datenbank verwenden keine Zyklen zum Anzeigen des Bilds, sondern nur einige Bytes für die Bild-URL. Im folgenden Razor-Code verweist `Model` auf eine Instanz der `FixItTask` Entitäts Klasse.

[!code-cshtml[Main](unstructured-blob-storage/samples/sample12.cshtml?highlight=11)]

Wenn Sie sich den HTML-Code der angezeigten Seite ansehen, sehen Sie, dass die URL direkt auf das Bild im BLOB-Speicher zeigt, etwa wie folgt:

[!code-cshtml[Main](unstructured-blob-storage/samples/sample13.cshtml?highlight=11)]

## <a name="summary"></a>Zusammenfassung

Sie haben gesehen, wie die Korrektur der IT-App Images im BLOB-Dienst und nur Bild-URLs in der SQL-Datenbank speichert. Wenn Sie den BLOB-Dienst verwenden, ist die SQL-Datenbank viel kleiner, als andernfalls. dadurch ist es möglich, eine Skalierung auf eine fast unbegrenzte Anzahl von Aufgaben durchzusetzen, ohne viel Code schreiben zu müssen.

Sie können Hunderte von Terabyte in einem Speicherkonto haben, und die Speicherkosten sind viel kostengünstiger als SQL-Daten Bank Speicher, beginnend bei ungefähr 3 Cent pro Gigabyte pro Monat zuzüglich einer geringen Transaktionsgebühr. Denken Sie daran, dass Sie nicht für die maximale Kapazität bezahlen, sondern nur für den Betrag, den Sie tatsächlich speichern, sodass Ihre APP skaliert werden kann, Sie aber nicht für die gesamte zusätzliche Kapazität bezahlen.

Im [nächsten Kapitel](design-to-survive-failures.md) wird erläutert, wie wichtig es ist, dass eine Cloud-App fehlerhafte Fehlerbehandlung ermöglicht.

## <a name="resources"></a>Ressourcen

Weitere Informationen finden Sie in den folgenden Ressourcen:

- [Eine Einführung in Azure BLOB Storage](https://www.simple-talk.com/cloud/cloud-data/an-introduction-to-windows-azure-blob-storage-/). Blog von Mike Wood.
- [Verwenden des Azure BLOB Storage-Dienstanbieter in .net](https://docs.microsoft.com/azure/storage/blobs/storage-dotnet-how-to-use-blobs). Offizielle Dokumentation auf der MicrosoftAzure.com-Website. Eine kurze Einführung in BLOB Storage, gefolgt von Codebeispielen, die zeigen, wie Sie eine Verbindung mit BLOB Storage herstellen, Container erstellen, BLOBs hochladen und herunterladen usw.
- [Failsafe: aufbauen skalierbarer, robuster Cloud Services](https://channel9.msdn.com/Series/FailSafe). Neun teilige Videoreihe von Ulrich Homann, Marc Mercuri und Mark Simms. Stellt allgemeine Konzepte und architektonische Prinzipien in einer sehr zugänglichen und interessanten Weise vor, wobei Storys von der Benutzerfreundlichkeit von Microsoft Customer Advisory Team (CAT) mit tatsächlichen Kunden erstellt werden. Eine Erläuterung zu Azure Storage-Dienst und blobblobvorschauen finden Sie in Episode 5 ab 35:13.
- [Microsoft Patterns and Practices: Azure-Leitfaden](https://msdn.microsoft.com/library/dn568099.aspx). Siehe Valet-Schlüssel Muster.

> [!div class="step-by-step"]
> [Zurück](data-partitioning-strategies.md)
> [Weiter](design-to-survive-failures.md)
