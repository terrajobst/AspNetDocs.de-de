---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-dependency-injection
title: ASP.NET MVC 4-Abhängigkeitsinjektion | Microsoft-Dokumentation
author: rick-anderson
description: 'Hinweis: Diese praktische Übungseinheit setzt voraus, dass Sie über grundlegende Kenntnisse der Filter ASP.NET MVC und ASP.NET MVC 4 verfügen. Wenn Sie noch keine ASP.NET MVC 4-Filter verwendet haben, wird empfohlen...'
ms.author: riande
ms.date: 02/18/2013
ms.assetid: 84c7baca-1c54-4c44-8f52-4282122d6acb
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-dependency-injection
msc.type: authoredcontent
ms.openlocfilehash: 15c9d4dcb9e2c6b9f6adf54d65d15737b32cca3b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78451821"
---
# <a name="aspnet-mvc-4-dependency-injection"></a>ASP.NET MVC 4 – Abhängigkeitsinjektion

vom [Web Camps-Team](https://twitter.com/webcamps)

[Webcamps-Trainingskit herunterladen](https://aka.ms/webcamps-training-kit)

Diese praktische Übung geht davon aus, dass Sie über grundlegende Kenntnisse der Filter **ASP.NET MVC** und **ASP.NET MVC 4**verfügen. Wenn Sie noch keine **ASP.NET MVC 4-Filter** verwendet haben, empfiehlt es sich, das praktische Lab **ASP.NET MVC Custom Action Filters** zu überspringen.

> [!NOTE]
> Der gesamte Beispielcode und die Code Ausschnitte sind im Web Camps-Trainingskit enthalten, das in den [Releases Microsoft-Web/webcamptrainingkit](https://aka.ms/webcamps-training-kit)verfügbar ist. Das für dieses Lab spezifische Projekt ist unter [ASP.NET MVC 4-Abhängigkeitsinjektion](https://github.com/Microsoft-Web/HOL-MVC4DependencyInjection)verfügbar.

Im **objektorientierten Programmier** Paradigma arbeiten Objekte in einem Zusammenarbeits Modell zusammen, in dem es Mitwirkende und Consumer gibt. Natürlich generiert dieses Kommunikationsmodell Abhängigkeiten zwischen Objekten und Komponenten und wird bei zunehmender Komplexität schwierig zu verwalten.

![Klassen Abhängigkeiten und Modell Komplexität](aspnet-mvc-4-dependency-injection/_static/image1.png "Klassen Abhängigkeiten und Modell Komplexität")

*Klassen Abhängigkeiten und Modell Komplexität*

Sie haben wahrscheinlich das **Factorymuster** und die Trennung zwischen der Schnittstelle und der Implementierung mithilfe von Diensten gehört, bei denen die Client Objekte oft für die Dienst Identifizierung verantwortlich sind.

Das Muster für die Abhängigkeitsinjektion ist eine bestimmte Implementierung der Inversion of Control. Die **Inversion of Control (IOC)** bedeutet, dass Objekte keine anderen Objekte erstellen, auf die Sie sich verlassen, um Ihre Arbeit zu erledigen. Stattdessen erhalten Sie die Objekte, die Sie benötigen, von einer externen Quelle (z. b. einer XML-Konfigurationsdatei).

Die **Abhängigkeitsinjektion (di)** bedeutet, dass dies ohne den Objekt Eingriff erfolgt, normalerweise durch eine frameworkkomponente, die Konstruktorparameter übergibt und Eigenschaften festgelegt.

<a id="The_Dependency_Injection_DI_Design_Pattern"></a>
### <a name="the-dependency-injection-di-design-pattern"></a>Das Entwurfsmuster für die Abhängigkeitsinjektion (di)

Auf hoher Ebene besteht das Ziel der Abhängigkeitsinjektion darin, dass eine Client Klasse (z. b. *der Golfer*) etwas benötigt, das eine Schnittstelle erfüllt (z. b. *iclub*). Es ist nicht wichtig, dass der konkrete Typ (z. b. *woodclub, ironclub, wedgeclub* oder *putterclub*) von einem anderen Benutzer behandelt wird (z. b. ein guter *Caddy*). Mit dem Abhängigkeits Konflikt Löser in ASP.NET MVC können Sie Ihre Abhängigkeits Logik an einer anderen Stelle registrieren (z. b. in einem Container oder einer Sammlung *von Schlägern*).

![Abhängigkeits Injection-Diagramm](aspnet-mvc-4-dependency-injection/_static/image2.png "Darstellung der Abhängigkeitsinjektion")

*Abhängigkeitsinjektion-Golf Analog*

Die Verwendung von Abhängigkeits Injektions Mustern und Inversion der Kontrolle bietet folgende Vorteile:

- Reduziert die Klassen Kopplung
- Erhöht die Wiederverwendung von Code.
- Verbesserte Code Wartbarkeit
- Verbessert das Testen von Anwendungen

> [!NOTE]
> Die Abhängigkeitsinjektion wird manchmal mit dem abstrakten Factoryentwurfsmuster verglichen, aber es gibt einen kleinen Unterschied zwischen beiden Ansätzen. Bei di gibt es ein Framework, mit dem Abhängigkeiten durch Aufrufen der Factorys und der registrierten Dienste gelöst werden können.

Nachdem Sie das Muster für die Abhängigkeitsinjektion verstanden haben, erfahren Sie in diesem Lab, wie Sie es in ASP.NET MVC 4 anwenden können. Sie beginnen mit der Verwendung der Abhängigkeitsinjektion in den **Controllern** , um einen Datenbankzugriffs Dienst einzubeziehen. Als nächstes wenden Sie eine Abhängigkeitsinjektion auf die **Sichten** an, um einen Dienst zu nutzen und Informationen anzuzeigen. Schließlich erweitern Sie den di-Filter auf ASP.NET MVC 4-Filter und in der Lösung einen benutzerdefinierten Aktionsfilter.

In dieser praktischen Übungseinheit erfahren Sie Folgendes:

- Integrieren von ASP.NET MVC 4 in Unity für die Abhängigkeitsinjektion mithilfe von nuget-Paketen
- Verwenden von Abhängigkeitsinjektion in einem ASP.NET-MVC-Controller
- Abhängigkeitsinjektion innerhalb einer ASP.NET-MVC-Ansicht verwenden
- Verwenden von Abhängigkeitsinjektion innerhalb eines ASP.NET MVC-Aktions Filters

> [!NOTE]
> Dieses Lab verwendet das Unity. Mvc3-nuget-Paket für die Abhängigkeitsauflösung, aber es ist möglich, jedes Framework für die Abhängigkeitsinjektion an ASP.NET MVC 4 anzupassen.

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

Wenn Sie mit den Visual Studio Code Ausschnitten nicht vertraut sind und wissen möchten, wie Sie Sie verwenden können, finden Sie den Anhang dieses Dokuments &quot;[Anhang B: Verwenden von Code Ausschnitten](#AppendixB)&quot;.

---

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a>Exerzitien

Diese praktische Übungseinheit besteht aus den folgenden Übungen:

1. [Übung 1: Einfügen eines Controllers](#Exercise1)
2. [Übung 2: Einfügen einer Ansicht](#Exercise2)
3. [Übung 3: Einfügen von Filtern](#Exercise3)

> [!NOTE]
> Jede Übung wird von einem **endordner** begleitet, der die sich ergebende Lösung enthält, die Sie nach Abschluss der Übungen erhalten. Sie können diese Lösung als Leitfaden verwenden, wenn Sie zusätzliche Hilfe beim Durcharbeiten der Übungen benötigen.

Geschätzte Zeit bis zum Abschluss dieses Labs: **30 Minuten**.

<a id="Exercise1"></a>

<a id="Exercise_1_Injecting_a_Controller"></a>
### <a name="exercise-1-injecting-a-controller"></a>Übung 1: Einfügen eines Controllers

In dieser Übung erfahren Sie, wie Sie die Abhängigkeitsinjektion in ASP.NET-MVC-Controllern verwenden, indem Sie Unity mithilfe eines nuget-Pakets integrieren. Aus diesem Grund werden Sie Dienste in Ihre mvcmusicstore-Controller einschließen, um die Logik vom Datenzugriff zu trennen. Die Dienste erstellen eine neue Abhängigkeit im controllerkonstruktor, die mithilfe von "Abhängigkeitsinjektion" mit der Hilfe von **Unity**aufgelöst wird.

Dieser Ansatz zeigt Ihnen, wie Sie weniger gekoppelte Anwendungen generieren, die flexibler und leichter zu verwalten und zu testen sind. Außerdem erfahren Sie, wie Sie ASP.NET MVC in Unity integrieren.

<a id="About_StoreManager_Service"></a>
#### <a name="about-storemanager-service"></a>Informationen zum StoreManager-Dienst

Der in der Lösung "BEGIN" bereitgestellte MVC Music Store enthält jetzt einen Dienst, der die Store Controller-Daten mit dem Namen " **storeservice**" verwaltet. Unten finden Sie die Store Service-Implementierung. Beachten Sie, dass alle Methoden Modell Entitäten zurückgeben.

[!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample1.cs)]

**StoreController** von der BEGIN-Lösung verwendet nun **storeservice**. Alle Daten Verweise wurden aus **StoreController**entfernt und können jetzt den aktuellen Datenzugriffs Anbieter ändern, ohne eine Methode zu ändern, die **storeservice**beansprucht.

Unten sehen Sie, dass die **StoreController** -Implementierung über eine Abhängigkeit mit **storeservice** innerhalb des Klassenkonstruktors verfügt.

> [!NOTE]
> Die in dieser Übung eingeführte Abhängigkeit bezieht sich auf die **Inversion of Control** (IOC).
> 
> Der **StoreController** -Klassenkonstruktor empfängt einen **istoreservice** -Typparameter, der für die Durchführung von Dienst aufrufen aus der-Klasse erforderlich ist. **StoreController** implementiert jedoch nicht den Standardkonstruktor (ohne Parameter), den jeder Controller für die Arbeit mit ASP.NET MVC benötigen muss.
> 
> Um die Abhängigkeit aufzulösen, muss der Controller von einer abstrakten Factory (einer Klasse, die ein beliebiges Objekt des angegebenen Typs zurückgibt) erstellt werden.

[!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample2.cs)]

> [!NOTE]
> Sie erhalten eine Fehlermeldung, wenn die Klasse versucht, StoreController zu erstellen, ohne das Dienst Objekt zu senden, da kein Parameter loser Konstruktor deklariert wurde.

<a id="Ex1Task1"></a>

<a id="Task_1_-_Running_the_Application"></a>
#### <a name="task-1---running-the-application"></a>Aufgabe 1: Ausführen der Anwendung

In dieser Aufgabe führen Sie die BEGIN-Anwendung aus, die den-Dienst in den Speicher Controller einschließt, der den Datenzugriff von der Anwendungslogik trennt.

Wenn Sie die Anwendung ausführen, erhalten Sie eine Ausnahme, da der Controller Dienst nicht standardmäßig als Parameter übergeben wird:

1. Öffnen Sie die Projekt Mappe " **Begin** " in **source\ex01-Input controller\begin**.

   1. Sie müssen einige fehlende nuget-Pakete herunterladen, bevor Sie den Vorgang fortsetzen. Klicken Sie hierzu auf das Menü **Projekt** , und wählen Sie **nuget-Pakete verwalten**aus.
   2. Klicken Sie im Dialogfeld **nuget-Pakete verwalten** auf **Wiederherstellen** , um fehlende Pakete herunterzuladen.
   3. Erstellen Sie abschließend die Projekt Mappe, indem Sie auf **Build** ** | Projekt Mappe erstellen klicken**.

      > [!NOTE]
      > Einer der Vorteile der Verwendung von nuget besteht darin, dass Sie nicht alle Bibliotheken in Ihrem Projekt liefern müssen, um die Projektgröße zu verringern. Mit nuget Power Tools können Sie durch Angabe der Paketversionen in der Datei "Packages. config" alle erforderlichen Bibliotheken herunterladen, wenn Sie das Projekt zum ersten Mal ausführen. Aus diesem Grund müssen Sie diese Schritte ausführen, nachdem Sie eine vorhandene Projekt Mappe in diesem Lab geöffnet haben.
2. Drücken Sie **STRG + F5** , um die Anwendung ohne Debuggen auszuführen. Sie erhalten die Fehlermeldung, &quot;**kein Parameter loser Konstruktor für dieses Objekt&quot;definiert** ist:

    ![Fehler beim Ausführen der ASP.NET MVC-BEGIN-Anwendung.](aspnet-mvc-4-dependency-injection/_static/image3.png "Fehler beim Ausführen der ASP.NET MVC-BEGIN-Anwendung.")

    *Fehler beim Ausführen der ASP.NET MVC-BEGIN-Anwendung.*
3. Schließen Sie den Browser.

In den folgenden Schritten arbeiten Sie mit der Music Store-Projekt Mappe zusammen, um die Abhängigkeit, die dieser Controller benötigt, einzufügen.

<a id="Ex1Task2"></a>

<a id="Task_2_-_Including_Unity_into_MvcMusicStore_Solution"></a>
#### <a name="task-2---including-unity-into-mvcmusicstore-solution"></a>Aufgabe 2: einschließen von Unity in eine mvcmusicstore-Lösung

In dieser Aufgabe fügen Sie das nuget-Paket " **Unity. Mvc3** " in die Projekt Mappe ein.

> [!NOTE]
> Das Unity. Mvc3-Paket wurde für ASP.NET MVC 3 entwickelt, ist aber vollständig mit ASP.NET MVC 4 kompatibel.
> 
> Unity ist ein schlanker, erweiterbarer Abhängigkeits Injection-Container mit optionaler Unterstützung für Instanzen und typabfang. Dabei handelt es sich um einen allgemeinen Container zur Verwendung in beliebigen .NET-Anwendungs Typen. Es bietet alle allgemeinen Features, die in Mechanismen für die Abhängigkeitsinjektion enthalten sind: Objekt Erstellung, Abstraktion von Anforderungen durch Angeben von Abhängigkeiten zur Laufzeit und Flexibilität, indem die Komponenten Konfiguration auf den Container verschoben wird.

1. Installieren Sie das nuget-Paket " **Unity. Mvc3** " im **mvcmusicstore** -Projekt. Öffnen Sie hierzu die Paket- **Manager-Konsole** in der **Ansicht** | **anderen Fenstern**.
2. Führen Sie den folgenden Befehl aus.

    PMC

    [!code-powershell[Main](aspnet-mvc-4-dependency-injection/samples/sample3.ps1)]

    ![Installieren des Unity. Mvc3-nuget-Pakets](aspnet-mvc-4-dependency-injection/_static/image4.png "Installieren des Unity. Mvc3-nuget-Pakets")

    *Installieren des Unity. Mvc3-nuget-Pakets*
3. Nachdem das **Unity. Mvc3** -Paket installiert wurde, untersuchen Sie die Dateien und Ordner, die automatisch hinzugefügt werden, um die Unity-Konfiguration zu vereinfachen.

    ![Unity. Mvc3-Paket installiert](aspnet-mvc-4-dependency-injection/_static/image5.png "Unity. Mvc3-Paket installiert")

    *Unity. Mvc3-Paket installiert*

<a id="Ex1Task3"></a>

<a id="Task_3_-_Registering_Unity_in_Globalasaxcs_Application_Start"></a>
#### <a name="task-3---registering-unity-in-globalasaxcs-application_start"></a>Aufgabe 3: Registrieren von Unity in der Global.asax.cs-Anwendung\_Start

In dieser Aufgabe aktualisieren Sie die **Anwendung\_Start** -Methode in **Global.asax.cs** , um den Unity-bootstrapperinitialisierer aufzurufen, und aktualisieren anschließend die Bootstrapperdatei, die den Dienst und den Controller registriert, den Sie für die Abhängigkeitsinjektion verwenden werden.

1. Nun verbinden Sie den Boots Trapper, der die Datei ist, die den Unity-Container und den Abhängigkeits Konflikt Löser initialisiert. Öffnen Sie hierzu **Global.asax.cs** , und fügen Sie den folgenden hervorgehobenen Code in der **Anwendung\_Start** -Methode hinzu.

    (Code Ausschnitt- *ASP.net-Abhängigkeitsinjektion Lab-Ex01-Initialize Unity*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample4.cs)]
2. Öffnen Sie die Datei **Bootstrapper.cs** .
3. Fügen Sie die folgenden Namespaces ein: **mvcmusicstore. Services** und **Musicstore. Controllers**.

    (Code Ausschnitt- *ASP.net-Abhängigkeitsinjektion Lab-Ex01-Bootstrapper Hinzufügen von Namespaces*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample5.cs)]
4. Ersetzen Sie den Inhalt der **buildunitycontainer** -Methode durch den folgenden Code, in dem Store Controller und Store Service registriert sind.

    (Code Ausschnitt- *ASP.net Abhängigkeitsinjektion Lab-Ex01-Store Controller und Dienst registrieren*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample6.cs)]

<a id="Ex1Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a>Aufgabe 4: Ausführen der Anwendung

In dieser Aufgabe führen Sie die Anwendung aus, um zu überprüfen, ob Sie jetzt nach dem einschließen von Unity geladen werden kann.

1. Drücken Sie **F5** , um die Anwendung auszuführen. die Anwendung sollte nun geladen werden, ohne dass eine Fehlermeldung angezeigt wird.

    ![Ausführen einer Anwendung mit Abhängigkeitsinjektion](aspnet-mvc-4-dependency-injection/_static/image6.png "Ausführen einer Anwendung mit Abhängigkeitsinjektion")

    *Ausführen einer Anwendung mit Abhängigkeitsinjektion*
2. Navigieren Sie zu **/Store**. Dadurch wird **StoreController**aufgerufen, das nun mithilfe von **Unity**erstellt wurde.

    ![MVC Music Store](aspnet-mvc-4-dependency-injection/_static/image7.png "MVC Music Store")

    *MVC Music Store*
3. Schließen Sie den Browser.

In den folgenden Übungen erfahren Sie, wie Sie den Bereich für die Abhängigkeitsinjektion erweitern, um ihn in ASP.NET MVC-Ansichten und-Aktions Filtern zu verwenden.

<a id="Exercise2"></a>

<a id="Exercise_2_Injecting_a_View"></a>
### <a name="exercise-2-injecting-a-view"></a>Übung 2: Einfügen einer Ansicht

In dieser Übung erfahren Sie, wie Sie die Abhängigkeitsinjektion in einer Ansicht mit den neuen Features von ASP.NET MVC 4 für die Unity-Integration verwenden. Dazu rufen Sie einen benutzerdefinierten Dienst in der Such Ansicht des Stores auf, in der eine Meldung und ein Bild unten angezeigt werden.

Anschließend integrieren Sie das Projekt in Unity und erstellen einen benutzerdefinierten Abhängigkeits Konflikt Löser zum Einfügen der Abhängigkeiten.

<a id="Ex2Task1"></a>

<a id="Task_1_-_Creating_a_View_that_Consumes_a_Service"></a>
#### <a name="task-1---creating-a-view-that-consumes-a-service"></a>Aufgabe 1: Erstellen einer Ansicht, die einen Dienst nutzt

In dieser Aufgabe erstellen Sie eine Ansicht, die einen Dienst aufzurufen, um eine neue Abhängigkeit zu generieren. Der Dienst besteht aus einem einfachen Messaging Dienst, der in dieser Lösung enthalten ist.

1. Öffnen Sie die Projekt Mappe " **Anfang** ", die sich im Ordner " **source\ex02-Input view\begin** " befindet. Andernfalls können Sie die **End** -Projekt Mappe, die Sie durch Abschließen der vorherigen Übung erhalten haben, weiterhin verwenden.

   1. Wenn Sie die bereitgestellte **Begin** -Lösung geöffnet haben, müssen Sie einige fehlende nuget-Pakete herunterladen, bevor Sie den Vorgang fortsetzen. Klicken Sie hierzu auf das Menü **Projekt** , und wählen Sie **nuget-Pakete verwalten**aus.
   2. Klicken Sie im Dialogfeld **nuget-Pakete verwalten** auf **Wiederherstellen** , um fehlende Pakete herunterzuladen.
   3. Erstellen Sie abschließend die Projekt Mappe, indem Sie auf **Build** ** | Projekt Mappe erstellen klicken**.

      > [!NOTE]
      > Einer der Vorteile der Verwendung von nuget besteht darin, dass Sie nicht alle Bibliotheken in Ihrem Projekt liefern müssen, um die Projektgröße zu verringern. Mit nuget Power Tools können Sie durch Angabe der Paketversionen in der Datei "Packages. config" alle erforderlichen Bibliotheken herunterladen, wenn Sie das Projekt zum ersten Mal ausführen. Aus diesem Grund müssen Sie diese Schritte ausführen, nachdem Sie eine vorhandene Projekt Mappe in diesem Lab geöffnet haben.
      > 
      > Weitere Informationen finden Sie in diesem Artikel: [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).
2. Schließen Sie die **MessageService.cs** -und **IMessageService.cs** -Klassen ein, die sich im Ordner " **Source \assets** " in **/Services**befinden. Klicken Sie hierzu mit der rechten Maustaste auf den Ordner **Dienste** , und wählen Sie **Vorhandenes Element hinzufügen**aus. Navigieren Sie zum Speicherort der Dateien, und fügen Sie Sie ein.

    ![Hinzufügen von Message Service und Dienst Schnittstelle](aspnet-mvc-4-dependency-injection/_static/image8.png "Hinzufügen von Message Service und Dienst Schnittstelle")

    *Hinzufügen von Message Service und Dienst Schnittstelle*

    > [!NOTE]
    > Die **imessageservice** -Schnittstelle definiert zwei Eigenschaften, die von der **MessageService** -Klasse implementiert werden. Diese Eigenschaften-**Message** und **ImageUrl**: Speichern Sie die Nachricht und die URL des anzuzeigenden Bilds.
3. Erstellen Sie den Ordner **/pages** im Stamm Ordner des Projekts, und fügen Sie dann die vorhandene Klasse **MyBasePage.cs** aus **source\assets**hinzu. Die Basis Seite, von der Sie erben, weist die folgende Struktur auf.

    ![Ordner "Pages"](aspnet-mvc-4-dependency-injection/_static/image9.png "Ordner „Seiten“")

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample7.cs)]
4. Öffnen Sie die Ansicht **Browse. cshtml** aus dem Ordner **/views/Store** , und legen Sie die Vererbung von **MyBasePage.cs**ab.

    [!code-cshtml[Main](aspnet-mvc-4-dependency-injection/samples/sample8.cshtml)]
