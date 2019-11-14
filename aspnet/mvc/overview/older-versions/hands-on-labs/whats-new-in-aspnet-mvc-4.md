---
uid: mvc/overview/older-versions/hands-on-labs/whats-new-in-aspnet-mvc-4
title: Neues in ASP.NET MVC 4 | Microsoft-Dokumentation
author: rick-anderson
description: ASP.NET MVC 4 ist ein Framework zum Erstellen skalierbarer, auf Standards basierender Webanwendungen mit bewährten Entwurfsmustern und der Leistungsfähigkeit des ASP.net und...
ms.author: riande
ms.date: 02/18/2013
ms.assetid: 48f7feb3-872f-485d-b96f-e30011ff8c4a
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/whats-new-in-aspnet-mvc-4
msc.type: authoredcontent
ms.openlocfilehash: 4235f4fe666cdeb7d0821127a2b349f2ff30cd6e
ms.sourcegitcommit: 295cf898a4c87e264b0c35c7254b0fa4169f2278
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/13/2019
ms.locfileid: "74057030"
---
# <a name="whats-new-in-aspnet-mvc-4"></a>Neue Funktionen in ASP.NET MVC 4

Vom [Web Camps-Team](https://twitter.com/webcamps)

[Webcamps-Trainingskit herunterladen](https://aka.ms/webcamps-training-kit)

ASP.NET MVC 4 ist ein Framework zum Erstellen skalierbarer, auf Standards basierender Webanwendungen mit bewährten Entwurfsmustern und der Leistungsfähigkeit von ASP.net und .NET Framework. Diese neue, vierte Version des Frameworks konzentriert sich darauf, die Entwicklung mobiler Webanwendungen zu vereinfachen.

Wenn Sie zunächst ein neues ASP.NET MVC 4-Projekt erstellen, gibt es jetzt eine Projektvorlage für mobile Anwendungen, die Sie zum Erstellen einer eigenständigen App speziell für mobile Geräte verwenden können. Außerdem ist ASP.NET MVC 4 über ein jQuery. Mobile. MVC-nuget-Paket in jQuery Mobile integriert. jQuery Mobile ist ein HTML5-basiertes Framework für die Entwicklung von Web-Apps, die mit allen gängigen mobilen Geräte Plattformen, einschließlich Windows Phone, iPhone, Android usw., kompatibel sind. Wenn Sie jedoch eine Spezialisierung benötigen, können Sie mit ASP.NET MVC 4 verschiedene Ansichten für verschiedene Geräte bereitstellen und gerätespezifische Optimierungen bereitstellen.

In dieser praktischen Übungseinheit beginnen Sie mit der ASP.NET MVC 4 &quot;Internet Application&quot;-Projektvorlage, um eine Fotogalerie-Anwendung zu erstellen. Sie werden die APP progressiv mithilfe der neuen Features von jQuery Mobile und ASP.NET MVC 4 verbessern, um Sie mit verschiedenen mobilen Geräten und Desktop-Webbrowsern kompatibel zu machen. Außerdem erfahren Sie mehr über neue Code Rezepte für die Codegenerierung und darüber, wie ASP.NET MVC 4 Ihnen das Schreiben von asynchronen Aktionsmethoden erleichtert, indem Sie Task&lt;Action result&gt; Rückgabe Typen unterstützt.

> [!NOTE]
> Der gesamte Beispielcode und die Code Ausschnitte sind im webcamps-Trainingskit enthalten, das unter [Microsoft-Web/webcamptrainingkit-Releases](https://aka.ms/webcamps-training-kit)verfügbar ist. Das Projekt, das für dieses Lab spezifisch ist, finden Sie unter [What es New in Web Forms in ASP.NET 4,5](https://github.com/Microsoft-Web/HOL-ASPNETWebForms).

<a id="Objectives"></a>
### <a name="objectives"></a>Ziele

In dieser praktischen Übungseinheit erfahren Sie Folgendes:

- Profitieren Sie von den Verbesserungen der ASP.NET MVC-Projektvorlagen (einschließlich der neuen Projektvorlage für mobile Anwendungen).
- Verwenden des HTML5-viewportattributs und von CSS-Medien Abfragen zur Verbesserung der Anzeige auf mobilen Geräten
- Verwenden von jQuery Mobile für Progressive Erweiterungen und zum Entwickeln von Touchscreen-optimierter Webbenutzer Oberfläche
- Erstellen von mobilen Sichten
- Verwenden der View-Switcher-Komponente zum Umschalten zwischen mobilen und Desktop Ansichten in der Anwendung
- Erstellen von asynchronen Controllern mithilfe von Task Unterstützung

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Erforderliche Voraussetzungen

Zum Durchführen dieses Labs müssen Sie über Folgendes verfügen:

- [Microsoft Visual Studio Express 2012 für Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) oder Superior (Informationen zur Installation finden Sie in [Anhang B](#AppendixB) ).
- [ASP.NET MVC 4](../../../mvc4.md) (in der Installation von Microsoft Visual Studio 2012 enthalten)
- Windows Phone Emulator (im [Windows Phone 7.1.1 SDK](https://www.microsoft.com/download/details.aspx?id=29233)enthalten)
- Optional- [webmatrix 2](https://www.microsoft.com/web/webmatrix/) mit der iPhone-Simulator-Erweiterung " **Electric Plum** " (nur für Übung 3 zum Durchsuchen der Webanwendung mit einem iPhone-Simulator)

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a>Setup

Im gesamten Lab-Dokument werden Sie angewiesen, Code Blöcke einzufügen. Der größte Teil dieses Codes wird zur einfacheren Verwendung als Visual Studio Code Ausschnitte bereitgestellt, die Sie in Visual Studio verwenden können, um das manuelle Hinzufügen zu vermeiden.

So installieren Sie die Code Ausschnitte:

1. Öffnen Sie ein Windows-Explorer-Fenster, und navigieren Sie zum Ordner " **source\setup** " des Labs.
2. Doppelklicken Sie auf die Datei **Setup. cmd** in diesem Ordner, um die Visual Studio-Code Ausschnitte zu installieren.

Wenn Sie mit den Visual Studio Code Ausschnitten nicht vertraut sind und wissen möchten, wie Sie Sie verwenden können, finden Sie den Anhang dieses Dokuments &quot;[Anhang A: Verwenden von Code Ausschnitten](#AppendixA)&quot;.

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a>Exerzitien

Diese praktische Übungseinheit umfasst die folgenden Übungen:

1. [Neue ASP.NET MVC 4-Projektvorlagen](#Exercise1)
2. [Erstellen der Fotogalerie-Webanwendung](#Exercise2)
3. [Hinzufügen von Unterstützung für mobile Geräte](#Exercise3)
4. [Verwenden von asynchronen Controllern](#Exercise4)

> [!NOTE]
> Jede Übung wird von einem **endordner** begleitet, der die sich ergebende Lösung enthält, die Sie nach Abschluss der Übungen erhalten. Sie können diese Lösung als Leitfaden verwenden, wenn Sie zusätzliche Hilfe beim Durcharbeiten der Übungen benötigen.

Geschätzte Zeit bis zum Abschluss dieses Labs: **60 Minuten**.

<a id="Exercise1"></a>

<a id="Exercise_1_New_ASPNET_MVC_4_Project_Templates"></a>
### <a name="exercise-1-new-aspnet-mvc-4-project-templates"></a>Übung 1: neue ASP.NET MVC 4-Projektvorlagen

In dieser Übung werden die Verbesserungen in den ASP.NET MVC 4-Projektvorlagen erläutert. Zusätzlich zur Internet Anwendungs Vorlage, die bereits in MVC 3 vorhanden ist, enthält diese Version jetzt eine separate Vorlage für mobile Anwendungen. Zuerst sehen Sie sich einige relevante Features der einzelnen Vorlagen an. Anschließend arbeiten Sie mit dem richtigen Ansatz an der ordnungsgemäßen Darstellung der Seite auf den verschiedenen Plattformen.

<a id="Task_1_-_Exploring_the_Internet_Application_Template"></a>
#### <a name="task-1---exploring-the-internet-application-template"></a>Aufgabe 1: Untersuchen der Internet Anwendungs Vorlage

1. Öffnen Sie **Visual Studio**.
2. Wählen Sie die Datei aus. **Neu |** Menübefehl "Projekt". Wählen Sie im Dialogfeld **Neues Projekt** die **Option C# Visual | Webvorlage** im linken Fensterbereich, und wählen Sie **ASP.NET MVC 4-Webanwendung aus.** Nennen Sie das Projekt " **Photogallery**", wählen Sie einen Speicherort (oder über **schreiben Sie den**Standardwert), und klicken Sie

    > [!NOTE]
    > Später passen Sie die Projekt Mappe "Photogallery ASP.NET MVC 4" an, die Sie gerade erstellen.

    ![Erstellen eines neuen Projekts](whats-new-in-aspnet-mvc-4/_static/image1.png "Erstellen eines neuen Projekts")

    *Erstellen eines neuen Projekts*
3. Wählen Sie im Dialogfeld **Neues ASP.NET MVC 4-Projekt** die Projektvorlage **Internet Anwendung** aus, und klicken Sie auf **OK**. Stellen Sie sicher, dass Sie Razor als Ansichts-Engine ausgewählt haben.

    ![Erstellen einer neuen ASP.NET MVC 4-Internet Anwendung](whats-new-in-aspnet-mvc-4/_static/image2.png "Erstellen einer neuen ASP.NET MVC 4-Internet Anwendung")

    *Erstellen einer neuen ASP.NET MVC 4-Internet Anwendung*

    > [!NOTE]
    > Razor-Syntax wurde in ASP.NET MVC 3 eingeführt. Das Ziel besteht darin, die Anzahl der in einer Datei benötigten Zeichen und Tastatureingaben zu minimieren und so einen schnellen und flüssigen Codierungs Workflow zu ermöglichen. Razor nutzt vorhandene C# /VB-Sprachkenntnisse (oder andere) und bietet eine Vorlagen Markup Syntax, die einen tollen HTML-Konstruktions Workflow ermöglicht.
4. Drücken Sie **F5** , um die Projekt Mappe auszuführen und die erneuerten Vorlagen anzuzeigen. Sehen Sie sich die folgenden Features an:

    **Moderne Vorlagen**

    Die Vorlagen wurden erneuert und bieten modernere Stile.

    ![ASP.NET MVC 4-Vorlagen mit Neuformatierung](whats-new-in-aspnet-mvc-4/_static/image3.png "Neu formatierte MVC 4-Vorlagen")

    *ASP.NET MVC 4-Vorlagen mit Neuformatierung*

    ![Neue Kontaktseite](whats-new-in-aspnet-mvc-4/_static/image4.png "Neue Kontaktseite")

    *Neue Kontaktseite*

    **Adaptive Rendering**

    Sehen Sie sich die Größe des Browserfensters an, und beachten Sie, wie das Seitenlayout dynamisch an die neue Fenstergröße angepasst wird. Diese Vorlagen verwenden die Adaptive renderingtechnik, um auf Desktop-und mobilen Plattformen ohne Anpassung ordnungsgemäß zu rendern.

    ![ASP.NET MVC 4-Projektvorlage in unterschiedlichen Browser Größen](whats-new-in-aspnet-mvc-4/_static/image5.png "ASP.NET MVC 4-Projektvorlage in unterschiedlichen Browser Größen")

    *ASP.NET MVC 4-Projektvorlage in unterschiedlichen Browser Größen*

    **Umfangreichere Benutzeroberfläche mit JavaScript**

    Eine weitere Erweiterung der Standard Projektvorlagen ist die Verwendung von JavaScript, um interaktives JavaScript bereitzustellen. Die in der Vorlage verwendeten Anmelde-und Registrierungs Links veranschaulichen, wie die jQuery-Überprüfungen verwendet werden, um die Eingabefelder von Clientseite zu validieren.

    ![jQuery-Validierung](whats-new-in-aspnet-mvc-4/_static/image6.png)

    *jQuery-Validierung*

    > [!NOTE]
    > Beachten Sie die beiden Anmelde Abschnitte. im ersten Abschnitt können Sie sich mit einem registrierten Konto des Standorts anmelden, und im zweiten Abschnitt können Sie sich alternativ auch mit einem anderen Authentifizierungsdienst wie Google anmelden (standardmäßig deaktiviert).
5. Schließen Sie den Browser, um den Debugger zu beenden und zu Visual Studio zurückzukehren.
6. Öffnen Sie die Datei **AuthConfig.cs** , die sich unter dem Ordner **App\_Start** befindet.
7. Entfernen Sie den Kommentar aus der letzten Zeile, um Google Client für die *OAuth* -Authentifizierung zu registrieren.

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample1.cs)]

    > [!NOTE]
    > Beachten Sie, dass Sie die Authentifizierung problemlos mit beliebigen OpenID-oder OAuth-Diensten wie Facebook, Twitter, Microsoft usw. aktivieren können.
8. Drücken Sie **F5** , um die Projekt Mappe auszuführen, und navigieren Sie zur Anmeldeseite.
9. Wählen Sie **Google** -Dienst aus, um sich anzumelden.

    ![Auswählen des Dienst Protokolls](whats-new-in-aspnet-mvc-4/_static/image7.png)

    *Auswählen des Dienst Protokolls*
10. Melden Sie sich mit Ihrem Google-Konto an.
11. Hiermit wird die Website (localhost) zum Abrufen von Informationen aus dem Google-Konto zugelassen.
12. Schließlich müssen Sie sich auf der Website registrieren, um das Google-Konto zuzuordnen.

   ![Ihr Google-Konto zuordnen](whats-new-in-aspnet-mvc-4/_static/image8.png)

   *Zuordnen Ihres Google-Kontos*
13. Schließen Sie den Browser, um den Debugger zu beenden und zu Visual Studio zurückzukehren.
14. Sehen Sie sich nun die Lösung an, um einige andere neue Features zu testen, die von ASP.NET MVC 4 in der Projektvorlage eingeführt wurden.

   ![Die Projektvorlage "ASP.NET MVC 4 Internet Application"](whats-new-in-aspnet-mvc-4/_static/image9.png "Die Projektvorlage "ASP.NET MVC 4 Internet Application"")

   *Die Projektvorlage "ASP.NET MVC 4 Internet Application"*

    - **HTML 5-Markup**

       Durchsuchen Sie Vorlagen Sichten, um das neue Design Markup zu ermitteln.

       ![Neue Vorlage mit Razor-und HTML5-Markup über. cshtml.](whats-new-in-aspnet-mvc-4/_static/image10.png "Neue Vorlage mit Razor-und HTML5-Markup über. cshtml.")

       *Neue Vorlage mit Razor-und HTML5-Markup (about. cshtml).*
    - **Aktualisierte JavaScript-Bibliotheken**

       Die ASP.NET MVC 4-Standardvorlage enthält jetzt "knockoutjs", ein JavaScript-MVVM-Framework, mit dem Sie umfangreiche und sehr reaktionsschnelle Webanwendungen mit JavaScript und HTML erstellen können. Wie in MVC3 sind auch jQuery-und jQuery-UI-Bibliotheken in ASP.NET MVC 4 enthalten.

     > [!NOTE]
     > Weitere Informationen zur Datei "knockoutjs" finden Sie unter diesem Link: [[http://learn.knockoutjs.com/](http://learn.knockoutjs.com/)](http://learn.knockoutjs.com/). Darüber hinaus erfahren Sie mehr über die jQuery-und jQuery-Benutzeroberfläche in [[http://docs.jquery.com/](http://docs.jquery.com/)](http://docs.jquery.com/).

<a id="Task_2_-_Exploring_the_Mobile_Application_Template"></a>
#### <a name="task-2---exploring-the-mobile-application-template"></a>Aufgabe 2: Erkunden der Vorlage für mobile Anwendungen

ASP.NET MVC 4 vereinfacht die Entwicklung von Websites für Mobile und Tablet-Browser. Diese Vorlage verfügt über die gleiche Anwendungs Struktur wie die Internet Anwendungs Vorlage (Beachten Sie, dass der Controller Code praktisch identisch ist), aber der Stil wurde so geändert, dass er auf Touchscreen-basierten mobilen Geräten ordnungsgemäß funktioniert.

1. Wählen Sie die Datei aus. **Neu |** Menübefehl "Projekt". Wählen Sie im Dialogfeld **Neues Projekt** die **Option C# Visual | Webvorlage** im linken Fensterbereich, und wählen Sie die **ASP.NET MVC 4-Webanwendung aus.** Nennen Sie das Projekt " **Photogallery. Mobile**", wählen Sie einen Speicherort aus (oder überlassen Sie die Standardeinstellung), wählen Sie &quot;&quot; hinzufügen aus, **und klicken Sie**
2. Wählen Sie im Dialogfeld **Neues ASP.NET MVC 4-Projekt** die Projektvorlage für **Mobile Anwendungen** aus, und klicken Sie auf **OK**. Stellen Sie sicher, dass Sie Razor als Ansichts-Engine ausgewählt haben.

    ![Erstellen einer neuen mobilen ASP.NET MVC 4-Anwendung](whats-new-in-aspnet-mvc-4/_static/image11.png "Erstellen einer neuen mobilen ASP.NET MVC 4-Anwendung")

    *Erstellen einer neuen mobilen ASP.NET MVC 4-Anwendung*
3. Jetzt können Sie die Lösung erkunden und einige der neuen Features kennenlernen, die von der Lösungs Vorlage ASP.NET MVC 4 für Mobile eingeführt wurden:

    - **Mobile jQuery-Bibliothek**

        Die Projektvorlage für mobile Anwendungen enthält die Mobile jQuery-Bibliothek, bei der es sich um eine Open-Source-Bibliothek für die Kompatibilität mobiler Browser handelt. jQuery Mobile wendet Progressive Verbesserungen auf Mobile Browser an, die CSS und JavaScript unterstützen. Die Progressive Erweiterung ermöglicht allen Browsern, den grundlegenden Inhalt einer Webseite anzuzeigen, während Sie nur die leistungsfähigsten Browser zum Anzeigen der umfangreichen Inhalte ermöglicht. Die JavaScript-und CSS-Dateien, die im jQuery Mobile-Stil enthalten sind, unterstützen Mobile Browser bei der Anpassung an den Inhalt des Bildschirms, ohne dass Änderungen am Seiten Markup vorgenommen werden.

        ![jQuery-mobile-Library-enthaltene-in-the-Vorlage](whats-new-in-aspnet-mvc-4/_static/image12.png)

        *in der Vorlage enthaltene Mobile jQuery-Bibliothek*
    - **HTML5-basiertes Markup**

        ![Mobile-Application-Template-using-HTML5-Markup](whats-new-in-aspnet-mvc-4/_static/image13.png)

        *Vorlage für mobile Anwendungen mit HTML5-Markup (Login. cshtml und Index. cshtml)*
4. Drücken Sie **F5** , um die Projekt Mappe auszuführen.
5. Öffnen Sie den **Windows Phone 7-Emulator**.
6. Öffnen Sie Internet Explorer auf dem Bildschirm "Telefon Start". Sehen Sie sich die URL an, unter der die Desktop Anwendung gestartet wurde, und navigieren Sie zu dieser URL vom Telefon (z. b. `http://localhost:[PortNumber]/`).
7. Nun können Sie die Anmeldeseite eingeben oder die Seite "Info" ansehen. Beachten Sie, dass der Stil der Website auf der neuen Metro-App für mobile basiert. Die ASP.NET MVC 4-Projektvorlage wird auf mobilen Geräten ordnungsgemäß angezeigt und stellt sicher, dass alle Elemente der Seite sichtbar und aktiviert sind. Beachten Sie, dass die Links für den Header groß genug sind, um darauf geklickt oder getippt zu werden.

    ![Projektvorlagen Seiten auf einem mobilen Gerät](whats-new-in-aspnet-mvc-4/_static/image14.png "Projektvorlagen Seiten auf einem mobilen Gerät")

    *Projektvorlagen Seiten auf einem mobilen Gerät*
8. Die neue Vorlage verwendet auch das **Viewport-Meta-Tag**. In den meisten mobilen Browsern wird eine Breite für ein virtuelles Browserfenster oder &quot;Viewport&quot;definiert, die größer als die tatsächliche Breite des mobilen Geräts ist. Auf diese Weise können Mobile Browser die gesamte Webseite innerhalb der virtuellen Anzeige anzeigen. Das **Viewport-Meta-Tag** ermöglicht Webentwicklern das Festlegen der Breite, Höhe und Skalierung des Browser Bereichs auf mobilen Geräten **.** Mit der ASP.NET MVC 4-Vorlage für mobile Anwendungen wird der Viewport auf die Gerätebreite (&quot;Width = Device-Width&quot;) in der Layoutvorlage (*views\shared\_Layout. cshtml*) festgelegt, sodass der Viewport für alle Seiten auf die Bildschirmbreite des Geräts festgelegt ist. Beachten Sie, dass das Viewport-Meta-Tag die Standardbrowser Ansicht nicht ändert.
9. Öffnen Sie **\_"Layout. cshtml**", das sich in den **Ansichten befindet | Frei gegebener Ordner,** und kommentieren Sie das Viewport-Meta-Tag aus. Führen Sie die Anwendung aus, wenn Sie nicht bereits geöffnet ist, und überprüfen Sie die Unterschiede.

[!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample2.cshtml)]

![Die Website nach dem kommentieren des viewportmeta-Tags](whats-new-in-aspnet-mvc-4/_static/image15.png "Die Website nach dem kommentieren des viewportmeta-Tags")

*Die Website nach dem kommentieren des viewportmeta-Tags*
10. Drücken Sie in Visual Studio **UMSCHALT** + **F5** , um das Debuggen der Anwendung zu verhindern.
11. Auskommentierung des viewportmeta-Tags aufheben.

[!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample3.cshtml)]

<a id="Task_3_-_Using_Adaptive_Rendering"></a>
#### <a name="task-3---using-adaptive-rendering"></a>Aufgabe 3: Verwenden des adaptiven Rendering

In dieser Aufgabe lernen Sie eine weitere Methode kennen, um eine Webseite auf mobilen Geräten und Webbrowser gleichzeitig korrekt zu gestalten, ohne Anpassungen vorzunehmen. Sie haben das Viewport-Meta-Tag bereits mit einem ähnlichen Zweck verwendet. Nun erreichen Sie eine weitere leistungsstarke Methode: *adaptive Rendering*.

Adaptive Rendering ist eine Technik, bei der **CSS3-Medien Abfragen** verwendet werden, um den auf eine Seite angewendeten Stil anzupassen. Medien Abfragen definieren Bedingungen innerhalb eines Stylesheets und Gruppieren CSS-Stile unter einer bestimmten Bedingung. Nur wenn die Bedingung true ist, wird der Stil auf die deklarierten Objekte angewendet.

Die Flexibilität, die von der adaptiven renderingtechnik geboten wird, ermöglicht jede Anpassung der Site auf unterschiedlichen Geräten. Sie können beliebig viele Stile in einem einzelnen Stylesheet definieren, ohne Logik Code zum Auswählen des Stils zu schreiben. Daher ist es eine sehr saubere Methode, Seiten Stile anzupassen, da dadurch der doppelte Code und die Logik für renderingzwecke reduziert werden. Auf der anderen Seite würde sich die Bandbreitennutzung erhöhen, da die Größe Ihrer CSS-Dateien geringfügig zunehmen könnte.

Mithilfe des adaptiven renderingverfahrens wird Ihre Website **unabhängig vom Browser ordnungsgemäß angezeigt.** Sie sollten jedoch berücksichtigen, ob die zusätzliche Bandbreitenauslastung von Bedeutung ist.

> [!NOTE]
> Das Grundformat einer Medien Abfrage ist: @media \[Bereich: alle | Handheld | Drucken | Projektion | Bildschirm\] ([Eigenschaft: Wert] und... [Eigenschaft: Wert])

Beispiele für Medien Abfragen: &gt; **@media all und (max-width: 1000 px) und (min-width: 700px) {}:** für alle Auflösungen zwischen 700 PX und 1000 px.

> **@media Bildschirm und (min-width: 400px) und (max-width: 700 PX) {...}:** Nur für Bildschirme. Die Auflösung muss zwischen 400 und 700 PX liegen.
> 
> **@media Handheld und (minimale Breite: 20em), Bildschirm und (minimale Breite: 20em) {...}:** Für Handhelds (Mobilgeräte und Geräte) und Bildschirme. Die minimale Breite muss größer als 20em sein.
> 
> Weitere Informationen hierzu finden Sie auf der W3C- [Website](http://www.w3.org/TR/css3-mediaqueries/).

Nun erfahren Sie, wie das adaptive Rendering funktioniert, um die Lesbarkeit der ASP.NET MVC 4-Standard Website Vorlage zu verbessern.

1. Öffnen Sie die Projekt Mappe **Photogallery. sln** , die Sie in Aufgabe 1 erstellt haben, und wählen Sie das Projekt **Photogallery** aus. Drücken Sie **F5** , um die Projekt Mappe auszuführen.
2. Ändern Sie die Größe des Browsers, und legen Sie die Fenster auf die Hälfte oder auf weniger als ein Quartal der ursprünglichen Größe fest. Beachten Sie, was mit den Elementen in der Kopfzeile geschieht: einige Elemente werden im sichtbaren Bereich des Headers nicht angezeigt.
3. Öffnen Sie die Datei " **Site. CSS** " im Projektmappen-Explorer von Visual Studio im **Inhalts** Projektordner. Drücken Sie **STRG + F** , um die integrierte Visual Studio-Suche zu öffnen, und `@media`, um die **CSS-Medien Abfrage**zu suchen.

    Die in dieser Vorlage definierte Medien Abfrage Bedingung funktioniert auf diese Weise: Wenn die Fenstergröße des Browsers unter **850 px**liegt, werden die CSS-Regeln, die in diesem medienblock definiert sind, angewendet.

    ![Suchen der Medien Abfrage](whats-new-in-aspnet-mvc-4/_static/image16.png "Suchen der Medien Abfrage")

    *Suchen der Medien Abfrage*
4. Ersetzen Sie den Attribut Wert "max-width", der in 850 px festgelegt ist, durch **10px**, um das adaptive Rendering zu deaktivieren, und drücken Sie **STRG + S** , um die Änderungen zu speichern. Kehren Sie zum Browser zurück, und drücken Sie **STRG + F5** , um die Seite mit den vorgenommenen Änderungen zu aktualisieren. Beachten Sie die Unterschiede auf beiden Seiten, wenn Sie die Breite des Fensters anpassen.

    ![Auf der linken Seite wendet die Seite den @media Stil an. auf der rechten Seite wird der Stil weggelassen.](whats-new-in-aspnet-mvc-4/_static/image17.png "Auf der linken Seite wendet die Seite den @Media Stil an. auf der rechten Seite wird der Stil weggelassen.")

    *Auf der linken Seite wendet die Seite den @media Stil an. auf der rechten Seite wird der Stil weggelassen.*

    Sehen wir uns nun an, was auf mobilen Geräten geschieht:

    ![Auf der linken Seite wendet die Seite den @media Stil an. auf der rechten Seite wird der Stil weggelassen.](whats-new-in-aspnet-mvc-4/_static/image18.png "Auf der linken Seite wendet die Seite den @Media Stil an. auf der rechten Seite wird der Stil weggelassen.")

    *Auf der linken Seite wendet die Seite den @media Stil an. auf der rechten Seite wird der Stil weggelassen.*

    Obwohl Sie feststellen, dass die Änderungen, wenn die Seite in einem Webbrowser gerendert wird, nicht sehr wichtig sind, werden die Unterschiede bei der Verwendung eines mobilen Geräts deutlicher. Auf der linken Seite des Bilds können wir sehen, dass der benutzerdefinierte Stil die Lesbarkeit verbessert hat.

    Adaptive Rendering kann in vielen weiteren Szenarien verwendet werden, sodass es einfacher ist, bedingte Formatierung auf eine Website anzuwenden und allgemeine Stil Probleme mit einem einfachen Ansatz zu lösen.

    Die Viewport-Meta-Tags und CSS-Medien Abfragen sind nicht spezifisch für ASP.NET MVC 4, sodass Sie diese Features in jeder Webanwendung nutzen können.
5. Drücken Sie in Visual Studio **UMSCHALT** + **F5** , um das Debuggen der Anwendung zu verhindern.

<a id="Exercise2"></a>

<a id="Exercise_2_Creating_the_Photo_Gallery_Web_Application"></a>
### <a name="exercise-2-creating-the-photo-gallery-web-application"></a>Übung 2: Erstellen der Fotogalerie-Webanwendung

In dieser Übung arbeiten Sie an einer Fotogalerie-Anwendung, um Fotos anzuzeigen. Sie beginnen mit der ASP.NET MVC 4-Projektvorlage und fügen dann eine Funktion hinzu, mit der Fotos von einem Dienst abgerufen und auf der Startseite angezeigt werden.

In der folgenden Übung werden Sie diese Lösung aktualisieren, um die Anzeige auf mobilen Geräten zu verbessern.

<a id="Task_1_-_Creating_a_Mock_Photo_Service"></a>
#### <a name="task-1---creating-a-mock-photo-service"></a>Aufgabe 1: Erstellen eines Mock-Foto Dienstanbieter

In dieser Aufgabe erstellen Sie ein Mock des Photo Service, um die Inhalte abzurufen, die im Katalog angezeigt werden. Zu diesem Zweck fügen Sie einen neuen Controller hinzu, der einfach eine JSON-Datei mit den Daten der einzelnen Fotos zurückgibt.

1. Öffnen Sie **Visual Studio** , wenn es nicht bereits geöffnet ist.
2. Wählen Sie die Datei aus. **Neu |** Menübefehl "Projekt". Wählen Sie im Dialogfeld **Neues Projekt** die **Option C# Visual | Webvorlage** im linken Fensterbereich, und wählen Sie **ASP.NET MVC 4-Webanwendung aus.** Nennen Sie das Projekt " **Photogallery**", wählen Sie einen Speicherort (oder über **schreiben Sie den**Standardwert), und klicken Sie Alternativ dazu können Sie auch weiterhin von der vorhandenen ASP.NET MVC 4- **Internet Anwendungs** Lösung aus **Übung 1** aus arbeiten und den nächsten Schritt überspringen.
3. Wählen Sie im Dialogfeld **Neues ASP.NET MVC 4-Projekt** die Projektvorlage **Internet Anwendung** aus, und klicken Sie auf **OK**. Stellen Sie sicher, dass Razor als Ansichts-Engine ausgewählt ist.
4. Klicken Sie im **Projektmappen-Explorer**mit der rechten Maustaste auf den Ordner **App\_Data** Ihres Projekts, und wählen Sie **hinzufügen aus. Vorhandenes Element**. Navigieren Sie in diesem Lab zum Ordner **source\asset\app\_Data** , und fügen Sie die Datei **Photos. JSON** hinzu.
5. Erstellen Sie einen neuen Controller mit dem Namen **photocontroller**. Klicken Sie hierzu mit der rechten Maustaste auf den Ordner " **Controllers** ", und wählen Sie "Controller **Hinzufügen** " aus **.** Vervollständigen Sie den Controller Namen, belassen Sie die **leere MVC-Controller** Vorlage, und klicken Sie auf **Hinzufügen**.

    ![Hinzufügen von "photocontroller"](whats-new-in-aspnet-mvc-4/_static/image19.png "Hinzufügen von "photocontroller"")

    *Hinzufügen von "photocontroller"*
6. Ersetzen Sie die **Index** -Methode durch **die folgende Katalog** Aktion, und geben Sie den Inhalt aus der JSON-Datei zurück, die Sie vor kurzem dem Projekt hinzugefügt haben.

    (Code Ausschnitt- *ASP.NET MVC 4 Lab-Ex02-Gallery Action*)

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample4.cs)]
7. Drücken Sie **F5** , um die Projekt Mappe auszuführen, und navigieren Sie zur folgenden URL, um den mockphoto-Dienst zu testen: `http://localhost:[port]/photo/gallery` (der Wert [Port] hängt vom aktuellen Port ab, auf dem die Anwendung gestartet wurde). Durch die Anforderung an diese URL sollte der Inhalt der Datei " **Photos. JSON** " abgerufen werden.

    ![Testen des mockfotodienstanbieter](whats-new-in-aspnet-mvc-4/_static/image20.png "Testen des mockfotodienstanbieter")

    *Testen des mockfotodienstanbieter*

In einer realen Implementierung können Sie [ASP.net-Web-API](../../../../web-api/index.md) verwenden, um den Photo Gallery-Dienst zu implementieren. ASP.net-Web-API ist ein Framework, das das Erstellen von http-Diensten erleichtert, die eine breite Palette von Clients erreichen, einschließlich Browsern und mobilen Geräten. Die ASP.NET-Web-API ist eine ideale Plattform zum Erstellen von RESTful-Anwendungen in .NET Framework.

<a id="Task_2_-_Displaying_the_Photo_Gallery"></a>
#### <a name="task-2---displaying-the-photo-gallery"></a>Aufgabe 2: Anzeigen der Fotogalerie

In dieser Aufgabe aktualisieren Sie die Startseite, sodass Sie die Fotogalerie mit dem simulierte Dienst anzeigt, den Sie in der ersten Aufgabe dieser Übung erstellt haben. Sie fügen Modelldateien hinzu und aktualisieren die Katalog Sichten.

1. Drücken Sie in Visual Studio **UMSCHALT** + **F5** , um das Debuggen der Anwendung zu verhindern.
2. Erstellen Sie im Ordner **Models** die **Photo** -Klasse. Klicken Sie dazu mit der rechten Maustaste auf den Ordner **Modelle** , wählen Sie **Hinzufügen** aus, und klicken Sie auf **Klasse**. Legen Sie dann den Namen auf **Photo.cs** fest, und klicken Sie auf **Hinzufügen**.
3. Fügen Sie der **Photo** -Klasse die folgenden Member hinzu.

    (Code Ausschnitt- *ASP.NET MVC 4 Lab-Ex02-Photo Model*)

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample5.cs)]
4. Öffnen Sie im Ordner **Controller** die Datei **HomeController.cs**.
5. Fügen Sie die folgenden using-Anweisungen hinzu.

    (Code Ausschnitt- *ASP.NET MVC 4 Lab-Ex02-HomeController*using-Direktiven)

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample6.cs)]
6. Aktualisieren Sie die **Index** -Aktion so, dass **HttpClient** zum Abrufen der Katalogdaten verwendet wird, und verwenden Sie dann den **JavaScriptSerializer** , um ihn in das Ansichts Modell zu deserialisieren.

    (Code Ausschnitt- *ASP.NET MVC 4 Lab-Ex02-Index Action*)

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample7.cs)]
7. Öffnen Sie die Datei " **Index. cshtml** ", die sich im Ordner " **views\home** " befindet, und ersetzen Sie den gesamten Inhalt durch den folgenden Code.

    Dieser Code durchläuft alle vom Dienst abgerufenen Fotos und zeigt Sie in einer ungeordneten Liste an.

    (Code Ausschnitt- *ASP.NET MVC 4 Lab-Ex02-Photo List*)

    [!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample8.cshtml)]
8. Klicken Sie im **Projektmappen-Explorer**mit der rechten Maustaste auf den **Inhalts** Ordner Ihres Projekts, und wählen Sie **hinzufügen aus. Vorhandenes Element**. Navigieren Sie zum Ordner " **source\assets\content** " dieses Labs, und fügen Sie die Datei " **Site. CSS** " hinzu. Sie müssen die Ersetzung bestätigen. Wenn Sie die Datei " **Site. CSS** " geöffnet haben, müssen Sie sicherstellen, dass Sie auch die Datei erneut laden.
9. Öffnen Sie den Datei-Explorer, und kopieren Sie den gesamten Ordner **Fotos** , der sich unter dem Ordner **source\assets** dieses Labs befindet, in den Stamm Ordner des Projekts in Projektmappen-Explorer.
10. Führen Sie die Anwendung aus. Nun sollte die Startseite angezeigt werden, auf der die Fotos im Katalog angezeigt werden.

    ![Fotogalerie](whats-new-in-aspnet-mvc-4/_static/image21.png "Fotogalerie")

    *Fotogalerie*
11. Drücken Sie in Visual Studio **UMSCHALT** + **F5** , um das Debuggen der Anwendung zu verhindern.

<a id="Exercise3"></a>

<a id="Exercise_3_Adding_support_for_mobile_devices"></a>
### <a name="exercise-3-adding-support-for-mobile-devices"></a>Übung 3: Hinzufügen von Unterstützung für mobile Geräte

Eines der wichtigsten Updates in ASP.NET MVC 4 ist die Unterstützung für die Mobile-Entwicklung. In dieser Übung lernen Sie ASP.NET MVC 4 neue Features für mobile Anwendungen kennen, indem Sie die in der vorherigen Übung erstellte Photogallery-Lösung erweitern.

<a id="Task_1_-_Installing_jQuery_Mobile_in_an_ASPNET_MVC_4_Application"></a>
#### <a name="task-1---installing-jquery-mobile-in-an-aspnet-mvc-4-application"></a>Aufgabe 1: Installieren von jQuery Mobile in einer ASP.NET MVC 4-Anwendung

1. Öffnen Sie die Projekt Mappe " **Anfang** " unter " **Source/EX3-mobilesupport/BEGIN/** Folder". Andernfalls können Sie die **End** -Projekt Mappe, die Sie durch Abschließen der vorherigen Übung erhalten haben, weiterhin verwenden.

   1. Wenn Sie die bereitgestellte **Begin** -Lösung geöffnet haben, müssen Sie einige fehlende nuget-Pakete herunterladen, bevor Sie den Vorgang fortsetzen. Klicken Sie hierzu auf das Menü **Projekt** , und wählen Sie **nuget-Pakete verwalten**aus.
   2. Klicken Sie im Dialogfeld **nuget-Pakete verwalten** auf **Wiederherstellen** , um fehlende Pakete herunterzuladen.
   3. Erstellen Sie abschließend die Projekt Mappe, indem Sie auf **Build** ** | Projekt Mappe erstellen klicken**.

      > [!NOTE]
      > Einer der Vorteile der Verwendung von nuget besteht darin, dass Sie nicht alle Bibliotheken in Ihrem Projekt liefern müssen, um die Projektgröße zu verringern. Mit nuget Power Tools können Sie durch Angabe der Paketversionen in der Datei "Packages. config" alle erforderlichen Bibliotheken herunterladen, wenn Sie das Projekt zum ersten Mal ausführen. Aus diesem Grund müssen Sie diese Schritte ausführen, nachdem Sie eine vorhandene Projekt Mappe in diesem Lab geöffnet haben.
2. Öffnen Sie die **Paket-Manager-Konsole** , indem Sie auf die Menüoption **Tools** > **nuget-Paket-Manager** > Paket- **Manager** klicken.

    ![Öffnen der nuget-Paket-Manager-Konsole](whats-new-in-aspnet-mvc-4/_static/image22.png "Öffnen der nuget-Paket-Manager-Konsole")

    *Öffnen der nuget-Paket-Manager-Konsole*
3. Führen Sie in der Paket-Manager-Konsole den folgenden Befehl aus, um das Paket " **jQuery. Mobile. MVC** " zu installieren.

    jQuery Mobile ist eine Open-Source-Bibliothek zum Entwickeln einer Touchscreen-optimierten Webbenutzer Oberfläche. Das nuget-Paket "jQuery. Mobile. MVC" umfasst Hilfsprogramme für die Verwendung von jQuery Mobile mit einer ASP.NET MVC 4-Anwendung.

    > [!NOTE]
    > Wenn Sie den folgenden Befehl ausführen, wird die jQuery. Mobile. MVC-Bibliothek von nuget heruntergeladen.

    PM

    [!code-powershell[Main](whats-new-in-aspnet-mvc-4/samples/sample9.ps1)]

    Mit diesem Befehl werden jQuery Mobile und einige Hilfsdateien installiert, einschließlich der folgenden:

    - **Views/Shared/\_Layout. Mobile. cshtml**: ist ein auf einem mobilen jQuery-basierendes Layout, das für einen kleineren Bildschirm optimiert ist. Wenn die Website eine Anforderung von einem mobilen Browser empfängt, ersetzt Sie das ursprüngliche Layout (\_Layout. cshtml) durch dieses.
    - Eine Ansichts switcherkomponente: besteht aus der Teilansicht **views/Shared/\_viewswitcher. cshtml** und dem **ViewSwitcherController.cs** -Controller. Diese Komponente zeigt einen Link auf mobilen Browsern an, damit Benutzer zur Desktop Version der Seite wechseln können.  
        ![Fotogalerie Projekt mit mobiler Unterstützung](whats-new-in-aspnet-mvc-4/_static/image23.png "PhProjekt mit Unterstützung mobiler Geräte im Projekt "Oto")

        *Fotogalerie Projekt mit mobiler Unterstützung*
4. Registrieren Sie die mobilen Pakete. Öffnen Sie hierzu die Datei **Global.asax.cs** , und fügen Sie die folgende Zeile hinzu.

    (Code Ausschnitt- *ASP.NET MVC 4 Lab-Ex03-Register Mobile Bundles*)

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample10.cs)]
5. Führen Sie die Anwendung mithilfe eines Desktop-Webbrowsers aus.
6. Öffnen Sie den **Windows Phone 7-Emulator,** der sich im **Startmenü befindet | Alle Programme | Windows Phone SDK 7,1 | Windows Phone Emulator.**
7. Öffnen Sie Internet Explorer auf dem Bildschirm "Telefon Start". Sehen Sie sich die URL an, unter der die Anwendung gestartet wurde, und navigieren Sie zu dieser URL mit dem Telefon Browser (z. b. `http://localhost:[PortNumber]/`).

    Sie werden feststellen, dass Ihre Anwendung im Windows Phone Emulator anders aussieht, da jQuery. Mobile. MVC neue Ressourcen in Ihrem Projekt erstellt hat, die für mobile Geräte optimierte Ansichten anzeigen.

    Beachten Sie die Meldung am oberen Rand des Telefons und zeigt den Link an, der zur Desktop Ansicht wechselt. Außerdem ist das Layout der **\_Layout. Mobile. cshtml** , das von dem installierten Paket erstellt wurde, ein anderes Layout in der Anwendung.

    > [!NOTE]
    > Bisher gibt es keinen Link zum zurückkehren zur mobilen Ansicht. Sie wird in späteren Versionen enthalten sein.

    ![Mobile Ansicht der Fotogalerie-Startseite](whats-new-in-aspnet-mvc-4/_static/image24.png "Mobile Ansicht der Fotogalerie-Startseite")

    *Mobile Ansicht der Fotogalerie-Startseite*
8. Drücken Sie in Visual Studio **UMSCHALT** + **F5** , um das Debuggen der Anwendung zu verhindern.

<a id="Task_2_-_Creating_Mobile_Views"></a>
#### <a name="task-2---creating-mobile-views"></a>Aufgabe 2: Erstellen von mobilen Ansichten

In dieser Aufgabe erstellen Sie eine mobile Version der Index Ansicht mit Inhalten, die für eine bessere Darstellung in mobilen Geräten angepasst werden.

1. Kopieren Sie die Ansicht **views\home\index.cshtml** , und fügen Sie Sie ein, um eine Kopie zu erstellen, und benennen Sie die neue Datei in **Index. Mobile. cshtml**um.
2. Öffnen Sie die neue erstellte **Index. Mobile. cshtml** -Ansicht, und ersetzen Sie die vorhandene &lt;UL&gt;-Tags durch diesen Code. Auf diese Weise aktualisieren Sie das &lt;UL&gt;-Tag mit jQuery Mobile-Daten Anmerkungen, um die mobilen Designs aus jQuery zu verwenden.

    [!code-html[Main](whats-new-in-aspnet-mvc-4/samples/sample11.html)]

    > [!NOTE] 
    > 
    > Beachten Sie Folgendes:
    > 
    > - Das auf **ListView** festgelegte **Daten Rollen** Attribut wird die Liste mithilfe der ListView-Stile Rendering.
    > 
    > - Das auf true festgelegte **Data-INSET** -Attribut zeigt die Liste mit abgerundeten Rahmen und Rand an.
    > 
    > - Wenn das auf true festgelegte **Daten Filter** Attribut auf **true** festgelegt ist, wird ein Suchfeld generiert.
    > 
    > Weitere Informationen zu den jQuery Mobile-Konventionen finden Sie in der Projektdokumentation: [ [http://jquerymobile.com/demos/1.1.1/](http://jquerymobile.com/demos/1.1.1/)](http://jquerymobile.com/demos/1.1.1/)
3. Drücken Sie **STRG + S** , um die Änderungen zu speichern.
4. Wechseln Sie zum **Windows Phone Emulator** , und aktualisieren Sie die Website. Beachten Sie das neue Erscheinungsbild der Galerie Liste sowie das neue Suchfeld im oberen Bereich. Geben Sie dann ein Wort in das Suchfeld (z. a. **Tulips**) ein, um eine Suche in der Fotogalerie zu starten.

    ![Katalog mit ListView-Stil und Filterung](whats-new-in-aspnet-mvc-4/_static/image25.png "Katalog mit ListView-Stil und Filterung")

    *Katalog mit ListView-Stil und Filterung*

    Zusammenfassend gesagt, haben Sie mithilfe des Ansichts mobilisierungsrezept eine Kopie der Index Ansicht mit dem &quot;Mobile&quot; Suffix erstellt. Mit diesem Suffix wird ASP.NET MVC 4 angegeben, dass bei jeder von einem mobilen Gerät generierten Anforderung diese Kopie des Indexes verwendet wird. Außerdem haben Sie die Mobile Version der Index Ansicht so aktualisiert, dass Sie jQuery Mobile verwendet, um das Erscheinungsbild von Websites auf mobilen Geräten zu verbessern.
5. Wechseln Sie zurück zu Visual Studio, und öffnen Sie **Site. Mobile. CSS** , das sich unter dem **Inhalts** Ordner befindet.
6. Korrigieren Sie die Positionierung des Foto Titels, damit er auf der rechten Seite des Bilds angezeigt wird. Fügen Sie zu diesem Zweck den folgenden Code in die Datei **Site. Mobile. CSS** ein.

    CSS

    [!code-css[Main](whats-new-in-aspnet-mvc-4/samples/sample12.css)]
7. Drücken Sie **STRG + S** , um die Änderungen zu speichern.
8. Wechseln Sie zurück zum **Windows Phone Emulator** , und aktualisieren Sie die Website. Beachten Sie, dass der Fototitel jetzt ordnungsgemäß positioniert ist.

    ![Titel auf der rechten Seite des Bilds positioniert](whats-new-in-aspnet-mvc-4/_static/image26.png "Titel auf der rechten Seite des Bilds positioniert")

    *Titel auf der rechten Seite des Bilds positioniert*

<a id="Task_3_-_jQuery_Mobile_Themes"></a>
#### <a name="task-3---jquery-mobile-themes"></a>Aufgabe 3: Mobile jQuery-Designs

Jedes Layout und Widget in jQuery Mobile wurde um ein neues objektorientiertes CSS-Framework entwickelt, das es ermöglicht, ein umfassendes einheitliches visuelles Entwurfs Design auf Websites und Anwendungen anzuwenden.

das Standarddesign von jQuery Mobile umfasst 5 Dragquellen, denen die Buchstaben (a, b, c, d, e) zur Kurzübersicht zugewiesen werden.

In dieser Aufgabe aktualisieren Sie das Mobile Layout so, dass es ein anderes Design als das standardmäßige verwendet.

1. Wechseln Sie zurück zu Visual Studio.
2. Öffnen Sie die Datei **\_"Layout. Mobile. cshtml** ", die sich unter " **views\shared**" befindet.
3. Suchen Sie das div-Element, bei dem die Daten Rolle auf &quot;Seite&quot; festgelegt ist, und aktualisieren Sie das **Daten-** Design auf &quot;**e**&quot;.

    [!code-html[Main](whats-new-in-aspnet-mvc-4/samples/sample13.html)]
4. Drücken Sie **STRG + S** , um die Änderungen zu speichern.
5. Aktualisieren Sie die Website im **Windows Phone Emulator** , und beachten Sie das neue Farben Schema.

    ![Mobiles Layout mit einem anderen Farbschema](whats-new-in-aspnet-mvc-4/_static/image27.png "Mobiles Layout mit einem anderen Farbschema")

    *Mobiles Layout mit einem anderen Farbschema*

<a id="Task_4_-_Using_the_View-Switcher_Component_and_the_Browser_Overriding_Features"></a>
#### <a name="task-4---using-the-view-switcher-component-and-the-browser-overriding-features"></a>Aufgabe 4: Verwenden der Ansichts Switcher-Komponente und der über schreibenden Features des Browsers

Eine Konvention für Mobilgeräte optimierte Webseiten besteht darin, einen Link hinzuzufügen, dessen Text etwas wie die Desktop Ansicht oder der vollständige Website Modus ist, mit dem Benutzer zu einer Desktop Version der Seite wechseln können. Das jQuery. Mobile. MVC-Paket enthält eine Beispiel Komponente für **Ansichts Switcher** für diesen Zweck, die in der **\_Layout. Mobile. cshtml** -Sicht verwendet wird.

![Link zum Wechseln zur Desktop Ansicht](whats-new-in-aspnet-mvc-4/_static/image28.png "Link zum Wechseln zur Desktop Ansicht")

*Link zum Wechseln zur Desktop Ansicht*

Der Ansichts Umschaltung verwendet ein neues Feature namens **Browser Überschreibung**. Mit dieser Funktion kann Ihre Anwendung Anforderungen so behandeln, als würden Sie von einem anderen Browser (Benutzer-Agent) stammen als dem, von dem aus Sie tatsächlich stammen.

In dieser Aufgabe untersuchen Sie die Beispiel Implementierung eines von jQuery. Mobile. MVC hinzugefügten Ansichts Switchers und der neuen Browser, der Funktionen in ASP.NET MVC 4 überschreibt.

1. Wechseln Sie zurück zu Visual Studio.
2. Öffnen Sie die Ansicht " **\_Layout. Mobile. cshtml** ", die sich unter dem Ordner " **views\shared** " befindet, und achten Sie darauf, dass die Komponente "View-Switcher" als Teilansicht

    ![Mobiles Layout mithilfe der Ansichts switcherkomponente](whats-new-in-aspnet-mvc-4/_static/image29.png "Mobiles Layout mithilfe der Ansichts switcherkomponente")

    *Mobiles Layout mithilfe der Ansichts switcherkomponente*
3. Öffnen Sie die Teilansicht **\_viewswitcher. cshtml** .

    In der Teilansicht wird die neue Methode **ViewContext. HttpContext. getoverriddenbrowser ()** verwendet, um den Ursprung der Webanforderung zu ermitteln und den entsprechenden Link zum Wechseln zu den Desktop-oder mobilen Ansichten anzuzeigen.

    Die **getoverriddenbrowser** -Methode gibt eine **HttpBrowserCapabilitiesBase** -Instanz zurück, die dem Benutzer-Agent entspricht, der zurzeit für die Anforderung festgelegt ist (tatsächlich oder überschrieben). Sie können diesen Wert verwenden, um Eigenschaften wie **IsMobileDevice**zu erhalten.

    ![Viewswitcher-Teilansicht](whats-new-in-aspnet-mvc-4/_static/image30.png "Viewswitcher-Teilansicht")

    *Viewswitcher-Teilansicht*
4. Öffnen Sie die **ViewSwitcherController.cs** -Klasse, die sich im Ordner **Controller** befindet. Sehen Sie sich an, dass die SwitchView-Aktion durch den Link in der viewswitcher-Komponente aufgerufen wird, und beachten Sie die neuen HttpContext-Methoden.

    - Die **HttpContext. clearoverriddenbrowser ()** -Methode entfernt alle überschriebenen Benutzer-Agents für die aktuelle Anforderung.
    - Die **HttpContext.** Setup-Methode () überschreibt den tatsächlichen Benutzer-Agent-Wert der Anforderung unter Verwendung des angegebenen Benutzer-Agents.  
        ![Viewswitcher-Controller](whats-new-in-aspnet-mvc-4/_static/image31.png "Viewswitcher-Controller ")  
*Viewswitcher-Controller*

        Die Überschreibung des Browsers ist ein zentrales Feature von ASP.NET MVC 4, das auch dann verfügbar ist, wenn Sie das Paket "jQuery. Mobile. MVC" nicht installieren. Diese Funktion wirkt sich jedoch nur auf Sicht, Layout und partielle Ansicht aus und wirkt sich nicht auf Funktionen aus, die vom Request. Browser-Objekt abhängen.

<a id="Task_5_-_Adding_the_View-Switcher_in_the_Desktop_View"></a>
#### <a name="task-5---adding-the-view-switcher-in-the-desktop-view"></a>Aufgabe 5: Hinzufügen des Ansichts Switchers in der Desktop Ansicht

In dieser Aufgabe aktualisieren Sie das Desktop Layout, sodass es den Ansichts Switcher einschließt. Dadurch können mobile Benutzer beim Durchsuchen der Desktop Ansicht zur mobilen Ansicht zurückkehren.

1. Aktualisieren Sie die Website im **Windows Phone-Emulator**.
2. Klicken Sie oben im Katalog auf den Link **Desktop Ansicht** . Beachten Sie, dass es in der Desktop Ansicht keinen Ansichts Switcher gibt, damit Sie zur mobilen Ansicht zurückkehren können.
3. Kehren Sie zu Visual Studio zurück, und öffnen Sie die Ansicht **\_Layout. cshtml** .
4. Suchen Sie den Anmelde Abschnitt, und fügen Sie einen-Befehl ein, um die **\_viewswitcher** -Teilansicht unterhalb der **\_logonpartial** -Teilansicht zu erzeugen. Drücken Sie dann **STRG + S** , um die Änderungen zu speichern.

    [!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample14.cshtml)]
5. Drücken Sie **STRG + S** , um die Änderungen zu speichern.
6. Aktualisieren Sie die Seite im Windows Phone Emulator, und doppelklicken Sie auf den Bildschirm, um ihn zu vergrößern. Beachten Sie, dass auf der Startseite nun der **Mobile Ansichts** Link angezeigt wird, der von der mobilen zur Desktop Ansicht wechselt.

    ![Anzeigen des in der Desktop Ansicht gerenderten Switchers](whats-new-in-aspnet-mvc-4/_static/image32.png "Anzeigen des in der Desktop Ansicht gerenderten Switchers")

    *Anzeigen des in der Desktop Ansicht gerenderten Switchers*
7. Wechseln Sie erneut zur mobilen Ansicht, und navigieren Sie **zur Seite "** Info" (http://localhost [Port]/Home/About). Beachten Sie, dass die Seite "Info" mit dem mobilen Layout (\_"Layout. Mobile. cshtml") angezeigt wird, auch wenn Sie keine about. Mobile. cshtml-Ansicht erstellt haben.

    ![Info-Seite](whats-new-in-aspnet-mvc-4/_static/image33.png "Seite „Info“")

    *Info-Seite*
8. Öffnen Sie schließlich die Website in einem Desktop-Webbrowser. Beachten Sie, dass sich keines der vorherigen Updates auf die Desktop Ansicht ausgewirkt hat.

    ![Photogallery-Desktop Ansicht](whats-new-in-aspnet-mvc-4/_static/image34.png "Photogallery-Desktop Ansicht")

    *Photogallery-Desktop Ansicht*

<a id="Task_6_-_Creating_New_Display_Modes"></a>
#### <a name="task-6---creating-new-display-modes"></a>Aufgabe 6: Erstellen von neuen Anzeigemodi

Mit der neuen Funktion Anzeigemodi kann eine Anwendung Ansichten abhängig vom Browser auswählen, der die Anforderung erzeugt. Wenn ein Desktop Browser z. b. die Startseite anfordert, gibt die Anwendung die Vorlage " **views\home\index.cshtml** " zurück. Wenn ein mobiler Browser die Startseite anfordert, wird die **Views\Home\Index.Mobile.cshtml** -Vorlage von der Anwendung zurückgegeben.

In dieser Aufgabe erstellen Sie ein angepasstes Layout für iPhone-Geräte, und Sie müssen Anforderungen von iPhone-Geräten simulieren. Zu diesem Zweck können Sie entweder einen iPhone-Emulator/-Simulator (z. b. einen [mobilen mobilen Simulator](http://www.electricplum.com/)) oder einen Browser mit Add-ons verwenden, die den Benutzer-Agent ändern. Anweisungen dazu, wie Sie die Benutzer-Agent-Zeichenfolge in einem Safari-Browser zum Emulieren eines iPhones festlegen, finden Sie unter Gewusst wie: zulassen, dass Safari im Blog von David Alison den Wert " [IE" vorgibt](http://www.davidalison.com/2008/05/how-to-let-safari-pretend-its-ie.html)

**Beachten Sie, dass diese Aufgabe optional ist und Sie das gesamte Lab fortsetzen können, ohne es auszuführen.**

1. Drücken Sie in Visual Studio **UMSCHALT** + **F5** , um das Debuggen der Anwendung zu verhindern.
2. Öffnen Sie **Global.asax.cs** , und fügen Sie die folgende using-Anweisung hinzu.

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample15.cs)]
3. Fügen Sie den folgenden hervorgehobenen Code in die Anwendung\_Start-Methode ein.

    (Code Ausschnitt- *ASP.NET MVC 4 Lab-Ex03-iPhone DisplayMode*)

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample16.cs)]

Sie haben einen neuen **defaultdisplaymode mit dem Namen &quot;iPhone&quot;** in der statischen **displaymodeprovider. Instance. Modes** -Liste registriert, der mit jeder eingehenden Anforderung abgeglichen wird. Wenn die eingehende Anforderung die Zeichenfolge &quot;iPhone&quot;enthält, findet ASP.NET MVC die Sichten, deren Name das &quot;iPhone-&quot; Suffix enthält. Der 0-Parameter gibt an, wie spezifisch der neue Modus ist. Diese Ansicht ist beispielsweise spezifischer als die allgemeine &quot;. Mobile&quot; Regel, die Anforderungen von mobilen Geräten abgleicht.

Wenn dieser Code ausgeführt wird und ein iPhone-Browser eine Anforderung generiert, verwendet die Anwendung das Layout **views\shared\\_Layout. iPhone. cshtml** , das Sie in den nächsten Schritten erstellen werden.

> [!NOTE]
> Diese Methode zum Testen der iPhone-Anforderung wurde zu Demonstrationszwecken vereinfacht und funktioniert möglicherweise nicht wie erwartet für jede iPhone-Benutzer-Agent-Zeichenfolge (z. b. bei Test wird Groß-/Kleinschreibung beachtet).

4. Erstellen Sie eine Kopie der Datei **\_Layout. Mobile. cshtml** im Ordner " **views\shared** ", und benennen Sie die Kopie in &quot; **\_Layout. iPhone. cshtml** -&quot;um.
5. Öffnen Sie **\_Layout. iPhone. cshtml** , das Sie im vorherigen Schritt erstellt haben.
6. Suchen Sie das div-Element, wobei das Daten Rollen Attribut auf **Page** festgelegt ist, und ändern Sie das **Data-Theme-** Attribut in &quot;**ein**&quot;.

[!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample17.cshtml)]

Nun verfügen Sie über drei Layouts in Ihrer ASP.NET MVC 4-Anwendung:

1. **\_"Layout. cshtml**": das Standardlayout, das für Desktop Browser verwendet wird.
2. **\_Layout. Mobile. cshtml**: Standardlayout, das für mobile Geräte verwendet wird.
3. **\_Layout. iPhone. cshtml**: bestimmtes Layout für iPhone-Geräte mit einem anderen Farbschema zur Unterscheidung von \_Layout. Mobile. cshtml.
7. Drücken Sie **F5** , um die Anwendung auszuführen und die Website im **Windows Phone Emulator**zu durchsuchen.
8. Öffnen Sie einen **iPhone-Simulator** (Anweisungen zum Installieren und Konfigurieren eines iPhone-Simulators finden Sie in [Anhang C](#AppendixC) ), und navigieren Sie zu der Website. Beachten Sie, dass jedes Telefon die jeweilige Vorlage verwendet.

    ![Verwenden von "-Different-Views-for-each-Mobile-Gerät2"](whats-new-in-aspnet-mvc-4/_static/image35.png)

    *Verwenden verschiedener Ansichten für die einzelnen mobilen Geräte*

<a id="Exercise4"></a>

<a id="Exercise_4_Using_Asynchronous_Controllers"></a>
### <a name="exercise-4-using-asynchronous-controllers"></a>Übung 4: Verwenden von asynchronen Controllern

Microsoft .NET Framework 4,5 führt neue sprach Features in C# und Visual Basic ein, um eine neue Grundlage für die asynchroniität in der .NET-Programmierung bereitzustellen. Diese neue Grundlage führt zu einer asynchronen Programmierung, die etwa so einfach wie die synchrone Programmierung ist. Sie können jetzt asynchrone Aktionsmethoden in ASP.NET MVC 4 schreiben, indem Sie die **asynccontroller** -Klasse verwenden. Sie können asynchrone Aktionsmethoden für nicht-CPU-gebundene Anforderungen mit langer Ausführungszeit verwenden. Dadurch wird verhindert, dass der Webserver bei der Verarbeitung der Anforderung funktioniert. Die asynccontroller-Klasse wird in der Regel für Webdienst Aufrufe mit langer Laufzeit verwendet.

In dieser Übung werden die Grundlagen des asynchronen Vorgangs in ASP.NET MVC 4 erläutert. Weitere Informationen finden Sie im folgenden Artikel: [ [https://msdn.microsoft.com/library/ee728598%28v=vs.100%29.aspx](https://msdn.microsoft.com/library/ee728598%28v=vs.100%29.aspx)](https://msdn.microsoft.com/library/ee728598%28v=vs.100%29.aspx)

<a id="Task_1_-_Implementing_an_Asynchronous_Controller"></a>
#### <a name="task-1---implementing-an-asynchronous-controller"></a>Aufgabe 1: Implementieren eines asynchronen Controllers

1. Öffnen Sie die Projekt Mappe " **Anfang** " unter " **Quelle/Ex4-Async/BEGIN/** Folder". Andernfalls können Sie die **End** -Projekt Mappe, die Sie durch Abschließen der vorherigen Übung erhalten haben, weiterhin verwenden.

   1. Wenn Sie die bereitgestellte **Begin** -Lösung geöffnet haben, müssen Sie einige fehlende nuget-Pakete herunterladen, bevor Sie den Vorgang fortsetzen. Klicken Sie hierzu auf das Menü **Projekt** , und wählen Sie **nuget-Pakete verwalten**aus.
   2. Klicken Sie im Dialogfeld **nuget-Pakete verwalten** auf **Wiederherstellen** , um fehlende Pakete herunterzuladen.
   3. Erstellen Sie abschließend die Projekt Mappe, indem Sie auf **Build** ** | Projekt Mappe erstellen klicken**.

      > [!NOTE]
      > Einer der Vorteile der Verwendung von nuget besteht darin, dass Sie nicht alle Bibliotheken in Ihrem Projekt liefern müssen, um die Projektgröße zu verringern. Mit nuget Power Tools können Sie durch Angabe der Paketversionen in der Datei "Packages. config" alle erforderlichen Bibliotheken herunterladen, wenn Sie das Projekt zum ersten Mal ausführen. Aus diesem Grund müssen Sie diese Schritte ausführen, nachdem Sie eine vorhandene Projekt Mappe in diesem Lab geöffnet haben.
2. Öffnen Sie die **HomeController.cs** -Klasse aus dem Ordner **Controllers** .
3. Fügen Sie die folgende using-Anweisung hinzu.

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample18.cs)]
4. Aktualisieren Sie die **HomeController** -Klasse so, dass Sie von **asynccontroller**geerbt wird. Controller, die von asynccontroller abgeleitet werden, ermöglichen es ASP.net, asynchrone Anforderungen zu verarbeiten, und Sie können weiterhin synchrone Aktionsmethoden verarbeiten.

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample19.cs)]
5. Fügen Sie der **Index** -Methode das **Async** -Schlüsselwort hinzu, und geben Sie die **typaufgabe&lt;"Aktions Ergebnis"&gt;** zurück.

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample20.cs)]

    > [!NOTE]
    > Das **Async** -Schlüsselwort ist eines der neuen Schlüsselwörter, die der .NET Framework 4,5 bereitstellt. Er weist den Compiler an, dass diese Methode asynchronen Code enthält. Ein **Task** -Objekt stellt einen asynchronen Vorgang dar, der zu einem späteren Zeitpunkt in der Zukunft ausgeführt werden kann.
