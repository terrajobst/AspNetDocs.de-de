---
uid: web-pages/overview/ui-layouts-and-themes/creating-and-using-a-helper-in-an-aspnet-web-pages-site
title: Erstellen und Verwenden eines Hilfsobjekts in einer ASP.net Web Pages-(Razor-) Website | Microsoft-Dokumentation
author: Rick-Anderson
description: In diesem Artikel wird beschrieben, wie ein Hilfsprogramm auf einer ASP.net Web Pages-Website (Razor) erstellt wird. Ein Hilfsprogramm ist eine wiederverwendbare Komponente, die Code und Markup für die Leistung enthält...
ms.author: riande
ms.date: 02/17/2014
ms.assetid: 46bff772-01e0-40f0-9ae6-9e18c5442ee6
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/creating-and-using-a-helper-in-an-aspnet-web-pages-site
msc.type: authoredcontent
ms.openlocfilehash: 380663951094c9fc7d5f0601e30995fa073a204b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78454305"
---
# <a name="creating-and-using-a-helper-in-an-aspnet-web-pages-razor-site"></a>Erstellen und Verwenden eines Hilfsobjekts in einer ASP.net Web Pages-(Razor-) Site

von [Tom fitzmacken](https://github.com/tfitzmac)

> In diesem Artikel wird beschrieben, wie ein Hilfsprogramm auf einer ASP.net Web Pages-Website (Razor) erstellt wird. Ein Hilfsprogramm ist eine wiederverwendbare Komponente, die Code und Markup enthält, *um eine Aufgabe* auszuführen, die vielleicht mühsam oder komplex ist.
> 
> **Lernen Sie Folgendes:** 
> 
> - Erstellen und Verwenden eines einfachen Hilfsprogramms.
> 
> Dies sind die ASP.NET-Funktionen, die im Artikel eingeführt wurden:
> 
> - Die `@helper`-Syntax.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Im Tutorial verwendete Software Versionen
> 
> 
> - ASP.net Web Pages (Razor) 3
>   
> 
> Dieses Tutorial funktioniert auch mit ASP.net Web Pages 2.

## <a name="overview-of-helpers"></a>Übersicht über Hilfsprogramme

Wenn Sie dieselben Aufgaben auf verschiedenen Seiten auf Ihrer Website ausführen müssen, können Sie ein Hilfsprogramm verwenden. ASP.net Web Pages umfasst eine Reihe von Hilfsprogrammen, die Sie herunterladen und installieren können. (Eine Liste der integrierten Hilfsprogramme in ASP.net Web Pages finden Sie in der Kurzübersicht über die [ASP.NET-API](https://go.microsoft.com/fwlink/?LinkId=202907).) Wenn keines der vorhandenen Hilfsprogramme Ihren Anforderungen entspricht, können Sie ein eigenes Hilfsprogramm erstellen.

Mit einem-Hilfsprogramm können Sie einen gemeinsamen Codeblock über mehrere Seiten hinweg verwenden. Angenommen, Sie möchten auf Ihrer Seite häufig ein Notiz Element erstellen, das von den normalen Absätzen getrennt ist. Der Hinweis wird möglicherweise als `<div>` Element erstellt, das als Feld mit einem Rahmen formatiert ist. Anstatt der Seite jedes Mal, wenn Sie eine Notiz anzeigen möchten, dasselbe Markup hinzuzufügen, können Sie das Markup als Hilfsobjekt verpacken. Anschließend können Sie den Hinweis in eine einzelne Codezeile einfügen, wo Sie ihn benötigen.

Wenn Sie ein Hilfsobjekt verwenden, wird der Code in jeder Ihrer Seiten einfacher und leichter lesbar. Außerdem ist es einfacher, Ihre Website zu verwalten, denn wenn Sie das Aussehen der Notizen ändern müssen, können Sie das Markup an einem Ort ändern.

## <a name="creating-a-helper"></a>Erstellen eines Hilfsprogramms

In diesem Verfahren wird gezeigt, wie Sie das Hilfsprogramm erstellen, das den Hinweis erstellt, wie soeben beschrieben. Dies ist ein einfaches Beispiel, aber das benutzerdefinierte Hilfsprogramm kann beliebigen Markup-und ASP.NET-Code enthalten, den Sie benötigen.

1. Erstellen Sie im Stamm Ordner der Website einen Ordner namens *App\_Code*. Dies ist ein reservierter Ordnername in ASP.net, in dem Sie Code für Komponenten wie Hilfsprogramme einfügen können.
2. Erstellen Sie in der *App\_-Code* Ordners eine neue *cshtml* -Datei, und nennen Sie Sie *myhilfsprogramme. cshtml*.
3. Ersetzen Sie den vorhandenen Inhalt durch Folgendes:

    [!code-cshtml[Main](creating-and-using-a-helper-in-an-aspnet-web-pages-site/samples/sample1.cshtml)]

    Der Code verwendet die `@helper` Syntax, um ein neues Hilfsprogramm mit dem Namen `MakeNote`zu deklarieren. Mit diesem speziellen Hilfsprogramm können Sie einen Parameter mit dem Namen `content` übergeben, der eine Kombination aus Text und Markup enthalten kann. Das Hilfsprogramm fügt die Zeichenfolge mithilfe der `@content` Variable in den Hinweis Text ein.

    Beachten Sie, dass die Datei " *myhelper. cshtml*" heißt, das Hilfsprogramm aber `MakeNote`heißt. Sie können mehrere benutzerdefinierte Hilfsprogramme in eine einzelne Datei einfügen.
4. Speichern und schließen Sie die Datei.

## <a name="using-the-helper-in-a-page"></a>Verwenden des Hilfsprogramms in einer Seite

1. Erstellen Sie im Stamm Ordner eine neue leere Datei mit dem Namen " *TestHelper. cshtml*".
2. Fügen Sie folgenden Code in die Datei ein:

    [!code-html[Main](creating-and-using-a-helper-in-an-aspnet-web-pages-site/samples/sample2.html)]

    Um das von Ihnen erstellte Hilfsprogramm aufzurufen, verwenden Sie `@` gefolgt von dem Dateinamen, in dem das Hilfsprogramm ist, einem Punkt und dem Namen des Hilfsprogramms. (Wenn Sie über mehrere Ordner in der *App\_-Code* Ordner verfügen, können Sie die-Syntax `@FolderName.FileName.HelperName` verwenden, um das Hilfsprogramm in einer beliebigen geschachtelten Ordnerebene aufzurufen.) Der Text, den Sie in Anführungszeichen innerhalb der Klammern einfügen, ist der Text, der vom Hilfsprogramm als Teil des Notiz Teils der Webseite angezeigt wird.
3. Speichern Sie die Seite, und führen Sie Sie in einem Browser aus. Das Hilfsprogramm generiert das Notiz Element, in dem Sie das Hilfsobjekt aufgerufen haben: zwischen den beiden Absätzen.

    ![Ein Screenshot, der die Seite im Browser anzeigt und zeigt, wie das Hilfsprogramm Markup generiert hat, das ein Feld um den angegebenen Text einfügt.](creating-and-using-a-helper-in-an-aspnet-web-pages-site/_static/image1.png)

## <a name="additional-resources"></a>Zusätzliche Ressourcen

[Horizontales Menü als Razor](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2341)-Hilfsobjekt. In diesem Blogbeitrag von Mike Papst wird gezeigt, wie Sie ein horizontales Menü als Hilfsobjekt mit Markup, CSS und Code erstellen.

[Nutzen von HTML5 in ASP.net Web Pages-Hilfsprogramme für webmatrix und ASP.net MVC3](http://geekswithblogs.net/wildturtle/archive/2010/11/08/html5-in-asp.net-web-pages-helpers-for-webmatrix-and_aspnet_mvc3.aspx). Dieser Blogeintrag von Sam Abraham zeigt ein Hilfsobjekt, das ein HTML5-`Canvas` Element rendert.

[Der Unterschied zwischen @Helpers und @Functions in webmatrix](http://www.mikesdotnetting.com/Article/173/The-Difference-Between-@Helpers-and-@Functions-In-WebMatrix). Dieser Blogeintrag von Mike Brind beschreibt `@helper` Syntax und `@function` Syntax und wann Sie verwendet werden sollten.
