---
uid: web-forms/overview/older-versions-getting-started/master-pages/multiple-contentplaceholders-and-default-content-cs
title: Mehrere contentplatzhalter und Standard InhaltC#() | Microsoft-Dokumentation
author: rick-anderson
description: Hier wird erläutert, wie Sie einer Master Seite mehrere Inhaltsort-Inhaber hinzufügen und wie Sie Standard Inhalt in den Besitzern für Inhalts Platz angeben.
ms.author: riande
ms.date: 05/21/2008
ms.assetid: b9b9798b-027d-46cc-9636-473378e437ac
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/multiple-contentplaceholders-and-default-content-cs
msc.type: authoredcontent
ms.openlocfilehash: e902bcae05c0e7976a20293f2b01e5f2e2bee13a
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2019
ms.locfileid: "74639460"
---
# <a name="multiple-contentplaceholders-and-default-content-c"></a>Mehrere ContentPlaceHolder-Steuerelemente und Standardinhalt (C#)

von [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Code herunterladen](https://download.microsoft.com/download/e/e/f/eef369f5-743a-4a52-908f-b6532c4ce0a4/ASPNET_MasterPages_Tutorial_02_CS.zip) oder [PDF herunterladen](https://download.microsoft.com/download/8/f/6/8f6349e4-6554-405a-bcd7-9b094ba5089a/ASPNET_MasterPages_Tutorial_02_CS.pdf)

> Hier wird erläutert, wie Sie einer Master Seite mehrere Inhaltsort-Inhaber hinzufügen und wie Sie Standard Inhalt in den Besitzern für Inhalts Platz angeben.

## <a name="introduction"></a>Einführung

Im vorherigen Tutorial wurde erläutert, wie die Masterseiten ASP.NET-Entwicklern ermöglichen, ein konsistentes, Site weites Layout zu erstellen. Master Seiten definieren sowohl Markup, das allen Inhaltsseiten und-Regionen gemeinsam ist, die seitenweise anpassbar sind. Im vorherigen Tutorial haben wir eine einfache Master Seite (`Site.master`) und zwei Inhaltsseiten (`Default.aspx` und `About.aspx`) erstellt. Unsere Master Seite umfasste zwei Inhalts Platzhalter namens "`head`" und "`MainContent`", die sich im `<head>`-Element bzw. im Webformular befanden. Obwohl die Inhaltsseiten jeweils über zwei Inhalts Steuerelemente verfügten, haben wir nur Markup für das `MainContent`angegeben, das den entspricht.

Wie von den beiden contentplachalter-Steuerelementen in `Site.master`gezeigt, kann eine Master Seite mehrere Content-Platzhalter enthalten. Darüber hinaus kann auf der Master Seite Standard Markup für die contentplachalter-Steuerelemente angegeben werden. Eine Inhaltsseite kann dann optional ein eigenes Markup angeben oder das Standard Markup verwenden. In diesem Tutorial betrachten wir die Verwendung mehrerer Inhalts Steuerelemente auf der Master Seite und erfahren, wie Sie das Standard Markup in den contentplachalter-Steuerelementen definieren.

## <a name="step-1-adding-additional-contentplaceholder-controls-to-the-master-page"></a>Schritt 1: Hinzufügen zusätzlicher contentplachalter-Steuerelemente zur Master Seite

Viele Website Entwürfe enthalten mehrere Bereiche auf dem Bildschirm, die seitenweise angepasst werden. `Site.master`ist die Master Seite, die wir im vorherigen Tutorial erstellt haben, im Webformular mit dem Namen `MainContent`einen einzelnen contentplachalter enthalten. Insbesondere befindet sich dieser contentplachalter innerhalb des `mainContent` `<div>`-Elements.

Abbildung 1 zeigt `Default.aspx`, wenn Sie in einem Browser angezeigt werden. Der rot Eingekreiste Bereich ist das Seiten spezifische Markup, das `MainContent`entspricht.

[![der kreisförmige Bereich den aktuell anpassbaren Bereich auf seitenweise anzeigt](multiple-contentplaceholders-and-default-content-cs/_static/image2.png)](multiple-contentplaceholders-and-default-content-cs/_static/image1.png)

**Abbildung 01**: der Kreisbereich zeigt den aktuell anpassbaren Bereich auf seitenweise an ([Klicken Sie, um das Bild in voller Größe anzuzeigen](multiple-contentplaceholders-and-default-content-cs/_static/image3.png)).

Stellen Sie sich vor, dass neben der in Abbildung 1 gezeigten Region auch Seiten spezifische Elemente in der linken Spalte unterhalb der Lektionen und der Nachrichten Abschnitte hinzugefügt werden müssen. Um dies zu erreichen, fügen Sie der Master Seite ein weiteres contentplachalter-Steuerelement hinzu. Öffnen Sie hierzu die `Site.master` Master Seite in Visual Web Developer, und ziehen Sie dann ein contentplachalter-Steuerelement aus der Toolbox auf den Designer nach dem News-Abschnitt. Legen Sie den `ID` contentplachalter auf `LeftColumnContent`fest.

[![der linken Spalte der Master Seite ein contentplachalter-Steuerelement hinzufügen](multiple-contentplaceholders-and-default-content-cs/_static/image5.png)](multiple-contentplaceholders-and-default-content-cs/_static/image4.png)

**Abbildung 02**: Hinzufügen eines contentplachalter-Steuer Elements zur linken Spalte der Master Seite ([Klicken Sie, um das Bild in voller Größe anzuzeigen](multiple-contentplaceholders-and-default-content-cs/_static/image6.png))

Durch das Hinzufügen des `LeftColumnContent` contentplachalter zur Master Seite können wir Inhalt für diesen Bereich seitenweise definieren, indem wir ein Inhalts Steuerelement auf der Seite einschließen, dessen `ContentPlaceHolderID` auf `LeftColumnContent`festgelegt ist. Dieser Vorgang wird in Schritt 2 untersucht.

## <a name="step-2-defining-content-for-the-new-contentplaceholder-in-the-content-pages"></a>Schritt 2: Definieren von Inhalten für den neuen contentplachalter auf den Inhaltsseiten

Wenn Sie der Website eine neue Inhaltsseite hinzufügen, erstellt Visual Web Developer automatisch ein Inhalts Steuerelement auf der Seite für jeden contentplachalter auf der ausgewählten Master Seite. Nachdem Sie der Master Seite in Schritt 1 einen `LeftColumnContent` contentplachalter hinzugefügt haben, haben neue ASP.NET-Seiten nun drei Inhalts Steuerelemente.

Um dies zu veranschaulichen, fügen Sie dem Stammverzeichnis eine neue Inhaltsseite mit dem Namen `MultipleContentPlaceHolders.aspx` hinzu, die an die `Site.master` Master Seite gebunden ist. Visual Web Developer erstellt diese Seite mit dem folgenden deklarativen Markup:

[!code-aspx[Main](multiple-contentplaceholders-and-default-content-cs/samples/sample1.aspx)]

Geben Sie einige Inhalte in das Inhalts Steuerelement ein, die auf die `MainContent` contentplatzhalter (`Content2`) verweisen. Fügen Sie als nächstes das folgende Markup zum `Content3` Content-Steuerelement hinzu (das auf den `LeftColumnContent` contentplachalter verweist):

[!code-html[Main](multiple-contentplaceholders-and-default-content-cs/samples/sample2.html)]

Nachdem Sie dieses Markup hinzugefügt haben, besuchen Sie die Seite über einen Browser. Wie in Abbildung 3 gezeigt, wird das im `Content3` Inhalts Steuerelement angezeigte Markup in der linken Spalte unterhalb des Abschnitts "News" angezeigt (rot). Das in `Content2` platzierte Markup wird im rechten Bereich der Seite angezeigt (in blau).

[![in der linken Spalte wird nun Seiten spezifischer Inhalt unterhalb des Abschnitts "News" angezeigt.](multiple-contentplaceholders-and-default-content-cs/_static/image8.png)](multiple-contentplaceholders-and-default-content-cs/_static/image7.png)

**Abbildung 03**: die linke Spalte enthält jetzt Seiten spezifischen Inhalt unterhalb des Abschnitts "News" ([Klicken Sie, um das Bild in voller Größe anzuzeigen](multiple-contentplaceholders-and-default-content-cs/_static/image9.png))

### <a name="defining-content-in-existing-content-pages"></a>Definieren von Inhalten in vorhandenen Inhaltsseiten

Beim Erstellen einer neuen Inhaltsseite wird automatisch das contentplachalter-Steuerelement integriert, das in Schritt 1 hinzugefügt wurde. Unsere beiden vorhandenen Inhaltsseiten-`About.aspx` und `Default.aspx` haben jedoch kein Inhalts Steuerelement für den `LeftColumnContent` contentplachalter. Um Inhalt für diesen contentplachalter in diesen beiden vorhandenen Seiten anzugeben, müssen wir ein Inhalts Steuerelement selbst hinzufügen.

Im Gegensatz zu den meisten ASP.net-websteuer Elementen enthält die Visual Web Developer-Toolbox kein Inhalts Steuerelement. Wir können das deklarative Markup des Inhalts Steuer Elements manuell in die Quell Ansicht eingeben, aber ein einfacherer und schnellerer Ansatz ist die Verwendung der Designansicht. Öffnen Sie die Seite `About.aspx`, und wechseln Sie zum Designansicht. Wie Abbildung 4 zeigt, wird der `LeftColumnContent` contentplachalter in der Designansicht angezeigt. Wenn Sie den Mauszeiger darüber bewegen, wird der Titel wie folgt angezeigt: "leftcolumncontent (Master)". Die Einbindung von "Master" in den Titel gibt an, dass auf der Seite für diesen contentplachalter kein Inhalts Steuerelement definiert ist. Wenn ein Inhalts Steuerelement für den contentplachalter vorhanden ist, wie im Fall von `MainContent`, liest der Titel Folgendes: "*ContentPlaceHolderID* (Custom)".

Wenn Sie ein Inhalts Steuerelement für den `LeftColumnContent` contentplachalter hinzufügen `About.aspx`möchten, erweitern Sie das Smarttag contentplachalter, und klicken Sie auf den Link benutzerdefinierten Inhalt erstellen.

[![die Entwurfs Ansicht für "about. aspx" den leftcolumncontent-contentplachalter anzeigt.](multiple-contentplaceholders-and-default-content-cs/_static/image11.png)](multiple-contentplaceholders-and-default-content-cs/_static/image10.png)

**Abbildung 04**: in der Entwurfs Ansicht für `About.aspx` wird die `LeftColumnContent` contentplachalter angezeigt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](multiple-contentplaceholders-and-default-content-cs/_static/image12.png))

