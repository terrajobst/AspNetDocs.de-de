---
uid: mvc/overview/older-versions-1/contact-manager/iteration-1-create-the-application-vb
title: 'Iterations #1 – Erstellen der Anwendung (VB) | Microsoft-Dokumentation'
author: microsoft
description: 'In der ersten Iterationen erstellen wir den Kontakt-Manager auf einfachste Art und Weise. Wir fügen Unterstützung für grundlegende Daten Bank Vorgänge hinzu: erstellen, lesen, aktualisieren und D...'
ms.author: riande
ms.date: 02/20/2009
ms.assetid: 5b033582-1646-42c2-b20d-7edc8814e970
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-1-create-the-application-vb
msc.type: authoredcontent
ms.openlocfilehash: c6bf4712fb734cf14420fd62c9eaf190a2c28168
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78487569"
---
# <a name="iteration-1--create-the-application-vb"></a>Iterations #1 – Erstellen der Anwendung (VB)

von [Microsoft](https://github.com/microsoft)

[Code herunterladen](iteration-1-create-the-application-vb/_static/contactmanager_1_vb1.zip)

> In der ersten Iterationen erstellen wir den Kontakt-Manager auf einfachste Art und Weise. Wir fügen Unterstützung für grundlegende Daten Bank Vorgänge hinzu: Create, Read, Update und DELETE (CRUD).

## <a name="building-a-contact-management-aspnet-mvc-application-vb"></a>Aufbauen einer Kontakt Verwaltung ASP.NET MVC-Anwendung (VB)

In dieser Reihe von Tutorials erstellen wir von Anfang bis Ende eine gesamte Kontakt Verwaltungs Anwendung. Mithilfe der Kontakt-Manager-Anwendung können Sie Kontaktinformationen (Namen, Telefonnummern und e-Mail-Adressen) für eine Liste von Personen speichern.

Die Anwendung wird über mehrere Iterationen erstellt. Bei jeder Iterationen verbessern wir die Anwendung allmählich. Das Ziel dieses mehrfaches Iterations Ansatzes besteht darin, Ihnen zu ermöglichen, den Grund für jede Änderung zu verstehen.

- Iterations #1: Erstellen Sie die Anwendung. In der ersten Iterationen erstellen wir den Kontakt-Manager auf einfachste Art und Weise. Wir fügen Unterstützung für grundlegende Daten Bank Vorgänge hinzu: Create, Read, Update und DELETE (CRUD).

- Iterations #2: machen Sie das Aussehen der Anwendung schön. In dieser Iterationen verbessern wir die Darstellung der Anwendung durch Ändern der standardmäßigen ASP.NET-MVC-Ansichts Master Seite und des Cascading Stylesheets.

- Iterations #3: Formular Validierung hinzufügen. In der dritten Iterationen fügen wir die grundlegende Formular Validierung hinzu. Wir hindern Personen daran, ein Formular zu senden, ohne die erforderlichen Formularfelder abzuschließen. Wir überprüfen auch die e-Mail-Adressen und Telefonnummern.

- Iterations #4: Legen Sie die Anwendung lose gekoppelt. In dieser vierten Iterationen nutzen wir mehrere Software Entwurfsmuster, um die Verwaltung und Änderung der Contact Manager-Anwendung zu vereinfachen. Beispielsweise können wir unsere Anwendung so umgestalten, dass Sie das Repository-Muster und das Muster für die Abhängigkeitsinjektion verwendet.

- Iterations #5: Erstellen von Komponententests. In der fünften Iterationen wird die Wartung und Änderung unserer Anwendung durch Hinzufügen von Komponententests vereinfacht. Wir simulieren unsere Datenmodell Klassen und erstellen Komponententests für unsere Controller und Validierungs Logik.

- Iterations #6: Verwenden Sie die Test gesteuerte Entwicklung. In dieser sechsten Iterationen fügen wir der Anwendung neue Funktionen hinzu, indem wir zuerst Komponententests schreiben und Code für die Komponententests schreiben. In dieser Iterationen fügen wir Kontaktgruppen hinzu.

- Iterations #7: Hinzufügen von AJAX-Funktionen. In der siebten Iterationen verbessern wir die Reaktionsfähigkeit und Leistung unserer Anwendung durch Hinzufügen von Unterstützung für AJAX.

## <a name="this-iteration"></a>Diese Iterations

In dieser ersten Iterationen wird die Basisanwendung erstellt. Ziel ist es, den Kontakt-Manager so schnell und einfach wie möglich zu erstellen. In späteren Iterationen verbessern wir das Design der Anwendung.

Die Kontakt-Manager-Anwendung ist eine grundlegende datenbankgesteuerte Anwendung. Sie können die Anwendung verwenden, um neue Kontakte zu erstellen, vorhandene Kontakte zu bearbeiten und Kontakte zu löschen.

In dieser Iterationen führen wir die folgenden Schritte aus:

1. ASP.NET MVC-Anwendung
2. Erstellen einer Datenbank zum Speichern unserer Kontakte
3. Generieren Sie eine Modell Klasse für unsere Datenbank mit dem Microsoft-Entity Framework
4. Erstellen Sie eine Controller Aktion und eine Ansicht, mit der wir alle Kontakte in der Datenbank auflisten können.
5. Erstellen Sie Controller Aktionen und eine Ansicht, mit der wir einen neuen Kontakt in der Datenbank erstellen können.
6. Erstellen Sie Controller Aktionen und eine Ansicht, mit der wir einen vorhandenen Kontakt in der Datenbank bearbeiten können.
7. Controller Aktionen und eine Ansicht erstellen, mit der ein vorhandener Kontakt in der Datenbank gelöscht werden kann

## <a name="software-prerequisites"></a>Erforderliche Software

In ASP.NET MVC-Anwendungen muss entweder Visual Studio 2008 oder Visual Web Developer 2008 auf dem Computer installiert sein (Visual Web Developer ist eine kostenlose Version von Visual Studio, die nicht alle erweiterten Features von Visual Studio umfasst). Sie können die Testversion von Visual Studio 2008 oder Visual Web Developer von der folgenden Adresse herunterladen:

[https://www.asp.net/downloads/essential/](https://www.asp.net/downloads/essential)

> [!NOTE] 
> 
> Für ASP.NET-MVC-Anwendungen mit Visual Web Developer muss Visual Web Developer Service Pack 1 installiert sein. Ohne Service Pack 1 können keine Webanwendungs Projekte erstellt werden.

ASP.NET-MVC-Framework. Sie können das ASP.NET MVC-Framework von der folgenden Adresse herunterladen:

[https://www.asp.net/mvc](../../../index.md)

In diesem Tutorial verwenden wir den Microsoft-Entity Framework, um auf eine Datenbank zuzugreifen. Der Entity Framework ist in .NET Framework 3,5 Service Pack 1 enthalten. Sie können diese Service Pack von folgendem Speicherort herunterladen:

[https://www.microsoft.com/downloads/details.aspx?familyid=ab99342f-5d1a-413d-8319-81da479ab0d7&amp;d isplaylang = en](https://www.microsoft.com/downloads/details.aspx?familyid=ab99342f-5d1a-413d-8319-81da479ab0d7&amp;displaylang=en)

Als Alternative zur Durchführung dieser Downloads können Sie den Webplattform-Installer (Web PI) nutzen. Sie können die Web Pi unter folgender Adresse herunterladen:

[https://www.asp.net/downloads/essential/](https://www.asp.net/downloads/essential)

## <a name="aspnet-mvc-project"></a>ASP.NET-MVC-Projekt

ASP.NET MVC-Webanwendungs Projekt. Starten Sie Visual Studio, und wählen Sie die Menü Options **Datei, neues Projekt**aus. Das Dialogfeld " **Neues Projekt** " wird angezeigt (siehe Abbildung 1). Wählen Sie den webprojekttyp und die Vorlage **ASP.NET MVC-Webanwendung** aus. Nennen Sie das neue Projekt *ContactManager* , und klicken Sie auf die Schaltfläche OK.

Stellen Sie sicher, dass in der Dropdown Liste oben rechts im Dialogfeld " **Neues Projekt** " .NET Framework 3,5 ausgewählt ist. Andernfalls wird die Vorlage ASP.NET MVC-Webanwendung nicht angezeigt.

[![des Dialog Felds "Neues Projekt"](iteration-1-create-the-application-vb/_static/image1.jpg)](iteration-1-create-the-application-vb/_static/image1.png)

**Abbildung 01**: das Dialogfeld "Neues Projekt" ([Klicken Sie, um das Bild in voller Größe anzuzeigen](iteration-1-create-the-application-vb/_static/image2.png))

ASP.NET MVC-Anwendung, wird das Dialogfeld Komponenten **Test Projekt erstellen** angezeigt. Mithilfe dieses Dialog Felds können Sie angeben, dass Sie ein Komponenten Testprojekt erstellen und zu ihrer Projekt Mappe hinzufügen möchten, wenn Sie die ASP.NET MVC-Anwendung erstellen. Obwohl wir in dieser Iterationen keine Komponententests erstellen, sollten Sie die Option **Ja, ein Komponenten Testprojekt erstellen** , da wir Vorhaben, Komponententests in einer späteren Iterationen hinzuzufügen. Das Hinzufügen eines Testprojekts, wenn Sie ein neues ASP.NET-MVC-Projekt erstellen, ist wesentlich einfacher als das Hinzufügen eines Testprojekts, nachdem das ASP.NET-MVC-Projekt erstellt wurde.

> [!NOTE] 
> 
> Da Visual Web Developer keine Test Projekte unterstützt, wird das Dialogfeld Komponenten Test Projekt erstellen bei Verwendung von Visual Web Developer nicht angezeigt.

[![des Dialog Felds "Neues Projekt"](iteration-1-create-the-application-vb/_static/image2.jpg)](iteration-1-create-the-application-vb/_static/image3.png)

**Abbildung 02**: das Dialogfeld zum Erstellen eines Komponenten Test Projekts ([Klicken Sie, um das Bild in voller Größe anzuzeigen](iteration-1-create-the-application-vb/_static/image4.png))

ASP.NET MVC-Anwendung wird im Fenster Visual Studio Projektmappen-Explorer angezeigt (siehe Abbildung 3). Wenn Sie das Projektmappen-Explorer Fenster nicht sehen, können Sie dieses Fenster öffnen, indem Sie die Menü Options **Ansicht Projektmappen-Explorer**auswählen. Beachten Sie, dass die Projekt Mappe zwei Projekte enthält: das ASP.NET MVC-Projekt und das Test Projekt. Das ASP.NET-MVC-Projekt heißt ContactManager, und das Test Projekt heißt ContactManager. Tests.

[![des Dialog Felds "Neues Projekt"](iteration-1-create-the-application-vb/_static/image3.jpg)](iteration-1-create-the-application-vb/_static/image5.png)

**Abbildung 03**: das Fenster "Projektmappen-Explorer" ([Klicken Sie, um das Bild in voller Größe anzuzeigen](iteration-1-create-the-application-vb/_static/image6.png))

## <a name="deleting-the-project-sample-files"></a>Löschen der Projektbeispiel Dateien

Die ASP.NET MVC-Projektvorlage enthält Beispieldateien für Controller und Ansichten. Vor dem Erstellen einer neuen ASP.NET MVC-Anwendung sollten Sie diese Dateien löschen. Sie können Dateien und Ordner im Fenster Projektmappen-Explorer löschen, indem Sie mit der rechten Maustaste auf eine Datei oder einen Ordner klicken und die Menüoption **Löschen**auswählen.

Sie müssen die folgenden Dateien aus dem ASP.NET MVC-Projekt löschen:

- \Controllers\HomeController.vb

- \Views\home\about.aspx

- \Views\home\index.aspx

Und Sie müssen die folgende Datei aus dem Test Projekt löschen:

\Controllers\HomeControllerTest.vb

## <a name="creating-the-database"></a>Erstellen der Datenbank

Die Kontakt-Manager-Anwendung ist eine datenbankgesteuerte Webanwendung. Wir verwenden eine Datenbank, um die Kontaktinformationen zu speichern.

Das ASP.NET-MVC-Framework mit einer beliebigen modernen Datenbank, einschließlich Microsoft SQL Server-, Oracle-, MySQL-und IBM DB2-Datenbanken. In diesem Tutorial verwenden wir eine Microsoft SQL Server Datenbank. Wenn Sie Visual Studio installieren, haben Sie die Möglichkeit, Microsoft SQL Server Express zu installieren, bei dem es sich um eine kostenlose Version der Microsoft SQL Server-Datenbank handelt.

Erstellen Sie eine neue Datenbank, indem Sie im Projektmappen-Explorer Fenster mit der rechten Maustaste auf die APP\_Datenordner klicken und die Menüoption **Neues Element hinzufügen**auswählen. Wählen Sie im Dialogfeld **Neues Element hinzufügen** die **Daten** Kategorie und die **SQL Server Daten Bank** Vorlage aus (siehe Abbildung 4). Nennen Sie die neue Datenbank contactmanagerdb. mdf, und klicken Sie auf die Schaltfläche OK.

[![des Dialog Felds "Neues Projekt"](iteration-1-create-the-application-vb/_static/image4.jpg)](iteration-1-create-the-application-vb/_static/image7.png)

**Abbildung 04**: Erstellen einer neuen Microsoft SQL Server Express Datenbank ([Klicken Sie, um das Bild in voller Größe anzuzeigen](iteration-1-create-the-application-vb/_static/image8.png))

Nachdem Sie die neue Datenbank erstellt haben, wird die Datenbank im Projektmappen-Explorer Fenster im Ordner App\_Daten angezeigt. Doppelklicken Sie auf die Datei ContactManager. mdf, um das Fenster Server-Explorer zu öffnen, und stellen Sie eine Verbindung mit der Datenbank her.

> [!NOTE] 
> 
> Das Fenster Server-Explorer wird im Fall von Microsoft Visual Web Developer als Datenbank-Explorer Fenster bezeichnet.

Sie können das Server-Explorer Fenster verwenden, um neue Datenbankobjekte (z. b. Datenbanktabellen, Sichten, Trigger und gespeicherte Prozeduren) zu erstellen. Klicken Sie mit der rechten Maustaste auf den Ordner Tabellen, und wählen Sie die Menüoption **neue Tabelle hinzufügen**. Der Tabellen-Designer der Datenbank wird angezeigt (siehe Abbildung 5).

[![des Dialog Felds "Neues Projekt"](iteration-1-create-the-application-vb/_static/image5.jpg)](iteration-1-create-the-application-vb/_static/image9.png)

**Abbildung 05**: die Daten Bank Tabellen-Designer ([Klicken Sie, um das Bild in voller Größe anzuzeigen](iteration-1-create-the-application-vb/_static/image10.png))

Wir müssen eine Tabelle erstellen, die die folgenden Spalten enthält:

<a id="0.2_table01"></a>

| **Spaltenname** | **Datentyp** | **NULL-Werte zulassen** |
| --- | --- | --- |
| Id | int | false |
| FirstName | nvarchar(50) | false |
| LastName | nvarchar(50) | false |
| Phone | nvarchar(50) | false |
| E-Mail | nvarchar(255) | false |

Die erste Spalte, die ID-Spalte, ist speziell. Sie müssen die ID-Spalte als Identitäts Spalte und Primärschlüssel Spalte markieren. Sie geben an, dass eine Spalte eine Identitäts Spalte ist, indem Sie Spalten Eigenschaften erweitern (betrachten Sie unten in Abbildung 6), und Scrollen Sie nach unten zur Eigenschaft Identity Specification (Identitäts Spezifikation). Legen Sie die Eigenschaft **(ist Identity)** auf den Wert **Ja**fest.

Sie markieren eine Spalte als Primärschlüssel Spalte, indem Sie die Spalte auswählen und dann auf die Schaltfläche mit dem Symbol einer Taste klicken. Nachdem eine Spalte als Primärschlüssel Spalte markiert wurde, wird ein Symbol eines Schlüssels neben der Spalte angezeigt (siehe Abbildung 6).

Nachdem Sie die Tabelle erstellt haben, klicken Sie auf die Schaltfläche Speichern (die Schaltfläche mit dem Symbol einer Diskette), um die neue Tabelle zu speichern. Benennen Sie die neue Tabelle mit dem Namen *Contacts*.

Nachdem Sie das Erstellen der Contact-Datenbanktabelle abgeschlossen haben, sollten Sie der Tabelle einige Datensätze hinzufügen. Klicken Sie im Fenster Server-Explorer mit der rechten Maustaste auf die Tabelle Kontakte, und wählen Sie die Menüoption **Tabellendaten anzeigen**aus. Geben Sie einen oder mehrere Kontakte in das Raster ein, das angezeigt wird.

## <a name="creating-the-data-model"></a>Erstellen des Datenmodells

Die ASP.NET MVC-Anwendung besteht aus Modellen, Ansichten und Controllern. Zunächst erstellen Sie eine Modell Klasse, die die im vorherigen Abschnitt erstellte Tabelle "Kontakte" darstellt.

In diesem Tutorial verwenden wir den Microsoft-Entity Framework, um automatisch eine Modell Klasse aus der Datenbank zu generieren.

> [!NOTE] 
> 
> Das ASP.NET-MVC-Framework ist in keiner Weise an die Microsoft-Entity Framework gebunden. Sie können ASP.NET MVC mit alternativen Datenbankzugriffs Technologien verwenden, einschließlich NHibernate, LINQ to SQL oder ADO.net.

Führen Sie die folgenden Schritte aus, um die Datenmodell Klassen zu erstellen:

1. Klicken Sie im Projektmappen-Explorer Fenster mit der rechten Maustaste auf den Ordner Modelle, und wählen Sie **hinzufügen, neues Element**aus. Das Dialogfeld **Neues Element hinzufügen** wird angezeigt (siehe Abbildung 6).
2. Wählen Sie die **Daten** Kategorie und die **ADO.NET-Entity Data Model** Vorlage aus. Nennen Sie das Datenmodell *contactmanagermodel. edmx* , und klicken Sie auf die Schaltfläche **Hinzufügen** . Der Entity Data Model-Assistent wird angezeigt (siehe Abbildung 7).
3. Wählen Sie im Schritt **Modell Inhalt auswählen** die Option **aus Datenbank generieren aus** (siehe Abbildung 7).
4. Wählen Sie im Schritt **Wählen Sie Ihre Datenverbindung** aus die Datenbank contactmanagerdb. mdf aus, und geben Sie den Namen *contactmanagerdbentities* für die Entitäts Verbindungseinstellungen ein (siehe Abbildung 8).
5. Wählen Sie im Schritt **Wählen Sie Ihre Datenbankobjekte** aus das Kontrollkästchen Tabellen aus (siehe Abbildung 9). Das Datenmodell enthält alle Tabellen, die in der Datenbank enthalten sind (es gibt nur eine Tabelle, die Tabelle "Kontakte"). Geben Sie die Namespace *Modelle*ein. Klicken Sie auf die Schaltfläche Fertigstellen, um den Assistenten abzuschließen.

[![des Dialog Felds "Neues Projekt"](iteration-1-create-the-application-vb/_static/image6.jpg)](iteration-1-create-the-application-vb/_static/image11.png)

**Abbildung 06**: das Dialogfeld "Neues Element hinzufügen" ([Klicken Sie, um das Bild in voller Größe anzuzeigen](iteration-1-create-the-application-vb/_static/image12.png))

[![des Dialog Felds "Neues Projekt"](iteration-1-create-the-application-vb/_static/image7.jpg)](iteration-1-create-the-application-vb/_static/image13.png)

**Abbildung 07**: Auswählen des Modell Inhalts ([Klicken Sie, um das Bild in voller Größe anzuzeigen](iteration-1-create-the-application-vb/_static/image14.png))

[![des Dialog Felds "Neues Projekt"](iteration-1-create-the-application-vb/_static/image8.jpg)](iteration-1-create-the-application-vb/_static/image15.png)

**Abbildung 08**: Auswählen der Datenverbindung ([Klicken Sie, um das Bild in voller Größe anzuzeigen](iteration-1-create-the-application-vb/_static/image16.png))

[![des Dialog Felds "Neues Projekt"](iteration-1-create-the-application-vb/_static/image9.jpg)](iteration-1-create-the-application-vb/_static/image17.png)

**Abbildung 09**: Auswählen der Datenbankobjekte ([Klicken Sie, um das Bild in voller Größe anzuzeigen](iteration-1-create-the-application-vb/_static/image18.png))

Nachdem Sie den Entity Data Model-Assistenten beendet haben, wird der Entity Data Model-Designer angezeigt. Der Designer zeigt eine Klasse an, die jeder Tabelle entspricht, die modelliert wird. Eine Klasse mit dem Namen "Contacts" sollte angezeigt werden.

Der Entity Data Model-Assistent generiert Klassennamen auf der Grundlage von Datenbanktabellen Namen. Sie müssen fast immer den Namen der Klasse ändern, die vom Assistenten generiert wird. Klicken Sie im Designer mit der rechten Maustaste auf die Klasse Contacts, und wählen Sie die Menüoption **Umbenennen**aus. Ändern Sie den Namen der Klasse von Contacts (Plural) in Contact (Singular). Nachdem Sie den Klassennamen geändert haben, sollte die Klasse wie in Abbildung 10 angezeigt werden.

[![des Dialog Felds "Neues Projekt"](iteration-1-create-the-application-vb/_static/image10.jpg)](iteration-1-create-the-application-vb/_static/image19.png)

**Abbildung 10**: die Contact-Klasse ([Klicken Sie, um das Bild in voller Größe anzuzeigen](iteration-1-create-the-application-vb/_static/image20.png))

An diesem Punkt haben wir unser Datenbankmodell erstellt. Wir können die Contact-Klasse verwenden, um einen bestimmten Kontaktdaten Satz in unserer Datenbank darzustellen.

## <a name="creating-the-home-controller"></a>Erstellen des Home-Controllers

Der nächste Schritt besteht darin, den Home-Controller zu erstellen. Der Home-Controller ist der Standard Controller, der in einer ASP.NET MVC-Anwendung aufgerufen wird.

Erstellen Sie die Home Controller-Klasse, indem Sie im Projektmappen-Explorer Fenster mit der rechten Maustaste auf den Ordner Controller klicken und die Menüoption **hinzufügen, Controller** (siehe Abbildung 11) auswählen. Beachten Sie das Kontrollkästchen **Add Action Methods for Create, Update, and Details Szenarios**. Aktivieren Sie dieses Kontrollkästchen, bevor Sie auf die Schaltfläche **Hinzufügen** klicken.

[![des Dialog Felds "Neues Projekt"](iteration-1-create-the-application-vb/_static/image11.jpg)](iteration-1-create-the-application-vb/_static/image21.png)

**Abbildung 11**: Hinzufügen des Home-Controllers ([Klicken Sie, um das Bild in voller Größe anzuzeigen](iteration-1-create-the-application-vb/_static/image22.png))

Wenn Sie den Home-Controller erstellen, erhalten Sie die Klasse in der Liste 1.

**Codebeispiel 1-controllers\homecontroller.vb**

[!code-vb[Main](iteration-1-create-the-application-vb/samples/sample1.vb)]

## <a name="listing-the-contacts"></a>Auflisten der Kontakte

Um die Datensätze in der Datenbanktabelle ' Contacts ' anzuzeigen, müssen Sie eine Index-Aktion () und eine Index Sicht erstellen.

Der Home-Controller enthält bereits eine Index ()-Aktion. Wir müssen diese Methode so ändern, dass Sie wie in der Liste 2 aussieht.

**Codebeispiel 2: controllers\homecontroller.vb**

[!code-vb[Main](iteration-1-create-the-application-vb/samples/sample2.vb)]

Beachten Sie, dass die Home Controller-Klasse in der Liste 2 ein privates Feld mit dem Namen \_Entitäten enthält. Das \_Entities-Feld stellt die Entitäten aus dem Datenmodell dar. Wir verwenden das Feld \_Entities, um mit der Datenbank zu kommunizieren.

Die Index ()-Methode gibt eine Ansicht zurück, die alle Kontakte aus der Contact-Datenbanktabelle darstellt. Der Ausdruck \_Entitäten. Contactset. CHANLIST () gibt die Liste der Kontakte als generische Liste zurück.

Nachdem wir den Index Controller erstellt haben, müssen wir als nächstes die Index Ansicht erstellen. Kompilieren Sie die Anwendung, bevor Sie die Index Ansicht erstellen, indem Sie die Menüoption **Build,** Projekt Mappe erstellen auswählen. Sie sollten das Projekt immer kompilieren, bevor Sie eine Ansicht hinzufügen, damit die Liste der Modellklassen im Dialog **Feld "Ansicht hinzufügen** " angezeigt wird.

Zum Erstellen der Index Ansicht klicken Sie mit der rechten Maustaste auf die Index ()-Methode, und wählen Sie die Menüoption **Ansicht hinzufügen** (siehe Abbildung 12). Wenn Sie diese Menüoption auswählen, wird das Dialog **Feld Ansicht hinzufügen** geöffnet (siehe Abbildung 13).

[![des Dialog Felds "Neues Projekt"](iteration-1-create-the-application-vb/_static/image12.jpg)](iteration-1-create-the-application-vb/_static/image23.png)

**Abbildung 12**: Hinzufügen der Index Ansicht ([Klicken Sie, um das Bild in voller Größe anzuzeigen](iteration-1-create-the-application-vb/_static/image24.png))

Aktivieren Sie im Dialog **Feld Ansicht hinzufügen** das Kontrollkästchen mit der Bezeichnung **eine stark typisierte Ansicht erstellen**. Wählen Sie die Ansichts Datenklasse ContactManager. Contact und die Liste Inhalt anzeigen aus. Bei Auswahl dieser Optionen wird eine Ansicht generiert, in der eine Liste der Kontaktdaten Sätze angezeigt wird.

[![des Dialog Felds "Neues Projekt"](iteration-1-create-the-application-vb/_static/image13.jpg)](iteration-1-create-the-application-vb/_static/image25.png)

**Abbildung 13**: Dialogfeld "Ansicht hinzufügen" ([Klicken Sie, um das Bild in voller Größe anzuzeigen](iteration-1-create-the-application-vb/_static/image26.png))

Wenn Sie auf die Schaltfläche **Hinzufügen** klicken, wird die Index Ansicht in der Liste 3 generiert. Beachten Sie die &lt;% @ Page%&gt;-Direktive, die am Anfang der Datei angezeigt wird. Die Index Sicht erbt von der ViewPage-&lt;IEnumerable&lt;ContactManager. Models. Contact&gt;&gt;-Klasse. Mit anderen Worten, die Modell Klasse in der Sicht stellt eine Liste von Contact-Entitäten dar.

Der Text der Index Ansicht enthält eine foreach-Schleife, die die einzelnen Kontakte durchläuft, die von der Modell Klasse dargestellt werden. Der Wert jeder Eigenschaft der Contact-Klasse wird in einer HTML-Tabelle angezeigt.

**Codebeispiel 3-views\home\index.aspx (nicht geändert)**

[!code-aspx[Main](iteration-1-create-the-application-vb/samples/sample3.aspx)]

Wir müssen eine Änderung an der Index Sicht vornehmen. Da wir keine Detailansicht erstellen, können wir den Link Details entfernen. Suchen und entfernen Sie den folgenden Code aus der Index Ansicht:

{. ID = Element. ID})%&gt;

Nachdem Sie die Index Ansicht geändert haben, können Sie die Kontakt-Manager-Anwendung ausführen. Wählen Sie die Menüoption Debuggen, Debugging starten aus, oder drücken Sie einfach F5. Wenn Sie die Anwendung zum ersten Mal ausführen, wird das Dialogfeld in Abbildung 14 angezeigt. Wählen Sie die Option **Web. config-Datei ändern aus, um das Debugging zu aktivieren,** und klicken Sie auf OK.

[![des Dialog Felds "Neues Projekt"](iteration-1-create-the-application-vb/_static/image14.jpg)](iteration-1-create-the-application-vb/_static/image27.png)

**Abbildung 14**: Aktivieren des Debuggens ([Klicken Sie, um das Bild in voller Größe anzuzeigen](iteration-1-create-the-application-vb/_static/image28.png))

Die Index Sicht wird standardmäßig zurückgegeben. In dieser Ansicht werden alle Daten aus der Datenbanktabelle "Contacts" aufgelistet (siehe Abbildung 15).

[![des Dialog Felds "Neues Projekt"](iteration-1-create-the-application-vb/_static/image15.jpg)](iteration-1-create-the-application-vb/_static/image29.png)

**Abbildung 15**: die Index Ansicht ([Klicken Sie, um das Bild in voller Größe anzuzeigen](iteration-1-create-the-application-vb/_static/image30.png))

Beachten Sie, dass die Index Sicht einen Link mit der Bezeichnung neu erstellen am unteren Rand der Sicht enthält. Im nächsten Abschnitt erfahren Sie, wie neue Kontakte erstellt werden.

## <a name="creating-new-contacts"></a>Erstellen neuer Kontakte

Damit Benutzer neue Kontakte erstellen können, müssen wir dem Home-Controller zwei Create ()-Aktionen hinzufügen. Wir müssen eine Create ()-Aktion erstellen, die ein HTML-Formular zum Erstellen eines neuen Kontakts zurückgibt. Wir müssen eine zweite Create ()-Aktion erstellen, die die tatsächliche Daten Bank Einfügung des neuen Kontakts ausführt.

Die neuen Create ()-Methoden, die wir dem Home-Controller hinzufügen müssen, sind in der Liste 4 enthalten.

**Codebeispiel 4-controllers\homecontroller.vb (mit Create-Methoden)**

[!code-vb[Main](iteration-1-create-the-application-vb/samples/sample4.vb)]

Die erste Create ()-Methode kann mit einem HTTP GET aufgerufen werden, während die zweite Create ()-Methode nur über eine HTTP Post-Methode aufgerufen werden kann. Das heißt, die zweite Create ()-Methode kann nur aufgerufen werden, wenn ein HTML-Formular bereitstellt wird. Die erste Create ()-Methode gibt einfach eine Ansicht zurück, die das HTML-Formular zum Erstellen eines neuen Kontakts enthält. Die zweite Create ()-Methode ist viel interessanter: Sie fügt der Datenbank den neuen Kontakt hinzu.

Beachten Sie, dass die zweite Create ()-Methode geändert wurde, um eine Instanz der Contact-Klasse zu akzeptieren. Die aus dem HTML-Formular geposteten Formular Werte werden vom ASP.NET-MVC-Framework automatisch an diese Contact-Klasse gebunden. Jedes Formularfeld aus dem HTML-Formular erstellen wird einer Eigenschaft des Contact-Parameters zugewiesen.

Beachten Sie, dass der Contact-Parameter mit einem [BIND]-Attribut versehen ist. Das [BIND]-Attribut wird verwendet, um die Eigenschaft "Contact ID" aus der Bindung auszuschließen. Da die ID-Eigenschaft eine Identity-Eigenschaft darstellt, möchten wir die ID-Eigenschaft nicht festlegen.

Im Text der Create ()-Methode wird der Entity Framework verwendet, um den neuen Kontakt in die Datenbank einzufügen. Der neue Kontakt wird dem vorhandenen Satz von Kontakten hinzugefügt, und die SaveChanges ()-Methode wird aufgerufen, um diese Änderungen an die zugrunde liegende Datenbank zurückzuleiten.

Sie können ein HTML-Formular zum Erstellen neuer Kontakte generieren, indem Sie mit der rechten Maustaste auf eine der beiden Create ()-Methoden klicken und die Menüoption **Ansicht hinzufügen** auswählen (siehe Abbildung 16).

[![des Dialog Felds "Neues Projekt"](iteration-1-create-the-application-vb/_static/image16.jpg)](iteration-1-create-the-application-vb/_static/image31.png)

**Abbildung 16**: Hinzufügen der Ansicht "erstellen" ([Klicken Sie, um das Bild in voller Größe anzuzeigen](iteration-1-create-the-application-vb/_static/image32.png))

Wählen Sie im Dialog **Feld Ansicht hinzufügen** die **ContactManager. Contact** -Klasse und die **Create** -Option für Inhalt anzeigen (siehe Abbildung 17). Wenn Sie auf die Schaltfläche **Hinzufügen** klicken, wird automatisch eine CREATE VIEW-Struktur generiert.

[![des Dialog Felds "Neues Projekt"](iteration-1-create-the-application-vb/_static/image17.jpg)](iteration-1-create-the-application-vb/_static/image33.png)

**Abbildung 17**: Anzeigen einer Seiten Explosion ([Klicken Sie, um das Bild in voller Größe anzuzeigen](iteration-1-create-the-application-vb/_static/image34.png))

Die CREATE VIEW enthält Formularfelder für jede der Eigenschaften der Contact-Klasse. Der Code für die CREATE-Sicht ist in der Liste 5 enthalten.

**Codebeispiel 5: views\home\kreate.aspx**

[!code-aspx[Main](iteration-1-create-the-application-vb/samples/sample5.aspx)]

Nachdem Sie die Create ()-Methoden geändert und die CREATE VIEW-Sicht hinzugefügt haben, können Sie die Anwendung Contact Manager ausführen und neue Kontakte erstellen. Klicken Sie auf den Link **neu erstellen** , der in der Ansicht Index angezeigt wird, um zur Ansicht erstellen zu navigieren. Die Ansicht in Abbildung 18 sollte angezeigt werden.

[![des Dialog Felds "Neues Projekt"](iteration-1-create-the-application-vb/_static/image18.jpg)](iteration-1-create-the-application-vb/_static/image35.png)

**Abbildung 18**: Ansicht "erstellen" ([Klicken Sie, um das Bild in voller Größe anzuzeigen](iteration-1-create-the-application-vb/_static/image36.png))

## <a name="editing-contacts"></a>Bearbeiten von Kontakten

Das Hinzufügen der Funktionalität zum Bearbeiten eines Kontaktdaten Satzes ähnelt dem Hinzufügen der Funktionalität zum Erstellen neuer Kontaktdaten Sätze. Zuerst müssen wir der Home Controller-Klasse zwei neue Bearbeitungsmethoden hinzufügen. Diese neuen Edit ()-Methoden sind in der Liste 6 enthalten.

**Auflisten von 6-controllers\homecontroller.vb (mit Bearbeitungsmethoden)**

[!code-vb[Main](iteration-1-create-the-application-vb/samples/sample6.vb)]

Die erste Edit ()-Methode wird durch einen HTTP Get-Vorgang aufgerufen. An diese Methode wird ein ID-Parameter übergeben, der die ID des Kontaktdaten Satzes darstellt, der bearbeitet wird. Der Entity Framework wird zum Abrufen eines Kontakts verwendet, der mit der ID übereinstimmt. Eine Ansicht, die ein HTML-Formular zum Bearbeiten eines Datensatzes enthält, wird zurückgegeben.

Mit der zweiten Edit ()-Methode wird das tatsächliche Update der Datenbank durchführt. Diese Methode akzeptiert eine Instanz der Contact-Klasse als Parameter. Das ASP.NET-MVC-Framework bindet die Formularfelder aus dem Bearbeitungs Formular automatisch an diese Klasse. Beachten Sie, dass Sie das [BIND]-Attribut beim Bearbeiten eines Kontakts nicht einschließen (der Wert der ID-Eigenschaft wird benötigt).

Der Entity Framework wird verwendet, um den geänderten Kontakt in der Datenbank zu speichern. Der ursprüngliche Kontakt muss zuerst aus der Datenbank abgerufen werden. Als nächstes wird die Entity Framework ApplyPropertyChanges ()-Methode aufgerufen, um die Änderungen am Kontakt aufzuzeichnen. Zum Schluss wird die Entity Framework SaveChanges ()-Methode aufgerufen, um die Änderungen an der zugrunde liegenden Datenbank beizubehalten.

Sie können die Ansicht mit dem Bearbeitungs Formular generieren, indem Sie mit der rechten Maustaste auf die Edit ()-Methode klicken und die Menüoption Ansicht hinzufügen auswählen. Wählen Sie im Dialogfeld Ansicht hinzufügen die **ContactManager. Models. Contact** -Klasse und den Inhalt der **Bearbeitungs** Ansicht aus (siehe Abbildung 19).

[![des Dialog Felds "Neues Projekt"](iteration-1-create-the-application-vb/_static/image19.jpg)](iteration-1-create-the-application-vb/_static/image37.png)

**Abbildung 19**: Hinzufügen einer Bearbeitungs Ansicht ([Klicken Sie, um das Bild in voller Größe anzuzeigen](iteration-1-create-the-application-vb/_static/image38.png))

Wenn Sie auf die Schaltfläche Hinzufügen klicken, wird automatisch eine neue Bearbeitungs Ansicht generiert. Das generierte HTML-Formular enthält Felder, die den einzelnen Eigenschaften der Contact-Klasse entsprechen (siehe Codebeispiel 7).

**Codebeispiel 7: views\home\edit.aspx**

[!code-aspx[Main](iteration-1-create-the-application-vb/samples/sample7.aspx)]

## <a name="deleting-contacts"></a>Löschen von Kontakten

Wenn Sie Kontakte löschen möchten, müssen Sie der Home Controller-Klasse zwei Delete ()-Aktionen hinzufügen. Die erste Delete ()-Aktion zeigt ein Bestätigungsformular für den Löschvorgang an. Die zweite Delete ()-Aktion führt den eigentlichen Löschvorgang aus.

> [!NOTE] 
> 
> Später wird in der Iterations #7 der Kontakt-Manager so geändert, dass er einen Schritt AJAX-Löschvorgang unterstützt.

Die beiden neuen Delete ()-Methoden sind in der Auflistung 8 enthalten.

**Auflisten von 8-controllers\homecontroller.vb (Delete-Methoden)**

[!code-vb[Main](iteration-1-create-the-application-vb/samples/sample8.vb)]

Die erste Delete ()-Methode gibt ein Bestätigungsformular zum Löschen eines Kontaktdaten Satzes aus der Datenbank zurück (siehe Figure20). Die zweite Delete ()-Methode führt den eigentlichen Löschvorgang für die Datenbank aus. Nachdem der ursprüngliche Kontakt aus der Datenbank abgerufen wurde, werden die Entity Framework DeleteObject ()-Methode und die SaveChanges ()-Methode aufgerufen, um die Daten Bank Löschung auszuführen.

[![des Dialog Felds "Neues Projekt"](iteration-1-create-the-application-vb/_static/image20.jpg)](iteration-1-create-the-application-vb/_static/image39.png)

**Abbildung 20**: die Bestätigungs Ansicht "Löschen" ([Klicken Sie, um das Bild in voller Größe anzuzeigen](iteration-1-create-the-application-vb/_static/image40.png))

Wir müssen die Index Sicht so ändern, dass Sie einen Link zum Löschen von Kontaktdaten Sätzen enthält (siehe Abbildung 21). Fügen Sie der gleichen Tabellenzelle, die den Link Bearbeiten enthält, den folgenden Code hinzu:

{. ID = Element. ID})%&gt;

[![des Dialog Felds "Neues Projekt"](iteration-1-create-the-application-vb/_static/image21.jpg)](iteration-1-create-the-application-vb/_static/image41.png)

**Abbildung 21**: Index Ansicht mit Bearbeitungs Link ([Klicken Sie, um das Bild in voller Größe anzuzeigen](iteration-1-create-the-application-vb/_static/image42.png))

Als nächstes müssen wir die Bestätigungs Ansicht für den Löschvorgang erstellen. Klicken Sie mit der rechten Maustaste auf die Delete ()-Methode in der Home Controller-Klasse, und wählen Sie die Menüoption Ansicht hinzufügen Das Dialogfeld Ansicht hinzufügen wird angezeigt (siehe Abbildung 22).

Anders als bei den Ansichten "auflisten", "erstellen" und "Bearbeiten" enthält das Dialogfeld "Ansicht hinzufügen" keine Option zum Erstellen einer Lösch Ansicht. Wählen Sie stattdessen die Datenklasse **ContactManager. Models. Contact** und den **leeren** Inhalt der Ansicht aus. Wenn Sie die Option "Inhalte anzeigen" auswählen, müssen wir die Ansicht selbst erstellen.

[![des Dialog Felds "Neues Projekt"](iteration-1-create-the-application-vb/_static/image22.jpg)](iteration-1-create-the-application-vb/_static/image43.png)

**Abbildung 22**: Hinzufügen der Bestätigungs Ansicht "Löschen" ([Klicken Sie, um das Bild in voller Größe anzuzeigen](iteration-1-create-the-application-vb/_static/image44.png))

Der Inhalt der Lösch Ansicht ist in der Liste 9 enthalten. Diese Ansicht enthält ein Formular, das bestätigt, ob ein bestimmter Kontakt gelöscht werden soll (siehe Abbildung 21).

**Codebeispiel 9: views\home\delete.aspx**

[!code-aspx[Main](iteration-1-create-the-application-vb/samples/sample9.aspx)]

## <a name="changing-the-name-of-the-default-controller"></a>Ändern des Namens des Standard Controllers

Möglicherweise ist der Name der Controller Klasse für die Arbeit mit Kontakten die HomeController-Klasse. Soll der Controller nicht "ContactController" heißen?

Dieses Problem ist leicht zu beheben. Zuerst müssen wir den Namen des Home-Controllers umgestalten. Öffnen Sie die Klasse HomeController im Visual Studio Code-Editor, klicken Sie mit der rechten Maustaste auf den Namen der Klasse, und wählen Sie die Menüoption **Umbenennen**aus. Durch Auswahl dieser Menüoption wird das Dialogfeld Umbenennen geöffnet.

[![des Dialog Felds "Neues Projekt"](iteration-1-create-the-application-vb/_static/image23.jpg)](iteration-1-create-the-application-vb/_static/image45.png)

**Abbildung 23**: Umgestalten eines Controller namens ([Klicken Sie, um das Bild in voller Größe anzuzeigen](iteration-1-create-the-application-vb/_static/image46.png))

[![des Dialog Felds "Neues Projekt"](iteration-1-create-the-application-vb/_static/image24.jpg)](iteration-1-create-the-application-vb/_static/image47.png)

**Abbildung 24**: Verwenden des Dialog Felds "umbenennen" ([Klicken Sie, um das Bild in voller Größe anzuzeigen](iteration-1-create-the-application-vb/_static/image48.png))

Wenn Sie die Controller Klasse umbenennen, aktualisiert Visual Studio auch den Namen des Ordners im Ordner "Views". Visual Studio benennt den Ordner "\views\home" in den Ordner "\views\contact" um.

Nachdem Sie diese Änderung vorgenommen haben, verfügt die Anwendung nicht mehr über einen Home-Controller. Wenn Sie die Anwendung ausführen, erhalten Sie die Fehlerseite in Abbildung 25.

[![des Dialog Felds "Neues Projekt"](iteration-1-create-the-application-vb/_static/image25.jpg)](iteration-1-create-the-application-vb/_static/image49.png)

**Abbildung 25**: kein Standard Controller ([Klicken Sie, um das Bild in voller Größe anzuzeigen](iteration-1-create-the-application-vb/_static/image50.png))

Wir müssen die Standardroute in der Datei "Global. asax" aktualisieren, um den Contact Controller anstelle des Home-Controllers zu verwenden. Öffnen Sie die Datei Global. asax, und ändern Sie den Standard Controller, der von der Standardroute verwendet wird (siehe Codebeispiel 10).

**Codebeispiel 10: Global. asax. vb**

[!code-vb[Main](iteration-1-create-the-application-vb/samples/sample10.vb)]

Nachdem Sie diese Änderungen vorgenommen haben, wird der Kontakt-Manager ordnungsgemäß ausgeführt. Nun wird die Contact Controller-Klasse als Standard Controller verwendet.

## <a name="summary"></a>Zusammenfassung

In dieser ersten Iterationen haben wir eine einfache Kontakt-Manager-Anwendung auf möglichst schnelle Weise erstellt. Wir haben Visual Studio genutzt, um den anfänglichen Code für unsere Controller und Ansichten automatisch zu generieren. Außerdem nutzten wir die Entity Framework, um die Datenbankmodell Klassen automatisch zu generieren.

Zurzeit können wir Kontaktdaten Sätze mit der Kontakt-Manager-Anwendung auflisten, erstellen, bearbeiten und löschen. Mit anderen Worten, wir können alle grundlegenden Daten Bank Vorgänge ausführen, die für eine datenbankgesteuerte Webanwendung erforderlich sind.

Leider treten bei der Anwendung einige Probleme auf. Und ich möchte das nicht, dass die Kontakt-Manager-Anwendung nicht die attraktivste Anwendung ist. Es sind einige Entwurfsarbeiten erforderlich. In der nächsten Iterationen wird erläutert, wie Sie die standardmäßige Ansichts Master Seite und das Cascading Stylesheet ändern können, um die Darstellung der Anwendung zu verbessern.

Zweitens haben wir keine Formular Validierung implementiert. Beispielsweise können Sie nicht verhindern, dass Sie das Formular zum Erstellen von Kontakten übermitteln, ohne Werte für eines der Formularfelder eingeben zu müssen. Außerdem können Sie ungültige Telefonnummern und e-Mail-Adressen eingeben. Wir beginnen damit, das Problem der Formular Validierung in der Iterations #3 zu beheben.

Und schließlich ist es am wichtigsten, dass die aktuelle Iterationen der Kontakt-Manager-Anwendung nicht einfach geändert oder gewartet werden können. Beispielsweise wird die Datenbankzugriffs Logik direkt in die Controller Aktionen integriert. Dies bedeutet, dass wir den Datenzugriffs Code nicht ändern können, ohne unsere Controller zu ändern. In späteren Iterationen untersuchen wir Software Entwurfsmuster, die implementiert werden können, damit der Kontakt-Manager stabiler geändert werden kann.

> [!div class="step-by-step"]
> [Zurück](iteration-7-add-ajax-functionality-cs.md)
> [Weiter](iteration-2-make-the-application-look-nice-vb.md)
