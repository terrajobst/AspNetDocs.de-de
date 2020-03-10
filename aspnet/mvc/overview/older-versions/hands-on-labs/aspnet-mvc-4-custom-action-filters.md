---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-custom-action-filters
title: ASP.NET MVC 4 benutzerdefinierte Aktionsfilter | Microsoft-Dokumentation
author: rick-anderson
description: ASP.NET MVC stellt Aktionsfilter zum Ausführen der Filter Logik bereit, bevor oder nachdem eine Aktionsmethode aufgerufen wird. Aktionsfilter sind benutzerdefinierte Attribute...
ms.author: riande
ms.date: 02/18/2013
ms.assetid: 969ab824-1b98-4552-81fe-b60ef5fc6887
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-custom-action-filters
msc.type: authoredcontent
ms.openlocfilehash: eaeb32180f79fabf557cbc38ff067eb26b47fea7
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78468177"
---
# <a name="aspnet-mvc-4-custom-action-filters"></a>ASP.NET MVC 4 – Benutzerdefinierte Aktionsfilter

vom [Web Camps-Team](https://twitter.com/webcamps)

[Webcamps-Trainingskit herunterladen](https://aka.ms/webcamps-training-kit)

ASP.NET MVC stellt Aktionsfilter zum Ausführen der Filter Logik bereit, bevor oder nachdem eine Aktionsmethode aufgerufen wird. Aktionsfilter sind benutzerdefinierte Attribute, die deklarative Mittel zum Hinzufügen von vor-und nach-Aktion-Verhalten zu den Aktionsmethoden des Controllers bereitstellen.

In dieser praktischen Übungseinheit erstellen Sie ein benutzerdefiniertes Aktionsfilter Attribut in der mvcmusicstore-Projekt Mappe, um die Anforderungen des Controllers abzufangen und die Aktivität einer Website in einer Datenbanktabelle zu protokollieren. Sie können den Protokollierungs Filter per Injection zu einem beliebigen Controller oder einer Aktion hinzufügen. Schließlich wird die Protokoll Ansicht angezeigt, in der die Liste der Besucher angezeigt wird.

Diese praktische Übung geht davon aus, dass Sie über grundlegende Kenntnisse von **ASP.NET MVC**verfügen. Wenn Sie **ASP.NET MVC** noch nicht verwendet haben, empfehlen wir Ihnen, **ASP.NET MVC 4-Grundlagen** praktische Übungseinheiten zu überspringen.

> [!NOTE]
> Der gesamte Beispielcode und die Code Ausschnitte sind im Web Camps-Trainingskit enthalten, das in den [Releases Microsoft-Web/webcamptrainingkit](https://aka.ms/webcamps-training-kit)verfügbar ist. Das für dieses Lab spezifische Projekt ist unter [ASP.NET MVC 4 Custom Action Filters](https://github.com/Microsoft-Web/HOL-MVC4CustomActionFilters)verfügbar.

<a id="Objectives"></a>
### <a name="objectives"></a>Ziele

In dieser praktischen Übungseinheit erfahren Sie Folgendes:

- Erstellen eines benutzerdefinierten Aktionsfilter Attributs zum Erweitern der Filterfunktionen
- Anwenden eines benutzerdefinierten Filter Attributs per injection auf eine bestimmte Ebene
- Globales Registrieren eines benutzerdefinierten Aktions Filters

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Voraussetzungen

Zum Durchführen dieses Labs müssen Sie über Folgendes verfügen:

- [Microsoft Visual Studio Express 2012 für Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) oder Superior (Informationen zur Installation finden Sie in [Anhang A](#AppendixA) ).

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a>Einrichten

**Installieren von Code Ausschnitten**

Der Vorteil ist, dass ein Großteil des Codes, den Sie in diesem Lab verwalten, als Visual Studio-Code Ausschnitte verfügbar ist. Um die Code Ausschnitte zu installieren, führen Sie **.\source\setup\codesnippeer-.vsi** -Datei aus.

Wenn Sie mit den Visual Studio Code Ausschnitten nicht vertraut sind und wissen möchten, wie Sie Sie verwenden können, können Sie den Anhang dieses Dokuments &quot;[Anhang C: Verwenden von Code Ausschnitten](#AppendixC)&quot;entnehmen.

---

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a>Exerzitien

Diese praktische Übungseinheit besteht aus den folgenden Übungen:

1. [Übung 1: Protokollierungs Aktionen](#Exercise1)
2. [Übung 2: Verwalten mehrerer Aktionsfilter](#Exercise2)

Geschätzte Zeit bis zum Abschluss dieses Labs: **30 Minuten**.

> [!NOTE]
> Jede Übung wird von einem **endordner** begleitet, der die sich ergebende Lösung enthält, die Sie nach Abschluss der Übungen erhalten. Sie können diese Lösung als Leitfaden verwenden, wenn Sie zusätzliche Hilfe beim Durcharbeiten der Übungen benötigen.

<a id="Exercise1"></a>

<a id="Exercise_1_Logging_Actions"></a>
### <a name="exercise-1-logging-actions"></a>Übung 1: Protokollierungs Aktionen

In dieser Übung erfahren Sie, wie Sie einen benutzerdefinierten Aktionsprotokoll Filter mithilfe von ASP.NET MVC 4-Filter Anbietern erstellen. Zu diesem Zweck wenden Sie einen Protokollierungs Filter auf die Musicstore-Website an, mit der alle Aktivitäten in den ausgewählten Controllern aufgezeichnet werden.

Der Filter erweitert **Action Filter attributeclass** und setzt die **OnAction** -Methode außer Kraft, um die einzelnen Anforderungen abzufangen und dann die Protokollierungs Aktionen auszuführen. Die Kontextinformationen zu HTTP-Anforderungen, zum Ausführen von Methoden, Ergebnissen und Parametern werden von der ASP.NET MVC-Klasse " **aktionsexecutingcontext** " bereitgestellt **.**

> [!NOTE]
> ASP.NET MVC 4 verfügt auch über Standardfilter Anbieter, die Sie verwenden können, ohne einen benutzerdefinierten Filter zu erstellen. ASP.NET MVC 4 bietet die folgenden Typen von Filtern:
> 
> - **Autorisierungs** Filter, der Sicherheitsentscheidungen darüber trifft, ob eine Aktionsmethode ausgeführt werden soll, z. b. das Durchführen der Authentifizierung oder das Überprüfen von Eigenschaften der Anforderung.
> - **Aktions** Filter, der die Ausführung der Aktionsmethode umschließt. Mit diesem Filter können zusätzliche Verarbeitungsschritte ausgeführt werden, z. b. das Bereitstellen zusätzlicher Daten für die Aktionsmethode, das Überprüfen des Rückgabewerts oder das Abbrechen der Ausführung der Aktionsmethode.
> - Der **Ergebnis** Filter, der die Ausführung des Aktions result-Objekts umschließt. Dieser Filter kann zusätzliche Verarbeitungsschritte durchführen, z. b. das Ändern der HTTP-Antwort.
> - **Ausnahme** Filter, der ausgeführt wird, wenn eine nicht behandelte Ausnahme in der Aktionsmethode ausgelöst wird, beginnend mit den Autorisierungs Filtern und mit der Ausführung des Ergebnisses. Ausnahme Filter können für Aufgaben wie z. b. die Protokollierung oder die Anzeige einer Fehlerseite verwendet werden.
> 
> Weitere Informationen zu Filter Anbietern finden Sie in diesem MSDN-Link: ([https://msdn.microsoft.com/library/dd410209.aspx](https://msdn.microsoft.com/library/dd410209.aspx)).

<a id="AboutLoggingFeature"></a>

<a id="About_MVC_Music_Store_Application_logging_feature"></a>
#### <a name="about-mvc-music-store-application-logging-feature"></a>Informationen zum MVC Music Store-Anwendungs Protokollierungs Feature

Diese Music Store-Projekt Mappe verfügt über eine neue Datenmodell Tabelle für die Website Protokollierung " **Action Log**" mit den folgenden Feldern: Name des Controllers, der eine Anforderung empfangen hat, so genannte Aktion, Client-IP und Zeitstempel.

![Datenmodell. Aktionsprotokoll Tabelle.](aspnet-mvc-4-custom-action-filters/_static/image1.png "Datenmodell. Aktionsprotokoll Tabelle.")

*Datenmodell-Aktionsprotokoll Tabelle*

Die Lösung bietet eine ASP.NET MVC-Ansicht für das Aktionsprotokoll, das Sie unter **mvcmusicstores/views/Action Log**finden:

![Aktionsprotokoll Ansicht](aspnet-mvc-4-custom-action-filters/_static/image2.png "Aktionsprotokoll Ansicht")

*Aktionsprotokoll Ansicht*

Bei dieser gegebenen Struktur konzentrieren sich alle Aufgaben auf das Unterbrechen der Anforderung des Controllers und das Ausführen der Protokollierung mithilfe der benutzerdefinierten Filterung.

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_a_Custom_Filter_to_Catch_a_Controllers_Request"></a>
#### <a name="task-1---creating-a-custom-filter-to-catch-a-controllers-request"></a>Aufgabe 1: Erstellen eines benutzerdefinierten Filters zum Abfangen der Anforderung eines Controllers

In dieser Aufgabe erstellen Sie eine benutzerdefinierte Filter Attribut Klasse, die die Protokollierungs Logik enthält. Zu diesem Zweck erweitern Sie die ASP.NET MVC **Action Filter Attribute** -Klasse und implementieren die **IAction Filter**-Schnittstelle.

> [!NOTE]
> Das **Action Filter Attribute-Attribut** ist die Basisklasse für alle Attribut Filter. Es stellt die folgenden Methoden bereit, um eine bestimmte Logik nach und vor der Ausführung der Controller Aktion auszuführen:
> 
> - **OnAction-Ausführung**(Action executingcontext Filter context): unmittelbar vor dem Aufruf der Action-Methode.
> - **OnAction**-Anweisung (Action executedcontext Filter context): nach dem Aufrufen der Aktionsmethode und vor dem Ausführen des Ergebnisses (vor dem Anzeigen des Rendering).
> - **OnResultExecuting**(resultexecutingcontext Filter context): unmittelbar vor dem Ausführen des Ergebnisses (vor dem Anzeigen des Rendering).
> - **OnResultExecuted**(resultexecutedcontext Filter context): nach dem Ausführen des Ergebnisses (nachdem die Ansicht gerendert wurde).
> 
> Wenn Sie eine dieser Methoden in einer abgeleiteten Klasse überschreiben, können Sie Ihren eigenen Filter Code ausführen.

1. Öffnen Sie die Projekt Mappe " **Begin** " im Ordner " **\source\ex01-loggingaktions\begin** ".

   1. Sie müssen einige fehlende nuget-Pakete herunterladen, bevor Sie den Vorgang fortsetzen. Klicken Sie hierzu auf das Menü **Projekt** , und wählen Sie **nuget-Pakete verwalten**aus.
   2. Klicken Sie im Dialogfeld **nuget-Pakete verwalten** auf **Wiederherstellen** , um fehlende Pakete herunterzuladen.
   3. Erstellen Sie abschließend die Projekt Mappe, indem Sie auf **Build** ** | Projekt Mappe erstellen klicken**.

      > [!NOTE]
      > Einer der Vorteile der Verwendung von nuget besteht darin, dass Sie nicht alle Bibliotheken in Ihrem Projekt liefern müssen, um die Projektgröße zu verringern. Mit nuget Power Tools können Sie durch Angabe der Paketversionen in der Datei "Packages. config" alle erforderlichen Bibliotheken herunterladen, wenn Sie das Projekt zum ersten Mal ausführen. Aus diesem Grund müssen Sie diese Schritte ausführen, nachdem Sie eine vorhandene Projekt Mappe in diesem Lab geöffnet haben.
      > 
      > Weitere Informationen finden Sie in diesem Artikel: [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).
2. Fügen Sie dem C# Ordner **Filters** eine neue Klasse hinzu, und nennen Sie Sie *CustomActionFilter.cs*. In diesem Ordner werden alle benutzerdefinierten Filter gespeichert.
3. Öffnen Sie **CustomActionFilter.cs** , und fügen Sie einen Verweis auf **System. Web. MVC** -und **mvcmusicstore. Models** -Namespaces hinzu:

    (Code Ausschnitt- *ASP.NET MVC 4 benutzerdefinierte Aktionsfilter-EX1-customaktionfilternamespaces*)

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample1.cs)]
4. Erben Sie die **CustomAction Filter** -Klasse von **Action Filter Attribute** , und legen Sie dann die **CustomAction Filter** -Klasse die **IAction Filter** -Schnittstelle an.

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample2.cs)]
5. Legen Sie die **customaktionfilter** -Klasse überschreiben Sie die **onaktionexecution** -Methode, und fügen Sie die erforderliche Logik zum Protokollieren der Filter Ausführung hinzu. Fügen Sie zu diesem Zweck den folgenden markierten Code in der **customaktionfilter** -Klasse hinzu.

    (Code Ausschnitt- *ASP.NET MVC 4 benutzerdefinierte Aktionsfilter-EX1-loggingactions*)

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample3.cs#Highlight)]

    > [!NOTE]
    > Die **onaktionsmethode** verwendet **Entity Framework** , um ein neues Aktionsprotokoll hinzuzufügen. Er erstellt und füllt eine neue Entitäts Instanz mit den Kontextinformationen aus **FilterContext**.
    > 
    > Weitere Informationen zur **controllerContext** -Klasse finden Sie auf der [MSDN](https://msdn.microsoft.com/library/system.web.mvc.controllercontext.aspx)-Website.

<a id="Ex1Task2"></a>

<a id="Task_2_-_Injecting_a_Code_Interceptor_into_the_Store_Controller_Class"></a>
#### <a name="task-2---injecting-a-code-interceptor-into-the-store-controller-class"></a>Aufgabe 2: Einfügen eines codeinterceptors in die Speicher Controller Klasse

In dieser Aufgabe fügen Sie den benutzerdefinierten Filter hinzu, indem Sie ihn in alle Controller Klassen und Controller Aktionen einfügen, die protokolliert werden. Für diese Übung verfügt die Store Controller-Klasse über ein Protokoll.

Die **OnAction** -Methode der Methode aus dem benutzerdefinierten **Action logfilterattribute** -Filter wird ausgeführt, wenn ein injiziertes Element aufgerufen wird.

Es ist auch möglich, eine bestimmte Controller Methode abzufangen.

1. Öffnen Sie **StoreController** unter **mvcmusicstore\controllers** , und fügen Sie einen Verweis auf den **Filter** -Namespace hinzu:

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample4.cs)]
2. Fügen Sie den benutzerdefinierten Filter **customaktionfilter** in die **StoreController** -Klasse ein, indem Sie das Attribut **[customaktionfilter]** vor der Klassen Deklaration hinzufügen

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample5.cs)]

   > [!NOTE]
   > Wenn ein Filter in eine Controller Klasse eingefügt wird, werden alle zugehörigen Aktionen ebenfalls eingefügt. Wenn Sie den Filter nur für eine Reihe von Aktionen anwenden möchten, müssten Sie **[customaktionfilter]** an jeden der folgenden Elemente einfügen:
   > 
   > [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample6.cs)]

<a id="Ex1Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a>Aufgabe 3: Ausführen der Anwendung

In dieser Aufgabe testen Sie, ob der Protokollierungs Filter funktioniert. Sie starten die Anwendung und besuchen den Store, und dann überprüfen Sie die protokollierten Aktivitäten.

1. Drücken Sie **F5**, um die Anwendung auszuführen.
2. Navigieren Sie zu **/ActionLog** , um den Anfangszustand der Protokoll Ansicht anzuzeigen:

    ![Protokoll verfolgungsstatus vor Seiten Aktivität](aspnet-mvc-4-custom-action-filters/_static/image3.png "Protokoll verfolgungsstatus vor Seiten Aktivität")

    *Protokoll verfolgungsstatus vor Seiten Aktivität*

   > [!NOTE]
   > Standardmäßig wird immer ein Element angezeigt, das beim Abrufen der vorhandenen Genres für das Menü generiert wird.
   > 
   > Aus Gründen der Einfachheit wird die Tabelle " **aktionlog** " bereinigt, wenn die Anwendung ausgeführt wird, sodass nur die Protokolle der Überprüfung der jeweiligen Aufgabe angezeigt werden.
   > 
   > Möglicherweise müssen Sie den folgenden Code aus der **Sitzungs\_Start** -Methode (in der **Global. asax** -Klasse) entfernen, um ein Verlaufs Protokoll für alle innerhalb des Speicher Controllers ausgeführten Aktionen zu speichern.
   > 
   > [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample7.cs)]
3. Klicken Sie im Menü auf eines der **Genres** , und führen Sie dort einige Aktionen aus, z. b. Durchsuchen eines verfügbaren Albums.
4. Navigieren Sie zu **/ActionLog** , und wenn das Protokoll leer ist, drücken Sie **F5** , um die Seite zu aktualisieren. Überprüfen Sie, ob Ihre Besuche nachverfolgt wurden:

    ![Aktionsprotokoll mit protokollierter Aktivität](aspnet-mvc-4-custom-action-filters/_static/image4.png "Aktionsprotokoll mit protokollierter Aktivität")

    *Aktionsprotokoll mit protokollierter Aktivität*

<a id="Exercise2"></a>

<a id="Exercise_2_Managing_Multiple_Action_Filters"></a>
### <a name="exercise-2-managing-multiple-action-filters"></a>Übung 2: Verwalten mehrerer Aktionsfilter

In dieser Übung fügen Sie der StoreController-Klasse einen zweiten benutzerdefinierten Aktions Filter hinzu und definieren die jeweilige Reihenfolge, in der beide Filter ausgeführt werden. Anschließend aktualisieren Sie den Code, um den Filter Global zu registrieren.

Beim Definieren der Ausführungsreihenfolge der Filter sind unterschiedliche Optionen zu berücksichtigen. Zum Beispiel die Order-Eigenschaft und der Filterbereich:

Sie können einen **Bereich** für jeden Filter definieren. Sie können z. b. alle Aktionsfilter so festlegen, dass Sie innerhalb des **Controller Bereichs**ausgeführt werden, und alle Autorisierungs Filter, die im globalen Gültigkeits **Bereich**ausgeführt werden. Die Bereiche haben eine definierte Ausführungsreihenfolge.

Außerdem verfügt jeder Aktionsfilter über eine Order-Eigenschaft, die verwendet wird, um die Ausführungsreihenfolge im Filterbereich zu bestimmen.

Weitere Informationen zur Ausführung von benutzerdefinierten Aktions Filtern finden Sie in diesem MSDN-Artikel: ([https://msdn.microsoft.com/library/dd381609(v=vs.98).aspx](https://msdn.microsoft.com/library/dd381609(v=vs.98).aspx)).

<a id="Ex2Task1"></a>

<a id="Task_1_Creating_a_new_Custom_Action_Filter"></a>
#### <a name="task-1-creating-a-new-custom-action-filter"></a>Aufgabe 1: Erstellen eines neuen benutzerdefinierten Aktions Filters

In dieser Aufgabe erstellen Sie einen neuen benutzerdefinierten Aktions Filter, der in die StoreController-Klasse eingefügt werden soll, und erfahren, wie Sie die Ausführungsreihenfolge der Filter verwalten.

1. Öffnen Sie die Projekt Mappe " **Begin** " im Ordner " **\source\ex02-managingmultipleaktionfilters\begin** ". Andernfalls können Sie die **End** -Projekt Mappe, die Sie durch Abschließen der vorherigen Übung erhalten haben, weiterhin verwenden.

    1. Wenn Sie die bereitgestellte **Begin** -Lösung geöffnet haben, müssen Sie einige fehlende nuget-Pakete herunterladen, bevor Sie den Vorgang fortsetzen. Klicken Sie hierzu auf das Menü **Projekt** , und wählen Sie **nuget-Pakete verwalten**aus.
    2. Klicken Sie im Dialogfeld **nuget-Pakete verwalten** auf **Wiederherstellen** , um fehlende Pakete herunterzuladen.
    3. Erstellen Sie abschließend die Projekt Mappe, indem Sie auf **Build** ** | Projekt Mappe erstellen klicken**.

        > [!NOTE]
        > Einer der Vorteile der Verwendung von nuget besteht darin, dass Sie nicht alle Bibliotheken in Ihrem Projekt liefern müssen, um die Projektgröße zu verringern. Mit nuget Power Tools können Sie durch Angabe der Paketversionen in der Datei "Packages. config" alle erforderlichen Bibliotheken herunterladen, wenn Sie das Projekt zum ersten Mal ausführen. Aus diesem Grund müssen Sie diese Schritte ausführen, nachdem Sie eine vorhandene Projekt Mappe in diesem Lab geöffnet haben.
        > 
        > Weitere Informationen finden Sie in diesem Artikel: [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).
2. Fügen Sie dem C# Ordner **Filters** eine neue Klasse hinzu, und nennen Sie Sie *MyNewCustomActionFilter.cs*
3. Öffnen Sie **MyNewCustomActionFilter.cs** , und fügen Sie einen Verweis auf **System. Web. MVC** und den **mvcmusicstore. Models** -Namespace hinzu:

    (Code Ausschnitt- *ASP.NET MVC 4 benutzerdefinierte Aktionsfilter-EX2-mynewcustomaktionfilternamespaces*)

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample8.cs)]
4. Ersetzen Sie die Standardklassen Deklaration durch den folgenden Code.

    (Code Ausschnitt- *ASP.NET MVC 4 benutzerdefinierte Aktionsfilter-EX2-mynewcustomaction filterClass*)

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample9.cs)]

    > [!NOTE]
    > Dieser benutzerdefinierte Aktions Filter ist beinahe identisch mit dem Filter, den Sie in der vorherigen Übung erstellt haben. Der Hauptunterschied besteht darin, dass die *&quot;von&quot;* Attribut mit diesem neuen Klassennamen protokolliert wurde, um zu ermitteln, welcher Filter das Protokoll registriert hat.