Wenn Sie auf den Link benutzerdefinierten Inhalt erstellen klicken, wird das erforderliche Inhalts Steuerelement auf der Seite generiert und seine `ContentPlaceHolderID`-Eigenschaft auf die `ID`des contentplachalters festgelegt Wenn Sie z. b. auf den Link benutzerdefinierten Inhalt erstellen für `LeftColumnContent` Region in `About.aspx` klicken, wird der Seite das folgende deklarative Markup hinzugefügt:

[!code-aspx[Main](multiple-contentplaceholders-and-default-content-cs/samples/sample3.aspx)]

### <a name="omitting-content-controls"></a>Weglassen von Inhalts Steuerelementen

ASP.net erfordert nicht, dass alle Inhaltsseiten Inhalts Steuerelemente für jeden und jeden contentplachalter enthalten, der auf der Master Seite definiert ist. Wenn ein Inhalts Steuerelement weggelassen wird, verwendet die ASP.net-Engine das innerhalb des contentplachalter-Elements auf der Master Seite definierte Markup. Dieses Markup wird als *Standard Inhalt* von contentplachalter bezeichnet und ist in Szenarios nützlich, in denen der Inhalt für eine bestimmte Region in den meisten Seiten üblich ist, aber für eine kleine Anzahl von Seiten angepasst werden muss. In Schritt 3 wird das Angeben von Standard Inhalten auf der Master Seite untersucht.

