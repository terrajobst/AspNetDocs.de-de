---
uid: web-forms/overview/older-versions-security/membership/creating-user-accounts-vb
title: Erstellen von Benutzerkonten (VB) | Microsoft-Dokumentation
author: rick-anderson
description: In diesem Tutorial erfahren Sie, wie Sie mithilfe des Mitgliedschafts-Frameworks (über den sqlmembership shipprovider) neue Benutzerkonten erstellen. Wir sehen, wie neue USA erstellt werden...
ms.author: riande
ms.date: 01/18/2008
ms.assetid: 9ef3e893-bebe-4b13-9fe5-8b71720dd85e
msc.legacyurl: /web-forms/overview/older-versions-security/membership/creating-user-accounts-vb
msc.type: authoredcontent
ms.openlocfilehash: 01be198c329f372ddcd529ad8a369f2d3426a9fc
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78474285"
---
# <a name="creating-user-accounts-vb"></a>Hinzufügen von Benutzerkonten (VB)

von [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Code herunterladen](https://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/ASPNET_Security_Tutorial_05_VB.zip) oder [PDF herunterladen](https://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/aspnet_tutorial05_CreatingUsers_vb.pdf)

> In diesem Tutorial erfahren Sie, wie Sie mithilfe des Mitgliedschafts-Frameworks (über den sqlmembership shipprovider) neue Benutzerkonten erstellen. Wir werden Ihnen zeigen, wie neue Benutzerprogramm gesteuert und über ASP erstellt werden. Das integrierte Steuerelement "kreateuserwizard" von net.

## <a name="introduction"></a>Einführung

<a id="_msoanchor_1"> </a>Im [vorherigen Tutorial](creating-the-membership-schema-in-sql-server-vb.md) haben wir das Anwendungs Dienst Schema in einer Datenbank installiert, mit der die vom `SqlMembershipProvider` und `SqlRoleProvider`benötigten Tabellen, Sichten und gespeicherten Prozeduren hinzugefügt wurden. Dadurch wurde die Infrastruktur erstellt, die für die restlichen Tutorials in dieser Reihe benötigt wird. In diesem Tutorial erfahren Sie, wie Sie mit dem Mitgliedschafts Framework (über das `SqlMembershipProvider`) neue Benutzerkonten erstellen. Wir werden Ihnen zeigen, wie neue Benutzerprogramm gesteuert und über ASP erstellt werden. Das integrierte Steuerelement "kreateuserwizard" von net.

Zusätzlich zu wissen, wie Sie neue Benutzerkonten erstellen, es müssen auch die Demo-Website aktualisieren wir zuerst erstellt, in haben der *<a id="_msoanchor_2"></a>[eine Übersicht der Formularauthentifizierung](../introduction/an-overview-of-forms-authentication-vb.md)* Lernprogramm, und klicken Sie dann in verbessert die *<a id="_msoanchor_3"></a>[Konfiguration der Formularauthentifizierung und erweiterte Themen](../introduction/forms-authentication-configuration-and-advanced-topics-vb.md)* Lernprogramm. Unsere demowebanwendung verfügt über eine Anmeldeseite, mit der die Benutzer Anmelde Informationen anhand von hart codierten Benutzernamen/Kennwort-Paaren überprüft werden. Darüber hinaus enthält `Global.asax` Code, mit dem benutzerdefinierte `IPrincipal` und `IIdentity` Objekte für authentifizierte Benutzer erstellt werden. Wir aktualisieren die Anmeldeseite, um die Benutzer Anmelde Informationen anhand des Mitgliedschafts-Framework zu überprüfen und die benutzerdefinierte Prinzipal-und Identitätslogik zu entfernen.

Erste Schritte

## <a name="the-forms-authentication-and-membership-checklist"></a>Prüfliste zur Formular Authentifizierung und Mitgliedschaft

Bevor wir mit dem Mitgliedschafts Framework arbeiten, nehmen wir uns einen Moment Zeit, um diese Punkte zu erreichen. Wenn Sie das Mitgliedschafts Framework mit dem `SqlMembershipProvider` in einem Formular basierten Authentifizierungs Szenario verwenden, müssen die folgenden Schritte ausgeführt werden, bevor Sie die Mitgliedschafts Funktionen in der Webanwendung implementieren:

1. **Aktivieren der Formular basierten Authentifizierung.** Wie in erläutert eine *<a id="_msoanchor_4"></a>[Übersicht der Formularauthentifizierung](../introduction/an-overview-of-forms-authentication-vb.md)* , Formularauthentifizierung wird aktiviert, indem Sie die Bearbeitung `Web.config` verwendet wird und die `<authentication>` Elements `mode` -Attribut `Forms`. Bei aktivierter Formular Authentifizierung wird jede eingehende Anforderung auf ein *Formular Authentifizierungs Ticket*überprüft, das, falls vorhanden, den Anforderer identifiziert.
2. **Fügen Sie das Anwendungs Dienst Schema der entsprechenden Datenbank hinzu.** Wenn Sie die `SqlMembershipProvider`, müssen wir das Anwendungs Dienst Schema in einer-Datenbank installieren. Normalerweise wird dieses Schema der Datenbank hinzugefügt, die das Datenmodell der Anwendung enthält. Die *<a id="_msoanchor_5"></a>[erstellen das Schema für die Mitgliedschaft in SQL Server](creating-the-membership-schema-in-sql-server-vb.md)* Lernprogramm erläutert, mit der `aspnet_regsql.exe` Tool, um dies zu erreichen.
3. **Passen Sie die Einstellungen der Webanwendung so an, dass auf die Datenbank aus Schritt 2 verwiesen wird** Das Tutorial *Erstellen des Mitgliedschafts Schemas in SQL Server* hat zwei Möglichkeiten zum Konfigurieren der Webanwendung aufgezeigt, damit die `SqlMembershipProvider` die in Schritt 2: durch Ändern des `LocalSqlServer` Verbindungs Zeichenfolgen-namens verwendete Datenbank verwenden würde. oder durch Hinzufügen eines neuen registrierten Anbieters zur Liste der Mitgliedschafts Framework-Anbieter und Anpassen des neuen Anbieters zur Verwendung der Datenbank aus Schritt 2.

Wenn Sie eine Webanwendung entwickeln, die die `SqlMembershipProvider` und die Formular basierte Authentifizierung verwendet, müssen Sie diese drei Schritte ausführen, bevor Sie die `Membership`-Klasse oder die ASP.net-Anmeldungs-websteuer Elemente verwenden. Da wir diese Schritte bereits in vorherigen Tutorials ausgeführt haben, können wir mit der Verwendung des Mitgliedschafts Frameworks beginnen!

## <a name="step-1-adding-new-aspnet-pages"></a>Schritt 1: Hinzufügen neuer ASP.NET-Seiten

In diesem Tutorial und den nächsten drei werden verschiedene Mitgliedschafts bezogene Funktionen und Funktionen untersucht. Wir benötigen eine Reihe von ASP.NET-Seiten, um die in diesen Tutorials untersuchten Themen zu implementieren. Nun erstellen wir diese Seiten und dann eine Site Übersichts Datei `(Web.sitemap)`.

Erstellen Sie zunächst einen neuen Ordner in dem Projekt mit dem Namen `Membership`. Fügen Sie als nächstes fünf neue ASP.NET-Seiten zum Ordner "`Membership`" hinzu, wobei jede Seite mit der `Site.master` Master Seite verknüpft wird. Benennen Sie die Seiten:

- `CreatingUserAccounts.aspx`
- `UserBasedAuthorization.aspx`
- `EnhancedCreateUserWizard.aspx`
- `AdditionalUserInfo.aspx`
- `Guestbook.aspx`

An diesem Punkt sollte die Projektmappen-Explorer des Projekts ähnlich wie der Screenshot in Abbildung 1 aussehen.

[dem Mitgliedschafts Ordner wurden ![fünf neue Seiten hinzugefügt.](creating-user-accounts-vb/_static/image2.png)](creating-user-accounts-vb/_static/image1.png)

**Abbildung 1**: Dem Ordner "`Membership`" wurden fünf neue Seiten hinzugefügt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](creating-user-accounts-vb/_static/image3.png)).

Jede Seite sollte an diesem Punkt über die beiden Inhalts Steuerelemente verfügen, eine für jede der Inhalts Platzhalter der Master Seite: `MainContent` und `LoginContent`.

[!code-aspx[Main](creating-user-accounts-vb/samples/sample1.aspx)]