<a id="Ex2Task2"></a>

<a id="Task_2_Injecting_a_new_Code_Interceptor_into_the_StoreController_Class"></a>
#### <a name="task-2-injecting-a-new-code-interceptor-into-the-storecontroller-class"></a>Aufgabe 2: Einfügen eines neuen Code Interceptors in die StoreController-Klasse

In dieser Aufgabe fügen Sie einen neuen benutzerdefinierten Filter in die StoreController-Klasse ein und führen die Projekt Mappe aus, um zu überprüfen, wie beide Filter zusammenarbeiten.

1. Öffnen Sie die **StoreController** -Klasse unter **mvcmusicstore\controllers** , und fügen Sie den neuen benutzerdefinierten Filter **mynewcustomaktionfilter** in die **StoreController** -Klasse ein, wie im folgenden Code gezeigt.

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample10.cs)]
2. Führen Sie nun die Anwendung aus, um zu sehen, wie diese beiden benutzerdefinierten Aktionsfilter funktionieren. Drücken Sie dazu **F5** , und warten Sie, bis die Anwendung gestartet wird.
3. Navigieren Sie zu **/ActionLog** , um den Anfangszustand der Protokoll Ansicht anzuzeigen.

    ![Protokoll verfolgungsstatus vor Seiten Aktivität](aspnet-mvc-4-custom-action-filters/_static/image5.png "Protokoll verfolgungsstatus vor Seiten Aktivität")

    *Protokoll verfolgungsstatus vor Seiten Aktivität*