Derzeit enthält `Default.aspx` zwei Inhalts Steuerelemente für die `head` und `MainContent` contentplatzhalter. Sie verfügt über kein Inhalts Steuerelement für `LeftColumnContent`. Folglich wird, wenn `Default.aspx` gerendert wird, der Standard Inhalt `LeftColumnContent` contentplachalter verwendet. Da wir noch keinen Standard Inhalt für diesen contentplachalter definieren müssen, ist der Nettoeffekt, dass für diesen Bereich kein Markup ausgegeben wird. Um dieses Verhalten zu überprüfen, besuchen Sie `Default.aspx` über einen Browser. Wie in Abbildung 5 gezeigt, wird in der linken Spalte unterhalb des News-Abschnitts kein Markup ausgegeben.

[![kein Inhalt für den leftcolumncontent-contentplachalter gerendert wird.](multiple-contentplaceholders-and-default-content-cs/_static/image14.png)](multiple-contentplaceholders-and-default-content-cs/_static/image13.png)

**Abbildung 05**: Es wird kein Inhalt für den `LeftColumnContent` contentplachalter gerendert ([Klicken Sie, um das Bild in voller Größe anzuzeigen](multiple-contentplaceholders-and-default-content-cs/_static/image15.png))

## <a name="step-3-specifying-default-content-in-the-master-page"></a>Schritt 3: Angeben von Standard Inhalten auf der Master Seite