5. Fügen Sie in der Ansicht **Durchsuchen** einen " **MessageService** "-Befehl hinzu, um ein Bild und eine vom Dienst abgerufene Nachricht anzuzeigen.
   (C#)

    [!code-cshtml[Main](aspnet-mvc-4-dependency-injection/samples/sample9.cshtml)]

<a id="Ex2Task2"></a>

<a id="Task_2_-_Including_a_Custom_Dependency_Resolver_and_a_Custom_View_Page_Activator"></a>
#### <a name="task-2---including-a-custom-dependency-resolver-and-a-custom-view-page-activator"></a>Aufgabe 2: Einschließen eines benutzerdefinierten Abhängigkeits Konflikt Lösers und einer benutzerdefinierten Ansichts Seiten Aktivierung

In der vorherigen Aufgabe haben Sie eine neue Abhängigkeit in eine Ansicht eingefügt, um darin einen Dienst aufzurufen. Nun lösen Sie diese Abhängigkeit durch Implementieren der ASP.NET MVC-Abhängigkeits Injektions Schnittstellen **iviewpageactivator** und **idetzdencyresolver**. Sie fügen in die Lösung eine Implementierung von **idepdencyresolver** ein, die den Dienst Abruf mithilfe von Unity behandelt. Anschließend fügen Sie eine weitere benutzerdefinierte Implementierung der **iviewpageactivator** -Schnittstelle ein, die die Erstellung der Sichten löst.

> [!NOTE]
> Seit ASP.NET MVC 3 hat die Implementierung für die Abhängigkeitsinjektion die Schnittstellen für die Registrierung von Diensten vereinfacht. **Idetzdencyresolver** und **iviewpageactivator** sind Teil der ASP.NET MVC 3-Funktionen für die Abhängigkeitsinjektion.
> 
> Die **idepdencyresolver-** Schnittstelle ersetzt den vorherigen imvcservicelocator. Implementierer von idepdencyresolver müssen eine Instanz des Dienstanbieter oder einer Dienst Auflistung zurückgeben.
> 
> 
> [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample10.cs)]
> 
> Die **iviewpageactivator-** Schnittstelle bietet eine präzisere Steuerung der instanziierten Ansichts Seiten mithilfe der Abhängigkeitsinjektion. Die Klassen, die die **iviewpageactivator** -Schnittstelle implementieren, können Ansichts Instanzen mithilfe von Kontextinformationen erstellen.
> 
> 
> [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample11.cs)]

