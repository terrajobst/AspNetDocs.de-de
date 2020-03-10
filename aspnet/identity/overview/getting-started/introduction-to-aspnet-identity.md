---
uid: identity/overview/getting-started/introduction-to-aspnet-identity
title: Einführung in ASP.net Identity-ASP.NET 4. x
author: jongalloway
description: Das ASP.NET-Mitgliedschaftssystem wurde mit ASP.NET 2,0 zurück in 2005 eingeführt, und seitdem gab es viele Änderungen an den Verwendungsmöglichkeiten für Webanwendungen...
ms.author: riande
ms.date: 01/22/2019
ms.assetid: 38717fc1-5989-43cf-952d-4007cc1dd923
ms.custom: seoapril2019
msc.legacyurl: /identity/overview/getting-started/introduction-to-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: 0268dfc16cd2cfb1e79ee14997a4c5eb247af950
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78471735"
---
# <a name="introduction-to-aspnet-identity"></a>Einführung in ASP.NET Identity

> Das ASP.NET-Mitgliedschaftssystem wurde mit ASP.NET 2,0 zurück in 2005 eingeführt, und seitdem gab es viele Änderungen an der Art und Weise, wie Webanwendungen die Authentifizierung und Autorisierung in der Regel verarbeiten. ASP.net Identity ist ein neuer Einblick in das Mitgliedschaftssystem, wenn Sie moderne Anwendungen für das Web, das Telefon oder das Tablet entwickeln.

## <a name="background-membership-in-aspnet"></a>Hintergrund: Mitgliedschaft in ASP.net

### <a name="aspnet-membership"></a>ASP.NET-Mitgliedschaft