Einige Website Entwürfe enthalten eine Region, deren Inhalt für alle Seiten auf der Website identisch ist, mit Ausnahme von einer oder zwei Ausnahmen. Angenommen, eine Website unterstützt Benutzerkonten. Eine solche Website erfordert eine Anmeldeseite, auf der Besucher Ihre Anmelde Informationen eingeben können, um sich bei der Website anzumelden. Um den Anmeldevorgang zu beschleunigen, können die Website-Designer in der linken oberen Ecke jeder Seite Benutzername-und Kenn Wort Textfelder einschließen, damit Benutzer sich anmelden können, ohne die Anmeldeseite explizit besuchen zu müssen. Obwohl diese Textfelder für Benutzername und Kennwort auf den meisten Seiten hilfreich sind, sind Sie auf der Anmeldeseite redundant, die bereits Textfelder für die Anmelde Informationen des Benutzers enthält.

Um diesen Entwurf zu implementieren, können Sie in der oberen linken Ecke der Master Seite ein contentplachalter-Steuerelement erstellen. Jede Seite, die zum Anzeigen der Textfelder für Benutzername und Kennwort in der oberen linken Ecke benötigt wird, würde ein Inhalts Steuerelement für diesen contentplachalter erstellen und die erforderliche Schnittstelle hinzufügen. Die Anmeldeseite hingegen würde entweder das Hinzufügen eines Inhalts Steuer Elements für diesen contentplachalter weglassen oder ein Inhalts Steuerelement ohne definiertes Markup erstellen. Der Nachteil dieses Ansatzes besteht darin, dass Sie die Textfelder "username" und "Password" allen Seiten hinzufügen müssen, die der Website hinzugefügt werden (mit Ausnahme der Anmeldeseite). Dabei werden Probleme gefordert. Wir sind davon auszugehen, dass Sie diese Textfelder zu einer Seite hinzufügen können, oder es ist nicht möglich, die Schnittstelle ordnungsgemäß zu implementieren (möglicherweise wird nur ein Textfeld anstelle von zwei hinzugefügt).

