---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/configuring-a-website-that-uses-application-services-vb
title: Konfigurieren einer Website, die Anwendungsdienste verwendet (VB) | Microsoft-Dokumentation
author: rick-anderson
description: In ASP.NET Version 2,0 wurde eine Reihe von Anwendungsdiensten eingeführt, die Teil der .NET Framework sind und als eine Suite von Baustein Diensten fungieren, die yo...
ms.author: riande
ms.date: 04/23/2009
ms.assetid: 9c31a42f-d8bb-4c0f-9ccc-597d4f70ac42
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/configuring-a-website-that-uses-application-services-vb
msc.type: authoredcontent
ms.openlocfilehash: 19e7258b558372259c7554a36c6ad73ce572dfa8
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78521655"
---
# <a name="configuring-a-website-that-uses-application-services-vb"></a>Konfigurieren einer Website mit Anwendungsdiensten (VB)

von [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Code herunterladen](https://download.microsoft.com/download/E/6/F/E6FE3A1F-EE3A-4119-989A-33D1A9F6F6DD/ASPNET_Hosting_Tutorial_09_VB.zip) oder [PDF herunterladen](https://download.microsoft.com/download/C/3/9/C391A649-B357-4A7B-BAA4-48C96871FEA6/aspnet_tutorial09_AppServicesConfig_vb.pdf)

> In ASP.NET Version 2,0 wurde eine Reihe von Anwendungsdiensten eingeführt, die Teil der .NET Framework sind und als eine Reihe von Baustein Diensten dienen, mit denen Sie Ihrer Webanwendung umfangreiche Funktionen hinzufügen können. In diesem Tutorial wird erläutert, wie Sie eine Website in der Produktionsumgebung für die Verwendung von Anwendungsdiensten konfigurieren und häufige Probleme beim Verwalten von Benutzerkonten und Rollen in der Produktionsumgebung beheben.

## <a name="introduction"></a>Einführung

In ASP.NET Version 2,0 wurde eine Reihe von *Anwendungsdiensten*eingeführt, die Teil der .NET Framework sind und als eine Reihe von Baustein Diensten dienen, mit denen Sie Ihrer Webanwendung umfangreiche Funktionen hinzufügen können. Die Anwendungsdienste umfassen Folgendes:

- **Mitgliedschaft** : eine API zum Erstellen und Verwalten von Benutzerkonten.
- **Rollen** : eine API zum Kategorisierung von Benutzern in Gruppen.
- **Profile** : eine API zum Speichern von benutzerdefiniertem, Benutzer spezifischem Inhalt.
- **Site Map** : eine API zum Definieren einer logischen Struktur einer Site in Form einer Hierarchie, die dann über Navigations Steuerelemente wie Menüs und Brotkrümel angezeigt werden kann.
- **Personalisierung** : eine API zum Verwalten von Anpassungs Einstellungen, die am häufigsten mit [*Webparts*](https://msdn.microsoft.com/library/e0s9t4ck.aspx)verwendet wird.
- System **Überwachung** : eine API zum Überwachen von Leistung, Sicherheit, Fehlern und anderen Systemintegritäts-Metriken für eine laufende Webanwendung.

Die Anwendungsdienste-APIs sind nicht an eine bestimmte Implementierung gebunden. Stattdessen weisen Sie die Anwendungsdienste an, einen bestimmten *Anbieter*zu verwenden, und dieser Anbieter implementiert den Dienst mithilfe einer bestimmten Technologie. Die am häufigsten verwendeten Anbieter für Internet basierte Webanwendungen, die auf einem Webhostingunternehmen gehostet werden, sind Anbieter, die eine SQL Server Datenbankimplementierung verwenden. Der `SqlMembershipProvider` ist beispielsweise ein Anbieter für die Mitgliedschafts-API, die Benutzerkontoinformationen in einer Microsoft SQL Server-Datenbank speichert.

Wenn Sie die Anwendungsdienste und SQL Server Anbieter verwenden, werden beim Bereitstellen der Anwendung einige Herausforderungen hinzugefügt. Zunächst müssen die Anwendungs Dienst-Datenbankobjekte für die Entwicklungs-und Produktionsdatenbanken ordnungsgemäß erstellt und entsprechend initialisiert werden. Es müssen auch wichtige Konfigurationseinstellungen vorgenommen werden.

> [!NOTE]
> Die Anwendungsdienste-APIs wurden mit dem [*Anbieter Modell*](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx)entworfen, einem Entwurfsmuster, das die Bereitstellung von API-Implementierungsdetails zur Laufzeit ermöglicht. Die .NET Framework wird mit einer Reihe von Anwendungs Dienstanbietern ausgeliefert, die verwendet werden können, wie z. b. die `SqlMembershipProvider` und `SqlRoleProvider`, die Anbieter für die Mitgliedschafts-und Rollen-APIs sind, die eine SQL Server Datenbankimplementierung verwenden. Sie können auch einen benutzerdefinierten Anbieter erstellen und einbinden. Tatsächlich enthält die Webanwendung "Book Reviews" bereits einen benutzerdefinierten Anbieter für die Site Map-API (`ReviewSiteMapProvider`), die die Site Übersicht aus den Daten in den Tabellen "`Genres`" und "`Books`" in der Datenbank erstellt.

In diesem Tutorial wird erläutert, wie ich die Webanwendung "Book Reviews" für die Verwendung der Mitgliedschafts-und Rollen-APIs erweitert habe. Anschließend wird die Bereitstellung einer Webanwendung erläutert, die Anwendungsdienste mit einer SQL Server-Datenbankimplementierung verwendet, und es wird durch die Behandlung allgemeiner Probleme bei der Verwaltung von Benutzerkonten und Rollen in der Produktionsumgebung erläutert.

## <a name="updates-to-the-book-reviews-application"></a>Aktualisierungen der Anwendung "Book Reviews"

In den letzten Lernprogrammen wurde das Buch Reviews-Webanwendung von einer statischen Website in eine dynamische, datengesteuerte Webanwendung mit einer Reihe von Verwaltungs Seiten für die Verwaltung von Genres und Reviews aktualisiert. Dieser Verwaltungsabschnitt ist jedoch zurzeit nicht geschützt, und jeder Benutzer, der die URL der Verwaltungsseite kennt (oder erraten) kann, kann auf unserer Website Überprüfungen erstellen, bearbeiten oder löschen. Eine gängige Methode, bestimmte Teile einer Website zu schützen, ist die Implementierung von Benutzerkonten und die anschließende Verwendung von URL-Autorisierungs Regeln, um den Zugriff auf bestimmte Benutzer oder Rollen einzuschränken. Die in diesem Tutorial zum Herunterladen verfügbare Webanwendung "Book Reviews" unterstützt Benutzerkonten und Rollen. Es ist eine einzige Rolle mit dem Namen admin definiert, und nur Benutzer mit dieser Rolle können auf die Verwaltungs Seiten zugreifen.

> [!NOTE]
> Ich habe drei Benutzerkonten in der "Book Reviews"-Webanwendung erstellt: Scott, jisun und Alice. Alle drei Benutzer haben dasselbe Kennwort: **Kennwort!** Scott und jisun sind in der Administrator Rolle, Alice nicht. Die nicht-Verwaltungs Seiten der Website sind für anonyme Benutzer weiterhin zugänglich. Das heißt, Sie müssen sich nicht mehr anmelden, um die Website zu besuchen, es sei denn, Sie möchten Sie verwalten. in diesem Fall müssen Sie sich als Benutzer in der Rolle "Administrator" anmelden.

Die Master Seite für die Buch Reviews-Anwendung wurde aktualisiert und enthält so eine andere Benutzeroberfläche für authentifizierte und anonyme Benutzer. Wenn ein anonymer Benutzer die Website besucht, wird ein Anmeldelink in der oberen rechten Ecke angezeigt. Einem authentifizierten Benutzer wird die Meldung "Willkommen zurück, *username*!" angezeigt. und einen Link zum Abmelden. Es gibt auch eine Anmeldeseite (`~/Login.aspx`), die ein Anmeldungs-websteuer Element enthält, das die Benutzeroberfläche und die Logik zum Authentifizieren eines Besuchers bereitstellt. Nur Administratoren können neue Konten erstellen. (Es gibt Seiten zum Erstellen und Verwalten von Benutzerkonten im Ordner "`~/Admin`".)

### <a name="configuring-the-membership-and-roles-apis"></a>Konfigurieren der Mitgliedschafts-und Rollen-APIs

Die Webanwendung Book Reviews verwendet die Mitgliedschafts-und Rollen-APIs, um Benutzerkonten zu unterstützen und diese Benutzer in Rollen zu gruppieren (nämlich als Administrator Rolle). Die `SqlMembershipProvider`-und `SqlRoleProvider` Provider-Klassen werden verwendet, da wir Konto-und Rollen Informationen in einer SQL Server-Datenbank speichern möchten.

> [!NOTE]
> Dieses Tutorial soll nicht als ausführliche Untersuchung der Konfiguration einer Webanwendung für die Unterstützung der Mitgliedschafts-und Rollen-APIs dienen. Ausführliche Informationen zu diesen APIs und den Schritten, die Sie ausführen müssen, um eine Website für deren Verwendung zu konfigurieren, finden Sie in den [*Tutorials zur Website Sicherheit*](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md).

Um die Anwendungsdienste mit einer SQL Server Datenbank zu verwenden, müssen Sie zuerst die Datenbankobjekte, die von diesen Anbietern verwendet werden, der Datenbank hinzufügen, in der das Benutzerkonto und die Rollen Informationen gespeichert werden sollen. Diese erforderlichen Datenbankobjekte umfassen eine Vielzahl von Tabellen, Sichten und gespeicherten Prozeduren. Sofern nicht anders angegeben, verwenden die Anbieter Klassen `SqlMembershipProvider` und `SqlRoleProvider` eine SQL Server Express Edition-Datenbank mit dem Namen `ASPNETDB`, die sich im `App_Data` Ordner der Anwendung befindet. Wenn eine solche Datenbank nicht vorhanden ist, wird Sie von diesen Anbietern zur Laufzeit automatisch mit den erforderlichen Datenbankobjekten erstellt.

Es ist möglich und in der Regel ideal, die Anwendungs Dienst-Datenbankobjekte in derselben Datenbank zu erstellen, in der die anwendungsspezifischen Daten der Website gespeichert sind. Der .NET Framework enthält ein Tool mit dem Namen `aspnet_regsql.exe`, mit dem die Datenbankobjekte in einer angegebenen Datenbank installiert werden. Ich habe dieses Tool verwendet, um diese Objekte der `Reviews.mdf` Datenbank im Ordner "`App_Data`" (der Entwicklungs Datenbank) hinzuzufügen. Wir werden später in diesem Tutorial erfahren, wie Sie dieses Tool verwenden, wenn wir diese Objekte der Produktionsdatenbank hinzufügen.

Wenn Sie die Anwendungs Dienst-Datenbankobjekte einer anderen Datenbank als `ASPNETDB` hinzufügen, müssen Sie die Konfigurationen `SqlMembershipProvider` und `SqlRoleProvider` Anbieter Klassen so anpassen, dass Sie die entsprechende Datenbank verwenden. Fügen Sie zum Anpassen des Mitgliedschafts Anbieters ein [ *&lt;Mitgliedschafts&gt; Element*](https://msdn.microsoft.com/library/1b9hw62f.aspx) innerhalb des `<system.web>` Abschnitts in `Web.config`; Verwenden Sie das [ *&gt;-Element&lt;roleManager*](https://msdn.microsoft.com/library/ms164660.aspx) , um den Rollen Anbieter zu konfigurieren. Der folgende Code Ausschnitt stammt aus der `Web.config` für die Buch Überprüfungen und zeigt die Konfigurationseinstellungen für die Mitgliedschafts-und Rollen-APIs. Beachten Sie, dass beide einen neuen Anbieter registrieren: `ReviewMembership` und `ReviewRole`, der die `SqlMembershipProvider`-und `SqlRoleProvider` Anbieter verwendet.

[!code-xml[Main](configuring-a-website-that-uses-application-services-vb/samples/sample1.xml)]

Das `Web.config` File s `<authentication>`-Element wurde ebenfalls für die Unterstützung der Formular basierten Authentifizierung konfiguriert.

[!code-xml[Main](configuring-a-website-that-uses-application-services-vb/samples/sample2.xml)]

### <a name="limiting-access-to-the-administration-pages"></a>Einschränken des Zugriffs auf die Verwaltungs Seiten

ASP.NET vereinfacht das gewähren oder Verweigern des Zugriffs auf eine bestimmte Datei oder einen bestimmten Ordner über das *URL-Autorisierungs* Feature durch einen Benutzer oder eine Rolle. (Die URL-Autorisierung wurde in den *wichtigsten Unterschieden zwischen IIS und dem ASP.NET Development Server* -Tutorial kurz erläutert, und es wurde gezeigt, wie IIS und die ASP.NET Development Server URL-Autorisierungs Regeln für statische und dynamische Inhalte anders anwenden.) Da der Zugriff auf den `~/Admin` Ordner außer den Benutzern in der Administrator Rolle untersagt werden soll, müssen wir diesem Ordner URL-Autorisierungs Regeln hinzufügen. Insbesondere müssen die URL-Autorisierungs Regeln Benutzern die Administrator Rolle gestatten und alle anderen Benutzer verweigern. Dies wird durch Hinzufügen einer `Web.config` Datei zum Ordner `~/Admin` mit folgendem Inhalt erreicht:

[!code-xml[Main](configuring-a-website-that-uses-application-services-vb/samples/sample3.xml)]

Weitere Informationen zum ASP.net s-URL-Autorisierungs Feature und dazu, wie Sie es verwenden, um Autorisierungs Regeln für Benutzer und Rollen zu benennen, finden Sie in den Tutorials zur [*benutzerbasierten Autorisierung*](../../older-versions-security/membership/user-based-authorization-vb.md) und [*rollenbasierten Autorisierung*](../../older-versions-security/roles/role-based-authorization-vb.md) aus den Lernprogrammen für meine [*Website Sicherheit*](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md).

## <a name="deploying-a-web-application-that-uses-application-services"></a>Bereitstellen einer Webanwendung, die Anwendungsdienste verwendet

Beim Bereitstellen einer Website, die Anwendungsdienste und einen Anbieter verwendet, der die Anwendungs Dienst Informationen in einer-Datenbank speichert, ist es zwingend erforderlich, dass die von den Anwendungsdiensten benötigten Datenbankobjekte in der Produktionsdatenbank erstellt werden. Anfänglich enthält die Produktionsdatenbank diese Objekte nicht, d. h., wenn die Anwendung zum ersten Mal bereitgestellt wird (oder wenn Sie nach dem Hinzufügen von Anwendungsdiensten zum ersten Mal bereitgestellt wird), müssen Sie zusätzliche Schritte ausführen, um diese erforderlichen Datenbankobjekte zu erhalten. Produktionsdatenbank.

Eine weitere Herausforderung kann auftreten, wenn Sie eine Website bereitstellen, die Anwendungsdienste verwendet, wenn Sie beabsichtigen, die in der Entwicklungsumgebung erstellten Benutzerkonten in der Produktionsumgebung zu replizieren. Abhängig von der Mitgliedschafts-und Rollen Konfiguration ist es möglich, dass selbst dann, wenn Sie die Benutzerkonten, die in der Entwicklungsumgebung erstellt wurden, erfolgreich in die Produktionsdatenbank kopiert wurde, diese Benutzer sich nicht bei der Webanwendung in der Produktion anmelden können. Wir sehen uns die Ursache für dieses Problem an und erörtern, wie Sie es verhindern können.

ASP.NET enthält ein nettes [*Websiteverwaltungs-Tool (WSAT)* ](https://msdn.microsoft.com/library/yy40ytx0.aspx) , das von Visual Studio aus gestartet werden kann und die Verwaltung von Benutzerkonten, Rollen und Autorisierungs Regeln über eine webbasierte Schnittstelle ermöglicht. Leider funktioniert das WSAT nur für lokale Websites. Dies bedeutet, dass es nicht zur Remote Verwaltung von Benutzerkonten, Rollen und Autorisierungs Regeln für die Webanwendung in der Produktionsumgebung verwendet werden kann. Wir sehen uns verschiedene Möglichkeiten zum Implementieren von WSAT-ähnlichen Verhalten von Ihrer Produktions Website an.

### <a name="adding-the-database-objects-using-aspnet_regsqlexe"></a>Hinzufügen der Datenbankobjekte mithilfe von ASPNET\_RegSql. exe

Im Tutorial zum Bereitstellen *einer Datenbank* wurde gezeigt, wie die Tabellen und Daten aus der Entwicklungs Datenbank in die Produktionsdatenbank kopiert werden. diese Techniken können auch verwendet werden, um die Anwendungs Dienst-Datenbankobjekte in die Produktionsdatenbank zu kopieren. Eine weitere Option ist das `aspnet_regsql.exe` Tool, mit dem die Anwendungs Dienst-Datenbankobjekte einer Datenbank hinzugefügt oder daraus entfernt werden.

> [!NOTE]
> Das `aspnet_regsql.exe` Tool erstellt die Datenbankobjekte in einer angegebenen Datenbank. Daten in diesen Datenbankobjekten werden von der Entwicklungs Datenbank nicht in die Produktionsdatenbank migriert. Wenn Sie das Benutzerkonto und die Rollen Informationen in der Entwicklungs Datenbank in die Produktionsdatenbank kopieren möchten, verwenden Sie die im Tutorial zum Bereitstellen *einer Datenbank* behandelten Verfahren.

Sehen Sie sich an, wie Sie die Datenbankobjekte mit dem `aspnet_regsql.exe` Tool der Produktionsdatenbank hinzufügen. Öffnen Sie zunächst Windows-Explorer, und navigieren Sie zum Verzeichnis .NET Framework Version 2,0 auf Ihrem Computer,% windir% \ Microsoft. NET\Framework\v2.0.50727. Dort finden Sie das `aspnet_regsql.exe` Tool. Dieses Tool kann über die Befehlszeile verwendet werden, enthält aber auch eine grafische Benutzeroberfläche. Doppelklicken Sie auf die Datei `aspnet_regsql.exe`, um die grafische Komponente zu starten.

Das Tool beginnt mit dem Anzeigen eines Begrüßungs Bildschirms, in dem der Zweck erläutert wird. Klicken Sie auf Weiter, um zum Bildschirm "Setup Option auswählen" zu gelangen, der in Abbildung 1 dargestellt wird. Von hier aus können Sie die Anwendungs Dienst-Datenbankobjekte hinzufügen oder aus einer Datenbank entfernen. Da wir diese Objekte der Produktionsdatenbank hinzufügen möchten, wählen Sie die Option "SQL Server für Anwendungsdienste konfigurieren" aus, und klicken Sie auf Weiter.

[![konfigurieren Sie SQL Server für Anwendungsdienste](configuring-a-website-that-uses-application-services-vb/_static/image2.jpg)](configuring-a-website-that-uses-application-services-vb/_static/image1.jpg)

**Abbildung 1**: Konfigurieren der SQL Server für Anwendungsdienste ([Klicken Sie, um das Bild in voller Größe anzuzeigen](configuring-a-website-that-uses-application-services-vb/_static/image3.jpg))

Auf der Seite "Server und Datenbank auswählen" werden Informationen zur Eingabe von Informationen für die Verbindung mit der Datenbank angezeigt. Geben Sie den Datenbankserver, die Sicherheits Anmelde Informationen und den von Ihrem Webhostingunternehmen bereitgestellten Datenbanknamen ein, und klicken Sie auf Weiter.

> [!NOTE]
> Nachdem Sie den Datenbankserver und die Anmelde Informationen eingegeben haben, erhalten Sie möglicherweise eine Fehlermeldung beim Erweitern der Dropdown Liste Datenbank. Das `aspnet_regsql.exe` Tool fragt die `sysdatabases` Systemtabelle ab, um eine Liste der Datenbanken auf dem Server abzurufen, aber einige Webhostingunternehmen Sperren Ihre Datenbankserver, sodass diese Informationen nicht öffentlich verfügbar sind. Wenn Sie diesen Fehler erhalten, können Sie den Datenbanknamen direkt in die Dropdown Liste eingeben.

[![Bereitstellen des Tools mit den Verbindungsinformationen Ihrer Datenbank](configuring-a-website-that-uses-application-services-vb/_static/image5.jpg)](configuring-a-website-that-uses-application-services-vb/_static/image4.jpg)

**Abbildung 2**: Bereitstellen des Tools mit den Verbindungsinformationen Ihrer Datenbank ([Klicken Sie, um das Bild in voller Größe anzuzeigen](configuring-a-website-that-uses-application-services-vb/_static/image6.jpg))

Auf dem nachfolgenden Bildschirm werden die Aktionen zusammengefasst, die durchgeführt werden müssen, nämlich, dass die Anwendungs Dienst-Datenbankobjekte der angegebenen Datenbank hinzugefügt werden. Klicken Sie auf Weiter, um diese Aktion abzuschließen. Nach einigen Augenblicken wird der letzte Bildschirm angezeigt, der darauf hinweist, dass die Datenbankobjekte hinzugefügt wurden (siehe Abbildung 3).

[![Erfolg! Die Anwendungsdienste-Datenbankobjekte wurden der Produktionsdatenbank hinzugefügt.](configuring-a-website-that-uses-application-services-vb/_static/image8.jpg)](configuring-a-website-that-uses-application-services-vb/_static/image7.jpg)

**Abbildung 3**: Erfolg! Die Anwendungsdienste Datenbankobjekte wurden der Produktionsdatenbank hinzugefügt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](configuring-a-website-that-uses-application-services-vb/_static/image9.jpg)).

Öffnen Sie SQL Server Management Studio, und stellen Sie eine Verbindung mit der Produktionsdatenbank her, um zu überprüfen, ob die Anwendungs Dienst-Datenbankobjekte der Produktionsdatenbank erfolgreich hinzugefügt Wie in Abbildung 4 gezeigt, sollten die Anwendungs Dienst-Datenbanktabellen nun in Ihrer Datenbank, `aspnet_Applications`, `aspnet_Membership`, `aspnet_Users`usw. angezeigt werden.

[![bestätigen, dass die Datenbankobjekte der Produktionsdatenbank hinzugefügt wurden.](configuring-a-website-that-uses-application-services-vb/_static/image11.jpg)](configuring-a-website-that-uses-application-services-vb/_static/image10.jpg)

**Abbildung 4**: sicherstellen, dass die Datenbankobjekte der Produktionsdatenbank hinzugefügt wurden ([Klicken Sie, um das Bild in voller Größe anzuzeigen](configuring-a-website-that-uses-application-services-vb/_static/image12.jpg))

Sie müssen das `aspnet_regsql.exe` Tool nur verwenden, wenn Sie Ihre Webanwendung zum ersten Mal oder zum ersten Mal verwenden, nachdem Sie die Anwendungsdienste verwendet haben. Sobald sich diese Datenbankobjekte in der Produktionsdatenbank befinden, müssen Sie nicht erneut hinzugefügt oder geändert werden.

### <a name="copying-user-accounts-from-development-to-production"></a>Kopieren von Benutzerkonten aus der Entwicklung in die Produktion

Wenn Sie die `SqlMembershipProvider`-und `SqlRoleProvider` Anbieter Klassen verwenden, um die Anwendungs Dienst Informationen in einer SQL Server-Datenbank zu speichern, werden das Benutzerkonto und die Rollen Informationen in einer Vielzahl von Datenbanktabellen gespeichert, einschließlich `aspnet_Users`, `aspnet_Membership`, `aspnet_Roles`und `aspnet_UsersInRoles`. Wenn Sie während der Entwicklung Benutzerkonten in der Entwicklungsumgebung erstellen, können Sie diese Benutzerkonten in der Produktionsumgebung replizieren, indem Sie die entsprechenden Datensätze aus den entsprechenden Datenbanktabellen kopieren. Wenn Sie den Datenbankveröffentlichungs-Assistenten zum Bereitstellen von Anwendungs Dienst-Datenbankobjekten verwendet haben, haben Sie möglicherweise auch die Datensätze kopiert, was dazu führen würde, dass die Benutzerkonten, die in der Entwicklung erstellt werden, auch in der Produktion Abhängig von Ihren Konfigurationseinstellungen können Sie jedoch feststellen, dass die Benutzer, deren Konten in der Entwicklung erstellt und in die Produktion kopiert wurden, sich nicht auf der Produktions Website anmelden können. Woran liegt das?

Die `SqlMembershipProvider`-und `SqlRoleProvider` Anbieter Klassen wurden so entworfen, dass eine einzelne Datenbank als Benutzerspeicher für mehrere Anwendungen fungieren könnte, wobei jede Anwendung theoretisch Benutzer mit überlappenden Benutzernamen und Rollen mit dem gleichen Namen haben könnte. Um diese Flexibilität zu ermöglichen, verwaltet die Datenbank eine Liste der Anwendungen in der `aspnet_Applications` Tabelle, und jeder Benutzer ist einer dieser Anwendungen zugeordnet. Die `aspnet_Users` Tabelle verfügt insbesondere über eine `ApplicationId` Spalte, die jeden Benutzer mit einem Datensatz in der `aspnet_Applications` Tabelle verknüpft.

Neben der Spalte `ApplicationId` enthält die `aspnet_Applications` Tabelle auch eine `ApplicationName` Spalte, die einen benutzerfreundlicheren Namen für die Anwendung bereitstellt. Wenn eine Website versucht, mit einem Benutzerkonto zu arbeiten, wie z. b. das Überprüfen der Anmelde Informationen eines Benutzers auf der Anmeldeseite, muss die `SqlMembershipProvider` Klasse angeben, mit welcher Anwendung gearbeitet werden soll. Dies erfolgt in der Regel durch Bereitstellen des Anwendungs namens, und der Wert stammt aus der Konfiguration des Anbieters in `Web.config`, insbesondere über das `applicationName`-Attribut.

Was geschieht jedoch, wenn das `applicationName`-Attribut in `Web.config`nicht angegeben ist? In einem solchen Fall verwendet das Mitgliedschaftssystem den Anwendungs Stamm Pfad als `applicationName` Wert. Wenn das `applicationName`-Attribut in `Web.config`nicht explizit festgelegt ist, besteht die Möglichkeit, dass die Entwicklungsumgebung und die Produktionsumgebung einen anderen Anwendungs Stamm verwenden und daher verschiedenen Anwendungsnamen in den Anwendungsdiensten zugeordnet werden. Wenn dies nicht der Fall ist, verfügen die Benutzer, die in der Entwicklungsumgebung erstellt werden, über einen `ApplicationId` Wert, der nicht mit dem `ApplicationId` Wert für die Produktionsumgebung übereinstimmt. Das Ergebnis ist, dass sich diese Benutzer nicht anmelden können.

> [!NOTE]
> Wenn Sie sich in dieser Situation befinden, wenn Benutzerkonten mit einem nicht übereinstimmenden `ApplicationId` Wert in die Produktion kopiert werden, können Sie eine Abfrage schreiben, um diese falschen `ApplicationId` Werte auf die `ApplicationId` zu aktualisieren, die in der Produktionsumgebung verwendet werden. Nach der Aktualisierung können sich die Benutzer, deren Konten in der Entwicklungsumgebung erstellt wurden, jetzt bei der Webanwendung in der Produktion anmelden.

Die gute Nachricht ist, dass es einen einfachen Schritt gibt, den Sie ausführen können, um sicherzustellen, dass die beiden Umgebungen denselben `ApplicationId` verwenden. Legen Sie das `applicationName`-Attribut in `Web.config` für alle Ihre Anwendungs Dienstanbieter explizit fest. Das `applicationName`-Attribut wird in den `<membership>`-und `<roleManager>`-Elementen explizit auf "BookReviews" festgelegt, wie in diesem Ausschnitt aus `Web.config` angezeigt.

[!code-xml[Main](configuring-a-website-that-uses-application-services-vb/samples/sample4.xml)]

Weitere Informationen zum Festlegen des `applicationName` Attributs und der zugrunde liegenden Begründung finden Sie im Blogbeitrag von [*Scott Guthrie*](https://weblogs.asp.net/scottgu/) s. [*legen Sie die ApplicationName-Eigenschaft immer fest, wenn Sie die ASP.NET-Mitgliedschaft und andere Anbieter konfigurieren*](https://weblogs.asp.net/scottgu/443634).

### <a name="managing-user-accounts-in-the-production-environment"></a>Verwalten von Benutzerkonten in der Produktionsumgebung

Das ASP.net Web Site Administration Tool (WSAT) vereinfacht das Erstellen und Verwalten von Benutzerkonten, das definieren und Anwenden von Rollen und das Buchstabieren von Benutzer-und rollenbasierten Autorisierungs Regeln. Sie können das WSAT von Visual Studio aus starten, indem Sie zum Projektmappen-Explorer navigieren und auf das Konfigurations Symbol "ASP.net" klicken, oder indem Sie zu den Menüs "Website" oder "Projekt" navigieren und das Menü Element "ASP.net Configuration" auswählen Leider kann das WSAT nur mit lokalen Websites funktionieren. Aus diesem Grund können Sie das WSAT nicht auf Ihrer Arbeitsstation verwenden, um die Website in der Produktionsumgebung zu verwalten.

Die gute Nachricht ist, dass die gesamte Funktionalität, die vom WSAT verfügbar gemacht wird, Programm gesteuert über die APIs für Mitgliedschaften und Rollen verfügbar ist. Außerdem verwenden viele der WSAT-Bildschirme die standardmäßigen ASP.NET-Steuerelemente für die Anmeldung. Kurz gesagt: Sie können Ihrer Website ASP.NET Seiten hinzufügen, die die erforderlichen Verwaltungsfunktionen bieten.

Beachten Sie, dass ein früheres Tutorial die Webanwendung Book Reviews so aktualisiert hat, dass Sie einen `~/Admin` Ordner enthält. dieser Ordner wurde so konfiguriert, dass nur Benutzer in der Administrator Rolle zugelassen werden. Ich fügte dem Ordner eine Seite mit dem Namen `CreateAccount.aspx` hinzu, von der ein Administrator ein neues Benutzerkonto erstellen kann. Auf dieser Seite werden die Benutzeroberfläche und die Back-End-Logik zum Erstellen eines neuen Benutzerkontos mithilfe des Steuer Elements "kreateuserwizard" angezeigt. Weitere Informationen: Ich habe das Steuerelement so angepasst, dass es ein Kontrollkästchen enthält, das auffordert, ob der neue Benutzer auch der Administrator Rolle hinzugefügt werden soll (siehe Abbildung 5). Mit etwas Arbeit können Sie einen benutzerdefinierten Satz von Seiten erstellen, der die Aufgaben für Benutzer-und Rollen Verwaltung implementiert, die andernfalls von WSAT bereitgestellt werden.

> [!NOTE]
> Weitere Informationen zur Verwendung der Mitgliedschafts-und Rollen-APIs zusammen mit den Anmelde bezogenen ASP.net-websteuer Elementen finden Sie in den Lernprogrammen für die [*Website Sicherheit*](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md). Weitere Informationen zum Anpassen des Steuer Elements "kreateuserwizard" [*finden Sie in*](http://www.erichpeterson.com/) den Tutorials [*Erstellen von Benutzerkonten*](../../older-versions-security/membership/creating-user-accounts-vb.md) und [*Speichern zusätzlicher Benutzerinformationen*](../../older-versions-security/membership/storing-additional-user-information-vb.md) . Sie können auch den Artikel zum [*Anpassen des Steuer Elements "kreateuserwizard*](http://aspnet.4guysfromrolla.com/articles/070506-1.aspx)" ansehen.

[![Administratoren können neue Benutzerkonten erstellen.](configuring-a-website-that-uses-application-services-vb/_static/image14.jpg)](configuring-a-website-that-uses-application-services-vb/_static/image13.jpg)

**Abbildung 5**: Administratoren können neue Benutzerkonten erstellen ([Klicken Sie, um das Bild in voller Größe anzuzeigen](configuring-a-website-that-uses-application-services-vb/_static/image15.jpg))

Wenn Sie die vollständige Funktionalität von WSAT benötigen, sehen Sie sich das [*Rollout Ihres eigenen Websiteverwaltungs-Tools*](http://aspnet.4guysfromrolla.com/articles/052307-1.aspx)an, in dem Autor Dan Clem den Prozess der Erstellung eines benutzerdefinierten WSAT-ähnlichen Tools erläutert. Dan gibt den Quellcode der Anwendung (in C#) frei und bietet schrittweise Anleitungen zum Hinzufügen des Codes zu ihrer gehosteten Website.

## <a name="summary"></a>Zusammenfassung

Beim Bereitstellen einer Webanwendung, die die Anwendungs Dienst-Datenbankimplementierung verwendet, müssen Sie zunächst sicherstellen, dass die Produktionsdatenbank über die erforderlichen Datenbankobjekte verfügt Diese Objekte können mithilfe der Techniken hinzugefügt werden, die im Tutorial zum Bereitstellen *einer Datenbank* erläutert werden. Alternativ können Sie das `aspnet_regsql.exe` Tool verwenden, wie in diesem Tutorial gezeigt. Andere Herausforderungen, die wir im Zusammenhang mit der Synchronisierung des in den Entwicklungs-und Produktionsumgebungen verwendeten Anwendungs namensfest gestellt haben (Dies ist wichtig, wenn Benutzer und Rollen, die in der Entwicklungsumgebung erstellt werden, in der Produktion gültig sein soll) und Techniken für Verwalten der Benutzer und Rollen in der Produktionsumgebung.

Fröhliche Programmierung!

### <a name="further-reading"></a>Weitere nützliche Informationen

Weitere Informationen zu den in diesem Tutorial behandelten Themen finden Sie in den folgenden Ressourcen:

- [*ASP.NET SQL Server Registrierungs Tool (Aspnet_regsql. exe)* ](https://msdn.microsoft.com/library/ms229862.aspx)
- [*Erstellen der Anwendungsdienste Datenbank für SQL Server*](https://msdn.microsoft.com/library/x28wfk74.aspx)
- [*Erstellen des Mitgliedschafts Schemas in SQL Server*](../../older-versions-security/membership/creating-the-membership-schema-in-sql-server-vb.md)
- [*Überprüfen der ASP.net s-Mitgliedschaft,-Rollen und-profile*](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx)
- [*Parallele Websiteverwaltungs-Tools*](http://aspnet.4guysfromrolla.com/articles/052307-1.aspx)
- [*Tutorials zur Website Sicherheit*](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md)
- [*Übersicht über das Websiteverwaltungs-Tool*](https://msdn.microsoft.com/library/yy40ytx0.aspx)

> [!div class="step-by-step"]
> [Zurück](configuring-the-production-web-application-to-use-the-production-database-vb.md)
> [Weiter](strategies-for-database-development-and-deployment-vb.md)