4. Klicken Sie im Menü auf eines der **Genres** , und führen Sie dort einige Aktionen aus, z. b. Durchsuchen eines verfügbaren Albums.
5. Überprüfen Sie dieses Mal. Ihre Besuche wurden zweimal nachverfolgt: einmal für jeden benutzerdefinierten Aktionsfilter, den Sie in der **storagecontroller** -Klasse hinzugefügt haben.

    ![Aktionsprotokoll mit protokollierter Aktivität](aspnet-mvc-4-custom-action-filters/_static/image6.png "Aktionsprotokoll mit protokollierter Aktivität")

    *Aktionsprotokoll mit protokollierter Aktivität*
6. Schließen Sie den Browser.

<a id="Ex2Task3"></a>

<a id="Task_3_Managing_Filter_Ordering"></a>
#### <a name="task-3-managing-filter-ordering"></a>Aufgabe 3: Verwalten der Filter Reihenfolge

In dieser Aufgabe erfahren Sie, wie Sie die Ausführungsreihenfolge der Filter mithilfe der Eigenschaft Order verwalten.

1. Öffnen Sie die **StoreController** -Klasse unter **mvcmusicstore\controllers** , und geben Sie die **Order** -Eigenschaft in beiden Filtern an, wie unten gezeigt.

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample11.cs)]
2. Überprüfen Sie nun, wie die Filter ausgeführt werden, abhängig vom Wert der Order-Eigenschaft. Sie werden feststellen, dass der Filter mit dem kleinsten Bestellwert (**customaktionfilter**) der erste ist, der ausgeführt wird. Drücken Sie **F5** , und warten Sie, bis die Anwendung gestartet wird.
3. Navigieren Sie zu **/ActionLog** , um den Anfangszustand der Protokoll Ansicht anzuzeigen.

    ![Protokoll verfolgungsstatus vor Seiten Aktivität](aspnet-mvc-4-custom-action-filters/_static/image7.png "Protokoll verfolgungsstatus vor Seiten Aktivität")

    *Protokoll verfolgungsstatus vor Seiten Aktivität*
