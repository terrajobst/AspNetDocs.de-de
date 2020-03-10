---
uid: web-api/overview/security/authentication-and-authorization-in-aspnet-web-api
title: Authentifizierung und Autorisierung in ASP.net-Web-API | Microsoft-Dokumentation
author: MikeWasson
description: Bietet eine allgemeine Übersicht über die Authentifizierung und Autorisierung in ASP.net-Web-API.
ms.author: riande
ms.date: 11/27/2012
ms.assetid: 6dfb51ea-9f4d-4e70-916c-8ef8344a88d6
msc.legacyurl: /web-api/overview/security/authentication-and-authorization-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 368d2b9456d12b2bb4063a23333e5c8837faa3b8
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78484419"
---
# <a name="authentication-and-authorization-in-aspnet-web-api"></a>Authentifizierung und Autorisierung in ASP.net-Web-API

von [Mike Wasson](https://github.com/MikeWasson)

Sie haben eine Web-API erstellt, aber jetzt möchten Sie den Zugriff darauf steuern. In dieser Artikel Reihe werden einige Optionen für die Sicherung einer Web-API vor nicht autorisierten Benutzern untersucht. Diese Reihe behandelt sowohl die Authentifizierung als auch die Autorisierung.

- Die *Authentifizierung* kennt die Identität des Benutzers. Beispielsweise meldet sich Alice mit Ihrem Benutzernamen und Kennwort an, und der Server verwendet das Kennwort zum Authentifizieren von Alice.
- Bei der *Autorisierung* wird entschieden, ob ein Benutzer eine Aktion ausführen darf. Beispielsweise hat Alice die Berechtigung, eine Ressource zu erhalten, aber keine Ressource zu erstellen.

Der erste Artikel in der Reihe bietet eine allgemeine Übersicht über die Authentifizierung und Autorisierung in ASP.net-Web-API. In anderen Themen werden gängige Authentifizierungs Szenarien für die Web-API beschrieben.

> [!NOTE]
> Vielen Dank an die Personen, die diese Reihe geprüft haben und wertvolle Feedback gegeben haben: Rick Anderson, Levi Broderick, Barry dorrane, Tom Dykstra, Hongmei ge, David Matson, Daniel Roth, Tim teebken.

## <a name="authentication"></a>Authentifizierung

Die Web-API geht davon aus, dass die Authentifizierung im Host erfolgt. Für das Webhosting ist der Host IIS, der HTTP-Module für die Authentifizierung verwendet. Sie können Ihr Projekt so konfigurieren, dass die in IIS oder ASP.NET integrierten Authentifizierungs Module verwendet werden, oder Sie können ein eigenes HTTP-Modul schreiben, um eine benutzerdefinierte Authentifizierung auszuführen.

Wenn der Host den Benutzer authentifiziert, erstellt er einen *Prinzipal*, bei dem es sich um ein [IPrincipal](https://msdn.microsoft.com/library/System.Security.Principal.IPrincipal.aspx) -Objekt handelt, das den Sicherheitskontext darstellt, in dem Code ausgeführt wird. Der Host fügt den Prinzipal an den aktuellen Thread an, indem **Thread. CurrentPrincipal**festgelegt wird. Der Prinzipal enthält ein zugeordnetes **Identitäts** Objekt, das Informationen über den Benutzer enthält. Wenn der Benutzer authentifiziert ist, gibt die **Identity. IsAuthenticated** -Eigenschaft " **true**" zurück. Bei anonymen Anforderungen gibt **IsAuthenticated** **false**zurück. Weitere Informationen zu Prinzipale finden Sie unter [rollenbasierte Sicherheit](https://msdn.microsoft.com/library/shz8h065.aspx).

### <a name="http-message-handlers-for-authentication"></a>HTTP-Nachrichten Handler für die Authentifizierung

Anstatt den Host für die Authentifizierung zu verwenden, können Sie die Authentifizierungs Logik in einem [http-Nachrichten Handler](../advanced/http-message-handlers.md)platzieren. In diesem Fall überprüft der Nachrichten Handler die HTTP-Anforderung und legt den Prinzipal fest.

Wann sollten Sie Nachrichten Handler für die Authentifizierung verwenden? Im folgenden sind einige Kompromisse aufgeführt:

- Ein HTTP-Modul sieht alle Anforderungen, die die ASP.NET-Pipeline durchlaufen. Ein Meldungs Handler sieht nur Anforderungen, die an die Web-API weitergeleitet werden.
- Sie können Nachrichten Handler pro Route festlegen, mit denen Sie ein Authentifizierungsschema auf eine bestimmte Route anwenden können.
- HTTP-Module sind spezifisch für IIS. Meldungs Handler sind Host agnostisch und können daher sowohl mit Webhosting als auch mit selbst Hosting verwendet werden.
- HTTP-Module nehmen an der IIS-Protokollierung, Überwachung usw. Teil.
- HTTP-Module werden zuvor in der Pipeline ausgeführt. Wenn Sie die Authentifizierung in einem Nachrichten Handler behandeln, wird der Prinzipal erst festgelegt, wenn der Handler ausgeführt wird. Darüber hinaus kehrt der Prinzipal auf den vorherigen Prinzipal zurück, wenn die Antwort den Nachrichten Handler verlässt.

Wenn Sie selbst Hosting nicht unterstützen müssen, ist ein HTTP-Modul im Allgemeinen eine bessere Option. Wenn Sie selbst Hosting unterstützen müssen, sollten Sie einen Nachrichten Handler in Erwägung gezogen.

### <a name="setting-the-principal"></a>Festlegen des Prinzipals

Wenn Ihre Anwendung eine benutzerdefinierte Authentifizierungs Logik ausführt, müssen Sie den Prinzipal an zwei Stellen festlegen:

- **Thread. CurrentPrincipal**. Diese Eigenschaft ist die Standardmethode zum Festlegen des Thread Prinzipals in .net.
- **HttpContext. Current. User**. Diese Eigenschaft ist für ASP.net spezifisch.

Der folgende Code zeigt, wie der Prinzipal festgelegt wird:

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample1.cs)]

