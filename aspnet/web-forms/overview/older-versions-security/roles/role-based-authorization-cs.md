---
uid: web-forms/overview/older-versions-security/roles/role-based-authorization-cs
title: Rollenbasierte Autorisierung (C#) | Microsoft-Dokumentation
author: rick-anderson
description: In diesem Tutorial wird erläutert, wie das Rollen-Framework den Rollen eines Benutzers seinen Sicherheitskontext zuordnet. Anschließend wird das Anwenden der rollenbasierten URL untersucht...
ms.author: riande
ms.date: 03/24/2008
ms.assetid: 4d9b63fa-c3d4-4e85-82b1-26ae3ba3ca1c
msc.legacyurl: /web-forms/overview/older-versions-security/roles/role-based-authorization-cs
msc.type: authoredcontent
ms.openlocfilehash: 46153ab310bdee814baaa53c372fb92f8a23ce11
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78509337"
---
# <a name="role-based-authorization-c"></a>Rollenbasierte Autorisierung (C#)

von [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Code herunterladen](https://download.microsoft.com/download/6/0/3/6032582f-360d-4739-b935-38721fdb86ea/CS.11.zip) oder [PDF herunterladen](https://download.microsoft.com/download/6/0/3/6032582f-360d-4739-b935-38721fdb86ea/aspnet_tutorial11_RoleAuth_cs.pdf)

> In diesem Tutorial wird erläutert, wie das Rollen-Framework den Rollen eines Benutzers seinen Sicherheitskontext zuordnet. Anschließend wird untersucht, wie rollenbasierte URL-Autorisierungs Regeln angewendet werden. Danach betrachten wir die deklarative und programmgesteuerte Methode zum Ändern der angezeigten Daten und der Funktionen, die von einer ASP.NET-Seite geboten werden.

## <a name="introduction"></a>Einführung

Im Tutorial <a id="_msoanchor_1"> </a>zur [*benutzerbasierten Autorisierung*](../membership/user-based-authorization-cs.md) haben Sie erfahren, wie Sie die URL-Autorisierung verwenden, um anzugeben, welche Benutzer eine bestimmte Gruppe von Seiten besuchen können. Mit nur einem wenig Markup in `Web.config`können wir ASP.NET anweisen, dass nur authentifizierte Benutzer eine Seite besuchen können. Oder wir legen fest, dass nur Benutzer Tito und Bob zulässig waren, oder geben an, dass alle authentifizierten Benutzer mit Ausnahme von Sam zulässig sind.

Zusätzlich zur URL-Autorisierung haben wir auch deklarative und programmgesteuerte Techniken zum Steuern der angezeigten Daten und der von einer Seite auf der Grundlage des Benutzers angebotenen Funktionen untersucht. Insbesondere haben wir eine Seite erstellt, die den Inhalt des aktuellen Verzeichnisses aufgelistet hat. Jeder kann diese Seite besuchen, aber nur authentifizierte Benutzer können die Inhalte der Dateien anzeigen, und nur die Dateien können von Tito gelöscht werden.

Das Anwenden von Autorisierungs Regeln auf Benutzerbasis kann zu einem Buchhaltungs-Alptraum werden. Eine besser wart bare Methode ist die Verwendung der rollenbasierten Autorisierung. Die gute Nachricht ist, dass die zur Verfügung stehenden Tools zum Anwenden von Autorisierungs Regeln gleichermaßen gut mit Rollen funktionieren, die für Benutzerkonten gelten. Mit URL-Autorisierungs Regeln können anstelle von Benutzern Rollen angegeben werden. Das LoginView-Steuerelement, das eine andere Ausgabe für authentifizierte und anonyme Benutzer rendert, kann so konfiguriert werden, dass unterschiedliche Inhalte auf der Grundlage der Rollen des angemeldeten Benutzers angezeigt werden. Und die Rollen-API beinhaltet Methoden zum Bestimmen der Rollen des angemeldeten Benutzers.

In diesem Tutorial wird erläutert, wie das Rollen-Framework den Rollen eines Benutzers seinen Sicherheitskontext zuordnet. Anschließend wird untersucht, wie rollenbasierte URL-Autorisierungs Regeln angewendet werden. Danach betrachten wir die deklarative und programmgesteuerte Methode zum Ändern der angezeigten Daten und der Funktionen, die von einer ASP.NET-Seite geboten werden. Erste Schritte

## <a name="understanding-how-roles-are-associated-with-a-users-security-context"></a>Grundlegendes zur Zuordnung von Rollen zum Sicherheitskontext eines Benutzers

Wenn eine Anforderung in die ASP.NET-Pipeline eintritt, wird Sie einem Sicherheitskontext zugeordnet, der Informationen enthält, die den Anforderer identifizieren. Wenn Sie die Formular Authentifizierung verwenden, wird ein Authentifizierungs Ticket als Identitäts Token verwendet. Wie in der <a id="_msoanchor_2"> </a>Übersicht über die Konfiguration der [*Formular Authentifizierung*](../introduction/an-overview-of-forms-authentication-cs.md) und <a id="_msoanchor_3"> </a>die Konfiguration der [*Formular Authentifizierung und*](../introduction/forms-authentication-configuration-and-advanced-topics-cs.md) die Lernprogramme für erweiterte Themen erläutert, ist der `FormsAuthenticationModule` für die Bestimmung der Identität des Anforderer zuständig, die er während des [`AuthenticateRequest` Ereignisses](https://msdn.microsoft.com/library/system.web.httpapplication.authenticaterequest.aspx)ausführt.

Wenn ein gültiges, nicht abgelaufenes Authentifizierungs Ticket gefunden wird, decodiert das `FormsAuthenticationModule` es, um die Identität des Anforderers zu ermitteln. Es wird ein neues `GenericPrincipal`-Objekt erstellt und dem `HttpContext.User`-Objekt zugewiesen. Der Zweck eines Prinzipals, wie `GenericPrincipal`, besteht darin, den Namen des authentifizierten Benutzers und die Rollen zu identifizieren, zu denen er gehört. Dieser Zweck wird durch die Tatsache ersichtlich, dass alle Prinzipal Objekte über eine `Identity`-Eigenschaft und eine `IsInRole(roleName)`-Methode verfügen. Der `FormsAuthenticationModule`ist jedoch nicht an der Aufzeichnung von Rollen Informationen interessiert, und das `GenericPrincipal` Objekt, das es erstellt, gibt keine Rollen an.

Wenn das Rollen Framework aktiviert ist, führt das [`RoleManagerModule`](https://msdn.microsoft.com/library/system.web.security.rolemanagermodule.aspx) HTTP-Modul nach der `FormsAuthenticationModule` Schritte aus und identifiziert die Rollen des authentifizierten Benutzers während des [`PostAuthenticateRequest` Ereignisses](https://msdn.microsoft.com/library/system.web.httpapplication.postauthenticaterequest.aspx), das nach dem `AuthenticateRequest` Ereignis ausgelöst wird. Wenn die Anforderung von einem authentifizierten Benutzer erfolgt, überschreibt der `RoleManagerModule` das vom `FormsAuthenticationModule` erstellte `GenericPrincipal` Objekt und ersetzt es durch ein [`RolePrincipal` Objekt](https://msdn.microsoft.com/library/system.web.security.roleprincipal.aspx). Die `RolePrincipal`-Klasse verwendet die Rollen-API, um zu bestimmen, welcher Rolle der Benutzer angehört.

Abbildung 1 zeigt den ASP.NET-Pipeline-Workflow bei Verwendung der Formular Authentifizierung und des Rollen-Frameworks. Der `FormsAuthenticationModule` wird zuerst ausgeführt, identifiziert den Benutzer über das Authentifizierungs Ticket und erstellt ein neues `GenericPrincipal`-Objekt. Im nächsten Schritt `RoleManagerModule` wird das `GenericPrincipal` Objekt mit einem `RolePrincipal`-Objekt überschrieben.

Wenn ein anonymer Benutzer die Website besucht, erstellt weder der `FormsAuthenticationModule` noch der `RoleManagerModule` ein Prinzipal Objekt.

[![der ASP.NET-Pipeline Ereignisse für einen authentifizierten Benutzer bei Verwendung der Formular Authentifizierung und des Rollen-Frameworks](role-based-authorization-cs/_static/image2.png)](role-based-authorization-cs/_static/image1.png)

**Abbildung 1**: die ASP.NET-Pipeline Ereignisse für einen authentifizierten Benutzer bei Verwendung der Formular Authentifizierung und des Rollen-Frameworks ([Klicken Sie, um das Bild in voller Größe anzuzeigen](role-based-authorization-cs/_static/image3.png))

### <a name="caching-role-information-in-a-cookie"></a>Zwischenspeichern von Rollen Informationen in einem Cookie

Mit der `IsInRole(roleName)`-Methode des `RolePrincipal` Objekts werden `Roles.GetRolesForUser` aufgerufen, um die Rollen für den Benutzer zu ermitteln, um zu bestimmen, ob der Benutzer ein Mitglied von *roleName*ist. Wenn Sie die `SqlRoleProvider`verwenden, führt dies zu einer Abfrage der Rollen Speicher-Datenbank. Bei Verwendung Rollen basierter URL-Autorisierungs Regeln wird die `IsInRole`-Methode der `RolePrincipal`für jede Anforderung an eine Seite aufgerufen, die durch die rollenbasierten URL-Autorisierungs Regeln geschützt wird. Anstatt die Rollen Informationen in jeder Anforderung in der Datenbank nachschlagen zu müssen, umfasst das Rollen Framework eine Option zum Zwischenspeichern der Rollen des Benutzers in einem Cookie.

Wenn das Rollen Framework so konfiguriert ist, dass die Rollen des Benutzers in einem Cookie zwischengespeichert werden, erstellt das `RoleManagerModule` das Cookie während des [`EndRequest` Ereignisses](https://msdn.microsoft.com/library/system.web.httpapplication.endrequest.aspx)der ASP.NET-Pipeline. Dieses Cookie wird in nachfolgenden Anforderungen in der `PostAuthenticateRequest`verwendet, bei der das `RolePrincipal` Objekt erstellt wird. Wenn das Cookie gültig ist und noch nicht abgelaufen ist, werden die Daten im Cookie analysiert und zum Auffüllen der Rollen des Benutzers verwendet. Dadurch wird die `RolePrincipal` nicht mehr `Roles` aufgerufen, um die Rollen des Benutzers zu bestimmen. Abbildung 2 zeigt diesen Workflow.

[![die Rollen Informationen des Benutzers in einem Cookie gespeichert werden, um die Leistung zu verbessern](role-based-authorization-cs/_static/image5.png)](role-based-authorization-cs/_static/image4.png)

**Abbildung 2**: die Rollen Informationen des Benutzers können in einem Cookie gespeichert werden, um die Leistung zu verbessern ([Klicken Sie, um das Bild in voller Größe anzuzeigen](role-based-authorization-cs/_static/image6.png))

Standardmäßig ist der Cookie-Mechanismus für den Rollen Cache deaktiviert. Sie kann über das `<roleManager>`-Konfigurations Markup in `Web.config`aktiviert werden. Wir haben die Verwendung des [`<roleManager>`-Elements](https://msdn.microsoft.com/library/ms164660.aspx) zum Angeben von Rollen <a id="_msoanchor_4"> </a>Anbietern im Tutorial zum [*Erstellen und Verwalten von Rollen*](creating-and-managing-roles-cs.md) erläutert, sodass Sie dieses Element bereits in der `Web.config` Datei Ihrer Anwendung haben. Die Einstellungen des Rollen Cache-Cookies werden als Attribute des `<roleManager>`-Elements angegeben und in Tabelle 1 zusammengefasst.

> [!NOTE]
> Die in Tabelle 1 aufgeführten Konfigurationseinstellungen geben die Eigenschaften des resultierenden Rollen Cache Cookies an. Weitere Informationen zu Cookies, ihrer Funktionsweise und ihren verschiedenen Eigenschaften finden Sie in [diesem Cookie-Tutorial](http://www.quirksmode.org/js/cookies.html).

| <strong>Eigenschaft</strong> |                                                                                                                                                                                                                                                                                                                                                         <strong>Beschreibung</strong>                                                                                                                                                                                                                                                                                                                                                          |
|---------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   `cacheRolesInCookie`    |                                                                                                                                                                                                                                                                                                                              Ein boolescher Wert, der angibt, ob das Zwischenspeichern von Cookies verwendet wird. Wird standardmäßig auf `false` festgelegt.                                                                                                                                                                                                                                                                                                                              |
|       `cookieName`        |                                                                                                                                                                                                                                                                                                                                     Der Name des Rollen Cache Cookies. Der Standardwert ist ". Aspxrollen ".                                                                                                                                                                                                                                                                                                                                     |
|       `cookiePath`        |                                                                                                                                                                                                                                Der Pfad des Rollennamen Cookies. Mithilfe des Path-Attributs kann ein Entwickler den Gültigkeitsbereich eines Cookies auf eine bestimmte Verzeichnishierarchie beschränken. Der Standardwert ist "/", der dem Browser mitteilt, das Authentifizierungs Ticket Cookie an jede an die Domäne gesendete Anforderung zu senden.                                                                                                                                                                                                                                 |
|    `cookieProtection`     |                                                                                                                                                               Gibt an, welche Techniken zum Schützen des Rollen Cache Cookies verwendet werden. Zulässige Werte: `All` (Standard); `Encryption`; `None`; und `Validation`. Weitere Informationen zu diesen Schutz Ebenen finden <a id="_anchor_5"> </a>Sie in Schritt 3 des Tutorials zur [*Konfiguration der Formular Authentifizierung und zu erweiterten Themen*](../introduction/forms-authentication-configuration-and-advanced-topics-cs.md) .                                                                                                                                                                |
|    `cookieRequireSSL`     |                                                                                                                                                                                                                                                                                                   Ein boolescher Wert, der angibt, ob eine SSL-Verbindung erforderlich ist, um das Authentifizierungs Cookie zu übermitteln. Standardwert: `false`.                                                                                                                                                                                                                                                                                                   |
| `cookieSlidingExpiration` |                                                                                                                                                                                                                                                  Ein boolescher Wert, der angibt, ob das Cookie-Timeout jedes Mal zurückgesetzt wird, wenn der Benutzer die Website während einer einzelnen Sitzung besucht. Standardwert: `false`. Dieser Wert ist nur relevant, wenn `createPersistentCookie` auf `true`festgelegt ist.                                                                                                                                                                                                                                                  |
|      `cookieTimeout`      |                                                                                                                                                                                                                                                                         Gibt die Zeit in Minuten an, nach der das Authentifizierungs Ticket Cookie abläuft. Standardwert: `30`. Dieser Wert ist nur relevant, wenn `createPersistentCookie` auf `true`festgelegt ist.                                                                                                                                                                                                                                                                         |
| `createPersistentCookie`  |                                                                                                                                                                   Ein boolescher Wert, der angibt, ob das Rollen Cache Cookie ein Sitzungs Cookie oder ein dauerhaftes Cookie ist. Wenn `false` (die Standardeinstellung), wird ein Sitzungs Cookie verwendet, das beim Schließen des Browsers gelöscht wird. Wenn `true`, wird ein dauerhaftes Cookie verwendet. Sie läuft `cookieTimeout` Anzahl von Minuten nach der Erstellung oder nach dem vorherigen Besuch ab, abhängig vom Wert `cookieSlidingExpiration`.                                                                                                                                                                    |
|         `domain`          |                                                                                                                                                 Gibt den Domänen Wert des Cookies an. Der Standardwert ist eine leere Zeichenfolge, die bewirkt, dass der Browser die Domäne verwendet, von der er ausgestellt wurde (z. b. www.yourdomain.com). In diesem Fall wird das Cookie <strong>nicht</strong> gesendet, wenn Anforderungen an Unterdomänen, z. b. admin.yourdomain.com, gesendet werden. Wenn Sie möchten, dass das Cookie an alle Unterdomänen übermittelt wird, müssen Sie das `domain`-Attribut anpassen, indem Sie es auf "yourdomain.com" festlegen.                                                                                                                                                 |
|    `maxCachedResults`     | Gibt die maximale Anzahl von Rollennamen an, die im Cookie zwischengespeichert werden. Der Standard ist 25. Der `RoleManagerModule` erstellt kein Cookie für Benutzer, die zu mehr als `maxCachedResults` Rollen gehören. Folglich verwendet die `IsInRole`-Methode des `RolePrincipal` Objekts die `Roles`-Klasse, um die Rollen des Benutzers zu bestimmen. Der Grund `maxCachedResults` vorhanden ist, weil viele Benutzer-Agents keine Cookies zulassen, die größer als 4.096 Bytes sind. Diese Begrenzung soll also die Wahrscheinlichkeit verringern, dass diese Größenbeschränkung überschritten wird. Wenn Sie über extrem lange Rollennamen verfügen, sollten Sie die Angabe eines kleineren `maxCachedResults` Werts in Erwägung haben. contrarialmäßig können Sie diesen Wert erhöhen, wenn Sie über extrem kurze Rollennamen verfügen. |

**Tabelle 1:** Die Konfigurationsoptionen des Rollen Cache Cookies

Wir konfigurieren unsere Anwendung so, dass Sie nicht persistente Rollen Cache Cookies verwendet. Um dies zu erreichen, aktualisieren Sie das `<roleManager>`-Element in `Web.config`, um die folgenden Cookie-bezogenen Attribute einzuschließen:

[!code-xml[Main](role-based-authorization-cs/samples/sample1.xml)]

Ich habe das `<roleManager>`-Element durch Hinzufügen von drei Attributen aktualisiert: `cacheRolesInCookie`, `createPersistentCookie`und `cookieProtection`. Wenn Sie `cacheRolesInCookie` auf `true`festlegen, werden die Rollen des Benutzers automatisch in einem Cookie zwischengespeichert `RoleManagerModule`, anstatt die Rollen Informationen des Benutzers für jede Anforderung zu suchen. Die Attribute "`createPersistentCookie`" und "`cookieProtection`" werden explizit auf "`false`" und "`All`" festgelegt. Technisch gesehen musste ich keine Werte für diese Attribute angeben, da ich Sie lediglich ihren Standardwerten zugewiesen habe, aber ich lege sie hier, um explizit klar zu machen, dass ich keine permanenten Cookies verwende und dass das Cookie verschlüsselt und überprüft wird.

Das ist schon alles! Im Rahmen des Rollen-Frameworks werden die Benutzer Rollen in Cookies zwischengespeichert. Wenn der Browser des Benutzers Cookies nicht unterstützt, oder wenn seine Cookies gelöscht oder verloren gehen, ist dies keine große Sache – das `RolePrincipal` Objekt verwendet einfach die `Roles` Klasse, wenn kein Cookie (oder ein ungültiges oder abgelaufenes) verfügbar ist.

> [!NOTE]
> Die &amp; Practices-Gruppe von Microsoft verhindert die Verwendung von persistenten Rollen Cache Cookies. Da der Besitz des Rollen Cache Cookies ausreicht, um die Rollen Mitgliedschaft zu beweisen, kann ein Hacker auf irgendeine Weise Zugriff auf das Cookie eines gültigen Benutzers erlangen, wenn er die Identität dieses Benutzers annehmen kann. Die Wahrscheinlichkeit dieses Ereignisses erhöht sich, wenn das Cookie im Browser des Benutzers persistent gespeichert wird. Weitere Informationen zu dieser Sicherheitsempfehlung sowie andere Sicherheitsbedenken finden Sie in der [Sicherheitsfrage Liste für ASP.NET 2,0](https://msdn.microsoft.com/library/ms998375.aspx).

## <a name="step-1-defining-role-based-url-authorization-rules"></a>Schritt 1: Definieren von rollenbasierten URL-Autorisierungs Regeln

Wie im Tutorial zur <a id="_msoanchor_6"> </a> [*benutzerbasierten Autorisierung*](../membership/user-based-authorization-cs.md) erläutert, bietet die URL-Autorisierung die Möglichkeit, den Zugriff auf eine Gruppe von Seiten auf Benutzer-oder Rollen Basis einzuschränken. Die URL-Autorisierungs Regeln werden in `Web.config` mithilfe des [`<authorization>`-Elements](https://msdn.microsoft.com/library/8d82143t.aspx) mit `<allow>` und `<deny>` untergeordneten Elementen ausgeschrieben. Zusätzlich zu den Benutzer bezogenen Autorisierungs Regeln, die in den vorherigen Tutorials erläutert wurden, kann jedes `<allow>` und `<deny>` untergeordnete Element auch Folgendes umfassen:

- Eine bestimmte Rolle
- Eine durch Trennzeichen getrennte Liste von Rollen

Die URL-Autorisierungs Regeln gewähren z. b. Zugriff auf die Benutzer in den Rollen "Administratoren" und "Supervisor", verweigern aber den Zugriff auf alle anderen:

[!code-xml[Main](role-based-authorization-cs/samples/sample2.xml)]

Das `<allow>` Element im obigen Markup gibt an, dass die Administrator-und Aufsichts Rollen zulässig sind. Das `<deny>`-Element weist an, dass *alle* Benutzer verweigert werden.

Konfigurieren Sie die Anwendung so, dass die `ManageRoles.aspx`-, `UsersAndRoles.aspx`-und `CreateUserWizardWithRoles.aspx` Seiten nur für die Benutzer in der Administrator Rolle verfügbar sind, während die `RoleBasedAuthorization.aspx` Seite für alle Besucher zugänglich bleibt.

Um dies zu erreichen, fügen Sie zunächst dem Ordner `Roles` eine `Web.config`-Datei hinzu.

[![dem Rollen Verzeichnis eine Web. config-Datei hinzufügen](role-based-authorization-cs/_static/image8.png)](role-based-authorization-cs/_static/image7.png)

**Abbildung 3**: Hinzufügen einer `Web.config` Datei zum `Roles` Verzeichnis ([Klicken Sie, um das Bild in voller Größe anzuzeigen](role-based-authorization-cs/_static/image9.png))

Fügen Sie als nächstes `Web.config`Folgendes Konfigurations Markup hinzu:

[!code-xml[Main](role-based-authorization-cs/samples/sample3.xml)]

Das `<authorization>`-Element im `<system.web>` Abschnitt gibt an, dass nur Benutzer in der Administrator Rolle auf die ASP.NET-Ressourcen im `Roles`-Verzeichnis zugreifen können. Das `<location>`-Element definiert einen alternativen Satz von URL-Autorisierungs Regeln für die `RoleBasedAuthorization.aspx` Seite, sodass alle Benutzer die Seite besuchen können.

Nachdem Sie die Änderungen an `Web.config`gespeichert haben, melden Sie sich als Benutzer an, der nicht der Rolle "-Administratoren" ist, und versuchen Sie dann, eine der geschützten Seiten zu besuchen. Der `UrlAuthorizationModule` erkennt, dass Sie nicht über die Berechtigung zum Besuchen der angeforderten Ressource verfügen. folglich leitet der `FormsAuthenticationModule` Sie zur Anmeldeseite weiter. Die Anmeldeseite leitet Sie dann an die Seite "`UnauthorizedAccess.aspx`" weiter (siehe Abbildung 4). Diese abschließende Umleitung von der Anmeldeseite zu `UnauthorizedAccess.aspx` tritt aufgrund von Code auf, der in Schritt 2 des <a id="_msoanchor_7"> </a>Tutorials zur [*benutzerbasierten Autorisierung*](../membership/user-based-authorization-cs.md) zur Anmeldeseite hinzugefügt wurde. Auf der Anmeldeseite werden alle authentifizierten Benutzer automatisch an `UnauthorizedAccess.aspx` umgeleitet, wenn die Abfrage Zeichenfolge einen `ReturnUrl` Parameter enthält, da dieser Parameter angibt, dass der Benutzer nach dem Versuch, eine Seite anzuzeigen, auf der Anmeldeseite angezeigt wurde.

[![nur Benutzer in der Administrator Rolle können die geschützten Seiten anzeigen.](role-based-authorization-cs/_static/image11.png)](role-based-authorization-cs/_static/image10.png)

**Abbildung 4**: nur Benutzer in der Administrator Rolle können die geschützten Seiten anzeigen ([Klicken Sie, um das Bild in voller Größe anzuzeigen](role-based-authorization-cs/_static/image12.png))

Melden Sie sich ab, und melden Sie sich dann als Benutzer an, der sich in der-Administrator Rolle befindet. Nun sollten Sie in der Lage sein, die drei geschützten Seiten anzuzeigen.

[![Tito kann die Seite "usersandrole. aspx" besuchen, weil er der Rolle "Administratoren" ist.](role-based-authorization-cs/_static/image14.png)](role-based-authorization-cs/_static/image13.png)

**Abbildung 5**: Tito kann die `UsersAndRoles.aspx` Seite besuchen, da er sich in der Rolle "Administratoren" befindet ([Klicken Sie, um das Bild in voller Größe anzuzeigen](role-based-authorization-cs/_static/image15.png))

> [!NOTE]
> Beim Angeben von URL-Autorisierungs Regeln – für Rollen oder Benutzer – ist es wichtig zu berücksichtigen, dass die Regeln nacheinander von oben nach unten analysiert werden. Sobald eine Entsprechung gefunden wird, wird dem Benutzer der Zugriff gewährt oder verweigert, abhängig davon, ob die Entsprechung in einem `<allow>` oder `<deny>` Element gefunden wurde. **Wenn keine Entsprechung gefunden wird, wird dem Benutzer der Zugriff gewährt.** Wenn Sie also den Zugriff auf ein oder mehrere Benutzerkonten einschränken möchten, ist es zwingend erforderlich, dass Sie ein `<deny>`-Element als letztes Element in der URL-Autorisierungs Konfiguration verwenden. **Wenn Ihre URL-Autorisierungs Regeln** kein`<deny>`- **Element enthalten, wird allen Benutzern der Zugriff gewährt.** Eine ausführlichere Erläuterung zur Analyse der URL-Autorisierungs Regeln finden Sie im Abschnitt "ein Blick auf die Verwendung der Autorisierungs Regeln durch <a id="_msoanchor_8"> </a>den `UrlAuthorizationModule`, um den Zugriff zu gewähren oder zu verweigern" im Tutorial zur [*benutzerbasierten Autorisierung*](../membership/user-based-authorization-cs.md) .

## <a name="step-2-limiting-functionality-based-on-the-currently-logged-in-users-roles"></a>Schritt 2: Einschränken der Funktionalität basierend auf den Rollen des aktuell angemeldeten Benutzers

Mithilfe der URL-Autorisierung können Sie ganz einfach grobe Autorisierungs Regeln angeben, die angeben, welche Identitäten zulässig sind und welche verweigert werden, wenn eine bestimmte Seite (oder alle Seiten in einem Ordner und seinen Unterordnern) nicht angezeigt werden. In bestimmten Fällen möchten wir jedoch möglicherweise allen Benutzern erlauben, eine Seite zu besuchen, aber die Funktionalität der Seite auf der Grundlage der Benutzer Rollen zu begrenzen. Dies kann das Einblenden oder Ausblenden von Daten basierend auf der Rolle des Benutzers oder das Bereitstellung zusätzlicher Funktionen für Benutzer umfassen, die zu einer bestimmten Rolle gehören.

Solche rollenbasierten Autorisierungs Regeln können entweder deklarativ oder Programm gesteuert (oder mithilfe einer Kombination der beiden) implementiert werden. Im nächsten Abschnitt erfahren Sie, wie Sie die deklarative fein Körnung über das LoginView-Steuerelement implementieren. Danach werden die programmgesteuerten Verfahren erläutert. Bevor wir uns mit dem Anwenden von Regeln für die differenzierte Autorisierung befassen, müssen wir zunächst eine Seite erstellen, deren Funktionalität von der Rolle des Benutzers abhängt, der Sie besucht.

Wir erstellen eine Seite, auf der alle Benutzerkonten im System in einer GridView aufgeführt sind. Die GridView enthält den Benutzernamen, die e-Mail-Adresse des Benutzers, das Datum der letzten Anmeldung und Kommentare zum Benutzer. Zusätzlich zum Anzeigen der Informationen der einzelnen Benutzer enthält die GridView Funktionen zum Bearbeiten und löschen. Wir erstellen diese Seite zunächst mit der Funktion "Bearbeiten" und "Löschen", die allen Benutzern zur Verfügung steht. In den Abschnitten "Verwenden des LoginView-Steuer Elements" und "Programm gesteuertes einschränken von Funktionen" wird beschrieben, wie Sie diese Features basierend auf der Rolle des Besuchs Benutzers aktivieren oder deaktivieren.

> [!NOTE]
> Auf der ASP.NET Seite, die wir gerade erstellen, wird ein GridView-Steuerelement verwendet, um die Benutzerkonten anzuzeigen. Da sich diese tutorialreihe auf Formular Authentifizierung, Autorisierung, Benutzerkonten und Rollen konzentriert, möchte ich nicht zu viel Zeit aufwenden, um die interne Funktionsweise des GridView-Steuer Elements zu erörtern. In diesem Tutorial finden Sie eine ausführliche Anleitung zum Einrichten dieser Seite. es wird jedoch nicht ausführlich erläutert, warum bestimmte Optionen getroffen wurden oder welche Auswirkungen bestimmte Eigenschaften auf die gerenderte Ausgabe haben. Eine gründliche Untersuchung des GridView-Steuer Elements finden Sie in der tutorialreihe zu *[Arbeiten mit Daten in ASP.NET 2,0](../../data-access/index.md)* .

Öffnen Sie zunächst die Seite `RoleBasedAuthorization.aspx` im Ordner `Roles`. Ziehen Sie eine GridView von der Seite auf den Designer, und legen Sie den `ID` auf `UserGrid`fest. In Kürze schreiben wir Code, der die `Membership.GetAllUsers`-Methode aufruft und das resultierende `MembershipUserCollection` Objekt an die GridView bindet. Die `MembershipUserCollection` enthält ein `MembershipUser` Objekt für jedes Benutzerkonto im System. `MembershipUser` Objekte verfügen über Eigenschaften wie `UserName`, `Email`, `LastLoginDate`usw.

Bevor wir den Code schreiben, der die Benutzerkonten an das Raster bindet, legen wir zuerst die Felder der GridView fest. Klicken Sie in der GridView-Smarttag auf den Link "Spalten bearbeiten", um das Dialogfeld "Felder bearbeiten" zu öffnen (siehe Abbildung 6). Deaktivieren Sie in der linken unteren Ecke das Kontrollkästchen "Felder automatisch generieren". Da diese GridView Funktionen zum Bearbeiten und Löschen enthalten soll, fügen Sie ein CommandField-Feld hinzu, und legen Sie dessen Eigenschaften für `ShowEditButton` und `ShowDeleteButton` auf true fest. Fügen Sie als nächstes vier Felder zum Anzeigen der Eigenschaften `UserName`, `Email`, `LastLoginDate`und `Comment` hinzu. Verwenden Sie ein BoundField für die zwei schreibgeschützten Eigenschaften (`UserName` und `LastLoginDate`) und templatefields für die beiden bearbeitbaren Felder (`Email` und `Comment`).

Lassen Sie das erste BoundField die `UserName`-Eigenschaft anzeigen. Legen Sie die Eigenschaften des `HeaderText` und `DataField` auf "username" fest. Dieses Feld kann nicht bearbeitet werden. Legen Sie daher die `ReadOnly`-Eigenschaft auf "true" fest. Konfigurieren Sie das `LastLoginDate` BoundField, indem Sie dessen `HeaderText` auf "Last Login" und dessen `DataField` auf "LastLoginDate" festlegen. Formatieren Sie die Ausgabe dieses BoundField, sodass nur das Datum angezeigt wird (anstelle von Datum und Uhrzeit). Um dies zu erreichen, legen Sie die `HtmlEncode`-Eigenschaft dieses BoundField auf false und seine `DataFormatString`-Eigenschaft auf "{0:d}" fest. Legen Sie außerdem die `ReadOnly`-Eigenschaft auf true fest.

Legen Sie die `HeaderText` Eigenschaften der beiden templatefields auf "Email" und "comment" fest.

[![die Felder der GridView über das Dialog Feld "Felder" konfiguriert werden können.](role-based-authorization-cs/_static/image17.png)](role-based-authorization-cs/_static/image16.png)

**Abbildung 6**: die Felder der GridView können über das Dialog Feld "Felder" konfiguriert werden ([Klicken Sie, um das Bild in voller Größe anzuzeigen](role-based-authorization-cs/_static/image18.png))

Wir müssen nun die `ItemTemplate` und `EditItemTemplate` für die templatefields-Eigenschaft "Email" und "comment" definieren. Fügen Sie jedem der `ItemTemplate` s ein Label-websteuer Element hinzu, und binden Sie dessen `Text` Eigenschaften an die `Email`-bzw. `Comment` Eigenschaften.

Fügen Sie für das "Email"-TemplateField der `EditItemTemplate` ein Textfeld mit dem Namen "`Email`" hinzu, und binden Sie seine `Text`-Eigenschaft mithilfe der bidirektionalen Datenbindung an die `Email`-Eigenschaft. Fügen Sie dem `EditItemTemplate` ein "Requirements dfieldvalidator" und "RegularExpressionValidator" hinzu, um sicherzustellen, dass eine Besucher-e-Mail-Eigenschaft eine gültige e-Mail-Adresse eingegeben Fügen Sie für das "comment" TemplateField-Steuerelement ein mehrzeilige Textfeld mit dem Namen `Comment` zu seiner `EditItemTemplate`hinzu. Legen Sie die Eigenschaften "`Columns`" und "`Rows`" des Textfelds auf "40" bzw. "4" fest, und binden Sie dann die `Text` Eigenschaft mithilfe der bidirektionalen Datenbindung an die `Comment` Eigenschaft.

Nach dem Konfigurieren dieser templatefields sollte Ihr deklaratives Markup in etwa wie folgt aussehen:

[!code-aspx[Main](role-based-authorization-cs/samples/sample4.aspx)]

Beim Bearbeiten oder Löschen eines Benutzerkontos müssen wir den `UserName` Eigenschafts Wert dieses Benutzers kennen. Legen Sie die `DataKeyNames`-Eigenschaft der GridView auf "username" fest, damit diese Informationen über die `DataKeys` Auflistung der GridView zur Verfügung stehen.

Fügen Sie abschließend der Seite ein ValidationSummary-Steuerelement hinzu, und legen Sie dessen `ShowMessageBox`-Eigenschaft auf true und die `ShowSummary`-Eigenschaft auf false fest. Mit diesen Einstellungen wird in ValidationSummary eine Client seitige Warnung angezeigt, wenn der Benutzer versucht, ein Benutzerkonto mit einer fehlenden oder ungültigen e-Mail-Adresse zu bearbeiten.

[!code-aspx[Main](role-based-authorization-cs/samples/sample5.aspx)]

Wir haben nun das deklarative Markup dieser Seite abgeschlossen. Die nächste Aufgabe besteht darin, den Satz von Benutzerkonten an die GridView zu binden. Fügen Sie der Code Behind-Klasse der `RoleBasedAuthorization.aspx` Seite eine Methode mit dem Namen `BindUserGrid` hinzu, die die von `Membership.GetAllUsers` zurückgegebenen `MembershipUserCollection` an die `UserGrid` GridView bindet. Rufen Sie diese Methode aus dem `Page_Load`-Ereignishandler auf der ersten Seite auf.

[!code-csharp[Main](role-based-authorization-cs/samples/sample6.cs)]

Wenn dieser Code vorhanden ist, besuchen Sie die Seite über einen Browser. Wie Abbildung 7 zeigt, sollten Sie eine GridView-Liste mit Informationen zu jedem Benutzerkonto im System sehen.

[![der usergrid-GridView listet Informationen zu jedem Benutzer im System auf.](role-based-authorization-cs/_static/image20.png)](role-based-authorization-cs/_static/image19.png)

**Abbildung 7**: die `UserGrid` GridView listet Informationen zu jedem Benutzer im System auf ([Klicken Sie, um das Bild in voller Größe anzuzeigen](role-based-authorization-cs/_static/image21.png))

> [!NOTE]
> Die `UserGrid` GridView listet alle Benutzer in einer nicht auslagerenden Oberfläche auf. Diese einfache Raster Schnittstelle eignet sich nicht für Szenarien, in denen mehrere Dutzend Benutzer vorhanden sind. Eine Möglichkeit besteht darin, die GridView so zu konfigurieren, dass Paging aktiviert wird. Die `Membership.GetAllUsers`-Methode verfügt über zwei über Ladungen: eine, die keine Eingabeparameter akzeptiert und alle Benutzer zurückgibt, und eine, die ganzzahlige Werte für den Seitenindex und die Seitengröße annimmt und nur die angegebene Teilmenge der Benutzer zurückgibt. Die zweite Überladung kann verwendet werden, um die Benutzer effizienter zu durchlaufen, da Sie nur die genaue Teilmenge der Benutzerkonten und nicht *alle* zurückgibt. Wenn Sie Tausende von Benutzerkonten haben, können Sie eine filterbasierte Schnittstelle in Erwägung nehmen, die nur die Benutzer anzeigt, deren Benutzername mit einem ausgewählten Zeichen beginnt. Der [`Membership.FindUsersByName method`](https://msdn.microsoft.com/library/system.web.security.membership.findusersbyname.aspx) eignet sich ideal zum Aufbau einer Filter basierten Benutzeroberfläche. Wir werden uns mit dem Aufbau einer solchen Schnittstelle in einem zukünftigen Tutorial beschäftigen.

Das GridView-Steuerelement bietet eine integrierte Unterstützung für die Bearbeitung und Löschung, wenn das Steuerelement an ein ordnungsgemäß konfiguriertes Datenquellen Steuerelement gebunden ist, z. b. SqlDataSource oder ObjectDataSource. Die Daten der `UserGrid` GridView sind jedoch Programm gesteuert gebunden. Daher müssen wir Code schreiben, um diese beiden Aufgaben auszuführen. Insbesondere müssen wir Ereignishandler für die `RowEditing`-, `RowCancelingEdit`-, `RowUpdating`-und `RowDeleting`-Ereignisse der GridView erstellen, die ausgelöst werden, wenn ein Besucher auf die Schaltflächen Bearbeiten, Abbrechen, aktualisieren oder Löschen der GridView klickt.

Beginnen Sie mit dem Erstellen der Ereignishandler für die Ereignisse `RowEditing`, `RowCancelingEdit`und `RowUpdating` von GridView, und fügen Sie dann den folgenden Code hinzu:

[!code-csharp[Main](role-based-authorization-cs/samples/sample7.cs)]

Die `RowEditing`-und `RowCancelingEdit`-Ereignishandler legen einfach die `EditIndex`-Eigenschaft der GridView fest und binden dann die Liste der Benutzerkonten erneut an das Raster. Das interessante passiert im `RowUpdating` Ereignishandler. Dieser Ereignishandler stellt zunächst sicher, dass die Daten gültig sind, und fängt dann den `UserName` Wert des bearbeiteten Benutzerkontos aus der `DataKeys` Auflistung. Auf die Textfelder "`Email`" und "`Comment`" in den beiden "templatefields"-`EditItemTemplate` s wird dann Programm gesteuert verwiesen. Die `Text` Eigenschaften enthalten die bearbeitete e-Mail-Adresse und den Kommentar.

Um ein Benutzerkonto über die Mitgliedschafts-API zu aktualisieren, müssen wir zuerst die Informationen des Benutzers abrufen. Dies geschieht über einen `Membership.GetUser(userName)`. Die Eigenschaften `Email` und `Comment` des zurückgegebenen `MembershipUser` Objekts werden dann mit den Werten aktualisiert, die in die beiden Textfelder der Bearbeitungs Schnittstelle eingegeben werden. Schließlich werden diese Änderungen mit einem- [`Membership.UpdateUser`](https://msdn.microsoft.com/library/system.web.security.membership.updateuser.aspx)gespeichert. Der `RowUpdating` Ereignishandler wird abgeschlossen, indem die GridView auf die Vorbearbeitungs Schnittstelle zurückgesetzt wird.

Erstellen Sie als nächstes den `RowDeleting`-Ereignishandler, und fügen Sie dann den folgenden Code hinzu:

[!code-csharp[Main](role-based-authorization-cs/samples/sample8.cs)]

Der obige Ereignishandler beginnt mit dem Abrufen des `UserName` Werts aus der `DataKeys` Auflistung der GridView. dieser `UserName` Wert wird dann an die [`DeleteUser`-Methode](https://msdn.microsoft.com/library/system.web.security.membership.deleteuser.aspx)der Mitgliedschafts Klasse weitergegeben. Mit der `DeleteUser`-Methode wird das Benutzerkonto aus dem System gelöscht, einschließlich zugehöriger Mitgliedschafts Daten (z. b. die Rollen, denen dieser Benutzer angehört). Nachdem der Benutzer gelöscht wurde, wird der `EditIndex` des Rasters auf-1 festgelegt (für den Fall, dass der Benutzer auf "Delete" geklickt hat, während eine andere Zeile im Bearbeitungsmodus war) und die `BindUserGrid`-Methode aufgerufen

> [!NOTE]
> Die Schaltfläche Löschen erfordert keine Bestätigung vom Benutzer, bevor das Benutzerkonto gelöscht wird. Ich empfehle Ihnen, eine Art von Benutzer Bestätigung hinzuzufügen, um die Wahrscheinlichkeit zu verringern, dass ein Konto versehentlich gelöscht wird. Eine der einfachsten Möglichkeiten, eine Aktion zu bestätigen, ist das Dialogfeld Client seitiges bestätigen. Weitere Informationen zu dieser Technik finden Sie unter [Hinzufügen der Client seitigen Bestätigung beim Löschen](https://asp.net/learn/data-access/tutorial-42-cs.aspx).

Vergewissern Sie sich, dass diese Seite erwartungsgemäß funktioniert. Sie sollten in der Lage sein, die e-Mail-Adresse und den Kommentar eines Benutzers zu bearbeiten sowie alle Benutzerkonten zu löschen. Da auf die `RoleBasedAuthorization.aspx` Seite für alle Benutzer zugegriffen werden kann, können alle Benutzer – auch anonyme Besucher – diese Seite besuchen und Benutzerkonten bearbeiten und löschen. Aktualisieren Sie diese Seite so, dass nur Benutzer in den Rollen "Supervisor" und "Administratoren" die e-Mail-Adresse und den Kommentar eines Benutzers bearbeiten können, und nur Administratoren können ein Benutzerkonto löschen.

Im Abschnitt "Verwenden des LoginView-Steuer Elements" wird das LoginView-Steuerelement verwendet, um die für die Rolle des Benutzers spezifischen Anweisungen anzuzeigen. Wenn eine Person in der Administrator Rolle diese Seite besucht, werden Anweisungen zum Bearbeiten und Löschen von Benutzern angezeigt. Wenn ein Benutzer in der Aufsichtsrolle diese Seite erreicht, werden die Anweisungen zum Bearbeiten von Benutzern angezeigt. Wenn der Besucher anonym ist oder weder der Aufsichts-noch der Administrator Rolle ist, wird eine Meldung angezeigt, die anzeigt, dass Benutzerkontoinformationen nicht bearbeitet oder gelöscht werden können. Im Abschnitt "Programm gesteuertes einschränken von Funktionen" wird Code geschrieben, der die Schaltflächen Bearbeiten und löschen basierend auf der Rolle des Benutzers Programm gesteuert anzeigt oder ausblendet.

### <a name="using-the-loginview-control"></a>Verwenden des LoginView-Steuer Elements

Wie bereits in den vorherigen Tutorials gezeigt, eignet sich das LoginView-Steuerelement zum Anzeigen verschiedener Schnittstellen für authentifizierte und anonyme Benutzer, aber das LoginView-Steuerelement kann auch verwendet werden, um basierend auf den Rollen des Benutzers ein anderes Markup anzuzeigen. Wir verwenden ein LoginView-Steuerelement, um verschiedene Anweisungen auf der Grundlage der Rolle des Besuchs Benutzers anzuzeigen.

Fügen Sie zunächst eine LoginView oberhalb der `UserGrid` GridView hinzu. Wie bereits erwähnt, verfügt das LoginView-Steuerelement über zwei integrierte Vorlagen: `AnonymousTemplate` und `LoggedInTemplate`. Geben Sie in beiden Vorlagen eine kurze Nachricht ein, die den Benutzer darüber informiert, dass Benutzerinformationen nicht bearbeitet oder gelöscht werden können.

[!code-aspx[Main](role-based-authorization-cs/samples/sample9.aspx)]

Zusätzlich zu den `AnonymousTemplate` und `LoggedInTemplate`kann das LoginView-Steuerelement *RoleGroups*enthalten, bei denen es sich um Rollen spezifische Vorlagen handelt. Jede RoleGroup enthält eine einzelne Eigenschaft, `Roles`, die angibt, für welche Rollen die RoleGroup gilt. Die `Roles`-Eigenschaft kann auf eine einzelne Rolle (z. b. "Administratoren") oder auf eine durch Trennzeichen getrennte Liste von Rollen (z. b. "Administratoren, Supervisor") festgelegt werden.

Um die RoleGroups zu verwalten, klicken Sie auf den Link "RoleGroups bearbeiten" im Smarttag des Steuer Elements, um den RoleGroup-Auflistungs-Editor aufzurufenden. Fügen Sie zwei neue RoleGroups hinzu. Legen Sie die `Roles`-Eigenschaft der ersten RoleGroup auf "Administratoren" und die zweite auf "Supervisor" fest.

[![Verwalten der Rollen spezifischen Vorlagen von LoginView über den RoleGroup-Auflistungs-Editor](role-based-authorization-cs/_static/image23.png)](role-based-authorization-cs/_static/image22.png)

**Abbildung 8**: Verwalten der Rollen spezifischen Vorlagen von LoginView über den RoleGroup-Auflistungs-Editor ([Klicken Sie, um das Bild in voller Größe anzuzeigen](role-based-authorization-cs/_static/image24.png))

Klicken Sie auf OK, um den RoleGroup-Sammlungs-Editor zu schließen Dadurch wird das deklarative Markup von LoginView so aktualisiert, dass ein `<RoleGroups>` Abschnitt mit einem `<asp:RoleGroup>` untergeordneten-Element für jede im RoleGroup-Auflistungs-Editor definierte RoleGroup-Eigenschaft enthalten ist. Darüber hinaus enthält die Dropdown Liste "Views" (Ansichten) im LoginView-Smarttag, das anfänglich nur die `AnonymousTemplate` und `LoggedInTemplate` – aufgelistete, jetzt auch die hinzugefügten RoleGroups.

Bearbeiten Sie die RoleGroups so, dass Benutzern in der Rolle "Supervisor" Anweisungen zum Bearbeiten von Benutzerkonten angezeigt werden, während Benutzern in der Administrator Rolle Anweisungen zum Bearbeiten und löschen angezeigt werden. Nachdem Sie diese Änderungen vorgenommen haben, sollte das deklarative Markup Ihrer LoginView in etwa wie folgt aussehen.

[!code-aspx[Main](role-based-authorization-cs/samples/sample10.aspx)]

Nachdem Sie diese Änderungen vorgenommen haben, speichern Sie die Seite, und besuchen Sie Sie dann über einen Browser. Besuchen Sie zuerst die Seite als anonymer Benutzer. Die Meldung "Sie sind nicht beim System angemeldet." angezeigt. Daher können Sie keine Benutzerinformationen bearbeiten oder löschen. " Melden Sie sich dann als authentifizierter Benutzer an, aber einen, der weder der Aufsichts-noch der Administrator Rolle entspricht. Dieses Mal sollte die folgende Meldung angezeigt werden: "Sie sind kein Mitglied der Rolle" Supervisor "oder" Administratoren ". Daher können Sie keine Benutzerinformationen bearbeiten oder löschen. "

Melden Sie sich als nächstes als ein Benutzer an, der Mitglied der Rolle "Supervisor" ist. Dieses Mal sollte die Rollen spezifische Nachricht "Supervisor" angezeigt werden (siehe Abbildung 9). Wenn Sie sich als Benutzer in der Rolle "Administratoren" anmelden, sollte Ihnen die Rollen spezifische Meldung "Administratoren" angezeigt werden (siehe Abbildung 10).

[![Bruce wird die Rollen spezifische Nachricht "Supervisor" angezeigt.](role-based-authorization-cs/_static/image26.png)](role-based-authorization-cs/_static/image25.png)

**Abbildung 9**: Bruce zeigt die Rollen spezifische Nachricht "Supervisor" ([Klicken Sie, um das Bild in voller Größe anzuzeigen](role-based-authorization-cs/_static/image27.png))

[![Tito wird die Rollen spezifische Nachricht "Administratoren" angezeigt.](role-based-authorization-cs/_static/image29.png)](role-based-authorization-cs/_static/image28.png)

**Abbildung 10**: Tito zeigt die Rollen spezifische Meldung "Administratoren" ([Klicken Sie, um das Bild in voller Größe anzuzeigen](role-based-authorization-cs/_static/image30.png))

Wenn die Screenshots in Abbildung 9 und 10 angezeigt werden, rendert LoginView nur eine Vorlage, auch wenn mehrere Vorlagen zutreffen. Bruce und Tito sind jeweils angemeldete Benutzer, aber die LoginView rendert nur die passende RoleGroup und nicht die `LoggedInTemplate`. Außerdem gehört Tito zu den Rollen "Administratoren" und "Supervisor", aber das LoginView-Steuerelement rendert die Rollen spezifische Vorlage "Administratoren" anstelle der Vorgesetzten.

Abbildung 11 zeigt den Workflow, der vom LoginView-Steuerelement verwendet wird, um zu bestimmen, welche Vorlage dargestellt werden soll. Beachten Sie Folgendes: Wenn mehrere RoleGroup-Objekte angegeben sind, rendert die LoginView-Vorlage die *erste* RoleGroup, die mit übereinstimmt. Anders ausgedrückt: Wenn wir die Vorgesetzten RoleGroup als erste RoleGroup und die Administratoren als zweite festgelegt haben, wird die Aufsichts Nachricht in Tito angezeigt, wenn Tito diese Seite besucht hat.

[![den Workflow des LoginView-Steuer Elements zum Bestimmen der zu Rendering-Vorlage](role-based-authorization-cs/_static/image32.png)](role-based-authorization-cs/_static/image31.png)

**Abbildung 11**: der Workflow des LoginView-Steuer Elements zum Bestimmen der zu Rendering[-Vorlage (Klicken Sie, um das Bild in voller Größe anzuzeigen](role-based-authorization-cs/_static/image33.png))

### <a name="programmatically-limiting-functionality"></a>Programmgesteuerte Einschränkung der Funktionalität

Während das LoginView-Steuerelement basierend auf der Rolle des Benutzers, der die Seite besucht, andere Anweisungen anzeigt, bleiben die Schaltflächen Bearbeiten und Abbrechen für alle sichtbar. Wir müssen die Bearbeitungs-und Lösch Schaltflächen für anonyme Besucher und Benutzer, die weder der Aufsichts-noch der Administrator Rolle gehören, Programm gesteuert ausblenden. Wir müssen die Schaltfläche "Löschen" für alle Personen ausblenden, die kein Administrator sind. Um dies zu erreichen, schreiben wir ein wenig Code, der Programm gesteuert auf die Link Schaltflächen "Bearbeiten" und "Löschen" des commandfelds verweist und ihre `Visible` Eigenschaften bei Bedarf auf `false`festlegt.

Die einfachste Möglichkeit, auf Steuerelemente in einem CommandField Programm gesteuert zu verweisen, besteht darin, Sie zuerst in eine Vorlage zu konvertieren. Um dies zu erreichen, klicken Sie auf den Link "Spalten bearbeiten" im Smarttagmenü der GridView, wählen Sie das CommandField aus der Liste der aktuellen Felder aus, und klicken Sie auf den Link "dieses Feld in einen TemplateField konvertieren". Dadurch wird das CommandField mit einem `ItemTemplate` und `EditItemTemplate`in ein TemplateField-Element umgewandelt. Die `ItemTemplate` enthält die Link Schaltflächen Bearbeiten und löschen, während der `EditItemTemplate` die Link Schaltflächen aktualisieren und Abbrechen enthält.

[Konvertieren von CommandField in ein TemplateField ![](role-based-authorization-cs/_static/image35.png)](role-based-authorization-cs/_static/image34.png)

**Abbildung 12**: Konvertieren des CommandField in ein TemplateField-Element ([Klicken Sie, um das Bild in voller Größe anzuzeigen](role-based-authorization-cs/_static/image36.png))

Aktualisieren Sie die Link Schaltflächen Bearbeiten und löschen im `ItemTemplate`, und legen Sie deren `ID` Eigenschaften auf die Werte `EditButton` bzw. `DeleteButton`fest.

[!code-aspx[Main](role-based-authorization-cs/samples/sample11.aspx)]

Wenn Daten an die GridView gebunden werden, listet die GridView die Datensätze in der `DataSource`-Eigenschaft auf und generiert ein entsprechendes `GridViewRow`-Objekt. Während jedes `GridViewRow` Objekt erstellt wird, wird das `RowCreated` Ereignis ausgelöst. Um die Bearbeitungs-und Lösch Schaltflächen für nicht autorisierte Benutzer auszublenden, müssen wir einen Ereignishandler für dieses Ereignis erstellen und Programm gesteuert auf die Link Schaltflächen Bearbeiten und löschen verweisen und deren `Visible` Eigenschaften entsprechend festlegen.

Erstellen Sie einen Ereignishandler für das `RowCreated`-Ereignis, und fügen Sie dann den folgenden Code hinzu:

[!code-csharp[Main](role-based-authorization-cs/samples/sample12.cs)]

Beachten Sie, dass das `RowCreated`-Ereignis für *alle* GridView-Zeilen ausgelöst wird, einschließlich Header, Fußzeile, Pager-Schnittstelle usw. Wir möchten nur Programm gesteuert auf Link Schaltflächen Bearbeiten und löschen verweisen, wenn wir mit einer Daten Zeile arbeiten, die nicht im Bearbeitungsmodus ist (da die Zeile im Bearbeitungsmodus anstelle von "Bearbeiten" und "Löschen" Schaltflächen zum Aktualisieren und Abbrechen aufweist). Diese Überprüfung wird von der `if`-Anweisung behandelt.

Wenn eine Daten Zeile, die sich nicht im Bearbeitungsmodus befindet, behandelt wird, wird auf die Link Schaltflächen Bearbeiten und löschen verwiesen, und ihre `Visible` Eigenschaften werden auf der Grundlage der booleschen Werte festgelegt, die von der `IsInRole(roleName)`-Methode des `User` Objekts zurückgegeben werden. Das Benutzerobjekt verweist auf den Prinzipal, der vom `RoleManagerModule`erstellt wurde. Folglich verwendet die `IsInRole(roleName)`-Methode die Rollen-API, um zu bestimmen, ob der aktuelle Besucher zu *roleName*gehört.

> [!NOTE]
> Wir hätten die Rollen Klasse direkt verwenden können, indem wir den `User.IsInRole(roleName)` durch einen aufzurufenden [`Roles.IsUserInRole(roleName)`-Methode](https://msdn.microsoft.com/library/system.web.security.roles.isuserinrole.aspx)ersetzen. In diesem Beispiel habe ich beschlossen, die `IsInRole(roleName)` Methode des Prinzipal Objekts zu verwenden, da es effizienter ist als die direkte Verwendung der Rollen-API. An früherer Stelle in diesem Tutorial haben wir den Rollen-Manager so konfiguriert, dass die Rollen des Benutzers in einem Cookie zwischengespeichert werden. Diese zwischengespeicherten Cookie-Daten werden nur verwendet, wenn die `IsInRole(roleName)`-Methode des Prinzipals aufgerufen wird. direkte Aufrufe der Rollen-API umfassen immer eine Fahrt zum Rollen Speicher. Auch wenn Rollen nicht in einem Cookie zwischengespeichert werden, ist das Aufrufen der `IsInRole(roleName)`-Methode des Prinzipal Objekts in der Regel effizienter, da Sie beim ersten Aufrufen während einer Anforderung die Ergebnisse zwischenspeichert. Die Rollen-API hingegen führt keine Zwischenspeicherung aus. Da das `RowCreated` Ereignis einmal für jede Zeile in der GridView ausgelöst wird, umfasst die Verwendung `User.IsInRole(roleName)` nur einen Ausflug zum Rollen Speicher, während `Roles.IsUserInRole(roleName)` *n* Fahrten erfordert, wobei *n* die Anzahl der im Raster angezeigten Benutzerkonten ist.

Die `Visible`-Eigenschaft der Bearbeitungs Schaltfläche ist auf `true` festgelegt, wenn der Benutzer, der diese Seite besucht, der Rolle "Administratoren" oder "Supervisor" Andernfalls wird Sie auf `false`festgelegt. Die `Visible`-Eigenschaft der Schaltfläche "Löschen" wird nur `true` festgelegt, wenn der Benutzer der Rolle "Administratoren" ist.

Testen Sie diese Seite über einen Browser. Wenn Sie die Seite als anonymer Besucher oder als Benutzer besuchen, der weder ein Supervisor noch ein Administrator ist, ist das CommandField leer. Es ist immer noch vorhanden, aber als schlanker Farbname ohne die Schaltflächen "Bearbeiten" oder "Löschen".

> [!NOTE]
> Es ist möglich, das CommandField vollständig auszublenden, wenn ein nicht-Supervisor-und nicht-Administrator die Seite besucht. Ich lasse dies als Übung für den Reader aus.

[![die Schaltflächen "Bearbeiten" und "Löschen" für nicht-Supervisor und nicht-Administratoren ausgeblendet sind.](role-based-authorization-cs/_static/image38.png)](role-based-authorization-cs/_static/image37.png)

**Abbildung 13**: die Schaltflächen "Bearbeiten" und "Löschen" sind für nicht-Supervisor und nicht-Administratoren ausgeblendet ([Klicken Sie, um das Bild in voller Größe anzuzeigen](role-based-authorization-cs/_static/image39.png))

Wenn ein Benutzer, der der Rolle "Supervisor" angehört (aber nicht der Administrator Rolle) besucht, wird nur die Schaltfläche "Bearbeiten" angezeigt.

[![die Schaltfläche "Bearbeiten" für die Supervisor verfügbar ist, wird die Schaltfläche "Löschen" ausgeblendet.](role-based-authorization-cs/_static/image41.png)](role-based-authorization-cs/_static/image40.png)

**Abbildung 14**: die Schaltfläche "Bearbeiten" ist zwar für die Vorgesetzten verfügbar, die Schaltfläche "Löschen" ist jedoch ausgeblendet ([Klicken Sie, um das Bild in voller Größe](role-based-authorization-cs/_static/image42.png)

Wenn ein Administrator den Besuch durch nimmt, hat er Zugriff auf die Schaltflächen Bearbeiten und löschen.

[![die Schaltflächen Bearbeiten und löschen sind nur für Administratoren verfügbar.](role-based-authorization-cs/_static/image44.png)](role-based-authorization-cs/_static/image43.png)

**Abbildung 15**: die Schaltflächen "Bearbeiten" und "Löschen" sind nur für Administratoren verfügbar ([Klicken Sie, um das Bild in voller Größe anzuzeigen](role-based-authorization-cs/_static/image45.png))

## <a name="step-3-applying-role-based-authorization-rules-to-classes-and-methods"></a>Schritt 3: anwenden Rollen basierter Autorisierungs Regeln auf Klassen und Methoden

In Schritt 2 haben wir die Bearbeitungsfunktionen für die Benutzer in den Rollen "Supervisor" und "Administratoren" und die Löschfunktionen nur für Administratoren beschränkt. Dies wurde erreicht, indem die zugehörigen Benutzeroberflächen Elemente für nicht autorisierte Benutzer über programmgesteuerte Techniken ausgeblendet wurden. Solche Measures garantieren nicht, dass ein nicht autorisierter Benutzer eine privilegierte Aktion nicht ausführen kann. Möglicherweise werden Benutzeroberflächen Elemente hinzugefügt, die später hinzugefügt werden oder für nicht autorisierte Benutzer ausgeblendet werden. Oder ein Hacker kann eine andere Methode ermitteln, um die Seite "ASP.net" zu erhalten, um die gewünschte Methode auszuführen.

Eine einfache Möglichkeit, um sicherzustellen, dass ein bestimmtes Element nicht von einem nicht autorisierten Benutzer aufgerufen werden kann, besteht darin, diese Klasse oder Methode mit dem [`PrincipalPermission`-Attribut](https://msdn.microsoft.com/library/system.security.permissions.principalpermissionattribute.aspx)zu ergänzen. Wenn die .NET-Runtime eine-Klasse verwendet oder eine ihrer Methoden ausführt, wird überprüft, ob der aktuelle Sicherheitskontext über die-Berechtigung verfügt. Das `PrincipalPermission`-Attribut stellt einen Mechanismus bereit, mit dem diese Regeln definiert werden können.

Wir haben uns mit der Verwendung des `PrincipalPermission`-Attributs im Tutorial zur <a id="_msoanchor_9"> </a> [*benutzerbasierten Autorisierung*](../membership/user-based-authorization-cs.md) beschäftigt. Insbesondere haben wir gesehen, wie die `SelectedIndexChanged` und `RowDeleting` Ereignishandler der GridView ergänzt werden, damit Sie nur von authentifizierten Benutzern und Tito ausgeführt werden können. Das `PrincipalPermission`-Attribut funktioniert genauso gut wie Rollen.

Wir veranschaulichen die Verwendung des `PrincipalPermission`-Attributs für die `RowUpdating` und `RowDeleting` Ereignishandler der GridView, um die Ausführung für nicht autorisierte Benutzer zu verhindern. Wir müssen lediglich das entsprechende Attribut für jede Funktionsdefinition hinzufügen:

[!code-csharp[Main](role-based-authorization-cs/samples/sample13.cs)]

Das-Attribut für den `RowUpdating`-Ereignishandler legt fest, dass nur Benutzer in den Administratoren-oder Aufsichts Rollen den Ereignishandler ausführen können, wobei das-Attribut des `RowDeleting`-Ereignis Handlers die Ausführung auf Benutzer in der-Administrator Rolle beschränkt.

> [!NOTE]
> Das `PrincipalPermission`-Attribut wird als Klasse im `System.Security.Permissions`-Namespace dargestellt. Stellen Sie sicher, dass Sie am Anfang der Code-Behind-Klassendatei eine `using System.Security.Permissions`-Anweisung hinzufügen, um diesen Namespace zu importieren.

Wenn ein nicht-Administrator irgendwie versucht, den `RowDeleting`-Ereignishandler auszuführen, oder wenn ein nicht-Supervisor-oder nicht-Administrator versucht, den `RowUpdating`-Ereignishandler auszuführen, wird von der .NET-Laufzeit eine `SecurityException`erhoben.

[![wenn der Sicherheitskontext nicht zum Ausführen der Methode autorisiert ist, wird eine SecurityException ausgelöst.](role-based-authorization-cs/_static/image47.png)](role-based-authorization-cs/_static/image46.png)

**Abbildung 16**: Wenn der Sicherheitskontext nicht zum Ausführen der Methode autorisiert ist, wird ein `SecurityException` ausgelöst ([Klicken Sie, um das Bild in voller Größe anzuzeigen](role-based-authorization-cs/_static/image48.png)).

Neben ASP.NET Seiten verfügen viele Anwendungen auch über eine Architektur, die verschiedene Ebenen umfasst, z. b. Geschäftslogik und Datenzugriffsebenen. Diese Ebenen werden in der Regel als Klassenbibliotheken implementiert und bieten Klassen und Methoden zum Ausführen von Geschäftslogik-und datenbezogenen Funktionen. Das `PrincipalPermission`-Attribut eignet sich auch zum Anwenden von Autorisierungs Regeln auf diese Ebenen.

Weitere Informationen zur Verwendung des `PrincipalPermission`-Attributs zum Definieren von Autorisierungs Regeln für Klassen und Methoden finden Sie im Blogbeitrag von [Scott Guthrie](https://weblogs.asp.net/scottgu/)( [Hinzufügen von Autorisierungs Regeln zu Geschäfts-und Daten Ebenen mit `PrincipalPermissionAttributes`](https://weblogs.asp.net/scottgu/archive/2006/10/04/Tip_2F00_Trick_3A00_-Adding-Authorization-Rules-to-Business-and-Data-Layers-using-PrincipalPermissionAttributes.aspx)).

## <a name="summary"></a>Zusammenfassung

In diesem Tutorial haben wir uns mit der Angabe von groben und differenzierten Autorisierungs Regeln auf Grundlage der Rollen des Benutzers beschäftigt. ASP. Das URL-Autorisierungs Feature von NET ermöglicht es einem Seiten Entwickler, anzugeben, welchen Identitäten der Zugriff auf welche Seiten gewährt oder verweigert wird. Wie wir im Tutorial zur <a id="_msoanchor_10"> </a> [*benutzerbasierten Autorisierung*](../membership/user-based-authorization-cs.md) gesehen haben, können URL-Autorisierungs Regeln auf Benutzer-zu-Benutzer-Basis angewendet werden. Sie können auch auf Rollen Weise angewendet werden, wie in Schritt 1 dieses Tutorials erläutert wurde.

Differenzierte Autorisierungs Regeln können deklarativ oder Programm gesteuert angewendet werden. In Schritt 2 haben wir uns mit der Verwendung der RoleGroups-Funktion des LoginView-Steuer Elements beschäftigt, um eine andere Ausgabe basierend auf den Rollen des Benutzer Besuchs zu Renten. Wir haben auch Möglichkeiten untersucht, um Programm gesteuert zu ermitteln, ob ein Benutzer einer bestimmten Rolle angehört und wie die Funktionalität der Seite entsprechend angepasst wird.

Fröhliche Programmierung!

### <a name="further-reading"></a>Weitere nützliche Informationen

Weitere Informationen zu den in diesem Tutorial behandelten Themen finden Sie in den folgenden Ressourcen:

- [Hinzufügen von Autorisierungs Regeln zu Geschäfts-und Daten Ebenen mithilfe von `PrincipalPermissionAttributes`](https://weblogs.asp.net/scottgu/archive/2006/10/04/Tip_2F00_Trick_3A00_-Adding-Authorization-Rules-to-Business-and-Data-Layers-using-PrincipalPermissionAttributes.aspx)
- [Überprüfen der Mitgliedschaft, Rollen und Profile von ASP.NET 2.0: Arbeiten mit Rollen](http://aspnet.4guysfromrolla.com/articles/121405-1.aspx)
- [Sicherheitsfrage Liste für ASP.NET 2,0](https://msdn.microsoft.com/library/ms998375.aspx)
- [Technische Dokumentation für das `<roleManager>`-Element](https://msdn.microsoft.com/library/ms164660.aspx)

### <a name="about-the-author"></a>Zum Autor

Scott Mitchell, Autor mehrerer ASP/ASP. net-Bücher und Gründer von 4GuysFromRolla.com, hat seit 1998 mit Microsoft-Webtechnologien gearbeitet. Scott arbeitet als unabhängiger Berater, Ausbilder und Writer. Sein letztes Buch ist *[Sams Teach Yourself ASP.NET 2,0 in 24 Stunden](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)* . Scott kann über [mitchell@4guysfromrolla.com](mailto:mitchell@4guysfromrolla.com) oder über seinen Blog unter [http://ScottOnWriting.NET](http://scottonwriting.net/)erreicht werden.

### <a name="special-thanks-to"></a>Besonders vielen Dank...

Diese tutorialreihe wurde von vielen hilfreichen Reviewern geprüft. Zu den führenden Reviewern für dieses Tutorial zählen Suchi Banerjee und Teresa Murphy. Möchten Sie meine bevorstehenden MSDN-Artikel überprüfen? Wenn dies der Fall ist, löschen Sie eine Zeile bei [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Zurück](assigning-roles-to-users-cs.md)
> [Weiter](creating-and-managing-roles-vb.md)
