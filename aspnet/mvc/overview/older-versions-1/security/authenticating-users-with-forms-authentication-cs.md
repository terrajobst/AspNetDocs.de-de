---
uid: mvc/overview/older-versions-1/security/authenticating-users-with-forms-authentication-cs
title: Authentifizieren von Benutzern mit der FormularC#Authentifizierung () | Microsoft-Dokumentation
author: microsoft
description: Erfahren Sie, wie Sie das Attribut [autorisieren] zum Kenn Wort Schutz bestimmter Seiten in ihrer MVC-Anwendung verwenden. Sie erfahren, wie Sie auch die Website Verwaltung verwenden...
ms.author: riande
ms.date: 01/27/2009
ms.assetid: 239fd3ca-5630-4b8d-bc4b-2f906b1d3504
msc.legacyurl: /mvc/overview/older-versions-1/security/authenticating-users-with-forms-authentication-cs
msc.type: authoredcontent
ms.openlocfilehash: bed2eafa47fec25ac04cb07e0037f596494bb7d9
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78435447"
---
# <a name="authenticating-users-with-forms-authentication-c"></a>Authentifizieren von Benutzern bei der Formularauthentifizierung (C#)

von [Microsoft](https://github.com/microsoft)

> Erfahren Sie, wie Sie das Attribut [autorisieren] zum Kenn Wort Schutz bestimmter Seiten in ihrer MVC-Anwendung verwenden. Sie erfahren, wie Sie das Websiteverwaltungs-Tool zum Erstellen und Verwalten von Benutzern und Rollen verwenden. Außerdem erfahren Sie, wie Sie konfigurieren, wo Benutzerkonto-und Rollen Informationen gespeichert werden.

In diesem Tutorial wird erläutert, wie Sie die Formular Authentifizierung verwenden können, um die Ansichten in Ihren ASP.NET-MVC-Anwendungen per Kennwort zu schützen. Sie erfahren, wie Sie das Websiteverwaltungs-Tool verwenden, um Benutzer und Rollen zu erstellen. Außerdem erfahren Sie, wie Sie verhindern können, dass nicht autorisierte Benutzer Controller Aktionen aufrufen. Schließlich erfahren Sie, wie Sie konfigurieren, wo Benutzernamen und Kenn Wörter gespeichert werden.

#### <a name="using-the-web-site-administration-tool"></a>Verwenden des Websiteverwaltungs-Tools

Bevor wir etwas anderes tun, sollten wir zunächst einige Benutzer und Rollen erstellen. Die einfachste Möglichkeit zum Erstellen neuer Benutzer und Rollen ist die Nutzung des Visual Studio 2008-Websiteverwaltungs-Tools. Sie können dieses Tool starten, indem Sie die Menüoption **Project, ASP.net Configuration**auswählen. Alternativ dazu können Sie das Websiteverwaltungs-Tool starten, indem Sie auf das Symbol (etwas beängstigend) des Hammers klicken, der auf die Welt trifft, die am oberen Rand des Projektmappen-Explorer Fensters angezeigt wird (siehe Abbildung 1).

**Abbildung 1 – Starten des Websiteverwaltungs-Tools**

![clip_image002](authenticating-users-with-forms-authentication-cs/_static/image1.jpg)

Im Websiteverwaltungs-Tool erstellen Sie neue Benutzer und Rollen, indem Sie die Registerkarte Sicherheit auswählen. Klicken Sie auf den Link **Create User (Benutzer erstellen** ), um einen neuen Benutzernamens Stephen zu erstellen (siehe Abbildung 2). Geben Sie dem Stephen-Benutzer ein beliebiges Kennwort (z. b. *geheimer*Schlüssel).

**Abbildung 2 – Erstellen eines neuen Benutzers**

![clip_image004](authenticating-users-with-forms-authentication-cs/_static/image2.jpg)

Sie erstellen neue Rollen, indem Sie zunächst Rollen aktivieren und eine oder mehrere Rollen definieren. Aktivieren Sie Rollen, indem Sie auf den Link **Rollen aktivieren** klicken. Erstellen Sie als nächstes eine Rolle mit dem Namen *Administratoren* , indem Sie auf den Link **Rollen erstellen oder verwalten** klicken (siehe Abbildung 3).

**Abbildung 3 – Erstellen einer neuen Rolle**

![clip_image006](authenticating-users-with-forms-authentication-cs/_static/image3.jpg)

Erstellen Sie abschließend einen neuen Benutzer mit dem Namen "Sally", und ordnen Sie die Rolle "Administratoren" zu, indem Sie auf den Link "Benutzer erstellen" klicken und "Administratoren" auswählen (siehe Abbildung 4).

**Abbildung 4 – Hinzufügen eines Benutzers zu einer Rolle**

![clip_image008](authenticating-users-with-forms-authentication-cs/_static/image4.jpg)

Wenn alles gesagt und abgeschlossen ist, sollten Sie über zwei neue Benutzer mit den Namen Stephen und Sally verfügen. Sie sollten auch über eine neue Rolle mit dem Namen Administratoren verfügen. Sally ist ein Mitglied der Administrator Rolle, und Stephen ist nicht.

#### <a name="requiring-authorization"></a>Autorisierung erforderlich

Sie können festlegen, dass ein Benutzer authentifiziert werden muss, bevor der Benutzer eine Controller Aktion aufruft, indem er der Aktion das Attribut [autorisieren] hinzufügt. Sie können das Attribut [autorisieren] auf eine einzelne Controller Aktion anwenden, oder Sie können dieses Attribut auf eine gesamte Controller Klasse anwenden.

Der Controller in der Liste 1 macht beispielsweise eine Aktion mit dem Namen companysecrets () verfügbar. Da diese Aktion mit dem Attribut [autorisieren] ergänzt wird, kann diese Aktion nur aufgerufen werden, wenn ein Benutzer authentifiziert ist.

**Codebeispiel 1 – controllers\homecontroller.cs**

[!code-csharp[Main](authenticating-users-with-forms-authentication-cs/samples/sample1.cs)]

Wenn Sie die Aktion companysecrets () aufrufen, indem Sie die URL/Home/CompanySecrets in der Adressleiste Ihres Browsers eingeben und Sie kein authentifizierter Benutzer sind, werden Sie automatisch zur Anmelde Ansicht umgeleitet (siehe Abbildung 5).

**Abbildung 5 – die Anmeldungs Ansicht**

![clip_image010](authenticating-users-with-forms-authentication-cs/_static/image5.jpg)

Sie können die Anmelde Ansicht verwenden, um Ihren Benutzernamen und Ihr Kennwort einzugeben. Wenn Sie kein registrierter Benutzer sind, können Sie auf den Link **registrieren** klicken, um zur Register Ansicht zu navigieren (siehe Abbildung 6). Mithilfe der Register Ansicht können Sie ein neues Benutzerkonto erstellen.

**Abbildung 6 – die Register Ansicht**

![clip_image012](authenticating-users-with-forms-authentication-cs/_static/image6.jpg)

Nachdem Sie sich erfolgreich angemeldet haben, wird die Ansicht "Unternehmensgeheimnisse" angezeigt (siehe Abbildung 7). Standardmäßig werden Sie weiterhin angemeldet, bis Sie das Browserfenster schließen.

**Abbildung 7 – die Ansicht "Unternehmensgeheimnisse"**

![clip_image014](authenticating-users-with-forms-authentication-cs/_static/image7.jpg)

#### <a name="authorizing-by-user-name-or-user-role"></a>Autorisierungs nach Benutzer Name oder Benutzerrolle

Sie können das Attribut [autorisieren] verwenden, um den Zugriff auf eine Controller Aktion auf eine bestimmte Gruppe von Benutzern oder einen bestimmten Satz von Benutzer Rollen zu beschränken. Der geänderte Home-Controller in der Liste 2 enthält z. b. zwei neue Aktionen mit den Namen "stephensecrets ()" und "AdministratorSecrets ()".

**Codebeispiel 2 – controllers\homecontroller.cs**

[!code-csharp[Main](authenticating-users-with-forms-authentication-cs/samples/sample2.cs)]

Nur ein Benutzer mit dem Benutzernamen Stephen kann die Aktion stephensecrets () aufrufen. Alle anderen Benutzer werden zur Anmelde Ansicht umgeleitet. Die Users-Eigenschaft akzeptiert eine durch Trennzeichen getrennte Liste von Benutzerkonto Namen.

Nur Benutzer in der Administrator Rolle können die AdministratorSecrets ()-Aktion aufrufen. Da Sally z. b. ein Mitglied der Gruppe "Administratoren" ist, kann Sie die Aktion "AdministratorSecrets ()" aufrufen. Alle anderen Benutzer werden zur Anmelde Ansicht umgeleitet. Die Role-Eigenschaft akzeptiert eine durch Trennzeichen getrennte Liste von Rollennamen.

#### <a name="configuring-authentication"></a>Konfigurieren der Authentifizierung

An diesem Punkt Fragen Sie sich vielleicht, wo das Benutzerkonto und die Rollen Informationen gespeichert werden. Standardmäßig werden die Informationen in einer SQL Express-Datenbank (RANU) mit dem Namen "aspnetdb. mdf" gespeichert, die sich in der APP\_Daten Ordners ihrer MVC-Anwendung befindet. Diese Datenbank wird von ASP.NET Framework automatisch generiert, wenn Sie mit der Mitgliedschaft beginnen.

Um die aspnetdb. mdf-Datenbank im Projektmappen-Explorer Fenster anzuzeigen, müssen Sie zuerst die Menüoption Projekt, alle Dateien anzeigen auswählen.

Die Verwendung der SQL Express-Standarddatenbank ist in Ordnung, wenn Sie eine Anwendung entwickeln. Wahrscheinlich möchten Sie jedoch nicht die aspnetdb. mdf-Standarddatenbank für eine Produktionsanwendung verwenden. In diesem Fall können Sie den Speicherort der Benutzerkontoinformationen ändern, indem Sie die folgenden zwei Schritte ausführen:

1. Hinzufügen der Anwendungsdienste Datenbankobjekte zur Produktionsdatenbank: Ändern Sie die Anwendungs Verbindungs Zeichenfolge so, dass Sie auf Ihre Produktionsdatenbank verweist.

Der erste Schritt besteht darin, alle erforderlichen Datenbankobjekte (Tabellen und gespeicherte Prozeduren) der Produktionsdatenbank hinzuzufügen. Die einfachste Möglichkeit, diese Objekte zu einer neuen Datenbank hinzuzufügen, besteht darin, den ASP.NET SQL Server Setup-Assistenten zu nutzen (siehe Abbildung 8). Sie können dieses Tool starten, indem Sie die Visual Studio 2008-Eingabeaufforderung in der Gruppe Microsoft Visual Studio 2008 öffnen und den folgenden Befehl an der Eingabeaufforderung ausführen:

ASPNET\_RegSql

**Abbildung 8 – Setup-Assistent für ASP.NET-SQL Server**

![clip_image016](authenticating-users-with-forms-authentication-cs/_static/image8.jpg)

Der Setup-Assistent für ASP.NET SQL Server ermöglicht es Ihnen, eine SQL Server-Datenbank im Netzwerk auszuwählen und alle Datenbankobjekte zu installieren, die für die ASP.NET-Anwendungsdienste erforderlich sind. Der Datenbankserver muss sich nicht auf dem lokalen Computer befinden.

> [!NOTE] 
> 
> Wenn Sie den Setup-Assistenten für ASP.NET SQL Server nicht verwenden möchten, können Sie SQL-Skripts zum Hinzufügen der Anwendungsdienste-Datenbankobjekte im folgenden Ordner finden:
> 
> > C:\Windows\Microsoft.NET\Framework\v2.0.50727

Nachdem Sie die erforderlichen Datenbankobjekte erstellt haben, müssen Sie die Datenbankverbindung ändern, die von der MVC-Anwendung verwendet wird. Ändern Sie die ApplicationServices-Verbindungs Zeichenfolge in der Webkonfigurationsdatei (Web. config), sodass Sie auf die Produktionsdatenbank verweist. Beispielsweise verweist die geänderte Verbindung in Auflistung 3 auf eine Datenbank mit dem Namen myproductiondb (die ursprüngliche ApplicationServices-Verbindungs Zeichenfolge wurde auskommentiert).

**Codebeispiel 3 – Web. config**

[!code-xml[Main](authenticating-users-with-forms-authentication-cs/samples/sample3.xml)]

#### <a name="configuring-database-permissions"></a>Konfigurieren von Daten Bank Berechtigungen

Wenn Sie die integrierte Sicherheit verwenden, um eine Verbindung mit Ihrer Datenbank herzustellen, müssen Sie das korrekte Windows-Benutzerkonto als Anmelde Namen zur Datenbank hinzufügen. Welches Konto richtig ist, hängt davon ab, ob Sie die ASP.NET Development Server oder Internetinformationsdienste als Webserver verwenden. Das richtige Benutzerkonto ist auch abhängig von Ihrem Betriebssystem.

Wenn Sie die ASP.NET Development Server (der von Visual Studio verwendete Standard Webserver) verwenden, wird Ihre Anwendung im Kontext des Windows-Benutzerkontos ausgeführt. In diesem Fall müssen Sie das Windows-Benutzerkonto als Anmelde Namen für den Datenbankserver hinzufügen.

Wenn Sie Internetinformationsdienste verwenden, müssen Sie alternativ entweder das ASPNET-Konto oder das NT-Autorität-/Netzwerkdienstkonto als Anmelde Namen für den Datenbankserver hinzufügen. Wenn Sie Windows XP verwenden, fügen Sie das ASPNET-Konto als Anmelde Namen zur Datenbank hinzu. Wenn Sie ein aktuelleres Betriebssystem verwenden, wie z. b. Windows Vista oder Windows Server 2008, fügen Sie das NT-Autorität-/Netzwerkdienstkonto als Daten Bank Anmeldung hinzu.

Sie können der Datenbank ein neues Benutzerkonto hinzufügen, indem Sie Microsoft SQL Server Management Studio verwenden (siehe Abbildung 9).

**Abbildung 9 – Erstellen einer neuen Microsoft SQL Server Anmeldung**

![clip_image018](authenticating-users-with-forms-authentication-cs/_static/image9.jpg)

Nachdem Sie die erforderliche Anmeldung erstellt haben, müssen Sie die Anmeldung einem Datenbankbenutzer mit den richtigen Daten bankrollen zuordnen. Doppelklicken Sie auf die Anmeldung, und wählen Sie die Registerkarte Benutzer Zuordnung aus. Wählen Sie mindestens eine Anwendungs Dienst-Daten Bank Rolle aus. Um z. b. Benutzer zu authentifizieren, müssen Sie die ASPNET-\_Mitgliedschaft\_der Daten Bank Rolle "BasicAccess" aktivieren. Um neue Benutzer zu erstellen, müssen Sie die ASPNET-\_Mitgliedschaft\_Daten Bank Rolle "FullAccess" aktivieren (siehe Abbildung 10).

**Abbildung 10 – Hinzufügen von Anwendungsdienste Daten bankrollen**

![clip_image020](authenticating-users-with-forms-authentication-cs/_static/image10.jpg)

#### <a name="summary"></a>Zusammenfassung

In diesem Tutorial haben Sie erfahren, wie Sie die Formular Authentifizierung verwenden, wenn Sie eine ASP.NET MVC-Anwendung entwickeln. Zuerst haben Sie erfahren, wie Sie neue Benutzer und Rollen erstellen, indem Sie das Websiteverwaltungs-Tool nutzen. Als nächstes haben Sie erfahren, wie Sie das Attribut [autorisieren] verwenden, um zu verhindern, dass nicht autorisierte Benutzer Controller Aktionen aufrufen. Schließlich haben Sie erfahren, wie Sie Ihre MVC-Anwendung so konfigurieren, dass Benutzer-und Rollen Informationen in einer Produktionsdatenbank gespeichert werden.

> [!div class="step-by-step"]
> [Weiter](authenticating-users-with-windows-authentication-cs.md)
