---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-security-guidance
title: Sicherheits Leit Faden für ASP.net-Web-API 2 odata-ASP.NET 4. x
author: MikeWasson
description: Beschreibt Sicherheitsaspekte, die bei der Bereitstellung eines Datasets über odata für ASP.net-Web-API 2 auf ASP.NET 4. x zu beachten sind.
ms.author: riande
ms.date: 02/06/2013
ms.custom: seoapril2019
ms.assetid: b91e6424-1544-4747-bd0b-d1f8418c9653
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-security-guidance
msc.type: authoredcontent
ms.openlocfilehash: 8194a368cb0629c30e32ec05bf4bed150d442ad8
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78448293"
---
# <a name="security-guidance-for-aspnet-web-api-2-odata"></a>Sicherheits Leit Faden für ASP.net-Web-API 2 odata

von [Mike Wasson](https://github.com/MikeWasson)

In diesem Thema werden einige Sicherheitsaspekte beschrieben, die Sie beim verfügbar machen eines Datasets über odata für ASP.net-Web-API 2 auf ASP.NET 4. x berücksichtigen sollten.

## <a name="edm-security"></a>EDM-Sicherheit

Die Abfrage Semantik basiert auf dem Entity Data Model (EDM), nicht auf den zugrunde liegenden Modelltypen. Sie können eine Eigenschaft aus dem EDM ausschließen und sind für die Abfrage nicht sichtbar. Angenommen, das Modell enthält einen Employee-Typ mit einer Gehalt-Eigenschaft. Möglicherweise möchten Sie diese Eigenschaft aus dem EDM ausschließen, um Sie von Clients auszublenden.

Es gibt zwei Möglichkeiten, eine Eigenschaft aus dem EDM auszuschließen. Sie können das Attribut **[ignoredatamember]** für die Eigenschaft in der Modell Klasse festlegen:

[!code-csharp[Main](odata-security-guidance/samples/sample1.cs)]

Sie können die-Eigenschaft auch Programm gesteuert aus dem EDM entfernen:

[!code-csharp[Main](odata-security-guidance/samples/sample2.cs)]

## <a name="query-security"></a>Abfrage Sicherheit

Ein böswilliger oder naive Client kann möglicherweise eine Abfrage erstellen, die für die Ausführung sehr viel Zeit in Anspruch nimmt. Im schlimmsten Fall kann dies den Zugriff auf Ihren Dienst stören.

Das **[querable]** -Attribut ist ein Aktionsfilter, der die Abfrage analysiert, überprüft und anwendet. Der Filter konvertiert die Abfrage Optionen in einen LINQ-Ausdruck. Wenn der odata-Controller einen **iquerable** -Typ zurückgibt, konvertiert der **iquerable** LINQ-Anbieter den LINQ-Ausdruck in eine Abfrage. Daher ist die Leistung abhängig von dem verwendeten LINQ-Anbieter und den besonderen Merkmalen Ihres Datasets oder Datenbankschemas.

Weitere Informationen zur Verwendung von odata-Abfrage Optionen in ASP.net-Web-API finden Sie [unter unterstützen von odata-Abfrage Optionen](supporting-odata-query-options.md).

Wenn Sie wissen, dass alle Clients vertrauenswürdig sind (z. b. in einer Unternehmensumgebung) oder wenn das DataSet klein ist, ist die Abfrageleistung möglicherweise kein Problem. Andernfalls sollten Sie die folgenden Empfehlungen in Erwägung gezogen werden.

- Testen Sie den Dienst mit verschiedenen Abfragen, und erstellen Sie ein Profil der Datenbank.
- Aktivieren Sie das Server gesteuerte Paging, um zu vermeiden, dass ein großes Dataset in einer Abfrage zurückgegeben wird. Weitere Informationen finden Sie unter [Server gesteuerte Auslagerung](supporting-odata-query-options.md#server-paging). 

    [!code-csharp[Main](odata-security-guidance/samples/sample3.cs)]
- Benötigen Sie $Filter und $OrderBy? Einige Anwendungen ermöglichen möglicherweise das Paging von Clients, indem $Top und $Skip verwendet werden, aber die anderen Abfrage Optionen werden deaktiviert. 

    [!code-csharp[Main](odata-security-guidance/samples/sample4.cs)]
- Beschränken Sie $OrderBy auf Eigenschaften in einem gruppierten Index. Das Sortieren von großen Daten ohne gruppierten Index ist langsam. 

    [!code-csharp[Main](odata-security-guidance/samples/sample5.cs)]
- Maximale Anzahl von Knoten: die **maxnodecount** -Eigenschaft für **[querable]** legt die maximale Anzahl von Knoten fest, die in der $Filter Syntax Struktur zulässig sind. Der Standardwert ist 100, Sie möchten jedoch möglicherweise einen niedrigeren Wert festlegen, da eine große Anzahl von Knoten langsam kompiliert werden kann. Dies trifft vor allem dann zu, wenn Sie LINQ to Objects verwenden (d. h. LINQ-Abfragen für eine Auflistung im Arbeitsspeicher, ohne einen LINQ-zwischen Anbieter). 

    [!code-csharp[Main](odata-security-guidance/samples/sample6.cs)]
- Deaktivieren Sie ggf. die Funktionen any () und all (), da diese langsam sein können. 

    [!code-csharp[Main](odata-security-guidance/samples/sample7.cs)]
- Wenn Zeichen folgen Eigenschaften z. b&#8212;. große Zeichen folgen enthalten, empfiehlt eine Produktbeschreibung&#8212;oder ein Blogeintrag die Deaktivierung der Zeichen folgen Funktionen. 

    [!code-csharp[Main](odata-security-guidance/samples/sample8.cs)]
- Sie sollten das Filtern auf Navigations Eigenschaften nicht zulassen. Das Filtern nach Navigations Eigenschaften kann zu einer Verknüpfung führen, die je nach Datenbankschema langsam verläuft. Der folgende Code zeigt ein Abfrage Validierungs Steuerelement, das das Filtern von Navigations Eigenschaften verhindert. Weitere Informationen zu Abfrage Validierungs Steuerelementen finden Sie unter [Abfrage Validierung](supporting-odata-query-options.md#query-validation). 

    [!code-csharp[Main](odata-security-guidance/samples/sample9.cs)]
- Beschränken Sie $Filter Abfragen, indem Sie ein Validierungs Steuerelement schreiben, das für Ihre Datenbank angepasst ist. Beachten Sie z. b. die folgenden beiden Abfragen: 

  - Alle Filme mit Actors, deren Nachname mit "A" beginnt.
  - Alle in 1994 veröffentlichten Filme.

    Wenn Filme nicht von Actors indiziert werden, kann es für die erste Abfrage erforderlich sein, dass die Datenbank-Engine die gesamte Liste der Filme scannt. Die zweite Abfrage ist zwar möglicherweise akzeptabel, angenommen, die Filme werden nach dem releasejahr indiziert.

    Der folgende Code zeigt ein Validierungs Steuerelement, das das Filtern nach den Eigenschaften "releaseyear" und "Title" ermöglicht, aber keine anderen Eigenschaften.

    [!code-csharp[Main](odata-security-guidance/samples/sample10.cs)]
- Im Allgemeinen sollten Sie überprüfen, welche $Filter Funktionen Sie benötigen. Wenn Ihre Clients die vollständige Ausdruckskraft $Filter nicht benötigen, können Sie die zulässigen Funktionen einschränken.