Eine bessere Lösung besteht darin, die Textfelder Benutzername und Kennwort als Standard Inhalt des contentplachalter-Steuerelemente zu definieren. Auf diese Weise müssen wir diese Standard Inhalte nur auf den wenigen Seiten außer Kraft setzen, die nicht die Textfelder "username" und "Password" (z. b. die Anmeldeseite) anzeigen. Um die Angabe von Standard Inhalten für ein contentplachalter-Steuerelement zu veranschaulichen, implementieren wir das soeben erörterte Szenario.

> [!NOTE]
> Im restlichen Teil dieses Tutorials wird unsere Website aktualisiert, sodass Sie in der linken Spalte eine Anmelde Schnittstelle für alle Seiten, aber die Anmeldeseite enthält. In diesem Tutorial wird jedoch nicht untersucht, wie Sie die Website für die Unterstützung von Benutzerkonten konfigurieren. Weitere Informationen zu diesem Thema finden Sie in den Tutorials meine [Formular Authentifizierung, Autorisierung, Benutzerkonten und Rollen](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md) .

### <a name="adding-a-contentplaceholder-and-specifying-its-default-content"></a>Hinzufügen eines contentplachalter-und angeben seines Standard Inhalts

Öffnen Sie die Seite `Site.master` Master, und fügen Sie der linken Spalte zwischen der `DateDisplay` Bezeichnung und dem Abschnitt Lektionen das folgende Markup hinzu:

[!code-aspx[Main](multiple-contentplaceholders-and-default-content-cs/samples/sample4.aspx)]

Nachdem Sie dieses Markup hinzugefügt haben, sollte die Designansicht der Master Seite in etwa wie in Abbildung 6 aussehen.

[![die Master Seite ein Anmelde Steuerelement enthält.](multiple-contentplaceholders-and-default-content-cs/_static/image17.png)](multiple-contentplaceholders-and-default-content-cs/_static/image16.png)

**Abbildung 06**: die Master Seite enthält ein Anmelde Steuerelement ([Klicken Sie, um das Bild in voller Größe anzuzeigen](multiple-contentplaceholders-and-default-content-cs/_static/image18.png))

Dieser contentplachalter (`QuickLoginUI`) verfügt über ein Login-websteuer Element als Standard Inhalt. Das-Anmelde Steuerelement zeigt eine Benutzeroberfläche an, die den Benutzer zur Eingabe des Benutzernamens und Kennworts zusammen mit einer Anmelde Schaltfläche auffordert. Nach dem Klicken auf die Schaltfläche Anmelden überprüft das Anmelde Steuerelement intern die Anmelde Informationen des Benutzers anhand der Mitgliedschafts-API. Um dieses Anmelde Steuerelement in der Praxis verwenden zu können, müssen Sie Ihren Standort für die Verwendung der Mitgliedschaft konfigurieren. Dieses Thema geht über den Rahmen dieses Tutorials hinaus. Weitere Informationen zum Entwickeln einer Webanwendung, die Benutzerkonten unterstützt, finden Sie in den Tutorials zur [Formular Authentifizierung, Autorisierung, Benutzerkonten und Rollen](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md) .

Sie können das Verhalten oder die Darstellung des Anmeldungs Steuer Elements anpassen. Ich habe zwei Eigenschaften festgelegt: `TitleText` und `FailureAction`. Der `TitleText`-Eigenschafts Wert, der standardmäßig auf "Anmelden" festgelegt wird, wird oben auf der Benutzeroberfläche des Steuer Elements angezeigt. Ich habe diese Eigenschaft so festgelegt, dass Sie den Text "Sign in" als `<h3>`-Element anzeigt. Die `FailureAction`-Eigenschaft gibt an, was geschehen soll, wenn die Anmelde Informationen des Benutzers ungültig sind. Standardmäßig wird der Wert `Refresh`verwendet, der den Benutzer auf derselben Seite verlässt und eine Fehlermeldung im Anmelde Steuerelement anzeigt. Ich habe Sie in "`RedirectToLoginPage`" geändert, wodurch der Benutzer im Falle ungültiger Anmelde Informationen an die Anmeldeseite gesendet wird. Ich ziehe es vor, den Benutzer an die Anmeldeseite zu senden, wenn ein Benutzer versucht, sich auf einer anderen Seite anzumelden, aber fehlschlägt, da die Anmeldeseite zusätzliche Anweisungen und Optionen enthalten kann, die nicht problemlos in die linke Spalte passen. Beispielsweise kann die Anmeldeseite Optionen zum Abrufen eines vergessenen Kennworts oder zum Erstellen eines neuen Kontos enthalten.

