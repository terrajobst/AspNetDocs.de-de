---
uid: aspnet/overview/owin-and-katana/owin-oauth-20-authorization-server
title: Owin OAuth 2,0-Autorisierungs Server | Microsoft-Dokumentation
author: hongyes
description: Dieses Tutorial führt Sie durch die Implementierung eines OAuth 2,0-Autorisierungs Servers mithilfe von owin OAuth-Middleware. Dabei handelt es sich um ein erweitertes Tutorial, das nur outlin...
ms.author: riande
ms.date: 01/28/2019
ms.assetid: 20acee16-c70c-41e9-b38f-92bfcf9a4c1c
msc.legacyurl: /aspnet/overview/owin-and-katana/owin-oauth-20-authorization-server
msc.type: authoredcontent
ms.openlocfilehash: 39c78ee57f791a94af7a5fb66c3ac42d7239760f
ms.sourcegitcommit: b95316530fa51087d6c400ff91814fe37e73f7e8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/23/2019
ms.locfileid: "70000683"
---
# <a name="owin-oauth-20-authorization-server"></a>OWIN-OAuth 2.0-Autorisierungsserver

Das [OAuth 2,0-Framework](http://tools.ietf.org/html/rfc6749) ermöglicht es einer Drittanbieter-APP, eingeschränkten Zugriff auf einen HTTP-Dienst zu erhalten. Anstatt die Anmelde Informationen des Ressourcen Besitzers für den Zugriff auf eine geschützte Ressource zu verwenden, erhält der Client ein Zugriffs Token (bei dem es sich um eine Zeichenfolge handelt, die einen bestimmten Bereich, eine bestimmte Lebensdauer und andere Zugriffs Attribute bezeichnet). Zugriffs Token werden von einem autorisierungsserver mit der Genehmigung des Ressourcen Besitzers an Clients von Drittanbietern ausgegeben.

Owin definiert eine Standardschnittstelle zwischen .net-Webservern und Webanwendungen. Das Ziel der owin-Schnittstelle besteht darin, Server und Anwendungen zu entkoppeln, die Entwicklung einfacher Module für die .net-Webentwicklung zu fördern und durch einen offenen Standard das Open Source-Ökosystem der .net-Webentwicklungs Tools zu fördern.

Weitere Informationen finden Sie unter [owin](http://owin.org/) .
