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
# <a name="owin-oauth-20-authorization-server"></a><span data-ttu-id="49953-104">OWIN-OAuth 2.0-Autorisierungsserver</span><span class="sxs-lookup"><span data-stu-id="49953-104">OWIN OAuth 2.0 Authorization Server</span></span>

<span data-ttu-id="49953-105">Das [OAuth 2,0-Framework](http://tools.ietf.org/html/rfc6749) ermöglicht es einer Drittanbieter-APP, eingeschränkten Zugriff auf einen HTTP-Dienst zu erhalten.</span><span class="sxs-lookup"><span data-stu-id="49953-105">The [OAuth 2.0 framework](http://tools.ietf.org/html/rfc6749) enables a third-party app to obtain limited access to an HTTP service.</span></span> <span data-ttu-id="49953-106">Anstatt die Anmelde Informationen des Ressourcen Besitzers für den Zugriff auf eine geschützte Ressource zu verwenden, erhält der Client ein Zugriffs Token (bei dem es sich um eine Zeichenfolge handelt, die einen bestimmten Bereich, eine bestimmte Lebensdauer und andere Zugriffs Attribute bezeichnet).</span><span class="sxs-lookup"><span data-stu-id="49953-106">Instead of using the resource owner's credentials to access a protected resource, the client obtains an access token (which is a string denoting a specific scope, lifetime, and other access attributes).</span></span> <span data-ttu-id="49953-107">Zugriffs Token werden von einem autorisierungsserver mit der Genehmigung des Ressourcen Besitzers an Clients von Drittanbietern ausgegeben.</span><span class="sxs-lookup"><span data-stu-id="49953-107">Access tokens are issued to third-party clients by an authorization server with the approval of the resource owner.</span></span>

<span data-ttu-id="49953-108">Owin definiert eine Standardschnittstelle zwischen .net-Webservern und Webanwendungen.</span><span class="sxs-lookup"><span data-stu-id="49953-108">OWIN defines a standard interface between .NET web servers and web applications.</span></span> <span data-ttu-id="49953-109">Das Ziel der owin-Schnittstelle besteht darin, Server und Anwendungen zu entkoppeln, die Entwicklung einfacher Module für die .net-Webentwicklung zu fördern und durch einen offenen Standard das Open Source-Ökosystem der .net-Webentwicklungs Tools zu fördern.</span><span class="sxs-lookup"><span data-stu-id="49953-109">The goal of the OWIN interface is to decouple server and application, encourage the development of simple modules for .NET web development, and, by being an open standard, stimulate the open source ecosystem of .NET web development tools.</span></span>

<span data-ttu-id="49953-110">Weitere Informationen finden Sie unter [owin](http://owin.org/) .</span><span class="sxs-lookup"><span data-stu-id="49953-110">For more information, see [OWIN](http://owin.org/)</span></span>