1. Erstellen Sie den**Ordner/** Factorys im Stamm Ordner des Projekts.
2. Fügen Sie **CustomViewPageActivator.cs** aus dem Ordner **/Sources/Assets/** in Factorys **in Ihre** Lösung ein. Klicken Sie dazu mit der rechten Maustaste auf den Ordner **/Factories** , und wählen Sie **hinzufügen aus. Vorhandenes Element** , und wählen Sie dann **CustomViewPageActivator.cs**aus. Diese Klasse implementiert die **iviewpageactivator** -Schnittstelle für den Unity-Container.

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample12.cs)]

    > [!NOTE]
    > **Customviewpageactivator** ist für die Verwaltung der Erstellung einer Ansicht mit einem Unity-Container zuständig.
3. Fügen Sie die **UnityDependencyResolver.cs** -Datei aus **/Sources/Assets** in den Ordner **/Factories** ein. Klicken Sie dazu mit der rechten Maustaste auf den Ordner **/Factories** , und wählen Sie **hinzufügen aus. Vorhandenes Element** , und wählen Sie dann **UnityDependencyResolver.cs** File aus.

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample13.cs)]

    > [!NOTE]
    > Die **unitydependencyresolver** -Klasse ist ein benutzerdefinierter DependencyResolver für Unity. Wenn ein Dienst im Unity-Container nicht gefunden werden kann, wird der basisresolver aufgerufen.