### <a name="creating-the-login-page-and-overriding-the-default-content"></a>Erstellen der Anmeldeseite und Überschreiben der Standard Inhalte

Wenn die Master Seite fertiggestellt ist, ist der nächste Schritt das Erstellen der Anmeldeseite. Fügen Sie dem Stammverzeichnis Ihrer Website eine ASP.NET-Seite mit dem Namen `Login.aspx`hinzu, und binden Sie Sie an die `Site.master` Master Seite. Dadurch wird eine Seite mit vier Inhalts Steuerelementen erstellt, eine für jeden der in `Site.master`definierten contentplatzhalter.

Fügen Sie dem `MainContent`-Inhalts Steuerelement ein Anmelde Steuerelement hinzu. Ebenso können Sie dem `LeftColumnContent` Bereich Inhalte hinzufügen. Achten Sie jedoch darauf, das Inhalts Steuerelement für den `QuickLoginUI` contentplachalter leer zu lassen. Dadurch wird sichergestellt, dass das Anmelde Steuerelement nicht in der linken Spalte der Anmeldeseite angezeigt wird.

Nachdem Sie den Inhalt für die `MainContent`-und `LeftColumnContent` Regionen definiert haben, sollte das deklarative Markup Ihrer Anmeldeseite in etwa wie folgt aussehen:

[!code-aspx[Main](multiple-contentplaceholders-and-default-content-cs/samples/sample5.aspx)]

In Abbildung 7 wird diese Seite angezeigt, wenn Sie in einem Browser angezeigt wird. Da auf dieser Seite ein Inhalts Steuerelement für den `QuickLoginUI` contentplachalter festgelegt wird, wird der auf der Master Seite angegebene Standard Inhalt überschrieben. Der Nettoeffekt ist, dass das im Designansicht der Master Seite angezeigte Anmelde Steuerelement (siehe Abbildung 6) auf dieser Seite nicht gerendert wird.

[![die Anmeldeseite den Standard Inhalt von quickloginui-contentplachalter erneut drückt](multiple-contentplaceholders-and-default-content-cs/_static/image20.png)](multiple-contentplaceholders-and-default-content-cs/_static/image19.png)

**Abbildung 07**: die Anmeldeseite drückt den Standard Inhalt des `QuickLoginUI` contentplachalter ([Klicken Sie, um das Bild in voller Größe anzuzeigen](multiple-contentplaceholders-and-default-content-cs/_static/image21.png))

### <a name="using-the-default-content-in-new-pages"></a>Verwenden des Standard Inhalts auf neuen Seiten

Wir möchten das Login-Steuerelement in der linken Spalte für alle Seiten mit Ausnahme der Anmeldeseite anzeigen. Um dies zu erreichen, sollten alle Inhaltsseiten außer der Anmeldeseite ein Inhalts Steuerelement für den `QuickLoginUI` contentplachalter weglassen. Wenn Sie ein Inhalts Steuerelement weglassen, wird stattdessen der Standard Inhalt des contentplachalter-Elements verwendet.

