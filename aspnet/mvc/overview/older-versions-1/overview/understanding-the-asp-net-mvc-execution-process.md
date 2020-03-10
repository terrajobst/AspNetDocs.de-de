---
uid: mvc/overview/older-versions-1/overview/understanding-the-asp-net-mvc-execution-process
title: Grundlegendes zum ASP.NET MVC-Ausführungsprozess | Microsoft-Dokumentation
author: microsoft
description: Erfahren Sie, wie das ASP.NET MVC-Framework eine Browser Anforderung Schritt für Schritt verarbeitet.
ms.author: riande
ms.date: 01/27/2009
ms.assetid: d1608db3-660d-4079-8c15-f452ff01f1db
msc.legacyurl: /mvc/overview/older-versions-1/overview/understanding-the-asp-net-mvc-execution-process
msc.type: authoredcontent
ms.openlocfilehash: 28940947253e0af43886cf1231f8aaf4615526cc
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78435453"
---
# <a name="understanding-the-aspnet-mvc-execution-process"></a>Grundlegendes zum ASP.NET MVC-Ausführungsprozess

von [Microsoft](https://github.com/microsoft)

> Erfahren Sie, wie das ASP.NET MVC-Framework eine Browser Anforderung Schritt für Schritt verarbeitet.

Anforderungen an eine ASP.NET MVC-basierte Webanwendung durchlaufen zunächst das **UrlRoutingModule** -Objekt, bei dem es sich um ein HTTP-Modul handelt. Dieses Modul analysiert die Anforderung und führt die Routenauswahl aus. Das **UrlRoutingModule** -Objekt wählt das erste Routen Objekt aus, das mit der aktuellen Anforderung übereinstimmt. (Ein Routen Objekt ist eine Klasse, die **RouteBase**implementiert, und ist in der Regel eine Instanz der **Route** -Klasse.) Wenn keine Routen gefunden werden, bewirkt das **UrlRoutingModule** -Objekt nichts, und die Anforderung kann auf die reguläre ASP.net-oder IIS-Anforderungs Verarbeitung zurückgreifen.

Vom ausgewählten **Routen** Objekt erhält das **UrlRoutingModule** -Objekt das **IRouteHandler** -Objekt, das dem **Routen** Objekt zugeordnet ist. In einer MVC-Anwendung handelt es sich in der Regel um eine Instanz von **MvcRouteHandler**. Die **IRouteHandler** -Instanz erstellt ein **IHttpHandler** -Objekt und übergibt es an das **ihttpcontext** -Objekt. Standardmäßig ist die **IHttpHandler** -Instanz für MVC das **MvcHandler** -Objekt. Das **MvcHandler** -Objekt wählt dann den Controller aus, der letztendlich die Anforderung verarbeitet.

> [!NOTE]
> Wenn eine ASP.NET MVC-Webanwendung in IIS 7,0 ausgeführt wird, ist für MVC-Projekte keine Dateinamenerweiterung erforderlich. In IIS 6,0 erfordert der Handler jedoch, dass Sie die MVC-Dateinamenerweiterung der ASP.NET ISAPI-DLL zuordnen.

Das Modul und der Handler sind die Einstiegspunkte zum ASP.NET-MVC-Framework. Sie führen die folgenden Aktionen aus:

- Wählen Sie den entsprechenden Controller in einer MVC-Webanwendung aus.
- Abrufen einer bestimmten Controller Instanz.
- Ruft die **Execute** -Methode des Controllers auf.

Im folgenden finden Sie eine Liste der Ausführungsphasen für ein MVC-Webprojekt:

- Erste Anforderung für die Anwendung empfangen 

    - In der Datei Global. asax werden **Routen** Objekte dem **RouteTable** -Objekt hinzugefügt.
- Routing ausführen 

    - Das Modul **UrlRoutingModule** verwendet das erste übereinstimmende **Routen** Objekt in der **RouteTable** -Auflistung, um das **RouteData** -Objekt zu erstellen, das dann verwendet wird, um ein **RequestContext** -Objekt (**ihttpcontext**) zu erstellen.
- MVC-Anforderungs Handler erstellen 

    - Das **MvcRouteHandler** -Objekt erstellt eine Instanz der **MvcHandler** -Klasse und übergibt sie an die **RequestContext** -Instanz.
- Controller erstellen 

    - Das **MvcHandler** -Objekt verwendet die **RequestContext** -Instanz, um das **IControllerFactory** -Objekt zu identifizieren (in der Regel eine Instanz der **defaultcontrollerfactory** -Klasse), mit der die Controller Instanz erstellt werden soll.
- Controller ausführen: die **MvcHandler** -Instanz Ruft die **Execute** -Methode des Controllers auf. |
- Aktion aufrufen 

    - Die meisten Controller erben von der **Controller** -Basisklasse. Für Controller, die dies tun, bestimmt das **ControllerAction Invoker** -Objekt, das dem Controller zugeordnet ist, welche Aktionsmethode der Controller Klasse aufgerufen werden soll, und ruft dann diese Methode auf.
- Ergebnis ausführen 

    - Eine typische Aktionsmethode empfängt möglicherweise Benutzereingaben, bereitet die entsprechenden Antwortdaten vor und führt das Ergebnis aus, indem ein Ergebnistyp zurückgegeben wird. Folgende integrierte Ergebnistypen können ausgeführt werden: **ViewResult** (das eine Ansicht rendert und der am häufigsten verwendete Ergebnistyp ist), **redirecttorouteresult**, **redirectresult**, **ContentResult**, **jsonresult**und **EmptyResult**.