4. Klicken Sie im Menü auf eines der **Genres** , und führen Sie dort einige Aktionen aus, z. b. Durchsuchen eines verfügbaren Albums.
5. Überprüfen Sie, ob die Besuche dieses Zeitraums nachverfolgt wurden, geordnet nach dem Reihenfolge Wert der Filter: **customaktionfilter** Logs.

    ![Aktionsprotokoll mit protokollierter Aktivität](aspnet-mvc-4-custom-action-filters/_static/image8.png "Aktionsprotokoll mit protokollierter Aktivität")

    *Aktionsprotokoll mit protokollierter Aktivität*
6. Nun aktualisieren Sie den Reihenfolge Wert der Filter und überprüfen, wie sich die Protokollierungs Reihenfolge ändert. Aktualisieren Sie in der **StoreController** -Klasse den Wert für die Reihenfolge der Filter, wie unten gezeigt.

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample12.cs)]
7. Führen Sie die Anwendung erneut aus, indem Sie **F5**drücken.
8. Klicken Sie im Menü auf eines der **Genres** , und führen Sie dort einige Aktionen aus, z. b. Durchsuchen eines verfügbaren Albums.
9. Überprüfen Sie, ob die vom **mynewcustomaktionfilter** -Filter erstellten Protokolle zuerst angezeigt werden.

    ![Aktionsprotokoll mit protokollierter Aktivität](aspnet-mvc-4-custom-action-filters/_static/image9.png "Aktionsprotokoll mit protokollierter Aktivität")

    *Aktionsprotokoll mit protokollierter Aktivität*