In der folgenden Aufgabe werden beide Implementierungen registriert, damit das Modell den Speicherort der Dienste und Sichten kennt.

<a id="Ex2Task3"></a>

<a id="Task_3_-_Registering_for_Dependency_Injection_within_Unity_container"></a>
#### <a name="task-3---registering-for-dependency-injection-within-unity-container"></a>Aufgabe 3: Registrieren für die Abhängigkeitsinjektion innerhalb des Unity-Containers

In dieser Aufgabe werden alle vorherigen Elemente zusammengefasst, um die Abhängigkeitsinjektion zu ermöglichen.

Bis jetzt verfügt Ihre Lösung über die folgenden Elemente:

- Eine **browseinsicht** , die von **MyBaseClass** erbt und **MessageService**verwendet.
- Eine Zwischenklasse (**MyBaseClass**), für die eine Abhängigkeitsinjektion für die Dienst Schnittstelle deklariert wurde.
- Einen Service- **MessageService** -und seine Schnittstelle **imessageservice**.
- Ein benutzerdefinierter Abhängigkeits Konflikt Löser für Unity- **unitydependencyresolver** , der den Dienst Abruf behandelt.
- Eine Ansichts Seiten-Activator- **customviewpageactivator** -, die die Seite erstellt.

Zum Einfügen der **browseinsicht** registrieren Sie nun den benutzerdefinierten Abhängigkeits Konflikt Löser im Unity-Container.

