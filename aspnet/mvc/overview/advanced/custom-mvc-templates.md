---
uid: mvc/overview/advanced/custom-mvc-templates
title: Benutzerdefinierte MVC-Vorlage | Microsoft-Dokumentation
author: joeloff
description: Erstellen Sie eine Vorlage als VSIX-Erweiterung.
ms.author: riande
ms.date: 12/10/2012
ms.assetid: b0a214c7-2f38-4dbc-b47f-bd7bd9df97bd
msc.legacyurl: /mvc/overview/advanced/custom-mvc-templates
msc.type: authoredcontent
ms.openlocfilehash: 0603bc24e070e223551813f66a75889a2e46fd35
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78499599"
---
# <a name="custom-mvc-template"></a>Benutzerdefinierte MVC-Vorlage

von [Jacques eloff](https://github.com/joeloff)

Mit der Veröffentlichung von MVC 3 Tools Update für Visual Studio 2010 wurde ein separater Projekt-Assistent für MVC-Projekte eingeführt. Die Änderung wurde durch zwei Faktoren gesteuert. Erstens: die Einführung neuer Vorlagen in MVC 3 und die Unterstützung zusätzlicher Ansichts-Engines, wie z. b. Razor, führen zu einer Überlastung des Dialog Felds "Neues Projekt" in Visual Studio. Zweitens haben Kunden nach Erweiterbarkeits Punkten gefragt, und der neue MVC-Projekt-Assistent bietet uns die Möglichkeit, auf diese Anforderungen zu reagieren.

Das Hinzufügen von benutzerdefinierten Vorlagen war ein mühsamer Prozess, bei dem die Verwendung der Registrierung zum Anzeigen neuer Vorlagen für den MVC-Projekt-Assistenten verwendet wurde. Der Autor einer neuen Vorlage musste Sie in eine MSI einschließen, um sicherzustellen, dass die erforderlichen Registrierungseinträge zur Installationszeit erstellt werden. Die Alternative war, eine ZIP-Datei mit der Vorlage verfügbar zu machen, und der Endbenutzer muss die erforderlichen Registrierungseinträge per Hand erstellen.

Keines der oben genannten Ansätze ist ideal, daher haben wir uns entschieden, einen Teil der vorhandenen Infrastruktur von [VSIX](https://msdn.microsoft.com/library/ff363239.aspx) -Erweiterungen zu nutzen, um das Erstellen, verteilen und Installieren von benutzerdefinierten MVC-Vorlagen, beginnend mit MVC 4 für Visual Studio 2012, zu vereinfachen. Diese Vorgehensweise bietet folgende Vorteile:

- Eine VSIX-Erweiterung kann mehrere Vorlagen enthalten, die unterschiedlicheC# Sprachen (und Visual Basic) und mehrere Ansichts-Engines (ASPX und Razor) unterstützen.
- Eine VSIX-Erweiterung kann mehrere SKUs von Visual Studio, einschließlich Express-SKUs, als Ziel haben.
- Die [Visual Studio Gallery](https://visualstudiogallery.msdn.microsoft.com/) ermöglicht das Verteilen der Erweiterung an eine breite Zielgruppe.
- VSIX-Erweiterungen können aktualisiert werden, sodass Korrekturen und Aktualisierungen Ihrer benutzerdefinierten Vorlagen leichter erstellt werden können.

## <a name="prerequisites"></a>Voraussetzungen

- Benutzer müssen mit der Erstellung von Projektvorlagen vertraut sein, einschließlich des erforderlichen Markups für VSTEMPLATE-Dateien usw.
- Benutzer müssen über Visual Studio Professional und höher verfügen. Express-SKUs unterstützen nicht das Erstellen von VSIX-Projekten.
- [Visual Studio 2012 SDK](https://www.microsoft.com/download/details.aspx?id=30668) ist installiert.

## <a name="example"></a>Beispiel

Der erste Schritt besteht darin, ein neues VSIX-Projekt mithilfe C# von oder Visual Basic zu erstellen. Wählen Sie **Datei > Neues Projekt**aus, klicken Sie im linken Bereich auf **Erweiterbarkeit** , und wählen Sie das **VSIX-Projekt**aus.

![Neues Projekt](custom-mvc-templates/_static/image1.jpg)

Nachdem das Projekt erstellt wurde, wird der VSIX-Designer geöffnet.

![Projekt-Designer-Metadaten](custom-mvc-templates/_static/image2.jpg)

Mit dem Designer können einige der allgemeinen Eigenschaften der Erweiterung bearbeitet werden, die Benutzern bei der Installation der Erweiterung angezeigt werden. Alternativ können Sie die installierten Erweiterungen in Visual Studio (**Tools > Erweiterungen und Updates**) durchsuchen. Nachdem Sie die allgemeinen Informationen abgeschlossen haben, klicken Sie auf die **Registerkarte Ziele installieren**.

![Projekt-Designer-Installations Ziele](custom-mvc-templates/_static/image3.jpg)

Diese Registerkarte wird verwendet, um die SKUs und Versionen von Visual Studio anzugeben, die von der Erweiterung unterstützt werden. Aktivieren Sie das Kontrollkästchen für **diese VSIX ist installiert für alle Benutzer** , um die Computer spezifische Installation der VSIX zu aktivieren. Klicken Sie auf der rechten Seite auf die Schaltfläche **neu** , um zusätzliche SKUs hinzuzufügen, z. b. Web Developer Express (VWD).

![Neues Installationsziel hinzufügen](custom-mvc-templates/_static/image4.jpg)

Wenn Sie beabsichtigen, alle professionellen und höheren SKUs (Professional, Premium und Ultimate) zu unterstützen, müssen Sie nur die minimale SKU in der Familie **Microsoft.VisualStudio.pro**auswählen. Denken Sie daran, alle Änderungen zu speichern, nachdem Sie die Installations Ziele abgeschlossen haben.

![Projekt-Designer-Installations Ziele](custom-mvc-templates/_static/image5.jpg)

Die Registerkarte **Objekte** wird verwendet, um alle Inhalts Dateien zu VSIX hinzuzufügen. Da MVC benutzerdefinierte Metadaten erfordert, bearbeiten Sie den unformatierten XML-Code der VSIX-Manifest-Datei, anstatt die Registerkarte **Assets** zum Hinzufügen von Inhalt zu verwenden. Fügen Sie zunächst den Vorlagen Inhalt dem VSIX-Projekt hinzu. Es ist wichtig, dass die Struktur des Ordners und der Inhalt das Layout des Projekts widerspiegeln. Das folgende Beispiel enthält vier Projektvorlagen, die von der grundlegenden MVC-Projektvorlage abgeleitet wurden. Stellen Sie sicher, dass alle Dateien, die Ihre Projektvorlage umfassen (alles unterhalb des Ordners "ProjectTemplates") der **Inhalts** -ItemGroup in der VSIX-Projektdatei hinzugefügt werden und dass jedes Element die " **copydeoutputdirectory** "-und " **includeinvsix** "-Metadaten enthält, wie im folgenden Beispiel gezeigt.

&lt;Inhalt umfasst =&quot;projecttemplates\mymvcwebapplicationprojecttemplate.csaspx\basicweb.config&quot;&gt;

&lt;copyaufoutputdirectory&gt;immer&lt;/CopyToOutputDirectory&gt;

&lt;includeinvsix&gt;true&lt;/IncludeInVSIX&gt;

&lt;/Content&gt;

Wenn dies nicht der Fall ist, versucht die IDE, den Inhalt der Vorlage zu kompilieren, wenn Sie die VSIX erstellen, und wahrscheinlich wird eine Fehlermeldung angezeigt. Code Dateien in Vorlagen enthalten häufig spezielle [Vorlagen Parameter](https://msdn.microsoft.com/library/eehb4faa(v=vs.110).aspx) , die von Visual Studio verwendet werden, wenn die Projektvorlage instanziiert wird und daher nicht in der IDE kompiliert werden kann.

![Projektmappen-Explorer](custom-mvc-templates/_static/image6.jpg)

Schließen Sie den VSIX-Designer, klicken Sie in **Projektmappen-Explorer** mit der rechten Maustaste auf die Datei " **Source. Extension. Manifest** ", und wählen Sie **Öffnen mit** und dann die Option **XML (Text) Editor** aus.

![Öffnen mit-Dialog Feld](custom-mvc-templates/_static/image7.jpg)

Erstellen Sie ein **&lt;Assets&gt;** -Element, und fügen Sie ein **&lt;Asset-&gt;** Element für jede Datei hinzu, die in die VSIX eingeschlossen werden muss. Das **Type** -Attribut jedes **&lt;Asset&gt;** -Elements muss auf **Microsoft. VisualStudio. MVC. Template**festgelegt werden. Dies ist ein benutzerdefinierter Namespace, den nur der MVC-Projekt-Assistent versteht. Weitere Informationen zur Struktur und zum Layout der Manifest-Datei finden Sie in der VSIX 2,0-Schema Dokumentation.

Das Hinzufügen der Dateien zu VSIX reicht nicht aus, um die Vorlagen mit dem MVC-Assistenten zu registrieren. Sie müssen Informationen wie den Vorlagen Namen, die Beschreibung, die unterstützten Ansichts-Engines und die Programmiersprache für den MVC-Assistenten angeben. Diese Informationen werden in benutzerdefinierten Attributen übernommen, die dem **&lt;Asset&gt;** -Element für jede **VSTEMPLATE** -Datei zugeordnet sind.

&lt;Asset d:vsixsubpath =&quot;projecttemplates\mymvcwebapplicationprojecttemplate.csaspx&quot;

Type =&quot;Microsoft. VisualStudio. MVC. Template&quot;

d:Source =&quot;Datei&quot;

Path =&quot;ProjectTemplates\MyMvcWebApplicationProjectTemplate.csaspx\BasicMvcWebApplicationProjectTemplate.11.csaspx.vstemplate&quot;

ProjectType =&quot;MVC-&quot;

Sprache =&quot;C#&quot;

Viewengine =&quot;aspx&quot;

TemplateID =&quot;mymvcapplication&quot;

Title =&quot;benutzerdefinierte einfache Webanwendung&quot;

Description =&quot;einer benutzerdefinierten Vorlage, die von einer grundlegenden MVC-Webanwendung (Razor) abgeleitet ist&quot;

Version =&quot;4,0&quot;/&gt;

Im folgenden finden Sie eine Erläuterung der benutzerdefinierten Attribute, die vorhanden sein müssen:

- **ProjectType** muss auf MVC festgelegt werden.
- Die **Sprache** bestimmt die von der Vorlage unterstützte Entwicklungssprache. Gültige Werte sind entweder C# oder VB.
- **Viewengine** bezeichnet das Ansichts Modul, das von der Vorlage unterstützt wird, z. b. aspx oder Razor. Sie können einen benutzerdefinierten Wert für dieses Feld angeben.
- **TemplateID** wird zum Gruppieren der Vorlagen verwendet. Wenn der Wert mit einer vorhandenen Vorlagen-ID übereinstimmt, werden Vorlagen außer Kraft gesetzt, die zuvor mit dem MVC-Assistenten registriert wurden.
- **Titel** gibt die kurze Beschreibung an, die im MVC-Assistenten unter jeder Projektvorlage angezeigt wird.
- Die **Beschreibung** bezeichnet eine ausführlichere Beschreibung der Vorlage.

Nachdem Sie alle Dateien zum Manifest hinzugefügt und gespeichert haben, werden Sie feststellen, dass auf der Registerkarte **Objekte** im Designer alle Dateien angezeigt werden, nicht jedoch die benutzerdefinierten Attribute, die Sie dem **&lt;Asset&gt;** Elementen für die **VSTEMPLATE** -Dateien hinzugefügt haben.

![Projekt-Designer-Assets](custom-mvc-templates/_static/image8.jpg)

Jetzt müssen Sie nur noch das VSIX-Projekt kompilieren und installieren.

Stellen Sie sicher, dass alle Instanzen von Visual Studio auf dem Computer geschlossen sind, auf dem Sie die VSIX-Erweiterung testen möchten. Visual Studio sucht während des Starts nach neuen Erweiterungen. Wenn also die IDE beim Installieren einer VSIX geöffnet ist, müssen Sie Visual Studio neu starten. Doppelklicken Sie im Explorer auf die vsix-Datei, um das **VSIX-Installations**Programm zu starten, klicken Sie auf **Installieren** , und starten Sie dann Visual Studio.

![VSIX-Installer](custom-mvc-templates/_static/image9.jpg)

Wählen Sie im Menü **Tools > Erweiterungen und Updates** aus, um zu bestätigen, dass die Erweiterung installiert wurde. Wenn das VSIX-Installationsprogramm während der Installation der Erweiterung Fehler gemeldet hat, können Sie das VSIX-Installationsprotokoll anzeigen, um weitere Informationen zu erhalten. Das Protokoll wird in der Regel im Ordner " **% Temp%** " des Benutzers erstellt, der die Erweiterung installiert hat, z. b. " **c:\users\bob\appdata\local\temp**".

![Erweiterungen und Aktualisierungen](custom-mvc-templates/_static/image10.jpg)

Nachdem Sie das Fenster geschlossen haben, können Sie ein MVC 4-Projekt erstellen, um festzustellen, ob Ihre neuen Vorlagen im MVC-Assistenten angezeigt werden.

![Neues ASP.NET MVC 4-Projekt](custom-mvc-templates/_static/image11.jpg)

## <a name="limitations"></a>Einschränkungen

1. Lokalisierte benutzerdefinierte Vorlagen werden vom MVC-Assistenten nicht unterstützt.
2. Der Assistent meldet keine Fehler, wenn benutzerdefinierte Vorlagen nicht gefunden werden können. Wenn eines der erforderlichen benutzerdefinierten Attribute fehlt, wird die Vorlage einfach vom Assistenten ausgeschlossen.
