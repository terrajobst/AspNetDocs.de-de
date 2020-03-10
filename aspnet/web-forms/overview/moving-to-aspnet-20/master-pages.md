---
uid: web-forms/overview/moving-to-aspnet-20/master-pages
title: Master Seiten | Microsoft-Dokumentation
author: microsoft
description: Eine der wichtigsten Komponenten einer erfolgreichen Website ist ein konsistentes Erscheinungsbild. In ASP.NET 1. x haben Entwickler Benutzer Steuerelemente verwendet, um die Common Page Elem zu replizieren...
ms.author: riande
ms.date: 02/20/2005
ms.assetid: 9c0cce4d-efd9-4c14-b0e8-a1a140abb3f4
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/master-pages
msc.type: authoredcontent
ms.openlocfilehash: 36f2caf7c2c9bcafd22c8f6681c1d6b19fe5078a
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78457581"
---
# <a name="master-pages"></a>Masterseiten

von [Microsoft](https://github.com/microsoft)

> Eine der wichtigsten Komponenten einer erfolgreichen Website ist ein konsistentes Erscheinungsbild. In ASP.NET 1. x haben Entwickler Benutzer Steuerelemente verwendet, um allgemeine Seitenelemente in einer Webanwendung zu replizieren. Obwohl dies sicherlich eine funktionierende Lösung ist, hat die Verwendung von Benutzer Steuerelementen einige Nachteile. Beispielsweise erfordert eine Änderung der Position eines Benutzer Steuer Elements eine Änderung an mehreren Seiten auf einer Site. Benutzer Steuerelemente werden auch nicht in Designansicht gerendert, nachdem Sie auf eine Seite eingefügt wurden.

Eine der wichtigsten Komponenten einer erfolgreichen Website ist ein konsistentes Erscheinungsbild. In ASP.NET 1. x haben Entwickler Benutzer Steuerelemente verwendet, um allgemeine Seitenelemente in einer Webanwendung zu replizieren. Obwohl dies sicherlich eine funktionierende Lösung ist, hat die Verwendung von Benutzer Steuerelementen einige Nachteile. Beispielsweise erfordert eine Änderung der Position eines Benutzer Steuer Elements eine Änderung an mehreren Seiten auf einer Site. Benutzer Steuerelemente werden auch nicht in Designansicht gerendert, nachdem Sie auf eine Seite eingefügt wurden.

ASP.NET 2,0 führt Masterseiten ein, um ein konsistentes Erscheinungsbild zu gewährleisten, und wie Sie bald sehen werden, stellen Masterseiten eine bedeutende Verbesserung gegenüber der Benutzer Steuermethode dar.

## <a name="why-master-pages"></a>Gründe für Master Seiten

Vielleicht Fragen Sie sich vielleicht, warum Masterseiten in ASP.NET 2,0 benötigt wurden. Schließlich verwenden Website Entwickler bereits Benutzer Steuerelemente in ASP.NET 1. x, um Inhaltsbereiche zwischen Seiten freizugeben. Es gibt mehrere Gründe, warum Benutzer Steuerelemente eine weniger optimale Lösung für das Erstellen eines gemeinsamen Layouts sind.

Benutzer Steuerelemente definieren das Seitenlayout nicht tatsächlich. Stattdessen definieren Sie das Layout und die Funktionalität für einen Teil einer Seite. Der Unterschied zwischen diesen beiden ist wichtig, da Sie die Verwaltbarkeit einer Benutzer Steuerelement-Lösung viel schwieriger macht. Wenn Sie z. b. die Position eines Benutzer Steuer Elements auf der Seite ändern möchten, müssen Sie die tatsächliche Seite bearbeiten, auf der das Benutzer Steuerelement angezeigt wird. Das ist in Ordnung, wenn Sie nur wenige Seiten haben, aber auf großen Websites wird es schnell zu einem Standort Verwaltungs Alptraum!

Ein weiterer Nachteil der Verwendung von Benutzer Steuerelementen zum Definieren eines gemeinsamen Layouts ist die Architektur von ASP.net selbst. Wenn ein öffentliches Element eines Benutzer Steuer Elements geändert wird, müssen Sie alle Seiten neu kompilieren, die das Benutzer Steuerelement verwenden. ASP.net dann werden Ihre Seiten beim ersten Zugriff auf Ihre Seiten neu erstellt. Dies führt wiederum zu einer nicht skalierbaren Architektur und einem Standort Verwaltungsproblem für größere Standorte.

Beide Probleme (und vieles mehr) werden von Masterseiten in ASP.NET 2,0 gut adressiert.

## <a name="how-master-pages-work"></a>Funktionsweise von Master Seiten

Eine Master Seite ist analog zu einer Vorlage für andere Seiten. Seitenelemente, die von anderen Seiten (z. b. Menüs, Rahmen usw.) gemeinsam genutzt werden sollen, werden der Master Seite hinzugefügt. Wenn der Site neue Seiten hinzugefügt werden, können Sie Sie einer Master Seite zuordnen. Eine Seite, die einer Master Seite zugeordnet ist, wird als **Inhaltsseite**bezeichnet. Standardmäßig wird auf einer Inhaltsseite die Darstellung von der Master Seite angezeigt. Wenn Sie jedoch eine Master Seite erstellen, können Sie Teile der Seite definieren, die von der Inhaltsseite durch ihren eigenen Inhalt ersetzt werden können. Diese Teile werden mithilfe eines neuen Steuer Elements definiert, das in ASP.NET 2,0 eingeführt wurde. Das **contentplachalter** -Steuerelement.

Eine Master Seite kann eine beliebige Anzahl von contentplachalter-Steuerelementen (oder überhaupt keine) enthalten. Auf der Seite Inhalt wird der Inhalt der contentplachalter-Steuerelemente in den **Inhalts** Steuerelementen angezeigt, ein weiteres neues Steuerelement in ASP.NET 2,0. Standardmäßig sind Inhaltsseiten Inhalts Steuerelemente leer, sodass Sie Ihre eigenen Inhalte bereitstellen können. Wenn Sie den Inhalt der Master Seite in den Inhalts Steuerelementen verwenden möchten, können Sie dies wie weiter unten in diesem Modul sehen. Das Content-Steuerelement wird dem contentplachalter-Steuerelement über das ContentPlaceHolderID-Attribut des Content-Steuer Elements zugeordnet. Der folgende Code ordnet ein Inhalts Steuerelement einem contentplachalter-Steuerelement zu, das auf einer Master Seite als Mainbody bezeichnet wird.

[!code-aspx[Main](master-pages/samples/sample1.aspx)]

> [!NOTE]
> Häufig hören Sie, dass die Masterseiten als Basisklasse für andere Seiten beschrieben werden. Dies trifft tatsächlich nicht zu. Die Beziehung zwischen Masterseiten und Inhaltsseiten ist nicht eine Vererbung.

**Abbildung 1** zeigt eine Master Seite und eine zugehörige Inhaltsseite, wie Sie in Visual Studio 2005 angezeigt werden. Das contentplachalter-Steuerelement wird auf der Master Seite und das entsprechende Inhalts Steuerelement auf der Seite Inhalt angezeigt. Beachten Sie, dass der Inhalt der Masterseiten, der sich außerhalb des contentplachalter-Inhalts befindet, sichtbar ist, aber auf der Inhaltsseite abgeblendet ist. Nur der Inhalt innerhalb von contentplachalter kann von der Inhaltsseite unterdrückt werden. Alle anderen Inhalte, die von der Master Seite stammen, sind unveränderlich.

![Eine Master Seite und die zugehörige Inhaltsseite](master-pages/_static/image1.jpg)

**Abbildung 1**: eine Master Seite und die zugehörige Inhaltsseite

## <a name="creating-a-master-page"></a>Erstellen einer Master Seite

So erstellen Sie eine neue Master Seite:

1. Öffnen Sie Visual Studio 2005, und erstellen Sie eine neue Website.
2. Klicken Sie auf Datei, neu, Datei.
3. Wählen Sie im Dialogfeld Neues Element hinzufügen die Option Master Datei aus, wie in **Abbildung 2**dargestellt.
4. Klicken Sie auf Hinzufügen.

![Erstellen einer neuen Master Seite](master-pages/_static/image2.jpg)

**Abbildung 2**: Erstellen einer neuen Master Seite

Beachten Sie, dass die Dateierweiterung für eine Master Seite *. Master*ist. Dies ist eine der Methoden, mit denen sich eine Master Seite von einer normalen Seite unterscheidet. Der andere Hauptunterschied besteht darin, dass die Master Seite anstelle einer @Page-Direktive eine @Master-Direktive enthält. Wechseln Sie zur Quell Ansicht für die Master Seite, die Sie soeben erstellt haben, und überprüfen Sie den Code.

Eine neue Master Seite verfügt standardmäßig über ein contentplachalter-Steuerelement. In den meisten Fällen ist es sinnvoller, die allgemeinen Seitenelemente zuerst zu erstellen und dann contentplachalter-Steuerelemente einzufügen, wenn benutzerdefinierter Inhalt gewünscht wird. In diesen Fällen möchten Entwickler das standardmäßige contentplachalter-Steuerelement löschen und neue einfügen, wenn die Seite entwickelt wird. Die Größe von contentplachalter-Steuerelementen kann nicht geändert werden, obwohl Sie die Größen Zieh Punkte der Anzeige anzeigen. Die contentplachalter-Steuerelement Größen werden automatisch basierend auf dem Inhalt, den Sie enthält, mit einer Ausnahme angezeigt. Wenn Sie ein contentplachalter-Steuerelement innerhalb eines Block Elements, z. b. einer Tabellenzelle, platzieren, wird die Größe entsprechend der Größe des Elements angepasst.

## <a name="lab-1-working-with-master-pages"></a>Übungseinheit 1 zum Arbeiten mit Master Seiten

In dieser Übungseinheit erstellen Sie eine neue Master Seite und definieren drei contentplachalter-Steuerelemente. Anschließend erstellen Sie eine neue Inhaltsseite und ersetzen den Inhalt aus mindestens einem der contentplachalter-Steuerelemente.

1. Erstellen Sie eine Master Seite und fügen Sie contentplachalter-Steuerelemente ein. 

    1. Erstellen Sie eine neue Master Seite, wie oben beschrieben.
    2. Löschen Sie das standardmäßige contentplachalter-Steuerelement.
    3. Wählen Sie das contentplachalter-Steuerelement aus, indem Sie auf den schattierten oberen Rahmen des Steuer Elements klicken und es dann durch Drücken der ENTF-Taste auf der Tastatur löschen.
    4. Fügen Sie eine neue Tabelle mithilfe der *Header-und Seiten* Vorlage ein, wie in Abbildung 3 gezeigt. Ändern Sie Breite und Höhe in 90%, damit die gesamte Tabelle im Designer sichtbar ist.

![](master-pages/_static/image3.jpg)

**Abbildung 3**

1. Platzieren Sie den Cursor in jeder Zelle der Tabelle, und legen Sie die Eigenschaft *VAlign* auf *Top*fest.
2. Fügen Sie aus der Toolbox ein contentplachalter-Steuerelement in die obere Zelle der Tabelle ein (die Header Zelle).
3. Wenn Sie dieses contentplachalter-Steuerelement einfügen, werden Sie bemerken, dass die Zeilenhöhe fast die gesamte Seite aufnimmt, wie in Abbildung 4 dargestellt. Machen Sie sich zu diesem Zeitpunkt nicht um dieses Thema.

![Der leere Leerraum befindet sich in derselben Zelle wie der contentplachalter.](master-pages/_static/image1.gif)

**Abbildung 4**: der leere Bereich befindet sich in der gleichen Zelle wie der contentplachalter.

1. Platzieren Sie ein contentplachalter-Steuerelement in den anderen beiden Zellen. Nachdem die anderen contentplachalter-Steuerelemente eingefügt wurden, sollte die Größe der Tabellenzellen wie erwartet lauten. Die Seite sollte nun wie die in **Abbildung 5**gezeigte Seite aussehen.

![Der Master mit allen contentplachalter-Steuerelementen. Beachten Sie, dass die Zellhöhe für die Header Zelle nun der Wert ist, den Sie haben sollte.](master-pages/_static/image2.gif)

**Abbildung 5**: der Master mit allen contentplachalter-Steuerelementen. Beachten Sie, dass die Zellhöhe für die Header Zelle nun der Wert ist, den Sie haben sollte.

1. Geben Sie in jedem der drei contentplachalter-Steuerelemente einen Text Ihrer Wahl ein.
2. Speichern Sie die Master Seite als exercise1. Master.
3. Erstellen Sie ein neues Webformular, und ordnen Sie es der Master Seite exercise1. Master zu.
4. Wählen Sie Datei, neu, Datei in Visual Studio 2005 aus.
5. Wählen Sie im Dialogfeld Neues Element hinzufügen die Option **Web Form** aus.
6. Stellen Sie sicher, dass das Kontrollkästchen Master Seite auswählen wie in Abbildung 6 dargestellt aktiviert ist.

![Hinzufügen einer neuen Inhaltsseite](master-pages/_static/image3.gif)

**Abbildung 6**: Hinzufügen einer neuen Inhaltsseite

1. Klicken Sie auf Hinzufügen.
2. Wählen Sie im Dialogfeld "Master Seite auswählen" die Option exercise1. Master aus, wie in Abbildung 7 dargestellt.
3. Klicken Sie auf OK, um die neue Inhaltsseite hinzuzufügen.

Die neue Inhaltsseite wird in Visual Studio mit einem Inhalts Steuerelement für jedes contentplachalter-Steuerelement auf der Master Seite angezeigt. Standardmäßig sind die Inhalts Steuerelemente leer, sodass Sie eigene Inhalte hinzufügen können. Wenn Sie den Inhalt aus dem contentplachalter-Steuerelement auf der Master Seite verwenden möchten, klicken Sie einfach auf das Smarttagsymbol (der kleine schwarze Pfeil in der oberen rechten Ecke des Steuer Elements), und wählen Sie *Standard aus, um den Inhalt* des Smarttags zu beherrschen, wie in **Abbildung 8**dargestellt. Wenn Sie dies tun, ändert sich das Menü Element, um *benutzerdefinierten Inhalt zu erstellen*. Wenn Sie an diesem Punkt darauf klicken, wird der Inhalt von der Master Seite entfernt, sodass Sie benutzerdefinierten Inhalt für das jeweilige Inhalts Steuerelement definieren können.

![Festlegen eines Inhalts Steuer Elements zum Standardwert des Master Seiteninhalts](master-pages/_static/image4.gif)

**Abbildung 7**: Festlegen des Inhalts Steuer Elements zum Standardwert für den Master Seiten Inhalt

## <a name="connecting-master-page-and-content-pages"></a>Verbinden von Master Seiten und Inhaltsseiten

Die Zuordnung zwischen einer Master Seite und einer Inhaltsseite kann auf vier verschiedene Arten konfiguriert werden:

- Das <strong>MasterPageFile</strong> -Attribut der @Page-Direktive
- Festlegen der **Page. MasterPageFile** -Eigenschaft im Code.
- Die **&lt;Seiten&gt;** Element in der Anwendungs Konfigurationsdatei ("Web. config" im Stamm Ordner der Anwendung).
- Die **&lt;Seiten&gt;** Element in einer Unterordner-Konfigurationsdatei (Web. config in einem Unterordner).

## <a name="masterpagefile-attribute"></a>MasterPageFile-Attribut

Mit dem MasterPageFile-Attribut können Sie auf einfache Weise eine Master Seite auf eine bestimmte ASP.NET Seite anwenden. Es ist auch die Methode, die zum Anwenden der Master Seite verwendet wird, wenn Sie das Kontrollkästchen **Master Seite auswählen** wie in Übung 1 aktivieren.

## <a name="setting-pagemasterpagefile-in-code"></a>Festlegen von "page. MasterPageFile" im Code

Durch Festlegen der MasterPageFile-Eigenschaft im Code können Sie zur Laufzeit eine bestimmte Master Seite auf ihren Inhalt anwenden. Dies ist in Fällen hilfreich, in denen Sie möglicherweise eine bestimmte Master Seite auf der Grundlage einer Benutzerrolle oder anderer Kriterien anwenden müssen. Die MasterPageFile-Eigenschaft muss in der PreInit-Methode festgelegt werden. Wenn Sie nach der PreInit-Methode festgelegt wird, wird eine InvalidOperationException ausgelöst. Die Seite, auf der diese Eigenschaft festgelegt wird, muss auch über ein Inhalts Steuerelement als Steuerelement der obersten Ebene für die Seite verfügen. Andernfalls wird eine HttpException ausgelöst, wenn die MasterPageFile-Eigenschaft festgelegt wird.

## <a name="using-the-ltpagesgt-element"></a>Verwenden der &lt;Seiten&gt; Elements

Sie können eine Master Seite für Ihre Seiten konfigurieren, indem Sie das MasterPageFile-Attribut in den &lt;Seiten&gt; Element der Datei "Web. config" festlegen. Beachten Sie bei Verwendung dieser Methode, dass die in der Anwendungs Struktur niedrigeren Web. config-Dateien diese Einstellung überschreiben können. Jedes MasterPageFile-Attribut, das in einer @Page Direktive festgelegt ist, setzt diese Einstellung ebenfalls außer Kraft Wenn Sie die &lt;Seiten&gt;-Elements verwenden, ist es einfach, eine *Master* Master Seite zu erstellen, die bei Bedarf in bestimmten Ordnern oder Dateien überschrieben werden kann.

## <a name="properties-in-master-pages"></a>Eigenschaften in Master Seiten

Eine Master Seite kann Eigenschaften verfügbar machen, indem Sie diese Eigenschaften einfach innerhalb der Master Seite öffentlich machen. Der folgende Code definiert z. b. eine Eigenschaft mit dem Namen "SomeProperty":

[!code-csharp[Main](master-pages/samples/sample2.cs)]

Um auf der Inhaltsseite auf die Eigenschaft "SomeProperty" zuzugreifen, müssen Sie die Master-Eigenschaft wie folgt verwenden:

[!code-csharp[Main](master-pages/samples/sample3.cs)]

## <a name="nesting-master-pages"></a>Schachtelungs Master Seiten

Master Seiten sind die perfekte Lösung, um ein gängiges Aussehen und Gefühl in einer großen Webanwendung zu gewährleisten. Es ist jedoch nicht ungewöhnlich, dass bestimmte Teile einer großen Site eine gemeinsame Schnittstelle aufweisen, während andere Teile eine andere Schnittstelle verwenden. Um diesen Bedarf gerecht zu werden, sind mehrere Masterseiten die perfekte Lösung. Dies geht jedoch immer noch nicht darauf ein, dass eine große Anwendung möglicherweise über bestimmte Komponenten (z. b. ein Menü) verfügt, die von allen Seiten und anderen Komponenten gemeinsam genutzt werden, die nur für bestimmte Abschnitte der Site freigegeben sind. Bei dieser Art von Situation ist es für die untergeordneten Masterseiten sehr schön. Wie Sie gesehen haben, besteht eine normale Master Seite aus einer Master Seite und einer Inhaltsseite. In einer Situation mit einer Situation in einer auf einer einzelnen Master Seite befinden sich Zweimaster Seiten: ein übergeordneter Master und ein untergeordneter Master. Die untergeordnete Master Seite ist auch eine Inhaltsseite, und ihre Master Seite ist die übergeordnete Master Seite.

Dies ist der Code für eine typische Master Seite:

[!code-aspx[Main](master-pages/samples/sample4.aspx)]

In einem Szenario mit einem solchen Szenario ist dies der übergeordnete Master. Diese Seite wird von einer anderen Master Seite als Master Seite verwendet, und dieser Code würde wie folgt aussehen:

[!code-aspx[Main](master-pages/samples/sample5.aspx)]

Beachten Sie, dass der untergeordnete Master in diesem Szenario auch eine Inhaltsseite für den übergeordneten Master ist. Der gesamte Inhalt des untergeordneten Masters wird in einem Inhalts Steuerelement angezeigt, das seinen Inhalt aus dem contentplachalter-Steuerelement des übergeordneten Elements abruft.

> [!NOTE]
> Die Designer Unterstützung ist für die unterstützten Masterseiten nicht verfügbar. Wenn Sie die Entwicklung mithilfe von Netz meistern durchführen, müssen Sie die Quell Ansicht verwenden.

Dieses Video zeigt eine exemplarische Vorgehensweise zur Verwendung von Vorgängen für die Verwendung von Vorgängen

![](master-pages/_static/image1.png)

[Vollbildvideos öffnen](master-pages/_static/nested1.wmv)

![Auswählen einer Master Seite](master-pages/_static/image4.jpg)

**Abbildung 8**: Auswählen einer Master Seite
