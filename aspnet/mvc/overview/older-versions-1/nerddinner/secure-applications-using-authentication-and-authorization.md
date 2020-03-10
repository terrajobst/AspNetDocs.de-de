---
uid: mvc/overview/older-versions-1/nerddinner/secure-applications-using-authentication-and-authorization
title: Sichern von Anwendungen mithilfe von Authentifizierung und Autorisierung | Microsoft-Dokumentation
author: microsoft
description: In Schritt 9 erfahren Sie, wie Sie die Authentifizierung und Autorisierung zum Schutz unserer "nerddinner"-Anwendung hinzufügen, sodass sich Benutzer registrieren und sich bei der Website anmelden müssen, um Sie zu erstellen.
ms.author: riande
ms.date: 07/27/2010
ms.assetid: 9e4d5cac-b071-440c-b044-20b6d0c964fb
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/secure-applications-using-authentication-and-authorization
msc.type: authoredcontent
ms.openlocfilehash: 8d509c5f15bb4d5014e53b8dc2a736454238e72c
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78486423"
---
# <a name="secure-applications-using-authentication-and-authorization"></a>Sichern von Anwendungen mithilfe von Authentifizierung und Autorisierung

von [Microsoft](https://github.com/microsoft)

[PDF herunterladen](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Dies ist Schritt 9 des kostenlosen ["nerddinner"](introducing-the-nerddinner-tutorial.md) -Lernprogramms, in dem erläutert wird, wie eine kleine, aber komplette Webanwendung mit ASP.NET MVC 1 erstellt wird.
> 
> In Schritt 9 erfahren Sie, wie Sie die Authentifizierung und Autorisierung zum Schutz unserer "nerddinner"-Anwendung hinzufügen, damit Benutzer sich registrieren und sich bei der Website anmelden müssen, um neue Abendessen zu erstellen
> 
> Wenn Sie ASP.NET MVC 3 verwenden, empfiehlt es sich, die Tutorials " [Getting Started with MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) " oder " [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) " zu befolgen.

## <a name="nerddinner-step-9-authentication-and-authorization"></a>Nerddinner Step 9: Authentifizierung und Autorisierung

Momentan gewährt unsere "nerddinner"-Anwendung allen Personen, die die Website besuchen, die Möglichkeit, die Details eines beliebigen Dinner zu erstellen und zu bearbeiten. Ändern wir dies, damit Benutzer sich registrieren und sich bei der Website anmelden müssen, um neue Abendessen zu erstellen, und eine Einschränkung hinzufügen, sodass nur der Benutzer, der ein Dinner als Host erstellt, ihn später bearbeiten kann.

Um dies zu ermöglichen, verwenden wir die Authentifizierung und Autorisierung, um die Anwendung zu schützen.

### <a name="understanding-authentication-and-authorization"></a>Grundlegendes zur Authentifizierung und Autorisierung

Bei der *Authentifizierung* wird die Identität eines Clients, der auf eine Anwendung zugreift, identifiziert und überprüft. Genauer ausgedrückt: Es geht darum, den Endbenutzer zu identifizieren, wenn er eine Website besucht. ASP.NET unterstützt mehrere Möglichkeiten, Browser Benutzer zu authentifizieren. Bei Internet Webanwendungen wird der am häufigsten verwendete Authentifizierungs Ansatz als "Formular Authentifizierung" bezeichnet. Die Formular Authentifizierung ermöglicht es einem Entwickler, ein HTML-Anmeldeformular innerhalb der Anwendung zu erstellen und dann den Benutzernamen/das Kennwort zu überprüfen, den ein Endbenutzer an eine Datenbank oder einen anderen Kenn Wort Anmelde Informationsspeicher übermittelt. Wenn die Kombination aus Benutzername und Kennwort richtig ist, kann der Entwickler dann ASP.net auffordern, ein verschlüsseltes HTTP-Cookie auszugeben, um den Benutzer über zukünftige Anforderungen zu identifizieren. Wir verwenden die Formular Authentifizierung mit unserer "nerddinner"-Anwendung.

*Autorisierung* ist der Prozess der Bestimmung, ob ein authentifizierter Benutzer über die Berechtigung zum Zugreifen auf eine bestimmte URL/Ressource verfügt, oder um eine Aktion auszuführen. Beispielsweise möchten wir innerhalb unserer nerddinner-Anwendung autorisieren, dass nur Benutzer, die angemeldet sind, auf die */Dinners/Create* -URL zugreifen und neue Dinner-Daten erstellen können. Wir möchten auch Autorisierungs Logik hinzufügen, sodass nur der Benutzer, der ein Dinner-Hosting durchläuft, ihn bearbeiten kann – und den Bearbeitungs Zugriff für alle anderen Benutzer verweigern.

### <a name="forms-authentication-and-the-accountcontroller"></a>Formular Authentifizierung und AccountController

Die standardmäßige Visual Studio-Projektvorlage für ASP.NET MVC aktiviert automatisch die Formular Authentifizierung, wenn neue ASP.NET MVC-Anwendungen erstellt werden. Außerdem wird dem Projekt automatisch eine vorgefertigte Implementierung der Konto Anmeldeseite hinzugefügt – dadurch ist es wirklich einfach, die Sicherheit innerhalb einer Site zu integrieren.

Auf der Standardseite Site. Master Master wird oben rechts auf der Website ein Link "Anmelden" angezeigt, wenn der Benutzer, der darauf zugreift, nicht authentifiziert ist:

![](secure-applications-using-authentication-and-authorization/_static/image1.png)

Wenn Sie auf den Link "Anmelden" klicken, wird ein Benutzer zur */Account/Logon* -URL hinzu:

![](secure-applications-using-authentication-and-authorization/_static/image2.png)

Besucher, die sich noch nicht registriert haben, können dies erreichen, indem Sie auf den Link "registrieren" klicken – der die */Account/Register* -URL erhält und Ihnen ermöglicht, Konto Details einzugeben:

![](secure-applications-using-authentication-and-authorization/_static/image3.png)

Wenn Sie auf die Schaltfläche "registrieren" klicken, wird ein neuer Benutzer innerhalb des ASP.net-Mitgliedschafts Systems erstellt, und der Benutzer wird mithilfe der Formular Authentifizierung auf der Website authentifiziert.

Wenn ein Benutzer angemeldet ist, ändert die Website. Master die obere rechte Seite der Seite, um eine "Willkommen [username]!" auszugeben. und rendert den Link "Abmelden" anstelle eines "Anmelden". Wenn Sie auf den Link "Abmelden" klicken, wird der Benutzer protokolliert:

![](secure-applications-using-authentication-and-authorization/_static/image4.png)

Die oben genannten Anmelde-, Abmelde-und Registrierungsfunktionen werden innerhalb der AccountController-Klasse implementiert, die von Visual Studio beim Erstellen des Projekts dem Projekt hinzugefügt wurde. Die Benutzeroberfläche für AccountController wird mithilfe von Ansichts Vorlagen im Verzeichnis "\views\account" implementiert:

![](secure-applications-using-authentication-and-authorization/_static/image5.png)

Die AccountController-Klasse verwendet das ASP.NET-Formular Authentifizierungssystem, um verschlüsselte Authentifizierungs Cookies auszugeben, und die ASP.net-Mitgliedschafts-API zum Speichern und Überprüfen von Benutzernamen und Kenn Wörtern. Die ASP.net-Mitgliedschafts-API ist erweiterbar und ermöglicht die Verwendung beliebiger Anmelde Informationsspeicher für Kenn Wörter. ASP.net wird mit integrierten Mitgliedschafts Anbieter Implementierungen ausgeliefert, die Benutzername/Kennwort in einer SQL-Datenbank oder innerhalb Active Directory speichern.

Wir können den Mitgliedschafts Anbieter konfigurieren, den unsere nerddinner-Anwendung verwenden soll, indem wir die Datei "Web. config" im Stammverzeichnis des Projekts öffnen und den Abschnitt &lt;Mitgliedschaft&gt; darin suchen. Die standardmäßige Web. config-Datei, die beim Erstellen des Projekts hinzugefügt wurde, registriert den SQL-Mitgliedschafts Anbieter und konfiguriert sie so, dass eine Verbindungs Zeichenfolge mit dem Namen "ApplicationServices" zum Angeben des Daten Bank Speicher Orts

Die Standard Verbindungs Zeichenfolge "ApplicationServices" (die im Abschnitt "&lt;connectionStrings&gt; der Datei" Web. config "angegeben ist) ist für die Verwendung von SQL Express konfiguriert. Er verweist auf eine SQL Express-Datenbank mit dem Namen "aspnetdb". MDF "unter dem Verzeichnis" App\_Data "der Anwendung. Wenn diese Datenbank nicht vorhanden ist, wenn die Mitgliedschafts-API zum ersten Mal innerhalb der Anwendung verwendet wird, erstellt ASP.NET automatisch die Datenbank und stellt das entsprechende Mitgliedschafts Datenbankschema darin bereit:

![](secure-applications-using-authentication-and-authorization/_static/image6.png)

Wenn Sie anstelle von SQL Express eine vollständige SQL Server Instanz verwenden möchten (oder eine Verbindung mit einer Remote Datenbank herstellen), müssen Sie lediglich die Verbindungs Zeichenfolge "ApplicationServices" in der Datei "Web. config" aktualisieren und sicherstellen, dass das entsprechende Mitgliedschafts Schema wurde der Datenbank hinzugefügt, auf die Sie verweist. Sie können das Hilfsprogramm "ASPNET\_RegSql. exe" im Verzeichnis \WINDOWS\Microsoft.NET\Framework\v2.0.50727\ ausführen, um das entsprechende Schema für die Mitgliedschaft und die anderen ASP.NET-Anwendungsdienste zu einer Datenbank hinzuzufügen.

### <a name="authorizing-the-dinnerscreate-url-using-the-authorize-filter"></a>Autorisieren der/Dinners/Create-URL mithilfe des [autorisieren]-Filters

Wir mussten keinen Code schreiben, um eine sichere Implementierung der Authentifizierung und Kontoverwaltung für die "nerddinner"-Anwendung zu ermöglichen. Benutzer können neue Konten bei unserer Anwendung registrieren und sich bei der Website anmelden/abmelden.

Nun können wir der Anwendung Autorisierungs Logik hinzufügen und den Authentifizierungs Status und den Benutzernamen der Besucher verwenden, um zu steuern, was Sie innerhalb der Site tun dürfen und was nicht. Fügen Sie zunächst der "Create"-Aktionsmethode der Klasse "dinnerscontroller" eine Autorisierungs Logik hinzu. Insbesondere ist es erforderlich, dass Benutzer, die auf die */Dinners/Create* -URL zugreifen, angemeldet werden müssen. Wenn Sie nicht angemeldet sind, werden Sie zur Anmeldeseite umgeleitet, damit Sie sich anmelden können.

Das Implementieren dieser Logik ist ziemlich einfach. Wir müssen lediglich ein [autorisieren]-Filter Attribut zu den Create Action-Methoden wie folgt hinzufügen:

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample1.cs)]

ASP.NET MVC unterstützt die Möglichkeit, "Aktionsfilter" zu erstellen, die verwendet werden können, um wiederverwendbare Logik zu implementieren, die deklarativ auf Aktionsmethoden angewendet werden kann. Der [Authorization]-Filter ist einer der integrierten Aktionsfilter, die von ASP.NET MVC bereitgestellt werden, und ermöglicht es einem Entwickler, deklarativ Autorisierungs Regeln auf Aktionsmethoden und Controller Klassen anzuwenden.

Beim Anwenden ohne Parameter (wie oben) erzwingt der Filter [autorisieren], dass der Benutzer, der die Anforderung der Aktionsmethode vornimmt, in – protokolliert werden muss, und der Browser wird automatisch an die Anmelde-URL umgeleitet, wenn dies nicht der Fall ist. Bei dieser Umleitung wird die ursprünglich angeforderte URL als QueryString-Argument weitergegeben (z. b.:/Account/LOGON? Rückgab =% 2madinner% 2F Create). Der AccountController leitet den Benutzer dann nach der Anmeldung an die ursprünglich angeforderte URL zurück.

Der [autorisieren]-Filter unterstützt optional die Möglichkeit, eine Eigenschaft "Benutzer" oder "Rollen" anzugeben, die verwendet werden kann, um anzugeben, dass der Benutzer sowohl in als auch in einer Liste zulässiger Benutzer oder Mitglied einer zulässigen Sicherheitsrolle angemeldet ist. Der folgende Code ermöglicht beispielsweise nur zwei bestimmten Benutzern, "scottgu" und "billg", den Zugriff auf die/Dinners/Create-URL:

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample2.cs)]