Die vorhandenen Inhaltsseiten `Default.aspx`, `About.aspx`und `MultipleContentPlaceHolders.aspx` enthalten kein Inhalts Steuerelement für `QuickLoginUI`, da Sie erstellt wurden, bevor das contentplachalter-Steuerelement der Master Seite hinzugefügt wurde. Daher müssen diese vorhandenen Seiten nicht aktualisiert werden. Neue Seiten, die der Website hinzugefügt werden, enthalten jedoch standardmäßig ein Inhalts Steuerelement für den `QuickLoginUI` contentplachalter. Daher müssen Sie diese Inhalts Steuerelemente jedes Mal entfernen, wenn Sie eine neue Inhaltsseite hinzufügen (es sei denn, wir möchten den Standard Inhalt von contentplachalter überschreiben, wie im Fall der Anmeldeseite).

Um das Inhalts Steuerelement zu entfernen, können Sie sein deklaratives Markup entweder manuell aus der Quell Ansicht löschen oder aus dem Designansicht den Link Standard für die Inhalts Verknüpfung des Masters aus dem Smarttag auswählen. Beide Ansätze entfernen das Inhalts Steuerelement aus der Seite und erzeugen denselben Nettoeffekt.

Abbildung 8 zeigt `Default.aspx`, wenn Sie in einem Browser angezeigt werden. Beachten Sie, dass `Default.aspx` nur zwei Inhalts Steuerelemente enthält, die im deklarativen Markup angegeben sind: einen für `head` und einen für `MainContent`. Folglich werden die Standard Inhalte für die `LeftColumnContent`-und `QuickLoginUI`-contentplatzhalter angezeigt.

[![der Standard Inhalt für die contentplatzhalter "leftcolumncontent" und "quickloginui" angezeigt.](multiple-contentplaceholders-and-default-content-cs/_static/image23.png)](multiple-contentplaceholders-and-default-content-cs/_static/image22.png)

**Abbildung 08**: der Standard Inhalt für die `LeftColumnContent`-und `QuickLoginUI`-contentplatzhalter wird angezeigt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](multiple-contentplaceholders-and-default-content-cs/_static/image24.png))

## <a name="summary"></a>Summary

Mit dem ASP.NET-Masterseiten Modell können beliebig viele contentplatzhalter auf der Master Seite angezeigt werden. Außerdem enthalten contentplatzhalter Standard Inhalt, der ausgegeben wird, wenn kein entsprechendes Inhalts Steuerelement auf der Inhaltsseite vorhanden ist. In diesem Tutorial haben Sie erfahren, wie Sie zusätzliche contentplachalter-Steuerelemente in die Master Seite einfügen und Inhalts Steuerelemente für diese neuen Inhalts Platzhalter auf neuen und vorhandenen ASP.NET-Seiten definieren. Außerdem haben wir uns mit der Angabe von Standard Inhalten in einem contentplachalter beschäftigt. Dies ist in Szenarios nützlich, in denen nur eine Minderheit von Seiten den anderweitig standardisierten Inhalt innerhalb einer bestimmten Region anpassen muss.

Im nächsten Tutorial untersuchen wir den `head` contentplachalter ausführlicher und erfahren, wie Sie den Titel, die Meta-Tags und andere HTML-Header auf seitenweise deklarativ und Programm gesteuert definieren.

Fröhliche Programmierung!

### <a name="about-the-author"></a>Informationen zum Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor mehrerer ASP/ASP. net-Bücher und Gründer von 4GuysFromRolla.com, hat seit 1998 mit Microsoft-Webtechnologien gearbeitet. Scott arbeitet als unabhängiger Berater, Ausbilder und Writer. Sein letztes Buch ist [*Sams Teach Yourself ASP.NET 3,5 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Scott kann über [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog unter [http://ScottOnWriting.NET](http://scottonwriting.net/)erreicht werden.

### <a name="special-thanks-to"></a>Besonders vielen Dank

Diese tutorialreihe wurde von vielen hilfreichen Reviewern geprüft. Lead Reviewer für dieses Tutorial war Suchi Banerjee. Möchten Sie meine bevorstehenden MSDN-Artikel überprüfen? Wenn dies der Fall ist, können Sie eine Zeile in [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)ablegen.

> [!div class="step-by-step"]
> [Zurück](creating-a-site-wide-layout-using-master-pages-cs.md)
> [Weiter](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs.md)