Beachten Sie, dass das Standard Markup des `LoginContent` contentplachalter einen Link zum Anmelden oder Abmelden der Website anzeigt, je nachdem, ob der Benutzer authentifiziert ist. Wenn das `Content2` Content-Steuerelement vorhanden ist, wird jedoch das Standard Markup der Master Seite überschrieben. Wie in erläutert *<a id="_msoanchor_6"></a>[eine Übersicht der Formularauthentifizierung](../introduction/an-overview-of-forms-authentication-vb.md)* Lernprogramm, dies ist hilfreich bei Seiten, in denen es sollten keine Anmeldeoptionen in der linken Spalte anzuzeigen.

Für diese fünf Seiten möchten wir jedoch das Standard Markup der Master Seite für den `LoginContent` contentplachalter anzeigen. Entfernen Sie deshalb das deklarative Markup für das `Content2` Content-Steuerelement. Anschließend sollte jedes Markup der fünf Seite nur ein Inhalts Steuerelement enthalten.

## <a name="step-2-creating-the-site-map"></a>Schritt 2: Erstellen der Site Übersicht

Alle außer den trivialen Websites müssen eine Form einer Navigations Benutzeroberfläche implementieren. Bei der Navigations Benutzeroberfläche kann es sich um eine einfache Liste mit Links zu den verschiedenen Abschnitten der Site handeln. Alternativ können diese Links in Menüs oder Struktur Ansichten angeordnet werden. Als Seiten Entwickler ist das Erstellen der Navigations Benutzeroberfläche nur die Hälfte der Story. Außerdem benötigen wir einige Möglichkeiten, die logische Struktur der Site in einer wart baren und aktualisierbaren Weise zu definieren. Wenn neue Seiten hinzugefügt oder vorhandene Seiten entfernt werden, möchten wir in der Lage sein, eine einzelne Quelle (die Site Übersicht) zu aktualisieren und diese Änderungen auf der Navigations Benutzeroberfläche der Website widerzuspiegeln.

