---
uid: web-api/overview/security/integrated-windows-authentication
title: Integrierte Windows-Authentifizierung | Microsoft-Dokumentation
author: MikeWasson
description: Beschreibt die Verwendung der integrierten Windows-Authentifizierung in ASP.net-Web-API.
ms.author: riande
ms.date: 12/18/2012
ms.assetid: 71ee4c78-c500-4d1c-b761-b4e161a291b5
msc.legacyurl: /web-api/overview/security/integrated-windows-authentication
msc.type: authoredcontent
ms.openlocfilehash: e4f31f191f3c0fabff308ea5dadb0f1d9ce7d448
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78504207"
---
# <a name="integrated-windows-authentication"></a>Integrierte Windows-Authentifizierung

von [Mike Wasson](https://github.com/MikeWasson)

Die integrierte Windows-Authentifizierung ermöglicht es Benutzern, sich mit Ihren Windows-Anmelde Informationen mithilfe von Kerberos oder NTLM anzumelden. Der Client sendet Anmelde Informationen im Autorisierungs Header. Die Windows-Authentifizierung eignet sich am besten für eine Intranetumgebung. Weitere Informationen finden Sie unter [Windows-Authentifizierung](https://www.iis.net/configreference/system.webserver/security/authentication/windowsauthentication).

| Vorteile | Nachteile |
| --- | --- |
| : In IIS integriert. : Sendet die Anmelde Informationen des Benutzers nicht in der Anforderung. -Wenn der Client Computer zur Domäne gehört (z. b. Intranetanwendung), muss der Benutzer keine Anmelde Informationen eingeben. | -Wird für Internet Anwendungen nicht empfohlen. -Erfordert die Kerberos-oder NTLM-Unterstützung auf dem Client. -Client muss in der Active Directory Domäne sein. |

> [!NOTE]
> Wenn Ihre Anwendung in Azure gehostet wird und Sie über eine lokale Active Directory Domäne verfügen, sollten Sie den Verbund Ihres lokalen AD mit Azure Active Directory in Erwägung gezogen. Auf diese Weise können sich Benutzer mit Ihren lokalen Anmelde Informationen anmelden, die Authentifizierung erfolgt jedoch Azure AD. Weitere Informationen finden Sie unter [Azure-Authentifizierung](../../../visual-studio/overview/2012/windows-azure-authentication.md).

Um eine Anwendung zu erstellen, die die integrierte Windows-Authentifizierung verwendet, wählen Sie die Vorlage "Intranetanwendung" im MVC 4-Projekt-Assistenten aus. Diese Projektvorlage legt die folgende Einstellung in der Datei "Web. config" ab:

[!code-xml[Main](integrated-windows-authentication/samples/sample1.xml)]

Auf der Clientseite funktioniert die integrierte Windows-Authentifizierung mit jedem Browser, der das [Aushandlungs](http://www.ietf.org/rfc/rfc4559.txt) Authentifizierungsschema unterstützt, das die meisten wichtigen Browser enthält. Für .NET-Client Anwendungen unterstützt die **HttpClient** -Klasse die Windows-Authentifizierung:

[!code-csharp[Main](integrated-windows-authentication/samples/sample2.cs)]

Die Windows-Authentifizierung ist anfällig für Angriffe durch Website übergreifende Anforderungs Fälschung (CSRF). Weitere Informationen finden Sie unter [verhindern von Angriffen für Website übergreifende Anforderungs Fälschung (CSRF)](preventing-cross-site-request-forgery-csrf-attacks.md).