1. Öffnen Sie die Datei **Bootstrapper.cs** .
2. Registrieren Sie eine Instanz von **MessageService** im Unity-Container, um den Dienst zu initialisieren:

    (Code Ausschnitt- *ASP.net-Abhängigkeitsinjektion Lab-Ex02-Register Message Service*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample14.cs)]
3. Fügen Sie einen Verweis auf den **mvcmusicstore.** factorynamespace hinzu.

    (Code Ausschnitt- *ASP.net-Abhängigkeitsinjektion Lab-Ex02-factorynamespace*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample15.cs)]
4. Registrieren Sie **customviewpageactivator** als Ansichts Seiten Aktivierung im Unity-Container:

    (Code Ausschnitt- *ASP.net-Abhängigkeitsinjektion Lab-Ex02-Register customviewpageactivator*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample16.cs)]
5. Ersetzen Sie ASP.NET MVC 4 Default-Abhängigkeits Konflikt Löser durch eine Instanz von **unitydependencyresolver**. Ersetzen Sie hierzu den **Initialisierungs** Methoden Inhalt durch den folgenden Code:

    (Code Ausschnitt- *ASP.net-Abhängigkeitsinjektion Lab-Ex02-Abhängigkeits*Konflikt Löser aktualisieren)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample17.cs)]

    > [!NOTE]
    > ASP.NET MVC stellt eine standardabhängigkeitsresolver-Klasse bereit. Um mit benutzerdefinierten Abhängigkeits Konflikt Löser zu arbeiten, die wir für Unity erstellt haben, muss dieser Konflikt Löser ersetzt werden.

