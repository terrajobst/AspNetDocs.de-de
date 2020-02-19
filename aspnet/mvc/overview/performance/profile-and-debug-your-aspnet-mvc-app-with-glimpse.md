---
uid: mvc/overview/performance/profile-and-debug-your-aspnet-mvc-app-with-glimpse
title: Profilerstellung und Debuggen Ihrer ASP.NET MVC-App mit Blick auf | Microsoft-Dokumentation
author: Rick-Anderson
description: Der Einblick ist eine wachsende und wachsende Familie von Open Source-nuget-Paketen, die ausführliche Informationen zu Leistung, Debugging und Diagnose für ASP.net a...
ms.author: riande
ms.date: 03/26/2015
ms.assetid: c205805f-efdd-4fa7-9616-f26eab180611
msc.legacyurl: /mvc/overview/performance/profile-and-debug-your-aspnet-mvc-app-with-glimpse
msc.type: authoredcontent
ms.openlocfilehash: d3689147a3bc3aa1f4180c377d2483a94bdd95a9
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/19/2020
ms.locfileid: "77457660"
---
# <a name="profile-and-debug-your-aspnet-mvc-app-with-glimpse"></a>Erstellen von Profilen und Debuggen der ASP.NET MVC-App mit Glimpse

von [Rick Anderson](https://twitter.com/RickAndMSFT)

> Der Einblick ist eine wachsende und wachsende Familie von Open Source-nuget-Paketen, die ausführliche Leistungs-, Debug-und Diagnoseinformationen für ASP.net-apps bereitstellt. Es ist trivial, einfach zu installieren, einfach und extrem schnell und zeigt wichtige Leistungsmetriken unten auf jeder Seite an. Sie können einen Drilldown in Ihre APP ausführen, wenn Sie herausfinden möchten, was auf dem Server läuft. Der Einblick bietet so viel nützliche Informationen, die Sie im gesamten Entwicklungszeitraum, einschließlich ihrer Azure-Testumgebung, verwenden sollten. [Wenngleich die](http://www.telerik.com/fiddler) Tools und die [F-12-Entwicklungs Tools](https://msdn.microsoft.com/library/ie/gg589512(v=vs.85).aspx) eine Client seitige Ansicht bereitstellen, bietet der Einblick eine detaillierte Ansicht des Servers. Dieses Tutorial konzentriert sich auf die Verwendung von "Glimpse ASP.NET MVC-und EF-Paketen", aber es sind viele weitere Pakete verfügbar. Wenn möglich, verwende ich eine Verknüpfung zu [den entsprechenden Informationen](http://getglimpse.com/Docs/) , die ich bei der Pflege unterstützen werde. Der Einblick ist ein Open-Source-Projekt. Sie können auch zum Quellcode und zu den Dokumenten beitragen.

- [Installieren von Glimpse](#ig)
- [Aktivieren Sie den Einblick für localhost.](#eg)
- [Registerkarte Zeitachse](#Time)
- [Modellbindung](#mb)
- [Routen](#route)
- [Verwenden von Glimpse in Azure](#da)
- [Weitere Ressourcen](#addRes)

<a id="ig"></a>
## <a name="installing-glimpse"></a>Installieren von Glimpse

Sie können den Einblick über die nuget-Paket-Manager-Konsole oder über die Konsole **nuget-Pakete verwalten** installieren. Für diese Demo werden die Mvc5-und EF6-Pakete installiert:

![Installieren Sie den Einblick von nuget Dlg](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image1.png)

Suchen Sie nach " *Glimpse. EF* "

!["Glimpse. EF" aus der nuget-Installation Dlg](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image2.png)

Wenn Sie **installierte Pakete**auswählen, sehen Sie, dass die glimabhängigen Module installiert sind:

![Installierte Glimpse-Pakete von Dlg](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image3.png)

Mit den folgenden Befehlen werden die Module "Glimpse MVC5" und "EF6" von der Paket-Manager-Konsole

[!code-console[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample1.cmd)]

<a id="eg"></a>
## <a name="enable-glimpse-for-localhost"></a>Aktivieren Sie den Einblick für localhost.

Navigieren Sie zu http://localhost:&lt;p Ort #&gt;/Glimpse.axd, und klicken Sie auf die Schaltfläche " <strong>Glimpse</strong> ein".

![Seite "Blick auf axd"](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image4.png)

Wenn Sie die Favoritenleiste angezeigt haben, können Sie die Schaltflächen mit dem Drag & Drop ablegen und als Bookmarklets hinzufügen:

![IE mit Glimpse-Bookmarklets](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image5.png)

Sie können jetzt in der APP navigieren, und die **Heads-Up-Anzeige** (HUD) wird unten auf der Seite angezeigt.

![Kontakt-Manager-Seite mit HUD](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image6.png)

Auf der [Seite "Glid](http://getglimpse.com/Docs/Heads-up-Display) " finden Sie Informationen zu den oben gezeigten Zeit Steuerungsinformationen. Die unaufdringlichen Leistungsdaten, die in der HUD angezeigt werden, können Sie sofort über ein Problem benachrichtigen, bevor Sie zum Testzeitraum gelangen. Wenn Sie in der unteren rechten Ecke auf die &quot;g-&quot; klicken, wird der Bereich "Glimpse" angezeigt:

![Bereich mit Blick](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image7.png)

In der obigen Abbildung ist die [Registerkarte Ausführung](http://getglimpse.com/Docs/Execution-Tab) ausgewählt, mit der Details zur zeitlichen Steuerung der Aktionen und Filter in der Pipeline angezeigt werden. Der [Zeitgeber für den halte Filter](http://www.nuget.org/packages/StopWatch/) Start in Phase 6 der Pipeline wird angezeigt. Während mein Lightweight-Timer nützliche Profil-und Zeit Steuerungsdaten bereitstellen kann, gibt er die Zeit, die für die Autorisierung und das Rendering der Ansicht aufgewendet wird Weitere Informationen finden Sie unter [Profil und Zeit Ihrer ASP.NET MVC-app in Azure](https://blogs.msdn.com/b/webdev/archive/2014/07/29/profile-and-time-your-asp-net-mvc-app-all-the-way-to-azure.aspx). Die [Registerkarten](http://getglimpse.com/Docs/Tabs) Seite enthält Links zu detaillierten Informationen zu den einzelnen Registerkarten.

<a id="Time"></a>
## <a name="the-timeline-tab"></a>Registerkarte Zeitachse

Ich habe das ausstehende [EF 6/MVC 5-Tutorial](../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) von Tom Dykstra geändert, mit der folgenden Codeänderung am Dozenten Controller:

[!code-csharp[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample2.cs?highlight=1,20-31)]

Der obige Code ermöglicht es mir, die Abfrage Zeichenfolge (`eager`) zu übergeben, um das sorgfältige oder explizite Laden von Daten zu steuern. In der folgenden Abbildung wird Explizites Laden verwendet, und auf der Seite für die zeitliche Steuerung werden alle Anmeldungen angezeigt, die in der `Index`-Aktionsmethode geladen werden

![Explizites Laden](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image8.png)

Im folgenden Code wird "eifrig" angegeben, und jede Registrierung wird abgerufen, nachdem die `Index` Ansicht aufgerufen wurde:

!["eifrig" ist angegeben](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image9.png)

Sie können auf ein Zeitsegment zeigen, um ausführliche Informationen zur zeitlichen Steuerung zu erhalten:

![zeigen Sie auf eine ausführliche zeitliche Steuerung](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image10.png)

<a id="mb"></a>
## <a name="model-binding"></a>Modellbindung

Auf der [Registerkarte Modell Bindung](http://getglimpse.com/Docs/Model-Binding-Tab) finden Sie eine Fülle von Informationen, die Ihnen helfen zu verstehen, wie Ihre Formularvariablen gebunden werden und warum einige nicht erwartungsgemäß gebunden werden. Die Abbildung unten zeigt den **?** ein Symbol, auf das Sie klicken können, um die Hilfeseite für das entsprechende Feature aufzurufenden.

![Ansicht für Modell Bindung](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image11.png)

<a id="route"></a>
## <a name="routes"></a>Routen

 Auf der Registerkarte "Glimpse-Routen" können Sie das Routing und das Routing verstehen. In der folgenden Abbildung ist die Produkt Route ausgewählt (und wird in grün, eine Glimpse-Konvention) angezeigt. ![Produktname](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image12.png) Routen Einschränkungen ausgewählt werden, werden Bereiche und Daten Token ebenfalls angezeigt. Weitere Informationen finden Sie unter [Einblicke in Routen](http://getglimpse.com/Docs/Routes-Tab) und [Attribut Routing in ASP.NET MVC 5](https://blogs.msdn.com/b/webdev/archive/2013/10/17/attribute-routing-in-asp-net-mvc-5.aspx) . 

<a id="da"></a>
## <a name="using-glimpse-on-azure"></a>Verwenden von Glimpse in Azure

Mit der Standard Sicherheitsrichtlinie für einen Blick können nur Daten vom lokalen Host angezeigt werden. Sie können diese Sicherheitsrichtlinie ändern, sodass Sie diese Daten auf einem Remote Server anzeigen können (z. b. eine Web-App in Azure). Fügen Sie für Testumgebungen in Azure die markierte Markierung am Ende der Datei " *Web. config* " hinzu, um den Einblick zu aktivieren:

[!code-xml[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample3.xml?highlight=2-6)]

Mit dieser Änderung können alle Benutzer Ihre Daten auf einer Remote Website einsehen. Sie sollten das obige Markup einem Veröffentlichungs Profil hinzufügen, damit es nur bei Verwendung dieses Veröffentlichungs Profils (z. b. Ihres Azure-Test Profils) bereitgestellt wird. Um die Anzeige von Daten zu beschränken, fügen wir die `canViewGlimpseData` Rolle hinzu und gestatten Benutzern in dieser Rolle nur die Anzeige von Daten.

Entfernen Sie die Kommentare aus der Datei *GlimpseSecurityPolicy.cs* , und ändern Sie den [IsInRole](https://msdn.microsoft.com/library/system.security.principal.iprincipal.isinrole(v=vs.110).aspx) -Aufrufen von `Administrator` in die `canViewGlimpseData` Rolle:

[!code-csharp[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample4.cs?highlight=6)]

> [!WARNING]
> Sicherheit: die umfangreichen Daten, die von Glimpse bereitgestellt werden, können die Sicherheit Ihrer app verfügbar machen. Microsoft hat keine Sicherheitsüberprüfung für die Verwendung in den Produktions-apps durchgeführt.

Weitere Informationen zum Hinzufügen von Rollen finden Sie im Tutorial bereitstellen [einer Secure ASP.NET MVC 5-Web-App mit Mitgliedschaft, OAuth und SQL-Datenbank in Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/) .

<a id="addRes"></a>
## <a name="additional-resources"></a>Weitere Ressourcen

- [Stellen Sie eine sichere ASP.NET MVC 5-App mit Mitgliedschaft, OAuth und SQL-Datenbank in Azure bereit.](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/)
- [Glimpse-Konfiguration](http://getglimpse.com/Docs/Configuration) : Seite "doc" zum Konfigurieren von Registerkarten, Lauf Zeit Richtlinie, Protokollierung und mehr.
