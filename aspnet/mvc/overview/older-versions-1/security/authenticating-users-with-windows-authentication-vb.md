---
uid: mvc/overview/older-versions-1/security/authenticating-users-with-windows-authentication-vb
title: Authentifizieren von Benutzern mit der Windows-Authentifizierung (VB) | Microsoft-Dokumentation
author: microsoft
description: Erfahren Sie, wie Sie die Windows-Authentifizierung im Kontext einer MVC-Anwendung verwenden. Sie erfahren, wie Sie die Windows-Authentifizierung im Web Ihrer Anwendung aktivieren...
ms.author: riande
ms.date: 01/27/2009
ms.assetid: 532fa051-7d5c-4d6d-87f6-339ce4b84c44
msc.legacyurl: /mvc/overview/older-versions-1/security/authenticating-users-with-windows-authentication-vb
msc.type: authoredcontent
ms.openlocfilehash: aa64b1f9ef6461a81611ca066310dca2d545baa3
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78506241"
---
# <a name="authenticating-users-with-windows-authentication-vb"></a>Authentifizieren von Benutzern bei der Windows-Authentifizierung (VB)

von [Microsoft](https://github.com/microsoft)

> Erfahren Sie, wie Sie die Windows-Authentifizierung im Kontext einer MVC-Anwendung verwenden. Sie erfahren, wie Sie die Windows-Authentifizierung in der Webkonfigurationsdatei Ihrer Anwendung aktivieren und wie Sie die Authentifizierung mit IIS konfigurieren. Schließlich erfahren Sie, wie Sie das Attribut [autorisieren] verwenden, um den Zugriff auf Controller Aktionen auf bestimmte Windows-Benutzer oder-Gruppen zu beschränken.

In diesem Tutorial wird erläutert, wie Sie die Internetinformationsdienste in integrierten Sicherheitsfeatures nutzen können, um die Ansichten in ihren MVC-Anwendungen per Kennwort zu schützen. Sie erfahren, wie Sie zulassen, dass Controller Aktionen nur von bestimmten Windows-Benutzern oder-Benutzern aufgerufen werden, die Mitglieder bestimmter Windows-Gruppen sind.

Die Verwendung der Windows-Authentifizierung ist sinnvoll, wenn Sie eine interne Unternehmenswebsite (eine Intranetsite) aufbauen und möchten, dass die Benutzer Ihre standardmäßigen Windows-Benutzernamen und-Kenn Wörter für den Zugriff auf die Website verwenden können. Wenn Sie eine nach außen gerichtete Website (Internet Website) aufbauen, sollten Sie stattdessen die Formular Authentifizierung verwenden.

#### <a name="enabling-windows-authentication"></a>Aktivieren der Windows-Authentifizierung

Wenn Sie eine neue ASP.NET MVC-Anwendung erstellen, ist die Windows-Authentifizierung standardmäßig nicht aktiviert. Die Formular Authentifizierung ist der für MVC-Anwendungen aktivierte Standard Authentifizierungstyp. Sie müssen die Windows-Authentifizierung aktivieren, indem Sie die Webkonfigurationsdatei (Web. config) Ihrer MVC-Anwendung ändern. Suchen Sie den Abschnitt &lt;Authentication&gt;, und ändern Sie ihn so, dass Windows anstelle der Formular Authentifizierung wie folgt verwendet wird:

[!code-xml[Main](authenticating-users-with-windows-authentication-vb/samples/sample1.xml)]

Wenn Sie die Windows-Authentifizierung aktivieren, wird Ihr Webserver für die Authentifizierung von Benutzern verantwortlich. In der Regel gibt es zwei verschiedene Typen von Webservern, die Sie beim Erstellen und Bereitstellen einer ASP.NET MVC-Anwendung verwenden.

Beim Entwickeln einer MVC-Anwendung verwenden Sie zunächst den ASP.NET Development-Webserver, der in Visual Studio enthalten ist. Standardmäßig führt der ASP.NET Development-Webserver alle Seiten im Kontext des aktuellen Windows-Kontos aus (welches Konto Sie zum Anmelden bei Windows verwendet haben).

Der ASP.NET Development-Webserver unterstützt auch die NTLM-Authentifizierung. Sie können die NTLM-Authentifizierung aktivieren, indem Sie im Projektmappen-Explorer Fenster mit der rechten Maustaste auf den Namen des Projekts klicken und Eigenschaften auswählen. Wählen Sie als nächstes die Registerkarte Web aus, und aktivieren Sie das Kontrollkästchen NTLM (siehe Abbildung 1).

**Abbildung 1 – Aktivieren der NTLM-Authentifizierung für den ASP.net-entwicklungsweb Server**

![clip_image002](authenticating-users-with-windows-authentication-vb/_static/image1.jpg)

Bei einer produktionsweb Anwendung verwenden Sie dagegen IIS als Webserver. IIS unterstützt verschiedene Arten der Authentifizierung, einschließlich:

- Standard Authentifizierung – als Teil des HTTP 1,0-Protokolls definiert. Benutzernamen und Kenn Wörter werden im Klartext (base64-codiert) über das Internet gesendet. -Digest-Authentifizierung – sendet einen Hashwert eines Kennworts anstelle des Kennworts über das Internet. -Integrierte Windows-Authentifizierung (NTLM) – der beste Authentifizierungstyp, der in Intranetumgebungen mithilfe von Windows verwendet werden soll. -Zertifikat Authentifizierung – aktiviert die Authentifizierung mithilfe eines Client seitigen Zertifikats. Das Zertifikat ist einem Windows-Benutzerkonto zugeordnet.

> [!NOTE] 
> 
> Eine ausführlichere Übersicht über die verschiedenen Authentifizierungs Typen finden Sie unter [https://msdn.microsoft.com/library/aa292114(VS.71).aspx](https://msdn.microsoft.com/library/aa292114(VS.71).aspx).

Sie können Internetinformationsdienste-Manager verwenden, um einen bestimmten Authentifizierungstyp zu aktivieren. Beachten Sie, dass alle Arten der Authentifizierung bei jedem Betriebssystem nicht verfügbar sind. Wenn Sie IIS 7,0 mit Windows Vista verwenden, müssen Sie außerdem die verschiedenen Arten der Windows-Authentifizierung aktivieren, bevor Sie im Internetinformationsdienste Manager angezeigt werden. Öffnen Sie die **Systemsteuerung, Programme, Programme und Funktionen, aktivieren oder deaktivieren Sie Windows-Funktionen**, und erweitern Sie den Knoten Internetinformationsdienste (siehe Abbildung 2).

**Abbildung 2 – Aktivieren von Windows IIS-Features**

![clip_image004](authenticating-users-with-windows-authentication-vb/_static/image2.jpg)

Mithilfe Internetinformationsdienste können Sie unterschiedliche Authentifizierungs Typen aktivieren oder deaktivieren. Abbildung 3 zeigt z. b. die Deaktivierung der anonymen Authentifizierung und die Aktivierung der NTLM-Authentifizierung (Integrated Windows) bei Verwendung von IIS 7,0.

**Abbildung 3 – Aktivieren der integrierten Windows-Authentifizierung**

![clip_image006](authenticating-users-with-windows-authentication-vb/_static/image3.jpg)

#### <a name="authorizing-windows-users-and-groups"></a>Autorialisieren von Windows-Benutzern und-Gruppen

Nachdem Sie die Windows-Authentifizierung aktiviert haben, können Sie das &lt;autorisieren&gt; Attribut verwenden, um den Zugriff auf Controller oder Controller Aktionen zu steuern. Dieses Attribut kann auf einen gesamten MVC-Controller oder eine bestimmte Controller Aktion angewendet werden.

Der Home-Controller in der Liste 1 macht z. b. drei Aktionen mit den Namen Index (), companysecrets () und stephensecrets () verfügbar. Jeder kann die Index ()-Aktion aufrufen. Allerdings können nur Mitglieder der lokalen Windows-Manager-Gruppe die companysecrets ()-Aktion aufrufen. Zum Schluss kann nur der Windows-Domänen Benutzer mit dem Namen Stephen (in der Redmond-Domäne) die Aktion stephensecrets () aufrufen.

**Codebeispiel 1 – controllers\homecontroller.vb**

[!code-vb[Main](authenticating-users-with-windows-authentication-vb/samples/sample2.vb)]

> [!NOTE]
> Aufgrund der Windows-Benutzerkontensteuerung (User Account Control, UAC) verhält sich die lokale Gruppe "Administratoren" bei der Arbeit mit Windows Vista oder Windows Server 2008 anders als andere Gruppen. Mit dem &lt;Autorisierungs&gt; Attribut wird ein Mitglied der lokalen Administratoren Gruppe nicht ordnungsgemäß erkannt, es sei denn, Sie ändern die UAC-Einstellungen Ihres Computers.

Genau was geschieht, wenn Sie versuchen, eine Controller Aktion aufzurufen, ohne die richtigen Berechtigungen zu haben, hängt vom Authentifizierungstyp ab, der aktiviert ist. Wenn Sie die ASP.NET Development Server verwenden, erhalten Sie standardmäßig einfach eine leere Seite. Die Seite wird mit dem HTTP-Antwort Status **401 nicht autorisiert** bereitgestellt.

Wenn Sie dagegen IIS mit deaktivierter anonymer Authentifizierung und Standard Authentifizierung verwenden, erhalten Sie immer dann eine Anmelde Dialogfeld-Eingabeaufforderung, wenn Sie die geschützte Seite anfordern (siehe Abbildung 4).

**Abbildung 4 – Anmelde Dialogfeld für die Standard Authentifizierung**

![clip_image008](authenticating-users-with-windows-authentication-vb/_static/image4.jpg)

#### <a name="summary"></a>Zusammenfassung

In diesem Tutorial wurde erläutert, wie Sie die Windows-Authentifizierung im Kontext einer ASP.NET MVC-Anwendung verwenden können. Sie haben gelernt, wie Sie die Windows-Authentifizierung in der Webkonfigurationsdatei Ihrer Anwendung aktivieren und wie Sie die Authentifizierung mit IIS konfigurieren. Schließlich haben Sie erfahren, wie Sie das &lt;autorisieren&gt; Attribut verwenden, um den Zugriff auf Controller Aktionen auf bestimmte Windows-Benutzer oder-Gruppen zu beschränken.

> [!div class="step-by-step"]
> [Zurück](authenticating-users-with-forms-authentication-vb.md)
> [Weiter](preventing-javascript-injection-attacks-vb.md)