<a id="Ex2Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a>Aufgabe 4: Ausführen der Anwendung

In dieser Aufgabe führen Sie die Anwendung aus, um zu überprüfen, ob der Store-Browser den Dienst verwendet und das Abbild und die abgerufene Nachricht angezeigt werden:

1. Drücken Sie **F5**, um die Anwendung auszuführen.
2. Klicken Sie im Menü Genres auf **Rock** , und sehen Sie sich an, wie der **MessageService** in die Ansicht eingefügt und die Willkommensnachricht und das Bild geladen wurde. In diesem Beispiel wechseln wir zu &quot;**Rock**&quot;:

    ![MVC Music Store-Einschleusung anzeigen](aspnet-mvc-4-dependency-injection/_static/image10.png "MVC Music Store-Einschleusung anzeigen")

    *MVC Music Store-Einschleusung anzeigen*
3. Schließen Sie den Browser.

<a id="Exercise3"></a>

<a id="Exercise_3_Injecting_Action_Filters"></a>
### <a name="exercise-3-injecting-action-filters"></a>Übung 3: Einfügen von Aktions Filtern

In den vorherigen praktischen Übungseinheiten für praktische Übungseinheiten **haben Sie mit** der Anpassung und Injektion von Filtern gearbeitet. In dieser Übung erfahren Sie, wie Sie mithilfe des Unity-Containers Filter mit Abhängigkeitsinjektion einfügen. Zu diesem Zweck fügen Sie der Music Store-Projekt Mappe einen benutzerdefinierten Aktionsfilter hinzu, der die Aktivität der Site verfolgt.

<a id="Ex3Task1"></a>

<a id="Task_1_-_Including_the_Tracking_Filter_in_the_Solution"></a>
#### <a name="task-1---including-the-tracking-filter-in-the-solution"></a>Aufgabe 1: einschließen des nach Verfolgungs Filters in der Lösung

In dieser Aufgabe fügen Sie in den Music Store einen benutzerdefinierten Aktionsfilter ein, um Ereignisse zu verfolgen. Da benutzerdefinierte Aktionsfilter Konzepte bereits in der vorherigen Lab-&quot;&quot;benutzerdefinierte Aktionsfilter behandelt werden, fügen Sie einfach die Filter-Klasse aus dem Ordner "Assets" dieses Labs ein, und erstellen Sie dann einen Filter Anbieter für Unity:

1. Öffnen Sie **die Projekt** Mappe "Start", die sich im Ordner " **source\ex03-einschleusung\begin** " befindet. Andernfalls können Sie die **End** -Projekt Mappe, die Sie durch Abschließen der vorherigen Übung erhalten haben, weiterhin verwenden.

   1. Wenn Sie die bereitgestellte **Begin** -Lösung geöffnet haben, müssen Sie einige fehlende nuget-Pakete herunterladen, bevor Sie den Vorgang fortsetzen. Klicken Sie hierzu auf das Menü **Projekt** , und wählen Sie **nuget-Pakete verwalten**aus.
   2. Klicken Sie im Dialogfeld **nuget-Pakete verwalten** auf **Wiederherstellen** , um fehlende Pakete herunterzuladen.
   3. Erstellen Sie abschließend die Projekt Mappe, indem Sie auf **Build** ** | Projekt Mappe erstellen klicken**.

      > [!NOTE]
      > Einer der Vorteile der Verwendung von nuget besteht darin, dass Sie nicht alle Bibliotheken in Ihrem Projekt liefern müssen, um die Projektgröße zu verringern. Mit nuget Power Tools können Sie durch Angabe der Paketversionen in der Datei "Packages. config" alle erforderlichen Bibliotheken herunterladen, wenn Sie das Projekt zum ersten Mal ausführen. Aus diesem Grund müssen Sie diese Schritte ausführen, nachdem Sie eine vorhandene Projekt Mappe in diesem Lab geöffnet haben.
      > 
      > Weitere Informationen finden Sie in diesem Artikel: [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).
2. Fügen Sie die **TraceActionFilter.cs** -Datei aus **/Sources/Assets** in den Ordner **/Filters** ein.

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample18.cs)]

    > [!NOTE]
    > Dieser benutzerdefinierte Aktionsfilter führt ASP.net-Ablauf Verfolgung aus. Sie können &quot;ASP.NET MVC 4-Filter für lokale und dynamische Aktionen&quot; Lab überprüfen, um weitere Informationen zu erhalten.
