---
uid: web-forms/overview/getting-started/hands-on-labs/using-page-inspector-in-visual-studio-2012
title: Verwenden von Seitenprüfung in Visual Studio 2012 | Microsoft-Dokumentation
author: rick-anderson
description: 'In dieser praktischen Übungseinheit finden Sie ein neues Tool zum Suchen und Beheben von Webseiten Problemen in Visual Studio: der Seitenprüfung. Seitenprüfung ist ein neues Tool, das b...'
ms.author: riande
ms.date: 02/18/2013
ms.assetid: 73232292-a5fe-4720-82a1-8f6553effd1f
msc.legacyurl: /web-forms/overview/getting-started/hands-on-labs/using-page-inspector-in-visual-studio-2012
msc.type: authoredcontent
ms.openlocfilehash: f42b1be2697ba7d1145b3e334fe8f4ebf019cd12
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78473799"
---
# <a name="using-page-inspector-in-visual-studio-2012"></a>Verwenden der Seitenprüfung in Visual Studio 2012

vom [Web Camps-Team](https://twitter.com/webcamps)

> In dieser praktischen Übungseinheit finden Sie ein neues Tool zum Suchen und Beheben von Webseiten Problemen in Visual Studio: der Seitenprüfung.
> 
> Seitenprüfung ist ein neues Tool, das Browser Diagnosetools in Visual Studio integriert und eine integrierte Darstellung zwischen Browser, ASP.net und Quellcode bietet. Es rendert eine Webseite (HTML, Web Forms, ASP.NET MVC oder Web Pages) direkt in der Visual Studio-IDE und ermöglicht Ihnen die Untersuchung des Quellcodes und der resultierenden Ausgabe. Mit Seitenprüfung können Sie problemlos eine Website zerlegen, schnell Seiten von Grund auf Erstellen und Probleme schnell diagnostizieren.
> 
> Heutzutage verfügen wir über eine Reihe von Webframe Works, die flexible und skalierbare Websites zeitnah erstellen, z. b. ASP.NET MVC und WebForms. Andererseits ist es schwieriger, Probleme auf den Seiten zu finden, da die IDE die Designer Ansicht in Vorlagen basierten Seiten und dynamischen Inhalten nicht unterstützt. Daher müssen diese Websites in einem Browser geöffnet werden, um zu sehen, wie Sie für einen Benutzer angezeigt werden.
> 
> Webentwickler verwenden externe Tools, um Probleme zu finden, die regelmäßig im Browser ausgeführt werden. Anschließend kehren Sie zur IDE zurück und beginnen mit der Korrektur. Diese hin-und hergeleitet-Aktivität zwischen der IDE, dem Browser und den Profil Erstellungs Tools kann ineffizient sein und manchmal eine neue Bereitstellung und Cache Bereinigung erfordern, wenn Sie ein Problem reproduzieren möchten.
> 
> Seitenprüfung Brücken eine Lücke bei der Webentwicklung zwischen dem Client (Browser Tools) und dem Server (ASP.net und Quellcode), indem Sie das Beste aus beiden Welten mithilfe eines kombinierten Satzes von Features zusammenbringen.
> 
> Mithilfe Seitenprüfung können Sie feststellen, welche Elemente in den Quelldateien (einschließlich Server seitigem Code) das HTML-Markup erstellt haben, das im Browser gerendert werden soll. Seitenprüfung können Sie auch CSS-Eigenschaften und DOM-Element Attribute ändern, um die Änderungen anzuzeigen, die sofort im Browser reflektiert werden.
> 
> Diese praktische Übungseinheit führt Sie durch die Seitenprüfung Features und zeigt Ihnen, wie Sie Sie verwenden können, um Probleme in Webanwendungen zu beheben. **Diese Übungseinheit enthält zwei Übungen, die ähnliche Flows verwenden, aber auf verschiedene Technologien abzielen. Wenn Sie ASP.NET MVC-Entwickler sind, führen Sie die folgenden Schritte aus: Wenn Sie ein WebForms-Entwickler sind, befolgen Sie die Übung zwei**.
> 
> Diese Übungseinheit führt Sie durch die Verbesserungen und neuen Features, die Sie zuvor beschrieben haben, indem Sie kleinere Änderungen an einer im Quellordner bereitgestellten beispielweb Anwendung anwenden.
> 
> Der gesamte Beispielcode und die Code Ausschnitte sind im Web Camps-Trainingskit enthalten, das unter [https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409](https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409)verfügbar ist.

<a id="Objectives"></a>

<a id="Objectives"></a>
### <a name="objectives"></a>Ziele

In dieser praktischen Übungseinheit erfahren Sie Folgendes:

- Zerlegen einer Website mithilfe von Seitenprüfung
- Überprüfen und Anzeigen von CSS-Formatvorlagen Änderungen mit Seitenprüfung
- Erkennen und beheben Sie Probleme in ihren Webseiten mithilfe von Seitenprüfung

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Voraussetzungen

Zum Durchführen dieses Labs müssen Sie über Folgendes verfügen:

- [Microsoft Visual Studio Express 2012 für Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) oder Superior (Informationen zur Installation finden Sie in [Anhang A](#AppendixA) ).
- Internet Explorer 9 oder höher

---

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a>Exerzitien

Diese praktische Übungseinheit umfasst die folgenden Übungen:

1. [Übung 1: Verwenden von Seitenprüfung in ASP.NET-MVC-Projekten](#Exercise1)
2. [Übung 2: Verwenden von Seitenprüfung in WebForms-Projekten](#Exercise2)

> [!NOTE]
> Jede Übung wird von einer Start Lösung begleitet, die sich im Ordner BEGIN der Übung befindet und Ihnen ermöglicht, die einzelnen Übungen unabhängig von den anderen zu befolgen. Innerhalb des Quellcodes für eine Übung finden Sie auch einen endordner mit einer Visual Studio-Projekt Mappe mit dem Code, der aus der Ausführung der Schritte in der entsprechenden Übung resultiert. Sie können diese Lösungen als Leitfaden verwenden, wenn Sie zusätzliche Hilfe benötigen, während Sie diese praktische Übungseinheit durcharbeiten.

Geschätzte Zeit bis zum Abschluss dieses Labs: **30 Minuten**.

<a id="Exercise1"></a>

<a id="Exercise_1_Using_Page_Inspector_in_ASPNET_MVC_Projects"></a>
### <a name="exercise-1-using-page-inspector-in-aspnet-mvc-projects"></a>Übung 1: Verwenden von Seitenprüfung in ASP.NET-MVC-Projekten

In dieser Übung erfahren Sie, wie Sie eine **ASP.NET MVC 4** -Lösung mithilfe von **Seitenprüfung**anzeigen und Debuggen. Zuerst führen Sie eine kurze Runde um das Tool aus, um die Funktionen zu erlernen, die den webdebugprozess vereinfachen. Anschließend arbeiten Sie auf einer Webseite, die Formatierungs Probleme enthält. Sie erfahren, wie Sie Seitenprüfung verwenden können, um den Quellcode zu finden, der das Problem generiert, und das Problem zu beheben.

<a id="Ex1Task1"></a>

<a id="Task_1_-_Exploring_Page_Inspector"></a>
#### <a name="task-1---exploring-page-inspector"></a>Aufgabe 1: Untersuchen von Seitenprüfung

In dieser Aufgabe erfahren Sie, wie die Seitenprüfung im Kontext eines ASP.NET MVC 4-Projekts verwendet wird, das eine Fotogalerie anzeigt.

1. Öffnen Sie die Projekt Mappe " **Anfang** " unter **Quelle/EX1-MVC4/BEGIN/** Folder.

   1. Sie müssen einige fehlende nuget-Pakete herunterladen, bevor Sie den Vorgang fortsetzen. Klicken Sie hierzu auf das Menü **Projekt** , und wählen Sie **nuget-Pakete verwalten**aus.
   2. Klicken Sie im Dialogfeld **nuget-Pakete verwalten** auf **Wiederherstellen** , um fehlende Pakete herunterzuladen.
   3. Erstellen Sie abschließend die Projekt Mappe, indem Sie auf **Build** ** | Projekt Mappe erstellen klicken**.

      > [!NOTE]
      > Einer der Vorteile der Verwendung von nuget besteht darin, dass Sie nicht alle Bibliotheken in Ihrem Projekt liefern müssen, um die Projektgröße zu verringern. Mit nuget Power Tools können Sie durch Angabe der Paketversionen in der Datei "Packages. config" alle erforderlichen Bibliotheken herunterladen, wenn Sie das Projekt zum ersten Mal ausführen. Aus diesem Grund müssen Sie diese Schritte ausführen, nachdem Sie eine vorhandene Projekt Mappe in diesem Lab geöffnet haben.
2. Suchen Sie im Projektmappen-Explorer die Ansicht **Index. cshtml** unter dem Projektordner **/views/Home** , klicken Sie mit der rechten Maustaste darauf, und wählen Sie **in Seitenprüfung anzeigen**aus.

    ![Auswählen einer Datei für die Vorschau in Seitenprüfung](using-page-inspector-in-visual-studio-2012/_static/image1.png "Auswählen einer Datei für die Vorschau in Seitenprüfung")

    *Auswählen einer Datei für die Vorschau in Seitenprüfung*
3. Im Fenster Seitenprüfung wird die */Home/Index* -URL angezeigt, die der ausgewählten Quell Ansicht zugeordnet ist.

    ![Der erste Kontakt mit pageingespector](using-page-inspector-in-visual-studio-2012/_static/image2.png)

    *Der erste Kontakt mit Seitenprüfung*

    Das Seitenprüfung Tool ist in Ihre Visual Studio-Umgebung integriert. Der Inspektor enthält einen eingebetteten Browser sowie einen leistungsfähigen HTML-Profiler. Beachten Sie, dass Sie die Lösung nicht ausführen müssen, um zu sehen, wie Ihre Seiten aussehen.

    > [!NOTE]
    > Wenn die Breite Seitenprüfung Browsers kleiner als die Breite der geöffneten Seite ist, wird die Seite nicht ordnungsgemäß angezeigt. Wenn dies der Fall ist, passen Sie die Breite des Seitenprüfung an.
4. Klicken Sie in Seitenprüfung auf die Registerkarte **Dateien** .

    Alle Quelldateien, die die Index Seite bilden, werden angezeigt. Mit dieser Funktion können alle Elemente auf einen Blick identifiziert werden, insbesondere wenn Sie mit partiellen Sichten und Vorlagen arbeiten. Beachten Sie, dass Sie auch alle Dateien öffnen können, wenn Sie auf die Verknüpfungen klicken.

    ![Die Registerkarte "-Files-"](using-page-inspector-in-visual-studio-2012/_static/image3.png)

    *Registerkarte "Dateien"*
5. Klicken Sie auf die Schaltfläche **Überprüfungsmodus umschalten** auf der linken Seite der Registerkarten.

    Mit diesem Tool können Sie ein beliebiges Element der Seite auswählen und den HTML-und Razor-Code anzeigen.

    ![Schaltfläche "umschalten-Überprüfung-Modus"](using-page-inspector-in-visual-studio-2012/_static/image4.png)

    *Schaltfläche "Überprüfungsmodus umschalten"*
6. Bewegen Sie im Seitenprüfung Browser den Mauszeiger über die Seitenelemente. Während Sie den Mauszeiger über einen beliebigen Teil der gerenderten Seite bewegen, wird der Elementtyp angezeigt, und das entsprechende Quell Markup bzw. der entsprechende Code wird im Visual Studio-Editor hervorgehoben.

    ![Inspektions Modus in Aktion](using-page-inspector-in-visual-studio-2012/_static/image5.png)

    *Inspektions Modus in Aktion*

    > [!NOTE]
    > Maximieren Sie das Seitenprüfung Fenster nicht, oder Sie können die Vorschau Registerkarte mit dem Quellcode nicht anzeigen. Wenn Sie in Seitenprüfung auf das-Element klicken, wenn es maximiert ist, wird der Quellcode der Auswahl angezeigt, aber das Seitenprüfung Fenster wird ausgeblendet.

    Wenn Sie die Datei " **Index. cshtml** " beachten, werden Sie feststellen, dass der Teil des Quellcodes, der das ausgewählte Element generiert, hervorgehoben wird. Diese Funktion vereinfacht die Bearbeitung von langen Quelldateien und bietet eine direkte und schnelle Möglichkeit, auf den Code zuzugreifen.

    ![Überprüfen von Elementen](using-page-inspector-in-visual-studio-2012/_static/image6.png)

    *Überprüfen von Elementen*
7. Klicken Sie auf die Schaltfläche **Überprüfungsmodus umschalten** (klicken![Sie auf die Registerkarte HTML, um den im Seitenprüfung Browser gerenderten HTML-Code anzuzeigen.](using-page-inspector-in-visual-studio-2012/_static/image7.png "Wählen Sie die Registerkarte HTML aus, um den im Seitenprüfung Browser gerenderten HTML-Code anzuzeigen.") ), um den Cursor zu deaktivieren.
8. Wählen Sie die Registerkarte **HTML** aus, um den im Seitenprüfung Browser gerenderten HTML-Code anzuzeigen.
9. Suchen Sie im HTML-Markup das Listenelement mit dem Link "Koala", und wählen Sie es aus.

    Beachten Sie, dass die entsprechende Ausgabe beim Auswählen des Codes automatisch im Browser hervorgehoben wird. Diese Funktion ist nützlich, um zu sehen, wie ein HTML-Block auf der Seite gerendert wird.

    ![HTML-Element auf der Seite wird ausgewählt](using-page-inspector-in-visual-studio-2012/_static/image8.png "HTML-Element auf der Seite wird ausgewählt")

    *HTML-Element auf der Seite wird ausgewählt*
10. Klicken Sie auf die Schaltfläche **Überprüfungsmodus** umschalten, um *Überprüfungsmodus* zu aktivieren, und klicken Sie auf die Navigationsleiste. Auf der rechten Seite des HTML-Codes wird im Bereich Stile eine Liste mit den CSS-Stilen angezeigt, die auf das ausgewählte Element angewendet werden.

    > [!NOTE]
    > Da der Header Teil des Site Layouts ist, wird Seitenprüfung auch \_Layout. cshtml-Datei öffnen und das Segment des betroffenen Codes hervorheben.

    ![Entdecken von Stilen](using-page-inspector-in-visual-studio-2012/_static/image9.png)

    *Erkennen von Stilen und Quelldateien eines ausgewählten Elements*
11. Bewegen Sie den Mauszeiger mit aktiviertem aktiviertem Überprüfungs Zeiger in die blaue Balken Leiste, und klicken Sie auf den Halbkreis.

    ![Auswählen eines Elements](using-page-inspector-in-visual-studio-2012/_static/image10.png "Auswählen eines Elements")

    *Auswählen eines Elements*
12. Suchen Sie im Bereich Stile nach dem Element **Hintergrundbild** unter der Gruppe **. Main-Content** . **Deaktivieren Sie** das **Hintergrundbild** , und sehen Sie sich an, was passiert. Sie werden feststellen, dass der Browser die Änderungen sofort widerspiegelt und der Kreis ausgeblendet ist.

    > [!NOTE]
    > Die Änderungen, die Sie auf der Registerkarte Seitenprüfung Stile anwenden, wirken sich nicht auf das ursprüngliche Stylesheet aus. Sie können Stile deaktivieren oder ihre Werte beliebig oft ändern, aber Sie werden nach dem Aktualisieren der Seite wieder hergestellt.

    ![Aktivieren und Deaktivieren von CSS-Stilen](using-page-inspector-in-visual-studio-2012/_static/image11.png "Aktivieren und Deaktivieren von CSS-Stilen")

    *Aktivieren und Deaktivieren von CSS-Stilen*
13. Klicken Sie nun mit dem Überprüfungs Modus auf den Text "**Ihr Logo hier**" in der Kopfzeile.
14. Suchen Sie auf der Registerkarte **Stile** das CSS **-Attribut Schriftart Größe** unter der Gruppe **. Site-Title** . Doppelklicken Sie auf den Attribut Wert, und ersetzen Sie den Wert 2,3 EM durch **3 EM**, und drücken Sie dann die **Eingabe**Taste. Beachten Sie, dass der Titel größer aussieht.

    ![Ändern von CSS-Werten im Seitenprüfung](using-page-inspector-in-visual-studio-2012/_static/image12.png "Ändern von CSS-Werten im Seitenprüfung")

    *Ändern von CSS-Werten im Seitenprüfung*
15. Klicken Sie auf die Registerkarte Ablauf **Verfolgungs Stile** , die sich im rechten Bereich Seitenprüfung befindet. Dies ist eine alternative Methode, um alle Stile anzuzeigen, die auf die Auswahl angewendet werden, geordnet nach Attribut Name.

    ![CSS-Formatvorlagen Verfolgung](using-page-inspector-in-visual-studio-2012/_static/image13.png)

    *CSS-Stile-Ablauf Verfolgung des ausgewählten Elements*
16. Ein weiteres Feature von Seitenprüfung ist der Layoutbereich. Wählen Sie im Inspektions Modus die Navigationsleiste aus, und klicken Sie dann im rechten Bereich auf die Registerkarte **Layout** . Sie sehen die genaue Größe des ausgewählten Elements sowie dessen Offset, Rand, Auffüllung und Rahmengröße. Beachten Sie, dass Sie auch die Werte in dieser Ansicht ändern können.

    ![Element Layout in Seitenprüfung](using-page-inspector-in-visual-studio-2012/_static/image14.png "Element Layout in Seitenprüfung")

    *Element Layout in Seitenprüfung*

<a id="Ex1Task2"></a>

<a id="Task_2_-_Finding_and_Fixing_Style_Issues_in_the_Photo_Gallery"></a>
#### <a name="task-2---finding-and-fixing-style-issues-in-the-photo-gallery"></a>Aufgabe 2: Suchen und Beheben von Stil Problemen in der Fotogalerie

Wie würden Sie Webseiten Probleme mit früheren Versionen von Visual Studio diagnostizieren? Sie sind wahrscheinlich mit webdebuggingtools vertraut, die außerhalb der Visual Studio-IDE ausgeführt werden, z. b. Internet Explorer Entwicklertools oder Firebug. Browser verstehen nur HTML, Skripterstellung und Stile, während ein zugrunde liegendes Framework den HTML-Code generiert, der gerendert wird. Aus diesem Grund müssen Sie häufig den gesamten Standort bereitstellen, um zu sehen, wie Webseiten aussehen.

Diese Schritte wurden wahrscheinlich befolgt, wenn Sie ein Problem auf Ihrer Website erkennen und beheben wollten:

1. Führen Sie die Projekt Mappe in Visual Studio aus, oder stellen Sie die Seite auf dem Webserver bereit.
2. Öffnen Sie im Browser die verwendeten Entwicklertools, oder öffnen Sie einfach den Quellcode und die Stile, und versuchen Sie, das Problem abzugleichen. Um die beteiligten Dateien zu finden, haben Sie den &quot;Search-&quot; oder &quot;in Dateien suchen&quot; Features mit dem Namen der Stil Klassen verwendet.
3. Nachdem der Fehler erkannt wurde, können Sie den Webbrowser und den Server unterbinden.
4. Löschen Sie den Browsercache.
5. Kehren Sie zu Visual Studio zurück, um ein Problem zu beheben. Wiederholen Sie alle zu testenden Schritte.

Da es in ASP.NET MVC 4 keinen echten WYSIWYG gibt, werden die meisten Stil Probleme in einer späteren Phase nach dem Ausführen oder Bereitstellen der Webanwendung erkannt. Nun können Sie mit Seitenprüfung eine beliebige Seite anzeigen, ohne die Projekt Mappe ausführen zu müssen.

In dieser Aufgabe verwenden Sie die Seiten Prüfung und beheben einige Probleme mit der Foto Gallery-Anwendung.

1. Suchen Sie mithilfe der Seitenprüfung auf der linken Seite des Headers nach den Links **Register** **und anmelden.**

    Beachten Sie, dass die Links nicht an der erwarteten Stelle auf der rechten Seite angezeigt werden, und Sie werden wie eine Auflistungs Liste angezeigt. Nun werden die Links rechtsbündig ausgerichtet und entsprechend formatieren.

    ![Suchen der Links "registrieren" und "Anmelden"](using-page-inspector-in-visual-studio-2012/_static/image15.png "Suchen der Links "registrieren" und "Anmelden"")

    *Suchen der Links "registrieren" und "Anmelden"*
2. Klicken Sie bei ausgewähltem umschalten Überprüfungsmodus auf Schließen bis, aber nicht auf den Link registrieren, um den Code zu öffnen.

    Beachten Sie, dass sich der Quellcode der Links in der **\_loginpartial. cshtml** -Datei befindet, nicht in der Datei "index. cshtml" und in der \_"Layout. cshtml", bei der es sich um die Orte handelt, an denen Sie möglicherweise zuerst suchen. Sie wurden direkt in die richtige Quelldatei eingefügt.
3. Suchen Sie auf **der Registerkarte** Formatvorlagen den **Abschnitt\<> #Login** Element, der der HTML-Container für diese Links ist, und klicken Sie darauf.

    Beachten Sie, dass sich der **#Login** Stil automatisch in " **Site. CSS** " befindet, nachdem Sie darauf klicken. Außerdem ist der Code jetzt hervorgehoben.

    ![Auswählen der CSS-Stile](using-page-inspector-in-visual-studio-2012/_static/image16.png "Auswählen der CSS-Stile")

    *Auswählen der CSS-Stile*
4. Entfernen Sie das **Text-align-** Attribut im hervorgehobenen Code, indem Sie die öffnenden und schließenden Zeichen entfernen und die Datei " **Site. CSS** " speichern.

    Seitenprüfung kennt alle unterschiedlichen Dateien, die die aktuelle Seite bilden, und kann erkennen, wenn sich eine dieser Dateien ändert. Sie werden benachrichtigt, wenn die aktuelle Seite im Browser nicht mit den Quelldateien synchron ist.
5. Klicken Sie im Seitenprüfung Browser auf die Leiste unter der Adressleiste, um die Seite neu zu laden.

    ![Die Seite wird erneut geladen.](using-page-inspector-in-visual-studio-2012/_static/image17.png)

    *Die Seite wird erneut geladen.*

    Die Links befinden sich jetzt auf der rechten Seite, aber Sie sehen weiterhin wie eine Auflistungs Liste aus. Nun entfernen Sie die Aufzählungs Zeichen und richten die Verknüpfungen horizontal aus.

    ![Aktualisierte Seite](using-page-inspector-in-visual-studio-2012/_static/image18.png)

    *Aktualisierte Seite*
6. Wählen Sie im Überprüfungs Modus eine der **&lt;Li&gt;** Elemente aus, die die &quot;Register&quot; und &quot;anmelden&quot; Links enthalten. Klicken Sie dann auf den **&lt;Abschnitt&gt; #Login** Element, um auf **Stile. CSS** -Code zuzugreifen.

    ![Suchen des Stils](using-page-inspector-in-visual-studio-2012/_static/image19.png "Suchen des Stils")

    *Suchen des Stils*
7. Heben Sie in " **Style. CSS**" die Auskommentierung des Codes für **#Login Li** Items auf. Der Stil, den Sie hinzufügen, verbirgt das Aufzählungs Zeichen und zeigt die Elemente horizontal an.

    ![Umgestalten der Anmelde Verknüpfungen](using-page-inspector-in-visual-studio-2012/_static/image20.png "Umgestalten der Anmelde Verknüpfungen")

    *Umgestalten der Anmelde Verknüpfungen*
8. Speichern Sie die Datei **Style. CSS** , und klicken Sie auf die Leiste unterhalb der Adresse, um die Seite neu zu laden. Beachten Sie, dass die Links ordnungsgemäß angezeigt werden.

    ![Links, die an der rechten Seite ausgerichtet sind](using-page-inspector-in-visual-studio-2012/_static/image21.png "Links, die an der rechten Seite ausgerichtet sind")

    *Links, die an der rechten Seite ausgerichtet sind*
9. Zum Schluss ändern Sie den Titel des Headers. Verwenden Sie den Inspektions Modus, um auf **Ihr Logo hier** zu klicken und zum Quellcode zu gelangen, der ihn generiert.
10. Sie befinden sich nun in **\_"Layout. cshtml**", indem Sie den Text "**your Logo here**" durch "**Photo Gallery**" ersetzen. Speichern und aktualisieren Sie den Seitenprüfung Browser.

    ![Zuweisen eines neuen Titels](using-page-inspector-in-visual-studio-2012/_static/image22.png "Zuweisen eines neuen Titels")

    *Zuweisen eines neuen Titels*

    ![Fotogalerie (Seite)](using-page-inspector-in-visual-studio-2012/_static/image23.png)

    *Fotogalerie Seite aktualisiert*
11. Wählen Sie abschließend das Projekt **Photogallery** aus, und drücken Sie **F5** , um die APP auszuführen. Sehen Sie sich alle Änderungen wie erwartet an.

---

<a id="Exercise2"></a>

<a id="Exercise_2_Using_Page_Inspector_in_WebForms_Projects"></a>
### <a name="exercise-2-using-page-inspector-in-webforms-projects"></a>Übung 2: Verwenden von Seitenprüfung in WebForms-Projekten

In dieser Übung erfahren Sie, wie Sie eine WebForms-Projekt Mappe mithilfe von Seitenprüfung anzeigen und Debuggen. Sie führen zunächst eine kurze Runde um das Tool aus, um die Seitenprüfung Features kennenzulernen, die den webdebugvorgang vereinfachen. Anschließend arbeiten Sie auf einer Webseite, die Formatierungs Probleme enthält. Sie erfahren, wie Sie Seitenprüfung verwenden können, um den Quellcode zu finden, der das Problem generiert, und das Problem zu beheben.

<a id="Ex2Task1"></a>

<a id="Task_1_-_Exploring_Page_Inspector"></a>
#### <a name="task-1---exploring-page-inspector"></a>Aufgabe 1: Untersuchen von Seitenprüfung

In dieser Aufgabe erfahren Sie, wie Sie die Seitenprüfung-Funktionen im Kontext eines WebForms-Projekts verwenden, das eine Fotogalerie anzeigt.

1. Öffnen Sie die Projekt Mappe " **Anfang** " unter " **Source/EX2-WebForms/BEGIN/** Folder".

   1. Sie müssen einige fehlende nuget-Pakete herunterladen, bevor Sie den Vorgang fortsetzen. Klicken Sie hierzu auf das Menü **Projekt** , und wählen Sie **nuget-Pakete verwalten**aus.
   2. Klicken Sie im Dialogfeld **nuget-Pakete verwalten** auf **Wiederherstellen** , um fehlende Pakete herunterzuladen.
   3. Erstellen Sie abschließend die Projekt Mappe, indem Sie auf **Build** ** | Projekt Mappe erstellen klicken**.

      > [!NOTE]
      > Einer der Vorteile der Verwendung von nuget besteht darin, dass Sie nicht alle Bibliotheken in Ihrem Projekt liefern müssen, um die Projektgröße zu verringern. Mit nuget Power Tools können Sie durch Angabe der Paketversionen in der Datei "Packages. config" alle erforderlichen Bibliotheken herunterladen, wenn Sie das Projekt zum ersten Mal ausführen. Aus diesem Grund müssen Sie diese Schritte ausführen, nachdem Sie eine vorhandene Projekt Mappe in diesem Lab geöffnet haben.
2. Suchen Sie im Projektmappen-Explorer die Seite **default. aspx** , klicken Sie mit der rechten Maustaste darauf, und wählen Sie **in Seitenprüfung anzeigen aus**.

    ![Öffnen von "default. aspx" mit Seitenprüfung](using-page-inspector-in-visual-studio-2012/_static/image24.png "Öffnen von "default. aspx" mit Seitenprüfung")

    *Öffnen von "default. aspx" mit Seitenprüfung*
3. Im Seitenprüfung Fenster wird default. aspx angezeigt.

    ![Anzeigen von "default. aspx" in Seitenprüfung](using-page-inspector-in-visual-studio-2012/_static/image25.png "Anzeigen von "default. aspx" in Seitenprüfung")

    *Anzeigen von "default. aspx" in Seitenprüfung*

    Das Seitenprüfung Tool ist in Ihre Visual Studio-Umgebung integriert. Der Inspektor enthält einen eingebetteten Browser sowie einen leistungsfähigen HTML-Profiler, der den ausgewählten Code anzeigt. Beachten Sie, dass Sie die Lösung nicht ausführen müssen, um zu sehen, wie Ihre Seiten aussehen.

    > [!NOTE]
    > Wenn die Breite Seitenprüfung Browsers kleiner als die Breite der geöffneten Seite ist, wird die Seite nicht ordnungsgemäß angezeigt. Wenn dies der Fall ist, passen Sie die Breite des Seitenprüfung an.
4. Klicken Sie in Seitenprüfung auf die Registerkarte **Dateien** .

    Alle Quelldateien, die die gerenderte Standardseite erstellen, werden angezeigt. Dies ist eine nützliche Funktion, um alle Elemente auf einen Blick zu identifizieren, insbesondere wenn Sie mit Benutzer Steuerelementen und Master Seiten arbeiten. Beachten Sie, dass Sie auch zu jeder der Dateien navigieren können.

    ![Registerkarte "Dateien"](using-page-inspector-in-visual-studio-2012/_static/image26.png "Registerkarte "Dateien"")

    *Registerkarte "Dateien"*
5. Klicken Sie auf die Schaltfläche **Überprüfungsmodus umschalten** auf der linken Seite der Registerkarten.

    Mit diesem Tool können Sie ein beliebiges Element der Seite auswählen und den HTML-Code und die ASPX-Quelle anzeigen.

    ![Schaltfläche "Überprüfungsmodus umschalten"](using-page-inspector-in-visual-studio-2012/_static/image27.png "Schaltfläche "Überprüfungsmodus umschalten"")

    *Schaltfläche "Überprüfungsmodus umschalten"*
6. Bewegen Sie den Mauszeiger über die Seitenelemente im Seitenprüfung Browser. Während Sie den Mauszeiger über einen beliebigen Teil der gerenderten Seite bewegen, wird der Elementtyp angezeigt, und das entsprechende Quell Markup bzw. der entsprechende Code wird im Visual Studio-Editor hervorgehoben.

    ![Inspektions Modus in Aktion](using-page-inspector-in-visual-studio-2012/_static/image28.png "Inspektions Modus in Aktion")

    *Inspektions Modus in Aktion*

    > [!NOTE]
    > Maximieren Sie das Seitenprüfung Fenster nicht, oder Sie können die Vorschau Registerkarte mit dem Quellcode nicht anzeigen. Wenn Sie in Seitenprüfung auf das-Element klicken, wenn es maximiert ist, wird der Quellcode der Auswahl angezeigt, aber das Seitenprüfung Fenster wird ausgeblendet.

    Wenn Sie die Datei " **default. aspx** " beachten, werden Sie feststellen, dass der Teil des Quellcodes, der das ausgewählte Element generiert, hervorgehoben wird. Diese Funktion vereinfacht die Edition von langen Quelldateien und bietet eine direkte und schnelle Möglichkeit, auf den Code zuzugreifen.

    ![Überprüfen von Elementen](using-page-inspector-in-visual-studio-2012/_static/image29.png "Überprüfen von Elementen")

    *Überprüfen von Elementen*
7. Klicken Sie auf die Schaltfläche **Überprüfungsmodus umschalten** (![Select-the-HTML-Tab-to-Display-the-HTML-Code-gerendert-in-the-page-Inspector-Browser.](using-page-inspector-in-visual-studio-2012/_static/image30.png "Select-the-HTML-Tab-to-Display-the-HTML-Code-gerendert-in-the-page-Inspector-Browser.") ), die sich in Seitenprüfung Registerkarten befinden, um den Cursor zu deaktivieren.
8. Wählen Sie die Registerkarte **HTML** aus, um den im Seitenprüfung Browser gerenderten HTML-Code anzuzeigen.
9. Suchen Sie im HTML-Code das Listenelement mit dem Link "Koala", und wählen Sie es aus.

    Beachten Sie, dass bei Auswahl des Codes die entsprechende Ausgabe automatisch hervorgehoben wird. Diese Funktion ist nützlich, um zu sehen, wie ein HTML-Block auf der Seite gerendert wird.

    ![Auswählen eines HTML-Elements auf der Seite](using-page-inspector-in-visual-studio-2012/_static/image31.png "Auswählen eines HTML-Elements auf der Seite")

    *Auswählen eines HTML-Elements auf der Seite*
10. Klicken Sie auf die Schaltfläche **Überprüfungsmodus** umschalten, um *Überprüfungsmodus* zu aktivieren, und klicken Sie auf die Navigationsleiste. Auf der rechten Seite des HTML-Codes wird im Bereich Stile eine Liste mit den CSS-Stilen angezeigt, die auf das ausgewählte Element angewendet werden.

    > [!NOTE]
    > Da der Header Teil des Site Layouts ist, wird Seitenprüfung auch die Datei "Site. Master" öffnen und das Segment des betroffenen Codes hervorheben.

    ![Entdecken von Stilen WebForms](using-page-inspector-in-visual-studio-2012/_static/image32.png "Erkennen von Stilen und Quelldateien eines ausgewählten Elements")

    *Erkennen von Stilen und Quelldateien eines ausgewählten Elements*
11. Wenn Sie den Schalter zum Umschalten der Überprüfung aktiviert haben, bewegen Sie den Mauszeiger unterhalb der Menüleiste, und klicken Sie auf den leeren Halbkreis.

    ![Auswählen eines Elements](using-page-inspector-in-visual-studio-2012/_static/image33.png "Auswählen eines Elements")

    *Auswählen eines Elements*
12. Suchen Sie im Bereich Stile nach dem Element **Hintergrundbild** unter der Gruppe **. Main-Content** . **Deaktivieren Sie** das **Hintergrundbild** , und sehen Sie sich an, was passiert. Sie werden feststellen, dass der Browser die Änderungen sofort widerspiegelt und der Kreis ausgeblendet ist.

    > [!NOTE]
    > Die Änderungen, die Sie auf der Registerkarte Seitenprüfung Stile anwenden, wirken sich nicht auf das ursprüngliche Stylesheet aus. Sie können Stile deaktivieren oder ihre Werte beliebig oft ändern, aber Sie werden nach dem Aktualisieren der Seite wieder hergestellt.

    ![Aktivieren und Deaktivieren von CSS-styles2](using-page-inspector-in-visual-studio-2012/_static/image34.png "Aktivieren und Deaktivieren von CSS-Stilen")

    *Aktivieren und Deaktivieren von CSS-Stilen*
13. Klicken Sie nun mit dem Überprüfungs Modus auf den Text "**Ihr** **Logo hier"** in der Kopfzeile.
14. Suchen Sie auf der Registerkarte **Stile** das CSS **-Attribut Schriftart Größe** unter der Gruppe **. Site-Title** . Doppelklicken Sie einmal auf das Attribut, um den Wert zu bearbeiten. Ersetzen Sie den 2.3 EM-Wert durch **3eM**, und drücken Sie dann die EINGABETASTE. Beachten Sie, dass der Titel größer aussieht.

    ![Ändern von CSS-Werten auf der Seite Inspector2](using-page-inspector-in-visual-studio-2012/_static/image35.png "Ändern von CSS-Werten im Seitenprüfung")

    *Ändern von CSS-Werten im Seitenprüfung*
15. Klicken Sie auf die Registerkarte Ablauf **Verfolgungs Stile** , die sich im rechten Bereich Seitenprüfung befindet. Dies ist eine alternative Methode, um alle Stile anzuzeigen, die auf die Auswahl angewendet werden, geordnet nach Attribut Name.

    ![CSS-Stile-Ablauf Verfolgung des ausgewählten Elements](using-page-inspector-in-visual-studio-2012/_static/image36.png "CSS-Stile-Ablauf Verfolgung des ausgewählten Elements")

    *CSS-Stile-Ablauf Verfolgung des ausgewählten Elements*
16. Ein weiteres Feature von Seitenprüfung ist der Layoutbereich. Wählen Sie im Inspektions Modus die Navigationsleiste aus, und klicken Sie dann im rechten Bereich auf die Registerkarte **Layout** . Sie sehen die genaue Größe des ausgewählten Elements sowie dessen Offset, Rand, Auffüllung und Rahmengröße. Beachten Sie, dass Sie auch die Werte in dieser Ansicht ändern können.

    ![Element Layout in Seitenprüfung](using-page-inspector-in-visual-studio-2012/_static/image37.png "Element Layout in Seitenprüfung")

    *Element Layout in Seitenprüfung*

<a id="Ex2Task2"></a>

<a id="Task_2_-_Finding_and_Fixing_Style_Issues_in_the_Photo_Gallery"></a>
#### <a name="task-2---finding-and-fixing-style-issues-in-the-photo-gallery"></a>Aufgabe 2: Suchen und Beheben von Stil Problemen in der Fotogalerie

Wie würden Sie Webseiten Probleme mit früheren Versionen von Visual Studio diagnostizieren? Sie sind wahrscheinlich mit webdebuggingtools vertraut, die außerhalb der Visual Studio-IDE ausgeführt werden, z. b. Internet Explorer Entwicklertools oder Firebug. Browser verstehen nur HTML, Skripterstellung und Stile, während ein zugrunde liegendes Framework den HTML-Code generiert, der gerendert wird. Aus diesem Grund müssen Sie häufig den gesamten Standort bereitstellen, um zu sehen, wie Webseiten aussehen.

Diese Schritte wurden wahrscheinlich befolgt, wenn Sie ein Problem auf Ihrer Website erkennen und beheben wollten:

1. Führen Sie die Projekt Mappe in Visual Studio aus, oder stellen Sie die Seite auf dem Webserver bereit.
2. Öffnen Sie im Browser die verwendeten Entwicklertools, oder öffnen Sie einfach den Quellcode und die Stile, und versuchen Sie, das Problem abzugleichen. Um die beteiligten Dateien zu finden, haben Sie den &quot;Search&quot; verwendet, oder &quot;in Dateien suchen&quot; Features mit dem Namen der Formatklassen.
3. Nachdem der Fehler erkannt wurde, können Sie den Webbrowser und den Server unterbinden.
4. Löschen Sie den Browsercache.
5. Kehren Sie zu Visual Studio zurück, um ein Problem zu beheben. Wiederholen Sie alle zu testenden Schritte.

Da es in ASP.net WebForms keinen echten WYSIWYG gibt, werden einige Formatierungs Probleme in einer späteren Phase nach der Ausführung oder der Bereitstellung erkannt. Nun können Sie mit Seitenprüfung eine beliebige Seite anzeigen, ohne die Projekt Mappe ausführen zu müssen.

In dieser Aufgabe verwenden Sie die Seiten Prüfung, um einige Probleme der Fotogalerie Anwendung zu beheben. In den folgenden Schritten erkennen und beheben Sie schnell einige einfache Formatierungs Probleme in der Kopfzeile.

1. Suchen Sie mithilfe der Seiten Überprüfung auf der linken Seite des Headers nach den Links **registrieren** **und anmelden.**

    Beachten Sie, dass der Link nicht an der erwarteten Stelle auf der rechten Seite angezeigt wird. Nun wird der Link rechtsbündig ausgerichtet und entsprechend formatieren.

    ![Link zum Anmelden auf der linken Seite](using-page-inspector-in-visual-studio-2012/_static/image38.png "Link zum Anmelden auf der linken Seite")

    *Link zum Anmelden auf der linken Seite*
2. Klicken Sie bei ausgewähltem umschalten Überprüfungsmodus auf den Link anmelden, um den Code zu öffnen.

    Beachten Sie, dass sich der Link Quellcode in der Datei " **Site. Master** " befindet, nicht auf der Seite "default. aspx". Dies ist die Stelle, an der Sie zuerst suchen werden. Sie wurden direkt in die richtige Quelldatei eingefügt.
3. Suchen Sie auf **der Registerkarte** Formatvorlagen den **Abschnitt&lt;&gt; #Login** Element, der der HTML-Container für diese Links ist, und klicken Sie darauf.

    Beachten Sie, dass sich der **#Login** Stil automatisch in " **Site. CSS** " befindet, nachdem Sie darauf klicken. Außerdem ist der Code jetzt hervorgehoben.

    ![Auswählen der CSS-Stile](using-page-inspector-in-visual-studio-2012/_static/image39.png "Auswählen der CSS-Stile")

    *Auswählen der CSS-Stile*
4. Entfernen Sie das **Text-align-** Attribut im hervorgehobenen Code, indem Sie die öffnenden und schließenden Zeichen entfernen und die Datei " **Site. CSS** " speichern.

    Seitenprüfung kennt alle unterschiedlichen Dateien, die die aktuelle Seite bilden, und kann erkennen, wenn sich eine dieser Dateien ändert. Sie werden benachrichtigt, wenn die aktuelle Seite im Browser nicht mit den Quelldateien synchron ist.
5. Klicken Sie im Seitenprüfung Browser auf die Leiste unter der Adressleiste, um die Änderungen zu speichern und die Seite neu zu laden.

    ![Die Seite wird erneut geladen.](using-page-inspector-in-visual-studio-2012/_static/image40.png)

    *Die Seite wird erneut geladen.*

    Die Links befinden sich jetzt auf der rechten Seite, aber Sie sehen weiterhin wie eine Auflistungs Liste aus. Nun entfernen Sie die Aufzählungs Zeichen und richten die Verknüpfungen horizontal aus.

    ![Aktualisierte Seite](using-page-inspector-in-visual-studio-2012/_static/image41.png)

    *Aktualisierte Seite*
6. Wählen Sie im Überprüfungs Modus eine der **&lt;Li&gt;** Elemente aus, die die &quot;Register&quot; und &quot;anmelden&quot; Links enthalten. Klicken Sie dann auf den **&lt;Abschnitt&gt; #Login** Element, um auf **Stile. CSS** -Code zuzugreifen.

    ![Suchen des Stils](using-page-inspector-in-visual-studio-2012/_static/image42.png "Suchen des Stils")

    *Suchen des Stils*
7. Heben Sie in " **Style. CSS**" die Auskommentierung des Codes für **#Login Li** Items auf. Der Stil, den Sie hinzufügen, verbirgt das Aufzählungs Zeichen und zeigt die Elemente horizontal an.

    ![Umgestalten der Anmelde Verknüpfungen](using-page-inspector-in-visual-studio-2012/_static/image43.png "Umgestalten der Anmelde Verknüpfungen")

    *Umgestalten der Anmelde Verknüpfungen*
8. Speichern Sie die Datei **Style. CSS** , und klicken Sie auf die Leiste unterhalb der Adresse, um die Seite neu zu laden. Beachten Sie, dass die Links ordnungsgemäß angezeigt werden.

    ![Links, die an der rechten Seite ausgerichtet sind](using-page-inspector-in-visual-studio-2012/_static/image44.png "Links, die an der rechten Seite ausgerichtet sind")

    *Links, die an der rechten Seite ausgerichtet sind*
9. Zum Schluss ändern Sie den Titel des Headers. Anstatt den Text "**your Logo here"** in allen Dateien zu suchen, verwenden Sie den Überprüfungs Modus, um auf den Text zu klicken und zum Quellcode zu gelangen, der ihn generiert.

    ![Suchen des Website Titels](using-page-inspector-in-visual-studio-2012/_static/image45.png "Suchen des Website Titels")

    *Suchen des Website Titels*
10. Wenn Sie sich nun in " **Site. Master**" befinden, ersetzen Sie den Text "**your Logo here**" durch "**Photo Gallery**". Speichern und aktualisieren Sie den Seitenprüfung Browser.

    ![Fotogalerie Seite aktualisiert](using-page-inspector-in-visual-studio-2012/_static/image46.png "Fotogalerie Seite aktualisiert")

    *Fotogalerie Seite aktualisiert*
11. Drücken Sie zum Schluss die **Taste F5** , um die APP auszuführen, und prüfen Sie, ob alle Änderungen erwartungsgemäß funktionieren.

---

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a>Zusammenfassung

Wenn Sie diese praktische Übungseinheit durcharbeiten, haben Sie gelernt, wie Sie mit Seitenprüfung eine Vorschau Ihrer Webanwendung anzeigen, ohne die Website in einem Browser neu erstellen und ausführen zu müssen. Außerdem haben Sie gelernt, wie Sie Fehler schnell finden und beheben können, indem Sie direkt von der gerenderten Ausgabe auf den Quellcode zugreifen.

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>Anhang A: Installieren von Visual Studio Express 2012 für das Web

Sie können **Microsoft Visual Studio Express 2012 für Web** oder eine andere &quot;Express&quot; Version mit dem **[Microsoft-Webplattform-Installer](https://www.microsoft.com/web/downloads/platform.aspx)** installieren. Die folgenden Anweisungen führen Sie durch die Schritte, die zum Installieren *von Visual Studio Express 2012 für Web* mithilfe von *Microsoft-Webplattform-Installer*erforderlich sind.

1. Wechseln Sie zu [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169). Wenn Sie den Webplattform-Installer bereits installiert haben, können Sie ihn auch öffnen und nach dem Produkt &quot;<em>Visual Studio Express 2012 für das Web mit dem Windows Azure SDK</em>&quot;suchen.
2. Klicken Sie auf **jetzt installieren**. Wenn Sie nicht über einen **Webplattform-Installer** verfügen, werden Sie umgeleitet, um Sie zuerst herunterzuladen und zu installieren.
3. Nachdem der **Webplattform-Installer** geöffnet ist, klicken Sie auf **Installieren** , um das Setup zu starten.

    ![Installieren von Visual Studio Express](using-page-inspector-in-visual-studio-2012/_static/image47.png "Installieren von Visual Studio Express")

    *Installieren von Visual Studio Express*
4. Lesen Sie die Lizenzbedingungen für alle Produkte, und klicken Sie auf **ich** Stimme zu, um fortzufahren.

    ![Akzeptieren der Lizenzbedingungen](using-page-inspector-in-visual-studio-2012/_static/image48.png)

    *Akzeptieren der Lizenzbedingungen*
5. Warten Sie, bis der Download-und Installationsvorgang abgeschlossen ist.

    ![Installationsstatus](using-page-inspector-in-visual-studio-2012/_static/image49.png)

    *Installationsfortschritt*
6. Wenn die Installation abgeschlossen ist, klicken Sie auf **Fertig**stellen.

    ![Installation abgeschlossen](using-page-inspector-in-visual-studio-2012/_static/image50.png)

    *Installation abgeschlossen*
7. Klicken Sie zum Schließen des Webplattform-Installers auf **Beenden** .
8. Um Visual Studio Express für das Web zu öffnen, navigieren Sie zum **Start** Bildschirm, und beginnen Sie mit dem Schreiben &quot;**vs Express**&quot;und klicken Sie dann auf die Kachel **vs Express für Web** .

    ![VS Express für Web-Kachel](using-page-inspector-in-visual-studio-2012/_static/image51.png)

    *VS Express für Web-Kachel*
