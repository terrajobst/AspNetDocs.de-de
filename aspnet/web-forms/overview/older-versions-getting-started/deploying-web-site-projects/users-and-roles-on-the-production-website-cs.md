---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/users-and-roles-on-the-production-website-cs
title: Benutzer und Rollen auf der Produktions Website (C#) | Microsoft-Dokumentation
author: rick-anderson
description: Das ASP.NET-Website-Verwaltungs Tool (WSAT) stellt eine webbasierte Benutzeroberfläche zum Konfigurieren von Mitgliedschafts-und Rollen Einstellungen sowie zum Erstellen, bearbeiten,
ms.author: riande
ms.date: 06/09/2009
ms.assetid: dbc54313-5d05-4285-98b3-726edea6d0c9
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/users-and-roles-on-the-production-website-cs
msc.type: authoredcontent
ms.openlocfilehash: c47bd2c1661f129dd8856916de04b8ba459fbfec
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2019
ms.locfileid: "74616567"
---
# <a name="users-and-roles-on-the-production-website-c"></a>Benutzer und Rollen auf der Produktions Website (C#)

von [Scott Mitchell](https://twitter.com/ScottOnWriting)

[PDF herunterladen](https://download.microsoft.com/download/5/C/5/5C57DB8C-5DEA-4B3A-92CA-4405544D313B/aspnet_tutorial16_CustomAWAT_cs.pdf)

> Das ASP.NET-Website-Verwaltungs Tool (WSAT) stellt eine webbasierte Benutzeroberfläche zum Konfigurieren von Mitgliedschafts-und Rollen Einstellungen sowie zum Erstellen, bearbeiten und Löschen von Benutzern und Rollen bereit. Leider funktioniert das WSAT nur beim Besuch von localhost. Dies bedeutet, dass Sie das Verwaltungs Tool der Produktions Website nicht über Ihren Browser erreichen können. Die gute Nachricht ist, dass es Problem Umgehungen gibt, die es ermöglichen, Benutzer und Rollen in der Produktionsumgebung zu verwalten. In diesem Tutorial werden diese Problem Umgehungen und andere behandelt.

## <a name="introduction"></a>Einführung

ASP.NET 2,0 hat eine Reihe von *Anwendungsdiensten*eingeführt, bei denen es sich um eine Sammlung von Baustein Diensten handelt, die Sie Ihrer Webanwendung hinzufügen können. Die Mitgliedschafts-und Rollen Dienste wurden der Book Reviews-Website im Tutorial [ *Konfigurieren einer Website, die Anwendungsdienste verwendet,* ](configuring-a-website-that-uses-application-services-cs.md)hinzugefügt. Der Mitgliedschafts Dienst erleichtert das Erstellen und Verwalten von Benutzerkonten. der Rollen Dienst bietet eine API für die Kategorisierung von Benutzern in Gruppen. Die Book Reviews-Website verfügt über drei Benutzerkonten: Scott, jisun und Alice und eine einzige Rolle, admin, mit Scott und jisun in der Administrator Rolle.

ASP. Die Anwendungsdienste von net sind nicht an eine bestimmte Implementierung gebunden. Stattdessen weisen Sie die Anwendungsdienste an, einen bestimmten *Anbieter*zu verwenden, und dieser Anbieter implementiert den Dienst mithilfe einer bestimmten Technologie. Wir haben die Webanwendung Book Reviews so konfiguriert, dass Sie die `SqlMembershipProvider`-und `SqlRoleProvider` Anbieter für die Mitgliedschafts-und Rollen Dienste verwendet. Diese beiden Anbieter speichern Benutzerkonto-und Rollen Informationen in einer SQL Server Datenbank und sind die am häufigsten verwendeten Anbieter für Internet basierte Webanwendungen, die in einem Webhostingunternehmen gehostet werden.

Eine gängige Herausforderung für Entwickler, die die Mitgliedschafts-und Rollen Dienste verwenden, ist die Verwaltung der Benutzer und Rollen in der Produktionsumgebung. Wie können Sie ein Benutzerkonto aus der Produktions Website löschen, eine neue Rolle hinzufügen oder einen vorhandenen Benutzer zu einer vorhandenen Rolle hinzufügen? In diesem Tutorial werden verschiedene Verfahren zum Verwalten von Benutzern und Rollen auf der Produktions Website erläutert.

## <a name="using-the-aspnet-web-site-administration-tool"></a>Verwenden des ASP.NET-Websiteverwaltungs-Tools

ASP.NET enthält ein [Websiteverwaltungs-Tool](https://msdn.microsoft.com/library/yy40ytx0.aspx) (WSAT), das das Erstellen und Verwalten von Benutzerkonten und-Rollen sowie die Angabe von Benutzer-und rollenbasierten Autorisierungs Regeln erleichtert. Um das WSAT zu verwenden, klicken Sie auf das ASP.NET-Konfigurations Symbol in der Projektmappen-Explorer, oder wechseln Sie zum Menü Website oder Projekt, und wählen Sie die Konfigurationsoption ASP.net aus. Beide Ansätze starten einen Webbrowser und weisen ihn auf die WSAT an einer Adresse wie den folgenden hin: `http://localhost:portNumber/asp.netwebadminfiles/default.aspx?applicationPhysicalPath=pathToApplication`

Das WSAT ist in drei Abschnitte unterteilt:

- **Sicherheit** : Verwalten von Benutzern, Rollen und Autorisierungs Regeln.
- **ApplicationConfiguration** : Verwalten Sie die &lt;appSettings-&gt; und die SMTP-Einstellungen hier. Sie können die Anwendung auch offline schalten und debuggingeinstellungen und Ablauf Verfolgungs Einstellungen verwalten und die Standardseite für benutzerdefinierte Fehler angeben.
- **Providerconfiguration** : Konfigurieren Sie die Anbieter, die von den Anwendungsdiensten verwendet werden.

Der Abschnitt "Sicherheit" (siehe **Abbildung 1**) enthält Links zum Erstellen neuer Benutzer, zum Verwalten von Benutzern, zum Erstellen und Verwalten von Rollen sowie zum Erstellen und Verwalten von Zugriffsregeln. Hier können Sie dem System eine neue Rolle hinzufügen, einen vorhandenen Benutzer löschen oder Rollen zu einem bestimmten Benutzerkonto hinzufügen oder daraus entfernen.

[![](users-and-roles-on-the-production-website-cs/_static/image2.png)](users-and-roles-on-the-production-website-cs/_static/image1.png)

**Abbildung 1**: der Abschnitt "WSAT-Sicherheit" enthält Optionen zum Verwalten von Benutzern und Rollen.  
([Klicken Sie, um das Bild in voller Größe anzuzeigen](users-and-roles-on-the-production-website-cs/_static/image3.png))

Leider ist die WSAT-Datei nur lokal zugänglich. Sie können das WSAT nicht auf Ihrer remoteproduktionswebsite besuchen. Wenn Sie `www.yoursite.com/asp.netwebadminfiles/default.aspx` besuchen Sie die Antwort "404 nicht gefunden". Der Code, der das WSAT unterstützt, verwendet die Klassen `Membership` und `Roles` in der .NET Framework, um Benutzer und Rollen zu erstellen, zu bearbeiten und zu löschen. Diese Klassen überprüfen die Konfigurationsinformationen der Webanwendung, um zu bestimmen, welcher Anbieter verwendet werden soll. wieder im Tutorial [ *Konfigurieren einer Website, die Anwendungsdienste verwendet,* ](configuring-a-website-that-uses-application-services-cs.md) wird die Website "Book Reviews" für die Verwendung der `SqlMembershipProvider`-und `SqlRoleProvider`-Anbieter eingerichtet. Dies umfasste das Hinzufügen von `<membership>`-und `<roleManager>` Abschnitten zu `Web.config`.

[!code-xml[Main](users-and-roles-on-the-production-website-cs/samples/sample1.xml)]

Beachten Sie, dass die Abschnitte "`<membership>`" und "`<roleManager>`" auf die `SqlMembershipProvider`-und `SqlRoleProvider` Anbieter in Ihrem `type`-Attribut verweisen. Diese Anbieter speichern die Benutzer-und Rollen Informationen in einer angegebenen SQL Server Datenbank. Die von diesen Anbietern verwendete Datenbank wird durch das `connectionStringName`-Attribut `ReviewsConnectionString`angegeben, das in der `~/ConfigSections/databaseConnectionStrings.config`-Datei definiert ist. Beachten Sie, dass die `databaseConnectionStrings.config`-Datei in der Entwicklungsumgebung die Verbindungs Zeichenfolge für die Entwicklungs Datenbank enthält, während die `databaseConnectionStrings.config` Datei in der Produktionsumgebung die Verbindungs Zeichenfolge für die Produktionsdatenbank enthält.

Kurz gesagt, der Zugriff auf das WSAT muss lokal über die Entwicklungsumgebung erfolgen, und es funktioniert mit den Benutzer-und Rollen Informationen in der Datenbank, die in der `databaseConnectionStrings.config`-Datei angegeben ist. Wenn die Verbindungs Zeichenfolgen-Informationen in der `databaseConnectionStrings.config`-Datei in der Entwicklungsumgebung geändert werden, können wir die WSAT-Datei lokal zum Verwalten von Benutzern und Rollen in der Produktionsumgebung verwenden.

Um diese Funktionalität zu veranschaulichen, öffnen Sie die Datei `databaseConnectionStrings.config` in Visual Studio in der Entwicklungsumgebung, und ersetzen Sie die Verbindungs Zeichenfolge der Entwicklungs Datenbank durch die Verbindungs Zeichenfolge der Produktionsdatenbank. Starten Sie dann das WSAT, wechseln Sie zur Registerkarte Sicherheit, und fügen Sie einen neuen Benutzer mit dem Namen Sam mit dem Kennwort "Password!" (weniger Anführungszeichen). **Abbildung 2** zeigt den WSAT-Bildschirm beim Erstellen dieses Kontos.

[![](users-and-roles-on-the-production-website-cs/_static/image5.png)](users-and-roles-on-the-production-website-cs/_static/image4.png)

**Abbildung 2**: Erstellen eines neuen Benutzers namens Sam in der Produktionsumgebung  
([Klicken Sie, um das Bild in voller Größe anzuzeigen](users-and-roles-on-the-production-website-cs/_static/image6.png))

Da wir die Verbindungs Zeichenfolge in `databaseConnectionStrings.config` geändert haben, um auf den Produktionsdaten Bankserver zu verweisen, wurde Sam in der Produktionsumgebung als Benutzer hinzugefügt. Um dies zu überprüfen, ändern Sie die Verbindungs Zeichenfolge in der `databaseConnectionStrings.config`-Datei zurück in die Entwicklungs Datenbank, und besuchen Sie dann die `Login.aspx` Seite in der Entwicklungsumgebung. Versuchen Sie, sich als Sam anzumelden (siehe **Abbildung 3**).

[![](users-and-roles-on-the-production-website-cs/_static/image8.png)](users-and-roles-on-the-production-website-cs/_static/image7.png)

**Abbildung 3**: Sie können sich in der Entwicklungsumgebung nicht als Sam anmelden.  
([Klicken Sie, um das Bild in voller Größe anzuzeigen](users-and-roles-on-the-production-website-cs/_static/image9.png))

Sie können sich in der Entwicklungsumgebung nicht als Sam anmelden, da die Benutzerkontoinformationen in der lokalen Datenbank nicht vorhanden sind. Stattdessen wurde der Produktionsdatenbank hinzugefügt. Um dies zu überprüfen, zeigen Sie den Inhalt der `aspnet_Users` Tabelle sowohl in der Entwicklungs-als auch in der Produktionsdatenbank an. In der Entwicklungsumgebung sollten nur drei Datensätze für Benutzer Scott, jisun und Alice vorhanden sein. Die `aspnet_Users` Tabelle in der Produktionsdatenbank enthält jedoch vier Datensätze: Scott, jisun, Alice und Sam. Folglich kann Sam sich über die Website in der Produktionsumgebung, aber nicht über die Entwicklungsumgebung anmelden.

[![](users-and-roles-on-the-production-website-cs/_static/image11.png)](users-and-roles-on-the-production-website-cs/_static/image10.png)

**Abbildung 4**: Sam kann sich auf der Produktions Website anmelden  
([Klicken Sie, um das Bild in voller Größe anzuzeigen](users-and-roles-on-the-production-website-cs/_static/image12.png))

> [!NOTE]
> Vergessen Sie nicht, die Verbindungs Zeichenfolge in der `databaseConnectionStrings.config`-Datei wieder in die Verbindungs Zeichenfolge der Entwicklungs Datenbank zu ändern, wenn Sie mit dem WSAT gearbeitet haben. andernfalls arbeiten Sie mit Produktionsdaten, wenn Sie den Standort über die Entwicklungsumgebung testen. Beachten Sie auch, dass wir mit dem soeben erläuterten Verfahren die Verwendung von WSAT zur Remote Verwaltung von Benutzern und Rollen, Änderungen an anderen WSAT-Konfigurationsoptionen (Zugriffsregeln, SMTP-Einstellungen, Debug-und Ablauf Verfolgungs Einstellungen usw.) in der `Web.config` Datei ändern können. Folglich gelten alle Änderungen, die an den Einstellungen vorgenommen werden, für die Entwicklungsumgebung und nicht für die Produktionsumgebung.

## <a name="creating-custom-user-and-role-management-web-pages"></a>Erstellen von benutzerdefinierten Webseiten für Benutzer-und Rollen Verwaltung

Das WSAT stellt ein Standardsystem zum Verwalten von Benutzern und Rollen bereit, kann jedoch nur lokal gestartet werden und erfordert Änderungen an den Verbindungs Zeichenfolgen-Informationen, um die Benutzer und Rollen in der Produktionsumgebung zu verwalten. Die meisten Websites, die Benutzerkonten unterstützen, enthalten auch eine Reihe von Webseiten zur Benutzer-und Rollen Verwaltung, mit denen Administratoren Benutzer und Rollen von Seiten innerhalb der Website aus verwalten können. Solche webbasierten Verwaltungs Seiten vereinfachen die Verwaltung von Benutzern und Rollen und sind für Websites, bei denen es möglicherweise viele Administratoren oder Administratoren gibt, die keinen Zugriff auf oder den technischen Hintergrund haben, für die Verwendung von Visual Studio zum Starten von WSAT.

ASP.NET enthält eine Reihe integrierter, Anmelde bezogener websteuer Elemente, mit denen viele dieser administrativen Webseiten so einfach wie Drag & Drop implementiert werden können. Beispielsweise können Sie eine Seite für Administratoren erstellen, um ein neues Benutzerkonto zu erstellen, indem Sie das Steuerelement "kreateuserwizard" auf die Seite ziehen und einige Eigenschaften festlegen. Tatsächlich wird auf der Seite zum Erstellen von Benutzern in der in **Abbildung 2** dargestellten WSAT dasselbe Steuerelement "kreateuserwizard" verwendet, das Sie Ihren Seiten hinzufügen können. Außerdem sind die Funktionen der Mitgliedschafts-und Rollen Dienste Programm gesteuert über die `Membership`-und `Roles` Klassen im .NET Framework verfügbar. Mit diesen Klassen können Sie Code schreiben, um Benutzer und Rollen zu erstellen, zu bearbeiten und zu löschen sowie um Benutzer zu Rollen hinzuzufügen oder zu entfernen, um zu bestimmen, welche Benutzer welche Rollen haben, und um andere Benutzer-und Rollen bezogene Aufgaben auszuführen.

Im Tutorial [ *Konfigurieren einer Website, die Anwendungsdienste verwendet,* ](configuring-a-website-that-uses-application-services-cs.md) habe ich dem Ordner `Admin` eine Seite mit dem Namen `CreateAccount.aspx`hinzugefügt. Auf dieser Seite kann ein Administrator ein neues Benutzerkonto zur Website hinzufügen und angeben, ob der neu erstellte Benutzer in der Rolle "Administrator" ist (siehe **Abbildung 5**).

[![](users-and-roles-on-the-production-website-cs/_static/image14.png)](users-and-roles-on-the-production-website-cs/_static/image13.png)

**Abbildung 5**: Administratoren können neue Benutzerkonten erstellen.  
([Klicken Sie, um das Bild in voller Größe anzuzeigen](users-and-roles-on-the-production-website-cs/_static/image15.png))

Eine ausführlichere Betrachtung der Entwicklung von Benutzer-und Rollen Verwaltungs Seiten sowie Schritt-für-Schritt-Anweisungen zur Verwendung der `Membership`-und `Roles` Klassen und der Anmelde bezogenen ASP.net-websteuer Elemente finden Sie unter meine [Website-Sicherheits](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md)Lernprogramme. Hier finden Sie Anleitungen zum Erstellen von Webseiten zum Erstellen neuer Konten, zum Erstellen und Verwalten von Rollen, zum Zuweisen von Benutzern zu Rollen und anderen allgemeinen Verwaltungsaufgaben.

Um WSAT-ähnliche Funktionen auf der Produktions Website zu implementieren, können Sie immer eine eigene Reihe von Webseiten erstellen, mit denen die Funktionen von WSAT implementiert werden. Informationen zu den ersten Schritten finden Sie im WSAT-Quellcode, der sich im Ordner `%WINDIR%\Microsoft.NET\Framework\v2.0.50727\ASP.NETWebAdminFiles`befindet. Eine andere Möglichkeit besteht darin, die WSAT-Alternative von Dan Clem zu verwenden, die er in seinem Artikel freigibt und ein [eigenes Websiteverwaltungs-Tool](http://aspnet.4guysfromrolla.com/articles/052307-1.aspx)bereitstellt. Dan führt Sie durch die Schritte zum Entwickeln eines benutzerdefinierten WSAT-ähnlichen Tools, enthält den Quellcode der Anwendung für den Download ( C#in) und enthält schrittweise Anleitungen zum Hinzufügen seines benutzerdefinierten WSAT zu einer gehosteten Website.

## <a name="summary"></a>Summary

Das ASP.net Web Site Administration Tool (WSAT) kann zusammen mit den Mitgliedschafts-und Rollen Anwendungsdiensten verwendet werden, um Benutzer-und Rollen Informationen für Ihre Website zu verwalten. Leider ist das WSAT nur lokal zugänglich und kann nicht von Ihrer Produktions Website besucht werden. Wenn Sie jedoch die Verbindungs Zeichenfolge in der Entwicklungsumgebung ändern, sodass Sie auf die Produktionsdatenbank verweist, können Sie die Benutzer und Rollen auf der Produktions Website mithilfe von WSAT verwalten.

Während der WSAT-Ansatz eine schnelle und einfache Möglichkeit zur Verwaltung von Benutzern und Rollen bietet, ist es erforderlich, die WSAT von Visual Studio aus und temporäre Änderungen an den Verbindungs Zeichenfolgen-Informationen zu starten. Das WSAT bietet eine schnelle Möglichkeit zum Verwalten von Benutzern und Rollen in der Produktionsumgebung, ist jedoch mühsam und funktioniert nicht gut für Websites mit mehreren Administratoren oder Administratoren, die Visual Studio und das WSAT nicht kennen oder nicht kennen. Aus diesen Gründen enthalten die meisten Websites, die Benutzerkonten unterstützen, eine Gruppe administrativer Webseiten. Eine solche Gruppe von Webseiten entfällt auf den Bedarf an WSAT und wird von verschiedenen Administratoren von einem beliebigen Computer verwendet.

Fröhliche Programmierung!

### <a name="further-reading"></a>Weiterführende Themen

Weitere Informationen zu den in diesem Tutorial behandelten Themen finden Sie in den folgenden Ressourcen:

- [Untersuchen von ASP. Netzwerk Mitgliedschaft, Rollen und Profil](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx)
- [Parallele Websiteverwaltungs-Tools](http://aspnet.4guysfromrolla.com/articles/052307-1.aspx)
- [Übersicht über das Websiteverwaltungs-Tool](https://msdn.microsoft.com/library/yy40ytx0.aspx)
- [Tutorials zur Website Sicherheit](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md)

> [!div class="step-by-step"]
> [Zurück](precompiling-your-website-cs.md)
> [Weiter](asp-net-hosting-options-vb.md)
