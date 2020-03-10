---
uid: visual-studio/overview/2013/aspnet-scaffolding-overview
title: ASP.net Gerüstbau in Visual Studio 2013 | Microsoft-Dokumentation
author: Rick-Anderson
description: ASP.net Gerüstbau ist ein neues Feature, das in Visual Studio 2013 enthalten ist.
ms.author: riande
ms.date: 04/09/2014
ms.assetid: a41ec9d4-8287-4f31-9e2a-460e7b7f04be
msc.legacyurl: /visual-studio/overview/2013/aspnet-scaffolding-overview
msc.type: authoredcontent
ms.openlocfilehash: cf4669b769cee28475e2dd6a6ddf07ea1434d04d
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78449565"
---
# <a name="aspnet-scaffolding-in-visual-studio-2013"></a>ASP.NET-Gerüstbau in Visual Studio 2013

von [Tom fitzmacken](https://github.com/tfitzmac)

> ASP.net Gerüstbau ist ein neues Feature, das in Visual Studio 2013 enthalten ist.

## <a name="overview"></a>Übersicht

ASP.net Gerüstbau ist ein Code Generierungs Framework für ASP.NET-Webanwendungen. Visual Studio 2013 enthält vorinstallierte Code Generatoren für MVC-und Web-API-Projekte. Sie fügen dem Projekt Gerüstbau hinzu, wenn Sie schnell Code hinzufügen möchten, der mit Datenmodellen interagiert. Die Verwendung von Gerüstbau kann den Zeitaufwand für die Entwicklung von Standarddaten Vorgängen in Ihrem Projekt verkürzen.

Standardmäßig unterstützt Visual Studio 2013 das Erstellen von Code für ein Web Forms Projekt nicht, aber Sie können Gerüstbau mit Web Forms verwenden, indem Sie dem Projekt entweder MVC-Abhängigkeiten hinzufügen oder eine Erweiterung installieren. Beide Ansätze sind unten dargestellt.

Visual Studio 2013 Update 2 (derzeit RC) bietet die Möglichkeit, ASP.net-Gerüstbau zu erweitern, um die Anforderungen Ihres Szenarios zu erfüllen. Mit dieser Funktion können Sie eine angepasste Gerüst Vorlage erstellen und hinzufügen, um das Dialogfeld Neues Gerüst hinzufügen hinzuzufügen. In der angepassten Vorlage geben Sie den Code an, der generiert wird, wenn Sie ein Gerüst Element hinzufügen. Weitere Informationen finden Sie unter [Erstellen eines benutzerdefinierten Gerüst für Visual Studio](https://go.microsoft.com/fwlink/p/?LinkId=395029).

## <a name="prerequisites"></a>Voraussetzungen

Um ASP.net Gerüstbau verwenden zu können, benötigen Sie Folgendes:

- Microsoft Visual Studio 2013
- WebEntwicklertools (Teil der standardmäßigen Visual Studio 2013 Installation)
- ASP.net Web Frameworks und Tools 2013 (Bestandteil der Standard Visual Studio 2013 Installation)

## <a name="add-a-scaffolded-item-to-mvc-or-web-api"></a>Hinzufügen eines Gerüsts zu MVC oder Web-API

Wenn Sie ein Gerüst hinzufügen möchten, klicken Sie mit der rechten Maustaste entweder auf das Projekt oder einen Ordner innerhalb des Projekts, und wählen Sie **Hinzufügen** – **Neues Gerüstbau Element**aus, wie in der folgenden Abbildung dargestellt.

![Gerüst Element hinzufügen](aspnet-scaffolding-overview/_static/image1.png)

Wählen Sie im Fenster **Gerüst hinzufügen** den Objekttyp aus, den Sie hinzufügen möchten.

![Typ des Gerüsts auswählen](aspnet-scaffolding-overview/_static/image2.png)

Das Fenster **Controller hinzufügen** bietet Ihnen die Möglichkeit, Optionen zum Erstellen des Controllers auszuwählen. Dies umfasst auch, ob Sie die neuen Async-Funktionen von Entity Framework 6 verwenden möchten.

![Controller hinzufügen](aspnet-scaffolding-overview/_static/image3.png)

Die relevanten Klassen und Seiten werden für Ihr Szenario erstellt. Die folgende Abbildung zeigt z. b. den MVC-Controller und Sichten, die über Gerüstbau für eine Modell Klasse mit dem Namen Filme erstellt wurden.

![Die erstellten Dateien](aspnet-scaffolding-overview/_static/image4.png)

## <a name="add-a-scaffolded-item-to-web-forms"></a>Fügen Sie ein Gerüst Element Web Forms

Zum Hinzufügen von Gerüstbau, der Web Forms Code generiert, müssen Sie entweder eine Erweiterung in Visual Studio installieren oder MVC-Abhängigkeiten hinzufügen. Beide Ansätze sind unten dargestellt, aber Sie müssen nur einen dieser Ansätze durchführen.

### <a name="web-forms-scaffolding-extension"></a>Erweiterung des Web Forms Gerüsts

Sie können eine Visual Studio-Erweiterung installieren, die es Ihnen ermöglicht, Gerüstbau mit einem Web Forms Projekt zu verwenden. **Wählen Sie** in Visual Studio Extras und dann **Erweiterungen und Updates**aus. In diesem Dialogfeld Suchen Sie in der Visual Studio Gallery nach **Web Forms Gerüstbau**.

![Web Forms-Gerüst installieren](aspnet-scaffolding-overview/_static/image5.png)

Weitere Informationen finden Sie unter [Web Forms Gerüstbau](https://go.microsoft.com/fwlink/p/?LinkId=396478).

### <a name="mvc-dependencies"></a>MVC-Abhängigkeiten

Wenn Sie MVC-Abhängigkeiten hinzufügen möchten, wählen Sie - **Neues Gerüst Element** **Hinzufügen** aus. Wählen Sie im Fenster Gerüst hinzufügen die Option **MVC-Abhängigkeiten**aus, wie unten gezeigt.

![MVC-Abhängigkeiten hinzufügen](aspnet-scaffolding-overview/_static/image6.png)

Es gibt zwei Optionen für das Gerüstbau von MVC: Minimal und vollständig. Wenn Sie minimal auswählen, werden nur die nuget-Pakete und-Verweise für ASP.NET MVC dem Projekt hinzugefügt. Wenn Sie die Option vollständig auswählen, werden die minimalen Abhängigkeiten sowie die erforderlichen Inhalts Dateien für ein MVC-Projekt hinzugefügt. Um das Gerüst problemlos zu verwenden, wählen Sie vollständige Abhängigkeiten aus.

![vollständige Abhängigkeiten auswählen](aspnet-scaffolding-overview/_static/image7.png)

Nachdem Sie die Abhängigkeiten hinzugefügt haben, wird die Datei "Infodatei **. txt** " angezeigt. Befolgen Sie die Anweisungen in dieser Datei, um sicherzustellen, dass das Projekt ordnungsgemäß funktioniert.

Wenn Sie die Schritte in der Datei "Infodatei. txt" ausgeführt haben, können Sie ein neues Gerüsts hinzufügen, wie im vorherigen Abschnitt über MVC und die Web-API gezeigt. Die automatisch generierten Sichten und der Controller funktionieren in Ihrem Projekt ordnungsgemäß.

## <a name="tutorials"></a>Lernprogramme

Informationen zum Erstellen eines angepassten Gerüstes finden Sie unter [Erstellen eines benutzerdefinierten Gerüstes für Visual Studio](https://go.microsoft.com/fwlink/p/?LinkId=395029).

Informationen zum Anpassen der generierten Dateien finden Sie unter [Anpassen der generierten Dateien aus dem Dialogfeld "neues Gerüst Element](https://blogs.msdn.com/b/webdev/archive/2013/12/26/how-to-customize-the-generated-files-from-the-new-scaffolded-item-dialog.aspx)".

Ein Beispiel für die Verwendung von Gerüstbau mit **Database First Entwicklung**finden Sie unter [EF Database First mit ASP.NET MVC](../../../mvc/overview/getting-started/database-first-development/setting-up-database.md).

Ein Beispiel für die Verwendung von Gerüstbau in einem **MVC** -Projekt finden Sie unter [Getting Started with ASP.NET MVC 5](../../../mvc/overview/getting-started/introduction/getting-started.md).

Ein Beispiel für die Verwendung von Gerüstbau in einem **Web-API** -Projekt finden Sie unter [Erstellen einer Rest-API mit Attribut Routing in der Web-API 2](../../../web-api/overview/web-api-routing-and-actions/create-a-rest-api-with-attribute-routing.md).