<a id="Ex2Task4"></a>

<a id="Task_4_Registering_Filters_Globally"></a>
#### <a name="task-4-registering-filters-globally"></a>Aufgabe 4: globales Registrieren von Filtern

In dieser Aufgabe aktualisieren Sie die Projekt Mappe so, dass der neue Filter (**mynewcustomaktionfilter**) als globaler Filter registriert wird. Auf diese Weise wird Sie von allen Aktionen ausgelöst, die in der Anwendung ausgeführt werden, und nicht nur in den StoreController-Anwendungen, wie Sie in der vorherigen Aufgabe ausgeführt wurden.

1. Entfernen Sie in der **StoreController** -Klasse das Attribut **[mynewcustomaktionfilter]** und die Order-Eigenschaft von **[customaktionfilter]** . Dieser sollte wie folgt aussehen:

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample13.cs)]
2. Öffnen Sie die Datei **Global. asax** , und suchen Sie die **Anwendung\_Start** -Methode. Beachten Sie, dass jedes Mal, wenn die Anwendung gestartet wird, die globalen Filter durch Aufrufen der **registerglobalfilters** -Methode innerhalb der Klasse **FilterConfig** registriert werden.

    ![Registrieren globaler Filter in "Global. asax"](aspnet-mvc-4-custom-action-filters/_static/image10.png "Registrieren globaler Filter in "Global. asax"")

    *Registrieren globaler Filter in "Global. asax"*
3. Öffnen Sie die Datei **FilterConfig.cs** im **App-\_Ordner Start** .
4. Fügen Sie mithilfe von System. Web. MVC; einen Verweis auf hinzu. Verwenden von mvcmusicstore. Filters; Namespace.

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample14.cs)]
5. Aktualisieren der **registerglobalfilters** -Methode hinzufügen des benutzerdefinierten Filters. Fügen Sie zu diesem Zweck den hervorgehobenen Code hinzu:

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample15.cs)]
6. Drücken Sie **F5**, um die Anwendung auszuführen.
7. Klicken Sie im Menü auf eines der **Genres** , und führen Sie dort einige Aktionen aus, z. b. Durchsuchen eines verfügbaren Albums.
8. Überprüfen Sie, ob jetzt **[mynewcustomaktionfilter]** in HomeController und aktionlogcontroller eingefügt wird.

    ![Aktionsprotokoll mit protokollierter Aktivität](aspnet-mvc-4-custom-action-filters/_static/image11.png "Aktionsprotokoll mit protokollierter Aktivität")

    *Aktionsprotokoll mit protokollierter globaler Aktivität*

