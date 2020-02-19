---
uid: identity/overview/migrations/migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity
title: Migrieren von universellen Anbieter Daten für Mitgliedschafts-und BenutzerC#Profile zu ASP.net Identity ()-ASP.NET 4. x
author: rustd
description: In diesem Tutorial werden die Schritte beschrieben, die zum Migrieren von Benutzer-und Rollen Daten und Benutzerprofil Daten erforderlich sind, die mit universelle Anbieter einer vorhandenen App erstellt wurden...
ms.author: riande
ms.date: 12/13/2013
ms.custom: seoapril2019
ms.assetid: 2e260430-d13c-4658-bd05-e256fc0d63b8
msc.legacyurl: /identity/overview/migrations/migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: 31f02a0cec3c531c45c37b7aad8456e01e80b5ea
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/19/2020
ms.locfileid: "77456113"
---
# <a name="migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity-c"></a>Migrieren von Daten eines universellen Anbieters in Bezug auf Mitgliedschaften und Benutzerprofilen zu ASP.NET Identity (C#)

von [Pranav Rastogi](https://github.com/rustd), [Rick Anderson](https://twitter.com/RickAndMSFT), [Robert McMurray](https://github.com/rmcmurray), [Suhas Joshi](https://github.com/suhasj)

> In diesem Tutorial werden die Schritte beschrieben, die zum Migrieren von Benutzer-und Rollen Daten und Benutzerprofil Daten, die mit universelle Anbieter einer vorhandenen Anwendung erstellt wurden, in das ASP.net Identity Modell erforderlich sind. Der hier erwähnte Ansatz zum Migrieren von Benutzerprofil Daten kann auch in einer Anwendung mit SQL-Mitgliedschaft verwendet werden.

Mit der Veröffentlichung von Visual Studio 2013 hat das ASP.net-Team ein neues ASP.net Identity System eingeführt. Weitere Informationen zu dieser Version finden Sie [hier](../../index.md). In diesem Artikel werden die Schritte zum Migrieren von Webanwendungen aus der [SQL-Mitgliedschaft zum neuen Identitätssystem](migrating-an-existing-website-from-sql-membership-to-aspnet-identity.md)erläutert. in diesem Artikel werden die Schritte zum Migrieren vorhandener Anwendungen erläutert, die dem Anbieter Modell für die Benutzer-und Rollen Verwaltung im neuen Identitäts Modell folgen. Der Schwerpunkt dieses Tutorials liegt hauptsächlich auf der Migration der Benutzerprofil Daten, um Sie nahtlos mit dem neuen System zu verbinden. Die Migration von Benutzer-und Rollen Informationen ähnelt der SQL-Mitgliedschaft. Der Ansatz, der zum Migrieren von Profildaten folgt, kann auch in einer Anwendung mit SQL-Mitgliedschaft verwendet werden.

Als Beispiel beginnen wir mit einer Web-App, die mit Visual Studio 2012 erstellt wird und das Provider-Modell verwendet. Fügen Sie dann Code für die Profilverwaltung hinzu, registrieren Sie einen Benutzer, fügen Sie Profildaten für die Benutzer hinzu, migrieren Sie das Datenbankschema, und ändern Sie dann die Anwendung, sodass Sie das Identitätssystem für die Benutzer-und Rollen Verwaltung verwendet. Als Migrationstest sollten Benutzer, die mithilfe von universelle Anbieter erstellt wurden, sich anmelden können, und neue Benutzer sollten sich registrieren können.

> [!NOTE]
> Das komplette Beispiel finden Sie unter [https://github.com/suhasj/UniversalProviders-Identity-Migrations](https://github.com/suhasj/UniversalProviders-Identity-Migrations).

## <a name="profile-data-migration-summary"></a>Zusammenfassung der Profildaten Migration

Bevor wir mit den Migrationen beginnen, sehen wir uns die Funktionen zum Speichern von Profildaten im Anbieter Modell an. Profildaten für Anwendungs Benutzer können auf verschiedene Weise gespeichert werden, wobei es sich bei den am häufigsten verwendeten Profil Anbietern um die zusammen mit dem universelle Anbieter handelt. Die Schritte umfassen

1. Fügen Sie eine Klasse hinzu, die über Eigenschaften zum Speichern von Profildaten verfügt.
2. Fügen Sie eine Klasse hinzu, die "ProfileBase" erweitert, und implementiert Methoden, um die obigen Profildaten für den Benutzer zu erhalten.
3. Aktivieren Sie die Verwendung von Standardprofil Anbietern in der Datei " *Web. config* ", und definieren Sie die in Schritt #2 deklarierte Klasse für den Zugriff auf Profilinformationen.

Die Profilinformationen werden als serialisierte XML-und Binärdaten in der Tabelle "Profiles" in der Datenbank gespeichert.

Nachdem die Anwendung zur Verwendung des neuen ASP.net Identity Systems migriert wurde, werden die Profilinformationen deserialisiert und als Eigenschaften in der User-Klasse gespeichert. Jede Eigenschaft kann dann Spalten in der Benutzertabelle zugeordnet werden. Der Vorteil hierbei ist, dass die Eigenschaften direkt mithilfe der User-Klasse bearbeitet werden können, wenn Sie nicht jedes Mal, wenn Sie darauf zugreifen, Daten Informationen serialisieren/deserialisieren müssen.

## <a name="getting-started"></a>Erste Schritte

1. Erstellen Sie eine neue 4,5 ASP.net-Web Forms-Anwendung in Visual Studio 2012. Im aktuellen Beispiel wird die Web Forms-Vorlage verwendet, aber Sie können auch die MVC-Anwendung verwenden.  
    ![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image1.jpg)
2. Erstellen Sie einen neuen Ordner "Models", um Profilinformationen zu speichern.  
    ![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image1.png)
3. Als Beispiel können wir das Geburtsdatum, den Ort, die Höhe und die Gewichtung des Benutzers im Profil speichern. Die Höhe und Gewichtung werden als benutzerdefinierte Klasse mit dem Namen "Personal Stats" gespeichert. Zum Speichern und Abrufen des Profils benötigen wir eine Klasse, die "ProfileBase" erweitert. Wir erstellen nun eine neue Klasse "appprofile", um Profilinformationen zu erhalten und zu speichern.

    [!code-csharp[Main](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/samples/sample1.cs)]
4. Aktivieren Sie das Profil in der Datei " *Web. config* ". Geben Sie den Klassennamen ein, der zum Speichern/Abrufen von Benutzerinformationen verwendet wird, die in Schritt #3 erstellt wurden.

    [!code-xml[Main](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/samples/sample2.xml)]
5. Fügen Sie im Ordner "Account" eine Web Forms-Seite hinzu, um die Profildaten vom Benutzer zu erhalten und zu speichern. Klicken Sie mit der rechten Maustaste auf Projekt, und wählen Sie "Neues Element hinzufügen" Fügen Sie eine neue WebForms-Seite mit der Master Seite "addprofiledata. aspx" hinzu. Kopieren Sie Folgendes in den Abschnitt "mainContent":

    [!code-html[Main](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/samples/sample3.html)]

   Fügen Sie im Code Behind den folgenden Code hinzu:

    [!code-csharp[Main](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/samples/sample4.cs)]

   Fügen Sie den Namespace hinzu, unter dem appprofile-Klasse definiert ist, um die Kompilierungsfehler zu entfernen.
6. Führen Sie die APP aus, und erstellen Sie einen neuen Benutzer mit dem Benutzernamen "**olduser".** Navigieren Sie zur Seite "addprofiledata", und fügen Sie Profilinformationen für den Benutzer hinzu.  
    ![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image2.png)

Mithilfe des Server-Explorer Fensters können Sie überprüfen, ob die Daten als serialisiertes XML in der Tabelle "Profile" gespeichert werden. Wählen Sie in Visual Studio im Menü "Ansicht" die Option "Server-Explorer" aus. Es sollte eine Datenverbindung für die in der Datei " *Web. config* " definierte Datenbank vorhanden sein. Wenn Sie auf die Datenverbindung klicken, werden unterschiedliche Unterkategorien angezeigt. Erweitern Sie "Tables", um die verschiedenen Tabellen in der Datenbank anzuzeigen. Klicken Sie dann mit der rechten Maustaste auf "Profiles", und wählen Sie "Tabellendaten anzeigen", um die in der Tabelle "Profile" gespeicherten Profildaten anzuzeigen.

![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image3.png)

![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image4.png)

## <a name="migrating-database-schema"></a>Migrationsdaten Bank Schema

Damit die vorhandene Datenbank mit dem Identitätssystem zusammenarbeiten kann, müssen wir das Schema in der Identitätsdatenbank aktualisieren, um die Felder zu unterstützen, die wir der ursprünglichen Datenbank hinzugefügt haben. Hierzu können Sie SQL-Skripts verwenden, um neue Tabellen zu erstellen und die vorhandenen Informationen zu kopieren. Erweitern Sie im Fenster "Server-Explorer" den Eintrag "DefaultConnection", um die Tabellen anzuzeigen. Klicken Sie mit der rechten Maustaste auf Tabellen, und wählen Sie neue Abfrage

![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image5.png)

Fügen Sie das SQL-Skript aus [https://raw.github.com/suhasj/UniversalProviders-Identity-Migrations/master/Migration.txt](https://raw.github.com/suhasj/UniversalProviders-Identity-Migrations/master/Migration.txt) ein und führen Sie es aus. Wenn "DefaultConnection" aktualisiert wird, können Sie sehen, dass die neuen Tabellen hinzugefügt werden. Sie können die Daten in den Tabellen überprüfen, um festzustellen, ob die Informationen migriert wurden.

![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image6.png)

## <a name="migrating-the-application-to-use-aspnet-identity"></a>Migrieren der Anwendung zur Verwendung ASP.net Identity

1. Installieren Sie die nuget-Pakete, die für ASP.net Identity benötigt werden:

    - Microsoft.AspNet.Identity.EntityFramework
    - Microsoft.AspNet.Identity.Owin
    - Microsoft.Owin.Host.SystemWeb
    - Microsoft.Owin.Security.Facebook
    - Microsoft.Owin.Security.Google
    - Microsoft.Owin.Security.MicrosoftAccount
    - Microsoft.Owin.Security.Twitter

   Weitere Informationen zum Verwalten von nuget-Paketen finden Sie [hier](http://docs.nuget.org/docs/start-here/Managing-NuGet-Packages-Using-The-Dialog) .
2. Um mit vorhandenen Daten in der Tabelle arbeiten zu können, müssen Modellklassen erstellt werden, die den Tabellen wieder zugeordnet und im Identitätssystem eingebunden werden. Als Teil des Identitäts Vertrags sollten die Modellklassen entweder die in der Identity. Core-DLL definierten Schnittstellen implementieren oder die vorhandene Implementierung dieser Schnittstellen erweitern, die in Microsoft. Aspnet. Identity. EntityFramework verfügbar sind. Wir verwenden die vorhandenen Klassen für Rollen, Benutzeranmeldungen und Benutzer Ansprüche. Wir müssen für unser Beispiel einen benutzerdefinierten Benutzer verwenden. Klicken Sie mit der rechten Maustaste auf das Projekt, und erstellen Sie den neuen Ordner identitymodels. Fügen Sie eine neue Benutzerklasse hinzu, wie unten gezeigt:

    [!code-csharp[Main](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/samples/sample5.cs)]

   Beachten Sie, dass "profilerfo" jetzt eine Eigenschaft für die Benutzerklasse ist. Daher können wir die User-Klasse verwenden, um direkt mit Profildaten zu arbeiten.

Kopieren Sie die Dateien in den Ordnern " **identitymodels** " und " **identityaccount** " aus der Download Quelle ( [https://github.com/suhasj/UniversalProviders-Identity-Migrations/tree/master/UniversalProviders-Identity-Migrations](https://github.com/suhasj/UniversalProviders-Identity-Migrations/tree/master/UniversalProviders-Identity-Migrations) ). Diese verfügen über die verbleibenden Modellklassen und die neuen Seiten, die für die Benutzer-und Rollen Verwaltung mithilfe der ASP.net Identity-APIs benötigt werden. Der verwendete Ansatz ähnelt der SQL-Mitgliedschaft, und die ausführliche Erläuterung finden Sie [hier](migrating-an-existing-website-from-sql-membership-to-aspnet-identity.md).

[!INCLUDE[](../../../includes/identity/alter-command-exception.md)]

## <a name="copying-profile-data-to-the-new-tables"></a>Kopieren von Profildaten in die neuen Tabellen

Wie bereits erwähnt, müssen die XML-Daten in den Profil Tabellen deserialisiert und in den Spalten der Tabelle "aspnettusers" gespeichert werden. Die neuen Spalten wurden im vorherigen Schritt in der Tabelle Benutzer erstellt, sodass Sie nur noch die Spalten mit den erforderlichen Daten auffüllen müssen. Zu diesem Zweck verwenden wir eine Konsolenanwendung, die einmal ausgeführt wird, um die neu erstellten Spalten in der Benutzertabelle aufzufüllen.

1. Erstellen Sie in der Projekt Mappe eine neue Konsolenanwendung.  
    ![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image2.jpg)
2. Installieren Sie die neueste Version des Entity Framework Pakets.
3. Fügen Sie die oben erstellte Webanwendung als Verweis auf die Konsolenanwendung hinzu. Klicken Sie dazu mit der rechten Maustaste auf "Projekt" und dann auf "Verweise hinzufügen". Klicken Sie dann auf das Projekt, und klicken Sie auf OK.
4. Kopieren Sie den folgenden Code in die Program.cs-Klasse. Diese Logik liest Profildaten für jeden Benutzer, serialisiert sie als ProfileInfo-Objekt und speichert Sie wieder in der Datenbank.

    [!code-csharp[Main](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/samples/sample6.cs)]

   Einige der verwendeten Modelle werden im Ordner "identitymodels" des Webanwendungs Projekts definiert, sodass Sie die entsprechenden Namespaces einschließen müssen.
5. Der obige Code funktioniert in der Datenbankdatei im Ordner App\_Data des Webanwendungs Projekts, das in den vorherigen Schritten erstellt wurde. Um darauf zu verweisen, aktualisieren Sie die Verbindungs Zeichenfolge in der Datei app. config der Konsolenanwendung mit der Verbindungs Zeichenfolge in der Datei Web. config der Webanwendung. Geben Sie außerdem den gesamten physischen Pfad in der Eigenschaft "AttachDBFilename" an.
6. Öffnen Sie eine Eingabeaufforderung, und navigieren Sie zum Ordner bin der obigen Konsolenanwendung. Führen Sie die ausführbare Datei aus, und überprüfen Sie die Protokoll Ausgabe wie in der folgenden Abbildung dargestellt.  
    ![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image3.jpg)
7. Öffnen Sie die Tabelle "aspnettusers" im Server-Explorer, und überprüfen Sie die Daten in den neuen Spalten, die die Eigenschaften enthalten. Sie sollten mit den entsprechenden Eigenschafts Werten aktualisiert werden.

## <a name="verify-functionality"></a>Überprüfen der Funktionalität

Verwenden Sie die neu hinzugefügten Mitgliedschafts Seiten, die mit ASP.net Identity implementiert werden, um einen Benutzer aus der alten Datenbank anzumelden. Der Benutzer sollte sich mit denselben Anmelde Informationen anmelden können. Testen Sie die anderen Funktionen wie das Hinzufügen von OAuth, das Erstellen eines neuen Benutzers, das Ändern eines Kennworts, das Hinzufügen von Rollen und das Hinzufügen von Benutzern zu Rollen

Die Profildaten für den alten Benutzer und die neuen Benutzer müssen abgerufen und in der Tabelle "Benutzer" gespeichert werden. Auf die alte Tabelle sollte nicht mehr verwiesen werden.

## <a name="conclusion"></a>Zusammenfassung

Der Artikel beschreibt den Prozess der Migration von Webanwendungen, die das Anbieter Modell für die Mitgliedschaft in ASP.net Identity verwendet haben. Der Artikel erläutert außerdem das Migrieren von Profildaten für Benutzer, die in das Identitätssystem eingebunden werden sollen. Wenn Sie Ihre APP migrieren, sollten Sie die folgenden Kommentare für Fragen und Probleme hinterlassen.

*Vielen Dank, dass Rick Anderson und Robert McMurray den Artikel überprüft haben.*
