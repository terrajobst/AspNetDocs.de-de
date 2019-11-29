---
uid: web-forms/overview/older-versions-getting-started/master-pages/nested-master-pages-vb
title: Masted Master Pages (VB) | Microsoft-Dokumentation
author: rick-anderson
description: Zeigt, wie eine Master Seite innerhalb einer anderen geschachtelt wird.
ms.author: riande
ms.date: 07/28/2008
ms.assetid: 14d9aa1b-4dca-43a0-aa9d-a6e891fee019
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/nested-master-pages-vb
msc.type: authoredcontent
ms.openlocfilehash: 9bb39712855c37f5cbcbb447f7691e9451b8dc92
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2019
ms.locfileid: "74642029"
---
# <a name="nested-master-pages-vb"></a>Geschachtelte Masterseiten (VB)

von [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Code herunterladen](https://download.microsoft.com/download/d/6/6/d66ad554-afdd-409e-a5c3-201b774fbb31/ASPNET_MasterPages_Tutorial_10_VB.zip) oder [PDF herunterladen](https://download.microsoft.com/download/d/6/6/d66ad554-afdd-409e-a5c3-201b774fbb31/ASPNET_MasterPages_Tutorial_10_VB.pdf)

> Zeigt, wie eine Master Seite innerhalb einer anderen geschachtelt wird.

## <a name="introduction"></a>Einführung

Im Verlauf der letzten neun Tutorials haben wir gesehen, wie Sie ein Standort weites Layout mit Masterseiten implementieren. Kurz gesagt: mithilfe von Masterseiten können wir, den Seiten Entwickler, ein gemeinsames Markup auf der Master Seite zusammen mit bestimmten Regionen definieren, die auf Inhaltsseiten einzeln angepasst werden können. Die contentplachalter-Steuerelemente in einer Master Seite zeigen die anpassbaren Bereiche an. Das angepasste Markup für die contentplachalter-Steuerelemente wird auf der Inhaltsseite über Inhalts Steuerelemente definiert.

Die bisher untersuchten Masterseiten Techniken sind hervorragend, wenn Sie über ein einzelnes Layout verfügen, das für die gesamte Website verwendet wird. Viele große Websites verfügen jedoch über ein Site Layout, das in verschiedenen Abschnitten angepasst ist. Stellen Sie sich z. b. eine Health Care-Anwendung vor, die vom Krankenhaus Personal zum Verwalten von Patienteninformationen, Aktivitäten und Rechnungen verwendet wird In dieser Anwendung sind möglicherweise drei Arten von Webseiten vorhanden:

- Mitarbeiter spezifische Seiten, auf denen Mitarbeiter Mitglieder die Verfügbarkeit aktualisieren, Zeitpläne anzeigen oder Urlaubszeit anfordern können.
- Patientenspezifische Seiten, auf denen Mitarbeiter Mitgliederinformationen für einen bestimmten Patienten anzeigen oder bearbeiten.
- Abrechnungs spezifische Seiten, bei denen die Buchhaltungs Prüfer aktuelle anspruchsstatus-und Finanzberichte überprüfen.

Jede Seite kann ein gemeinsames Layout haben, z. b. ein Menü oben und eine Reihe häufig verwendeter Links im unteren Bereich. Die Mitarbeiter-, Patienten-und Abrechnungs spezifischen Seiten müssen jedoch möglicherweise dieses generische Layout anpassen. Beispielsweise sollten alle Personal spezifischen Seiten eine Kalender-und Aufgabenliste enthalten, die die Verfügbarkeit des aktuell angemeldeten Benutzers und den täglichen Zeitplan anzeigt. Möglicherweise müssen alle patientenspezifischen Seiten den Namen, die Adresse und die Versicherungsinformationen für den Patienten anzeigen, dessen Informationen bearbeitet werden.

Es ist möglich, solche angepassten Layouts mithilfe von untergeordneten *Seiten*zu erstellen. Um das oben beschriebene Szenario zu implementieren, erstellen Sie zunächst eine Master Seite, die das Site weite Layout, den Menü-und den Fußzeile-Inhalt definiert, wobei Inhalts Platzhalter die anpassbaren Bereiche definieren. Anschließend erstellen wir drei nebeneinander erstellte Masterseiten, eine für jeden Typ von Webseite. Jede Seite mit der Seite "Master" definiert den Inhalt unter dem Typ der Inhaltsseiten, die die Master Seite verwenden. Mit anderen Worten: die Seite für die Seite für patientenspezifische Inhaltsseiten enthält Markup und programmgesteuerte Logik zum Anzeigen von Informationen über den zu bearbeitenden Patienten. Wenn Sie eine neue patientenspezifische Seite erstellen, binden wir Sie an diese vorgebundene Master Seite.

In diesem Tutorial werden die Vorteile der vorgerufenen Masterseiten hervorgehoben. Anschließend wird gezeigt, wie Sie die Erstellung und Verwendung von masted Masterseiten durchführen.

> [!NOTE]
> In der Version 2,0 der .NET Framework sind möglicherweise gezwischenbte Masterseiten möglich. Visual Studio 2005 enthielt jedoch keine Entwurfszeit Unterstützung für die Unterstützung von untergeordneten Seiten. Die gute Nachricht ist, dass Visual Studio 2008 eine umfangreiche Entwurfszeit-Ober Arbeit für die Verwendung von auf der Master Seite Verfügbarkeit bietet. Wenn Sie daran interessiert sind, die Verwendung von in der Liste verwendeten Masterseiten zu verwenden, aber weiterhin Visual Studio 2005 verwenden, lesen Sie den Blogbeitrag von [Scott Guthrie](https://weblogs.asp.net/scottgu/), [Tipps für gemasterte Masterseiten in VS 2005-Entwurfszeit](https://weblogs.asp.net/scottgu/archive/2005/11/11/430382.aspx).

## <a name="the-benefits-of-nested-master-pages"></a>Die Vorteile der in der Liste vorgerufenen Master Seiten

Viele Websites verfügen über einen übergreifenden Website Entwurf sowie weitere angepasste Entwürfe, die für bestimmte Seiten Typen spezifisch sind. In der Demo-Webanwendung haben wir beispielsweise einen rudimentären Verwaltungsabschnitt (die Seiten im Ordner "`~/Admin`") erstellt. Derzeit verwenden die Webseiten im Ordner "`~/Admin`" dieselbe Master Seite wie die Seiten, die sich nicht im Abschnitt "Verwaltung" befinden (`Site.master` oder `Alternate.master`, abhängig von der Auswahl des Benutzers).

> [!NOTE]
> Geben Sie vorerst an, dass unsere Website nur eine Master Seite hat, `Site.master`. In diesem Tutorial wird die Verwendung von untergeordneten Seiten mit zwei (oder mehr) Masterseiten behandelt, die mit der Verwendung einer auf einer Seite mit der Seite "eine Reihe von Dashboardseite" beginnen.

Stellen Sie sich vor, dass wir das Layout der Verwaltungs Seiten angepasst haben, um zusätzliche Informationen oder Links zu enthalten, die andernfalls nicht auf anderen Seiten der Website vorhanden wären. Es gibt vier Techniken, um diese Anforderung zu implementieren:

1. Fügen Sie die Verwaltungs spezifischen Informationen und Verknüpfungen zu jeder Inhaltsseite im Ordner "`~/Admin`" manuell hinzu.
2. Aktualisieren Sie die Seite `Site.master` Master, sodass Sie die Verwaltungs Abschnitts spezifischen Informationen und Links enthält, und fügen Sie dann der Master Seite Code hinzu, um diese Abschnitte basierend darauf anzuzeigen oder auszublenden, ob eine der Verwaltungs Seiten besucht wird.
3. Erstellen Sie eine neue Master Seite speziell für den Abschnitt Verwaltung, kopieren Sie das Markup aus `Site.master`, fügen Sie die Verwaltungs Abschnitts spezifischen Informationen und Verknüpfungen hinzu, und aktualisieren Sie dann die Inhaltsseiten im Ordner `~/Admin`, damit diese neue Master Seite verwendet werden kann.
4. Erstellen Sie eine auf der Seite mit der Seite für die Verbindung an `Site.master` und die Inhaltsseiten im Ordner "`~/Admin`" diese neue, auf der Seite verwendete Master Seite. Diese vordefinierte Master Seite enthält nur die zusätzlichen Informationen und Verknüpfungen, die spezifisch für die Verwaltungs Seiten sind. Sie müssen das bereits in `Site.master`definierte Markup nicht wiederholen.

Die erste Option ist die am wenigsten schmackbare. Der Hauptpunkt der Verwendung von Masterseiten besteht darin, das manuelle Kopieren und Einfügen von allgemeinen Markup Daten auf neue ASP.NET Seiten zu vermeiden. Die zweite Option ist akzeptabel, bewirkt aber, dass die Anwendung weniger wartungsfähig ist, da Sie die Masterseiten mit Markup aufführt, das nur gelegentlich angezeigt wird, und dass Entwickler die Master Seite bearbeiten müssen, um dieses Markup zu umgehen. genau, wird ein bestimmtes Markup angezeigt, wenn es ausgeblendet ist. Diese Vorgehensweise ist weniger anpassbar als Anpassungen von mehr und mehr Webseiten, die von dieser einzelnen Master Seite untergebracht werden müssen.

Mit der dritten Option werden die Übersichtlichkeit und die Komplexitäts Probleme mit der zweiten Option entfernt. Der Hauptnachteil von Option 3 besteht jedoch darin, dass es erforderlich ist, das gemeinsame Layout von `Site.master` in die neue Verwaltungs Abschnitts spezifische Master Seite zu kopieren und einzufügen. Wenn Sie sich später entscheiden, das standortweite Layout zu ändern, müssen Sie daran denken, es an zwei Stellen zu ändern.

Mit der vierten Option, den Geviert enden Seiten, können wir das Beste aus den zweiten und dritten Optionen machen. Die Standort weiten Layoutinformationen werden in einer Datei verwaltet: der Master Seite auf oberster Ebene, während der für bestimmte Regionen spezifische Inhalt in verschiedene Dateien aufgeteilt wird.

Dieses Tutorial beginnt mit einem Blick auf das Erstellen und Verwenden einer einfachen, auf einer Seite eingefügten Master Seite. Wir erstellen eine neue, neue Master Seite auf oberster Ebene, zwei auf der Seite vorgenannte Masterseiten und zwei Inhaltsseiten. Beginnend mit der Seite "using a netsted Master page for the Administration Section" (über eine Seite mit einer Liste der Dashboardseite für den Verwaltungsabschnitt) wird beschrieben, wie Sie die vorhandene Masterseiten Architektur aktualisieren, um Insbesondere erstellen wir eine Seite für eine Seite mit einer Liste und verwenden diese, um zusätzliche benutzerdefinierte Inhalte für die Inhaltsseiten im Ordner "`~/Admin`" einzuschließen.

## <a name="step-1-creating-a-simple-top-level-master-page"></a>Schritt 1: Erstellen einer einfachen Master Seite auf oberster Ebene

Wenn Sie einen auf einer der vorhandenen Masterseiten basierenden, auf einer der obersten Ebene verwendeten Masterseiten erstellen und dann eine vorhandene Inhaltsseite aktualisieren, um diese neue, nicht für die Master Seite der obersten Ebene zu verwenden, wird eine gewisse Komplexität verursacht, da die vorhandenen Inhaltsseiten bereits einen bestimmten Wert erwarten. Contentplachalter-Steuerelemente, die auf der Master Seite der obersten Ebene definiert sind. Aus diesem Grund muss die Seite für die untergeordneten Master Seite auch dieselben contentplachalter-Steuerelemente mit denselben Namen enthalten. Außerdem verfügt unsere besondere Demoanwendung über Zweimaster Seiten (`Site.master` und `Alternate.master`), die einer Inhaltsseite basierend auf den Einstellungen eines Benutzers dynamisch zugewiesen werden, was diese Komplexität weiter erhöht. Wir sehen uns nun an, wie Sie die vorhandene Anwendung für die Verwendung von untergeordneten Masterseiten später in diesem Tutorial aktualisieren, aber wir konzentrieren uns zunächst auf ein einfaches Beispiel für ein Beispiel für eine Beispiel-Master Seite.

Erstellen Sie einen neuen Ordner mit dem Namen `NestedMasterPages`, und fügen Sie diesem Ordner mit dem Namen `Simple.master`eine neue Masterseiten Datei hinzu. (In Abbildung 1 finden Sie einen Screenshot der Projektmappen-Explorer, nachdem dieser Ordner und die Datei hinzugefügt wurden.) Ziehen Sie die `AlternateStyles.css` Stylesheet-Datei aus dem Projektmappen-Explorer auf den Designer. Dadurch wird der Stylesheet-Datei im `<head>`-Element ein `<link>` Element hinzugefügt. Anschließend sollte das Markup der `<head>` Elemente der Master Seite wie folgt aussehen:

[!code-aspx[Main](nested-master-pages-vb/samples/sample1.aspx)]

Fügen Sie als nächstes das folgende Markup im Webformular `Simple.master`hinzu:

[!code-aspx[Main](nested-master-pages-vb/samples/sample2.aspx)]

Dieses Markup zeigt einen Link mit dem Titel "verknüpfte Master Seiten (Simple)" am oberen Rand der Seite in einer großen weißen Schriftart in einem Navy-Hintergrund an. Unterhalb dieses-`MainContent` contentplachalter. Abbildung 1 zeigt die `Simple.master` Master Seite, wenn Sie im Visual Studio-Designer geladen wird.

[![die Seite "auf der Seite" auf der Seite "Seite" definiert Inhalte definiert, die für die Seiten im](nested-master-pages-vb/_static/image2.png)](nested-master-pages-vb/_static/image1.png)

**Abbildung 01**: die Seite "vordefiniert" definiert Inhalte, die für die Seiten im Abschnitt "Verwaltung" spezifisch sind ([Klicken Sie, um das Bild in voller Größe anzuzeigen](nested-master-pages-vb/_static/image3.png))

## <a name="step-2-creating-a-simple-nested-master-page"></a>Schritt 2: Erstellen einer einfachen, auf einer Seite eingefügten Master Seite

`Simple.master` enthält zwei contentplachalter-Steuerelemente: den `MainContent` contentplachalter, den wir innerhalb des Webformulars hinzugefügt haben, zusammen mit dem `head` contentplachalter im `<head>` Element. Wenn eine Inhaltsseite erstellt und an `Simple.master` gebunden werden soll, würde die Inhaltsseite zwei Inhalts Steuerelemente enthalten, die auf die beiden Content-Platzhalter verweisen. Ebenso gilt, wenn wir eine Seite mit einer auf einer Seite erstellten Master Seite erstellen und diese an `Simple.master` binden.

Fügen wir dem Ordner "`NestedMasterPages`" eine neue Seite mit dem Namen "`SimpleNested.master`" hinzu. Klicken Sie mit der rechten Maustaste auf den Ordner `NestedMasterPages`, und wählen Sie neues Element hinzufügen. Dadurch wird das Dialogfeld Neues Element hinzufügen in Abbildung 2 angezeigt. Wählen Sie den Vorlagentyp der Master Seite aus, und geben Sie den Namen der neuen Master Seite ein. Aktivieren Sie das Kontrollkästchen "Master Seite auswählen", um anzugeben, dass es sich bei der neuen Master Seite um eine Seite mit der Master Seite handelt.

Klicken Sie anschließend auf die Schaltfläche hinzufügen. Dadurch wird das gleiche Dialogfeld "Master Seite auswählen" angezeigt, das beim Binden einer Inhaltsseite an eine Master Seite angezeigt wird (siehe Abbildung 3). Wählen Sie im Ordner `NestedMasterPages` die `Simple.master` Master Seite aus, und klicken Sie auf OK.

> [!NOTE]
> Wenn Sie Ihre ASP.NET-Website mit dem Webanwendungs Projekt Modell anstelle des Website Projekt Modells erstellt haben, wird im Dialogfeld Neues Element hinzufügen in Abbildung 2 das Kontrollkästchen "Master Seite auswählen" nicht angezeigt. Wenn Sie bei Verwendung des Webanwendungs Projekt Modells eine Seite mit einer untergeordneten Master Seite erstellen möchten, müssen Sie die Vorlage für die Vorlage für die Vorlage für die Vorlage (anstelle der Vorlage für die Master Seite Nachdem Sie die Vorlage für die Vorlage für die Vorlage und das Klicken auf hinzufügen ausgewählt haben, wird das gleiche in Abbildung 3 dargestellte Dialogfeld "Master Seite auswählen" angezeigt.

[![aktivieren Sie das Kontrollkästchen &quot;Master Seite auswählen&quot;, um eine Liste der untergeordneten Masterseiten hinzuzufügen.](nested-master-pages-vb/_static/image5.png)](nested-master-pages-vb/_static/image4.png)

**Abbildung 02**: Aktivieren Sie das Kontrollkästchen "Master Seite auswählen", um eine voreingesterte Master Seite hinzuzufügen ([Klicken Sie, um das Bild in voller Größe anzuzeigen](nested-master-pages-vb/_static/image6.png)).

[Binden Sie die Seite für die Seite "" auf der Seite "Simple. Master" ![.](nested-master-pages-vb/_static/image8.png)](nested-master-pages-vb/_static/image7.png)

**Abbildung 03**: Binden der Seite für die vorgebundene Master Seite an die `Simple.master` Master Seite ([Klicken Sie, um das Bild in voller Größe anzuzeigen](nested-master-pages-vb/_static/image9.png))

Das deklarative Markup der der dargestellten Master Seite, wie unten gezeigt, enthält zwei Inhalts Steuerelemente, die auf die beiden contentplachalter-Steuerelemente der obersten Ebene verweisen.

[!code-aspx[Main](nested-master-pages-vb/samples/sample3.aspx)]

Mit Ausnahme der `<%@ Master %>`-Direktive ist das anfängliche deklarative Markup der gemasterten Master Seite mit dem Markup identisch, das anfänglich generiert wird, wenn eine Inhaltsseite an dieselbe Master Seite auf oberster Ebene gebunden wird. Wie die `<%@ Page %>`-Direktive einer Inhaltsseite enthält die `<%@ Master %>`-Direktive hier ein `MasterPageFile`-Attribut, das die übergeordnete Master Seite der Seite der Seite angibt. Der Hauptunterschied zwischen der Seite für die untergeordneten Master Seite und einer Inhaltsseite, die an dieselbe Master Seite auf oberster Ebene gebunden ist, besteht darin, dass die Seite für die vorgebundene Master Seite contentplac-Steuerelemente enthalten kann Die contentplachalter-Steuerelemente der untergeordneten Seite definieren die Bereiche, in denen die Inhaltsseiten das Markup anpassen können.

Aktualisieren Sie diese geschachtelte Master Seite, sodass Sie den Text "Hello, from simplentzig!" anzeigt. im Inhalts Steuerelement, das dem `MainContent` contentplachalter-Steuerelement entspricht.

[!code-aspx[Main](nested-master-pages-vb/samples/sample4.aspx)]

Nachdem Sie dieses Add-in hinzugefügt haben, speichern Sie die Seite für die vorgebundene Master Seite, und fügen Sie dann eine neue Inhaltsseite zum Ordner "`NestedMasterPages`" mit `SimpleNested.master` dem Namen `Default.aspx`hinzu Wenn Sie diese Seite hinzufügen, sind Sie möglicherweise überrascht, dass Sie keine Inhalts Steuerelemente enthält (siehe Abbildung 4). Auf einer Inhaltsseite kann nur auf die contentplatzhalter der über *geordneten* Master Seite zugegriffen werden. `SimpleNested.master` enthält keine contentplachalter-Steuerelemente. Daher können alle an diese Master Seite gebundenen Inhaltsseiten keine Inhalts Steuerelemente enthalten.

[![die neue Inhaltsseite keine Inhalts Steuerelemente enthält.](nested-master-pages-vb/_static/image11.png)](nested-master-pages-vb/_static/image10.png)

**Abbildung 04**: die neue Inhaltsseite enthält keine Inhalts Steuerelemente ([Klicken Sie, um das Bild in voller Größe anzuzeigen](nested-master-pages-vb/_static/image12.png))

Zu diesem Zweck müssen Sie die Seite für die untergeordneten Master Seite (`SimpleNested.master`) aktualisieren, um contentplachalter-Steuerelemente einzuschließen. In der Regel möchten Sie, dass ihre untergeordneten Masterseiten für jeden contentplachalter, der durch die übergeordnete Master Seite definiert ist, einen contentplachalter enthalten. Dadurch kann die untergeordnete Master Seite oder Inhaltsseite mit jedem der Inhalts Platzhalter der obersten Ebene der Master Seite verwendet werden. Steuerelemente.

Aktualisieren Sie die `SimpleNested.master` Master Seite, um einen contentplachalter in den beiden Inhalts Steuerelementen hinzufügen. Erteilen Sie dem contentplachalter-Steuerelement denselben Namen wie das contentplachalter-Steuerelement, auf das sich das Inhalts Steuerelement bezieht. Fügen Sie dem Content-Steuerelement in `SimpleNested.master`, das auf den `MainContent` contentplachalter in `Simple.master`verweist, ein contentplachalter-Steuerelement mit dem Namen `MainContent` hinzu. Gehen Sie im Inhalts Steuerelement, das auf die `head` contentplachalter verweist, genauso vor.

> [!NOTE]
> Es empfiehlt sich, die contentplachalter-Steuerelemente auf der Seite für die untergeordneten Master Seite genauso zu benennen wie die contentplatzhalter auf der Master Seite der obersten Ebene. diese Benennungs Symmetrie ist jedoch nicht erforderlich. Sie können die contentplachalter-Steuerelemente auf der Seite für die untergeordneten Elemente beliebig benennen. Es ist jedoch leichter zu merken, welche Content-Platzhalter welchen Bereichen der Seite entsprechen, wenn meine Master Seite auf oberster Ebene und die untergeordneten Masterseiten dieselben Namen verwenden.

Nachdem Sie diese Ergänzungen vorgenommen haben, sollte das deklarative Markup der `SimpleNested.master` Master Seite in etwa wie folgt aussehen:

[!code-aspx[Main](nested-master-pages-vb/samples/sample5.aspx)]

Löschen Sie die Seite `Default.aspx` Inhalt, die wir soeben erstellt haben, und fügen Sie Sie erneut hinzu, um Sie an die `SimpleNested.master` Master Seite zu binden. Dieses Mal fügt Visual Studio dem `Default.aspx`zwei Inhalts Steuerelemente hinzu, die auf die in `SimpleNested.master` definierten contentplatzhalter verweisen (siehe Abbildung 6). Fügen Sie den Text "Hello, from default. aspx!" hinzu. im Inhalts Steuerelement, auf das `MainContent`verwiesen wird.

Abbildung 5 zeigt die hier beteiligten drei Entitäten-`Simple.master`, `SimpleNested.master`und `Default.aspx` und die Beziehung zueinander. Wie das Diagramm zeigt, implementiert die Seite für die untergeordnete Master Seite Inhalts Steuerelemente für den contentplachalter ihres übergeordneten Elements. Wenn auf diese Regionen für die Inhaltsseite zugegriffen werden muss, muss die Seite "auf der Seite" der Seite "" der Inhalts Steuerung eigene Content-Platzhalter hinzufügen.

[das Layout der Inhaltsseite wird ![auf der obersten Ebene und in der Liste der übergeordneten Master Seiten vorgegeben.](nested-master-pages-vb/_static/image14.png)](nested-master-pages-vb/_static/image13.png)

**Abbildung 05**: das Layout der Inhaltsseite wird auf der obersten Ebene und in der Liste der übergeordneten Master Seiten vorgegeben ([Klicken Sie, um das Bild in voller Größe anzuzeigen](nested-master-pages-vb/_static/image15.png))

Dieses Verhalten veranschaulicht, wie eine Inhaltsseite oder eine Master Seite nur von der übergeordneten Master Seite erkannt wird. Dieses Verhalten wird auch durch den Visual Studio-Designer angezeigt. Abbildung 6 zeigt den Designer für `Default.aspx`. Während der Designer eindeutig anzeigt, welche Bereiche von der Inhaltsseite bearbeitet werden können und welche Teile nicht sind, wird nicht unterschieden, welche nicht bearbeitbaren Bereiche von der gisted Master Seite stammen und welche Regionen von der Master Seite auf oberster Ebene stammen.

[![die Inhaltsseite nun Inhalts Steuerelemente für die Content-Platzhalter der auf der Seite der nsted Master Seite enthält](nested-master-pages-vb/_static/image17.png)](nested-master-pages-vb/_static/image16.png)

**Abbildung 06**: auf der Inhaltsseite sind nun Inhalts Steuerelemente für die contentplatzhalter der Seite der Hauptseite enthalten ([Klicken Sie, um das Bild in voller Größe anzuzeigen](nested-master-pages-vb/_static/image18.png))

## <a name="step-3-adding-a-second-simple-nested-master-page"></a>Schritt 3: Hinzufügen einer zweiten einfachen, auf der Seite eingefügten Seite

Der Vorteil von untergeordneten Masterseiten ist deutlicher, wenn es mehrere unterschiedliche Masterseiten gibt. Um diesen Vorteil zu veranschaulichen, erstellen Sie im Ordner "`NestedMasterPages`" eine weitere Seite mit einer anderen Seite. Benennen Sie diese neue, `SimpleNestedAlternate.master` Seite, und binden Sie Sie an die `Simple.master` Master Seite. Fügen Sie die contentplachalter-Steuerelemente in den zwei Inhalts Steuerelementen der nten Master Seite hinzu, wie in Schritt 2 gezeigt. Fügen Sie außerdem den Text "Hello, from simplenestedalternate!" hinzu. im Inhalts Steuerelement, das der `MainContent` contentplachalter der obersten Ebene der Master Seite entspricht. Nachdem Sie diese Änderungen vorgenommen haben, sollte das deklarative Markup der neuen Seite der neuen Master Seite in etwa wie folgt aussehen:

[!code-aspx[Main](nested-master-pages-vb/samples/sample6.aspx)]

Erstellen Sie im Ordner "`NestedMasterPages`" eine Inhaltsseite mit dem Namen `Alternate.aspx`, und binden Sie Sie an die `SimpleNestedAlternate.master` die Seite "Master". Fügen Sie den Text "Hello, from Alternate!" hinzu. im Inhalts Steuerelement, das `MainContent`entspricht. Abbildung 7 zeigt `Alternate.aspx`, wenn Sie im Visual Studio-Designer angezeigt werden.

[![Alternate. aspx ist an die "simplenestedalternate. Master"-Master Seite gebunden.](nested-master-pages-vb/_static/image20.png)](nested-master-pages-vb/_static/image19.png)

**Abbildung 07**: `Alternate.aspx` ist an die `SimpleNestedAlternate.master` Master Seite gebunden ([Klicken Sie, um das Bild in voller Größe anzuzeigen](nested-master-pages-vb/_static/image21.png))

Vergleichen Sie den Designer in Abbildung 7 mit dem Designer in Abbildung 6. Beide Inhaltsseiten verwenden dasselbe Layout, das auf der Master Seite der obersten Ebene (`Simple.master`) definiert ist, nämlich den Titel "eingefügter Lernprogramm für Masterseiten (Simple)". In den übergeordneten Masterseiten haben beide jedoch unterschiedliche Inhalte definiert: der Text "Hello, from simplentzig!" in Abbildung 6 und "Hello, from simplenestedalternate!" in Abbildung 7. Diese Unterschiede sind in diesem Beispiel trivial, aber Sie können dieses Beispiel erweitern, um aussagekräftigere Unterschiede zu berücksichtigen. Beispielsweise kann die Seite `SimpleNested.master` ein Menü mit spezifischen Optionen für Ihre Inhaltsseiten enthalten, während `SimpleNestedAlternate.master` möglicherweise Informationen zu den Inhaltsseiten enthält, die an Sie gebunden sind.

Stellen Sie sich nun vor, dass wir eine Änderung an dem übergeordneten Site Layout vornehmen müssen. Angenommen, Sie möchten eine Liste gemeinsamer Links zu allen Inhaltsseiten hinzufügen. Um dies zu erreichen, aktualisieren wir die Master Seite auf oberster Ebene, `Simple.master`. Alle Änderungen, die in den gespiegelten Masterseiten und durch Erweiterung ihrer Inhaltsseiten sofort wiedergegeben werden.

Um die Benutzerfreundlichkeit zu veranschaulichen, mit der wir das übergeordnete Standort Layout ändern können, öffnen Sie die Seite `Simple.master` Master, und fügen Sie das folgende Markup zwischen den Elementen `topContent` und `mainContent` `<div>` hinzu:

[!code-aspx[Main](nested-master-pages-vb/samples/sample7.aspx)]

Dadurch werden zwei Links zum Anfang jeder Seite hinzugefügt, die an `Simple.master`, `SimpleNested.master`oder `SimpleNestedAlternate.master`gebunden werden. Diese Änderungen gelten sofort für alle untergeordneten Seiten und ihre Inhaltsseiten. Abbildung 8 zeigt `Alternate.aspx`, wenn Sie in einem Browser angezeigt werden. Beachten Sie das Hinzufügen der Links oben auf der Seite (im Vergleich zu Abbildung 7).

[![, die auf die Master Seite auf oberster Ebene geändert wurden, werden sofort in den von Ihnen gespiegelten Masterseiten und ihren Inhaltsseiten reflektiert.](nested-master-pages-vb/_static/image23.png)](nested-master-pages-vb/_static/image22.png)

**Abbildung 08**: Änderungen an der Master Seite auf oberster Ebene werden sofort in den geänderten Masterseiten und ihren Inhaltsseiten widergespiegelt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](nested-master-pages-vb/_static/image24.png))

## <a name="using-a-nested-master-page-for-the-administration-section"></a>Verwenden einer im Abschnitt "Verwaltung" verwendeten Seite für die Verwaltung

An dieser Stelle haben wir uns mit den Vorteilen der geschlagenen Masterseiten beschäftigt und gesehen, wie Sie in einer ASP.NET-Anwendung erstellt und verwendet werden können. Die Beispiele in den Schritten 1, 2 und 3 beinhalteten jedoch das Erstellen einer neuen Master Seite auf oberster Ebene, neuer, von der Liste erstellter Masterseiten und neuer Inhaltsseiten. Was ist, wenn einer Website eine neue, auf der obersten Ebene bestehende Master Seite und Inhaltsseiten hinzugefügt werden?

Das Integrieren einer auf einer vorhandenen Website bestehenden Website in eine vorhandene Website und die Zuordnung zu vorhandenen Inhaltsseiten erfordert etwas mehr Aufwand als von Grund auf neu zu beginnen. In den Schritten 4, 5, 6 und 7 werden diese Herausforderungen erläutert, da wir unsere Demoanwendung erweitern, um eine neue, auf der Seite mit dem Namen `AdminNested.master`, die Anweisungen für den Administrator enthält und die von den ASP.NET-Seiten im Ordner `~/Admin` verwendet wird.

Durch die Integration einer voreingefallenen Master Seite in unsere Demoanwendung werden folgende Hürden eingeführt:

- Die vorhandenen Inhaltsseiten im `~/Admin` Ordner weisen bestimmte Erwartungen von der Master Seite auf. Zunächst erwarten Sie, dass bestimmte contentplachalter-Steuerelemente vorhanden sind. Außerdem wird auf den Seiten "`~/Admin/AddProduct.aspx`" und "`~/Admin/Products.aspx`" die öffentliche `RefreshRecentProductsGrid`-Methode der Master Seite aufgerufen, deren `GridMessageText`-Eigenschaft festgelegt oder ein Ereignishandler für das `PricesDoubled` Ereignis festgelegt. Folglich muss unsere auf der Seite mit der Seite für die Master Seite dieselben contentplatzhalter und öffentlichen Member bereitstellen.
- Im vorherigen Tutorial haben wir die `BasePage` Klasse verbessert, um die `MasterPageFile`-Eigenschaft des `Page` Objekts basierend auf einer Sitzungsvariablen dynamisch festzulegen. Wie werden dynamische Masterseiten unterstützt, wenn Sie mit der Verwendung von untergeordneten Masterseiten?

Diese beiden Herausforderungen stellen eine Oberfläche dar, wenn wir die Seite für die untergeordneten Master Seite erstellen und auf unseren vorhandenen Inhaltsseiten verwenden. Diese Probleme werden untersucht und aufgetreten.

## <a name="step-4-creating-the-nested-master-page"></a>Schritt 4: Erstellen der eingefügten Master Seite

Unsere erste Aufgabe besteht darin, die von den Seiten im Abschnitt "Verwaltung" zu verwendende Seite für die vorseitige Master Seite zu erstellen. Wie in Schritt 2 gezeigt, müssen Sie beim Hinzufügen einer neuen, auf einer neuen Seite beschriebenen Master Seite die übergeordnete Master Seite der Seite der Seite angeben. Wir haben jedoch Zweimaster Seiten auf oberster Ebene: `Site.master` und `Alternate.master`. Wir erinnern uns, dass wir im vorherigen Tutorial `Alternate.master` erstellt und Code in der `BasePage`-Klasse geschrieben haben, die die `MasterPageFile`-Eigenschaft des `Page`-Objekts zur Laufzeit entweder auf `Site.master` oder `Alternate.master`, abhängig vom Wert der `MyMasterPage` Sitzungsvariablen, festgelegt haben.

Wie konfigurieren wir unsere geschachtelte Master Seite so, dass Sie die entsprechende Master Seite auf oberster Ebene verwendet? Wir haben zwei Möglichkeiten:

- Erstellen Sie zwei vorgebundene Masterseiten, `AdminNestedSite.master` und `AdminNestedAlternate.master`, und binden Sie diese an die Masterseiten der obersten Ebene `Site.master` bzw. `Alternate.master`. In `BasePage`haben wir die `MasterPageFile` des `Page` Objekts auf die entsprechende geschachtelte Master Seite festgelegt.
- Erstellen Sie eine einzelne, für die Seite verwendete Master Seite, und lassen Sie die Inhaltsseiten diese bestimmte Master Seite verwenden. Zur Laufzeit müssten wir die `MasterPageFile`-Eigenschaft der geschachtelte-Master Seite zur Laufzeit auf die entsprechende Master Seite auf oberster Ebene festlegen. (Wie Sie vielleicht schon einmal herausgefunden haben, verfügen Masterseiten auch über eine `MasterPageFile`-Eigenschaft.)

Verwenden Sie die zweite Option. Erstellen Sie eine einzelne Auslagerungs Datei im Ordner "`~/Admin`" mit dem Namen `AdminNested.master`. Da sowohl `Site.master` als auch `Alternate.master` denselben Satz an contentplachalter-Steuerelementen aufweisen, ist es egal, an welche Master Seite Sie gebunden sind. Ich empfehle Ihnen jedoch, Sie für die Konsistenz an `Site.master` zu binden.

[![fügen Sie dem Ordner ~/Admin eine "masted Master"-Seite hinzu.](nested-master-pages-vb/_static/image26.png)](nested-master-pages-vb/_static/image25.png)

**Abbildung 09**: Hinzufügen einer masted Master Seite zum Ordner "`~/Admin`". ([Klicken Sie, um das Bild in voller Größe anzuzeigen](nested-master-pages-vb/_static/image27.png))

Da die geöffnette Master Seite an eine Master Seite mit vier contentplachalter-Steuerelementen gebunden ist, fügt Visual Studio dem ursprünglichen Markup der neuen Datei der Geviert ten Master Seite vier Inhalts Steuerelemente hinzu. Fügen Sie wie in den Schritten 2 und 3 ein contentplachalter-Steuerelement in jedem Inhalts Steuerelement hinzu, und geben Sie dabei den gleichen Namen wie das contentplachalter-Steuerelement der obersten Ebene der Master Seite an. Fügen Sie dem Content-Steuerelement, das dem `MainContent` contentplachalter entspricht, auch das folgende Markup hinzu:

[!code-html[Main](nested-master-pages-vb/samples/sample8.html)]

Definieren Sie als nächstes die `instructions` CSS-Klasse in den `Styles.css`-und `AlternateStyles.css`-CSS-Dateien. Die folgenden CSS-Regeln bewirken, dass HTML-Elemente, die mit der `instructions`-Klasse formatiert sind, mit einer hellen gelben Hintergrundfarbe und einem schwarzen, durch festen Rahmen angezeigt werden:

[!code-css[Main](nested-master-pages-vb/samples/sample9.css)]

Da dieses Markup der Seite der gemasterten Master Seite hinzugefügt wurde, wird es nur auf den Seiten angezeigt, die diese gemasterte Master Seite verwenden (also auf den Seiten im Abschnitt Verwaltung).

Nachdem Sie diese Ergänzungen auf der Seite für die vorgenommenen Änderungen vorgenommen haben, sollte Ihr deklaratives Markup in etwa wie folgt aussehen:

[!code-aspx[Main](nested-master-pages-vb/samples/sample10.aspx)]

Beachten Sie, dass jedes Inhalts Steuerelement über ein contentplachalter-Steuerelement verfügt und dass den `ID` Eigenschaften der contentplachalter-Steuerelemente die gleichen Werte wie die entsprechenden contentplachalter-Steuerelemente auf der Master Seite der obersten Ebene zugewiesen werden. Darüber hinaus wird das Verwaltungs Abschnitts spezifische Markup im `MainContent` contentplachalter angezeigt.

Abbildung 10 zeigt die `AdminNested.master` die Seite für die untergeordneten Master Seite, wenn Sie im Visual Studio-Designer angezeigt wird. Die Anweisungen werden im gelben Feld am oberen Rand des `MainContent` Content-Steuer Elements angezeigt.

[![die Seite für die vorgenannte Master Seite erweitert die Master Seite auf oberster Ebene, sodass Sie Anweisungen für den Administrator enthält.](nested-master-pages-vb/_static/image29.png)](nested-master-pages-vb/_static/image28.png)

**Abbildung 10**: die vorgenannte Master Seite erweitert die Master Seite auf oberster Ebene, um Anweisungen für den Administrator einzuschließen. ([Klicken Sie, um das Bild in voller Größe anzuzeigen](nested-master-pages-vb/_static/image30.png))

## <a name="step-5-updating-the-existing-content-pages-to-use-the-new-nested-master-page"></a>Schritt 5: Aktualisieren der vorhandenen Inhaltsseiten für die Verwendung der neuen, auf der Seite verwendeten Master Seite

Jedes Mal, wenn wir dem Bereich "Verwaltung" eine neue Inhaltsseite hinzufügen, müssen wir diese an die soeben erstellte `AdminNested.master` Master Seite binden. Aber wie sieht es mit den vorhandenen Inhaltsseiten aus? Derzeit werden alle Inhaltsseiten der Website von der `BasePage`-Klasse abgeleitet, die die Master Seite der Inhaltsseite zur Laufzeit Programm gesteuert festlegt. Dies ist nicht das gewünschte Verhalten für die Inhaltsseiten im Abschnitt "Verwaltung". Stattdessen möchten wir, dass diese Inhaltsseiten immer die `AdminNested.master` Seite verwenden. Es liegt in der Verantwortung der Seite "auf der Seite" auf der Seite "Seite", die die richtige Inhaltsseite auf oberster Ebene zur Laufzeit auszuwählen.

Um dieses gewünschte Verhalten am besten zu erreichen, erstellen Sie eine neue benutzerdefinierte Basis Seiten Klasse mit dem Namen `AdminBasePage`, die die `BasePage`-Klasse erweitert. `AdminBasePage` können dann die `SetMasterPageFile` überschreiben und die `MasterPageFile` des `Page` Objekts auf den hart codierten Wert "~/admin/AdminNested.Master" festlegen. Auf diese Weise verwendet jede Seite, die von `AdminBasePage` abgeleitet wird, `AdminNested.master`, während jede Seite, die von `BasePage` abgeleitet wird, die `MasterPageFile`-Eigenschaft dynamisch auf "~/Site.Master" oder "~/Alternate.Master" basierend auf dem Wert der `MyMasterPage` Sitzungsvariablen festgelegt wird.

Fügen Sie zunächst eine neue Klassendatei zum `App_Code` Ordner mit dem Namen `AdminBasePage.vb`hinzu. `AdminBasePage` `BasePage` erweitern und dann die `SetMasterPageFile`-Methode überschreiben. Weisen Sie in dieser Methode den `MasterPageFile` den Wert "~/admin/AdminNested.Master" zu. Nachdem Sie diese Änderungen vorgenommen haben, sollte die Klassendatei in etwa wie folgt aussehen:

[!code-vb[Main](nested-master-pages-vb/samples/sample11.vb)]

Nun müssen die vorhandenen Inhaltsseiten im Verwaltungsabschnitt von `AdminBasePage` abgeleitet werden, anstatt `BasePage`. Wechseln Sie für jede Inhaltsseite im Ordner "`~/Admin`" zur Code Behind-Klassendatei, und nehmen Sie diese Änderung vor. In `~/Admin/Default.aspx` Sie z. b. die Code-Behind-Klassen Deklaration von ändern:

[!code-vb[Main](nested-master-pages-vb/samples/sample12.vb)]

Bis:

[!code-vb[Main](nested-master-pages-vb/samples/sample13.vb)]

In Abbildung 11 ist dargestellt, wie die Master Seite auf oberster Ebene (`Site.master` oder `Alternate.master`), die Seite für die Zusammenstellung der Seite (`AdminNested.master`) und die Inhaltsseiten des Verwaltungs Abschnitts aufeinander zueinander stehen.

[![die Seite "auf der Seite" auf der Seite "Seite" definiert Inhalte definiert, die für die Seiten im](nested-master-pages-vb/_static/image32.png)](nested-master-pages-vb/_static/image31.png)

**Abbildung 11**: die Seite "vordefiniert" definiert Inhalte, die für die Seiten im Abschnitt "Verwaltung" spezifisch sind ([Klicken Sie, um das Bild in voller Größe anzuzeigen](nested-master-pages-vb/_static/image33.png))

## <a name="step-6-mirroring-the-master-pages-public-methods-and-properties"></a>Schritt 6: Spiegeln der öffentlichen Methoden und Eigenschaften der Master Seite

Beachten Sie, dass die `~/Admin/AddProduct.aspx`-und `~/Admin/Products.aspx` Seiten Programm gesteuert mit der Master Seite interagieren: `~/Admin/AddProduct.aspx` die öffentliche `RefreshRecentProductsGrid`-Methode der Master Seite aufruft und deren `GridMessageText`-Eigenschaft festlegt. `~/Admin/Products.aspx` über einen Ereignishandler für das `PricesDoubled`-Ereignis verfügt. Im vorherigen Tutorial haben wir eine `MustInherit` `BaseMasterPage` Klasse erstellt, die diese öffentlichen Member definiert.

Auf den Seiten `~/Admin/AddProduct.aspx` und `~/Admin/Products.aspx` wird davon ausgegangen, dass die Master Seite von der `BaseMasterPage`-Klasse abgeleitet ist. Die `AdminNested.master` Seite erweitert jedoch derzeit die `System.Web.UI.MasterPage` Klasse. Daher wird beim Besuch `~/Admin/Products.aspx` eine `InvalidCastException` mit der folgenden Meldung ausgelöst: "das Objekt des Typs" ASP. admin\_adminnested\_Master "kann nicht in den Typ" basemasterpage "umgewandelt werden.

Um dieses Problem zu beheben, muss die `AdminNested.master` Code Behind-Klasse `BaseMasterPage`erweitert werden. Aktualisieren Sie die Code-Behind-Klassen Deklaration der blten Master Seite von:

[!code-vb[Main](nested-master-pages-vb/samples/sample14.vb)]

Bis:

[!code-vb[Main](nested-master-pages-vb/samples/sample15.vb)]

Wir sind noch nicht fertig. Wir müssen die als `MustOverride`markierten Member außer Kraft setzen, nämlich `RefreshRecentProductsGrid` und `GridMessageText`. Diese Member werden von den Masterseiten der obersten Ebene verwendet, um Ihre Benutzeroberflächen zu aktualisieren. (Nur die `Site.master` Master Seite verwendet diese Methoden, obwohl beide Masterseiten der obersten Ebene diese Methoden implementieren, da beide `BaseMasterPage`erweitern.)

Obwohl diese Member in `AdminNested.master`implementiert werden müssen, müssen all diese Implementierungen einfach denselben Member auf der obersten Ebene der Master Seite aufzurufen, die von der schsted Master Seite verwendet wird. Wenn z. b. eine Inhaltsseite im Verwaltungsabschnitt die `RefreshRecentProductsGrid` Methode der schten Master Seite aufruft, muss die gesamte schalte Master Seite wiederum `Site.master` oder `Alternate.master``RefreshRecentProductsGrid` Methode aufrufen.

Um dies zu erreichen, fügen Sie zunächst die folgende `@MasterType`-Direktive am Anfang `AdminNested.master`hinzu:

[!code-aspx[Main](nested-master-pages-vb/samples/sample16.aspx)]

Beachten Sie, dass die `@MasterType`-Direktive der Code-Behind-Klasse mit dem Namen `Master`eine stark typisierte Eigenschaft hinzufügt. Überschreiben Sie dann die `RefreshRecentProductsGrid`-und `GridMessageText`-Member, und delegieren Sie einfach den Aufruf an die zugehörige-Methode des `Master`:

[!code-vb[Main](nested-master-pages-vb/samples/sample17.vb)]

Wenn dieser Code vorhanden ist, sollten Sie in der Lage sein, die Inhaltsseiten im Abschnitt Verwaltung zu besuchen und zu verwenden. In Abbildung 12 wird die `~/Admin/Products.aspx` Seite angezeigt, wenn Sie in einem Browser angezeigt wird. Wie Sie sehen können, enthält die Seite das Feld Verwaltungsanweisungen, das auf der Seite für die vordefinierte Master Seite definiert ist.

[![die Inhaltsseiten im Abschnitt "Verwaltung" Anweisungen oben auf jeder Seite enthalten.](nested-master-pages-vb/_static/image35.png)](nested-master-pages-vb/_static/image34.png)

**Abbildung 12**: die Inhaltsseiten im Abschnitt "Verwaltung" enthalten Anweisungen oben auf jeder Seite ([Klicken Sie, um das Bild in voller Größe anzuzeigen](nested-master-pages-vb/_static/image36.png))

## <a name="step-7-using-the-appropriate-top-level-master-page-at-runtime"></a>Schritt 7: Verwenden der entsprechenden Master Seite auf oberster Ebene zur Laufzeit

Während alle Inhaltsseiten im Verwaltungsabschnitt voll funktionsfähig sind, verwenden Sie alle dieselbe Master Seite auf oberster Ebene und ignorieren die vom Benutzer ausgewählte Master Seite bei `ChooseMasterPage.aspx`. Dieses Verhalten ist darauf zurückzuführen, dass die `MasterPageFile`-Eigenschaft der geteigenten Master Seite statisch auf `Site.master` in der `<%@ Master %>`-Direktive festgelegt ist.

Um die vom Endbenutzer ausgewählte Master Seite auf oberster Ebene zu verwenden, müssen wir die `MasterPageFile`-Eigenschaft des `AdminNested.master`auf den Wert in der `MyMasterPage` Session-Variablen festlegen. Da wir die `MasterPageFile` Eigenschaften der Inhaltsseiten in `BasePage`festlegen, werden Sie möglicherweise feststellen, dass die `MasterPageFile`-Eigenschaft der-Master Seite in `BaseMasterPage` oder in der Code Behind-Klasse des `AdminNested.master`festgelegt wird. Dies funktioniert jedoch nicht, da die `MasterPageFile`-Eigenschaft am Ende der PreInit-Phase festgelegt werden muss. Die früheste Zeit, die Sie Programm gesteuert auf den Seiten Lebenszyklus von einer Master Seite aus tippen können, ist die Initialisierungsphase (die nach der PreInit-Phase auftritt).

Aus diesem Grund müssen wir die `MasterPageFile`-Eigenschaft der-Eigenschaft der-Master Seite auf den Inhaltsseiten festlegen. Die einzigen Inhaltsseiten, die die `AdminNested.master` Master Seite verwenden, werden von `AdminBasePage`abgeleitet. Daher können wir diese Logik hier einfügen. In Schritt 5 haben wir die `SetMasterPageFile`-Methode überschritten und die `MasterPageFile`-Eigenschaft des Page-Objekts auf "~/admin/AdminNested.Master" festgelegt. Aktualisieren Sie `SetMasterPageFile`, um auch die `MasterPageFile`-Eigenschaft der Master Seite auf das in der Sitzung gespeicherte Ergebnis festzulegen:

[!code-vb[Main](nested-master-pages-vb/samples/sample18.vb)]

Die `GetMasterPageFileFromSession`-Methode, die wir der `BasePage`-Klasse im vorherigen Tutorial hinzugefügt haben, gibt den entsprechenden Datei Pfad der Master Seite basierend auf dem Sitzungsvariablen Wert zurück.

Nachdem diese Änderung vorgenommen wurde, wird die Auswahl der Master Seite des Benutzers in den Abschnitt Verwaltung übernommen. In Abbildung 13 wird dieselbe Seite wie in Abbildung 12 gezeigt, aber nachdem der Benutzer seine Masterseiten Auswahl in `Alternate.master`geändert hat.

[![die Seite "auf der Seite" auf der Seite "auf der Seite für die Seite](nested-master-pages-vb/_static/image38.png)](nested-master-pages-vb/_static/image37.png)

**Abbildung 13**: auf der Seite "auf der Seite" der Seite "wird die vom Benutzer ausgewählte Master Seite auf oberster Ebene verwendet ([Klicken Sie, um das Bild in voller Größe anzuzeigen](nested-master-pages-vb/_static/image39.png))

## <a name="summary"></a>Summary

Ähnlich wie Inhaltsseiten an eine Master Seite gebunden werden können, ist es möglich, eine untergeordnete Master Seite zu erstellen, indem eine untergeordnete Master Seite an eine übergeordnete Master Seite gebunden wird. Die untergeordnete Master Seite kann Inhalts Steuerelemente für jeden der übergeordneten Inhalts Platzhalter definieren. Er kann dann seine eigenen contentplachalter-Steuerelemente (sowie andere Markup) zu diesen Inhalts Steuerelementen hinzufügen. Die Verwendung von in großen Webanwendungen untergeordneten Seiten ist sehr nützlich, wenn alle Seiten ein übergreifendes Aussehen und Gefühl haben, aber für bestimmte Abschnitte der Website sind eindeutige Anpassungen erforderlich.

Fröhliche Programmierung!

### <a name="further-reading"></a>Weiterführende Themen

Weitere Informationen zu den in diesem Tutorial behandelten Themen finden Sie in den folgenden Ressourcen:

- [ASP.net-Master Seiten](https://msdn.microsoft.com/library/x2b3ktt7.aspx)
- [Tipps für die Entwurfsseiten und VS 2005-Entwurfszeit](https://weblogs.asp.net/scottgu/archive/2005/11/11/430382.aspx)
- [VS 2008-Unterstützung für unterstützte Master Seiten](https://weblogs.asp.net/scottgu/archive/2007/07/09/vs-2008-nested-master-page-support.aspx)

### <a name="about-the-author"></a>Informationen zum Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor mehrerer ASP/ASP. net-Bücher und Gründer von 4GuysFromRolla.com, hat seit 1998 mit Microsoft-Webtechnologien gearbeitet. Scott arbeitet als unabhängiger Berater, Ausbilder und Writer. Sein letztes Buch ist [*Sams Teach Yourself ASP.NET 3,5 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672329972/4guysfromrollaco). Scott kann über [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog unter [http://ScottOnWriting.NET](http://scottonwriting.net/)erreicht werden.

### <a name="special-thanks-to"></a>Besonders vielen Dank

Diese tutorialreihe wurde von vielen hilfreichen Reviewern geprüft. Möchten Sie meine bevorstehenden MSDN-Artikel überprüfen? Wenn dies der Fall ist, löschen Sie eine Zeile bei [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Vorheriges](specifying-the-master-page-programmatically-vb.md)