3. Fügen Sie dem Projekt im Ordner **/Filters.** die leere **FilterProvider.cs** -Klasse hinzu.
4. Fügen Sie die Namespaces **System. Web. MVC** und **Microsoft. Practices. unity** in **FilterProvider.cs**hinzu.

    (Code Ausschnitt- *ASP.net-Abhängigkeitsinjektion Lab-Ex03-Filter Anbieter hinzufügen von Namespaces*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample19.cs)]
5. Erbt die Klasse von der **ifilterprovider** -Schnittstelle.

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample20.cs)]
6. Fügen Sie in der Klasse **filterprovider** eine **IUnityContainer** -Eigenschaft hinzu, und erstellen Sie dann einen Klassenkonstruktor, um den Container zuzuweisen.

    (Code Ausschnitt- *ASP.net-Abhängigkeitsinjektion Lab-Ex03-Filter anbieterkonstruktor*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample21.cs)]

    > [!NOTE]
    > Der filteranbieterklassenkonstruktor erstellt kein **Neues** Objekt in. Der Container wird als Parameter übergeben, und die Abhängigkeit wird von Unity gelöst.
7. Implementieren Sie in der **filterprovider** -Klasse die **getFilters** -Methode von der **ifilterprovider** -Schnittstelle.

    (Code Ausschnitt- *ASP.net-Abhängigkeitsinjektion Lab-Ex03-Filter Anbieter getFilters*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample22.cs)]

<a id="Ex3Task2"></a>

<a id="Task_2_-_Registering_and_Enabling_the_Filter"></a>
#### <a name="task-2---registering-and-enabling-the-filter"></a>Aufgabe 2: registrieren und Aktivieren des Filters

In dieser Aufgabe aktivieren Sie die Standort Nachverfolgung. Zu diesem Zweck registrieren Sie den Filter in der **Bootstrapper.cs buildunitycontainer** -Methode, um die Ablauf Verfolgung zu starten:

1. Öffnen Sie die Datei " **Web. config** " im Stammverzeichnis des Projekts, und aktivieren Sie die Ablauf Verfolgungs Verfolgung in der Gruppe "System

    [!code-xml[Main](aspnet-mvc-4-dependency-injection/samples/sample23.xml)]
2. Öffnen Sie **Bootstrapper.cs** im Projektstamm.
3. Fügen Sie einen Verweis auf den **mvcmusicstore. Filters** -Namespace hinzu.

    (Code Ausschnitt- *ASP.net-Abhängigkeitsinjektion Lab-Ex03-Bootstrapper Hinzufügen von Namespaces*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample24.cs)]
4. Wählen Sie die **buildunitycontainer** -Methode aus, und registrieren Sie den Filter im Unity-Container. Sie müssen den Filter Anbieter und den Aktionsfilter registrieren.

    (Code Ausschnitt- *ASP.net-Abhängigkeitsinjektion Lab-Ex03-Register filterprovider und Action Filter*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample25.cs)]

<a id="Ex3Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a>Aufgabe 3: Ausführen der Anwendung

In dieser Aufgabe führen Sie die Anwendung aus und testen, ob der benutzerdefinierte Aktionsfilter die Aktivität verfolgt:

1. Drücken Sie **F5**, um die Anwendung auszuführen.
2. Klicken Sie im Menü Genres auf **Rock** . Wenn Sie möchten, können Sie nach weiteren Genres suchen.

    ![Music Store](aspnet-mvc-4-dependency-injection/_static/image11.png "Music Store")

    *Music Store*
3. Navigieren Sie zu **/Trace.axd** , um die Seite für die Anwendungs Ablauf Verfolgung anzuzeigen, und klicken Sie dann auf **Details anzeigen**.

    ![Anwendungs Ablauf Verfolgungs Protokoll](aspnet-mvc-4-dependency-injection/_static/image12.png "Anwendungs Ablauf Verfolgungs Protokoll")

    *Anwendungs Ablauf Verfolgungs Protokoll*

    ![Anwendungs Ablauf Verfolgung: Anforderungs Details](aspnet-mvc-4-dependency-injection/_static/image13.png "Anwendungs Ablauf Verfolgung: Anforderungs Details")

    *Anwendungs Ablauf Verfolgung: Anforderungs Details*
4. Schließen Sie den Browser.

---

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a>Zusammenfassung

Wenn Sie diese praktische Übungseinheit durcharbeiten, haben Sie gelernt, wie Sie die Abhängigkeitsinjektion in ASP.NET MVC 4 verwenden, indem Sie Unity mithilfe eines nuget-Pakets integrieren. Um dies zu erreichen, haben Sie Abhängigkeitsinjektion innerhalb von Controllern, Ansichten und Aktions Filtern verwendet.

Die folgenden Konzepte wurden behandelt:

- ASP.NET MVC 4-Abhängigkeits einschleusungs Features
- Unity-Integration mit dem Unity. Mvc3-nuget-Paket
- Abhängigkeitsinjektion in Controller
- Abhängigkeitsinjektion in Sichten
- Abhängigkeitsinjektion von Aktions Filtern

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>Anhang A: Installieren von Visual Studio Express 2012 für das Web