6. Ersetzen Sie den- **Client. Getasync ()** -Aufrufe mit der vollständigen Async-Version, die das Erwartungs Wort erwartet, wie unten dargestellt.

    (Code Ausschnitt- *ASP.NET MVC 4 Lab-Ex04-getasync*)

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample21.cs)]

    > [!NOTE]
    > In der vorherigen Version haben Sie die **Result** -Eigenschaft des **Task** -Objekts verwendet, um den Thread zu blockieren, bis das Ergebnis zurückgegeben wird (Synchronisierungs Version).
    > 
    > Das Hinzufügen des **Wait** -Schlüssel Worts weist den Compiler an, asynchron auf die vom Methoden aufrufzeichen zurückgegebene Aufgabe zu warten. Dies bedeutet, dass der restliche Code nur dann als Rückruf ausgeführt wird, nachdem die erwartete Methode abgeschlossen wurde. Ein weiterer Hinweis ist, dass Sie den try-catch-Block nicht ändern müssen, um dies zu erreichen: die Ausnahmen, die im Hintergrund oder im Vordergrund auftreten, werden nach wie vor ohne zusätzliche Arbeit abgefangen, indem ein vom Framework bereitgestellter Handler verwendet wird.
7. Ändern Sie den Code, um die asynchrone Implementierung fortzusetzen, indem Sie die Zeilen durch den neuen Code ersetzen, wie unten gezeigt.

    (Code Ausschnitt- *ASP.NET MVC 4 Lab-Ex04-leserasstringasync*)

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample22.cs)]
8. Führen Sie die Anwendung aus. Sie werden feststellen, dass keine größeren Änderungen vorgenommen werden, aber der Code blockiert keinen Thread aus dem Thread Pool, wodurch die Server Ressourcen besser genutzt und die Leistung verbessert wird.

    > [!NOTE]
    > Weitere Informationen zu den neuen Funktionen für die asynchrone Programmierung in der Lab-&quot;die **asynchrone Programmierung in .net C# 4,5 mit und Visual Basic**&quot;, die im Visual Studio-Trainingskit enthalten sind.