Diese beiden Aufgaben: Definieren der Site Übersicht und Implementieren einer Navigations Benutzeroberfläche basierend auf der Site Übersicht: Dank des Site Übersichts Frameworks und der in ASP.NET-Version 2,0 hinzugefügten navigationsweb-Steuerelemente leicht zu erreichen. Mithilfe des Site Übersichts Frameworks kann ein Entwickler eine Site Übersicht definieren und dann über eine programmgesteuerte API (die`SiteMap`- [Klasse](https://msdn.microsoft.com/library/system.web.sitemap.aspx)) darauf zugreifen. Die integrierten Navigations-websteuer Elemente enthalten ein [Menü Steuer](https://msdn.microsoft.com/library/bz09dy46.aspx)Element, das [TreeView-Steuer](https://msdn.microsoft.com/library/3eafky27.aspx)Element und das [SiteMapPath-Steuer](https://msdn.microsoft.com/library/3eafky27.aspx)Element.

Wie das Framework für Mitgliedschaften und Rollen wird das Site Übersichts Framework über das [Anbieter Modell](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx)erstellt. Der Auftrag der Site Übersichts Anbieter-Klasse besteht darin, die in-Memory-Struktur zu generieren, die von der `SiteMap`-Klasse aus einem persistenten Datenspeicher verwendet wird, z. b. einer XML-Datei oder einer Datenbanktabelle. Der .NET Framework wird mit einem standardmäßigen Site Übersichts Anbieter geliefert, der die Site Übersichts Daten aus einer XML-Datei liest ([`XmlSiteMapProvider`](https://msdn.microsoft.com/library/system.web.xmlsitemapprovider.aspx)). Dies ist der Anbieter, den wir in diesem Tutorial verwenden werden. Informationen zu einigen Alternativen Site Übersichts Anbieter-Implementierungen finden Sie im Abschnitt Weitere Messwerte am Ende dieses Tutorials.

Der Standard-Site Übersichts Anbieter erwartet, dass eine ordnungsgemäß formatierte XML-Datei mit dem Namen `Web.sitemap` das Stammverzeichnis vorhanden ist. Da wir diesen Standardanbieter verwenden, müssen wir eine solche Datei hinzufügen und die Struktur der Site Map im entsprechenden XML-Format definieren. Klicken Sie zum Hinzufügen der Datei in Projektmappen-Explorer mit der rechten Maustaste auf den Projektnamen, und wählen Sie neues Element hinzufügen aus. Wählen Sie im Dialogfeld die Option zum Hinzufügen einer Datei vom Typ Site Map mit dem Namen `Web.sitemap`.

[![dem Stammverzeichnis des Projekts eine Datei mit dem Namen "Web. Sitemap" hinzufügen](creating-user-accounts-vb/_static/image5.png)](creating-user-accounts-vb/_static/image4.png)

**Abbildung 2**: Fügen Sie dem Stammverzeichnis des Projekts eine Datei mit dem Namen `Web.sitemap` hinzu ([Klicken Sie, um das Bild in voller Größe anzuzeigen](creating-user-accounts-vb/_static/image6.png)).

Die XML-Site Zuordnungs Datei definiert die Struktur der Website als Hierarchie. Diese hierarchische Beziehung wird in der XML-Datei über die Herkunft der `<siteMapNode>` Elemente modelliert. Der `Web.sitemap` muss mit einem übergeordneten Knoten `<siteMap>` beginnen, der genau ein `<siteMapNode>` untergeordnetes Element aufweist. Dieses `<siteMapNode>` Element der obersten Ebene stellt den Stamm der Hierarchie dar und kann über eine beliebige Anzahl von untergeordneten Knoten verfügen. Jedes `<siteMapNode>` Element muss ein `title` Attribut enthalten und unter anderem optional `url`-und `description` Attribute enthalten. jedes nicht leere `url` Attribut muss eindeutig sein.

Geben Sie den folgenden XML-Code in die `Web.sitemap` Datei ein:

[!code-xml[Main](creating-user-accounts-vb/samples/sample2.xml)]

Das obige Site Übersichts Markup definiert die Hierarchie, die in Abbildung 3 dargestellt ist.

[![Site Map eine hierarchische Navigationsstruktur darstellt](creating-user-accounts-vb/_static/image8.png)](creating-user-accounts-vb/_static/image7.png)

**Abbildung 3**: Die Site Übersicht stellt eine hierarchische Navigationsstruktur dar ([Klicken Sie, um das Bild in voller Größe anzuzeigen](creating-user-accounts-vb/_static/image9.png)).

## <a name="step-3-updating-the-master-page-to-include-a-navigational-user-interface"></a>Schritt 3: Aktualisieren der Master Seite, um eine navigationale Benutzeroberfläche einzuschließen

ASP.NET enthält eine Reihe von Navigations bezogenen websteuer Elementen zum Entwerfen einer Benutzeroberfläche. Hierzu gehören das Menü, TreeView und die SiteMapPath-Steuerelemente. Das Menü-und TreeView-Steuerelement wird die Struktur der Site Map in einem Menü bzw. in einer Struktur darstellen, während von SiteMapPath ein Breadcrumb-Element angezeigt wird, das den aktuellen Knoten und seine Vorgänger anzeigt. Die Site Übersichts Daten können mithilfe von SiteMapDataSource an andere datenweb Steuerelemente gebunden werden, und Sie können Programm gesteuert über die `SiteMap`-Klasse aufgerufen werden.

Da ist eine ausführliche Beschreibung der Siteübersicht-Framework und die Navigationssteuerelemente Gegenstand dieser Reihe von Lernprogrammen, sondern als eingesparten Zeit, die wir Erstellen von eigenen Benutzeroberfläche für die Navigation stattdessen leihen verwendet meinem *[ Arbeiten mit Daten in ASP.NET 2.0](../../data-access/index.md)* Reihe von Lernprogrammen, die Wiederholungsmodul-Steuerelement verwendet eine zwei-Deep Aufzählung der Navigationslinks, anzeigen, wie in Abbildung 4 dargestellt.

### <a name="adding-a-two-level-list-of-links-in-the-left-column"></a>Hinzufügen einer Liste mit Links auf zwei Ebenen in der linken Spalte

Um diese Schnittstelle zu erstellen, fügen Sie der linken Spalte der `Site.master` Master Seite das folgende deklarative Markup hinzu, in dem der Text TODO: Das Menü wird hier angezeigt... befindet sich zurzeit.

[!code-aspx[Main](creating-user-accounts-vb/samples/sample3.aspx)]

Das obige Markup bindet ein Repeater-Steuerelement mit dem Namen `menu` an eine SiteMapDataSource, die die in `Web.sitemap`definierte Site Übersichts Hierarchie zurückgibt. Da die [`ShowStartingNode`-Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sitemapdatasource.showstartingnode.aspx) des SiteMapDataSource-Steuer Elements auf false festgelegt ist, wird die Hierarchie der Site Map zurückgegeben, beginnend mit den Nachfolger Elementen des Home-Knotens. Der Repeater zeigt jeden dieser Knoten (derzeit nur die Mitgliedschaft) in einem `<li>`-Element an. Ein anderer, innerer Repeater zeigt dann die untergeordneten Elemente des aktuellen Knotens in einer geschachtelten ungeordneten Liste an.

Abbildung 4 zeigt die Ausgabe des obigen Markups mit der in Schritt 2 erstellten Site Map-Struktur. Der Repeater rendert das ungeordnete Listen Markup der Vanille. die in `Styles.css` definierten Cascading Stylesheet-Regeln sind für das ästhetisch ansprechende Layout verantwortlich. Eine ausführlichere Beschreibung der Funktionsweise des obigen Markups finden Sie im Tutorial zu [Master Seiten und zur Website Navigation](https://asp.net/learn/data-access/tutorial-03-vb.aspx) .

[![die Navigations Benutzeroberfläche mithilfe von nicht geordneten Listen gerendert wird.](creating-user-accounts-vb/_static/image11.png)](creating-user-accounts-vb/_static/image10.png)

**Abbildung 4**: Die Navigations Benutzeroberfläche wird mithilfe von nicht geordneten Listen gerendert ([Klicken Sie, um das Bild in voller Größe anzuzeigen](creating-user-accounts-vb/_static/image12.png)).

### <a name="adding-breadcrumb-navigation"></a>Hinzufügen der Breadcrumb-Navigation

Zusätzlich zur Liste der Links in der linken Spalte soll auch jede Seite einen [Breadcrumb](http://en.wikipedia.org/wiki/Breadcrumb_%28navigation%29)anzeigen. Breadcrumb ist ein Element der Navigations Benutzeroberfläche, mit dem Benutzer schnell Ihre aktuelle Position in der Website Hierarchie anzeigen. Das SiteMapPath-Steuerelement verwendet das Site Map-Framework, um den Speicherort der aktuellen Seite in der Site Übersicht zu ermitteln, und zeigt dann basierend auf diesen Informationen einen Breadcrumb an.

Fügen Sie dem Header `<div>`-Element der Master Seite insbesondere ein `<span>`-Element hinzu, und legen Sie das `class`-Attribut des neuen `<span>` Elements auf Breadcrumb fest. (Die `Styles.css` Klasse enthält eine Regel für eine Breadcrumb-Klasse.) Fügen Sie als nächstes ein SiteMapPath zu diesem neuen `<span>` Element hinzu.

[!code-aspx[Main](creating-user-accounts-vb/samples/sample4.aspx)]

Abbildung 5 zeigt die Ausgabe von SiteMapPath beim Besuch `~/Membership/CreatingUserAccounts.aspx`.

[![Breadcrumb zeigt die aktuelle Seite und deren Vorgänger in der Site Übersicht an.](creating-user-accounts-vb/_static/image14.png)](creating-user-accounts-vb/_static/image13.png)

**Abbildung 5**: Breadcrumb zeigt die aktuelle Seite und deren Vorgänger in der Site Übersicht an ([Klicken Sie, um das Bild in voller Größe anzuzeigen](creating-user-accounts-vb/_static/image15.png))

## <a name="step-4-removing-the-custom-principal-and-identity-logic"></a>Schritt 4: Entfernen der benutzerdefinierten Prinzipal-und Identitätslogik

In der *<a id="_msoanchor_7"></a>[Konfiguration der Formularauthentifizierung und erweiterte Themen](../introduction/forms-authentication-configuration-and-advanced-topics-vb.md)* Lernprogramm wurde erläutert, wie benutzerdefinierte Haupt- und Identitätsobjekte Objekte dem authentifizierten Benutzer verknüpfen. Dies erreichen Sie, indem Sie in `Global.asax` für das `PostAuthenticateRequest` Ereignis der Anwendung einen Ereignishandler erstellen, der ausgelöst wird, nachdem der Benutzer vom `FormsAuthenticationModule` authentifiziert wurde. In diesem Ereignishandler haben wir die `GenericPrincipal`-und `FormsIdentity` Objekte, die durch den `FormsAuthenticationModule` hinzugefügt wurden, durch die in diesem Tutorial erstellten `CustomPrincipal`-und `CustomIdentity` Objekte ersetzt.

Obwohl benutzerdefinierte Prinzipal-und Identitäts Objekte in bestimmten Szenarien nützlich sind, sind die `GenericPrincipal`-und `FormsIdentity` Objekte in den meisten Fällen ausreichend. Folglich wäre es sinnvoll, zum Standardverhalten zurückzukehren. Nehmen Sie diese Änderung vor, indem Sie entweder den `PostAuthenticateRequest` Ereignishandler entfernen oder auskommentieren oder die `Global.asax` Datei vollständig löschen.

> [!NOTE]
> Nachdem Sie den Code in `Global.asax`kommentiert oder entfernt haben, müssen Sie den Code in `Default.aspx's` Code Behind-Klasse auskommentieren, die die `User.Identity`-Eigenschaft in eine `CustomIdentity`-Instanz umwandelt.

## <a name="step-5-programmatically-creating-a-new-user"></a>Schritt 5: Programm gesteuertes Erstellen eines neuen Benutzers

Um ein neues Benutzerkonto über das Mitgliedschafts Framework zu erstellen, verwenden Sie die [`CreateUser`-Methode](https://msdn.microsoft.com/library/system.web.security.membership.createuser.aspx)der `Membership`-Klasse. Diese Methode verfügt über Eingabeparameter für Benutzername, Kennwort und andere Benutzer bezogene Felder. Beim Aufruf delegiert Sie die Erstellung des neuen Benutzerkontos an den konfigurierten Mitgliedschafts Anbieter und gibt dann ein [`MembershipUser` Objekt](https://msdn.microsoft.com/library/system.web.security.membershipuser.aspx) zurück, das das soeben erstellte Benutzerkonto darstellt.

Die `CreateUser`-Methode verfügt über vier über Ladungen, die jeweils eine andere Anzahl von Eingabe Parametern akzeptieren:

- [`CreateUser(username, password)`](https://msdn.microsoft.com/library/d8t4h2es.aspx)
- [`CreateUser(username, password, email)`](https://msdn.microsoft.com/library/t8yy6w3h.aspx)
- [`CreateUser(username, password, email, passwordQuestion, passwordAnswer, isApproved, MembershipCreateStatus)`](https://msdn.microsoft.com/library/82xx2e62.aspx)
- [`CreateUser(username, password, email, passwordQuestion, passwordAnswer, isApproved, providerUserKey, MembershipCreateStatus)`](https://msdn.microsoft.com/library/ms152012.aspx)

Diese vier über Ladungen unterscheiden sich von der Menge der gesammelten Informationen. Die erste Überladung erfordert z. b. nur den Benutzernamen und das Kennwort für das neue Benutzerkonto, während die zweite Überladung auch die e-Mail-Adresse des Benutzers erfordert.

Diese über Ladungen sind vorhanden, da die zum Erstellen eines neuen Benutzerkontos benötigten Informationen von den Konfigurationseinstellungen des Mitgliedschafts Anbieters abhängen. In der *<a id="_msoanchor_8"></a>[erstellen das Schema für die Mitgliedschaft in SQL Server](creating-the-membership-schema-in-sql-server-vb.md)* Angabe Mitgliedschaft Konfigurationseinstellungen im untersucht Lernprogramm `Web.config`. Tabelle 2 enthält eine komplette Liste der Konfigurationseinstellungen.

Eine solche Konfigurationseinstellung für den Mitgliedschafts Anbieter, die sich darauf auswirkt, welche `CreateUser` Überladungen verwendet werden können, ist die `requiresQuestionAndAnswer` Einstellung. Wenn `requiresQuestionAndAnswer` auf `true` (Standardeinstellung) festgelegt ist, müssen Sie beim Erstellen eines neuen Benutzerkontos eine Sicherheitsfrage und-Antwort angeben. Diese Informationen werden später verwendet, wenn der Benutzer sein Kennwort zurücksetzen oder ändern muss. Insbesondere wird zu diesem Zeitpunkt die Sicherheitsfrage angezeigt, und Sie müssen die richtige Antwort eingeben, um Ihr Kennwort zurückzusetzen oder zu ändern. Wenn die `requiresQuestionAndAnswer` auf festgelegt ist, führt das Aufrufen einer der beiden ersten `CreateUser` Überladungen daher `true` zu einer Ausnahme, da die Sicherheitsfrage und-Antwort fehlen. Da unsere Anwendung zurzeit so konfiguriert ist, dass eine Sicherheitsfrage und-Antwort erforderlich ist, müssen wir eine der beiden letzten über Ladungen verwenden, wenn die Benutzerprogramm gesteuert erstellt werden.

Um die Verwendung der `CreateUser`-Methode zu veranschaulichen, erstellen wir eine Benutzeroberfläche, bei der der Benutzer zur Eingabe von Name, Kennwort, e-Mail und Antwort auf eine vordefinierte Sicherheitsfrage aufgefordert wird. Öffnen Sie die Seite `CreatingUserAccounts.aspx` im Ordner `Membership`, und fügen Sie dem Content-Steuerelement die folgenden websteuer Elemente hinzu:

- Ein Textfeld mit dem Namen `Username`
- Ein Textfeld mit dem Namen `Password`, dessen `TextMode`-Eigenschaft auf festgelegt ist `Password`
- Ein Textfeld mit dem Namen `Email`
- Eine Bezeichnung mit dem Namen `SecurityQuestion`, deren `Text` Eigenschaft gelöscht wurde.
- Ein Textfeld mit dem Namen `SecurityAnswer`
- Eine Schaltfläche mit dem Namen `CreateAccountButton`, deren `Text` Eigenschaft zum Erstellen des Benutzerkontos festgelegt ist.
- Ein Bezeichnung-Steuerelement mit dem Namen `CreateAccountResults`, dessen `Text` Eigenschaft gelöscht wurde.

An diesem Punkt sollte der Bildschirm in etwa wie in Abbildung 6 gezeigt aussehen.

[![die verschiedenen websteuer Elemente zur Seite "Seite" von "" ""](creating-user-accounts-vb/_static/image17.png)](creating-user-accounts-vb/_static/image16.png)

**Abbildung 6**: Fügen Sie die verschiedenen websteuer Elemente zum `CreatingUserAccounts.aspx Page` hinzu ([Klicken Sie, um das Bild in voller Größe anzuzeigen](creating-user-accounts-vb/_static/image18.png)).

Die `SecurityQuestion` Bezeichnung und `SecurityAnswer` Textfeld sollen eine vordefinierte Sicherheitsfrage anzeigen und die Benutzer Antwort erfassen. Beachten Sie, dass die Sicherheitsfrage und-Antwort auf Benutzerbasis gespeichert werden. es ist daher möglich, dass jeder Benutzer seine eigene Sicherheitsfrage definieren kann. In diesem Beispiel habe ich mich jedoch entschieden, eine universelle Sicherheitsfrage zu verwenden, nämlich: Welche Farbe ist Ihre Lieblingsfarbe?

Um diese vordefinierte Sicherheitsfrage zu implementieren, fügen Sie eine Konstante zur Code-Behind-Klasse der Seite mit dem Namen "`passwordQuestion`" hinzu, und weisen Sie Ihr die Sicherheitsfrage zu. Weisen Sie dann im Ereignishandler für `Page_Load` diese Konstante der `Text`-Eigenschaft der `SecurityQuestion` Bezeichnung zu:

[!code-vb[Main](creating-user-accounts-vb/samples/sample5.vb)]

Erstellen Sie als nächstes einen Ereignishandler für das `CreateAccountButton'` s `Click`-Ereignis, und fügen Sie den folgenden Code hinzu:

[!code-vb[Main](creating-user-accounts-vb/samples/sample6.vb)]

Der `Click` Ereignishandler beginnt mit dem Definieren einer Variablen mit dem Namen `createStatus` vom Typ [`MembershipCreateStatus`](https://msdn.microsoft.com/library/system.web.security.membershipcreatestatus.aspx). `MembershipCreateStatus` ist eine Enumeration, die den Status des `CreateUser` Vorgangs angibt. Wenn das Benutzerkonto z. b. erfolgreich erstellt wurde, wird die resultierende `MembershipCreateStatus` Instanz auf den Wert `Success;` festgelegt. wenn der Vorgang fehlschlägt, weil bereits ein Benutzer mit demselben Benutzernamen vorhanden ist, wird er auf den Wert `DuplicateUserName`festgelegt. In der von uns verwendeten `CreateUser` Überladung müssen wir eine `MembershipCreateStatus` Instanz an die-Methode übergeben. Dieser Parameter ist auf den entsprechenden Wert in der `CreateUser`-Methode festgelegt, und wir können den Wert nach dem Methoden Aufrufwert überprüfen, um zu bestimmen, ob das Benutzerkonto erfolgreich erstellt wurde.

Nachdem Sie `CreateUser`aufgerufen haben und `createStatus`übergeben, wird eine `Select Case`-Anweisung verwendet, um abhängig vom Wert, der `createStatus`zugewiesen ist, eine entsprechende Meldung auszugeben. In Abbildung 7 wird die Ausgabe angezeigt, wenn ein neuer Benutzer erfolgreich erstellt wurde. Die Abbildungen 8 und 9 zeigen die Ausgabe an, wenn das Benutzerkonto nicht erstellt wurde. In Abbildung 8 hat der Besucher ein Kennwort mit fünf Buchstaben eingegeben, das die Anforderungen an die Kenn Wort Stärke nicht erfüllt, die in den Konfigurationseinstellungen des Mitgliedschafts Anbieters aufgeführt sind. In Abbildung 9 versucht der Besucher, ein Benutzerkonto mit einem vorhandenen Benutzernamen (dem in Abbildung 7 erstellten Benutzernamen) zu erstellen.

[![ein neues Benutzerkonto erfolgreich erstellt wurde](creating-user-accounts-vb/_static/image20.png)](creating-user-accounts-vb/_static/image19.png)

**Abbildung 7**: Ein neues Benutzerkonto wurde erfolgreich erstellt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](creating-user-accounts-vb/_static/image21.png))

[![wird das Benutzerkonto nicht erstellt, weil das angegebene Kennwort zu schwach ist.](creating-user-accounts-vb/_static/image23.png)](creating-user-accounts-vb/_static/image22.png)

**Abbildung 8**: Das Benutzerkonto wird nicht erstellt, weil das angegebene Kennwort zu schwach ist ([Klicken Sie, um das Bild in voller Größe anzuzeigen](creating-user-accounts-vb/_static/image24.png)).

[![wird das Benutzerkonto nicht erstellt, da der Benutzername bereits verwendet wird.](creating-user-accounts-vb/_static/image26.png)](creating-user-accounts-vb/_static/image25.png)

**Abbildung 9**: Das Benutzerkonto wurde nicht erstellt, da der Benutzername bereits verwendet wird ([Klicken Sie, um das Bild in voller Größe anzuzeigen](creating-user-accounts-vb/_static/image27.png)).

> [!NOTE]
> Sie Fragen sich vielleicht, wie Sie Erfolg oder Fehler ermitteln, wenn Sie eine der ersten beiden `CreateUser` Methoden Überladungen verwenden, die nicht über einen Parameter vom Typ "`MembershipCreateStatus`" verfügen. Diese ersten beiden über Ladungen lösen bei einem Fehler eine [`MembershipCreateUserException` Ausnahme](https://msdn.microsoft.com/library/system.web.security.membershipcreateuserexception.aspx) aus, die eine [`StatusCode`-Eigenschaft](https://msdn.microsoft.com/library/system.web.security.membershipcreateuserexception.statuscode.aspx) des Typs `MembershipCreateStatus`enthält.

Nachdem Sie einige Benutzerkonten erstellt haben, vergewissern Sie sich, dass die Konten erstellt wurden, indem Sie den Inhalt der `aspnet_Users`-und `aspnet_Membership` Tabellen in der `SecurityTutorials.mdf` Datenbank auflisten. Wie Abbildung 10 zeigt, habe ich über die Seite "`CreatingUserAccounts.aspx`" zwei Benutzer hinzugefügt: Tito und Bruce.

[![zwei Benutzer im Mitgliedschafts Benutzerspeicher vorhanden sind: Tito und Bruce](creating-user-accounts-vb/_static/image29.png)](creating-user-accounts-vb/_static/image28.png)

**Abbildung 10**: Der Mitgliedschafts Benutzerspeicher enthält zwei Benutzer: Tito und Bruce ([Klicken Sie, um das Bild in voller Größe anzuzeigen](creating-user-accounts-vb/_static/image30.png))

Während der Mitgliedschafts Benutzerspeicher nun die Kontoinformationen von Bruce und Tito enthält, müssen wir noch Funktionen implementieren, die es Bruce oder Tito ermöglichen, sich bei der Website anzumelden. Derzeit überprüft `Login.aspx` die Anmelde Informationen des Benutzers anhand eines hart codierten Satzes von Benutzername/Kennwort-Paaren. die angegebenen Anmelde Informationen werden *nicht* mit dem Mitgliedschafts Framework überprüft. Nun müssen die neuen Benutzerkonten in den `aspnet_Users`-und `aspnet_Membership` Tabellen ausreichen. In den nächsten Lernprogrammen *<a id="_msoanchor_9"></a>[überprüft die Benutzer Anmeldeinformationen für die Mitgliedschaft Benutzer speichern](validating-user-credentials-against-the-membership-user-store-vb.md)* , aktualisieren wir die Anmeldeseite für den Speicher für die Mitgliedschaft überprüft.

> [!NOTE]
> Wenn in der `SecurityTutorials.mdf` Datenbank keine Benutzer angezeigt werden, liegt dies möglicherweise daran, dass die Webanwendung den Standard Mitgliedschafts Anbieter verwendet, `AspNetSqlMembershipProvider`, der die `ASPNETDB.mdf` Datenbank als Benutzerspeicher verwendet. Klicken Sie im Projektmappen-Explorer auf die Schaltfläche Aktualisieren, um zu bestimmen, ob dies das Problem ist. Wenn eine Datenbank mit dem Namen `ASPNETDB.mdf` dem `App_Data` Ordner hinzugefügt wurde, ist dies das Problem. Zurück zu Schritt 4 der *<a id="_msoanchor_10"></a>[erstellen das Schema für die Mitgliedschaft in SQL Server](creating-the-membership-schema-in-sql-server-vb.md)* Tutorial Anleitungen zum ordnungsgemäßen Konfigurieren der Mitgliedschaftsanbieter.

In den meisten Szenarios zur Erstellung von Benutzerkonten wird dem Besucher eine Schnittstelle zur Eingabe von Benutzername, Kennwort, e-Mail-Adresse und sonstigen wichtigen Informationen angezeigt. zu diesem Zeitpunkt wird ein neues Konto erstellt. In diesem Schritt haben wir uns mit dem Aufbau einer solchen Schnittstelle und der Verwendung der `Membership.CreateUser`-Methode zum programmgesteuerten Hinzufügen des neuen Benutzerkontos auf der Grundlage der Benutzereingaben beschäftigt. Der Code hat jedoch soeben das neue Benutzerkonto erstellt. Es wurden keine nach Verfolgungs Aktionen durchgeführt, wie z. b. die Anmeldung des Benutzers bei der Website unter dem soeben erstellten Benutzerkonto oder das Senden einer Bestätigungs-e-Mail an den Benutzer. Diese zusätzlichen Schritte erfordern zusätzlichen Code im `Click` Ereignishandler der Schaltfläche.

ASP.net wird mit dem Steuerelement "kreateuserwizard" ausgeliefert, das für das Erstellen des Benutzerkontos konzipiert ist, vom Rendern einer Benutzeroberfläche zum Erstellen neuer Benutzerkonten, von der Erstellung des Kontos im Mitgliedschafts Framework und durch das Ausführen des Postkontos. Erstellungs Aufgaben, z. b. das Senden einer Bestätigungs-e-Mail und die Protokollierung des soeben erstellten Benutzers an der Website. Die Verwendung des Steuer Elements "kreateuserwizard" ist so einfach wie das Ziehen des Steuer Elements "kreateuserwizard" aus der Toolbox auf eine Seite und das anschließende Festlegen einiger Eigenschaften. In den meisten Fällen müssen Sie keine einzige Codezeile schreiben. In Schritt 6 wird dieses sehr ausführliche Steuerelement ausführlich erläutert.

Wenn neue Benutzerkonten nur über eine typische Webseite zum Erstellen eines Kontos erstellt werden, ist es unwahrscheinlich, dass Sie jemals Code schreiben müssen, der die `CreateUser` Methode verwendet, da das Steuerelement "kreateuserwizard" wahrscheinlich Ihre Anforderungen erfüllt. Die `CreateUser`-Methode ist jedoch in Szenarios nützlich, in denen Sie eine hochgradig angepasste Benutzeroberfläche zum Erstellen von Konten benötigen oder wenn Sie neue Benutzerkonten Programm gesteuert über eine alternative Benutzeroberfläche erstellen müssen. Angenommen, Sie verfügen über eine Seite, auf der ein Benutzer eine XML-Datei hochladen kann, die Benutzerinformationen aus einer anderen Anwendung enthält. Die Seite analysiert möglicherweise den Inhalt der hochgeladenen XML-Datei und erstellt ein neues Konto für jeden Benutzer, der in der XML-Datei dargestellt wird, indem die `CreateUser`-Methode aufgerufen wird.

## <a name="step-6-creating-a-new-user-with-the-createuserwizard-control"></a>Schritt 6: Erstellen eines neuen Benutzers mit dem Steuerelement "kreateuserwizard"

ASP.net wird mit einer Reihe von Anmeldungs-websteuer Elementen ausgeliefert. Diese Steuerelemente unterstützen viele gängige Szenarien für Benutzerkonten und Anmelde Informationen. Das [Steuerelement "kreateuserwizard](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/login/createuserwizard.aspx) " ist ein solches Steuerelement, das eine Benutzeroberfläche zum Hinzufügen eines neuen Benutzerkontos zum Mitgliedschafts Framework darstellen soll.

Wie viele der anderen Anmelde bezogenen websteuer Elemente kann der "feateuserwizard" verwendet werden, ohne eine einzige Codezeile zu schreiben. Sie stellt intuitiv eine Benutzeroberfläche bereit, die auf den Konfigurationseinstellungen des Mitgliedschafts Anbieters basiert, und ruft intern die `CreateUser` Methode der `Membership` Klasse auf, nachdem der Benutzer die erforderlichen Informationen eingegeben hat, und klickt auf die Schaltfläche Benutzer erstellen. Das Steuerelement "kreateuserwizard" ist äußerst anpassbar. Es gibt eine Reihe von Ereignissen, die in verschiedenen Phasen des kontoerstellungsprozesses ausgelöst werden. Wir können bei Bedarf Ereignishandler erstellen, um benutzerdefinierte Logik in den Workflow zum Erstellen von Konten einzufügen. Außerdem ist die Darstellung von "kreateuserwizard" sehr flexibel. Es gibt eine Reihe von Eigenschaften, die die Darstellung der Standardschnittstelle definieren. bei Bedarf kann das Steuerelement in eine Vorlage konvertiert werden, oder es können weitere Benutzer Registrierungsschritte hinzugefügt werden.

Wir beginnen mit einem Blick auf die Verwendung der Standardschnittstelle und des Verhaltens des "kreateuserwizard"-Steuer Elements. Anschließend erfahren Sie, wie Sie das Erscheinungsbild über die Eigenschaften und Ereignisse des Steuer Elements anpassen.

### <a name="examining-the-createuserwizards-default-interface-and-behavior"></a>Überprüfen der Standardschnittstelle und des Verhaltens von "feeuserwizard"

Wechseln Sie zurück zur Seite `CreatingUserAccounts.aspx` im Ordner `Membership`, wechseln Sie in den Entwurfs-oder Split-Modus, und fügen Sie dann am oberen Rand der Seite ein Steuerelement "dashateuserwizard" hinzu. Das Steuerelement "kreateuserwizard" wird im Abschnitt Anmeldungs Steuerelemente der Toolbox abgelegt. Legen Sie nach dem Hinzufügen des Steuer Elements seine `ID`-Eigenschaft auf `RegisterUser`fest. Wie der Screenshot in Abbildung 11 zeigt, rendert der Benutzername, das Kennwort, die e-Mail-Adresse und die Sicherheitsfrage und-Antwort für den neuen Benutzer eine Schnittstelle mit Textfeldern.

[![das Steuerelement "kreateuserwizard" eine generische Create User-Oberfläche rendert](creating-user-accounts-vb/_static/image32.png)](creating-user-accounts-vb/_static/image31.png)

**Abbildung 11**: Das Steuerelement "kreateuserwizard" rendert eine generische Create User-Oberfläche ([Klicken Sie, um das Bild in voller Größe anzuzeigen](creating-user-accounts-vb/_static/image33.png)

Nehmen wir uns einen Moment Zeit, um die vom Steuerelement "kreateuserwizard" generierte Standardbenutzer Oberfläche mit der Schnittstelle zu vergleichen, die wir in Schritt 5 erstellt haben. Zunächst einmal ermöglicht das Steuerelement "kreateuserwizard" dem Besucher die Angabe der Sicherheitsfrage und-Antwort, während für unsere manuell erstellte Schnittstelle eine vordefinierte Sicherheitsfrage verwendet wurde. Die Schnittstelle des Steuer Elements "kreateuserwizard" umfasst auch Validierungs Steuerelemente, während wir die Validierung der Formularfelder unserer Schnittstelle noch implementiert haben. Und die Schnittstelle von "kreateuserwizard" enthält das Textfeld "Kennwort bestätigen" (zusammen mit einem CompareValidator, um sicherzustellen, dass die Textfelder Kennwort und Kennwort vergleichen gleich sind).

Interessant ist, dass das Steuerelement "kreateuserwizard" die Konfigurationseinstellungen des Mitgliedschafts Anbieters beim Rendern seiner Benutzeroberfläche konsultiert. Beispielsweise werden die Textfelder für die Sicherheitsfrage und-Antwort nur angezeigt, wenn `requiresQuestionAndAnswer` auf true festgelegt ist. Gleichermaßen fügt der "anateuserwizard" automatisch ein RegularExpressionValidator-Steuerelement hinzu, um sicherzustellen, dass die Anforderungen an die Kenn Wort Sicherheit erfüllt werden, und legt seine `ErrorMessage` und `ValidationExpression` Eigenschaften basierend auf den Konfigurationseinstellungen für `minRequiredPasswordLength`, `minRequiredNonalphanumericCharacters`und `passwordStrengthRegularExpression` fest.

Das Steuerelement "kreateuserwizard", wie der Name schon sagt, wird vom [Assistenten-Steuer](https://msdn.microsoft.com/library/s2etd1ek.aspx)Element abgeleitet. Die Assistenten-Steuerelemente sind so konzipiert, dass Sie eine Schnittstelle für das Ausführen von Aufgaben mit mehreren Schritten Ein Assistenten-Steuerelement kann über eine beliebige Anzahl von `WizardSteps`verfügen, von denen jede eine Vorlage ist, die die HTML-und websteuer Elemente für diesen Schritt definiert. Das Assistenten-Steuerelement zeigt anfänglich das erste `WizardStep`zusammen mit Navigations Steuerelementen an, die es dem Benutzer ermöglichen, von einem Schritt zum nächsten fortzufahren oder zu den vorherigen Schritten zurückzukehren.

Wie das deklarative Markup in Abbildung 11 zeigt, enthält die Standardschnittstelle des kreateuserwizard-Steuer Elements zwei `WizardStep` s:

- [`CreateUserWizardStep`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizardstep.aspx) ? Rendert die-Schnittstelle zum Sammeln von Informationen zum Erstellen des neuen Benutzerkontos. Dies ist der in Abbildung 11 gezeigte Schritt.
- [`CompleteWizardStep`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.completewizardstep.aspx) ? Rendert eine Meldung, die angibt, dass das Konto erfolgreich erstellt wurde.

Sie können die Darstellung und das Verhalten von "kreateuserwizard" ändern, indem Sie einen dieser Schritte in Vorlagen oder durch Hinzufügen eigener `WizardStep` s ändern. Im Tutorial zum *Speichern zusätzlicher Benutzerinformationen* wird das Hinzufügen eines `WizardStep` zur Registrierungs Schnittstelle untersucht.

Sehen wir uns das Steuerelement "kreateuserwizard" in Aktion an. Besuchen Sie die Seite `CreatingUserAccounts.aspx` über einen Browser. Beginnen Sie, indem Sie einige ungültige Werte in die Benutzeroberfläche von "feateuserwizard" eingeben. Geben Sie ein Kennwort ein, das nicht den Anforderungen an die Kenn Wort Stärke entspricht, oder belassen Sie das Textfeld Benutzer Name leer. Der-Assistent zeigt eine entsprechende Fehlermeldung an. Abbildung 12 zeigt die Ausgabe, wenn versucht wird, einen Benutzer mit einem unzureichend sicheren Kennwort zu erstellen.

[![der "kreateuserwizard" automatisch Validierungs Steuerelemente einfügt.](creating-user-accounts-vb/_static/image35.png)](creating-user-accounts-vb/_static/image34.png)

**Abbildung 12**: Der "feateuserwizard" fügt automatisch Validierungs Steuerelemente ein ([Klicken Sie, um das Bild in voller Größe anzuzeigen](creating-user-accounts-vb/_static/image36.png)).

Geben Sie als nächstes die entsprechenden Werte in den Datei-Assistenten ein, und klicken Sie auf die Schaltfläche Benutzer erstellen. Wenn Sie davon ausgehen, dass die erforderlichen Felder eingegeben wurden und das Kennwort des Kennworts ausreichend ist, erstellt der "anateuserwizard" ein neues Benutzerkonto über das Mitgliedschafts Framework und zeigt dann die Schnittstelle des `CompleteWizardStep`an (siehe Abbildung 13). Im Hintergrund ruft der "feateuserwizard" die `Membership.CreateUser`-Methode auf, wie in Schritt 5.

[![ein neues Benutzerkonto wurde erfolgreich erstellt.](creating-user-accounts-vb/_static/image38.png)](creating-user-accounts-vb/_static/image37.png)

**Abbildung 13**: Ein neues Benutzerkonto wurde erfolgreich erstellt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](creating-user-accounts-vb/_static/image39.png)).

> [!NOTE]
> Wie in Abbildung 13 gezeigt, enthält die-Schnittstelle des `CompleteWizardStep`die Schaltfläche Weiter. An diesem Punkt wird jedoch nur ein Postback durchführt, das den Besucher auf derselben Seite belassen wird. Im Abschnitt "Anpassen des Erscheinungs Bilds und Verhaltens des dashateuserwizard" im Abschnitt "Eigenschaften" wird erläutert, wie diese Schaltfläche den Besucher an `Default.aspx` (oder eine andere Seite) senden kann.

Nachdem Sie ein neues Benutzerkonto erstellt haben, kehren Sie zu Visual Studio zurück, und überprüfen Sie die `aspnet_Users`-und `aspnet_Membership` Tabellen, wie in Abbildung 10 dargestellt, um sicherzustellen, dass das Konto erfolgreich erstellt wurde.

### <a name="customizing-the-createuserwizards-behavior-and-appearance-through-its-properties"></a>Anpassen des Verhaltens und der Darstellung von "anateuserwizard" mithilfe der zugehörigen Eigenschaften

Der "feateuserwizard" kann auf verschiedene Weise mithilfe von Eigenschaften, `WizardStep` s und Ereignis Handlern angepasst werden. In diesem Abschnitt wird erläutert, wie die Darstellung des Steuer Elements über seine Eigenschaften angepasst wird. im nächsten Abschnitt wird das Verhalten des Steuer Elements mithilfe von Ereignis Handlern erweitert.

Nahezu der gesamte Text, der in der Standardbenutzer Oberfläche des Steuer Elements "kreateuserwizard" angezeigt wird, kann über seine Vielzahl von Eigenschaften angepasst werden. Beispielsweise können der Benutzername, Kennwort, Kennwort bestätigen, E-mail, Sicherheitsfrage und Sicherheitsantwort Bezeichnungen auf der linken Seite der Textfelder angepasst werden, durch die [ `UserNameLabelText` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.usernamelabeltext.aspx), [ `PasswordLabelText` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.passwordlabeltext.aspx), [ `ConfirmPasswordLabelText` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.confirmpasswordlabeltext.aspx), [ `EmailLabelText` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.emaillabeltext.aspx), [ `QuestionLabelText` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.questionlabeltext.aspx), und [ `AnswerLabelText` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.answerlabeltext.aspx) Eigenschaften, bzw. Ebenso gibt es Eigenschaften, mit denen der Text für die Schaltflächen "Benutzer erstellen" und "Fortfahren" in den `CreateUserWizardStep` und `CompleteWizardStep`angegeben werden kann, sowie wenn diese Schaltflächen als Schaltflächen, Linkschaltflächen oder imagebuttons gerendert werden.

Die Farben, Rahmen, Schriftarten und anderen visuellen Elemente können über einen Host von Stileigenschaften konfiguriert werden. Das Steuerelement "kreateuserwizard" verfügt über die allgemeinen Eigenschaften des websteuer Elements "`BackColor`", "`BorderStyle`", "`CssClass`", "`Font`" usw. und es gibt eine Reihe von Format Eigenschaften zum Definieren der Darstellung für bestimmte Abschnitte der Schnittstelle von "fiateuserwizard". Die [`TextBoxStyle`-Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.textboxstyle.aspx)definiert beispielsweise die Stile für die Textfelder in der `CreateUserWizardStep`, während die [`TitleTextStyle`-Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.titletextstyle.aspx) den Stil für den Titel definiert (registrieren Sie sich für Ihr neues Konto).

Zusätzlich zu den Darstellungs bezogenen Eigenschaften gibt es eine Reihe von Eigenschaften, die sich auf das Verhalten des Steuer Elements "kreateuserwizard" auswirken. Wenn die [`DisplayCancelButton`-Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.wizard.displaycancelbutton.aspx)auf true festgelegt ist, wird neben der Schaltfläche Benutzer erstellen eine Schaltfläche Abbrechen angezeigt (der Standardwert ist false). Wenn Sie die Schaltfläche Abbrechen anzeigen, achten Sie darauf, dass Sie auch die [`CancelDestinationPageUrl`-Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.continuedestinationpageurl.aspx)festlegen, die die Seite angibt, an die der Benutzer nach dem Klicken auf Abbrechen gesendet wird. Wie bereits im vorherigen Abschnitt erwähnt, verursacht die Schaltfläche weiter in der-Schnittstelle des `CompleteWizardStep`ein Postback, aber der Besucher bleibt auf derselben Seite. Um den Besucher nach dem Klicken auf die Schaltfläche "weiter" an eine andere Seite zu senden, geben Sie einfach die URL in der [`ContinueDestinationPageUrl`-Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.continuedestinationpageurl.aspx)an.

Aktualisieren Sie das `RegisterUser`-Steuerelement von "", um die Schaltfläche Abbrechen anzuzeigen und den Besucher an `Default.aspx` zu senden, wenn auf die Schaltflächen Abbrechen oder fortfahren geklickt wird. Um dies zu erreichen, legen Sie die `DisplayCancelButton`-Eigenschaft auf true fest, und die Eigenschaften `CancelDestinationPageUrl` und `ContinueDestinationPageUrl` auf ~/Default.aspx. In Abbildung 14 wird der aktualisierte auflisteuserwizard angezeigt, wenn er über einen Browser angezeigt wird.

[![der "kreateuserwizardstep" eine Schaltfläche "Abbrechen" enthält](creating-user-accounts-vb/_static/image41.png)](creating-user-accounts-vb/_static/image40.png)

**Abbildung 14**: Die `CreateUserWizardStep` enthält eine Schaltfläche "Abbrechen" ([Klicken Sie, um das Bild in voller Größe anzuzeigen](creating-user-accounts-vb/_static/image42.png)).

Wenn ein Besucher einen Benutzernamen, ein Kennwort, eine e-Mail-Adresse und eine Sicherheitsfrage und-Antwort eingibt und auf "Benutzer erstellen" klickt, wird ein neues Benutzerkonto erstellt, und der Besucher wird als dieser neu erstellte Benutzer angemeldet. Wenn die Person, die die Seite besucht, ein neues Konto für sich selbst erstellt, ist dies wahrscheinlich das gewünschte Verhalten. Möglicherweise möchten Sie jedoch Administratoren erlauben, neue Benutzerkonten hinzuzufügen. Auf diese Weise wird das Benutzerkonto erstellt, aber der Administrator bleibt als Administrator angemeldet (und nicht als neu erstelltes Konto). Dieses Verhalten kann durch die boolesche`LoginCreatedUser`- [Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.logincreateduser.aspx)geändert werden.

Benutzerkonten im Mitgliedschafts Framework enthalten ein genehmigtes Flag. Benutzer, die nicht genehmigt sind, können sich nicht bei der Website anmelden. Standardmäßig wird ein neu erstelltes Konto als genehmigt gekennzeichnet, sodass sich der Benutzer sofort bei der Website anmelden kann. Es ist jedoch möglich, dass neue Benutzerkonten als "nicht genehmigt" gekennzeichnet sind. Vielleicht möchten Sie, dass ein Administrator neue Benutzer manuell genehmigen kann, bevor er sich anmelden kann. oder Sie möchten sicherstellen, dass die bei der Registrierung eingegebene e-Mail-Adresse gültig ist, bevor sich ein Benutzer anmelden kann. Wenn dies der Fall ist, können Sie das neu erstellte Benutzerkonto als "nicht genehmigt" markieren, indem Sie die [`DisableCreatedUser`-Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.disablecreateduser.aspx) des Steuer Elements "kreateuserwizard" auf "true" festlegen (die Standardeinstellung ist "false").

Weitere verhaltensbezogene Eigenschaften von Hinweis sind `AutoGeneratePassword` und `MailDefinition`. Wenn die [`AutoGeneratePassword`-Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.autogeneratepassword.aspx) auf true festgelegt ist, werden in der `CreateUserWizardStep` die Textfelder Kennwort und Kennwort bestätigen nicht angezeigt. Stattdessen wird das neu erstellte Benutzer Kennwort automatisch mit der [`GeneratePassword`-Methode](https://msdn.microsoft.com/library/system.web.security.membership.generatepassword.aspx)der `Membership`-Klasse generiert. Die `GeneratePassword`-Methode erstellt ein Kennwort mit einer angegebenen Länge und mit einer ausreichenden Anzahl nicht alphanumerischer Zeichen, um den konfigurierten Anforderungen an die Kenn Wort Sicherheit gerecht zu werden.

Die [`MailDefinition`-Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.maildefinition.aspx) ist nützlich, wenn Sie eine e-Mail an die e-Mail-Adresse senden möchten, die während des Konto Erstellungs Vorgangs angegeben wurde. Die `MailDefinition`-Eigenschaft enthält eine Reihe von unter Eigenschaften zum Definieren von Informationen über die erstellte e-Mail-Nachricht. Zu diesen unter Eigenschaften gehören Optionen wie `Subject`, `Priority`, `IsBodyHtml`, `From`, `CC`und `BodyFileName`. Die [`BodyFileName`-Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.maildefinition.bodyfilename.aspx) verweist auf eine Text-oder HTML-Datei, die den Textkörper für die e-Mail-Nachricht enthält. Der Text unterstützt zwei vordefinierte Platzhalter: `<%UserName%>` und `<%Password%>`. Diese Platzhalter, sofern Sie in der `BodyFileName` Datei vorhanden sind, werden durch den soeben erstellten Benutzernamen und das zugehörige Kennwort ersetzt.

> [!NOTE]
> Die `MailDefinition`-Eigenschaft des `CreateUserWizard`-Steuer Elements gibt nur Details zu der e-Mail-Nachricht an, die gesendet wird, wenn ein neues Konto erstellt wird. Es enthält keine Details darüber, wie die e-Mail-Nachricht tatsächlich gesendet wird (d. h. ob ein SMTP-Server oder ein e-Mail-Ablage Verzeichnis verwendet wird, Authentifizierungsinformationen usw.). Diese Details auf niedriger Ebene müssen im Abschnitt `<system.net>` in `Web.config`definiert werden. Weitere Informationen zu diesen Konfigurationseinstellungen und zum Senden von e-Mails von ASP.NET 2,0 im Allgemeinen finden Sie in den häufig gestellten Fragen [unter SystemNetMail.com](http://www.systemnetmail.com/) und [im Artikel senden von e-Mails in ASP.NET 2,0](http://aspnet.4guysfromrolla.com/articles/072606-1.aspx).

### <a name="extending-the-createuserwizards-behavior-using-event-handlers"></a>Erweitern des Verhaltens von "kreateuserwizard" mithilfe von Ereignis Handlern

Das Steuerelement "kreateuserwizard" löst während des Workflows eine Reihe von Ereignissen aus. Wenn z. b. ein Besucher seinen Benutzernamen, Ihr Kennwort und andere relevante Informationen eingibt und auf die Schaltfläche "Benutzer erstellen" klickt, löst das Steuerelement "kreateuserwizard" das [`CreatingUser` Ereignis](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.creatinguser.aspx)aus Wenn während des Erstellungs Vorgangs ein Problem auftritt, wird das [`CreateUserError` Ereignis](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.createusererror.aspx) ausgelöst. Wenn der Benutzer jedoch erfolgreich erstellt wurde, wird das [`CreatedUser`-Ereignis](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.createduser.aspx) ausgelöst. Es gibt weitere Ereignisse des Objekts "kreateuserwizard", die ausgelöst werden, aber dies sind die drei meisten.

In bestimmten Szenarien kann es sinnvoll sein, den Workflow "" von "" zu erstellen. Dies kann durch Erstellen eines Ereignis Handlers für das entsprechende Ereignis erreicht werden. Um dies zu veranschaulichen, erweitern wir das `RegisterUser`-Steuerelement "kreateuserwizard", um eine benutzerdefinierte Validierung des Benutzernamens und des Kennworts einzubeziehen. Wir erweitern den "" "" "" "" "" "" "" "" "", "", "", "" Kurz gesagt, wir möchten verhindern, dass jemand einen Benutzernamen wie "Scott" erstellt oder eine Kombination aus Benutzername und Kennwort wie Scott und Scott. 1234 hat.

Um dies zu erreichen, erstellen wir einen Ereignishandler für das `CreatingUser`-Ereignis, um zusätzliche Validierungs Überprüfungen durchzuführen. Wenn die angegebenen Daten ungültig sind, muss der Erstellungs Vorgang abgebrochen werden. Außerdem müssen wir der Seite ein Label-websteuer Element hinzufügen, um eine Meldung anzuzeigen, die anzeigt, dass der Benutzername oder das Kennwort ungültig ist. Fügen Sie zunächst unter dem Steuerelement "kreateuserwizard" ein Label-Steuerelement hinzu, und legen Sie dessen `ID`-Eigenschaft auf `InvalidUserNameOrPasswordMessage` und dessen `ForeColor`-Eigenschaft auf `Red`. Löschen Sie die `Text`-Eigenschaft, und legen Sie die Eigenschaften `EnableViewState` und `Visible` auf false fest.

[!code-aspx[Main](creating-user-accounts-vb/samples/sample7.aspx)]

Erstellen Sie als nächstes einen Ereignishandler für das `CreatingUser`-Ereignis des Steuer Elements "". Um einen Ereignishandler zu erstellen, wählen Sie das Steuerelement im Designer aus, und navigieren Sie dann zum Eigenschaftenfenster. Klicken Sie dort auf das Blitz Symbol, und doppelklicken Sie dann auf das entsprechende Ereignis, um den Ereignishandler zu erstellen.

Fügen Sie dem `CreatingUser` -Ereignishandler folgenden Code hinzu:

[!code-vb[Main](creating-user-accounts-vb/samples/sample8.vb)]

Beachten Sie, dass Benutzername und Kennwort eingegeben werden, in das Steuerelement CreateUserWizard über seine [ `UserName` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.username.aspx) und [ `Password` Eigenschaften](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.password.aspx)bzw. Wir verwenden diese Eigenschaften im obigen Ereignishandler, um zu bestimmen, ob der angegebene Benutzername führende oder nachfolgende Leerzeichen enthält und ob der Benutzername innerhalb des Kennworts gefunden wird. Wenn eine dieser Bedingungen erfüllt ist, wird eine Fehlermeldung in der `InvalidUserNameOrPasswordMessage`-Bezeichnung angezeigt, und die `e.Cancel`-Eigenschaft des Ereignis Handlers wird auf `True`festgelegt. Wenn `e.Cancel` auf `True`festgelegt ist, wird der Workflow von "debugeuserwizard" kurz abgebrochen, sodass der Erstellungs Vorgang des Benutzerkontos tatsächlich abgebrochen wird.

Abbildung 15 zeigt einen Screenshot der `CreatingUserAccounts.aspx`, wenn der Benutzer einen Benutzernamen mit führenden Leerzeichen eingibt.

[![Benutzernamen mit führenden oder nachfolgenden Leerzeichen sind nicht zulässig.](creating-user-accounts-vb/_static/image44.png)](creating-user-accounts-vb/_static/image43.png)

**Abbildung 15**: Benutzernamen mit führenden oder nachfolgenden Leerzeichen sind nicht zulässig ([Klicken Sie, um das Bild in voller Größe anzuzeigen](creating-user-accounts-vb/_static/image45.png))

> [!NOTE]
> Sehen wir ein Beispiel der Verwendung des Steuerelements CreateUserWizard `CreatedUser` Ereignis in der *<a id="_msoanchor_11"></a>[zusätzliche Benutzerinformationen speichern](storing-additional-user-information-vb.md)* Lernprogramm.

## <a name="summary"></a>Zusammenfassung

Mit der `CreateUser`-Methode der `Membership` Klasse wird ein neues Benutzerkonto im Mitgliedschafts Framework erstellt. Dies erfolgt durch Delegieren des Aufrufens an den konfigurierten Mitgliedschafts Anbieter. Im Fall der `SqlMembershipProvider`fügt die `CreateUser`-Methode der Datenbanktabelle `aspnet_Users` und `aspnet_Membership` einen Datensatz hinzu.

Neue Benutzerkonten können zwar Programm gesteuert erstellt werden (wie in Schritt 5 erläutert), doch ein schnellerer und einfacherer Ansatz besteht darin, das CreateUserWizard-Steuerelement zu verwenden. Dieses Steuerelement rendert eine mehrstufige Benutzeroberfläche zum Sammeln von Benutzerinformationen und zum Erstellen eines neuen Benutzers im Mitgliedschafts Framework. Unter dem Hintergrund verwendet dieses Steuerelement dieselbe `Membership.CreateUser` Methode wie in Schritt 5, aber das Steuerelement erstellt die Benutzeroberfläche, Validierungs Steuerelemente und antwortet auf Fehler beim Erstellen von Benutzerkonten, ohne dass ein Code von einem Code geschrieben werden muss.

An dieser Stelle haben wir die Funktionalität zum Erstellen neuer Benutzerkonten. Die Anmeldeseite überprüft jedoch weiterhin diese hart codierten Anmelde Informationen, die wir im zweiten Tutorial zurück angegeben haben. <a id="_msoanchor_12"> </a>Im [nächsten Tutorial](validating-user-credentials-against-the-membership-user-store-vb.md) aktualisieren wir `Login.aspx`, um die vom Benutzer bereitgestellten Anmelde Informationen anhand des Mitgliedschafts-Frameworks zu überprüfen.

Fröhliche Programmierung!

### <a name="further-reading"></a>Weiterführende Themen

Weitere Informationen zu den in diesem Tutorial behandelten Themen finden Sie in den folgenden Ressourcen:

- [`CreateUser` technische Dokumentation](https://msdn.microsoft.com/library/system.web.security.membershipprovider.createuser.aspx)
- [Übersicht über das kreateuserwizard-Steuerelement](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/login/createuserwizard.aspx)
- [Erstellen eines Datei System basierten Site Übersichts Anbieters](http://aspnet.4guysfromrolla.com/articles/020106-1.aspx)
- [Erstellen einer Schritt-für-Schritt-Benutzeroberfläche mit dem ASP.NET 2,0-Assistenten-Steuerelement](http://aspnet.4guysfromrolla.com/articles/061406-1.aspx)
- [Überprüfen der Site Navigation in ASP.NET 2.0](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx)
- [Master Seiten und Website Navigation](https://asp.net/learn/data-access/tutorial-03-vb.aspx)
- [Der SQL Site Map-Anbieter, auf den Sie gewartet haben](https://msdn.microsoft.com/msdnmag/issues/06/02/WickedCode/default.aspx)

### <a name="about-the-author"></a>Zum Autor

Scott Mitchell, Autor mehrerer ASP/ASP. net-Bücher und Gründer von 4GuysFromRolla.com, hat seit 1998 mit Microsoft-Webtechnologien gearbeitet. Scott arbeitet als unabhängiger Berater, Ausbilder und Writer. Sein neueste Buch wird *[Sams Schulen selbst ASP.NET 2.0 in 24 Stunden](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)* . Scott erreicht werden kann, zur [mitchell@4guysfromrolla.com](mailto:mitchell@4guysfromrolla.com) oder über seinen Blog unter [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Besonders vielen Dank

Diese tutorialreihe wurde von vielen hilfreichen Reviewern geprüft. Lead Reviewer für dieses Tutorial war Teresa Murphy. Möchten Sie meine bevorstehenden MSDN-Artikel überprüfen? Wenn dies der Fall ist, können Sie eine Zeile in [mitchell@4GuysFromRolla.com](mailto:mitchell@4guysfromrolla.com)ablegen.

> [!div class="step-by-step"]
> [Zurück](creating-the-membership-schema-in-sql-server-vb.md)
> [Weiter](validating-user-credentials-against-the-membership-user-store-vb.md)
