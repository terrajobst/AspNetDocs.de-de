---
uid: web-pages/overview/ui-layouts-and-themes/3-creating-a-consistent-look
title: Erstellen eines konsistenten Layouts in ASP.net Web Pages Websites (Razor) | Microsoft-Dokumentation
author: Rick-Anderson
description: Um es effizienter zu machen, Webseiten für Ihre Website zu erstellen, können Sie wiederverwendbare Inhaltsblöcke (z. b. Kopf-und Fußzeilen) für Ihre Website erstellen.
ms.author: riande
ms.date: 03/10/2014
ms.assetid: d7bd001b-6db2-4422-9b78-f3d08b743b00
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/3-creating-a-consistent-look
msc.type: authoredcontent
ms.openlocfilehash: 3f63ce68ae4c13970ac0df196167ace0b22b592c
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78509097"
---
# <a name="creating-a-consistent-layout-in-aspnet-web-pages-razor-sites"></a>Erstellen eines konsistenten Layouts in ASP.net Web Pages Websites (Razor)

von [Tom fitzmacken](https://github.com/tfitzmac)

> In diesem Artikel wird erläutert, wie Sie Layoutseiten in einer ASP.net Web Pages-Website (Razor) verwenden können, um wiederverwendbare Inhaltsblöcke (z. b. Kopf-und Fußzeilen) zu erstellen und ein konsistentes Aussehen für alle Seiten der Website zu erstellen.
> 
> **Lernen Sie Folgendes:** 
> 
> - Erstellen wiederverwendbarer Inhaltsblöcke wie Kopf-und Fußzeilen.
> - So erstellen Sie ein konsistentes Aussehen für alle Seiten in Ihrer Site mithilfe eines Layouts.
> - Übergeben von Daten zur Laufzeit an eine Layoutseite.
> 
> Dies sind die ASP.NET-Funktionen, die im Artikel eingeführt wurden:
> 
> - Inhaltsblöcke, bei denen es sich um Dateien handelt, die HTML-formatierte Inhalte enthalten, die auf mehreren Seiten eingefügt werden sollen.
> - Layoutseiten, bei denen es sich um Seiten mit HTML-formatiertem Inhalt handelt, die von Seiten auf der Website gemeinsam genutzt werden können.
> - Die Methoden `RenderPage`, `RenderBody`und `RenderSection`, die angeben ASP.net wo Seitenelemente eingefügt werden sollen.
> - Das `PageData` Wörterbuch, mit dem Sie Daten zwischen Inhalts Blöcken und Layoutseiten freigeben können.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Im Tutorial verwendete Software Versionen
> 
> 
> - ASP.net Web Pages (Razor) 3
>   
> 
> Dieses Tutorial funktioniert auch mit ASP.net Web Pages 2.

## <a name="about-layout-pages"></a>Informationen zu Layoutseiten

Viele Websites verfügen über Inhalte, die auf jeder Seite angezeigt werden, wie z. b. eine Kopf-und Fußzeile, oder ein Feld, das den Benutzern mitteilt, dass Sie angemeldet sind. ASP.NET ermöglicht das Erstellen einer separaten Datei mit einem Inhalts Block, der Text, Markup und Code enthalten kann, genau wie eine reguläre Webseite. Anschließend können Sie den Inhalts Block in andere Seiten auf der Website einfügen, auf der die Informationen angezeigt werden sollen. Auf diese Weise müssen Sie nicht denselben Inhalt kopieren und in jede Seite einfügen. Wenn Sie allgemeine Inhalte wie diese erstellen, ist es auch einfacher, Ihre Website zu aktualisieren. Wenn Sie den Inhalt ändern müssen, können Sie nur eine einzelne Datei aktualisieren, und die Änderungen werden dann überall dort widergespiegelt, wo der Inhalt eingefügt wurde.

Das folgende Diagramm zeigt, wie Inhaltsblöcke funktionieren. Wenn ein Browser eine Seite vom Webserver anfordert, fügt ASP.net die Inhaltsblöcke an dem Punkt ein, an dem die `RenderPage`-Methode auf der Hauptseite aufgerufen wird. Die fertiggestellte Seite (zusammengeführt) wird dann an den Browser gesendet.

![Konzeptionelles Diagramm, das zeigt, wie die renderpage-Methode eine referenzierte Seite in die aktuelle Seite einfügt.](3-creating-a-consistent-look/_static/image1.jpg)

In diesem Verfahren erstellen Sie eine Seite, die auf zwei Inhaltsblöcke (eine Kopfzeile und eine Fußzeile) verweist, die sich in separaten Dateien befinden. Sie können dieselben Inhaltsblöcke auf jeder Seite der Website verwenden. Wenn Sie fertig sind, erhalten Sie eine Seite wie die folgende:

![Screenshot, der eine Seite im Browser anzeigt, die sich aus der Ausführung einer Seite ergibt, die Aufrufe der renderpage-Methode einschließt.](3-creating-a-consistent-look/_static/image2.png)

1. Erstellen Sie im Stamm Ordner der Website eine Datei namens *Index. cshtml*.
2. Ersetzen Sie das vorhandene Markup durch Folgendes:

    [!code-html[Main](3-creating-a-consistent-look/samples/sample1.html)]
3. Erstellen Sie im Stamm Ordner einen Ordner mit dem Namen *Shared*.

    > [!NOTE]
    > Es ist üblich, Dateien zu speichern, die von Webseiten in einem Ordner mit dem Namen *Shared*gemeinsam genutzt werden.
4. Erstellen Sie im frei *gegebenen* Ordner eine Datei mit dem Namen *\_Header. cshtml*.
5. Ersetzen Sie vorhandene Inhalte durch Folgendes:

    [!code-html[Main](3-creating-a-consistent-look/samples/sample2.html)]

    Beachten Sie, dass der Dateiname *\_Header. cshtml*mit einem Unterstrich (\_) als Präfix ist. ASP.NET sendet keine Seite an den Browser, wenn der Name mit einem Unterstrich beginnt. Dies hindert Personen daran, diese Seiten direkt oder anderweitig anzufordern. Es ist ratsam, einen Unterstrich zu verwenden, um Seiten zu benennen, die Inhaltsblöcke enthalten, da Benutzer nicht wirklich in der Lage sein sollen, diese Seiten &#8212; , die Sie bereits vorhanden sind, in andere Seiten einzufügen.
6. Erstellen Sie im frei *gegebenen* Ordner eine Datei namens " *\_footer. cshtml* ", und ersetzen Sie den Inhalt durch Folgendes:

    [!code-html[Main](3-creating-a-consistent-look/samples/sample3.html)]
7. Fügen Sie auf der Seite *Index. cshtml* zwei Aufrufe der `RenderPage`-Methode hinzu, wie hier gezeigt:

    [!code-html[Main](3-creating-a-consistent-look/samples/sample4.html)]

    Dies zeigt, wie ein Inhalts Block in eine Webseite eingefügt wird. Sie nennen die `RenderPage`-Methode und übergeben ihr den Namen der Datei, deren Inhalt Sie an diesem Punkt einfügen möchten. Hier fügen Sie den Inhalt der Dateien " *\_Header. cshtml* " und " *\_footer. cshtml* " in die Datei " *Index. cshtml* " ein.
8. Führen Sie die *Index. cshtml* -Seite in einem Browser aus. (Klicken Sie in webmatrix im Arbeitsbereich " **Dateien** " mit der rechten Maustaste auf die Datei, und wählen Sie dann **im Browser starten aus**.)
9. Zeigen Sie im Browser die Seitenquelle an. (Klicken Sie in Internet Explorer z. b. mit der rechten Maustaste auf die Seite, und klicken Sie dann auf **Quelle anzeigen**.)

    Dadurch wird das auf dem Browser gesendete Webseiten Markup angezeigt, das das Markup der Indexseite mit den Inhalts Blöcken kombiniert. Das folgende Beispiel zeigt die Seitenquelle, die für " *Index. cshtml*" gerendert wird. Die Aufrufe von `RenderPage`, die Sie in " *Index. cshtml* " eingefügt haben, wurden durch den eigentlichen Inhalt der Header-und footerdateien ersetzt.

    [!code-html[Main](3-creating-a-consistent-look/samples/sample5.html)]

## <a name="creating-a-consistent-look-using-layout-pages"></a>Erstellen eines konsistenten Erscheinungsbild mithilfe von Layoutseiten

Bisher haben Sie gesehen, dass es einfach ist, denselben Inhalt auf mehreren Seiten zu integrieren. Eine strukturiertere Methode zum Erstellen einer konsistenten Suche nach einer Site ist die Verwendung von Layoutseiten. Eine Layoutseite definiert die Struktur einer Webseite, enthält jedoch keinen tatsächlichen Inhalt. Nachdem Sie eine Layoutseite erstellt haben, können Sie Webseiten erstellen, die den Inhalt enthalten, und Sie dann mit der Layoutseite verknüpfen. Wenn diese Seiten angezeigt werden, werden Sie entsprechend der Layoutseite formatiert. (In diesem Sinne fungiert eine Layoutseite als eine Art Vorlage für Inhalte, die auf anderen Seiten definiert sind.)

Die Layoutseite ist genauso wie jede beliebige HTML-Seite, mit dem Unterschied, dass Sie einen aufzurufenden `RenderBody` Methode enthält. Die Position der `RenderBody` Methode auf der Layoutseite bestimmt, wo die Informationen von der Inhaltsseite eingeschlossen werden.

Das folgende Diagramm zeigt, wie Inhaltsseiten und Layoutseiten zur Laufzeit kombiniert werden, um die fertige Webseite zu erstellen. Der Browser fordert eine Inhaltsseite an. Die Inhaltsseite enthält Code, der die Layoutseite angibt, die für die Struktur der Seite verwendet werden soll. Auf der Layoutseite wird der Inhalt an dem Punkt eingefügt, an dem die `RenderBody`-Methode aufgerufen wird. Inhaltsblöcke können auch auf der Layoutseite eingefügt werden, indem Sie die `RenderPage`-Methode wie im vorherigen Abschnitt aufrufen. Wenn die Webseite fertig ist, wird Sie an den Browser gesendet.

![Screenshot, der eine Seite im Browser anzeigt, die sich aus der Ausführung einer Seite ergibt, die Aufrufe der RenderBody-Methode einschließt.](3-creating-a-consistent-look/_static/image3.jpg)

Im folgenden Verfahren wird gezeigt, wie eine Layoutseite erstellt und Inhaltsseiten verknüpft werden.

1. Erstellen Sie im frei *gegebenen* Ordner der Website eine Datei mit dem Namen *\_layout1. cshtml*.
2. Ersetzen Sie vorhandene Inhalte durch Folgendes:

    [!code-html[Main](3-creating-a-consistent-look/samples/sample6.html)]

    Verwenden Sie die `RenderPage`-Methode in einer Layoutseite, um Inhaltsblöcke einzufügen. Eine Layoutseite kann nur einen aufzurufenden `RenderBody`-Methode enthalten.
3. Erstellen Sie im frei *gegebenen* Ordner eine Datei mit dem Namen *\_Header2. cshtml* , und ersetzen Sie vorhandene Inhalte durch Folgendes:

    [!code-html[Main](3-creating-a-consistent-look/samples/sample7.html)]
4. Erstellen Sie im Stamm Ordner einen neuen Ordner, und benennen Sie ihn *Stile*.
5. Erstellen Sie im Ordner " *Stile* " eine Datei mit dem Namen " *Site. CSS* ", und fügen Sie die folgenden Stildefinitionen hinzu:

    [!code-css[Main](3-creating-a-consistent-look/samples/sample8.css)]

    Diese Stildefinitionen werden hier nur angezeigt, um zu veranschaulichen, wie Stylesheets mit Layoutseiten verwendet werden können. Wenn Sie möchten, können Sie eigene Stile für diese Elemente definieren.
6. Erstellen Sie im Stamm Ordner eine Datei mit dem Namen *Content1. cshtml* , und ersetzen Sie vorhandene Inhalte durch Folgendes:

    [!code-cshtml[Main](3-creating-a-consistent-look/samples/sample9.cshtml)]

    Dies ist eine Seite, auf der eine Layoutseite verwendet wird. Der Codeblock am oberen Rand der Seite gibt an, welche Layoutseite zum Formatieren dieses Inhalts verwendet werden soll.
7. Führen Sie *Content1. cshtml* in einem Browser aus. Die gerenderte Seite verwendet das Format und Stylesheet, das in *\_layout1. cshtml* definiert ist, und den Text (Content), der in *Content1. cshtml*definiert ist.

    ![Klang](3-creating-a-consistent-look/_static/image4.png)

    Sie können Schritt 6 wiederholen, um zusätzliche Inhaltsseiten zu erstellen, die die gleiche Layoutseite gemeinsam verwenden können.

    > [!NOTE]
    > Sie können Ihre Website so einrichten, dass Sie automatisch die gleiche Layoutseite für alle Inhaltsseiten in einem Ordner verwenden können. Weitere Informationen finden Sie unter [Anpassen des Standort weiten Verhaltens für ASP.net Web Pages](https://go.microsoft.com/fwlink/?LinkId=202906).

## <a name="designing-layout-pages-that-have-multiple-content-sections"></a>Entwerfen von Layoutseiten mit mehreren Inhalts Abschnitten

Eine Inhaltsseite kann mehrere Abschnitte enthalten. Dies ist nützlich, wenn Sie Layouts verwenden möchten, die mehrere Bereiche mit austauschbarem Inhalt aufweisen. Auf der Seite Inhalt übergeben Sie jedem Abschnitt einen eindeutigen Namen. (Der Standardabschnitt ist unbenannt.) Fügen Sie auf der Seite Layout eine `RenderBody` Methode hinzu, um anzugeben, wo der unbenannte (Standard-) Abschnitt angezeigt werden soll. Fügen Sie dann separate `RenderSection` Methoden hinzu, um benannte Abschnitte einzeln zu Rendering.

Das folgende Diagramm zeigt, wie ASP.net Inhalte verarbeitet, die in mehrere Abschnitte unterteilt sind. Jeder benannte Abschnitt ist in einem Abschnitts Block auf der Inhaltsseite enthalten. (Sie werden im Beispiel `Header` und `List` benannt.) Das Framework fügt den Inhalts Abschnitt an dem Punkt, an dem die `RenderSection`-Methode aufgerufen wird, in die Layoutseite ein. Der unbenannte (Standard-) Abschnitt wird an dem Punkt eingefügt, an dem die `RenderBody`-Methode aufgerufen wird, wie Sie zuvor gesehen haben.

![Konzeptionelles Diagramm, das zeigt, wie die rendersection-Methode Verweis Abschnitte in die aktuelle Seite einfügt.](3-creating-a-consistent-look/_static/image5.jpg)

In diesem Verfahren wird gezeigt, wie eine Inhaltsseite mit mehreren Inhalts Abschnitten erstellt wird und wie diese mithilfe einer Layoutseite, die mehrere Inhalts Abschnitte unterstützt, erstellt wird.

1. Erstellen Sie im frei *gegebenen* Ordner eine Datei mit dem Namen *\_Layout2. cshtml*.
2. Ersetzen Sie vorhandene Inhalte durch Folgendes:

    [!code-html[Main](3-creating-a-consistent-look/samples/sample10.html)]

    Verwenden Sie die `RenderSection`-Methode, um die Header-und Listen Abschnitte zu Rendering.
3. Erstellen Sie im Stamm Ordner eine Datei mit dem Namen *Content2. cshtml* , und ersetzen Sie vorhandene Inhalte durch Folgendes:

    [!code-cshtml[Main](3-creating-a-consistent-look/samples/sample11.cshtml)]

    Diese Inhaltsseite enthält einen Codeblock am oberen Rand der Seite. Jeder benannte Abschnitt ist in einem Abschnitts Block enthalten. Der Rest der Seite enthält den Standard Inhalts Abschnitt (unbenannt).
4. Führen Sie *Content2. cshtml* in einem Browser aus.

    ![Screenshot, der eine Seite im Browser anzeigt, die sich aus der Ausführung einer Seite ergibt, die Aufrufe der rendersection-Methode einschließt.](3-creating-a-consistent-look/_static/image6.png)

## <a name="making-content-sections-optional"></a>Inhalts Abschnitte sind optional

Normalerweise müssen die Abschnitte, die Sie auf einer Inhaltsseite erstellen, den Abschnitten entsprechen, die auf der Layoutseite definiert sind. Wenn eine der folgenden Aktionen auftritt, können Sie Fehlermeldungen erhalten:

- Die Seite Inhalt enthält einen Abschnitt, der über keinen entsprechenden Abschnitt auf der Layoutseite verfügt.
- Die Layoutseite enthält einen Abschnitt, für den kein Inhalt vorhanden ist.
- Die Layoutseite enthält Methodenaufrufe, die versuchen, denselben Abschnitt mehrmals zu Rendering auszuführen.

Sie können dieses Verhalten jedoch für einen benannten Abschnitt überschreiben, indem Sie den Abschnitt So deklarieren, dass er auf der Layoutseite optional ist. Auf diese Weise können Sie mehrere Inhaltsseiten definieren, die eine Layoutseite gemeinsam verwenden können, die aber möglicherweise keinen Inhalt für einen bestimmten Abschnitt enthalten.

1. Öffnen Sie *Content2. cshtml* , und entfernen Sie den folgenden Abschnitt:

    [!code-cshtml[Main](3-creating-a-consistent-look/samples/sample12.cshtml)]
2. Speichern Sie die Seite, und führen Sie Sie dann in einem Browser aus. Eine Fehlermeldung wird angezeigt, da die Inhaltsseite keinen Inhalt für einen Abschnitt bereitstellt, der auf der Layoutseite definiert ist, nämlich den Header Abschnitt.

    ![Screenshot, der den Fehler anzeigt, der auftritt, wenn Sie eine Seite ausführen, die die rendersection-Methode aufruft, der entsprechende Abschnitt jedoch nicht bereitgestellt wird.](3-creating-a-consistent-look/_static/image7.png)
3. Öffnen Sie im frei *gegebenen* Ordner die Seite *\_Layout2. cshtml* , und ersetzen Sie diese Zeile:

    [!code-javascript[Main](3-creating-a-consistent-look/samples/sample13.js)]

    durch den folgenden Code:

    [!code-javascript[Main](3-creating-a-consistent-look/samples/sample14.js)]

    Als Alternative können Sie die vorherige Codezeile durch den folgenden Codeblock ersetzen, der die gleichen Ergebnisse erzeugt:

    [!code-cshtml[Main](3-creating-a-consistent-look/samples/sample15.cshtml)]
4. Führen Sie die *Content2. cshtml* -Seite in einem Browser erneut aus. (Wenn diese Seite weiterhin im Browser geöffnet ist, können Sie Sie einfach aktualisieren.) Dieses Mal wird die Seite ohne Fehler angezeigt, auch wenn Sie über keinen Header verfügt.

## <a name="passing-data-to-layout-pages"></a>Übergeben von Daten an Layoutseiten

Möglicherweise verfügen Sie über Daten, die auf der Inhaltsseite definiert sind, auf die Sie auf einer Layoutseite verweisen müssen. Wenn dies der Fall ist, müssen Sie die Daten von der Inhaltsseite an die Layoutseite übergeben. Beispielsweise möchten Sie möglicherweise den Anmeldestatus eines Benutzers anzeigen, oder Sie können Inhaltsbereiche auf der Grundlage von Benutzereingaben anzeigen oder ausblenden.

Wenn Sie Daten von einer Inhaltsseite an eine Layoutseite übergeben möchten, können Sie Werte in der `PageData`-Eigenschaft der Inhaltsseite ablegen. Die `PageData`-Eigenschaft ist eine Sammlung von Name-Wert-Paaren, die die Daten enthalten, die Sie zwischen Seiten übergeben möchten. Auf der Layoutseite können Sie Werte aus der `PageData`-Eigenschaft lesen.

Im folgenden finden Sie ein weiteres Diagramm. In diesem Beispiel wird gezeigt, wie ASP.net die `PageData`-Eigenschaft verwenden kann, um Werte von einer Inhaltsseite an die Layoutseite zu übergeben. Wenn ASP.net mit dem Erstellen der Webseite beginnt, wird die `PageData`-Sammlung erstellt. Auf der Seite Inhalt schreiben Sie Code zum Einfügen von Daten in die `PageData` Auflistung. Auf Werte in der `PageData` Auflistung kann auch in anderen Abschnitten der Inhaltsseite oder durch zusätzliche Inhaltsblöcke zugegriffen werden.

![Konzeptionelles Diagramm, das zeigt, wie eine Inhaltsseite ein PageData-Wörterbuch auffüllen und diese Informationen an die Layoutseite übergeben kann.](3-creating-a-consistent-look/_static/image8.jpg)

Im folgenden Verfahren wird gezeigt, wie Daten von einer Inhaltsseite an eine Layoutseite übergeben werden. Wenn die Seite ausgeführt wird, wird eine Schaltfläche angezeigt, mit der der Benutzer eine Liste ausblenden oder anzeigen kann, die auf der Layoutseite definiert ist. Wenn Benutzer auf die Schaltfläche klicken, wird in der `PageData`-Eigenschaft der Wert true/false (Boolean) festgelegt. Auf der Layoutseite wird dieser Wert gelesen, und wenn der Wert false ist, wird die Liste ausgeblendet. Der Wert wird auch auf der Seite Inhalt verwendet, um zu bestimmen, ob die Schaltfläche **Liste ausblenden** oder **Liste** anzeigen angezeigt werden soll.

![Klang](3-creating-a-consistent-look/_static/image9.jpg)

1. Erstellen Sie im Stamm Ordner eine Datei mit dem Namen *Content3. cshtml* , und ersetzen Sie vorhandene Inhalte durch Folgendes:

    [!code-cshtml[Main](3-creating-a-consistent-look/samples/sample16.cshtml)]

    Der Code speichert zwei Datenelemente in der `PageData`-Eigenschaft &#8212; mit dem Titel der Webseite und true oder false, um anzugeben, ob eine Liste angezeigt werden soll.

    Beachten Sie, dass Sie mithilfe von ASP.net HTML-Markup mithilfe eines Codeblocks bedingt auf der Seite ablegen können. Beispielsweise bestimmt der `if/else`-Block im Text der Seite, welches Formular angezeigt wird, je nachdem, ob `PageData["ShowList"]` auf true festgelegt ist.
2. Erstellen Sie im frei *gegebenen* Ordner eine Datei mit dem Namen *\_Layout3. cshtml* , und ersetzen Sie vorhandene Inhalte durch Folgendes:

    [!code-cshtml[Main](3-creating-a-consistent-look/samples/sample17.cshtml)]

    Die Layoutseite enthält einen Ausdruck in das `<title>`-Element, das den Titelwert aus der `PageData`-Eigenschaft abruft. Außerdem wird der `ShowList` Wert der `PageData`-Eigenschaft verwendet, um zu bestimmen, ob der Listen Inhalts Block angezeigt werden soll.
3. Erstellen Sie im frei *gegebenen* Ordner eine Datei namens *\_List. cshtml* , und ersetzen Sie vorhandene Inhalte durch Folgendes:

    [!code-html[Main](3-creating-a-consistent-look/samples/sample18.html)]
4. Führen Sie die *Content3. cshtml* -Seite in einem Browser aus. Die Seite wird mit der Liste angezeigt, die auf der linken Seite der Seite angezeigt wird, und der Schaltfläche **Liste ausblenden** unten.

    ![Screenshot, der die Seite mit der Liste und eine Schaltfläche mit dem Text "Ausblenden der Liste" anzeigt.](3-creating-a-consistent-look/_static/image10.png)
5. Klicken Sie auf **Liste ausblenden**. Die Liste wird nicht mehr angezeigt, und die Schaltfläche wechselt zur **Liste anzeigen**.

    ![Screenshot, der die Seite anzeigt, die die Liste nicht enthält, und eine Schaltfläche mit dem Hinweis "Liste anzeigen".](3-creating-a-consistent-look/_static/image11.png)
6. Klicken Sie auf die Schaltfläche **Liste anzeigen** , und die Liste wird erneut angezeigt.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

[Anpassen des Standort weiten Verhaltens für ASP.net Web Pages](https://go.microsoft.com/fwlink/?LinkId=202906)
