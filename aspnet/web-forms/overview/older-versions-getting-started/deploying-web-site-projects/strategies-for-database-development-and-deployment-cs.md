---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/strategies-for-database-development-and-deployment-cs
title: Strategien für die Datenbankentwicklung undC#-Bereitstellung () | Microsoft-Dokumentation
author: rick-anderson
description: Wenn Sie eine datengesteuerte Anwendung zum ersten Mal bereitstellen, können Sie die Datenbank in der Entwicklungsumgebung Blind in die Produktionsumgebung kopieren. B...
ms.author: riande
ms.date: 04/23/2009
ms.assetid: 3e8b0627-3eb7-488e-807e-067cba7cec05
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/strategies-for-database-development-and-deployment-cs
msc.type: authoredcontent
ms.openlocfilehash: 4d9dbaf41926b43af171619ee34f58da84b5dab1
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78509385"
---
# <a name="strategies-for-database-development-and-deployment-c"></a>Strategien zur Datenbankentwicklung und -bereitstellung (C#)

von [Scott Mitchell](https://twitter.com/ScottOnWriting)

[PDF herunterladen](https://download.microsoft.com/download/C/3/9/C391A649-B357-4A7B-BAA4-48C96871FEA6/aspnet_tutorial10_DBDevel_cs.pdf)

> Wenn Sie eine datengesteuerte Anwendung zum ersten Mal bereitstellen, können Sie die Datenbank in der Entwicklungsumgebung Blind in die Produktionsumgebung kopieren. Durch das Ausführen einer Blind Kopie in nachfolgenden bereit Stellungen werden jedoch alle Daten überschrieben, die in die Produktionsdatenbank eingegeben werden. Stattdessen müssen bei der Bereitstellung einer Datenbank die Änderungen übernommen werden, die seit der letzten Bereitstellung auf der Produktionsdatenbank an der Entwicklungs Datenbank vorgenommen wurden. Dieses Tutorial behandelt diese Herausforderungen und bietet verschiedene Strategien zur Unterstützung bei der chronisierung und Anwendung der Änderungen, die seit der letzten Bereitstellung an der Datenbank vorgenommen wurden.

## <a name="introduction"></a>Einführung

Wie in den vorherigen Tutorials erläutert, bedeutet das Bereitstellen einer ASP.NET-Anwendung, dass der relevante Inhalt aus der Entwicklungsumgebung in die Produktionsumgebung kopiert wird. Bei der Bereitstellung handelt es sich nicht um ein einmalige Ereignis, sondern um etwas, das jedes Mal erfolgt, wenn eine neue Version der Software veröffentlicht wird oder wenn Fehler oder Sicherheitsprobleme identifiziert und behoben wurden. Beim Kopieren von ASP.NET-Seiten, Bildern, JavaScript-Dateien und anderen Dateien in die Produktionsumgebung müssen Sie sich nicht darum kümmern, wie diese Datei seit der letzten Bereitstellung geändert wurde. Sie können die Datei Blind in die Produktionsumgebung kopieren und den vorhandenen Inhalt überschreiben. Leider ist diese Einfachheit nicht auf die Bereitstellung der Datenbank ausgedehnt.

Wenn Sie eine datengesteuerte Anwendung zum ersten Mal bereitstellen, können Sie die Datenbank in der Entwicklungsumgebung Blind in die Produktionsumgebung kopieren. Durch das Ausführen einer Blind Kopie in nachfolgenden bereit Stellungen werden jedoch alle Daten überschrieben, die in die Produktionsdatenbank eingegeben werden. Stattdessen müssen bei der Bereitstellung einer Datenbank die *Änderungen* übernommen werden, die seit der letzten Bereitstellung auf der Produktionsdatenbank an der Entwicklungs Datenbank vorgenommen wurden. Dieses Tutorial behandelt diese Herausforderungen und bietet verschiedene Strategien zur Unterstützung bei der chronisierung und Anwendung der Änderungen, die seit der letzten Bereitstellung an der Datenbank vorgenommen wurden.

## <a name="the-challenges-of-deploying-a-database"></a>Die Herausforderungen bei der Bereitstellung einer Datenbank

Bevor eine datengesteuerte Anwendung zum ersten Mal bereitgestellt wurde, gibt es nur eine Datenbank, nämlich die Datenbank in der Entwicklungsumgebung. aus diesem Grund können Sie bei der erstmaligen Bereitstellung einer datengesteuerten Anwendung die Datenbank im Entwicklungsumgebung in der Produktionsumgebung. Nachdem die Anwendung bereitgestellt wurde, gibt es zwei Kopien der Datenbank: eine in der Entwicklung und eine in der Produktion.

Zwischen bereit Stellungen können die Entwicklungs-und Produktionsdatenbanken nicht mehr synchronisiert werden. Obwohl das Schema der Produktionsdatenbank unverändert bleibt, kann sich das Schema der Entwicklungs Datenbank ändern, wenn neue Funktionen hinzugefügt werden. Sie können Spalten, Tabellen, Sichten oder gespeicherte Prozeduren hinzufügen oder entfernen. Es können auch wichtige Daten vorhanden sein, die der Entwicklungs Datenbank hinzugefügt werden. Viele datengesteuerte Anwendungen enthalten Nachschlage Tabellen mit hart codierten anwendungsspezifischen Daten, die nicht vom Benutzer bearbeitet werden können. Beispielsweise kann eine Auktionswebsite eine Dropdown Liste mit Auswahlmöglichkeiten enthalten, die die Bedingung des zu versteigernden Elements beschreiben: neu, wie z. b. "neu", "gut" und "Fair". Anstatt diese Optionen direkt in der Dropdown Liste hart codieren zu müssen, ist es in der Regel besser, Sie in einer Datenbanktabelle zu platzieren. Wenn bei der Entwicklung der Tabelle eine neue Bedingung mit dem Namen "schlecht" hinzugefügt wird, muss der gleiche Datensatz bei der Bereitstellung der Anwendung der Nachschlage Tabelle in der Produktionsdatenbank hinzugefügt werden.

Im Idealfall umfasst das Bereitstellen der Datenbank das Kopieren der Datenbank aus der Entwicklung in die Produktion. Beachten Sie jedoch, dass die Produktionsdatenbank nach der Bereitstellung der Anwendung und der fortgesetzten Entwicklung mit echten Daten von echten Benutzern aufgefüllt wird. Wenn Sie die Datenbank bei der nächsten Bereitstellung aus der Entwicklung in die Produktion kopieren, würden Sie daher die Produktionsdatenbank überschreiben und die vorhandenen Daten verlieren. Das Ergebnis ist, dass die Bereitstellung der Datenbank auf die Anwendung der Änderungen zurückzuführen ist, die seit der letzten Bereitstellung an der Entwicklungs Datenbank vorgenommen wurden.

Das Bereitstellen einer Datenbank umfasst das Anwenden der Änderungen im Schema und ggf. die Daten seit der letzten Bereitstellung. der Verlauf der Änderungen muss beibehalten (oder zur Bereitstellung festgelegt werden), damit diese Änderungen auf die Produktion angewendet werden können. Es gibt eine Reihe von Techniken zum Verwalten und Anwenden von Änderungen am Datenmodell.

### <a name="defining-the-baseline"></a>Definieren der Baseline

Um die Änderungen an der Datenbank der Anwendung beizubehalten, benötigen Sie einen Anfangszustand, eine Baseline, auf die die Änderungen angewendet werden. Im Extremfall könnte der Startstatus eine leere Datenbank ohne Tabellen, Sichten oder gespeicherte Prozeduren sein. Eine solche Baseline führt zu einem großen Änderungsprotokoll, da es die Erstellung sämtlicher Tabellen, Sichten und gespeicherter Prozeduren der Datenbank sowie alle Änderungen beinhalten muss, die nach der ersten Bereitstellung vorgenommen wurden. Am anderen Ende des Spektrums können Sie die Baseline als Version der Datenbank festlegen, die anfänglich in der Produktionsumgebung bereitgestellt wird. Diese Auswahl führt zu einem wesentlich geringeren Änderungsprotokoll, da es nur die Änderungen enthält, die nach der ersten Bereitstellung an der Datenbank vorgenommen wurden. Dies ist der bevorzugte Ansatz. Natürlich können Sie auch einen weiteren Mittelpunkt der Straße wählen, indem Sie die Baseline als einen Punkt zwischen der ersten Erstellung der Datenbank und der ersten Bereitstellung der Datenbank definieren.

Wenn Sie eine Baseline ausgewählt haben, sollten Sie ein SQL-Skript erstellen, das ausgeführt werden kann, um die Baselineversion neu zu erstellen Mit einem solchen Skript können Sie schnell die Baseline-Version der Datenbank neu erstellen. Diese Funktion ist besonders nützlich in größeren Projekten, in denen es möglicherweise mehrere Entwickler am Projekt oder in zusätzlichen Umgebungen, wie z. b. Tests oder Staging, arbeiten, die jeweils eine eigene Kopie der Datenbank benötigen.

Es stehen eine Vielzahl von Tools zur Verfügung, um ein SQL-Skript der Baselineversion zu generieren. Über SQL Server Management Studio (SSMS) können Sie mit der rechten Maustaste auf die Datenbank klicken, im Untermenü Aufgaben auf die Option Skripts generieren klicken. Dadurch wird der Skript-Assistent gestartet, den Sie anweisen können, eine Datei zu generieren, die die SQL-Befehle zum Erstellen der Objekte der Datenbank enthält. Eine weitere Option ist der Datenbankveröffentlichungs-Assistent, der die SQL-Befehle generieren kann, um nicht nur das Datenbankschema zu erstellen, sondern auch die Daten in den Datenbanktabellen. Der Datenbankveröffentlichungs-Assistent wurde im Tutorial bereitstellen *einer Datenbank* ausführlich erläutert. Unabhängig davon, welches Tool Sie verwenden, sollten Sie am Ende über eine Skriptdatei verfügen, mit der Sie die Baselineversion Ihrer Datenbank neu erstellen können, falls dies erforderlich ist.

## <a name="documenting-the-database-changes-in-prose"></a>Dokumentieren der Daten Bank Änderungen in der Prosa

Die einfachste Möglichkeit, ein Protokoll der Änderungen am Datenmodell während der Entwicklungsphase beizubehalten, besteht darin, die Änderungen in der Prosa aufzuzeichnen. Wenn Sie z. b. während der Entwicklung einer bereits bereitgestellten Anwendung der `Employees` Tabelle eine neue Spalte hinzufügen, eine Spalte aus der `Orders` Tabelle entfernen und eine neue Tabelle (`ProductCategories`) hinzufügen, erhalten Sie eine Textdatei oder ein Microsoft Word-Dokument mit folgendem Verlauf:

<a id="0.4_table01"></a>

| **Änderungsdatum** | **Details ändern** |
| --- | --- |
| 2009-02-03: | Der `Employees` Tabelle wurde die Spalten `DepartmentID` (`int`, nicht null) hinzugefügt. Eine FOREIGN KEY-Einschränkung wurde von `Departments.DepartmentID` zu `Employees.DepartmentID`hinzugefügt. |
| 2009-02-05: | Die Spalten `TotalWeight` aus der `Orders` Tabelle wurde entfernt. Daten wurden bereits in zugeordneten `OrderDetails` Datensätzen aufgezeichnet. |
| 2009-02-12: | Die `ProductCategories` Tabelle wurde erstellt. Es gibt drei Spalten: `ProductCategoryID` (`int`, `IDENTITY`, `NOT NULL`), `CategoryName` (`nvarchar(50)`, `NOT NULL`) und `Active` (`bit`, `NOT NULL`). Der `ProductCategoryID`wurde eine PRIMARY KEY-Einschränkung und der Standardwert 1 für `Active`hinzugefügt. |

Dieser Ansatz hat eine Reihe von Nachteilen. Zunächst einmal gibt es keine Hoffnung für die Automatisierung. Jedes Mal, wenn diese Änderungen auf eine Datenbank angewendet werden müssen, z. b. wenn die Anwendung bereitgestellt wird, muss ein Entwickler jede Änderung nacheinander manuell implementieren. Wenn Sie darüber hinaus eine bestimmte Version der Datenbank aus der Baseline mithilfe des Änderungsprotokolls rekonstruieren müssen, nimmt dies mehr und mehr Zeit in Anspruch, wenn die Größe des Protokolls zunimmt. Ein weiterer Nachteil dieser Methode besteht darin, dass die Klarheit und Detailgenauigkeit jedes Änderungsprotokoll Eintrags für die Person verbleibt, die die Änderung aufgezeichnet hat. In einem Team mit mehreren Entwicklern können einige ausführlichere, besser lesbare oder präzisere Einträge als andere vornehmen. Außerdem sind typpos und andere personenbezogene Dateneingabe Fehler möglich.

Der Hauptvorteil der Dokumentation der Daten Bank Änderungen in der Prosa ist die Einfachheit. Sie müssen nicht mit der SQL-Syntax zum Erstellen und Ändern von Datenbankobjekten vertraut sein. Stattdessen können Sie die Änderungen in der Prosa aufzeichnen und mithilfe der grafischen Benutzeroberfläche SQL Server Management Studio s implementieren.

Das Verwalten Ihres Änderungsprotokolls in der Prosa ist zugegebenermaßen nicht sehr komplex und funktioniert nicht gut mit bestimmten Projekten, wie z. b. denen, die im Bereich liegen, häufige Änderungen am Datenmodell aufweisen oder mehrere Entwickler einbeziehen. Aber ich habe gesehen, dass diese Vorgehensweise in kleinen Projekten mit nur einem Mann sehr gut funktioniert, die nur gelegentlich Änderungen am Datenmodell aufweisen und dass der Einzelentwickler keinen starken Hintergrund in der SQL-Syntax zum Erstellen und Ändern von Datenbankobjekten hat.

> [!NOTE]
> Obwohl die Informationen im Änderungsprotokoll, technisch gesehen, nur für die Bereitstellung benötigt werden, empfehle ich, einen Verlauf der Änderungen beizubehalten. Anstatt eine einzelne, immer wachsende Änderungsprotokoll Datei zu verwalten, sollten Sie jedoch eine andere Änderungsprotokoll Datei für jede Datenbankversion haben. In der Regel ist es ratsam, die Datenbank bei jeder Bereitstellung zu Versions. Durch die Beibehaltung eines Protokolls von Änderungs Protokollen können Sie ab der Baseline eine beliebige Datenbankversion neu erstellen, indem Sie die Änderungsprotokoll Skripts ab Version 1 ausführen und fortfahren, bis Sie die Version erreichen, die Sie neu erstellen müssen.

## <a name="recording-the-sql-change-statements"></a>Aufzeichnen der SQL-Änderungs Anweisungen

Der Hauptnachteil der Beibehaltung des Änderungsprotokolls in der Prosa ist die fehlende Automatisierung. Im Idealfall wäre das Implementieren der Daten Bank Änderungen in der Produktionsdatenbank zur Bereitstellungs Zeit genauso einfach wie das Klicken auf eine Schaltfläche, um ein Skript auszuführen, anstatt manuell eine Liste von Anweisungen auszuführen. Eine solche Automatisierung ist möglich, indem ein Änderungsprotokoll verwaltet wird, das die SQL-Befehle enthält, die zum Ändern des Datenmodells verwendet werden.

Die SQL-Syntax umfasst eine Reihe von Anweisungen zum Erstellen und Ändern verschiedener Datenbankobjekte. Beispielsweise wird mit der [*CREATE TABLE-Anweisung*](https://msdn.microsoft.com/library/ms174979.aspx)eine neue Tabelle mit den angegebenen Spalten und Einschränkungen erstellt, wenn Sie ausgeführt wird. Mit der [*ALTER TABLE-Anweisung*](https://msdn.microsoft.com/library/ms190273.aspx) wird eine vorhandene Tabelle geändert, um die Spalten oder Einschränkungen hinzuzufügen, zu entfernen oder zu ändern. Es gibt auch Anweisungen zum Erstellen, ändern und Löschen von Indizes, Sichten, benutzerdefinierten Funktionen, gespeicherten Prozeduren, Triggern und anderen Datenbankobjekten.

Wenn Sie zum vorherigen Beispiel zurückkehren, wird das Image, dass Sie während der Entwicklung einer bereits bereitgestellten Anwendung der `Employees` Tabelle eine neue Spalte hinzufügen, eine Spalte aus der `Orders` Tabelle entfernen und eine neue Tabelle (`ProductCategories`) hinzufügen. Solche Aktionen führen zu einer Änderungsprotokoll Datei mit den folgenden SQL-Befehlen:

[!code-sql[Main](strategies-for-database-development-and-deployment-cs/samples/sample1.sql)]

Das Übertragen dieser Änderungen an die Produktionsdatenbank während der Bereitstellung ist ein One-Click-Vorgang: Öffnen Sie SQL Server Management Studio, stellen Sie eine Verbindung mit der Produktionsdatenbank her, öffnen Sie ein neues Abfragefenster, fügen Sie den Inhalt des Änderungsprotokolls ein, und klicken Sie auf Ausführen, um das Skript auszuführen.

## <a name="using-a-comparison-tool-to-synchronize-the-data-models"></a>Verwenden eines Vergleichs Tools zum Synchronisieren der Datenmodelle

Das Dokumentieren von Daten Bank Änderungen in der Prosa ist einfach, aber das Implementieren der Änderungen erfordert, dass ein Entwickler jede Änderung in der Produktionsdatenbank nacheinander vornimmt. das Dokumentieren der Change SQL-Befehle bewirkt, dass diese Änderungen in der Produktionsdatenbank so einfach und schnell wie beim Klicken auf eine Schaltfläche implementiert werden. Allerdings müssen Sie die SQL-Anweisungen und die Syntax zum Erstellen und Ändern von Datenbankobjekten erlernen. Daten Bank Vergleichs Tools erzielen am besten von beiden Ansätzen und verwerfen den schlechtesten.

Ein Daten Bank Vergleichstool vergleicht das Schema oder die Daten zweier Datenbanken und zeigt einen Zusammenfassungs Bericht an, der anzeigt, wie sich die Datenbanken unterscheiden. Anschließend können Sie mit dem Klicken auf eine Schaltfläche die SQL-Befehle zum Synchronisieren eines oder mehrerer Datenbankobjekte generieren. Kurz gesagt, Sie können ein Daten Bank Vergleichstool verwenden, um die Entwicklungs-und Produktionsdatenbanken zum Zeitpunkt der Bereitstellung zu vergleichen und eine Datei zu erstellen, die die SQL-Befehle enthält, die bei der Ausführung die Änderungen auf das s-Schema der Produktionsdatenbank anwenden. spiegelt das Schema der Entwicklungs Datenbank wider.

Es gibt eine Vielzahl von Daten Bank Vergleichs Tools von Drittanbietern, die von vielen unterschiedlichen Anbietern angeboten werden. Ein Beispiel hierfür ist [*SQL Compare*](http://www.red-gate.com/products/SQL_Compare/)von [*Red Gate Software*](http://www.red-gate.com/). Im folgenden wird erläutert, wie Sie SQL Compare zum Vergleichen und Synchronisieren der Entwicklungs-und Produktionsdaten Bank Schemas verwenden.

> [!NOTE]
> Zum Zeitpunkt der Erstellung dieses Artikels lautete die aktuelle Version von SQL Compare in Version 7,1, bei der Standard Edition $395. Hierzu können Sie eine kostenlose 14-Tage-Testversion herunterladen.

Wenn SQL Compare gestartet wird, wird das Dialogfeld Vergleichs Projekte geöffnet, in dem die gespeicherten SQL Compare-Projekte angezeigt werden. Erstellen Sie ein neues Projekt. Dadurch wird der Projektkonfigurations-Assistent gestartet, der Informationen zu den zu vergleichenden Datenbanken auffordert (siehe Abbildung 1). Geben Sie die Informationen für die Entwicklungs-und Produktions Umgebungs Datenbanken ein.

[![vergleichen der Entwicklungs-und Produktionsdatenbanken](strategies-for-database-development-and-deployment-cs/_static/image2.jpg)](strategies-for-database-development-and-deployment-cs/_static/image1.jpg)

**Abbildung 1**: Vergleichen der Entwicklungs-und Produktionsdatenbanken ([Klicken Sie, um das Bild in voller Größe anzuzeigen](strategies-for-database-development-and-deployment-cs/_static/image3.jpg))

> [!NOTE]
> Wenn es sich bei der Entwicklungs Umgebungs Datenbank um eine SQL Express Edition-Datenbankdatei im Ordner `App_Data` Ihrer Website handelt, müssen Sie die Datenbank auf dem SQL Server Express Daten Bank Server registrieren, um Sie aus dem in Abbildung 1 dargestellten Dialogfeld auszuwählen. Dies lässt sich am einfachsten erreichen, indem Sie SQL Server Management Studio (SSMS) öffnen, eine Verbindung mit dem SQL Server Express Daten Bank Server herstellen und die Datenbank anfügen. Wenn SSMS nicht auf dem Computer installiert ist, können Sie den kostenlosen [*SQL Server 2008 Management Studio Basic-Version*](https://www.microsoft.com/downloads/details.aspx?FamilyId=7522A683-4CB2-454E-B908-E805E9BD4E28&amp;displaylang=en)herunterladen und installieren.

Zusätzlich zur Auswahl der zu vergleichenden Datenbanken können Sie auf der Registerkarte "Optionen" auch eine Vielzahl von Vergleichs Einstellungen angeben. Eine Möglichkeit, die Sie aktivieren möchten, ist die "Einschränkung von Einschränkungs-und Indexnamen". Beachten Sie, dass im vorherigen Tutorial die Anwendungs Dienst-Datenbankobjekte zu den Entwicklungs-und Produktionsdatenbanken hinzugefügt wurden. Wenn Sie das `aspnet_regsql.exe` Tool verwendet haben, um diese Objekte in der Produktionsdatenbank zu erstellen, werden Sie feststellen, dass sich die Namen der PRIMARY KEY-und Unique-Einschränkung zwischen den Entwicklungs-und Produktionsdatenbanken unterscheiden. Folglich werden in SQL Compare alle Tabellen der Anwendungsdienste als unterschiedlich gekennzeichnet. Sie können entweder die Option "Einschränkungs-und Indexnamen ignorieren" deaktiviert lassen und die Einschränkungs Namen synchronisieren oder den SQL-Vergleich anweisen, diese Unterschiede zu ignorieren.

Nachdem Sie die zu vergleichenden Datenbanken ausgewählt (und die Vergleichs Optionen überprüft haben), klicken Sie auf die Schaltfläche jetzt vergleichen, um den Vergleich zu beginnen. In den nächsten Sekunden untersucht SQL Compare die Schemas der beiden Datenbanken und generiert einen Bericht darüber, wie sich diese unterscheiden. Ich habe absichtlich einige Änderungen an der Entwicklungs Datenbank vorgenommen, um zu veranschaulichen, wie diese Abweichungen in der SQL Compare-Schnittstelle vermerkt werden. Wie in Abbildung 2 gezeigt, habe ich der Tabelle "`Authors`" eine `BirthDate` Spalte hinzugefügt, die Spalte "`ISBN`" aus der `Books` Tabelle entfernt und eine neue Tabelle hinzugefügt `Ratings`, mit der die Benutzer auf der Website die überprüften Bücher besuchen können.

> [!NOTE]
> Die Änderungen an den Datenmodellen, die in diesem Tutorial vorgenommen wurden, wurden mit einem Daten Bank Vergleichstool veranschaulicht. Sie finden diese Änderungen in zukünftigen Tutorials nicht in der Datenbank.

[in ![SQL-Vergleich werden die Unterschiede zwischen den Entwicklungs-und Produktionsdatenbanken aufgeführt.](strategies-for-database-development-and-deployment-cs/_static/image5.jpg)](strategies-for-database-development-and-deployment-cs/_static/image4.jpg)

**Abbildung 2**: SQL Compare listet die Unterschiede zwischen den Entwicklungs-und Produktionsdatenbanken auf ([Klicken Sie, um das Bild in voller Größe anzuzeigen](strategies-for-database-development-and-deployment-cs/_static/image6.jpg))

Der SQL-Vergleich teilt die Datenbankobjekte in Gruppen auf und zeigt Ihnen schnell, welche Objekte in beiden Datenbanken vorhanden sind, aber unterschiedlich sind, welche Objekte in einer Datenbank, aber nicht in der anderen vorhanden sind und welche Objekte identisch sind. Wie Sie sehen können, gibt es zwei Objekte, die in beiden Datenbanken vorhanden sind, sich jedoch unterscheiden: die `Authors` Tabelle, der eine Spalte hinzugefügt wurde, und die `Books` Tabelle, für die eine Spalte entfernt wurde. Es gibt ein-Objekt, das nur in der Entwicklungs Datenbank vorhanden ist, nämlich die neu erstellte `Ratings` Tabelle. Und es gibt 117-Objekte, die in beiden Datenbanken identisch sind.

Wenn Sie ein Datenbankobjekt auswählen, wird das Fenster SQL-Unterschiede angezeigt Das Fenster SQL-Unterschiede, das am unteren Rand in Abbildung 2 angezeigt wird, verdeutlicht, dass die `Authors` Tabelle in der Entwicklungs Datenbank die `BirthDate` Spalte enthält, die in der `Authors`-Tabelle der Produktionsdatenbank nicht gefunden wurde.

Nachdem Sie die Unterschiede überprüft und ausgewählt haben, welche Objekte synchronisiert werden sollen, besteht der nächste Schritt darin, die SQL-Befehle zu generieren, die erforderlich sind, um das s-Schema der Produktionsdatenbank entsprechend der Entwicklungs Datenbank zu aktualisieren Dies erfolgt über den Synchronisierungs-Assistenten. Der Synchronisierungs-Assistent bestätigt, welche Objekte synchronisiert werden sollen, und fasst den Aktionsplan zusammen (siehe Abbildung 3). Sie können die Datenbanken sofort synchronisieren oder ein Skript mit den SQL-Befehlen generieren, die Sie in ihrer Freizeit ausführen können.

[![mit dem Synchronisierungs-Assistenten Ihre Datenbankschemas synchronisieren](strategies-for-database-development-and-deployment-cs/_static/image8.jpg)](strategies-for-database-development-and-deployment-cs/_static/image7.jpg)

**Abbildung 3**: Verwenden des Synchronisierungs-Assistenten zum Synchronisieren der Datenbankschemas ([Klicken Sie, um das Bild in voller Größe anzuzeigen](strategies-for-database-development-and-deployment-cs/_static/image9.jpg))

Daten Bank Vergleichs Tools wie Red Gate Software s SQL Compare machen das Anwenden der Änderungen auf das Schema der Entwicklungs Datenbank so einfach wie Point und Click.

> [!NOTE]
> SQL Compare vergleicht und synchronisiert zwei Daten Bank *Schemas*. Leider werden die Daten in zwei Datenbanktabellen nicht verglichen und synchronisiert. Die Red Gate-Software bietet ein Produkt mit dem Namen [*SQL Data Compare*](http://www.red-gate.com/products/SQL_Data_Compare/) , das die Daten zwischen zwei Datenbanken vergleicht und synchronisiert. es handelt sich jedoch um ein separates Produkt aus SQL-vergleichen und Kosten mit einem weiteren $395.

## <a name="taking-the-application-offline-during-deployment"></a>Offline schalten der Anwendung während der Bereitstellung

Wie wir in diesen Tutorials gesehen haben, ist die Bereitstellung ein Prozess, der mehrere Schritte umfasst: Kopieren der ASP.NET-Seiten, Masterseiten, CSS-Dateien, JavaScript-Dateien, Bilder und anderer erforderlicher Inhalte aus der Entwicklungsumgebung in die Produktionsumgebung. Umgebung nach Bedarf werden die Konfigurationsinformationen für die Produktionsumgebung kopiert. und Anwenden der Änderungen auf das Datenmodell seit der letzten Bereitstellung. Abhängig von der Anzahl der Dateien und der Komplexität der Änderungen an der Datenbank kann es zwischen wenigen Sekunden und mehreren Minuten dauern, bis diese Schritte abgeschlossen sind. Während dieses Fensters befindet sich die Webanwendung in Flux, und Benutzer, die die Website besuchen, können Fehler oder unerwartetes Verhalten auftreten.

Beim Bereitstellen einer Website ist es am besten, die Webanwendung "Offline" zu nehmen, bis die Bereitstellung abgeschlossen ist. Wenn Sie die Anwendung offline schalten (und Sie nach Abschluss des Bereitstellungs Prozesses wieder in Betrieb nehmen), ist das Hochladen einer Datei und das anschließende Löschen von Dateien ganz einfach. Ab ASP.NET 2,0 wird durch das bloße vorhanden sein einer Datei mit dem Namen `app_offline.htm` im Stammverzeichnis der Anwendung die gesamte Website "Offline". Jede Anforderung an eine ASP.NET-Seite auf dieser Website wird automatisch mit dem Inhalt der `app_offline.htm` Datei beantwortet. Nachdem diese Datei entfernt wurde, wird die Anwendung wieder online geschaltet.

Wenn eine Anwendung während der Bereitstellung offline geschaltet wird, ist es so einfach wie das Hochladen einer `app_offline.htm` Datei in das Stammverzeichnis der Produktionsumgebung vor dem Start des Bereitstellungs Prozesses und dem anschließenden löschen (oder umbenennen in etwas anderes) nach Abschluss der Bereitstellung. Weitere Informationen zu diesem Verfahren finden Sie im Artikel über das offline schalten einer [*ASP.NET-Anwendung*](http://www.15seconds.com/issue/061207.htm)in John Peterson.

## <a name="summary"></a>Zusammenfassung

Die größte Herausforderung beim Bereitstellen einer datengesteuerten Anwendung liegt auf der Bereitstellung der Datenbank. Da es zwei Versionen der-Datenbank gibt: eine in der Entwicklungsumgebung und eine in der Produktionsumgebung. diese beiden Datenbankschemas können nicht mehr synchronisiert werden, da neue Features in der Entwicklung hinzugefügt werden. Wenn die Produktionsdatenbank mit echten Daten von echten Benutzern aufgefüllt wird, können Sie die Produktionsdatenbank nicht mit der geänderten Entwicklungs Datenbank überschreiben, wie Sie es beim Bereitstellen der Dateien, aus denen die Anwendung besteht (die ASP.NET Seiten, Bilddateien usw.). Stattdessen muss bei der Bereitstellung einer Datenbank der genaue Satz von Änderungen implementiert werden, die seit der letzten Bereitstellung an der Entwicklungs Datenbank in der Produktionsdatenbank vorgenommen wurden.

In diesem Tutorial wurden drei Techniken zum Verwalten und Anwenden eines Protokolls von Daten Bank Änderungen behandelt. Der einfachste Ansatz besteht darin, die Änderungen in der Prosa aufzuzeichnen. Diese Taktik bewirkt zwar, dass diese Änderungen in der Produktionsdatenbank manuell verarbeitet werden, aber es ist nicht erforderlich, die SQL-Befehle zum Erstellen und Ändern von Datenbankobjekten zu kennen. Ein anspruchsvolleres Verfahren, das in größeren Projekten oder Projekten mit mehreren Entwicklern weitaus besser erkennbar ist, besteht darin, die Änderungen als eine Reihe von SQL-Befehlen aufzuzeichnen. Dies hat deutlich zu tun, dass diese Änderungen in der Zieldatenbank durchgängig werden. Das Beste aus beiden Ansätzen kann mit einem Daten Bank Vergleichstool erzielt werden, wie z. b. Red Gate Software s SQL Compare.

Dieses Tutorial schließt den Schwerpunkt auf die Bereitstellung einer datengesteuerten Anwendung ab. In den nächsten Tutorials wird erläutert, wie auf Fehler in der Produktionsumgebung reagiert wird. Wir sehen uns an, wie anstelle des gelben Bildschirms eine benutzerfreundliche Fehlerseite angezeigt wird. Und wir sehen, wie die Fehlerdetails protokolliert werden und wie Sie benachrichtigt werden, wenn derartige Fehler auftreten.

Fröhliche Programmierung!

> [!div class="step-by-step"]
> [Zurück](configuring-a-website-that-uses-application-services-cs.md)
> [Weiter](displaying-a-custom-error-page-cs.md)