Sie können **Microsoft Visual Studio Express 2012 für Web** oder eine andere &quot;Express&quot; Version mit dem **[Microsoft-Webplattform-Installer](https://www.microsoft.com/web/downloads/platform.aspx)** installieren. Die folgenden Anweisungen führen Sie durch die Schritte, die zum Installieren *von Visual Studio Express 2012 für Web* mithilfe von *Microsoft-Webplattform-Installer*erforderlich sind.

1. Wechseln Sie zu [https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169). Wenn Sie den Webplattform-Installer bereits installiert haben, können Sie ihn auch öffnen und nach dem Produkt &quot;<em>Visual Studio Express 2012 für das Web mit dem Windows Azure SDK</em>&quot;suchen.
2. Klicken Sie auf **jetzt installieren**. Wenn Sie nicht über einen **Webplattform-Installer** verfügen, werden Sie umgeleitet, um Sie zuerst herunterzuladen und zu installieren.
3. Nachdem der **Webplattform-Installer** geöffnet ist, klicken Sie auf **Installieren** , um das Setup zu starten.

    ![Installieren von Visual Studio Express](aspnet-mvc-4-dependency-injection/_static/image14.png "Installieren von Visual Studio Express")

    *Installieren von Visual Studio Express*
4. Lesen Sie die Lizenzbedingungen für alle Produkte, und klicken Sie auf **ich** Stimme zu, um fortzufahren.

    ![Akzeptieren der Lizenzbedingungen](aspnet-mvc-4-dependency-injection/_static/image15.png)

    *Akzeptieren der Lizenzbedingungen*
5. Warten Sie, bis der Download-und Installationsvorgang abgeschlossen ist.

    ![Installationsstatus](aspnet-mvc-4-dependency-injection/_static/image16.png)

    *Installationsfortschritt*
6. Wenn die Installation abgeschlossen ist, klicken Sie auf **Fertig**stellen.

    ![Installation abgeschlossen](aspnet-mvc-4-dependency-injection/_static/image17.png)

    *Installation abgeschlossen*
7. Klicken Sie zum Schließen des Webplattform-Installers auf **Beenden** .
8. Um Visual Studio Express für das Web zu öffnen, navigieren Sie zum **Start** Bildschirm, und beginnen Sie mit dem Schreiben &quot;**vs Express**&quot;und klicken Sie dann auf die Kachel **vs Express für Web** .

    ![VS Express für Web-Kachel](aspnet-mvc-4-dependency-injection/_static/image18.png)

    *VS Express für Web-Kachel*

<a id="AppendixB"></a>

<a id="Appendix_B_Using_Code_Snippets"></a>
## <a name="appendix-b-using-code-snippets"></a>Anhang B: Verwenden von Code Ausschnitten

Mit Code Ausschnitten haben Sie den gesamten Code, den Sie benötigen. Im Lab-Dokument werden Sie genau wissen, wann Sie Sie verwenden können, wie in der folgenden Abbildung dargestellt.

![Verwenden von Visual Studio-Code Ausschnitten zum Einfügen von Code in Ihr Projekt](aspnet-mvc-4-dependency-injection/_static/image19.png "Verwenden von Visual Studio-Code Ausschnitten zum Einfügen von Code in Ihr Projekt")

*Verwenden von Visual Studio-Code Ausschnitten zum Einfügen von Code in Ihr Projekt*

***So fügen Sie einen Code Ausschnitt mithilfe der Tastatur hinzuC# (nur)***

1. Platzieren Sie den Cursor an der Stelle, an der Sie den Code einfügen möchten.
2. Beginnen Sie mit der Eingabe des Ausschnitt namens (ohne Leerzeichen oder Bindestriche).
3. Sehen Sie sich an, wie IntelliSense übereinstimmende Code Ausschnitte anzeigt.
4. Wählen Sie den richtigen Ausschnitt aus (oder geben Sie die Eingabe fort, bis der gesamte Name des Ausschnitts ausgewählt ist).
5. Drücken Sie zweimal die Tab-Taste, um den Ausschnitt an der Cursorposition einzufügen.

![Beginnen Sie mit der Eingabe des Ausschnitt namens.](aspnet-mvc-4-dependency-injection/_static/image20.png "Beginnen Sie mit der Eingabe des Ausschnitt namens.")

*Beginnen Sie mit der Eingabe des Ausschnitt namens.*

![Drücken Sie TAB, um den markierten Ausschnitt auszuwählen.](aspnet-mvc-4-dependency-injection/_static/image21.png "Drücken Sie TAB, um den markierten Ausschnitt auszuwählen.")

*Drücken Sie TAB, um den markierten Ausschnitt auszuwählen.*

![Drücken Sie erneut die Tab-Taste, und der Ausschnitt wird erweitert.](aspnet-mvc-4-dependency-injection/_static/image22.png "Drücken Sie erneut die Tab-Taste, und der Ausschnitt wird erweitert.")

*Drücken Sie erneut die Tab-Taste, und der Ausschnitt wird erweitert.*

So ***fügen Sie einen Code Ausschnitt mithilfe der Maus hinzuC#(, Visual Basic und XML)*** 1. Klicken Sie mit der rechten Maustaste auf den Ort, an dem Sie den Code Ausschnitt einfügen möchten.

1. Wählen Sie **Ausschnitt einfügen** und dann **meine Code Ausschnitte**aus.
2. Wählen Sie den entsprechenden Code Ausschnitt aus der Liste aus, indem Sie darauf klicken.

![Klicken Sie mit der rechten Maustaste darauf, wo Sie den Code Ausschnitt einfügen möchten, und wählen Sie Ausschnitt einfügen aus.](aspnet-mvc-4-dependency-injection/_static/image23.png "Klicken Sie mit der rechten Maustaste darauf, wo Sie den Code Ausschnitt einfügen möchten, und wählen Sie Ausschnitt einfügen aus.")

*Klicken Sie mit der rechten Maustaste darauf, wo Sie den Code Ausschnitt einfügen möchten, und wählen Sie Ausschnitt einfügen aus.*

![Wählen Sie den entsprechenden Code Ausschnitt aus der Liste aus, indem Sie darauf klicken.](aspnet-mvc-4-dependency-injection/_static/image24.png "Wählen Sie den entsprechenden Code Ausschnitt aus der Liste aus, indem Sie darauf klicken.")

*Wählen Sie den entsprechenden Code Ausschnitt aus der Liste aus, indem Sie darauf klicken.*
