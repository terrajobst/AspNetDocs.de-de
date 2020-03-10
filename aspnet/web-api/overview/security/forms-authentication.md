---
uid: web-api/overview/security/forms-authentication
title: Formular Authentifizierung in ASP.net-Web-API | Microsoft-Dokumentation
author: MikeWasson
description: Beschreibt die Verwendung der Formular Authentifizierung in ASP.net-Web-API.
ms.author: riande
ms.date: 12/12/2012
ms.assetid: 9f06c1f2-ffaa-4831-94a0-2e4a3befdf07
msc.legacyurl: /web-api/overview/security/forms-authentication
msc.type: authoredcontent
ms.openlocfilehash: 147bfab76e48497f35a72b28cd935f40ec4193bf
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78484377"
---
# <a name="forms-authentication-in-aspnet-web-api"></a>Formular Authentifizierung in ASP.net-Web-API

von [Mike Wasson](https://github.com/MikeWasson)

Bei der Formular Authentifizierung wird ein HTML-Formular verwendet, um die Anmelde Informationen des Benutzers an den Server zu senden. Es handelt sich nicht um einen Internet Standard. Die Formular Authentifizierung eignet sich nur für Web-APIs, die von einer Webanwendung aufgerufen werden, damit der Benutzer mit dem HTML-Formular interagieren kann.

| Vorteile | Nachteile |
| --- | --- |
| -Einfach zu implementieren: in ASP.NET integriert. -Verwendet den ASP.net-Mitgliedschafts Anbieter, der die Verwaltung von Benutzerkonten erleichtert. | -Kein standardmäßiger HTTP-Authentifizierungsmechanismus; verwendet HTTP-Cookies anstelle des standardmäßigen Autorisierungs Headers. -Erfordert einen Browser Client. -Anmelde Informationen werden als Klartext gesendet. -Anfällig für Website übergreifende Anforderungs Fälschung (CSRF); erfordert Anti-CSRF-Measures. -Die Verwendung von nicht-Browser-Clients ist schwierig. Der Anmelde Name erfordert einen Browser. -Benutzer Anmelde Informationen werden in der Anforderung gesendet. : Einige Benutzer deaktivieren Cookies. |

Die Formular Authentifizierung in ASP.NET funktioniert wie folgt:

1. Der Client fordert eine Ressource an, für die eine Authentifizierung erforderlich ist.
2. Wenn der Benutzer nicht authentifiziert ist, gibt der Server HTTP 302 (gefunden) zurück und leitet ihn an eine Anmeldeseite um.
3. Der Benutzer gibt die Anmelde Informationen ein und sendet das Formular.
4. Der Server gibt einen weiteren HTTP 302-Wert zurück, der zurück an den ursprünglichen URI umgeleitet wird. Diese Antwort enthält ein Authentifizierungs Cookie.
5. Der Client fordert die Ressource erneut an. Die Anforderung enthält das Authentifizierungs Cookie, sodass der Server die Anforderung erteilt.

![](forms-authentication/_static/image1.png)

Weitere Informationen finden Sie unter [Übersicht über die Formular Authentifizierung.](../../../web-forms/overview/older-versions-security/introduction/an-overview-of-forms-authentication-cs.md)

## <a name="using-forms-authentication-with-web-api"></a>Verwenden der Formular Authentifizierung mit der Web-API

Um eine Anwendung zu erstellen, die die Formular Authentifizierung verwendet, wählen Sie die Vorlage "Internet Anwendung" im MVC 4-Projekt-Assistenten aus. Mit dieser Vorlage werden MVC-Controller für die Kontoverwaltung erstellt. Sie können auch die Vorlage "Single-Page Application" verwenden, die in der ASP.net Fall 2012-Aktualisierung verfügbar ist.

In Ihren Web-API-Controllern können Sie den Zugriff mithilfe des `[Authorize]`-Attributs einschränken, wie unter [Verwenden des [autorisieren]-Attributs](authentication-and-authorization-in-aspnet-web-api.md#auth3)beschrieben.

Bei der Formular Authentifizierung wird ein Sitzungs Cookie verwendet, um Anforderungen zu authentifizieren. Browser senden automatisch alle relevanten Cookies an die Zielwebsite. Durch diese Funktion wird die Formular Authentifizierung potenziell anfällig für Angriffe gegen Website übergreifende Anforderungs Fälschungen (CSRF). Weitere Informationen finden Sie unter [verhindern von Cross-Site Request Fälschungs Angriffen (CSRF)](preventing-cross-site-request-forgery-csrf-attacks.md).

Bei der Formular Authentifizierung werden die Anmelde Informationen des Benutzers nicht verschlüsselt. Daher ist die Formular Authentifizierung nicht sicher, es sei denn, Sie wird mit SSL verwendet. Siehe [Arbeiten mit SSL in der Web-API](working-with-ssl-in-web-api.md).
