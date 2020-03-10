---
uid: aspnet/overview/developing-apps-with-windows-azure/maintainable-azure-websites-managing-change-and-scale
title: 'Praktische Übungseinheit: wart bare Azure-Websites: Verwalten von Änderungen und Skalierungen | Microsoft-Dokumentation'
author: rick-anderson
description: In dieser Übungseinheit erfahren Sie, wie Microsoft Azure das Erstellen und Bereitstellen von Websites in der Produktion erleichtert.
ms.author: riande
ms.date: 07/16/2014
ms.assetid: ecfd0eb4-c4ad-44e6-9db9-a2a66611ff6a
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/maintainable-azure-websites-managing-change-and-scale
msc.type: authoredcontent
ms.openlocfilehash: c88bae40a8aa092037c0b359ee391acaf161cf10
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78506373"
---
# <a name="hands-on-lab-maintainable-azure-websites-managing-change-and-scale"></a>Praktische Übungseinheit: wart bare Azure-Websites: Verwalten von Änderungen und Skalierungen

vom [Web Camps-Team](https://twitter.com/webcamps)

[Webcamps-Trainingskit herunterladen](https://aka.ms/webcamps-training-kit)

> Microsoft Azure erleichtert das Erstellen und Bereitstellen von Websites in der Produktion. Sie sind jedoch nicht fertig, wenn Ihre Anwendung Live ist. Sie werden erst gestartet. Sie müssen sich ändernde Anforderungen, Datenbankaktualisierungen, Skalierungen und vieles mehr kümmern. Glücklicherweise haben Sie Azure App Service mit vielen Features abgedeckt, die Ihnen helfen, ihre Websites reibungslos zu betreiben.
>
> Azure bietet sichere und flexible Entwicklungs-, Bereitstellungs-und Skalierungs Optionen für Webanwendungen jeder Größe. Nutzen Sie Ihre vorhandenen Tools, um Anwendungen zu erstellen und bereitzustellen, ohne die Infrastruktur zu verwalten.
>
> Stellen Sie in wenigen Minuten eine produktionsweb Anwendung bereit, indem Sie problemlos Inhalte bereitstellen, die mit Ihrem bevorzugten Entwicklungs Tool erstellt wurden Sie können eine vorhandene Site direkt aus der Quell Code Verwaltung mit Unterstützung für **git**, **GitHub**, **Bitbucket**, **TFS**und sogar **Dropbox**bereitstellen. Stellen Sie direkt über Ihre bevorzugte IDE oder mithilfe von **PowerShell** in Windows oder **CLI** -Tools, die unter einem beliebigen Betriebssystem ausgeführt werden, bereit. Behalten Sie nach der Bereitstellung ihre Websites ständig auf dem neuesten Stand, und Unterstützung für Continuous Deployment.
>
> Azure bietet skalierbare, dauerhafte cloudspeicher-, Sicherungs-und Wiederherstellungs Lösungen für beliebige Daten, groß oder klein. Beim Bereitstellen von Anwendungen in einer Produktionsumgebung können Sie mithilfe von Speicherdiensten wie Tabellen, BLOB-und SQL-Datenbanken Ihre Anwendung in der Cloud skalieren.
>
> Bei SQL-Datenbanken ist es wichtig, dass ihre produktive Datenbank immer auf dem neuesten Stand ist, wenn Sie neue Versionen der Anwendung bereitstellen. Dank der **Entity Framework Code First-Migrationen**wurde die Entwicklung und Bereitstellung Ihres Datenmodells vereinfacht, um Ihre Umgebungen innerhalb weniger Minuten zu aktualisieren. Diese praktische Übungseinheit zeigt Ihnen die verschiedenen Themen, die bei der Bereitstellung Ihrer Web-App in Produktionsumgebungen in Microsoft Azure auftreten können.
>
> Der gesamte Beispielcode und die Code Ausschnitte sind im Web Camps-Trainingskit enthalten, das unter [https://aka.ms/webcamps-training-kit](https://aka.ms/webcamps-training-kit)verfügbar ist.
>
> Ausführlichere Informationen zu diesem Thema finden [Sie im e-Book zum Aufbauen von realen Cloud-apps mit Azure](building-real-world-cloud-apps-with-windows-azure/introduction.md).

<a id="Overview"></a>
## <a name="overview"></a>Übersicht

<a id="Objectives"></a>
### <a name="objectives"></a>Ziele

In dieser praktischen Übungseinheit erfahren Sie Folgendes:

- Aktivieren von Entity Framework Migrationen mit einem vorhandenen Modell
- Aktualisieren des Objektmodells und der Datenbank entsprechend mithilfe Entity Framework Migrationen
- Bereitstellen in Azure App Service mithilfe von git
- Zurücksetzen auf eine vorherige Bereitstellung mithilfe des Azure-Verwaltungs Portals
- Verwenden von Azure Storage zum Skalieren einer Web-App
- Konfigurieren der automatischen Skalierung für eine Web-App mithilfe des Azure-Verwaltungsportal
- Erstellen und Konfigurieren eines Auslastungs Testprojekts in Visual Studio

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Voraussetzungen

Zum Durchführen dieser praktischen Übungseinheit ist Folgendes erforderlich:

- [Visual Studio Express 2013 für Web](https://www.microsoft.com/visualstudio/) oder höher
- [Azure SDK für .NET 2,2](https://www.microsoft.com/windowsazure/sdk/)
- [GIT-Versionskontrollsystem](http://git-scm.com/download)
- Ein Microsoft Azure-Abonnement

    - Registrieren Sie sich für eine [Kostenlose Testversion](https://aka.ms/watk-freetrial)
    - Wenn Sie Visual Studio Professional, Test Professional, Premium oder Ultimate mit MSDN oder MSDN Platforms Abonnent sind, aktivieren Sie Ihren [MSDN-Vorteil](https://aka.ms/watk-msdn) nun, um mit der Entwicklung und dem Testen in Azure zu beginnen.
    - [BizSpark](https://aka.ms/watk-bizspark) -Mitglieder erhalten den Azure-Vorteil automatisch über Ihre Visual Studio Ultimate mit MSDN-Abonnements
    - Mitglieder des [Microsoft Partner Network](https://aka.ms/watk-mpn) Cloud Essentials-Programms erhalten monatliche Azure-Gutschriften kostenlos.

<a id="Setup"></a>
### <a name="setup"></a>Einrichten

Um die Übungen in dieser praktischen Übungseinheit auszuführen, müssen Sie zuerst Ihre Umgebung einrichten.

1. Öffnen Sie Windows-Explorer, und navigieren Sie zum **Quell** Ordner des Labs.
2. Klicken Sie mit der rechten Maustaste auf **Setup. cmd** , und wählen Sie **als Administrator ausführen** aus, um den Setup Vorgang zu starten, mit dem die Umgebung konfiguriert wird, und die Visual Studio-Code Ausschnitte für diese Übungseinheit
3. Wenn das Dialogfeld "Benutzerkontensteuerung" angezeigt wird, bestätigen Sie die Aktion, um fortzufahren.

> [!NOTE]
> Vergewissern Sie sich, dass Sie vor dem Ausführen des Setups alle Abhängigkeiten für dieses Lab geprüft haben.

<a id="CodeSnippets"></a>
### <a name="using-the-code-snippets"></a>Verwenden der Code Ausschnitte

Im gesamten Lab-Dokument werden Sie angewiesen, Code Blöcke einzufügen. Der größte Teil dieses Codes wird zur einfacheren Verwendung als Visual Studio Code Ausschnitte bereitgestellt, auf die Sie in Visual Studio 2013 zugreifen können, um das manuelle Hinzufügen zu vermeiden.

> [!NOTE]
> Jede Übung wird von einer Start Lösung begleitet, die sich im Ordner " **Begin** " der Übung befindet und Ihnen ermöglicht, die einzelnen Übungen unabhängig von den anderen zu befolgen. Beachten Sie, dass die während einer Übung hinzugefügten Code Ausschnitte in diesen Projektmappen fehlen und möglicherweise erst nach Abschluss der Übung funktionieren. Innerhalb des Quellcodes für eine Übung finden Sie auch einen **endordner** mit einer Visual Studio-Projekt Mappe mit dem Code, der aus der Ausführung der Schritte in der entsprechenden Übung resultiert. Sie können diese Lösungen als Leitfaden verwenden, wenn Sie zusätzliche Hilfe benötigen, während Sie diese praktische Übungseinheit durcharbeiten.

---

<a id="Exercises"></a>
## <a name="exercises"></a>Exerzitien

Diese praktische Übungseinheit umfasst die folgenden Übungen:

1. [Verwenden von Entity Framework Migrationen](#Exercise1)
2. [Bereitstellen einer Web-App für die Bereitstellung](#Exercise2)
3. [Ausführen eines Bereitstellungs Rollbacks in der Produktionsumgebung](#Exercise3)
4. [Skalieren mithilfe Azure Storage](#Exercise4)
5. [Verwenden der automatischen Skalierung für Web-Apps](#Exercise5) (optional für Visual Studio 2013 Ultimate Edition)

Geschätzte Zeit bis zum Abschluss dieses Labs: **75 Minuten**

> [!NOTE]
> Wenn Sie Visual Studio zum ersten Mal starten, müssen Sie eine der vordefinierten Einstellungs Sammlungen auswählen. Jede vordefinierte Sammlung ist so konzipiert, dass Sie einem bestimmten Entwicklungsstil entspricht, und bestimmt Fensterlayouts, das Editor-Verhalten, IntelliSense-Code Ausschnitte und Dialogfeld Optionen. In den Prozeduren in dieser Übungseinheit werden die Aktionen beschrieben, die zum Ausführen einer bestimmten Aufgabe in Visual Studio bei Verwendung der **allgemeinen Entwicklungs Einstellungs** Auflistung erforderlich sind. Wenn Sie eine andere Einstellungs Sammlung für Ihre Entwicklungsumgebung auswählen, kann es Unterschiede in den Schritten geben, die Sie berücksichtigen sollten.

<a id="Exercise1"></a>
### <a name="exercise-1-using-entity-framework-migrations"></a>Übung 1: Verwenden von Entity Framework Migrationen

Wenn Sie eine Anwendung entwickeln, kann sich das Datenmodell im Laufe der Zeit ändern. Diese Änderungen können sich auf das vorhandene Modell in der Datenbank auswirken (wenn Sie eine neue Version erstellen), und es ist wichtig, die Datenbank auf dem neuesten Stand zu halten, um Fehler zu verhindern.

Um die Nachverfolgung dieser Änderungen im Modell zu vereinfachen, erkennen **Entity Framework Code First-Migrationen** automatisch Änderungen, die das Modell mit dem Datenbankschema vergleichen, und generieren spezifischen Code zum Aktualisieren der Datenbank, wodurch neue *Versionen* der Datenbank erstellt werden.

Diese Übung zeigt Ihnen, wie **Migrationen** für Ihre Anwendung aktiviert werden und wie Sie mühelos Änderungen zum Aktualisieren Ihrer Datenbanken erkennen und generieren können.

<a id="Ex1Task1"></a>
#### <a name="task-1--enabling-migrations"></a>Aufgabe 1 – Aktivieren von Migrationen

In dieser Aufgabe durchlaufen Sie die Schritte zum Aktivieren von **Entity Framework Code First-Migrationen** für die **Geek-Quiz** Datenbank, zum Ändern des Modells und zum Verständnis der Art und Weise, wie diese Änderungen in der Datenbank widergespiegelt werden.

1. Öffnen Sie Visual Studio, und öffnen Sie die **Projekt Mappe geekquiz. sln** von **source\ex1-usingentityframeworkmigrations\begin**.
2. Erstellen Sie die Projekt Mappe, um die **nuget** -Paketabhängigkeiten herunterzuladen und zu installieren. Klicken Sie hierzu mit der rechten Maustaste auf die Projekt Mappe, und klicken Sie auf Projekt Mappe **Erstellen** , oder drücken Sie **STRG + UMSCHALT + B**.
3. Wählen Sie **im Menü Extras** in Visual Studio den **nuget-Paket-Manager**aus, und klicken Sie dann auf Paket-Manager- **Konsole**.
4. Geben Sie in der **Paket-Manager-Konsole**den folgenden Befehl ein, und drücken Sie dann die **Eingabe**Taste. Eine anfängliche Migration basierend auf dem vorhandenen Modell wird erstellt.

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample1.ps1)]

    ![Aktivieren von Migrationen](maintainable-azure-websites-managing-change-and-scale/_static/image1.png "Aktivieren von Migrationen")

    *Aktivieren von Migrationen*

    > [!NOTE]
    > Mit diesem Befehl wird ein **Migrations** Ordner zum Geek Quiz Project hinzugefügt, das eine Datei namens **Configuration.cs**enthält. Mit der- **Konfigurations** Klasse können Sie konfigurieren, wie Migrationen für den Kontext Verhalten.
5. Wenn Migrationen aktiviert sind, müssen Sie die **Konfigurations** Klasse aktualisieren, um die Datenbank mit den anfänglichen Daten aufzufüllen, die für den **Geek-Quiz** erforderlich sind. Ersetzen Sie unter **Migrationen**die Datei **Configuration.cs** durch die Datei, die sich im Ordner **source\assets** dieses Labs befindet.

    > [!NOTE]
    > Da **Migrationen** die **Seed** -Methode bei jedem Datenbankupdate aufruft, müssen Sie sicherstellen, dass die Datensätze nicht in der Datenbank dupliziert werden. Mithilfe der **addorupdate** -Methode können doppelte Daten verhindert werden.
6. Um eine anfängliche Migration hinzuzufügen, geben Sie den folgenden Befehl ein, und drücken Sie dann die **Eingabe**Taste.

    > [!NOTE]
    > Stellen Sie sicher, dass in ihrer localdb-Instanz keine Datenbank mit dem Namen &quot;geekquiz Prod&quot; vorhanden ist.

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample2.ps1)]

    ![Hinzufügen der Basis Schema Migration](maintainable-azure-websites-managing-change-and-scale/_static/image2.png "Hinzufügen der Basis Schema Migration")

    *Hinzufügen der Basis Schema Migration*

    > [!NOTE]
    > Durch **Hinzufügen der Migration** wird das Gerüst der nächsten Migration basierend auf den Änderungen, die Sie seit der letzten Migration an Ihrem Modell vorgenommen haben, gebildet. In diesem Fall werden die Skripts zum Erstellen aller Tabellen hinzugefügt, die in der **triviacontext** -Klasse definiert sind, da es sich hierbei um die erste Migration des Projekts handelt.
7. Führen Sie die Migration aus, um die Datenbank zu aktualisieren, indem Sie den folgenden Befehl ausführen. Für diesen Befehl geben Sie das Flag " **verbose** " an, um die SQL-Anweisungen anzuzeigen, die auf die Zieldatenbank angewendet werden.

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample3.ps1)]

    ![Anfängliche Datenbank wird erstellt](maintainable-azure-websites-managing-change-and-scale/_static/image3.png "Anfängliche Datenbank wird erstellt")

    *Anfängliche Datenbank wird erstellt*

    > [!NOTE]
    > **Update-Database** wendet alle ausstehenden Migrationen auf die Datenbank an. In diesem Fall wird die Datenbank mithilfe der Verbindungs Zeichenfolge erstellt, die in der Datei "Web. config" definiert ist.
8. Wechseln Sie zum Menü **Ansicht** , und öffnen Sie **SQL Server-Objekt-Explorer**.

    ![In SQL Server-Objekt-Explorer öffnen](maintainable-azure-websites-managing-change-and-scale/_static/image4.png "In SQL Server-Objekt-Explorer öffnen")

    *In SQL Server-Objekt-Explorer öffnen*
9. Stellen Sie im **SQL Server-Objekt-Explorer** Fenster eine Verbindung mit ihrer localdb-Instanz her, indem Sie mit der rechten Maustaste auf den Knoten **SQL Server** klicken und **SQL Server hinzufügen** auswählen.

    ![Hinzufügen einer SQL Server Instanz](maintainable-azure-websites-managing-change-and-scale/_static/image5.png "Hinzufügen einer SQL Server Instanz")

    *Hinzufügen einer SQL Server Instanz zu SQL Server-Objekt-Explorer*
10. Legen Sie den **Servernamen** auf *(localdb) \v11.0* fest, und lassen Sie **Windows-Authentifizierung** als Authentifizierungsmodus fest. Klicken Sie auf **Verbinden** , um den Vorgang fortzusetzen.

    ![Herstellen einer Verbindung mit localdb](maintainable-azure-websites-managing-change-and-scale/_static/image6.png "Herstellen einer Verbindung mit localdb")

    *Herstellen einer Verbindung mit localdb*
11. Öffnen Sie die Datenbank **geekquiz Prod** , und erweitern Sie den Knoten **Tabellen** . Wie Sie sehen, hat der **Update-Database-** Befehl alle in der **triviacontext** -Klasse definierten Tabellen generiert. Suchen Sie nach dem **dbo. Triviaquestions** -Tabelle, und öffnen Sie den Knoten Spalten. In der nächsten Aufgabe fügen Sie dieser Tabelle eine neue Spalte hinzu und aktualisieren die Datenbank mithilfe von **Migrationen**.

    ![Spalten mit Trivia-Fragen](maintainable-azure-websites-managing-change-and-scale/_static/image7.png "Spalten mit Trivia-Fragen")

    *Spalten mit Trivia-Fragen*

<a id="Ex1Task2"></a>
#### <a name="task-2--updating-database-schema-using-migrations"></a>Aufgabe 2 – Aktualisieren des Datenbankschemas mit Migrationen

In dieser Aufgabe verwenden Sie **Entity Framework Code First-Migrationen** , um eine Änderung im Modell zu erkennen und den erforderlichen Code zum Aktualisieren der Datenbank zu generieren. Sie aktualisieren die Entität " **triviaquestions** ", indem Sie Ihr eine neue Eigenschaft hinzufügen. Anschließend führen Sie-Befehle aus, um eine neue Migration zum Einschließen der neuen Spalte in die Tabelle zu erstellen.

1. Doppelklicken Sie in **Projektmappen-Explorer**auf die Datei " **TriviaQuestion.cs** ", die sich im Ordner " **Models** " befindet.
2. Fügen Sie eine neue Eigenschaft mit dem Namen **Hint**hinzu, wie im folgenden Code Ausschnitt gezeigt.

    [!code-csharp[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample4.cs)]
3. Geben Sie in der **Paket-Manager-Konsole**den folgenden Befehl ein, und drücken Sie dann die **Eingabe**Taste. Eine neue Migration wird erstellt, die die Änderung in unserem Modell widerspiegelt.

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample5.ps1)]

    ![Migration hinzufügen](maintainable-azure-websites-managing-change-and-scale/_static/image8.png "Migration hinzufügen")

    *Migration hinzufügen*

    > [!NOTE]
    > Eine Migrations Datei besteht aus zwei Methoden (nach **oben** und **unten**).
    >
    > - Die **up** -Methode wird verwendet, um anzugeben, welche Änderungen die aktuelle Version der Anwendung auf die Datenbank anwenden muss.
    > - Die **down** -Methode wird verwendet, um die Änderungen umzukehren, die der **up** -Methode hinzugefügt wurden.
    >
    > Wenn die Datenbank durch die Daten Bank Migration aktualisiert wird, werden alle Migrationen in der Zeitstempel Reihenfolge ausgeführt, und nur diejenigen, die seit dem letzten Update nicht verwendet wurden (in der \_migrationhistory-Tabelle wird nachverfolgt, welche Migrationen angewendet wurden). Die **up** -Methode aller Migrationen wird aufgerufen, und die von uns angegebenen Änderungen werden an die Datenbank übernehmen. Wenn wir uns für eine frühere Migration entscheiden, wird die **down** -Methode aufgerufen, um die Änderungen in umgekehrter Reihenfolge wiederholen.
4. Geben Sie in der **Paket-Manager-Konsole**den folgenden Befehl ein, und drücken Sie dann die **Eingabe**Taste.

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample6.ps1)]
5. Die Ausgabe des **Update-Database-** Befehls hat eine **ALTER TABLE** SQL-Anweisung generiert, um der Tabelle **triviaquestions** eine neue Spalte hinzuzufügen, wie in der folgenden Abbildung dargestellt.

    ![SQL-Anweisung für Spalte hinzufügen generiert](maintainable-azure-websites-managing-change-and-scale/_static/image9.png "SQL-Anweisung für Spalte hinzufügen generiert")

    *SQL-Anweisung für Spalte hinzufügen generiert*
6. Aktualisieren Sie in **SQL Server-Objekt-Explorer**den **dbo. Triviaquestions** -Tabelle, und überprüfen Sie, ob die neue **Hinweis** Spalte angezeigt wird.

    ![Die neue Hinweis Spalte wird angezeigt.](maintainable-azure-websites-managing-change-and-scale/_static/image10.png "Die neue Hinweis Spalte wird angezeigt.")

    *Die neue Hinweis Spalte wird angezeigt.*
7. Fügen Sie im **TriviaQuestion.cs** -Editor der *Hint* -Eigenschaft eine **StringLength** -Einschränkung hinzu, wie im folgenden Code Ausschnitt gezeigt.

    [!code-csharp[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample7.cs)]
8. Geben Sie in der **Paket-Manager-Konsole**den folgenden Befehl ein, und drücken Sie dann die **Eingabe**Taste.

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample8.ps1)]
9. Geben Sie in der **Paket-Manager-Konsole**den folgenden Befehl ein, und drücken Sie dann die **Eingabe**Taste.

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample9.ps1)]
10. Die Ausgabe des **Update-Database-** Befehls hat eine **ALTER TABLE** SQL-Anweisung generiert, um den *Hinweis* Spaltentyp der Tabelle " **triviaquestions** " zu aktualisieren, wie in der folgenden Abbildung dargestellt.

    ![Alter Column-SQL-Anweisung generiert](maintainable-azure-websites-managing-change-and-scale/_static/image11.png "Alter Column-SQL-Anweisung generiert")

    *Alter Column-SQL-Anweisung generiert*
11. Aktualisieren Sie in **SQL Server-Objekt-Explorer**den **dbo. Triviaquestions** -Tabelle, und überprüfen Sie, ob der **Hinweis** Spaltentyp **nvarchar (150)** ist.

    ![Die neue Einschränkung wird angezeigt.](maintainable-azure-websites-managing-change-and-scale/_static/image12.png "Die neue Einschränkung wird angezeigt.")

    *Die neue Einschränkung wird angezeigt.*

<a id="Exercise2"></a>
### <a name="exercise-2-deploying-a-web-app-to-staging"></a>Übung 2: Bereitstellen einer Web-App für die Bereitstellung

Mit **Web-Apps in Azure App Service** können Sie eine gestaffelte Veröffentlichung durchführen. Die gestaffelte Veröffentlichung erstellt einen stagingsiteslot für jede Standard Produktions Site und ermöglicht es Ihnen, diese Slots ohne Ausfallzeit auszutauschen. Dies ist sehr nützlich, um Änderungen vor der Veröffentlichung in der Öffentlichkeit zu überprüfen, Website Inhalte inkrementell zu integrieren und einen Rollback auszuführen, wenn die Änderungen nicht erwartungsgemäß funktionieren.

In dieser Übung stellen Sie die Anwendung " **Geek Quiz** " mithilfe der git-Quell Code Verwaltung in der Stagingumgebung Ihrer Web-App bereit. Zu diesem Zweck erstellen Sie die Web-App und stellen die erforderlichen Komponenten im Verwaltungs Portal bereit. Konfigurieren Sie ein **git** -Repository, und verschieben Sie den Quellcode der Anwendung vom lokalen Computer in den stagingslot. Außerdem aktualisieren Sie die Produktionsdatenbank mit dem **Code First-Migrationen** , den Sie in der vorherigen Übung erstellt haben. Anschließend führen Sie die Anwendung in dieser Testumgebung aus, um den Vorgang zu überprüfen. Wenn Sie zufrieden sind, dass Sie gemäß Ihren Erwartungen funktioniert, werden Sie die Anwendung auf die Produktion herauf Stufen.

> [!NOTE]
> Zum Aktivieren der stagingveröffentlichung muss sich die Web-App im **Standard Modus**befinden. Beachten Sie, dass zusätzliche Gebühren anfallen, wenn Sie Ihre Web-App in den Standard Modus ändern. Weitere Informationen zu den Preisen finden Sie unter [App Service Preise](https://azure.microsoft.com/pricing/details/app-service/).

<a id="Ex2Task1"></a>
#### <a name="task-1--creating-a-web-app-in-azure-app-service"></a>Aufgabe 1 – Erstellen einer Web-App in Azure App Service

In dieser Aufgabe erstellen Sie eine Web-App in **Azure App Service** aus dem Verwaltungs Portal. Außerdem konfigurieren Sie eine **SQL-Datenbank** , um die Anwendungsdaten beizubehalten, und konfigurieren ein lokales git-Repository für die Quell Code Verwaltung.

1. Wechseln Sie zum [Azure-Verwaltungs Portal](https://manage.windowsazure.com) , und melden Sie sich mit der Microsoft-Konto an, die Ihrem Abonnement zugeordnet ist.

    ![Melden Sie sich beim Azure-Verwaltungs Portal an.](maintainable-azure-websites-managing-change-and-scale/_static/image13.png)

    *Melden Sie sich beim Azure-Verwaltungs Portal an.*
2. Klicken Sie in der Befehlsleiste unten auf der Seite auf **neu** .

    ![Erstellen einer neuen Web-App](maintainable-azure-websites-managing-change-and-scale/_static/image14.png "Erstellen einer neuen Web-App")

    *Erstellen einer neuen Web-App*
3. Klicken Sie auf **Compute**, **Website** und dann auf **Benutzer definiert erstellen**.

    ![Erstellen einer neuen Web-App mit benutzerdefiniertem erstellen](maintainable-azure-websites-managing-change-and-scale/_static/image15.png "Erstellen einer neuen Web-App mit benutzerdefiniertem erstellen")

    *Erstellen einer neuen Web-App mit benutzerdefiniertem erstellen*
4. Geben Sie im Dialogfeld **neue Website-benutzerdefiniertes erstellen** eine verfügbare **URL** an (z. b. " *Geek-Quiz*"), wählen Sie einen Speicherort in der Dropdown Liste **Region** aus, und wählen Sie in der Dropdown Liste **Datenbank** die Option **neue SQL-Datenbank erstellen** aus. Wählen Sie abschließend das Kontrollkästchen **aus Quell Code Verwaltung veröffentlichen aus** , und klicken Sie auf **weiter**.

    ![Anpassen der neuen Web-App](maintainable-azure-websites-managing-change-and-scale/_static/image16.png)

    *Anpassen der neuen Web-App*
5. Geben Sie die folgenden Informationen für die Datenbankeinstellungen an:

   - Geben Sie im Textfeld **Name** einen Datenbanknamen ein (z. b. *geekquiz\_DB*).
   - Wählen Sie in der **Dropdown** Liste Server die Option **neuer SQL-Daten Bank Server**aus. Wahlweise können Sie auch einen vorhandenen Server auswählen.
   - Geben Sie in den Feldern **Datenbank-Benutzername** und **Daten Bank Kennwort** den Benutzernamen und das Kennwort für den SQL-Datenbankserver ein Wenn Sie einen Server auswählen, den Sie bereits erstellt haben, werden Sie zur Eingabe des Kennworts aufgefordert.

     ![Angeben der Datenbankeinstellungen](maintainable-azure-websites-managing-change-and-scale/_static/image17.png)

     *Angeben der Datenbankeinstellungen*
6. Klicken Sie auf **Weiter**, um fortzufahren.
7. Wählen Sie **Lokales git-Repository** für die Quell Code Verwaltung aus, und klicken Sie auf **weiter**.

    > [!NOTE]
    > Möglicherweise werden Sie zur Eingabe der Anmelde Informationen für die Bereitstellung aufgefordert (Benutzername und Kennwort).

    ![Erstellen des git-Repository](maintainable-azure-websites-managing-change-and-scale/_static/image18.png)

    *Erstellen des git-Repository*
8. Warten Sie, bis die neue Web-App erstellt wurde.

    > [!NOTE]
    > Standardmäßig stellt Azure Domänen unter *azurewebsites.net* bereit, bietet Ihnen jedoch auch die Möglichkeit, benutzerdefinierte Domänen mithilfe des Azure-Verwaltungs Portals festzulegen. Sie können jedoch nur benutzerdefinierte Domänen verwalten, wenn Sie bestimmte Azure App Service Modi verwenden.
    >
    > Azure App Service ist in den Editionen Free, Shared, Basic, Standard und Premium verfügbar. Im Modus "kostenlos" und "freigegeben" werden alle Web-Apps in einer mehr Instanzen fähigen Umgebung ausgeführt und verfügen über Kontingente für die CPU-, Arbeitsspeicher-und Netzwerk Auslastung. Die maximale Anzahl von kostenlosen Apps kann mit Ihrem Plan variieren. Im Standard Modus wählen Sie aus, welche apps auf dedizierten virtuellen Computern ausgeführt werden, die den Standard mäßigen Azure-Compute-Ressourcen entsprechen. Sie finden die Konfiguration des Web-App-Modus im Menü **skalieren** Ihrer Web-App.
    >
    > ![Azure App Service Modi](maintainable-azure-websites-managing-change-and-scale/_static/image19.png "Azure App Service Modi")
    >
    > Wenn Sie den Modus "frei **gegeben** " oder " **Standard** " verwenden, können Sie benutzerdefinierte Domänen für Ihre Web-App verwalten, indem Sie zum Menü **Konfigurieren** der App wechseln und Unterdomänen *Namen*auf **Domänen verwalten** klicken.
    >
    > ![Verwalten von Domänen](maintainable-azure-websites-managing-change-and-scale/_static/image20.png "Verwalten von Domänen")
    >
    > ![Benutzerdefinierte Domänen verwalten](maintainable-azure-websites-managing-change-and-scale/_static/image21.png "Benutzerdefinierte Domänen verwalten")
9. Nachdem die Web-App erstellt wurde, klicken Sie auf den Link unter der Spalte **URL** , um zu überprüfen, ob die neue Web-App ausgeführt wird.

    ![Navigieren zur neuen Web-App](maintainable-azure-websites-managing-change-and-scale/_static/image22.png)

    *Navigieren zur neuen Web-App*

    ![Ausführung der Web-App](maintainable-azure-websites-managing-change-and-scale/_static/image23.png)

    *Ausführung der Web-App*

<a id="Ex2Task2"></a>
#### <a name="task-2--creating-the-production-sql-database"></a>Aufgabe 2 – Erstellen der SQL-Produktionsdatenbank

In dieser Aufgabe verwenden Sie die **Entity Framework Code First-Migrationen** , um die Datenbank für die **Azure SQL-Daten Bank** Instanz zu erstellen, die Sie in der vorherigen Aufgabe erstellt haben.

1. Navigieren Sie in der Verwaltungsportal zu der Web-App, die Sie in der vorherigen Aufgabe erstellt haben, und wechseln Sie zum **Dashboard**.
2. Klicken Sie auf der Seite **Dashboard** im Abschnitt **kurzer Blick** auf den Link Verbindungs Zeichenfolgen **anzeigen** .

    ![Verbindungs Zeichenfolgen anzeigen](maintainable-azure-websites-managing-change-and-scale/_static/image24.png "Verbindungs Zeichenfolgen anzeigen")

    *Verbindungs Zeichenfolgen anzeigen*
3. Kopieren Sie den Wert der **Verbindungs Zeichenfolge** und schließen Sie das Dialogfeld.

    ![Verbindungs Zeichenfolge in Azure Verwaltungsportal](maintainable-azure-websites-managing-change-and-scale/_static/image25.png "Verbindungs Zeichenfolge in Azure Verwaltungsportal")

    *Verbindungs Zeichenfolge in Azure Verwaltungsportal*
4. Klicken Sie auf **SQL-Datenbanken** , um die Liste der SQL-Datenbanken in Azure

    ![SQL-Datenbank-Menü](maintainable-azure-websites-managing-change-and-scale/_static/image26.png "SQL-Datenbank-Menü")

    *SQL-Datenbank-Menü*
5. Suchen Sie die Datenbank, die Sie in der vorherigen Aufgabe erstellt haben, und klicken Sie auf den Server.

    ![SQL-Datenbankserver](maintainable-azure-websites-managing-change-and-scale/_static/image27.png "SQL-Datenbank-Server")

    *SQL-Datenbankserver*
6. Klicken Sie auf der Seite **Schnellstart** des Servers auf **Konfigurieren**.

    ![Menü konfigurieren](maintainable-azure-websites-managing-change-and-scale/_static/image28.png "Menü konfigurieren")

    *Menü konfigurieren*
7. Klicken Sie im Abschnitt **zulässige IP-Adressen** auf **Hinzufügen zum Link zulässige IP-Adressen** , um die IP-Adresse für die Verbindung mit dem SQL-Datenbankserver zu aktivieren.

    ![Zulässige IP-Adressen](maintainable-azure-websites-managing-change-and-scale/_static/image29.png "Zulässige IP-Adressen")

    *Zulässige IP-Adressen*
8. Klicken Sie unten auf der Seite auf **Save** , um den Schritt abzuschließen.
9. Wechseln Sie zurück zu Visual Studio.
10. Führen Sie in der **Paket-Manager-Konsole**den folgenden Befehl aus, indem Sie den Platzhalter *[Your-Connection-String]* durch die aus Azure kopierte Verbindungs Zeichenfolge ersetzen.

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample10.ps1)]

    ![Aktualisieren der Datenbank für die Windows Azure SQL-Datenbank](maintainable-azure-websites-managing-change-and-scale/_static/image30.png "Aktualisieren der Datenbank für die Windows Azure SQL-Datenbank")

    *Aktualisieren der Datenbank für die Azure SQL-Datenbank*

<a id="Ex2Task3"></a>
#### <a name="task-3--deploying-geek-quiz-to-staging-using-git"></a>Aufgabe 3 – Bereitstellen von Geek Quiz für Staging mithilfe von git

In dieser Aufgabe aktivieren Sie die gestaffelte Veröffentlichung in Ihrer Web-App. Anschließend verwenden Sie git, um die Geek-Quiz Anwendung direkt von Ihrem lokalen Computer in der Stagingumgebung Ihrer Web-App zu veröffentlichen.

1. Wechseln Sie zurück zum Portal, und klicken Sie auf den Namen der Web-App unter der Spalte **Name** , um die Verwaltungs Seiten anzuzeigen.

    ![Öffnen der Web-App-Verwaltungs Seiten](maintainable-azure-websites-managing-change-and-scale/_static/image31.png)

    *Öffnen der Web-App-Verwaltungs Seiten*
2. Navigieren Sie zur Seite **skalieren** . Wählen Sie im Abschnitt **Allgemein** die Option **Standard** für die Konfiguration aus, und klicken Sie auf der Befehlsleiste auf **Speichern** .

    > [!NOTE]
    > Wenn Sie alle Web-Apps in der aktuellen Region und im **Standard** Modus ausführen möchten, lassen Sie das Kontrollkästchen **Alle auswählen** in der Konfiguration **Websites auswählen** aktiviert. Deaktivieren Sie andernfalls das Kontrollkästchen **Alles auswählen** .

    ![Aktualisieren der Web-App auf den Standard Modus](maintainable-azure-websites-managing-change-and-scale/_static/image32.png "Aktualisieren der Web-App auf den Standard Modus")

    *Aktualisieren der Web-App auf den Standard Modus*
3. Klicken Sie auf **Ja** , um die Änderungen zu bestätigen.

    ![Bestätigen der Änderung am Standard Modus](maintainable-azure-websites-managing-change-and-scale/_static/image33.png "Fortsetzen der Änderung des Web-App-Modus")

    *Bestätigen der Änderung am Standard Modus*
4. Wechseln Sie zur Seite **Dashboard** , und klicken Sie im Abschnitt **kurzer Blick** auf bereitgestellte **Veröffentlichung aktivieren** .

    ![Aktivieren der gestaffelten Veröffentlichung](maintainable-azure-websites-managing-change-and-scale/_static/image34.png "Aktivieren der gestaffelten Veröffentlichung")

    *Aktivieren der gestaffelten Veröffentlichung*
5. Klicken Sie zum Aktivieren der stagingveröffentlichung auf **Ja** .

    ![Bestätigung der gestaffelten Veröffentlichung](maintainable-azure-websites-managing-change-and-scale/_static/image35.png "Klicken Sie zum Aktivieren der stagingveröffentlichung auf Ja")

    *Bestätigung der gestaffelten Veröffentlichung*
6. Erweitern Sie in der Liste der Web-Apps die Markierung links neben Ihrem Web-App-Namen, um den stagingsiteslot anzuzeigen. Sie hat den Namen Ihrer Web-App gefolgt von ***(Staging)***. Klicken Sie auf die Stagingsite, um zur Verwaltungsseite zu gelangen.

    ![Navigieren zur Staging-Web-App](maintainable-azure-websites-managing-change-and-scale/_static/image36.png "Navigieren zur Staging-Web-App")

    *Navigieren zur Staging-App*
7. Beachten Sie, dass die Verwaltungsseite wie jede andere Verwaltungsseite der Web-App aussieht. Navigieren Sie zur Seite bereit **Stellungen** , und kopieren Sie den Wert der **git-URL** . Sie werden Sie später in dieser Übung verwenden.

    ![Kopieren des git-URL-Werts](maintainable-azure-websites-managing-change-and-scale/_static/image37.png)

    *Kopieren des git-URL-Werts*
8. Öffnen Sie eine neue **git bash** -Konsole, und führen Sie die folgenden Befehle aus. Aktualisieren Sie den Platzhalter *[your-application-path]* mit dem Pfad zur **geekquiz-Projekt** Mappe, die sich im Ordner **source\ex1-deployingwebsitedestaging\begin** dieses Labs befindet.

    [!code-console[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample11.cmd)]

    ![Git-Initialisierung und erster Commit](maintainable-azure-websites-managing-change-and-scale/_static/image38.png)

    *Git-Initialisierung und erster Commit*
9. Führen Sie den folgenden Befehl aus, um Ihre Web-App an das **git** -Remote Repository zu überführen. Ersetzen Sie den Platzhalter durch die URL, die Sie aus dem Verwaltungs Portal abgerufen haben. Sie werden zur Eingabe Ihres Bereitstellungs Kennworts aufgefordert.

    [!code-console[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample12.cmd)]

    ![Pushübertragung an Windows Azure](maintainable-azure-websites-managing-change-and-scale/_static/image39.png)

    *Pushübertragung an Azure*

    > [!NOTE]
    > Wenn Sie Inhalte auf dem FTP-Host oder im git-Repository einer Web-App bereitstellen, müssen Sie sich mit den Anmelde Informationen für die **Bereitstellung** authentifizieren, die Sie auf den Verwaltungs Seiten **Schnellstart** oder **Dashboard** der Web-App erstellt haben. Wenn Sie Ihre Anmelde Informationen für die Bereitstellung nicht kennen, können Sie Sie problemlos mithilfe des Verwaltungs Portals zurücksetzen. Öffnen Sie die Seite Web-App- **Dashboard** , und klicken Sie auf den Link zur **Bereitstellung der Anmelde** Informationen Geben **Sie ein**neues Kennwort ein, und klicken Sie auf Anmelde Informationen für die Bereitstellung können für alle Web-Apps verwendet werden, die Ihrem Abonnement zugeordnet sind.
10. Wechseln Sie zurück zum Verwaltungs Portal, und klicken Sie auf **Websites**, um zu überprüfen, ob die Web-App erfolgreich in Azure übermittelt wurde.
11. Suchen Sie Ihre Web-App, und erweitern Sie den Eintrag, um den stagingsiteslot anzuzeigen. Klicken Sie auf den **Namen** , um zur Verwaltungsseite zu gelangen.
12. Klicken Sie auf bereit **Stellungen** , um den **Bereitstellungs Verlauf**anzuzeigen. Vergewissern Sie sich, dass eine **aktive Bereitstellung** mit Ihrem *&quot;anfänglichen Commit-&quot;* vorhanden ist.

    ![Aktive Bereitstellung](maintainable-azure-websites-managing-change-and-scale/_static/image40.png)

    *Aktive Bereitstellung*
13. Klicken Sie abschließend in der Befehlsleiste auf **Durchsuchen** , um zur Web-App zu wechseln.

    ![Web-App durchsuchen](maintainable-azure-websites-managing-change-and-scale/_static/image41.png)

    *Web-App durchsuchen*
14. Wenn die Anwendung erfolgreich bereitgestellt wurde, wird die Anmeldeseite für den Geek-Quiz angezeigt.

    > [!NOTE]
    > Die Adress-URL der bereitgestellten Anwendung enthält den Namen Ihrer Web-App gefolgt von *-Staging*.

    ![Anwendung, die in der Stagingumgebung ausgeführt wird](maintainable-azure-websites-managing-change-and-scale/_static/image42.png)

    *Anwendung, die in der Stagingumgebung ausgeführt wird*
15. Wenn Sie die Anwendung untersuchen möchten, klicken Sie auf **registrieren** , um einen neuen Benutzer zu registrieren. Vervollständigen Sie die Konto Details, indem Sie Benutzername, e-Mail-Adresse und Kennwort eingeben. Im nächsten Schritt zeigt die Anwendung die erste Frage des Quiz. Beantworten Sie einige Fragen, um sicherzustellen, dass Sie erwartungsgemäß funktioniert.

    ![Die Anwendung kann verwendet werden.](maintainable-azure-websites-managing-change-and-scale/_static/image43.png)

    *Die Anwendung kann verwendet werden.*

<a id="Ex2Task4"></a>
#### <a name="task-4--promoting-the-web-app-to-production"></a>Aufgabe 4 – herauf Stufen der Web-App zur Produktion

Nachdem Sie überprüft haben, dass die Web-App in der Stagingumgebung ordnungsgemäß funktioniert, können Sie Sie in die Produktion herauf Stufen. In dieser Aufgabe tauschen Sie den stagingsiteslot mit dem Produktionsstandort Slot aus.

1. Wechseln Sie zurück zum Verwaltungs Portal, und wählen Sie den Slot der Stagingsite aus. Klicken Sie in der Befehlsleiste auf **austauschen** .

    ![In Produktionsumgebungen austauschen](maintainable-azure-websites-managing-change-and-scale/_static/image44.png)

    *In Produktionsumgebungen austauschen*
2. Klicken Sie im Bestätigungs Dialogfeld auf **Ja** , um mit dem Austausch Vorgang fortzufahren. Azure tauscht den Inhalt der Produktions Website sofort mit dem Inhalt der Stagingsite aus.

    > [!NOTE]
    > Einige Einstellungen der gestaffelten Version werden automatisch in die Produktionsversion kopiert (z. b. außer Kraft setzungen für Verbindungs Zeichenfolgen, Handlerzuordnungen usw.), andere Einstellungen werden jedoch nicht geändert (z. b. DNS-Endpunkte, SSL-Bindungen usw.).

    ![Bestätigen des Swap-Vorgangs](maintainable-azure-websites-managing-change-and-scale/_static/image45.png)

    *Bestätigen des Swap-Vorgangs*
3. Wählen Sie nach Abschluss des Austauschs den Produktions Slot aus, und klicken Sie in der Befehlsleiste auf **Durchsuchen** , um die Produktions Website zu öffnen. Beachten Sie die URL in der Adressleiste.

    > [!NOTE]
    > Möglicherweise müssen Sie Ihren Browser aktualisieren, um den Cache zu löschen. In Internet Explorer können Sie dazu **STRG + R**drücken.

    ![Ausführung der Web-App in der Produktionsumgebung](maintainable-azure-websites-managing-change-and-scale/_static/image46.png)
4. Aktualisieren Sie in der **gitbash** -Konsole die Remote-URL für das lokale git-Repository auf den Produktions Slot. Führen Sie hierzu den folgenden Befehl aus, und ersetzen Sie die Platzhalter durch Ihren Benutzernamen für die Bereitstellung und den Namen Ihrer Web-App.

    > [!NOTE]
    > In den folgenden Übungen verschieben Sie die Änderungen an die Produktions Website, anstatt Sie nur für die Einfachheit des Labs zu Staging. In einem realen Szenario empfiehlt es sich, die Änderungen in der Stagingumgebung vor der herauf Stufung in die Produktion zu überprüfen.

    [!code-console[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample13.cmd)]

<a id="Exercise3"></a>
### <a name="exercise-3-performing-deployment-rollback-in-production"></a>Übung 3: Ausführen eines Rollbacks für die Bereitstellung in der Produktion

Es gibt Szenarien, in denen Sie keinen stagingslot haben, um einen aktiven Austausch zwischen Staging und Produktion auszuführen, z. b. Wenn Sie mit dem Modus " **Free** " oder " **Shared** " arbeiten. In diesen Szenarien sollten Sie Ihre Anwendung in einer Testumgebung testen – entweder lokal oder an einem Remote Standort – vor der Bereitstellung in der Produktion. Es ist jedoch möglich, dass ein während der Testphase nicht erkanntes Problem am Produktionsstandort auftritt. In diesem Fall ist es wichtig, dass Sie über einen Mechanismus verfügen, um so schnell wie möglich zu einer früheren und stabileren Version der Anwendung zu wechseln.

In **Azure App Service**ermöglicht Continuous Deployment der Quell Code Verwaltung dies dank der Aktion " **erneut** bereitstellen", die im Verwaltungs Portal verfügbar ist. Azure verfolgt die bereit Stellungen, die den Commits zugeordnet sind, die an das Repository übermittelt wurden, und bietet eine Option zum erneuten Bereitstellen Ihrer Anwendung mithilfe Ihrer vorherigen bereit Stellungen.

In dieser Übung führen Sie eine Änderung am Code in der Anwendung " **Geek Quiz** " aus, die absichtlich einen *Fehler*einfügt. Sie stellen die Anwendung in der Produktionsumgebung bereit, um den Fehler anzuzeigen, und dann nutzen Sie die Funktion zum erneuten bereitstellen, um zum vorherigen Zustand zurückzukehren.

<a id="Ex3Task1"></a>
#### <a name="task-1--updating-the-geek-quiz-application"></a>Aufgabe 1 – Aktualisieren der Anwendung "Geek Quiz"

In dieser Aufgabe gestalten Sie einen kleinen Teil des Codes der **triviacontroller** -Klasse um, um einen Teil der Logik zu extrahieren, der die ausgewählte Quiz Option aus der Datenbank in eine neue Methode abruft.

1. Wechseln Sie in der vorherigen Übung zur Visual Studio-Instanz mit der **geekquiz** -Lösung.
2. Öffnen Sie in **Projektmappen-Explorer**die Datei **TriviaController.cs** im Ordner **Controllers** .
3. Suchen Sie nach der **storeasync** -Methode, und wählen Sie den in der folgenden Abbildung markierten Code aus.

    ![Auswählen des Codes](maintainable-azure-websites-managing-change-and-scale/_static/image47.png)

    *Auswählen des Codes*
4. Klicken Sie mit der rechten Maustaste auf den ausgewählten Code, erweitern Sie das Menü **umgestalten** , und wählen Sie **Methode extrahieren...** aus.

    ![Extrahieren des Codes als neue Methode](maintainable-azure-websites-managing-change-and-scale/_static/image48.png)

    *Auswählen der Methode extrahieren*
5. Benennen Sie im Dialogfeld **Methode extrahieren** die neue Methode *matchesoption* , und klicken Sie auf **OK**.

    ![Angeben des Methoden namens](maintainable-azure-websites-managing-change-and-scale/_static/image49.png)

    *Angeben des Namens für die extrahierte Methode*
6. Der ausgewählte Code wird dann in die **matchesoption** -Methode extrahiert. Der resultierende Code wird im folgenden Code Ausschnitt gezeigt.

    [!code-csharp[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample14.cs)]
7. Drücken Sie **STRG + S** , um die Änderungen zu speichern.

<a id="Ex3Task2"></a>
#### <a name="task-2--redeploying-the-geek-quiz-application"></a>Aufgabe 2 – ernedas erneute Bereitstellen der Anwendung "Geek Quiz"

Sie schieben nun die Änderungen, die Sie in der vorherigen Aufgabe vorgenommen haben, an das Repository, das eine neue Bereitstellung in der Produktionsumgebung auslöst. Dann beheben Sie ein Problem mithilfe der von Internet Explorer bereitgestellten **F12-Entwicklungs Tools** und führen dann ein Rollback zur vorherigen Bereitstellung über das Azure-Verwaltungs Portal aus.

1. Öffnen Sie eine neue **git-bash** -Konsole, um die aktualisierte Anwendung für Azure App Service bereitzustellen.
2. Führen Sie die folgenden Befehle aus, um die Änderungen an Azure zu überführen. Aktualisieren Sie den Platzhalter *[your-application-path]* mit dem Pfad zur **geekquiz-Projekt** Mappe. Sie werden zur Eingabe Ihres Bereitstellungs Kennworts aufgefordert.

    [!code-console[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample15.cmd)]

    ![Übertragen von umgestalteten Code in Azure per Push](maintainable-azure-websites-managing-change-and-scale/_static/image50.png)

    *Übertragen von umgestalteten Code in Azure per Push*
3. Öffnen Sie Internet Explorer, und navigieren Sie zu Ihrer Web-App (z. b. `http://<your-web-site>.azurewebsites.net`). Melden Sie sich mit den zuvor erstellten Anmelde Informationen an.
4. Drücken Sie **F12** , um die Entwicklungs Tools zu starten, wählen Sie die Registerkarte **Netzwerk** aus, und klicken Sie dann auf **die Schaltfläche** wiedergeben

    ![Netzwerk Aufzeichnung wird gestartet](maintainable-azure-websites-managing-change-and-scale/_static/image51.png "Netzwerk Aufzeichnung wird gestartet")

    *Netzwerk Aufzeichnung wird gestartet*
5. Wählen Sie eine beliebige Option des Quiz aus. Sie werden festzustellen, dass nichts passiert.
6. Im Fenster **F12** zeigt der Eintrag, der der HTTP-Anforderung Post entspricht, ein HTTP **500** -Ergebnis an.

    ![HTTP 500-Fehler](maintainable-azure-websites-managing-change-and-scale/_static/image52.png)

    *HTTP 500-Fehler*
7. Wählen Sie die Registerkarte **Konsole** aus. Ein Fehler wird mit den Details der Ursache protokolliert.

    ![Protokollierter Fehler](maintainable-azure-websites-managing-change-and-scale/_static/image53.png)

    *Protokollierter Fehler*
8. Suchen Sie den Abschnitt "Details" des Fehlers. Dieser Fehler wird eindeutig durch den Code umgestaltet, den Sie in den vorherigen Schritten committet haben.

    [https://login.microsoftonline.com/consumers/](`Details: LINQ to Entities does not recognize the method 'Boolean MatchesOption ...`).
9. Schließen Sie den Browser nicht.
10. Navigieren Sie in einer neuen Browser Instanz zum [Azure-Verwaltungs Portal](https://manage.windowsazure.com) , und melden Sie sich mit der Microsoft-Konto an, die Ihrem Abonnement zugeordnet ist.
11. Wählen Sie **Websites** aus, und klicken Sie auf die in Übung 2 erstellte Web-App.
12. Navigieren Sie zur Seite bereit **Stellungen** . Beachten Sie, dass alle ausgeführten Commits im Bereitstellungs Verlauf aufgeführt sind.

    ![Liste vorhandener bereit Stellungen](maintainable-azure-websites-managing-change-and-scale/_static/image54.png)

    *Liste vorhandener bereit Stellungen*
13. Wählen Sie den vorherigen Commit aus, und klicken Sie auf der Befehlsleiste auf **erneut** bereitstellen.

    ![Der vorherige Commit wird erneut bereitgestellt.](maintainable-azure-websites-managing-change-and-scale/_static/image55.png)

    *Der vorherige Commit wird erneut bereitgestellt.*
14. Wenn Sie aufgefordert werden, die Auswahl zu bestätigen, klicken Sie auf **Ja**.

    ![Bestätigung der erneuten Bereitstellung](maintainable-azure-websites-managing-change-and-scale/_static/image56.png)
15. Wenn die Bereitstellung abgeschlossen ist, wechseln Sie zurück zur Browser Instanz mit Ihrer Web-App, und drücken Sie **STRG + F5**.
16. Klicken Sie auf eine der Optionen. Die Flip-Animation wird jetzt durchgeführt, und das Ergebnis (*richtig/falsch*) wird angezeigt.
17. Optionale Wechseln Sie zur **git-bash** -Konsole, und führen Sie die folgenden Befehle aus, um zum vorherigen Commit zurückzukehren.

    > [!NOTE]
    > Mit diesen Befehlen wird ein neuer Commit erstellt, mit dem alle Änderungen im git-Repository, die im ungültigen Commit vorgenommen wurden, übernommen werden. Azure stellt die Anwendung dann mit dem neuen Commit erneut bereit.

    [!code-console[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample16.cmd)]

<a id="Exercise4"></a>
### <a name="exercise-4-scaling-using-azure-storage"></a>Übung 4: Skalierung mithilfe Azure Storage

**BLOB** sind die einfachste Möglichkeit zum Speichern großer Mengen unstrukturierter Text-oder Binärdaten, z. b. Videos, Audiodateien und Bilder. Wenn Sie den statischen Inhalt der Anwendung in den Speicher verschieben, ist es hilfreich, Ihre Anwendung zu skalieren, indem Sie Images oder Dokumente direkt im Browser reservieren.

In dieser Übung verschieben Sie den statischen Inhalt der Anwendung in einen BLOB-Container. Anschließend konfigurieren Sie die Anwendung so, dass Sie eine **ASP.net URL Rewrite-Regel** in der Datei " **Web. config** " hinzufügt, um Ihre Inhalte in den BlobContainer umzuleiten.

<a id="Ex4Task1"></a>
#### <a name="task-1--creating-an-azure-storage-account"></a>Aufgabe 1 – Erstellen eines Azure Storage Kontos

In dieser Aufgabe erfahren Sie, wie Sie ein neues Speicherkonto mithilfe des Verwaltungs Portals erstellen.

1. Navigieren Sie zum [Azure-Verwaltungs Portal](https://manage.windowsazure.com) , und melden Sie sich mit der Microsoft-Konto an, die Ihrem Abonnement zugeordnet ist.
2. **Neu auswählen | Data Services | Speicher | Schneller** Fassung, um mit der Erstellung eines neuen Speicher Kontos zu beginnen. Geben Sie einen eindeutigen Namen für das Konto ein, und wählen Sie eine **Region** aus der Liste aus. Klicken Sie auf **Speicherkonto erstellen** , um fortzufahren.

    ![Erstellen eines neuen Speicher Kontos](maintainable-azure-websites-managing-change-and-scale/_static/image57.png "Erstellen eines neuen Speicher Kontos")

    *Erstellen eines neuen Speicher Kontos*
3. Warten Sie im Abschnitt **Speicher** , bis der Status des neuen Speicher Kontos in *Online* geändert wird, um mit dem folgenden Schritt fortzufahren.

    ![Speicherkonto erstellt](maintainable-azure-websites-managing-change-and-scale/_static/image58.png "Speicherkonto erstellt")

    *Speicherkonto erstellt*
4. Klicken Sie auf den Namen des Speicher Kontos, und klicken Sie dann oben auf der Seite auf den Link **Dashboard** . Auf der Seite **Dashboard** finden Sie Informationen über den Status des Kontos und die Dienst Endpunkte, die in Ihren Anwendungen verwendet werden können.

    ![Anzeigen des Speicherkonto-Dashboards](maintainable-azure-websites-managing-change-and-scale/_static/image59.png "Anzeigen des Speicherkonto-Dashboards")

    *Anzeigen des Speicherkonto-Dashboards*
5. Klicken Sie in der Navigationsleiste auf die Schaltfläche **Zugriffsschlüssel verwalten** .

    ![Schaltfläche zum Verwalten von Zugriffs Schlüsseln](maintainable-azure-websites-managing-change-and-scale/_static/image60.png "Schaltfläche zum Verwalten von Zugriffs Schlüsseln")

    *Schaltfläche zum Verwalten von Zugriffs Schlüsseln*
6. Kopieren Sie im Dialogfeld **Zugriffsschlüssel verwalten** den **Speicherkonto Namen** und den **primären Zugriffsschlüssel** , da Sie ihn in der folgenden Übung benötigen. Schließen Sie anschließend das Dialogfeld.

    ![Dialogfeld "Zugriffsschlüssel verwalten"](maintainable-azure-websites-managing-change-and-scale/_static/image61.png "Dialogfeld "Zugriffsschlüssel verwalten"")

    *Dialogfeld "Zugriffsschlüssel verwalten"*

<a id="Ex4Task2"></a>
#### <a name="task-2--uploading-an-asset-to-azure-blob-storage"></a>Aufgabe 2 – Hochladen eines Assets in Azure BLOB Storage

In dieser Aufgabe verwenden Sie das Server-Explorer Fenster in Visual Studio, um eine Verbindung mit Ihrem Speicherkonto herzustellen. Anschließend erstellen Sie einen BLOB-Container und laden eine Datei mit dem Geek-Quiz-Logo in den Container hoch.

1. Wechseln Sie in der vorherigen Übung zur Visual Studio-Instanz mit der **geekquiz** -Lösung.
2. Wählen Sie in der Menüleiste **Ansicht** aus, und klicken Sie dann auf **Server-Explorer**.
3. Klicken Sie in **Server-Explorer**mit der rechten Maustaste auf den Knoten **Azure** , und wählen Sie **mit Azure verbinden...** aus. Melden Sie sich mit dem Microsoft-Konto an, der Ihrem Abonnement zugeordnet ist.

    ![Herstellen einer Verbindung mit Windows Azure](maintainable-azure-websites-managing-change-and-scale/_static/image62.png)

    *Herstellen einer Verbindung mit Azure*
4. Erweitern Sie den Knoten **Azure** , klicken Sie mit der rechten Maustaste auf **Speicher** , und wählen Sie **externen Speicher anfügen**aus.
5. Geben Sie im Dialogfeld **Neues Speicherkonto hinzufügen** den **Kontonamen** und den **Kontoschlüssel** ein, die Sie in der vorherigen Aufgabe abgerufen haben, und klicken Sie auf **OK**.

    ![Dialogfeld "neues Speicherkonto hinzufügen"](maintainable-azure-websites-managing-change-and-scale/_static/image63.png)

    *Dialogfeld "neues Speicherkonto hinzufügen"*
6. Ihr Speicherkonto sollte unter dem Knoten **Speicher** angezeigt werden. Erweitern Sie das Speicherkonto, klicken Sie mit der rechten Maustaste auf **BLOBs** , und wählen Sie **BlobContainer erstellen...** aus.

    ![BLOB-Container erstellen](maintainable-azure-websites-managing-change-and-scale/_static/image64.png "BLOB-Container erstellen")

    *BLOB-Container erstellen*
7. Geben Sie im Dialogfeld **BLOB-Container erstellen** einen Namen für den BlobContainer ein, und klicken Sie auf **OK**.

    ![Dialogfeld "BLOB-Container erstellen"](maintainable-azure-websites-managing-change-and-scale/_static/image65.png "Dialogfeld "BLOB-Container erstellen"")

    *Dialogfeld "BLOB-Container erstellen"*
8. Der neue BLOB-Container sollte dem Knoten **BLOBs** hinzugefügt werden. Ändern Sie die Zugriffsberechtigungen im Container, um den Container als öffentlich zu gestalten. Klicken Sie hierzu mit der rechten Maustaste auf den Container **Images** , und wählen Sie **Eigenschaften**aus.

    ![Bild Container Eigenschaften](maintainable-azure-websites-managing-change-and-scale/_static/image66.png "Bild Container Eigenschaften")

    *Bild Container Eigenschaften*
9. Legen Sie im Fenster **Eigenschaften** den **öffentlichen Lesezugriff** auf **Container**fest.

    ![Ändern der Eigenschaft für den öffentlichen Lesezugriff](maintainable-azure-websites-managing-change-and-scale/_static/image67.png "Ändern der Eigenschaft für den öffentlichen Lesezugriff")

    *Ändern der Eigenschaft für den öffentlichen Lesezugriff*
10. Wenn Sie gefragt werden, ob Sie die öffentliche Zugriffs Eigenschaft ändern möchten, klicken Sie auf **Ja**.

    ![Microsoft Visual Studio Warnung](maintainable-azure-websites-managing-change-and-scale/_static/image68.png "Microsoft Visual Studio Warnung")

    *Microsoft Visual Studio Warnung*
11. Klicken Sie in **Server-Explorer**mit der rechten Maustaste in den BlobContainer **Images** , und wählen Sie **BlobContainer anzeigen**aus.

    ![BLOB-Container anzeigen](maintainable-azure-websites-managing-change-and-scale/_static/image69.png "BLOB-Container anzeigen")

    *BLOB-Container anzeigen*
12. Der Container Images sollte in einem neuen Fenster geöffnet werden, und eine Legende ohne Einträge sollte angezeigt werden. Klicken Sie auf das Symbol **hochladen** , um eine Datei in den BLOB-Container hochzuladen.

    ![Bild Container ohne Einträge](maintainable-azure-websites-managing-change-and-scale/_static/image70.png "Bild Container ohne Einträge")

    *Bild Container ohne Einträge*
13. Navigieren Sie im Dialogfeld **BLOB hochladen** zum Ordner **Assets** des Labs. Wählen Sie die Datei **Logo-Big. png** aus, und klicken Sie auf **Öffnen**.
14. Warten Sie, bis die Datei hochgeladen wurde. Wenn der Upload abgeschlossen ist, sollte die Datei im Container Images aufgeführt werden. Klicken Sie mit der rechten Maustaste auf den Datei Eintrag, und wählen Sie **URL kopieren**

    ![BLOB-URL kopieren](maintainable-azure-websites-managing-change-and-scale/_static/image71.png "Kopieren der BLOB-Datei-URL")

    *BLOB-URL kopieren*
15. Öffnen Sie Internet Explorer, und fügen Sie die URL ein. Das folgende Bild sollte im Browser angezeigt werden.

    ![Logo-Big. png-Bild von Windows BLOB Storage](maintainable-azure-websites-managing-change-and-scale/_static/image72.png "Logo-Big. png-Bild aus Storage")

    *Logo-Big. png-Bild von Azure BLOB Storage*

<a id="Ex4Task3"></a>
#### <a name="task-3--updating-the-solution-to-consume-static-content-from-azure-blob-storage"></a>Aufgabe 3 – Aktualisieren der Lösung zur Nutzung statischer Inhalte aus Azure BLOB Storage

In dieser Aufgabe konfigurieren Sie die **geekquiz-Projekt** Mappe so, dass das Bild, das in Azure BLOB Storage (anstelle des Bilds in der Web-App) hochgeladen wird, durch Hinzufügen einer ASP.net-URL-Rewrite-Regel in der **Web. config** -Datei verwendet wird.

1. Öffnen Sie in Visual Studio die Datei **Web. config** im **geekquiz** -Projekt, und suchen Sie nach dem **&lt;System. Webserver&gt;** -Element.
2. Fügen Sie den folgenden Code hinzu, um eine URL-Rewrite-Regel hinzuzufügen, und aktualisieren Sie den Platzhalter mit Ihrem Speicherkonto Namen.

    (Code Ausschnitt- *websitesinproduction-Ex4-urlreschreiterule*)

    [!code-xml[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample17.xml)]

    > [!NOTE]
    > Das Umschreiben von URLs ist der Prozess, bei dem eine eingehende Webanforderung abgefangen und die Anforderung an eine andere Ressource umgeleitet wird. Die URL-Umschreib Regeln weisen die Umschreib-Engine an, wenn eine Anforderung umgeleitet werden muss, und wohin Sie umgeleitet werden soll. Eine Umschreib Regel besteht aus zwei Zeichen folgen: dem Muster, nach dem in der angeforderten URL gesucht werden soll (normalerweise mithilfe regulärer Ausdrücke), und der Zeichenfolge, mit der das Muster ersetzt werden soll, sofern gefunden. Weitere Informationen finden Sie unter [URL-Umschreibung in ASP.net](https://msdn.microsoft.com/library/ms972974.aspx).
3. Drücken Sie **STRG + S** , um die Änderungen zu speichern.
4. Öffnen Sie eine neue **git-bash** -Konsole, um die aktualisierte Anwendung für Azure App Service bereitzustellen.
5. Führen Sie die folgenden Befehle aus, um die Änderungen an Azure zu überführen. Aktualisieren Sie den Platzhalter *[your-application-path]* mit dem Pfad zur **geekquiz-Projekt** Mappe. Sie werden zur Eingabe Ihres Bereitstellungs Kennworts aufgefordert.

    [!code-console[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample18.cmd)]

    ![Bereitstellen von Updates in Azure](maintainable-azure-websites-managing-change-and-scale/_static/image73.png)

    *Bereitstellen von Updates in Azure*

<a id="Ex4Task4"></a>
#### <a name="task-4--verification"></a>Aufgabe 4 – Überprüfung

In dieser Aufgabe verwenden Sie **Internet Explorer** , um die Anwendung " **Geek Quiz** " zu durchsuchen und zu überprüfen, ob die URL-Rewrite-Regel für Images funktioniert und Sie zu dem auf **Azure BLOB Storage**gehosteten Image umgeleitet werden.

1. Öffnen Sie Internet Explorer, und navigieren Sie zu Ihrer Web-App (z. b. `http://<your-web-site>.azurewebsites.net`). Melden Sie sich mit den zuvor erstellten Anmelde Informationen an.

    ![Anzeige der Geek Quiz-Web-App mit dem Image](maintainable-azure-websites-managing-change-and-scale/_static/image74.png "Anzeige der Geek Quiz-Web-App mit dem Image")

    *Anzeige der Geek Quiz-Web-App mit dem Image*
2. Drücken Sie **F12** , um die Entwicklungs Tools zu starten, wählen Sie die Registerkarte **Netzwerk** und dann Aufzeichnung starten aus.

    ![Netzwerk Aufzeichnung wird gestartet](maintainable-azure-websites-managing-change-and-scale/_static/image75.png "Netzwerk Aufzeichnung wird gestartet")

    *Netzwerk Aufzeichnung wird gestartet*
3. Drücken Sie **STRG + F5** , um die Webseite zu aktualisieren.
4. Nachdem die Seite geladen wurde, sollten Sie eine HTTP-Anforderung für die **/IMG/Logo-Big.png** -URL mit einem HTTP **301** -Ergebnis (Redirect) und eine weitere Anforderung für `http://[YOUR-STORAGE-ACCOUNT].blob.core.windows.net/images/logo-big.png`-URL mit einem HTTP **200** -Ergebnis sehen.

    ![Überprüfen der URL-Umleitung](maintainable-azure-websites-managing-change-and-scale/_static/image76.png "Die Umleitung in Entwicklungs Tools wird angezeigt.")

    *Überprüfen der URL-Umleitung*

<a id="Exercise5"></a>
### <a name="exercise-5-using-autoscale-for-web-apps"></a>Übung 5: Verwenden der automatischen Skalierung für Web-Apps

> [!NOTE]
> Diese Übung ist optional, da Sie Unterstützung für Web Load &amp; Leistungstests erfordert, die nur für **Visual Studio 2013 Ultimate Edition**verfügbar ist. Weitere Informationen zu bestimmten Visual Studio 2013 Funktionen [finden Sie hier](https://www.microsoft.com/visualstudio/eng/products/compare).

**Azure App Service-Web-Apps** bietet die Funktion für die automatische Skalierung für Web-Apps im **Standard Modus**. Die automatische Skalierung ermöglicht es Azure, die Anzahl der Instanzen Ihrer Web-App abhängig von der Auslastung automatisch zu skalieren. Wenn die automatische Skalierung aktiviert ist, überprüft Azure alle fünf Minuten die CPU Ihrer Web-App und fügt zu diesem Zeitpunkt Instanzen nach Bedarf hinzu. Wenn die CPU-Auslastung gering ist, entfernt Azure alle zwei Stunden Instanzen, um sicherzustellen, dass die Leistung Ihrer Web-App nicht beeinträchtigt wird.

In dieser Übung führen Sie die Schritte aus, die zum Konfigurieren des Features für die **Automatische Skalierung** für die **Geek Quiz** -Web-App erforderlich sind. Sie überprüfen dieses Feature, indem Sie einen Visual Studio-Auslastungs Test ausführen, um eine ausreichende CPU-Auslastung für die Anwendung zu generieren und ein instanzupgrade auslösen

<a id="Ex5Task1"></a>
#### <a name="task-1--configuring-autoscale-based-on-the-cpu-metric"></a>Aufgabe 1 – Konfigurieren der automatischen Skalierung basierend auf der CPU-Metrik

In dieser Aufgabe verwenden Sie das Azure-Verwaltungs Portal, um die Funktion für die automatische Skalierung für die Web-App zu aktivieren, die Sie in Übung 2 erstellt haben.

1. Wählen Sie im [Azure-Verwaltungs Portal](https://manage.windowsazure.com/) **Websites** aus, und klicken Sie auf die Web-App, die Sie in Übung 2 erstellt haben.
2. Navigieren Sie zur Seite **skalieren** . Wählen Sie im Abschnitt **Kapazität** für die Konfiguration **nach Metrik skalieren** die Option **CPU** aus.

    > [!NOTE]
    > Bei der Skalierung nach CPU passt Azure die Anzahl der von der APP verwendeten Instanzen dynamisch an, wenn sich die CPU-Auslastung ändert.

    ![Auswählen der Skalierung nach CPU](maintainable-azure-websites-managing-change-and-scale/_static/image77.png "Auswählen der CPU-Metrik für die automatische Skalierung")

    *Auswählen der Skalierung nach CPU*
3. Ändern Sie die **Ziel-CPU** -Konfiguration in **20**-**40** Prozent.

    > [!NOTE]
    > Dieser Bereich stellt die durchschnittliche CPU-Auslastung für Ihre Web-App dar. Azure wird Instanzen hinzufügen oder entfernen, um Ihre Web-App in diesem Bereich zu speichern. Die minimale und maximale Anzahl von Instanzen, die für die Skalierung verwendet werden, wird in der **Instanzanzahl** konfiguriert. Azure geht nie über diesen Grenzwert hinaus.
    >
    > Die standardmäßigen **CPU-Ziel** Werte werden nur für die Zwecke dieses Labs geändert. Wenn Sie den CPU-Bereich mit kleinen Werten konfigurieren, erhöhen Sie die Wahrscheinlichkeit, dass die automatische Skalierung auslöst, wenn eine mittlere Auslastung in der Anwendung erfolgt.

    ![Ändern der Ziel-CPU zwischen 20 und 40 Prozent](maintainable-azure-websites-managing-change-and-scale/_static/image78.png "Ändern der Ziel-CPU zwischen 20 und 40 Prozent")

    *Ändern der Ziel-CPU zwischen 20 und 40 Prozent*
4. Klicken Sie in der Befehlsleiste auf **Speichern** , um die Änderungen zu speichern.

<a id="Ex5Task2"></a>
#### <a name="task-2--load-testing-with-visual-studio"></a>Aufgabe 2 – Auslastungs Tests mit Visual Studio

Nachdem die **Automatische Skalierung** konfiguriert wurde, erstellen Sie ein **webleistungs-und Auslastungs Test Projekt** in Visual Studio, um eine CPU-Auslastung für Ihre Web-App zu generieren.

1. Öffnen Sie **Visual Studio Ultimate 2013** , und wählen Sie Datei aus.  **Neu | Projekt...** , um eine neue Projekt Mappe zu starten.

    ![Erstellen eines neuen Projekts](maintainable-azure-websites-managing-change-and-scale/_static/image79.png "Erstellen eines neuen Projekts")

    *Erstellen eines neuen Projekts*
2. Wählen Sie im Dialogfeld **Neues Projekt** unter dem visuellen **C# Element die Option webleistungs-und Auslastungs Test Projekt aus.** Registerkarte "Test". Stellen Sie sicher, dass **.NET Framework 4,5** ausgewählt ist, nennen Sie das Projekt *webandloadtestproject*, **Wählen Sie einen** **Speicherort** aus, und klicken

    ![Erstellen eines neuen Web-und Auslastungs Test Projekts](maintainable-azure-websites-managing-change-and-scale/_static/image80.png "Erstellen eines neuen Web-und Auslastungs Test Projekts")

    *Erstellen eines neuen Web-und Auslastungs Test Projekts*
3. Klicken Sie im **WebTest1. Webtest** mit der rechten Maustaste auf den Knoten **WebTest1** , und klicken Sie auf **Anforderung hinzufügen**.

    ![Hinzufügen einer Anforderung zu WebTest1](maintainable-azure-websites-managing-change-and-scale/_static/image81.png "Hinzufügen einer Anforderung zu WebTest1")

    *Hinzufügen einer Anforderung zu WebTest1*
4. Aktualisieren Sie im Fenster **Eigenschaften** des Knotens neue Anforderung die **URL** -Eigenschaft so, dass Sie auf die URL Ihrer Web-App verweist (z. b. *[http://geek-quiz.azurewebsites.net/](http://geek-quiz.azurewebsites.net/)* ).

    ![Ändern der URL-Eigenschaft](maintainable-azure-websites-managing-change-and-scale/_static/image82.png "Ändern der URL-Eigenschaft")

    *Ändern der URL-Eigenschaft*
5. Klicken Sie im Fenster **WebTest1. Webtest** mit der rechten Maustaste auf **WebTest1** , und klicken Sie auf **Schleife hinzufügen...** .

    ![Hinzufügen einer Schleife zu WebTest1](maintainable-azure-websites-managing-change-and-scale/_static/image83.png "Hinzufügen einer Schleife zu WebTest1")

    *Hinzufügen einer Schleife zu WebTest1*
6. Wählen Sie im Dialogfeld **bedingte Regel und Elemente zu Schleife hinzufügen** die **for-Schleifen Regel aus** , und ändern Sie die folgenden Eigenschaften.

   1. **Terminierender Wert:** 1000
   2. **Kontext Parameter Name:** Iterator
   3. **Inkrement-Wert:** 1

      ![Auswählen der for-Schleifen Regel und Aktualisieren der Eigenschaften](maintainable-azure-websites-managing-change-and-scale/_static/image84.png "Auswählen der for-Schleifen Regel und Aktualisieren der Eigenschaften")

      *Auswählen der for-Schleifen Regel und Aktualisieren der Eigenschaften*
7. Wählen Sie im Abschnitt **Elemente in der Schleife** die zuvor erstellte Anforderung als erstes und letztes Element für die Schleife aus. Klicken Sie zum Fortsetzen des Vorgangs auf **OK** .

    ![Auswählen der ersten und letzten Elemente für die Schleife](maintainable-azure-websites-managing-change-and-scale/_static/image85.png "Auswählen der ersten und letzten Elemente für die Schleife")

    *Auswählen der ersten und letzten Elemente für die Schleife*
8. Klicken Sie in **Projektmappen-Explorer**mit der rechten Maustaste auf das Projekt **webandloadtestproject** , erweitern Sie das Menü **Hinzufügen** , und wählen Sie **Auslastungs Test**aus.

    ![Hinzufügen eines Auslastungs Tests zum Projekt "webandloadtestproject"](maintainable-azure-websites-managing-change-and-scale/_static/image86.png "Hinzufügen eines Auslastungs Tests zum Projekt "webandloadtestproject"")

    *Hinzufügen eines Auslastungs Tests zum Projekt "webandloadtestproject"*
9. Klicken Sie im Dialogfeld **neue Auslastungstest-Assistent** auf **weiter**.

    ![Neue Auslastungstest-Assistent](maintainable-azure-websites-managing-change-and-scale/_static/image87.png "Neue Auslastungstest-Assistent")

    *Neue Auslastungstest-Assistent*
10. Wählen Sie auf der Seite **Szenario** die Option **keine Gedanken Zeiten verwenden** aus, und klicken Sie auf **weiter**.

    ![Auswählen von "nicht für die Verwendung von"](maintainable-azure-websites-managing-change-and-scale/_static/image88.png "Auswählen von "nicht für die Verwendung von"")

    *Auswählen von "nicht für die Verwendung von"*
11. Vergewissern Sie sich auf der Seite **Auslastungs Muster** , dass die Option **konstantes laden** ausgewählt ist. Ändern Sie die Einstellung **Benutzer Anzahl** in **250** Benutzer, und klicken Sie auf **weiter**.

    ![Ändern der Benutzer Anzahl in 250](maintainable-azure-websites-managing-change-and-scale/_static/image89.png "Ändern der Benutzer Anzahl in 250")

    *Ändern der Benutzer Anzahl in 250*
12. Wählen Sie auf der Seite **Test Mischungs Modell** die Option **basierend auf sequenzieller Test Reihenfolge** , **und klicken Sie**

    ![Auswählen des Test Mischungs Modells](maintainable-azure-websites-managing-change-and-scale/_static/image90.png "Auswählen des Test Mischungs Modells")

    *Auswählen des Test Mischungs Modells*
13. Klicken Sie auf der Seite **Test Mischungs Modell** auf **hinzufügen...** , um der Mischung einen Test hinzuzufügen.

    ![Hinzufügen eines Tests zur Test Mischung](maintainable-azure-websites-managing-change-and-scale/_static/image91.png "Hinzufügen eines Tests zur Test Mischung")

    *Hinzufügen eines Tests zur Test Mischung*
14. Doppelklicken Sie im Dialogfeld **Tests hinzufügen** auf **WebTest1** , um den Test der Liste **Ausgewählte Tests** hinzuzufügen. Klicken Sie zum Fortsetzen des Vorgangs auf **OK** .

    ![Hinzufügen des WebTest1-Tests](maintainable-azure-websites-managing-change-and-scale/_static/image92.png "Hinzufügen des WebTest1-Tests")

    *Hinzufügen des WebTest1-Tests*
15. Klicken Sie auf der Seite " **Test Mischung** " auf **weiter**.

    ![Fertigstellen der Seite "Test Mischung"](maintainable-azure-websites-managing-change-and-scale/_static/image93.png "Fertigstellen der Seite "Test Mischung"")

    *Fertigstellen der Seite "Test Mischung"*
16. Klicken Sie auf der Seite **Netzwerk Mischung** auf **weiter**.

    ![Klicken auf "weiter" auf der Seite "Netzwerk Mischung"](maintainable-azure-websites-managing-change-and-scale/_static/image94.png "Klicken auf "weiter" auf der Seite "Netzwerk Mischung"")

    *Klicken auf "weiter" auf der Seite "Netzwerk Mischung"*
17. Wählen Sie auf der Seite **Browser Mischung** **Internet Explorer 10,0** als Browsertyp aus, und klicken Sie auf **weiter**.

    ![Auswählen des Browsertyps](maintainable-azure-websites-managing-change-and-scale/_static/image95.png "Auswählen des Browsertyps")

    *Auswählen des Browsertyps*
18. Klicken Sie auf der Seite **Indikatorensätze** auf **weiter**.

    ![Klicken auf der Seite Indikatorensätze auf weiter](maintainable-azure-websites-managing-change-and-scale/_static/image96.png "Klicken auf der Seite Indikatorensätze auf weiter")

    *Klicken auf der Seite Indikatorensätze auf weiter*
19. Legen Sie auf der Seite Test **Lauf Einstellungen** den **Auslastungs Test** auf **5 Minuten** fest, und klicken Sie auf **Fertig**stellen.

    ![Festlegen der Dauer des Auslastungs Tests auf 5 Minuten](maintainable-azure-websites-managing-change-and-scale/_static/image97.png "Festlegen der Dauer des Auslastungs Tests auf 5 Minuten")

    *Festlegen der Dauer des Auslastungs Tests auf 5 Minuten*
20. Doppelklicken Sie in **Projektmappen-Explorer**auf die Datei **local. Settings** , um die Testeinstellungen zu untersuchen. Standardmäßig verwendet Visual Studio den lokalen Computer, um die Tests auszuführen.

    > [!NOTE]
    > Alternativ können Sie das Testprojekt so konfigurieren, dass die Auslastungs Tests in der Cloud mit **Azure Test Plans**ausgeführt werden. Azure Test Plans bietet einen cloudbasierten Auslastungs Test Dienst, der eine realistischere Last simuliert, sodass Einschränkungen in der lokalen Umgebung wie CPU-Kapazität, verfügbarer Arbeitsspeicher und Netzwerkbandbreite vermieden werden. Weitere Informationen zum Verwenden von Azure Test Plans zum Ausführen von Auslastungs Tests finden Sie unter [Auslastungs Testszenarien](/azure/devops/test/load-test/overview?view=vsts).

    ![Testeinstellungen](maintainable-azure-websites-managing-change-and-scale/_static/image98.png)

<a id="Ex5Task3"></a>
#### <a name="task-3--autoscale-verification"></a>Aufgabe 3 – automatische Skalierungs Überprüfung

Nun führen Sie den Auslastungs Test aus, den Sie in der vorherigen Aufgabe erstellt haben, und sehen, wie sich Ihre Web-App unter Last verhält.

1. Doppelklicken Sie in **Projektmappen-Explorer**auf **loadtest1. LoadTest** , um den Auslastungs Test zu öffnen.

    ![Öffnen von "loadtest1. LoadTest"](maintainable-azure-websites-managing-change-and-scale/_static/image99.png "Öffnen von "loadtest1. LoadTest"")

    *Öffnen von "loadtest1. LoadTest"*
2. Klicken Sie im Fenster **loadtest1. LoadTest** auf die erste Schaltfläche in der Toolbox, um den Auslastungs Test auszuführen.

    ![Ausführen des Auslastungs Tests](maintainable-azure-websites-managing-change-and-scale/_static/image100.png "Ausführen des Auslastungs Tests")

    *Ausführen des Auslastungs Tests*
3. Warten Sie, bis der Auslastungs Test abgeschlossen ist.

    > [!NOTE]
    > Mit dem Auslastungs Test werden mehrere Benutzer simuliert, die gleichzeitig Anforderungen an die Web-App senden. Wenn der Test ausgeführt wird, können Sie die verfügbaren Leistungsindikatoren überwachen, um Fehler, Warnungen oder andere Informationen im Zusammenhang mit dem Auslastungs Testlauf zu erkennen.

    ![Auslastungs Test ausgeführt](maintainable-azure-websites-managing-change-and-scale/_static/image101.png "Warten, bis der Auslastungs Test abgeschlossen ist")

    *Auslastungs Test ausgeführt*
4. Nachdem der Test abgeschlossen ist, wechseln Sie zurück zum Verwaltungs Portal, und navigieren Sie zur Seite **skalieren** Ihrer Web-App. Im Abschnitt **Kapazität** sollte im Diagramm angezeigt werden, dass eine neue Instanz automatisch bereitgestellt wurde.

    ![Neue Instanz automatisch bereitgestellt](maintainable-azure-websites-managing-change-and-scale/_static/image102.png)

    *Neue Instanz automatisch bereitgestellt*

    > [!NOTE]
    > Es kann einige Minuten dauern, bis die Änderungen im Diagramm angezeigt werden (drücken Sie in regelmäßigen Abständen **STRG + F5** , um die Seite zu aktualisieren). Wenn keine Änderungen angezeigt werden, können Sie Folgendes versuchen:
    >
    > - Dauer des Auslastungs Tests erhöhen (z. b. auf **10 Minuten**)
    > - Reduzieren Sie die maximalen und minimalen Werte für den **Ziel-CPU** -Bereich in der Konfiguration für die automatische Skalierung Ihrer Web-App.
    > - Führen Sie den Auslastungs Test in der Cloud mit **Azure Test Plans**aus. Weitere Informationen finden Sie [hier](/azure/devops/test/load-test/index?view=vsts).

---

<a id="Summary"></a>
## <a name="summary"></a>Zusammenfassung

In dieser praktischen Übungseinheit haben Sie erfahren, wie Sie Ihre Anwendung in Produktions-Web-Apps in Azure einrichten und bereitstellen. Sie haben damit begonnen, die Datenbanken mithilfe **Entity Framework Code First-Migrationen**zu erkennen und zu aktualisieren. Anschließend haben Sie die Bereitstellung neuer Versionen Ihres Standorts mithilfe von **git** und die Durchführung von Rollbacks auf die neueste stabile Version Ihres Standorts fortgesetzt. Außerdem haben Sie erfahren, wie Sie Ihre App mithilfe von Speicher skalieren, um statische Inhalte in einen BLOB-Container zu verschieben.
