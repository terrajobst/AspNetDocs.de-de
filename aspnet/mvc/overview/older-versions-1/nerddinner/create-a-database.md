---
uid: mvc/overview/older-versions-1/nerddinner/create-a-database
title: Erstellen einer Datenbank | Microsoft-Dokumentation
author: microsoft
description: Schritt 2 zeigt die Schritte zum Erstellen der Datenbank, in der alle Dinner-und RSVP-Daten für unsere "nerddinner"-Anwendung enthalten sind.
ms.author: riande
ms.date: 07/27/2010
ms.assetid: 983f3ffa-08b8-4868-b8c9-aa34593fc683
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/create-a-database
msc.type: authoredcontent
ms.openlocfilehash: b0aa7c8cdf741f44e09ed18e2b2f73fe6bf786ae
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78469299"
---
# <a name="create-a-database"></a>Erstellen einer Datenbank

von [Microsoft](https://github.com/microsoft)

[PDF herunterladen](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Dies ist Schritt 2 des kostenlosen ["nerddinner"](introducing-the-nerddinner-tutorial.md) -Lernprogramms, in dem erläutert wird, wie eine kleine, aber komplette Webanwendung mit ASP.NET MVC 1 erstellt wird.
> 
> Schritt 2 zeigt die Schritte zum Erstellen der Datenbank, in der alle Dinner-und RSVP-Daten für unsere "nerddinner"-Anwendung enthalten sind.
> 
> Wenn Sie ASP.NET MVC 3 verwenden, empfiehlt es sich, die Tutorials " [Getting Started with MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) " oder " [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) " zu befolgen.

## <a name="nerddinner-step-2-creating-the-database"></a>Nerddinner Step 2: Erstellen der Datenbank

Wir verwenden eine Datenbank, um alle Dinner-und RSVP-Daten für unsere "nerddinner"-Anwendung zu speichern.

Die folgenden Schritte zeigen, wie Sie die Datenbank mit der kostenlosen SQL Server Express Edition erstellen (die Sie mithilfe von V2 des [Microsoft-Webplattform-Installer](https://www.microsoft.com/web/downloads/platform.aspx)problemlos installieren können). Der gesamte Code, den wir schreiben, funktioniert sowohl mit SQL Server Express als auch mit der vollständigen SQL Server.

### <a name="creating-a-new-sql-server-express-database"></a>Erstellen einer neuen SQL Server Express Datenbank

Beginnen Sie mit der rechten Maustaste auf das Webprojekt, und wählen Sie dann den Menübefehl **Add-&gt;New Item** :

![](create-a-database/_static/image1.png)

Dadurch wird das Dialogfeld "Neues Element hinzufügen" in Visual Studio angezeigt. Wir Filtern nach der Kategorie "Data" (Daten) und wählen die Element Vorlage "SQL Server Datenbank" aus:

![](create-a-database/_static/image2.png)

Benennen Sie die SQL Server Express Datenbank, die wir erstellen möchten, und klicken Sie auf OK. In Visual Studio werden Sie gefragt, ob Sie diese Datei dem Verzeichnis \app\_-Daten hinzufügen möchten (bei dem es sich um ein Verzeichnis handelt, das bereits mit Lese-und Schreib Sicherheits-ACLs eingerichtet wurde):

![](create-a-database/_static/image3.png)

Wir klicken auf "Ja", und die neue Datenbank wird erstellt und dem Projektmappen-Explorer hinzugefügt:

![](create-a-database/_static/image4.png)

### <a name="creating-tables-within-our-database"></a>Erstellen von Tabellen in der Datenbank

Wir verfügen nun über eine neue leere Datenbank. Fügen wir einige Tabellen hinzu.

Zu diesem Zweck navigieren wir in Visual Studio zum Registerkarten Fenster "Server-Explorer", das es uns ermöglicht, Datenbanken und Server zu verwalten. SQL Server Express Datenbanken, die im Ordner \app\_Data der Anwendung gespeichert sind, werden automatisch innerhalb der Server-Explorer angezeigt. Optional können Sie das Symbol "Verbindung mit Datenbank herstellen" oben im Fenster "Server-Explorer" verwenden, um der Liste zusätzliche SQL Server Datenbanken (sowohl lokal als auch Remote) hinzuzufügen:

![](create-a-database/_static/image5.png)

Wir fügen unserer "nerddinner Database" zwei Tabellen hinzu – eines zum Speichern unserer Abendessen und das andere, um die RSVP-Akzeptanz zu verfolgen. Wir können neue Tabellen erstellen, indem Sie mit der rechten Maustaste auf den Ordner "Tables" in der Datenbank klicken und den Menübefehl "neue Tabelle hinzufügen" auswählen:

![](create-a-database/_static/image6.png)

Dadurch wird ein Tabellen-Designer geöffnet, der es uns ermöglicht, das Schema der Tabelle zu konfigurieren. In der Tabelle "Dinner" (Dinner) werden 10 Datenspalten hinzugefügt:

![](create-a-database/_static/image7.png)

Wir möchten, dass die Spalte "dinnerid" ein eindeutiger Primärschlüssel für die Tabelle ist. Wir können dies konfigurieren, indem Sie mit der rechten Maustaste auf die Spalte "dinnerid" klicken und das Menü Element "Primärschlüssel festlegen" auswählen:

![](create-a-database/_static/image8.png)

Zusätzlich zum Erstellen von dinnerid als Primärschlüssel soll die Spalte auch als Identitäts Spalte konfiguriert werden, deren Wert automatisch erhöht wird, wenn der Tabelle neue Daten Zeilen hinzugefügt werden (was bedeutet, dass die erste eingefügte Dinner-Zeile eine dinnerid von 1, die zweite eingefügte Zeile hat). hat eine dinnerid von 2 usw.).

Wählen Sie hierzu die Spalte "dinnerid" aus, und legen Sie dann mit dem Editor "Spalten Eigenschaften" die Eigenschaft "(ist Identity)" für die Spalte auf "yes" fest. Wir verwenden die Standardeinstellungen der Identität (Start bei 1 und Inkrement 1 für jede neue Dinner-Zeile):

![](create-a-database/_static/image9.png)

Anschließend speichern wir die Tabelle durch Eingabe von STRG + S oder mithilfe des Menübefehls **File-&gt;Save** . Dadurch werden wir aufgefordert, die Tabelle zu benennen. Wir nennen es "Dinner":

![](create-a-database/_static/image10.png)

Unsere neue Dinner-Tabelle wird dann im Server-Explorer in unserer Datenbank angezeigt.

Anschließend wiederholen Sie die obigen Schritte und erstellen eine "RSVP"-Tabelle. Diese Tabelle verfügt über drei Spalten. Wir richten die rsvpid-Spalte als Primärschlüssel ein und machen Sie auch zu einer Identitäts Spalte:

![](create-a-database/_static/image11.png)

Wir speichern Sie und nennen Sie den Namen "RSVP".

### <a name="setting-up-a-foreign-key-relationship-between-tables"></a>Einrichten einer Fremdschlüssel Beziehung zwischen Tabellen

Wir verfügen jetzt über zwei Tabellen in der Datenbank. Der letzte Schema Entwurfs Schritt ist das Einrichten einer 1: n-Beziehung zwischen diesen beiden Tabellen – damit wir jede Dinner-Zeile mit 0 (null) oder mehr RSVP-Zeilen verknüpfen können, die für Sie gelten. Hierzu konfigurieren Sie die Spalte "dinnerid" der RSVP-Tabelle, um eine Fremdschlüssel Beziehung mit der Spalte "dinnerid" in der Tabelle "Dinner" zu erhalten.

Zu diesem Zweck öffnen Sie die RSVP-Tabelle im Tabellen-Designer, indem Sie im Server-Explorer darauf doppelklicken. Wählen Sie dann die Spalte "dinnerid" darin aus, klicken Sie mit der rechten Maustaste, und wählen Sie "Beziehungen..." aus. Kontextmenü Befehl:

![](create-a-database/_static/image12.png)

Dadurch wird ein Dialogfeld angezeigt, das zum Einrichten von Beziehungen zwischen Tabellen verwendet werden kann:

![](create-a-database/_static/image13.png)

Wir klicken auf die Schaltfläche "hinzufügen", um eine neue Beziehung zum Dialogfeld hinzuzufügen. Nachdem eine Beziehung hinzugefügt wurde, erweitern wir den Strukturansicht-Knoten "Tabellen-und Spaltenspezifikation" innerhalb des Eigenschaften Rasters rechts neben dem Dialogfeld, und klicken Sie dann auf "..." rechts neben der Schaltfläche:

![](create-a-database/_static/image14.png)

Klicken auf die Schaltfläche "..." Schaltfläche öffnet ein weiteres Dialogfeld, in dem Sie angeben können, welche Tabellen und Spalten an der Beziehung beteiligt sind, und dass wir die Beziehung benennen können.

Die Primärschlüssel Tabelle wird in "Dinner" geändert, und die Spalte "dinnerid" in der Tabelle "Dinner" wird als Primärschlüssel ausgewählt. Unsere RSVP-Tabelle ist die Fremdschlüssel Tabelle und die RSVP. Die dinnerid-Spalte wird als Fremdschlüssel zugeordnet:

![](create-a-database/_static/image15.png)

Nun wird jede Zeile in der RSVP-Tabelle einer Zeile in der Dinner-Tabelle zugeordnet. SQL Server behält die referenzielle Integrität für uns – und hindert uns daran, eine neue RSVP-Zeile hinzuzufügen, wenn Sie nicht auf eine gültige Dinner-Zeile verweist. Außerdem wird verhindert, dass wir eine Dinner-Zeile löschen, wenn noch RSVP-Zeilen darauf verweisen.

### <a name="adding-data-to-our-tables"></a>Hinzufügen von Daten zu den Tabellen

Abschließend fügen wir einige Beispiel Daten zu unserer Dinner-Tabelle hinzu. Wir können einer Tabelle Daten hinzufügen, indem Sie im Server-Explorer mit der rechten Maustaste darauf klicken und den Befehl "Tabellendaten anzeigen" auswählen:

![](create-a-database/_static/image16.png)

Wir fügen einige Zeilen mit Dinner Data hinzu, die Sie später verwenden können, wenn wir mit der Implementierung der Anwendung beginnen:

![](create-a-database/_static/image17.png)

### <a name="next-step"></a>Nächster Schritt

Die Erstellung unserer Datenbank ist abgeschlossen. Nun erstellen wir Modellklassen, die Sie verwenden können, um Sie abzufragen und zu aktualisieren.

> [!div class="step-by-step"]
> [Zurück](create-a-new-aspnet-mvc-project.md)
> [Weiter](build-a-model-with-business-rule-validations.md)