> [!NOTE]
> Außerdem können Sie diese Anwendung in Windows Azure-Websites bereitstellen, [indem Sie Anhang B: Veröffentlichen einer ASP.NET MVC 4-Anwendung mit Web deploy verwenden](#AppendixB).

---

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a>Zusammenfassung

Durch die Durchführung dieses praktischen Labs haben Sie gelernt, wie Sie einen Aktionsfilter zum Ausführen von benutzerdefinierten Aktionen erweitern. Sie haben auch gelernt, wie Sie einen beliebigen Filter an Ihre Seiten Controller einfügen. Die folgenden Konzepte wurden verwendet:

- Erstellen von benutzerdefinierten Aktions Filtern mit der ASP.NET MVC Action Filter Attribute-Klasse
- Einfügen von Filtern in ASP.NET-MVC-Controller
- Verwalten der Filter Reihenfolge mit der Order-Eigenschaft
- Globales Registrieren von Filtern

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>Anhang A: Installieren von Visual Studio Express 2012 für das Web

Sie können **Microsoft Visual Studio Express 2012 für Web** oder eine andere &quot;Express&quot; Version mit dem **[Microsoft-Webplattform-Installer](https://www.microsoft.com/web/downloads/platform.aspx)** installieren. Die folgenden Anweisungen führen Sie durch die Schritte, die zum Installieren *von Visual Studio Express 2012 für Web* mithilfe von *Microsoft-Webplattform-Installer*erforderlich sind.

1. Wechseln Sie zu [https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169). Wenn Sie den Webplattform-Installer bereits installiert haben, können Sie ihn auch öffnen und nach dem Produkt &quot;<em>Visual Studio Express 2012 für das Web mit dem Windows Azure SDK</em>&quot;suchen.
2. Klicken Sie auf **jetzt installieren**. Wenn Sie nicht über einen **Webplattform-Installer** verfügen, werden Sie umgeleitet, um Sie zuerst herunterzuladen und zu installieren.
3. Nachdem der **Webplattform-Installer** geöffnet ist, klicken Sie auf **Installieren** , um das Setup zu starten.

    ![Installieren von Visual Studio Express](aspnet-mvc-4-custom-action-filters/_static/image12.png "Installieren von Visual Studio Express")

    *Installieren von Visual Studio Express*
4. Lesen Sie die Lizenzbedingungen für alle Produkte, und klicken Sie auf **ich** Stimme zu, um fortzufahren.

    ![Akzeptieren der Lizenzbedingungen](aspnet-mvc-4-custom-action-filters/_static/image13.png)

    *Akzeptieren der Lizenzbedingungen*
5. Warten Sie, bis der Download-und Installationsvorgang abgeschlossen ist.

    ![Installationsstatus](aspnet-mvc-4-custom-action-filters/_static/image14.png)

    *Installationsfortschritt*
6. Wenn die Installation abgeschlossen ist, klicken Sie auf **Fertig**stellen.

    ![Installation abgeschlossen](aspnet-mvc-4-custom-action-filters/_static/image15.png)

    *Installation abgeschlossen*
7. Klicken Sie zum Schließen des Webplattform-Installers auf **Beenden** .
8. Um Visual Studio Express für das Web zu öffnen, navigieren Sie zum **Start** Bildschirm, und beginnen Sie mit dem Schreiben &quot;**vs Express**&quot;und klicken Sie dann auf die Kachel **vs Express für Web** .

    ![VS Express für Web-Kachel](aspnet-mvc-4-custom-action-filters/_static/image16.png)

    *VS Express für Web-Kachel*

<a id="AppendixB"></a>

<a id="Appendix_B_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-b-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Anhang B: Veröffentlichen einer ASP.NET MVC 4-Anwendung mit Web deploy

In diesem Anhang wird gezeigt, wie Sie eine neue Website aus dem Windows Azure-Verwaltungsportal erstellen und die Anwendung veröffentlichen, die Sie durch die folgenden Schritte abgerufen haben. nutzen Sie dabei die von Windows Azure bereitgestellte Funktion zur Web deploy Veröffentlichung.

<a id="ApxBTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-windows-azure-portal"></a>Aufgabe 1: Erstellen einer neuen Website über das Windows Azure-Portal

1. Wechseln Sie zum [Windows Azure-Verwaltungsportal](https://manage.windowsazure.com/) und melden Sie sich mit den Microsoft-Anmelde Informationen an, die Ihrem Abonnement zugeordnet sind.

    > [!NOTE]
    > Mit Windows Azure können Sie 10 ASP.NET Websites kostenlos hosten und dann skalieren, wenn Ihr Datenverkehr zunimmt. Sie können sich [hier](https://aka.ms/aspnet-hol-azure)registrieren.

    ![Anmelden bei Windows Azure-Portal](aspnet-mvc-4-custom-action-filters/_static/image17.png "Anmelden bei Windows Azure-Portal")

    *Anmelden bei Windows Azure Verwaltungsportal*
2. Klicken Sie in der Befehlsleiste auf **neu** .

    ![Erstellen einer neuen Website](aspnet-mvc-4-custom-action-filters/_static/image18.png "Erstellen einer neuen Website")

    *Erstellen einer neuen Website*
3. Klicken Sie auf **Compute** - | **Website**. Wählen Sie dann **schneller** Fassung aus. Geben Sie eine verfügbare URL für die neue Website an, und klicken Sie auf **Website erstellen**.

    > [!NOTE]
    > Eine Windows Azure-Website ist der Host für eine Webanwendung, die in der Cloud ausgeführt wird und die Sie steuern und verwalten können. Mit der Option schneller Fassung können Sie eine abgeschlossene Webanwendung von außerhalb des Portals auf der Windows Azure-Website bereitstellen. Sie enthält keine Schritte zum Einrichten einer Datenbank.

    ![Erstellen einer neuen Website mithilfe der schneller Fassung](aspnet-mvc-4-custom-action-filters/_static/image19.png "Erstellen einer neuen Website mithilfe der schneller Fassung")

    *Erstellen einer neuen Website mithilfe der schneller Fassung*
4. Warten Sie, bis die neue **Website** erstellt wurde.
5. Nachdem die Website erstellt wurde, klicken Sie auf den Link unter der **URL** -Spalte. Überprüfen Sie, ob die neue Website funktioniert.

    ![Navigieren zur neuen Website](aspnet-mvc-4-custom-action-filters/_static/image20.png "Navigieren zur neuen Website")

    *Navigieren zur neuen Website*

    ![Website wird ausgeführt](aspnet-mvc-4-custom-action-filters/_static/image21.png "Website wird ausgeführt")

    *Website wird ausgeführt*
6. Wechseln Sie zurück zum Portal, und klicken Sie in der Spalte **Name** auf den Namen der Website, um die Verwaltungs Seiten anzuzeigen.

    ![Öffnen der Website-Verwaltungs Seiten](aspnet-mvc-4-custom-action-filters/_static/image22.png "Öffnen der Website-Verwaltungs Seiten")

    *Öffnen der Website-Verwaltungs Seiten*
7. Klicken Sie auf der Seite **Dashboard** im Abschnitt **kurzer Blick** auf den Link **Veröffentlichungs Profil herunterladen** .

    > [!NOTE]
    > Das *Veröffentlichungs Profil* enthält alle Informationen, die zum Veröffentlichen einer Webanwendung auf einer Windows Azure-Website für die einzelnen aktivierten Veröffentlichungs Methoden erforderlich sind. Das Veröffentlichungs Profil enthält die URLs, Benutzer Anmelde Informationen und Daten Bank Zeichenfolgen, die erforderlich sind, um eine Verbindung mit den einzelnen Endpunkten herzustellen und diese zu authentifizieren, für die eine Veröffentlichungs Methode aktiviert ist. **Microsoft webmatrix 2**, **Microsoft Visual Studio Express für Web** und **Microsoft Visual Studio 2012** unterstützen das Lesen von Veröffentlichungs Profilen, um die Konfiguration dieser Programme für das Veröffentlichen von Webanwendungen auf Windows Azure-Websites zu automatisieren.

    ![Das Website-Veröffentlichungs Profil wird heruntergeladen.](aspnet-mvc-4-custom-action-filters/_static/image23.png "Das Website-Veröffentlichungs Profil wird heruntergeladen.")

    *Das Website-Veröffentlichungs Profil wird heruntergeladen.*
8. Laden Sie die Veröffentlichungs Profil Datei an einen bekannten Speicherort herunter. In dieser Übung erfahren Sie, wie Sie diese Datei zum Veröffentlichen einer Webanwendung auf Windows Azure-Websites in Visual Studio verwenden.

    ![Die Veröffentlichungs Profil Datei wird gespeichert.](aspnet-mvc-4-custom-action-filters/_static/image24.png "Das Veröffentlichungs Profil wird gespeichert.")

    *Die Veröffentlichungs Profil Datei wird gespeichert.*

<a id="ApxBTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a>Aufgabe 2: Konfigurieren des Datenbankservers

Wenn Ihre Anwendung SQL Server Datenbanken nutzt, müssen Sie einen SQL-Daten Bank Server erstellen. Wenn Sie eine einfache Anwendung bereitstellen möchten, die SQL Server nicht verwendet, können Sie diese Aufgabe überspringen.

1. Sie benötigen einen SQL-Datenbankserver zum Speichern der Anwendungsdatenbank. Sie können die SQL-Datenbankserver aus Ihrem Abonnement im Windows Azure-Verwaltungs Portal unter **SQL-Datenbanken** | **Server** | **Dashboard des Servers**anzeigen. Wenn Sie keinen Server erstellt haben, können Sie ihn mithilfe der Schaltfläche **Hinzufügen** auf der Befehlsleiste erstellen. Notieren Sie sich den **Servernamen und die URL, den Administrator Anmelde Namen und das Kennwort**, da Sie Sie in den nächsten Aufgaben verwenden werden. Erstellen Sie die Datenbank noch nicht, da Sie in einer späteren Phase erstellt wird.

    ![Dashboard des SQL-Datenbankservers](aspnet-mvc-4-custom-action-filters/_static/image25.png "Dashboard des SQL-Datenbankservers")

    *Dashboard des SQL-Datenbankservers*
2. In der nächsten Aufgabe testen Sie die Datenbankverbindung aus Visual Studio. aus diesem Grund müssen Sie Ihre lokale IP-Adresse in die Liste der **zulässigen IP-Adressen**des Servers einschließen. Klicken Sie hierzu auf **Konfigurieren**, wählen Sie die IP-Adresse der **aktuellen Client-IP-Adresse** aus, und fügen Sie Sie in die Textfelder Start-IP- **Adresse** und End- **IP-Adresse** ein, und klicken Sie auf die Schaltfläche ![Add-Client-IP-Address-OK-Button](aspnet-mvc-4-custom-action-filters/_static/image26.png)

    ![Client-IP-Adresse](aspnet-mvc-4-custom-action-filters/_static/image27.png)

    *Client-IP-Adresse*
3. Sobald die **Client-IP-Adresse** der Liste zulässige IP-Adressen hinzugefügt wurde, klicken Sie auf **Speichern** , um die Änderungen zu bestätigen.

    ![Änderungen bestätigen](aspnet-mvc-4-custom-action-filters/_static/image28.png)

    *Änderungen bestätigen*

<a id="ApxBTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Aufgabe 3: Veröffentlichen einer ASP.NET MVC 4-Anwendung mit Web deploy

1. Kehren Sie zur ASP.NET MVC 4-Lösung zurück. Klicken Sie im **Projektmappen-Explorer**mit der rechten Maustaste auf das Website Projekt, und wählen Sie **veröffentlichen**aus.

    ![Veröffentlichen der Anwendung](aspnet-mvc-4-custom-action-filters/_static/image29.png "Veröffentlichen der Anwendung")

    *Veröffentlichen der Website*
2. Importieren Sie das Veröffentlichungs Profil, das Sie in der ersten Aufgabe gespeichert haben.

    ![Importieren des Veröffentlichungs Profils](aspnet-mvc-4-custom-action-filters/_static/image30.png "Importieren des Veröffentlichungs Profils")

    *Veröffentlichungs Profil wird importiert.*
3. Klicken Sie auf **Verbindung**überprüfen. Klicken Sie nach Abschluss der Überprüfung auf **weiter**.

    > [!NOTE]
    > Die Überprüfung ist fertiggestellt, sobald ein grünes Häkchen neben der Schaltfläche Verbindung überprüfen angezeigt wird.

    ![Die Verbindung wird überprüft.](aspnet-mvc-4-custom-action-filters/_static/image31.png "Die Verbindung wird überprüft.")

    *Die Verbindung wird überprüft.*
4. Klicken Sie auf der Seite **Einstellungen** unter dem Abschnitt **Datenbanken** auf die Schaltfläche neben dem Textfeld der Datenbankverbindung (z. b. **DefaultConnection**).

    ![Webbereitstellungs Konfiguration](aspnet-mvc-4-custom-action-filters/_static/image32.png "Webbereitstellungs Konfiguration")

    *Webbereitstellungs Konfiguration*
5. Konfigurieren Sie die Datenbankverbindung wie folgt:

   - Geben Sie unter **Server Name** die URL des SQL-Datenbankservers unter Verwendung des *TCP:* -Präfix ein.
   - Geben Sie unter **Benutzername** den Anmelde Namen des Server Administrators ein.
   - Geben Sie unter **Kennwort** Ihren Server Administrator-Anmelde Kennwort ein.
   - Geben Sie einen neuen Datenbanknamen ein.

     ![Konfigurieren der Ziel Verbindungs Zeichenfolge](aspnet-mvc-4-custom-action-filters/_static/image33.png "Konfigurieren der Ziel Verbindungs Zeichenfolge")

     *Konfigurieren der Ziel Verbindungs Zeichenfolge*
6. Klicken Sie dann auf **OK**. Wenn Sie zum Erstellen der Datenbank aufgefordert werden, klicken Sie auf **Ja**.

    ![Erstellen der Datenbank](aspnet-mvc-4-custom-action-filters/_static/image34.png "Erstellen der Daten Bank Zeichenfolge")

    *Erstellen der Datenbank*
7. Die Verbindungs Zeichenfolge, die Sie zum Herstellen einer Verbindung mit der SQL-Datenbank in Windows Azure verwenden, wird im Textfeld Standardverbindung angezeigt. Klicken Sie dann auf **Weiter**.

    ![Verbindungs Zeichenfolge mit Verweis auf SQL-Datenbank](aspnet-mvc-4-custom-action-filters/_static/image35.png "Verbindungs Zeichenfolge mit Verweis auf SQL-Datenbank")

    *Verbindungs Zeichenfolge mit Verweis auf SQL-Datenbank*
8. Klicken Sie auf der Seite **Vorschau** auf **veröffentlichen**.

    ![Veröffentlichen der Webanwendung](aspnet-mvc-4-custom-action-filters/_static/image36.png "Veröffentlichen der Webanwendung")

    *Veröffentlichen der Webanwendung*
9. Nachdem der Veröffentlichungs Vorgang abgeschlossen ist, wird die veröffentlichte Website in Ihrem Standardbrowser geöffnet.

<a id="AppendixC"></a>

<a id="Appendix_C_Using_Code_Snippets"></a>
## <a name="appendix-c-using-code-snippets"></a>Anhang C: Verwenden von Code Ausschnitten

Mit Code Ausschnitten haben Sie den gesamten Code, den Sie benötigen. Im Lab-Dokument werden Sie genau wissen, wann Sie Sie verwenden können, wie in der folgenden Abbildung dargestellt.

![Verwenden von Visual Studio-Code Ausschnitten zum Einfügen von Code in Ihr Projekt](aspnet-mvc-4-custom-action-filters/_static/image37.png "Verwenden von Visual Studio-Code Ausschnitten zum Einfügen von Code in Ihr Projekt")

*Verwenden von Visual Studio-Code Ausschnitten zum Einfügen von Code in Ihr Projekt*

***So fügen Sie einen Code Ausschnitt mithilfe der Tastatur hinzuC# (nur)***

1. Platzieren Sie den Cursor an der Stelle, an der Sie den Code einfügen möchten.
2. Beginnen Sie mit der Eingabe des Ausschnitt namens (ohne Leerzeichen oder Bindestriche).
3. Sehen Sie sich an, wie IntelliSense übereinstimmende Code Ausschnitte anzeigt.
4. Wählen Sie den richtigen Ausschnitt aus (oder geben Sie die Eingabe fort, bis der gesamte Name des Ausschnitts ausgewählt ist).
5. Drücken Sie zweimal die Tab-Taste, um den Ausschnitt an der Cursorposition einzufügen.

![Beginnen Sie mit der Eingabe des Ausschnitt namens.](aspnet-mvc-4-custom-action-filters/_static/image38.png "Beginnen Sie mit der Eingabe des Ausschnitt namens.")

*Beginnen Sie mit der Eingabe des Ausschnitt namens.*

![Drücken Sie TAB, um den markierten Ausschnitt auszuwählen.](aspnet-mvc-4-custom-action-filters/_static/image39.png "Drücken Sie TAB, um den markierten Ausschnitt auszuwählen.")

*Drücken Sie TAB, um den markierten Ausschnitt auszuwählen.*

![Drücken Sie erneut die Tab-Taste, und der Ausschnitt wird erweitert.](aspnet-mvc-4-custom-action-filters/_static/image40.png "Drücken Sie erneut die Tab-Taste, und der Ausschnitt wird erweitert.")

*Drücken Sie erneut die Tab-Taste, und der Ausschnitt wird erweitert.*

So ***fügen Sie einen Code Ausschnitt mithilfe der Maus hinzuC#(, Visual Basic und XML)*** 1. Klicken Sie mit der rechten Maustaste auf den Ort, an dem Sie den Code Ausschnitt einfügen möchten.

1. Wählen Sie **Ausschnitt einfügen** und dann **meine Code Ausschnitte**aus.
2. Wählen Sie den entsprechenden Code Ausschnitt aus der Liste aus, indem Sie darauf klicken.

![Klicken Sie mit der rechten Maustaste darauf, wo Sie den Code Ausschnitt einfügen möchten, und wählen Sie Ausschnitt einfügen aus.](aspnet-mvc-4-custom-action-filters/_static/image41.png "Klicken Sie mit der rechten Maustaste darauf, wo Sie den Code Ausschnitt einfügen möchten, und wählen Sie Ausschnitt einfügen aus.")

*Klicken Sie mit der rechten Maustaste darauf, wo Sie den Code Ausschnitt einfügen möchten, und wählen Sie Ausschnitt einfügen aus.*

![Wählen Sie den entsprechenden Code Ausschnitt aus der Liste aus, indem Sie darauf klicken.](aspnet-mvc-4-custom-action-filters/_static/image42.png "Wählen Sie den entsprechenden Code Ausschnitt aus der Liste aus, indem Sie darauf klicken.")

*Wählen Sie den entsprechenden Code Ausschnitt aus der Liste aus, indem Sie darauf klicken.*