Die [ASP.NET-Mitgliedschaft](https://msdn.microsoft.com/library/yh26yfzy(v=VS.100).aspx) wurde entwickelt, um die Anforderungen an die Website Mitgliedschaft zu lösen, die in 2005 üblich waren, die die Formular Authentifizierung umfasste, sowie eine SQL Server Datenbank für Benutzernamen, Kenn Wörter und Profildaten. Heute gibt es eine weitaus größere Anzahl von Datenspeicher Optionen für Webanwendungen, und die meisten Entwickler möchten ihren Standorten die Verwendung sozialer Identitäts Anbieter für Authentifizierungs-und Autorisierungs Funktionen ermöglichen. Diese Umstellung wird durch die Einschränkungen des Entwurfs der ASP.NET-Mitgliedschaft erschwert:

- Das Datenbankschema wurde für SQL Server entwickelt und kann nicht geändert werden. Sie können Profilinformationen hinzufügen, die zusätzlichen Daten werden jedoch in einer anderen Tabelle verpackt, wodurch der Zugriff auf alle Mittel, außer über die Profil Anbieter-API, erschwert wird.
- Das Anbieter System ermöglicht es Ihnen, den Sicherungsdaten Speicher zu ändern, aber das System ist auf Annahmen zugeschnitten, die für eine relationale Datenbank geeignet sind. Sie können einen Anbieter schreiben, um Mitgliedschafts Informationen in einem nicht relationalen Speichermechanismus (z. b. Azure Storage Tabellen) zu speichern, aber dann müssen Sie den relationalen Entwurf umgehen, indem Sie viel Code und viele `System.NotImplementedException` Ausnahmen für Methoden schreiben, die nicht für nosql-Datenbanken gelten.
- Da die Anmelde-/Abmelde-Funktionalität auf der Formular Authentifizierung basiert, kann [owin](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md)vom Mitgliedschaftssystem nicht verwendet werden. Owin enthält Middleware-Komponenten für die Authentifizierung, einschließlich der Unterstützung für Anmeldungen mit externen Identitäts Anbietern (wie Microsoft-Konten, Facebook, Google, Twitter) und Anmeldungen mithilfe von Organisations Konten aus lokalem Active Directory oder Azure Active Directory. Owin bietet auch Unterstützung für OAuth 2,0, JWT und cors.

### <a name="aspnet-simple-membership"></a>ASP.net einfache Mitgliedschaft

[ASP.net die einfache Mitgliedschaft](../../../web-pages/overview/security/16-adding-security-and-membership.md) wurde als Mitgliedschaftssystem für ASP.net Web Pages entwickelt. Es wurde mit webmatrix und Visual Studio 2010 SP1 veröffentlicht. Das Ziel der einfachen Mitgliedschaft war, das Hinzufügen von Mitgliedschafts Funktionen zu einer Web Pages-Anwendung zu erleichtern.

Die einfache Mitgliedschaft vereinfacht die Anpassung von Benutzerprofil Informationen, aber Sie gibt weiterhin die anderen Probleme mit der ASP.NET-Mitgliedschaft aus und weist einige Einschränkungen auf:

- Es war schwierig, Mitgliedschaftssystem Daten in einem nicht relationalen Speicher beizubehalten.
- Die Verwendung mit owin ist nicht möglich.
- Es funktioniert nicht gut für vorhandene ASP.net-Mitgliedschafts Anbieter und ist nicht erweiterbar.

### <a name="aspnet-universal-providers"></a>ASP.NET-Universelle Anbieter

[ASP.net-universelle Anbieter](http://www.hanselman.com/blog/IntroducingSystemWebProvidersASPNETUniversalProvidersForSessionMembershipRolesAndUserProfileOnSQLCompactAndSQLAzure.aspx) wurden entwickelt, um das Beibehalten von Mitgliedschafts Informationen in Microsoft Azure SQL-Datenbank zu ermöglichen, und Sie funktionieren auch mit SQL Server Compact. Die universelle Anbieter wurden auf Entity Framework Code First erstellt, was bedeutet, dass das universelle Anbieter verwendet werden kann, um Daten in jedem von EF unterstützten Speicher beizubehalten. Mit dem universelle Anbieter wurde das Datenbankschema ebenfalls sehr viel bereinigt.

Die universelle Anbieter basieren auf der ASP.net-Mitgliedschafts Infrastruktur, sodass Sie weiterhin dieselben Einschränkungen wie der sqlmembership-Anbieter aufweisen. Das heißt, Sie wurden für relationale Datenbanken entwickelt, und es ist schwierig, Profil-und Benutzerinformationen anzupassen. Diese Anbieter verwenden auch weiterhin die Formular Authentifizierung für die Anmelde-und Abmelde Funktionalität.

## <a name="aspnet-identity"></a>ASP.NET Identity

Da sich die Mitgliedschafts Story in ASP.net im Laufe der Jahre entwickelt hat, hat das ASP.net-Team viel von Feedback von Kunden gelernt.

Die Annahme, dass sich Benutzer anmelden, indem Sie einen Benutzernamen und ein Kennwort eingeben, die Sie in ihrer eigenen Anwendung registriert haben, sind nicht mehr gültig. Das Web ist sozialer geworden. Benutzer interagieren in Echtzeit über soziale Netzwerke wie Facebook, Twitter und andere soziale Websites. Entwickler möchten, dass sich Benutzer mit ihren Identitäten für soziale Netzwerke anmelden können, damit Sie auf ihren Websites eine umfassende Benutzer Darstellung haben können. Ein modernes Mitgliedschaftssystem muss Umleitungs basierte Anmeldungen an Authentifizierungs Anbieter wie Facebook, Twitter und andere ermöglichen.

Bei der Entwicklung der Webentwicklung hat sich die Webentwicklung verändert. Komponententests von Anwendungscode wurden Hauptanliegen für Anwendungsentwickler. In 2008 ASP.net wurde ein neues Framework basierend auf dem Model-View-Controller (MVC)-Muster hinzugefügt, um Entwickler dabei zu unterstützen, Komponenten testbare ASP.NET-Anwendungen zu erstellen. Entwickler, die eine Komponenten Test Ihrer Anwendungslogik durchführen wollten, wollten dies auch mit dem Mitgliedschaftssystem durchführen können.

In Anbetracht dieser Änderungen bei der Entwicklung von Webanwendungen wurde ASP.net Identity mit den folgenden Zielen entwickelt:

- **Ein ASP.net Identity System**

    - ASP.net Identity können mit allen ASP.NET-Frameworks wie z. b. ASP.NET MVC, Web Forms, Webseiten, Web-API und signalr verwendet werden.
    - ASP.net Identity können bei der Erstellung von Web-, Phone-, Store-oder Hybrid Anwendungen verwendet werden.
- **Einfache Plug von Profildaten über den Benutzer**

    - Sie haben die Kontrolle über das Schema von Benutzer-und Profilinformationen. Beispielsweise können Sie das System zum Speichern von Geburtsdaten, die von Benutzern eingegeben werden, auf einfache Weise aktivieren, wenn Sie ein Konto in Ihrer Anwendung registrieren.

- **Persistenzsteuerung**

    - Standardmäßig speichert das ASP.net Identity System alle Benutzerinformationen in einer Datenbank. ASP.net Identity verwendet Entity Framework Code First, um den gesamten Persistenzmechanismus zu implementieren.
    - Da Sie das Datenbankschema steuern, ist es einfach, häufige Aufgaben wie das Ändern von Tabellennamen oder das Ändern des Datentyps von primär Schlüsseln auszuführen.
    - Es ist einfach, unterschiedliche Speicher Mechanismen wie SharePoint, Azure Storage Tabellen Dienst, nosql-Datenbanken usw. einzuschließen, ohne `System.NotImplementedExceptions` Ausnahmen auslösen zu müssen.
- **Einheiten Testability**

    - ASP.net Identity bewirkt, dass die Webanwendung mehr Einheiten testfähig ist. Sie können Komponententests für die Teile Ihrer Anwendung schreiben, die ASP.net Identity verwenden.
- **Rollen Anbieter**

    - Es gibt einen Rollen Anbieter, mit dem Sie den Zugriff auf Teile Ihrer Anwendung durch Rollen einschränken können. Sie können problemlos Rollen wie "admin" erstellen und Rollen hinzufügen.
- **Anspruchs basiert**

    - ASP.net Identity unterstützt die Anspruchs basierte Authentifizierung, bei der die Identität des Benutzers als Satz von Ansprüchen dargestellt wird. Ansprüche ermöglichen es Entwicklern, die Identität eines Benutzers zu beschreiben, als die Rollen zulassen. Während die Rollen Mitgliedschaft nur ein boolescher Wert (Member oder nicht Mitglied) ist, kann ein Anspruch umfassende Informationen über die Identität des Benutzers und die Mitgliedschaft enthalten.
- **Anmelde Anbieter für soziale Netzwerke**

    - Sie können Ihrer Anwendung problemlos soziale Anmeldungen wie Microsoft-Konto, Facebook, Twitter, Google und andere hinzufügen und die benutzerspezifischen Daten in der Anwendung speichern.

- **Owin-Integration**

    - Die ASP.NET-Authentifizierung basiert jetzt auf der owin-Middleware, die auf einem beliebigen owin-basierten Host verwendet werden kann. ASP.net Identity hat keine Abhängigkeit von System. Web. Es handelt sich um ein vollständig kompatibles owin-Framework, das in jeder von owin gehosteten Anwendung verwendet werden kann.
    - ASP.net Identity verwendet owin-Authentifizierung für die Anmeldung und die Abmeldung von Benutzern auf der Website. Dies bedeutet, dass die Anwendung zum Generieren des Cookies anstelle von FormsAuthentication die owin-Cookieauthentifizierung verwendet.
- **NuGet-Paket**

    - ASP.net Identity wird als ein nuget-Paket verteilt, das in den ASP.NET MVC-, Web Forms-und Web-API-Vorlagen, die mit Visual Studio 2017 ausgeliefert werden, installiert wird. Sie können dieses nuget-Paket aus dem nuget-Katalog herunterladen.
    - Das Freigeben von ASP.net Identity als nuget-Paket vereinfacht das ASP.net-Team, neue Features und Fehlerbehebungen zu durchlaufen und diese auf agile Weise für Entwickler bereitzustellen.

## <a name="get-started-with-aspnet-identity"></a>Beginnen Sie mit ASP.net Identity

ASP.net Identity wird in den Visual Studio 2017-Projektvorlagen für ASP.NET MVC, Web Forms, Web-API und Spa verwendet. In dieser exemplarischen Vorgehensweise wird veranschaulicht, wie die Projektvorlagen ASP.net Identity verwenden, um Funktionen zum Registrieren, anmelden und Abmelden eines Benutzers hinzuzufügen.

ASP.net Identity wird mithilfe des folgenden Verfahrens implementiert. Der Zweck dieses Artikels besteht darin, Ihnen einen allgemeinen Überblick über ASP.net Identity zu verschaffen. Sie können Sie Schritt für Schritt befolgen oder einfach die Details lesen. Ausführlichere Anweisungen zum Erstellen von apps mit ASP.net Identity, einschließlich der Verwendung der neuen API zum Hinzufügen von Benutzern, Rollen und Profilinformationen, finden Sie im Abschnitt nächste Schritte am Ende dieses Artikels.

1. Erstellen Sie eine ASP.NET MVC-Anwendung mit einzelnen Konten. Sie können ASP.net Identity in ASP.NET MVC, Web Forms, Web API, signalr usw. verwenden. In diesem Artikel beginnen wir mit einer ASP.NET MVC-Anwendung.  
  
    ![](introduction-to-aspnet-identity/_static/image1.png)
2. Das erstellte Projekt enthält die folgenden drei Pakete für ASP.net Identity.

    - [`Microsoft.AspNet.Identity.EntityFramework`](http://www.nuget.org/packages/Microsoft.AspNet.Identity.EntityFramework/)  
   Dieses Paket verfügt über die Entity Framework Implementierung von ASP.net Identity bei der die ASP.net Identity Daten und das Schema für SQL Server persistent gespeichert werden.
    - [`Microsoft.AspNet.Identity.Core`](http://www.nuget.org/packages/Microsoft.AspNet.Identity.Core/)  
   Dieses Paket verfügt über die Kern Schnittstellen für ASP.net Identity. Dieses Paket kann verwendet werden, um eine Implementierung für ASP.net Identity zu schreiben, die unterschiedliche Beibehaltungs Speicher wie Azure Table Storage, nosql-Datenbanken usw. als Ziel hat.
    - [`Microsoft.AspNet.Identity.OWIN`](http://www.nuget.org/packages/Microsoft.AspNet.Identity.Owin/)  
   Dieses Paket enthält Funktionen, die zum Einbinden der owin-Authentifizierung mit ASP.net Identity in ASP.NET-Anwendungen verwendet werden. Diese wird verwendet, wenn Sie Ihrer Anwendung Anmelde Funktionen hinzufügen und die owin-cookenauthentifizierungs-Middleware aufzurufen, um ein Cookie zu generieren.
3. Erstellen eines Benutzers.  
   Starten Sie die Anwendung, und klicken Sie dann auf den Link **registrieren** , um einen Benutzer zu erstellen. In der folgenden Abbildung wird die Register Seite angezeigt, auf der der Benutzername und das Kennwort erfasst werden.  
  
    ![](introduction-to-aspnet-identity/_static/image2.png)  
  
   Wenn der Benutzer die Schaltfläche **registrieren** auswählt, erstellt die `Register` Aktion des Konto Controllers den Benutzer durch Aufrufen der ASP.net Identity-API, wie unten gezeigt:

    [!code-csharp[Main](introduction-to-aspnet-identity/samples/sample1.cs?highlight=8-9)]
4. Melden Sie sich an.  
   Wenn der Benutzer erfolgreich erstellt wurde, wird er durch die `SignInAsync`-Methode angemeldet.  

    [!code-csharp[Main](introduction-to-aspnet-identity/samples/sample6.cs?highlight=12)]

   Die `SignInManager.SignInAsync`-Methode generiert eine " [ClaimsIdentity](https://msdn.microsoft.com/library/system.security.claims.claimsidentity.aspx)". Da ASP.net Identity-und owin-Cookieauthentifizierung Anspruchs basiertes System sind, erfordert das Framework, dass die APP eine "ClaimsIdentity" für den Benutzer generiert. ClaimsIdentity enthält Informationen über alle Ansprüche für den Benutzer, z. b. die Rollen, denen der Benutzer angehört.   
 
5. Melden Sie sich ab.  
   Wählen Sie den Link Abmelden aus, um die **Abmelde** Aktion im Konto Controller aufzurufen. 

    [!code-csharp[Main](introduction-to-aspnet-identity/samples/sample5.cs?highlight=6)]

   Der hervorgehobene Code oben zeigt die owin-`AuthenticationManager.SignOut`-Methode. Dies entspricht der [FormsAuthentication. SignOut](https://msdn.microsoft.com/library/system.web.security.formsauthentication.signout.aspx) -Methode, die vom [FormsAuthentication](https://msdn.microsoft.com/library/system.web.security.formsauthenticationmodule.aspx) -Modul in Web Forms verwendet wird.

## <a name="components-of-aspnet-identity"></a>Komponenten von ASP.net Identity

Das folgende Diagramm zeigt die Komponenten des ASP.net Identity Systems (Wählen Sie [dieses](introduction-to-aspnet-identity/_static/image3.png) oder im Diagramm aus, um es zu vergrößern). Die Pakete in grün bilden das ASP.net Identity System. Alle anderen Pakete sind Abhängigkeiten, die für die Verwendung des ASP.net Identity Systems in ASP.NET-Anwendungen erforderlich sind.

[![](introduction-to-aspnet-identity/_static/image5.png)](introduction-to-aspnet-identity/_static/image4.png)

Im folgenden finden Sie eine kurze Beschreibung der nuget-Pakete, die nicht bereits erwähnt wurden:

- [Microsoft.Owin.Security.Cookies](http://www.nuget.org/packages/Microsoft.Owin.Security.Cookies/)  
 Middleware, die einer Anwendung die Verwendung der cookiebasierten Authentifizierung ermöglicht, ähnlich wie bei ASP. Formular Authentifizierung von net.
- [EntityFramework](http://www.nuget.org/packages/EntityFramework/)  
 Entity Framework ist die empfohlene Datenzugriffs Technologie von Microsoft für relationale Datenbanken.

## <a name="migrating-from-membership-to-aspnet-identity"></a>Migrieren von der Mitgliedschaft zu ASP.net Identity

Wir hoffen, bald eine Anleitung zum Migrieren vorhandener apps bereitzustellen, die ASP.NET-Mitgliedschaft oder einfache Mitgliedschaft beim neuen ASP.net Identity System verwenden.

## <a name="next-steps"></a>Nächste Schritte

- [Erstellen einer ASP.NET MVC 5-App mit Facebook und Google OAuth2 und OpenID-Anmeldung](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md)  
 Das Tutorial verwendet die ASP.net Identity-API, um der Benutzerdatenbank Profilinformationen hinzuzufügen und zu erfahren, wie Sie sich mit Google und Facebook authentifizieren.
- [Erstellen einer ASP.NET MVC-App mit Authentifizierung, SQL-Datenbank und Bereitstellung in Azure App Service](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data)  
 In diesem Tutorial wird gezeigt, wie Sie die Identitäts-API verwenden, um Benutzer und Rollen hinzuzufügen.
- [https://github.com/rustd/AspnetIdentitySample](https://github.com/rustd/AspnetIdentitySample)  
 Beispielanwendung, die zeigt, wie Sie grundlegende Rollen und Benutzerunterstützung hinzufügen und wie Sie Rollen und Benutzerverwaltung durchführen.