Das Einbetten bestimmter Benutzernamen in Code ist in der Regel nicht möglich. Ein besserer Ansatz besteht darin, Rollen höherer Ebene zu definieren, die vom Code überprüft werden, und dann die Benutzer der Rolle entweder mithilfe einer Datenbank oder eines Active Directory-Systems zuzuordnen (sodass die tatsächliche Benutzer Zuordnungs Liste Extern aus dem Code gespeichert werden kann). ASP.NET enthält eine integrierte Rollen Verwaltungs-API sowie eine integrierte Gruppe von Rollen Anbietern (einschließlich derjenigen für SQL und Active Directory), die bei der Durchführung dieser Benutzer-/Rollenzuordnung helfen können. Anschließend können wir den Code aktualisieren, sodass nur Benutzer innerhalb einer bestimmten Administrator Rolle auf die/Dinners/Create-URL zugreifen können:

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample3.cs)]

### <a name="using-the-useridentityname-property-when-creating-dinners"></a>Verwenden der User.Identity.Name-Eigenschaft beim Erstellen von Abendessen

Wir können den Benutzernamen des aktuell angemeldeten Benutzers einer Anforderung mithilfe der User.Identity.Name-Eigenschaft abrufen, die für die Controller-Basisklasse verfügbar gemacht wird.

