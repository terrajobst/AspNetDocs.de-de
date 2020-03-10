---
uid: web-api/overview/formats-and-model-binding/model-validation-in-aspnet-web-api
title: Modell Validierung in ASP.net-Web-API-ASP.NET 4. x
author: MikeWasson
description: Übersicht über die Modell Validierung in ASP.net-Web-API für ASP.NET 4. x.
ms.author: riande
ms.date: 07/20/2012
ms.custom: seoapril2019
ms.assetid: 7d061207-22b8-4883-bafa-e89b1e7749ca
msc.legacyurl: /web-api/overview/formats-and-model-binding/model-validation-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 531a66b7ab642bd012663517640f2766f1917f25
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78448929"
---
# <a name="model-validation-in-aspnet-web-api"></a>Modell Validierung in ASP.net-Web-API

von [Mike Wasson](https://github.com/MikeWasson)

In diesem Artikel wird gezeigt, wie Sie Ihre Modelle mit Anmerkungen versehen, die Anmerkungen für die Datenvalidierung verwenden und Validierungs Fehler in Ihrer Web-API behandeln. Wenn ein Client Daten an Ihre Web-API sendet, empfiehlt es sich, die Daten vor der Verarbeitung zu validieren. 

## <a name="data-annotations"></a>Datenanmerkungen

In ASP.net-Web-API können Sie Attribute aus dem [System. ComponentModel. DataAnnotations](/dotnet/api/system.componentmodel.dataannotations) -Namespace verwenden, um Validierungsregeln für Eigenschaften in Ihrem Modell festzulegen. Sehen Sie sich das folgende Modell an:

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample1.cs)]

Wenn Sie die Modell Validierung in ASP.NET MVC verwendet haben, sollte dies Ihnen bekannt vorkommen. Das **erforderliche** Attribut besagt, dass die `Name`-Eigenschaft nicht NULL sein darf. Das **Range** -Attribut gibt an, dass `Weight` zwischen 0 und 999 liegen muss.

Angenommen, ein Client sendet eine Post-Anforderung mit der folgenden JSON-Darstellung:

[!code-json[Main](model-validation-in-aspnet-web-api/samples/sample2.json)]

Sie können sehen, dass der Client nicht die `Name`-Eigenschaft enthielt, die als erforderlich markiert ist. Wenn die Web-API den JSON-Code in eine `Product` Instanz konvertiert, überprüft er die `Product` anhand der Validierungs Attribute. In ihrer Controller Aktion können Sie überprüfen, ob das Modell gültig ist:

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample3.cs)]

Die Modell Validierung garantiert nicht, dass Client Daten sicher sind. In anderen Ebenen der Anwendung ist möglicherweise eine zusätzliche Überprüfung erforderlich. (Beispielsweise kann die Datenschicht Foreign Key-Einschränkungen erzwingen.) Im Tutorial [Verwenden der Web-API mit Entity Framework](../data/using-web-api-with-entity-framework/part-1.md) werden einige dieser Probleme untersucht.

**"Bei**der Bereitstellung": eine unter Veröffentlichung erfolgt, wenn der Client einige Eigenschaften verlässt. Nehmen wir beispielsweise an, der Client sendet Folgendes:

[!code-json[Main](model-validation-in-aspnet-web-api/samples/sample4.json)]

Hier hat der Client keine Werte für `Price` oder `Weight`angegeben. Der JSON-Formatierer weist den fehlenden Eigenschaften den Standardwert 0 (null) zu.

![](model-validation-in-aspnet-web-api/_static/image1.png)

Der Modell Zustand ist gültig, da NULL ein gültiger Wert für diese Eigenschaften ist. Ob dies ein Problem ist, hängt von Ihrem Szenario ab. Beispielsweise können Sie bei einem Aktualisierungs Vorgang zwischen "0" und "nicht festgelegt" unterscheiden. Legen Sie die Eigenschaft auf NULL fest, und legen Sie das **erforderliche** Attribut fest, um zu erzwingen, dass Clients einen Wert festlegen:

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample5.cs?highlight=1-2)]

**"Überschreiben"** : ein Client kann auch *mehr* Daten als erwartet senden. Beispiel:

[!code-json[Main](model-validation-in-aspnet-web-api/samples/sample6.json)]

Hier enthält die JSON-Eigenschaft eine Eigenschaft ("Color"), die im `Product` Modell nicht vorhanden ist. In diesem Fall ignoriert der JSON-Formatierer diesen Wert einfach. (Das XML-Formatierer führt dasselbe aus.) Eine Übertragung verursacht Probleme, wenn Ihr Modell über Eigenschaften verfügt, die Sie als schreibgeschützt feststellen möchten. Beispiel:

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample7.cs)]

Sie möchten nicht, dass Benutzer die `IsAdmin`-Eigenschaft aktualisieren und sich nicht auf Administratoren erhöhen! Die sicherste Strategie besteht darin, eine Modell Klasse zu verwenden, die genau mit dem übereinstimmt, was der Client senden darf:

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample8.cs)]

> [!NOTE]
> Der Blogbeitrag von Brad Wilson (Eingabe Überprüfung und[Modell Überprüfung in ASP.NET MVC](http://bradwilson.typepad.com/blog/2010/01/input-validation-vs-model-validation-in-aspnet-mvc.html)) enthält eine gute Erörterung der unter-und über-und über-und Übertragung. Obwohl es sich bei dem Beitrag um ASP.NET MVC 2 handelt, sind die Probleme weiterhin für die Web-API relevant.

## <a name="handling-validation-errors"></a>Behandeln von Validierungs Fehlern

Die Web-API gibt beim Fehlschlagen der Validierung nicht automatisch einen Fehler an den Client zurück. Es liegt an der Controller Aktion, den Modell Status zu überprüfen und entsprechend zu reagieren.

Sie können auch einen Aktionsfilter erstellen, um den Modell Status zu überprüfen, bevor die Controller Aktion aufgerufen wird. Der folgende Code zeigt ein Beispiel:

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample9.cs)]

Wenn die Modell Validierung fehlschlägt, gibt dieser Filter eine HTTP-Antwort zurück, die die Validierungs Fehler enthält. In diesem Fall wird die Controller Aktion nicht aufgerufen.

[!code-console[Main](model-validation-in-aspnet-web-api/samples/sample10.cmd)]

Wenn Sie diesen Filter auf alle Web-API-Controller anwenden möchten, fügen Sie der **httpconfiguration. Filters** -Auflistung während der Konfiguration eine Instanz des Filters hinzu:

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample11.cs)]

Eine weitere Möglichkeit besteht darin, den Filter als Attribut für einzelne Controller oder Controller Aktionen festzulegen:

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample12.cs)]