<a id="Task_2_-_Handling_Time-Outs_with_Cancellation_Tokens"></a>
#### <a name="task-2---handling-time-outs-with-cancellation-tokens"></a>Aufgabe 2: Behandeln von Timeouts mit Abbruch Token

Asynchrone Aktionsmethoden, die Task Instanzen zurückgeben, können auch Timeouts unterstützen. In dieser Aufgabe aktualisieren Sie den Code der Index Methode, um ein Timeout Szenario mit einem Abbruch Token zu verarbeiten.

1. Kehren Sie zu Visual Studio zurück, und drücken Sie **UMSCHALT + F5** , um das Debugging zu verhindern.
2. Fügen Sie der **HomeController.cs** -Datei die folgende using-Anweisung hinzu.

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample23.cs)]
3. Aktualisieren Sie die Index Aktion, um ein **CancellationToken** -Argument zu empfangen.

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample24.cs)]
4. Aktualisieren Sie den **getasync** -Befehl, um das Abbruch Token zu übergeben.

    (Code Ausschnitt- *ASP.NET MVC 4 Lab-Ex04-SendAsync mit CancellationToken*)

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample25.cs)]
5. Ergänzen Sie die *Index* -Methode **mit einem** **AsyncTimeout** -Attribut, das auf 500 Millisekunden festgelegt ist, und ein " **Lenker Error** "-Attribut, das für die Behandlung von **TaskCanceledException** konfiguriert ist.

    (Code Ausschnitt- *ASP.NET MVC 4 Lab-Ex04-Attribute*)

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample26.cs)]
6. Öffnen Sie die **photocontroller** -Klasse, und aktualisieren Sie die **Gallery** -Methode, um die Ausführung 1000 Millisekunden (1 Sekunde) zu verzögern, um eine Aufgabe mit langer Laufzeit zu simulieren.

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample27.cs)]
7. Öffnen Sie die Datei **Web. config** , und aktivieren Sie benutzerdefinierte Fehler, indem Sie das folgende Element hinzufügen.

    [!code-xml[Main](whats-new-in-aspnet-mvc-4/samples/sample28.xml)]