Früher, als wir die HTTP-Post-Version unserer Create ()-Aktionsmethode implementiert haben, hatten wir die Eigenschaft "hostedby" des Dinner in eine statische Zeichenfolge hart codiert. Wir können nun diesen Code aktualisieren, um stattdessen die User.Identity.Name-Eigenschaft zu verwenden, und automatisch eine RSVP für den Host hinzufügen, der das Dinner erstellt:

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample4.cs)]

Da wir der Create ()-Methode ein [autorisiert]-Attribut hinzugefügt haben, stellt ASP.NET MVC sicher, dass die Aktionsmethode nur ausgeführt wird, wenn der Benutzer, der die/Dinners/Create-URL besucht, auf der Website angemeldet ist. Daher enthält der User.Identity.Name-Eigenschafts Wert immer einen gültigen Benutzernamen.

### <a name="using-the-useridentityname-property-when-editing-dinners"></a>Verwenden der User.Identity.Name-Eigenschaft beim Bearbeiten von Abendessen

Nun fügen wir eine Autorisierungs Logik hinzu, mit der Benutzer eingeschränkt werden, sodass Sie nur die Eigenschaften von Abendessen bearbeiten können, die Sie selbst Hosting.

Um dies zu unterstützen, fügen wir dem Dinner-Objekt (innerhalb der Dinner.cs partiellen Klasse, die wir zuvor erstellt haben) zuerst eine "ishostedby (username)"-Hilfsmethode hinzu. Diese Hilfsmethode gibt true oder false zurück, abhängig davon, ob ein angegebener Benutzername mit der Dinner-by-Eigenschaft übereinstimmt, und kapselt die erforderliche Logik, um einen Zeichen folgen Vergleich mit Berücksichtigung der Groß-/Kleinschreibung zu

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample5.cs)]

