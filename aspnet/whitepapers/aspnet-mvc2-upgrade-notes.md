---
uid: whitepapers/aspnet-mvc2-upgrade-notes
title: Aktualisieren einer ASP.NET MVC 1,0-Anwendung auf ASP.NET MVC 2 | Microsoft-Dokumentation
author: rick-anderson
description: In diesem Dokument wird beschrieben, wie Sie manuell und mit dem Assistenten eine ASP.NET MVC 1,0-Anwendung auf ASP.NET MVC 2 aktualisieren. Dieses Dokument ist auch für "d..." verfügbar.
ms.author: riande
ms.date: 04/08/2010
ms.assetid: f1a01759-d251-4b09-8835-e112e336c6dd
msc.legacyurl: /whitepapers/aspnet-mvc2-upgrade-notes
msc.type: content
ms.openlocfilehash: 27589f1b1c9d5038118e5ff0cc2e7cecae17d5ed
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78517299"
---
# <a name="upgrading-an-aspnet-mvc-10-application-to-aspnet-mvc-2"></a>Durchführen eines Upgrades für eine ASP.NET MVC 1.0-Anwendung auf ASP.NET MVC 2

> In diesem Dokument wird beschrieben, wie Sie manuell und mit dem Assistenten eine ASP.NET MVC 1,0-Anwendung auf ASP.NET MVC 2 aktualisieren. Dieses Dokument steht auch zum [herunterladen](https://download.microsoft.com/download/F/1/6/F16F9AF9-8EF4-4845-BC97-639791D5699C/MVC2-Upgrade-Notes.pdf) zur Verfügung.

## <a name="introduction"></a>Einführung

ASP.NET MVC 2 kann parallel zu ASP.NET MVC 1,0 auf demselben Server installiert werden. Dies bietet Anwendungsentwicklern Flexibilität bei der Wahl, wann eine ASP.NET MVC 1,0-Anwendung auf ASP.NET MVC 2 aktualisiert werden soll.

Visual Studio 2010 umfasst einen Assistenten, mit dem vorhandene ASP.NET MVC 1,0-Projekte aktualisiert werden, die mit Visual Studio 2008 auf ASP.NET MVC 2 erstellt wurden. Der Upgrade-Assistent wird initiiert, indem Sie in Visual Studio 2010 ein ASP.NET MVC 1,0-Projekt öffnen.

## <a name="upgrade-wizard-for-aspnet-mvc-10-on-visual-studio-2008-sp1"></a>Upgrade-Assistent für ASP.NET MVC 1,0 in Visual Studio 2008 SP1

Um eine ASP.NET MVC 1,0-Anwendung auf ASP.NET MVC 2 in Visual Studio 2008 SP1 zu aktualisieren, verwenden Sie die (nicht unterstützte) mvcappconverter-Anwendung. Sie können diese Anwendung unter folgender URL herunterladen:

[https://go.microsoft.com/fwlink/?LinkID=185351](https://go.microsoft.com/fwlink/?LinkID=185351)

## <a name="manually-upgrading-an-aspnet-mvc-10-project"></a>Manuelles Upgrade eines ASP.NET MVC 1,0-Projekts

Führen Sie die folgenden Schritte aus, um eine vorhandene ASP.NET MVC 1,0-Anwendung manuell auf Version 2 zu aktualisieren:

1. Erstellen Sie eine Sicherung des vorhandenen Projekts.
2. Öffnen Sie in einem Text-Editor die Projektdatei (die Datei mit der Dateierweiterung ". csproj" oder ". vbproj"), und suchen Sie nach dem projecttypeer GUID-Element. Ersetzen Sie als Wert dieses Elements die GUID {603c0e0b-db56-11dc-be95-000d561079b0} durch {F85E285D-A4E0-4152-9332-AB1D724D3325}. Wenn Sie den Vorgang abgeschlossen haben, sollte der Wert dieses Elements wie folgt lauten: 

    `{F85E285D-A4E0-4152-9332-AB1D724D3325};{349c5851-65df-11da-9384-00065b846f21};{fae04ec0-301f-11d3-bf4b-00c04f79efbc}`
3. Bearbeiten Sie die Datei "Web. config" im Stamm Ordner der Webanwendung. Suchen Sie nach System. Web. MVC, Version = 1.0.0.0, und ersetzen Sie alle Instanzen durch System. Web. MVC, Version = 2.0.0.0.
4. Wiederholen Sie den vorherigen Schritt für die Datei "Web. config" im Ordner "Views".
5. Öffnen Sie das Projekt mit Visual Studio, und erweitern Sie in **Projektmappen-Explorer**den Knoten **Verweise** . Löschen Sie den Verweis auf System. Web. MVC (der auf die Assembly der Version 1,0 verweist). Fügen Sie einen Verweis auf System. Web. MVC (v 2.0.0.0) hinzu.
6. Fügen Sie das folgende bindingRedirect-Element der Datei "Web. config" im Anwendungs Stamm im Abschnitt "Konfigurations" hinzu:   

    [!code-xml[Main](aspnet-mvc2-upgrade-notes/samples/sample1.xml)]
7. Erstellen Sie eine neue leere ASP.NET MVC 2-Anwendung. Kopieren Sie die Dateien aus dem Ordner "Scripts" der neuen Anwendung in den Ordner "Scripts" der vorhandenen Anwendung.
8. Aktualisieren Sie die vorhandene CSS-Datei "Application-™ s" mit den CSS-Format Definitionen in der Datei "Site. CSS".
9. Kompilieren Sie die Anwendung, und führen Sie Sie aus. Wenn Fehler auftreten, finden Sie weitere Informationen im Abschnitt wichtige Änderungen auf der Seite [Neuerungen in ASP.NET MVC 2](https://go.microsoft.com/fwlink/?LinkID=185038) .