8. Erstellen Sie eine neue Ansicht in **views\shared** benannte **TimedOut,** und verwenden Sie das Standardlayout. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf den Ordner " **views\shared** ", und wählen Sie **Hinzufügen | Anzeigen**.

    ![Verwenden verschiedener Ansichten für die einzelnen mobilen Geräte](whats-new-in-aspnet-mvc-4/_static/image36.png "Verwenden verschiedener Ansichten für die einzelnen mobilen Geräte")

    *Verwenden verschiedener Ansichten für die einzelnen mobilen Geräte*
9. Aktualisieren Sie den Inhalt der **TimedOut** -Ansicht, wie unten gezeigt.

    [!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample29.cshtml)]
10. Führen Sie die Anwendung aus, und navigieren Sie zur Stamm-URL. Wenn Sie einen **Thread** hinzugefügt haben. der Standbymodus beträgt 1000 Millisekunden, erhalten Sie einen Timeout Fehler, der vom Attribut " **AsyncTimeout** " generiert und durch das Attribut " **Lenker Error** " abgefangen wird.

    ![Behandelte Timeout Ausnahme](whats-new-in-aspnet-mvc-4/_static/image37.png "Behandelte Timeout Ausnahme")

    *Behandelte Timeout Ausnahme*

> [!NOTE]
> Außerdem können Sie diese Anwendung auf Windows Azure-Websites bereitstellen, [indem Sie Anhang D: Veröffentlichen einer ASP.NET MVC 4-Anwendung mit Web deploy verwenden](#AppendixD).

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a>Zusammenfassung

In dieser praktischen Übungseinheit haben Sie einige der neuen Features in ASP.NET MVC 4 beobachtet. Die folgenden Konzepte wurden erläutert:

- Profitieren Sie von den Verbesserungen der ASP.NET MVC-Projektvorlagen (einschließlich der neuen Projektvorlage für mobile Anwendungen).
- Verwenden des HTML5-viewportattributs und von CSS-Medien Abfragen zur Verbesserung der Anzeige auf mobilen Geräten
- Verwenden von jQuery Mobile für Progressive Erweiterungen und zum Entwickeln von Touchscreen-optimierter Webbenutzer Oberfläche
- Erstellen von mobilen Sichten
- Verwenden der View-Switcher-Komponente zum Umschalten zwischen mobilen und Desktop Ansichten in der Anwendung
- Erstellen von asynchronen Controllern mithilfe von Task Unterstützung

<a id="AppendixA"></a>

<a id="Appendix_A_Using_Code_Snippets"></a>
## <a name="appendix-a-using-code-snippets"></a>Anhang A: Verwenden von Code Ausschnitten

Mit Code Ausschnitten haben Sie den gesamten Code, den Sie benötigen. Im Lab-Dokument werden Sie genau wissen, wann Sie Sie verwenden können, wie in der folgenden Abbildung dargestellt.

![Verwenden von Visual Studio-Code Ausschnitten zum Einfügen von Code in Ihr Projekt](whats-new-in-aspnet-mvc-4/_static/image38.png "Verwenden von Visual Studio-Code Ausschnitten zum Einfügen von Code in Ihr Projekt")

*Verwenden von Visual Studio-Code Ausschnitten zum Einfügen von Code in Ihr Projekt*

***So fügen Sie einen Code Ausschnitt mithilfe der Tastatur hinzuC# (nur)***

1. Platzieren Sie den Cursor an der Stelle, an der Sie den Code einfügen möchten.
2. Beginnen Sie mit der Eingabe des Ausschnitt namens (ohne Leerzeichen oder Bindestriche).
3. Sehen Sie sich an, wie IntelliSense übereinstimmende Code Ausschnitte anzeigt.
4. Wählen Sie den richtigen Ausschnitt aus (oder geben Sie die Eingabe fort, bis der gesamte Name des Ausschnitts ausgewählt ist).
5. Drücken Sie zweimal die Tab-Taste, um den Ausschnitt an der Cursorposition einzufügen.

![Beginnen Sie mit der Eingabe des Ausschnitt namens.](whats-new-in-aspnet-mvc-4/_static/image39.png "Beginnen Sie mit der Eingabe des Ausschnitt namens.")

*Beginnen Sie mit der Eingabe des Ausschnitt namens.*

![Drücken Sie TAB, um den markierten Ausschnitt auszuwählen.](whats-new-in-aspnet-mvc-4/_static/image40.png "Drücken Sie TAB, um den markierten Ausschnitt auszuwählen.")

*Drücken Sie TAB, um den markierten Ausschnitt auszuwählen.*

![Drücken Sie erneut die Tab-Taste, und der Ausschnitt wird erweitert.](whats-new-in-aspnet-mvc-4/_static/image41.png "Drücken Sie erneut die Tab-Taste, und der Ausschnitt wird erweitert.")

*Drücken Sie erneut die Tab-Taste, und der Ausschnitt wird erweitert.*

***So fügen Sie einen Code Ausschnitt mithilfe der Maus hinzuC#(, Visual Basic und XML)***

1. Klicken Sie mit der rechten Maustaste auf den Ort, an dem Sie den Code Ausschnitt einfügen möchten.
2. Wählen Sie **Ausschnitt einfügen** und dann **meine Code Ausschnitte**aus.
3. Wählen Sie den entsprechenden Code Ausschnitt aus der Liste aus, indem Sie darauf klicken.

![Klicken Sie mit der rechten Maustaste darauf, wo Sie den Code Ausschnitt einfügen möchten, und wählen Sie Ausschnitt einfügen aus.](whats-new-in-aspnet-mvc-4/_static/image42.png "Klicken Sie mit der rechten Maustaste darauf, wo Sie den Code Ausschnitt einfügen möchten, und wählen Sie Ausschnitt einfügen aus.")

*Klicken Sie mit der rechten Maustaste darauf, wo Sie den Code Ausschnitt einfügen möchten, und wählen Sie Ausschnitt einfügen aus.*

![Wählen Sie den entsprechenden Code Ausschnitt aus der Liste aus, indem Sie darauf klicken.](whats-new-in-aspnet-mvc-4/_static/image43.png "Wählen Sie den entsprechenden Code Ausschnitt aus der Liste aus, indem Sie darauf klicken.")

*Wählen Sie den entsprechenden Code Ausschnitt aus der Liste aus, indem Sie darauf klicken.*

<a id="AppendixB"></a>

<a id="Appendix_B_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-b-installing-visual-studio-express-2012-for-web"></a>Anhang B: Installieren von Visual Studio Express 2012 für das Web

Sie können **Microsoft Visual Studio Express 2012 für Web** oder eine andere &quot;Express&quot; Version mit dem **[Microsoft-Webplattform-Installer](https://www.microsoft.com/web/downloads/platform.aspx)** installieren. Die folgenden Anweisungen führen Sie durch die Schritte, die zum Installieren *von Visual Studio Express 2012 für Web* mithilfe von *Microsoft-Webplattform-Installer*erforderlich sind.

1. Wechseln Sie zu [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169). Wenn Sie den Webplattform-Installer bereits installiert haben, können Sie ihn auch öffnen und nach dem Produkt &quot;*Visual Studio Express 2012 für das Web mit dem Windows Azure SDK*&quot;suchen.
2. Klicken Sie auf **jetzt installieren**. Wenn Sie nicht über einen **Webplattform-Installer** verfügen, werden Sie umgeleitet, um Sie zuerst herunterzuladen und zu installieren.
3. Nachdem der **Webplattform-Installer** geöffnet ist, klicken Sie auf **Installieren** , um das Setup zu starten.

    ![Installieren von Visual Studio Express](whats-new-in-aspnet-mvc-4/_static/image44.png "Installieren von Visual Studio Express")

    *Installieren von Visual Studio Express*
4. Lesen Sie die Lizenzbedingungen für alle Produkte, und klicken Sie auf **ich** Stimme zu, um fortzufahren.

    ![Akzeptieren der Lizenzbedingungen](whats-new-in-aspnet-mvc-4/_static/image45.png)

    *Akzeptieren der Lizenzbedingungen*
5. Warten Sie, bis der Download-und Installationsvorgang abgeschlossen ist.

    ![Installationsstatus](whats-new-in-aspnet-mvc-4/_static/image46.png)

    *Installationsfortschritt*
6. Wenn die Installation abgeschlossen ist, klicken Sie auf **Fertig**stellen.

    ![Installation abgeschlossen](whats-new-in-aspnet-mvc-4/_static/image47.png)

    *Installation abgeschlossen*
7. Klicken Sie zum Schließen des Webplattform-Installers auf **Beenden** .
8. Um Visual Studio Express für das Web zu öffnen, navigieren Sie zum **Start** Bildschirm, und beginnen Sie mit dem Schreiben &quot;**vs Express**&quot;und klicken Sie dann auf die Kachel **vs Express für Web** .

    ![VS Express für Web-Kachel](whats-new-in-aspnet-mvc-4/_static/image48.png)

    *VS Express für Web-Kachel*

<a id="AppendixC"></a>

<a id="Appendix_C_Installing_WebMatrix_2_and_iPhone_Simulator"></a>
## <a name="appendix-c-installing-webmatrix-2-and-iphone-simulator"></a>Anhang C: Installieren von webmatrix 2 und iPhone Simulator

Wenn Sie Ihre Website auf einem simulierten iPhone-Gerät ausführen möchten, können Sie die webmatrix-Erweiterung &quot;mobilen mobilen Simulators für das iPhone-&quot;verwenden. Außerdem können Sie dieselbe Erweiterung zum Ausführen des Simulators aus Visual Studio 2012 konfigurieren.

<a id="ApxCTask1"></a>

<a id="Task_1_-_Installing_WebMatrix_2"></a>
#### <a name="task-1---installing-webmatrix-2"></a>Aufgabe 1: Installieren von webmatrix 2

1. Wechseln Sie zu [[https://go.microsoft.com/?linkid=9809776](https://go.microsoft.com/?linkid=9809776)](https://go.microsoft.com/?linkid=9810169). Wenn Sie den Webplattform-Installer bereits installiert haben, können Sie ihn auch öffnen und nach dem Produkt &quot;*webmatrix 2*&quot;suchen.
2. Klicken Sie auf **jetzt installieren**. Wenn Sie nicht über einen **Webplattform-Installer** verfügen, werden Sie umgeleitet, um Sie zuerst herunterzuladen und zu installieren.
3. Nachdem der **Webplattform-Installer** geöffnet ist, klicken Sie auf **Installieren** , um das Setup zu starten.

    ![Installieren von webmatrix 2](whats-new-in-aspnet-mvc-4/_static/image49.png "Installieren von webmatrix 2")

    *Installieren von webmatrix 2*
4. Lesen Sie die Lizenzbedingungen für alle Produkte, und klicken Sie auf **ich** Stimme zu, um fortzufahren.

    ![Akzeptieren der Lizenzbedingungen](whats-new-in-aspnet-mvc-4/_static/image50.png "Akzeptieren der Lizenzbedingungen")

    *Akzeptieren der Lizenzbedingungen*
5. Warten Sie, bis der Download-und Installationsvorgang abgeschlossen ist.

    ![Installationsfortschritt](whats-new-in-aspnet-mvc-4/_static/image51.png "Installationsstatus")

    *Installationsfortschritt*
6. Wenn die Installation abgeschlossen ist, klicken Sie auf **Fertig**stellen.

    ![Installation abgeschlossen](whats-new-in-aspnet-mvc-4/_static/image52.png "Installation abgeschlossen")

    *Installation abgeschlossen*
7. Klicken Sie zum Schließen des Webplattform-Installers auf **Beenden** .

<a id="ApxCTask2"></a>

<a id="Task_2_-_Installing_the_iPhone_Simulator_Extension"></a>
#### <a name="task-2---installing-the-iphone-simulator-extension"></a>Aufgabe 2: Installieren der iPhone-Simulator-Erweiterung

1. Führen Sie **webmatrix** aus, und öffnen Sie eine beliebige vorhandene Website, oder erstellen Sie eine neue Website.
2. Klicken Sie im Menüband **Start** auf die Schaltfläche **Ausführen** , und wählen Sie **neue hinzufügen**.

    ![Hinzufügen einer neuen webmatrix-Erweiterung](whats-new-in-aspnet-mvc-4/_static/image53.png "Hinzufügen einer neuen webmatrix-Erweiterung")

    *Hinzufügen einer neuen webmatrix-Erweiterung*
3. Wählen Sie **iPhone Simulator** aus, und klicken Sie auf **Installieren**.

    ![Webmatrix-Erweiterungen durchsuchen](whats-new-in-aspnet-mvc-4/_static/image54.png "Webmatrix-Erweiterungen durchsuchen")

    *Webmatrix-Erweiterungen durchsuchen*
4. Klicken Sie in den Paket Details auf **Installieren** , um mit der Installation der Erweiterung fortzufahren.

    ![iPhone-Simulator-Erweiterung](whats-new-in-aspnet-mvc-4/_static/image55.png "iPhone-Simulator-Erweiterung")

    *iPhone-Simulator-Erweiterung*
5. Lesen und akzeptieren Sie die EULA für die Erweiterung.

    ![EULA für webmatrix-Erweiterungen](whats-new-in-aspnet-mvc-4/_static/image56.png "EULA für webmatrix-Erweiterungen")

    *EULA für webmatrix-Erweiterungen*
6. Nun können Sie die Website mit der iPhone-Simulator-Option aus webmatrix ausführen.

    ![Mit iPhone ausführen](whats-new-in-aspnet-mvc-4/_static/image57.png "Mit iPhone ausführen")

    *Mit iPhone ausführen*

<a id="ApxCTask3"></a>

<a id="Task_3_-_Configuring_Visual_Studio_2012_to_run_iPhone_Simulator"></a>
#### <a name="task-3---configuring-visual-studio-2012-to-run-iphone-simulator"></a>Aufgabe 3: Konfigurieren von Visual Studio 2012 zum Ausführen des iPhone-Simulators

1. Öffnen Sie **Visual Studio 2012** , und öffnen Sie eine beliebige Website, oder erstellen Sie ein neues Projekt.
2. Klicken Sie in der Schaltfläche Ausführen auf den Pfeil nach unten, und wählen Sie **Durchsuchen mit**

    ![Durchsuchen](whats-new-in-aspnet-mvc-4/_static/image58.png "Durchsuchen")

    *Durchsuchen*
3. Klicken Sie im Dialogfeld &quot;mit&quot; durchsuchen auf **Hinzufügen**.
4. Verwenden Sie im Dialog &quot;Dialog&quot; "Programm hinzufügen" die folgenden Werte:

   - **Programm**: c:\Users\*{CurrentUser} * \appdata\local\microsoft\webmatrix\extensions\20\iphonesimulator\elektricmobilesim\elektricmobilesim.exe *(aktualisieren Sie den Pfad entsprechend)*
   - **Argumente**: &quot;1&quot;
   - Anzeige **Name**: iPhone-Simulator

     ![Programm hinzufügen](whats-new-in-aspnet-mvc-4/_static/image59.png "Programm hinzufügen")

     *Programm zum Durchsuchen hinzufügen*
5. Klicken Sie auf **OK** , und schließen Sie die Dialogfelder.
6. Nun können Sie Ihre Webanwendungen im iPhone-Simulator von Visual Studio 2012 ausführen.

    ![Mit iPhone-Simulator durchsuchen](whats-new-in-aspnet-mvc-4/_static/image60.png "Mit iPhone-Simulator durchsuchen")

    *Mit iPhone-Simulator durchsuchen*

<a id="AppendixD"></a>

<a id="Appendix_D_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-d-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Anhang D: Veröffentlichen einer ASP.NET MVC 4-Anwendung mit Web deploy

In diesem Anhang wird gezeigt, wie Sie eine neue Website aus dem Windows Azure-Verwaltungsportal erstellen und die Anwendung veröffentlichen, die Sie durch die folgenden Schritte abgerufen haben. nutzen Sie dabei die von Windows Azure bereitgestellte Funktion zur Web deploy Veröffentlichung.

<a id="ApxDTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-windows-azure-portal"></a>Aufgabe 1: Erstellen einer neuen Website über das Windows Azure-Portal

1. Wechseln Sie zum [Windows Azure-Verwaltungsportal](https://manage.windowsazure.com/) und melden Sie sich mit den Microsoft-Anmelde Informationen an, die Ihrem Abonnement zugeordnet sind.

    > [!NOTE]
    > Mit Windows Azure können Sie 10 ASP.NET Websites kostenlos hosten und dann skalieren, wenn Ihr Datenverkehr zunimmt. Sie können sich [hier](https://aka.ms/aspnet-hol-azure)registrieren.

    ![Anmelden bei Windows Azure-Portal](whats-new-in-aspnet-mvc-4/_static/image61.png "Anmelden bei Windows Azure-Portal")

    *Anmelden bei Windows Azure Verwaltungsportal*
2. Klicken Sie in der Befehlsleiste auf **neu** .

    ![Erstellen einer neuen Website](whats-new-in-aspnet-mvc-4/_static/image62.png "Erstellen einer neuen Website")

    *Erstellen einer neuen Website*
3. Klicken Sie auf **Compute** - | **Website**. Wählen Sie dann **schneller** Fassung aus. Geben Sie eine verfügbare URL für die neue Website an, und klicken Sie auf **Website erstellen**.

    > [!NOTE]
    > Eine Windows Azure-Website ist der Host für eine Webanwendung, die in der Cloud ausgeführt wird und die Sie steuern und verwalten können. Mit der Option schneller Fassung können Sie eine abgeschlossene Webanwendung von außerhalb des Portals auf der Windows Azure-Website bereitstellen. Sie enthält keine Schritte zum Einrichten einer Datenbank.

    ![Erstellen einer neuen Website mithilfe der schneller Fassung](whats-new-in-aspnet-mvc-4/_static/image63.png "Erstellen einer neuen Website mithilfe der schneller Fassung")

    *Erstellen einer neuen Website mithilfe der schneller Fassung*
4. Warten Sie, bis die neue **Website** erstellt wurde.
5. Nachdem die Website erstellt wurde, klicken Sie auf den Link unter der **URL** -Spalte. Überprüfen Sie, ob die neue Website funktioniert.

    ![Navigieren zur neuen Website](whats-new-in-aspnet-mvc-4/_static/image64.png "Navigieren zur neuen Website")

    *Navigieren zur neuen Website*

    ![Website wird ausgeführt](whats-new-in-aspnet-mvc-4/_static/image65.png "Website wird ausgeführt")

    *Website wird ausgeführt*
6. Wechseln Sie zurück zum Portal, und klicken Sie in der Spalte **Name** auf den Namen der Website, um die Verwaltungs Seiten anzuzeigen.

    ![Öffnen der Website-Verwaltungs Seiten](whats-new-in-aspnet-mvc-4/_static/image66.png "Öffnen der Website-Verwaltungs Seiten")

    *Öffnen der Website-Verwaltungs Seiten*
7. Klicken Sie auf der Seite **Dashboard** im Abschnitt **kurzer Blick** auf den Link **Veröffentlichungs Profil herunterladen** .

    > [!NOTE]
    > Das *Veröffentlichungs Profil* enthält alle Informationen, die zum Veröffentlichen einer Webanwendung auf einer Windows Azure-Website für die einzelnen aktivierten Veröffentlichungs Methoden erforderlich sind. Das Veröffentlichungs Profil enthält die URLs, Benutzer Anmelde Informationen und Daten Bank Zeichenfolgen, die erforderlich sind, um eine Verbindung mit den einzelnen Endpunkten herzustellen und diese zu authentifizieren, für die eine Veröffentlichungs Methode aktiviert ist. **Microsoft webmatrix 2**, **Microsoft Visual Studio Express für Web** und **Microsoft Visual Studio 2012** unterstützen das Lesen von Veröffentlichungs Profilen, um die Konfiguration dieser Programme für das Veröffentlichen von Webanwendungen auf Windows Azure-Websites zu automatisieren.

    ![Das Website-Veröffentlichungs Profil wird heruntergeladen.](whats-new-in-aspnet-mvc-4/_static/image67.png "Das Website-Veröffentlichungs Profil wird heruntergeladen.")

    *Das Website-Veröffentlichungs Profil wird heruntergeladen.*
8. Laden Sie die Veröffentlichungs Profil Datei an einen bekannten Speicherort herunter. In dieser Übung erfahren Sie, wie Sie diese Datei zum Veröffentlichen einer Webanwendung auf Windows Azure-Websites in Visual Studio verwenden.

    ![Die Veröffentlichungs Profil Datei wird gespeichert.](whats-new-in-aspnet-mvc-4/_static/image68.png "Das Veröffentlichungs Profil wird gespeichert.")

    *Die Veröffentlichungs Profil Datei wird gespeichert.*

<a id="ApxDTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a>Aufgabe 2: Konfigurieren des Datenbankservers

Wenn Ihre Anwendung SQL Server Datenbanken nutzt, müssen Sie einen SQL-Daten Bank Server erstellen. Wenn Sie eine einfache Anwendung bereitstellen möchten, die SQL Server nicht verwendet, können Sie diese Aufgabe überspringen.

1. Sie benötigen einen SQL-Datenbankserver zum Speichern der Anwendungsdatenbank. Sie können die SQL-Datenbankserver aus Ihrem Abonnement im Windows Azure-Verwaltungs Portal unter **SQL-Datenbanken** | **Server** | **Dashboard des Servers**anzeigen. Wenn Sie keinen Server erstellt haben, können Sie ihn mithilfe der Schaltfläche **Hinzufügen** auf der Befehlsleiste erstellen. Notieren Sie sich den **Servernamen und die URL, den Administrator Anmelde Namen und das Kennwort**, da Sie Sie in den nächsten Aufgaben verwenden werden. Erstellen Sie die Datenbank noch nicht, da Sie in einer späteren Phase erstellt wird.

    ![Dashboard des SQL-Datenbankservers](whats-new-in-aspnet-mvc-4/_static/image69.png "Dashboard des SQL-Datenbankservers")

    *Dashboard des SQL-Datenbankservers*
2. In der nächsten Aufgabe testen Sie die Datenbankverbindung aus Visual Studio. aus diesem Grund müssen Sie Ihre lokale IP-Adresse in die Liste der **zulässigen IP-Adressen**des Servers einschließen. Klicken Sie hierzu auf **Konfigurieren**, wählen Sie die IP-Adresse der **aktuellen Client-IP-Adresse** aus, und fügen Sie Sie in die Textfelder Start-IP- **Adresse** und End- **IP-Adresse** ein, und klicken Sie auf die Schaltfläche ![Add-Client-IP-Address-OK-Button](whats-new-in-aspnet-mvc-4/_static/image70.png)

    ![Client-IP-Adresse](whats-new-in-aspnet-mvc-4/_static/image71.png)

    *Client-IP-Adresse*
3. Sobald die **Client-IP-Adresse** der Liste zulässige IP-Adressen hinzugefügt wurde, klicken Sie auf **Speichern** , um die Änderungen zu bestätigen.

    ![Änderungen bestätigen](whats-new-in-aspnet-mvc-4/_static/image72.png)

    *Änderungen bestätigen*

<a id="ApxDTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Aufgabe 3: Veröffentlichen einer ASP.NET MVC 4-Anwendung mit Web deploy

1. Kehren Sie zur ASP.NET MVC 4-Lösung zurück. Klicken Sie im **Projektmappen-Explorer**mit der rechten Maustaste auf das Website Projekt, und wählen Sie **veröffentlichen**aus.

    ![Veröffentlichen der Anwendung](whats-new-in-aspnet-mvc-4/_static/image73.png "Veröffentlichen der Anwendung")

    *Veröffentlichen der Website*
2. Importieren Sie das Veröffentlichungs Profil, das Sie in der ersten Aufgabe gespeichert haben.

    ![Importieren des Veröffentlichungs Profils](whats-new-in-aspnet-mvc-4/_static/image74.png "Importieren des Veröffentlichungs Profils")

    *Veröffentlichungs Profil wird importiert.*
3. Klicken Sie auf **Verbindung**überprüfen. Klicken Sie nach Abschluss der Überprüfung auf **weiter**.

    > [!NOTE]
    > Die Überprüfung ist fertiggestellt, sobald ein grünes Häkchen neben der Schaltfläche Verbindung überprüfen angezeigt wird.

    ![Die Verbindung wird überprüft.](whats-new-in-aspnet-mvc-4/_static/image75.png "Die Verbindung wird überprüft.")

    *Die Verbindung wird überprüft.*
4. Klicken Sie auf der Seite **Einstellungen** unter dem Abschnitt **Datenbanken** auf die Schaltfläche neben dem Textfeld der Datenbankverbindung (z. b. **DefaultConnection**).

    ![Webbereitstellungs Konfiguration](whats-new-in-aspnet-mvc-4/_static/image76.png "Webbereitstellungs Konfiguration")

    *Webbereitstellungs Konfiguration*
5. Konfigurieren Sie die Datenbankverbindung wie folgt:

   - Geben Sie unter **Server Name** die URL des SQL-Datenbankservers unter Verwendung des *TCP:* -Präfix ein.
   - Geben Sie unter **Benutzername** den Anmelde Namen des Server Administrators ein.
   - Geben Sie unter **Kennwort** Ihren Server Administrator-Anmelde Kennwort ein.
   - Geben Sie einen neuen Datenbanknamen ein, z. b.: *MVC4SampleDB*.

     ![Konfigurieren der Ziel Verbindungs Zeichenfolge](whats-new-in-aspnet-mvc-4/_static/image77.png "Konfigurieren der Ziel Verbindungs Zeichenfolge")

     *Konfigurieren der Ziel Verbindungs Zeichenfolge*
6. Klicken Sie dann auf **OK**. Wenn Sie zum Erstellen der Datenbank aufgefordert werden, klicken Sie auf **Ja**.

    ![Erstellen der Datenbank](whats-new-in-aspnet-mvc-4/_static/image78.png "Erstellen der Daten Bank Zeichenfolge")

    *Erstellen der Datenbank*
7. Die Verbindungs Zeichenfolge, die Sie zum Herstellen einer Verbindung mit der SQL-Datenbank in Windows Azure verwenden, wird im Textfeld Standardverbindung angezeigt. Klicken Sie dann auf **Weiter**.

    ![Verbindungs Zeichenfolge mit Verweis auf SQL-Datenbank](whats-new-in-aspnet-mvc-4/_static/image79.png "Verbindungs Zeichenfolge mit Verweis auf SQL-Datenbank")

    *Verbindungs Zeichenfolge mit Verweis auf SQL-Datenbank*
8. Klicken Sie auf der Seite **Vorschau** auf **veröffentlichen**.

    ![Veröffentlichen der Webanwendung](whats-new-in-aspnet-mvc-4/_static/image80.png "Veröffentlichen der Webanwendung")

    *Veröffentlichen der Webanwendung*
9. Nachdem der Veröffentlichungs Vorgang abgeschlossen ist, wird die veröffentlichte Website in Ihrem Standardbrowser geöffnet.

    ![In Windows Azure veröffentlichte Anwendung](whats-new-in-aspnet-mvc-4/_static/image81.png "In Windows Azure veröffentlichte Anwendung")

    *In Windows Azure veröffentlichte Anwendung*