Anschließend fügen wir den Edit ()-Aktionsmethoden in unserer dinnerscontroller-Klasse ein Attribut [autorisieren] hinzu. Dadurch wird sichergestellt, dass Benutzer angemeldet sein müssen, um eine */Dinners/Edit/[ID]* -URL anzufordern.

Wir können dann den Bearbeitungsmethoden Code hinzufügen, der mithilfe der hilfsprogrammmethode Dinner. ishostedby (username) überprüft, ob der angemeldete Benutzer mit dem Dinner Host übereinstimmt. Wenn der Benutzer nicht der Host ist, wird eine "invalidowner"-Ansicht angezeigt, und die Anforderung wird beendet. Der Code hierfür sieht wie folgt aus:

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample6.cs)]

Klicken Sie mit der rechten Maustaste auf das Verzeichnis "\views\dinner", und wählen Sie den Menübefehl Add-&gt;View, um eine neue "invalidowner"-Ansicht zu erstellen. Wir füllen ihn mit der folgenden Fehlermeldung auf:

[!code-aspx[Main](secure-applications-using-authentication-and-authorization/samples/sample7.aspx)]

Wenn ein Benutzer nun versucht, ein Dinner zu bearbeiten, das er nicht besitzt, erhalten Sie eine Fehlermeldung:

![](secure-applications-using-authentication-and-authorization/_static/image7.png)

Wir können die gleichen Schritte für die Delete ()-Aktionsmethoden im Controller wiederholen, um die Berechtigung zum Löschen von Abendessen zu sperren, und sicherzustellen, dass nur der Host eines Dinner Sie löschen kann.

### <a name="showinghiding-edit-and-delete-links"></a>Ein-/Ausblenden von Links zum Bearbeiten und löschen

Wir verknüpfen mit der Edit and Delete-Aktionsmethode unserer dinnerscontroller-Klasse aus unserer Details-URL:

![](secure-applications-using-authentication-and-authorization/_static/image8.png)

Zurzeit zeigen wir die Links zum Bearbeiten und Löschen von Aktionen an, unabhängig davon, ob der Besucher der Detail-URL der Host des Dinner ist. Ändern wir dies so, dass die Verknüpfungen nur angezeigt werden, wenn der besuchte Benutzer der Besitzer des Dinner ist.

Die "Details ()"-Aktionsmethode in unserem "dinnerscontroller" Ruft ein Dinner-Objekt ab und übergibt es dann als Modell Objekt an unsere Ansichts Vorlage:

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample8.cs)]

Wir können unsere Ansichts Vorlage aktualisieren, um die Bearbeitungs-und Lösch Links bedingt anzuzeigen/auszublenden, indem wir die Dinner. ishostedby ()-Hilfsmethode wie im folgenden Beispiel verwenden:

[!code-aspx[Main](secure-applications-using-authentication-and-authorization/samples/sample9.aspx)]

#### <a name="next-steps"></a>Nächste Schritte

Sehen wir uns nun an, wie wir authentifizierten Benutzern die RSVP für Dinner mit AJAX ermöglichen können.

> [!div class="step-by-step"]
> [Zurück](implement-efficient-data-paging.md)
> [Weiter](use-ajax-to-deliver-dynamic-updates.md)