Bei Webhosting müssen Sie den Prinzipal an beiden Stellen festlegen. Andernfalls kann der Sicherheitskontext inkonsistent werden. Für selbst Hosting ist **HttpContext. Current** jedoch NULL. Um sicherzustellen, dass Ihr Code Host agnostisch ist, sollten Sie vor dem zuweisen zu **HttpContext. Current**auf NULL prüfen, wie gezeigt.

## <a name="authorization"></a>Authorization

Die Autorisierung erfolgt später in der Pipeline, näher an dem Controller. Auf diese Weise können Sie präzisetere Entscheidungen treffen, wenn Sie den Zugriff auf Ressourcen gewähren.

- *Autorisierungs Filter* werden vor der Controller Aktion ausgeführt. Wenn die Anforderung nicht autorisiert ist, gibt der Filter eine Fehler Antwort zurück, und die Aktion wird nicht aufgerufen.
- Innerhalb einer Controller Aktion können Sie den aktuellen Prinzipal aus der **apicontroller. User** -Eigenschaft erhalten. Beispielsweise können Sie eine Liste von Ressourcen basierend auf dem Benutzernamen filtern und nur die Ressourcen zurückgeben, die zu diesem Benutzer gehören.

![](authentication-and-authorization-in-aspnet-web-api/_static/image1.png)

<a id="auth3"></a>
### <a name="using-the-authorize-attribute"></a>Verwenden des [autorisieren]-Attributs

Die Web-API stellt einen integrierten Autorisierungs Filter, [autorizeattribute](https://msdn.microsoft.com/library/system.web.http.authorizeattribute.aspx), bereit. Dieser Filter überprüft, ob der Benutzer authentifiziert ist. Andernfalls wird der HTTP-Statuscode 401 (nicht autorisiert) zurückgegeben, ohne dass die Aktion aufgerufen wird.

Sie können den Filter Global, auf Controller Ebene oder auf der Ebene einzelner Aktionen anwenden.

**Global**: um den Zugriff für jeden Web-API-Controller einzuschränken, fügen Sie den Filter " **Autorisierung** " der globalen Filterliste hinzu:

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample2.cs)]

**Controller**: Fügen Sie dem Controller den Filter als Attribut hinzu, um den Zugriff für einen bestimmten Controller einzuschränken:

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample3.cs)]

**Aktion**: Fügen Sie der Aktionsmethode das-Attribut hinzu, um den Zugriff auf bestimmte Aktionen einzuschränken:

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample4.cs)]

Alternativ können Sie den Controller einschränken und dann den anonymen Zugriff auf bestimmte Aktionen zulassen, indem Sie das `[AllowAnonymous]`-Attribut verwenden. Im folgenden Beispiel ist die `Post`-Methode eingeschränkt, aber die `Get`-Methode ermöglicht den anonymen Zugriff.

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample5.cs)]

In den vorherigen Beispielen ermöglicht der Filter allen authentifizierten Benutzern den Zugriff auf die eingeschränkten Methoden. nur anonyme Benutzer werden beibehalten. Sie können auch den Zugriff auf bestimmte Benutzer oder auf Benutzer in bestimmten Rollen einschränken:

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample6.cs)]

> [!NOTE]
> Der **Autoritäts** Filter für Web-API-Controller befindet sich im **System. Web. http** -Namespace. Es gibt einen ähnlichen Filter für MVC-Controller im **System. Web. MVC** -Namespace, der nicht mit Web-API-Controllern kompatibel ist.

### <a name="custom-authorization-filters"></a>Benutzerdefinierte Autorisierungs Filter

Um einen benutzerdefinierten Autorisierungs Filter zu schreiben, leiten Sie einen der folgenden Typen ab:

- **Autorität Attribute**. Erweitern Sie diese Klasse, um eine Autorisierungs Logik basierend auf dem aktuellen Benutzer und den Rollen des Benutzers auszuführen.
- **Authorizationfilterattribute**. Erweitern Sie diese Klasse, um eine synchrone Autorisierungs Logik auszuführen, die nicht unbedingt auf dem aktuellen Benutzer oder der Rolle basiert.
- **Iauthorizationfilter**. Implementieren Sie diese Schnittstelle, um asynchrone Autorisierungs Logik auszuführen. Wenn Ihre Autorisierungs Logik beispielsweise asynchrone e/a-oder Netzwerk Aufrufe ausführt. (Wenn Ihre Autorisierungs Logik CPU-gebunden ist, ist es einfacher, von **authorizationfilterattribute**abzuleiten, da Sie dann keine asynchrone Methode schreiben müssen.)

Das folgende Diagramm zeigt die Klassenhierarchie für die Klasse " **autorizeattribute** ".

![](authentication-and-authorization-in-aspnet-web-api/_static/image2.png)

### <a name="authorization-inside-a-controller-action"></a>Autorisierung innerhalb einer Controller Aktion

In einigen Fällen kann es vorkommen, dass eine Anforderung fortgesetzt werden kann, das Verhalten jedoch auf Grundlage des Prinzipals geändert wird. Beispielsweise können sich die Informationen, die Sie zurückgeben, abhängig von der Rolle des Benutzers ändern. Innerhalb einer Controller Methode können Sie den aktuellen Prinzipal aus der **apicontroller. User** -Eigenschaft erhalten.

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample7.cs)]
