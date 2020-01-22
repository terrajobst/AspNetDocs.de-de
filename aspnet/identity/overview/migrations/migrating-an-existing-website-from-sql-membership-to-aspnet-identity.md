---
uid: identity/overview/migrations/migrating-an-existing-website-from-sql-membership-to-aspnet-identity
title: Migrieren einer vorhandenen Website von der SQL-Mitgliedschaft zu ASP.net Identity-ASP.NET 4. x
author: Rick-Anderson
description: In diesem Tutorial werden die Schritte zum Migrieren einer vorhandenen Webanwendung mit Benutzer-und Rollen Daten veranschaulicht, die mithilfe der SQL-Mitgliedschaft zum neuen ASP.net Identity s erstellt wurden...
ms.author: riande
ms.date: 12/19/2014
ms.custom: seoapril2019
ms.assetid: 220d3d75-16b2-4240-beae-a5b534f06419
msc.legacyurl: /identity/overview/migrations/migrating-an-existing-website-from-sql-membership-to-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: eacfbb8a5b2d1aa3678892bc2077a56185fdebbc
ms.sourcegitcommit: 88fc80e3f65aebdf61ec9414810ddbc31c543f04
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/22/2020
ms.locfileid: "76519153"
---
# <a name="migrating-an-existing-website-from-sql-membership-to-aspnet-identity"></a>Migrieren einer vorhandenen Website von einem SQL-Mitgliedschaftsanbieter zu ASP.NET Identity

von [Rick Anderson]((https://twitter.com/RickAndMSFT)), [Suhas Joshi](https://github.com/suhasj)

> In diesem Tutorial werden die Schritte zum Migrieren einer vorhandenen Webanwendung mit Benutzer-und Rollen Daten erläutert, die mit der SQL-Mitgliedschaft für das neue ASP.net Identity System erstellt wurden Diese Vorgehensweise umfasst das Ändern des vorhandenen Datenbankschemas in das, das von der ASP.net Identity benötigt wird, und das Hook in den alten bzw. neuen Klassen. Nachdem Sie diesen Ansatz übernommen haben, werden zukünftige Updates der Identität nach der Migration der Datenbank mühelos behandelt.

In diesem Tutorial erstellen wir eine webanwendungvorlage (Web Forms), die mithilfe von Visual Studio 2010 erstellt wird, um Benutzer-und Rollen Daten zu erstellen. Anschließend werden SQL-Skripts verwendet, um die vorhandene Datenbank zu den vom Identitätssystem benötigten Tabellen zu migrieren. Als nächstes installieren Sie die erforderlichen nuget-Pakete und fügen neue Kontoverwaltungsseiten hinzu, die das Identitätssystem für die Mitgliedschafts Verwaltung verwenden. Als Migrationstest sollten Benutzer, die mithilfe der SQL-Mitgliedschaft erstellt wurden, sich anmelden können, und neue Benutzer sollten sich registrieren können. Das vollständige Beispiel finden Sie [hier](https://github.com/aspnet/samples/tree/master/samples/aspnet/Identity/SQLMembership-Identity-OWIN/). Siehe auch [Migrieren von ASP.NET Mitgliedschaft zu ASP.net Identity](http://travis.io/blog/2015/03/24/migrate-from-aspnet-membership-to-aspnet-identity.html).

## <a name="getting-started"></a>Erste Schritte

### <a name="creating-an-application-with-sql-membership"></a>Erstellen einer Anwendung mit SQL-Mitgliedschaft

1. Wir müssen mit einer vorhandenen Anwendung beginnen, die die SQL-Mitgliedschaft verwendet und über Benutzer-und Rollen Daten verfügt. Im Rahmen dieses Artikels erstellen wir eine Webanwendung in Visual Studio 2010.

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image1.jpg)
2. Erstellen Sie mit dem ASP.NET-Konfigurationstool 2 Benutzer: **oldadminuser** und **olduser.**

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image1.png)

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image2.jpg)
3. Erstellen Sie eine Rolle mit dem Namen admin, und fügen Sie in dieser Rolle "oldadminuser" als Benutzer hinzu.

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image2.png)
4. Erstellen Sie einen Administrator Abschnitt der Site mit der Standardeinstellung. aspx. Legen Sie das Autorisierungs-Tag in der Datei "Web. config" fest, um den Zugriff nur Benutzern in Administrator Rollen zu ermöglichen. Weitere Informationen finden Sie hier [https://www.asp.net/web-forms/tutorials/security/roles/role-based-authorization-cs](../../../web-forms/overview/older-versions-security/roles/role-based-authorization-cs.md)

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image3.png)
5. Zeigen Sie die Datenbank in Server-Explorer an, um die vom SQL-Mitgliedschaftssystem erstellten Tabellen zu verstehen. Die Benutzer Anmeldedaten werden in den ASPNET-\_Benutzer-und ASPNET-\_Mitgliedschafts Tabellen gespeichert, während Rollen Daten in der Tabelle "ASPNET\_Rollen" gespeichert werden. Informationen dazu, in welchen Benutzern Rollen in der Tabelle ASPNET\_usersinrollen gespeichert werden. Bei der grundlegenden Verwaltung ist es ausreichend, die Informationen in den obigen Tabellen auf das ASP.net Identity System zu portieren.

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image4.png)

### <a name="migrating-to-visual-studio-2013"></a>Migrieren zu Visual Studio 2013

1. Installieren Sie Visual Studio Express 2013 für Web oder Visual Studio 2013 zusammen mit den [neuesten Updates](https://www.microsoft.com/download/details.aspx?id=44921).
2. Öffnen Sie das obige Projekt in der installierten Version von Visual Studio. Wenn SQL Server Express nicht auf dem Computer installiert ist, wird eine Eingabeaufforderung angezeigt, wenn Sie das Projekt öffnen, da die Verbindungs Zeichenfolge SQL Express verwendet. Sie können entweder SQL Express installieren oder die Verbindungs Zeichenfolge in localdb ändern. In diesem Artikel wird der Wert in localdb geändert.
3. Öffnen Sie Web. config, und ändern Sie die Verbindungs Zeichenfolge von. SQLExpress zu (localdb) v 11.0. Entfernen Sie "User Instance = true" aus der Verbindungs Zeichenfolge.

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image3.jpg)
4. Öffnen Sie Server-Explorer, und überprüfen Sie, ob das Tabellen Schema und die Daten beobachtet werden können.
5. Das ASP.net Identity System kann mit Version 4,5 oder höher des Frameworks verwendet werden. Richten Sie die Anwendung erneut auf 4,5 oder höher aus.

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image5.png)

    Erstellen Sie das Projekt, um sicherzustellen, dass keine Fehler vorliegen.

### <a name="installing-the-nuget-packages"></a>Installieren von nuget-Paketen

1. Klicken Sie in Projektmappen-Explorer mit der rechten Maustaste auf das Projekt &gt; **nuget-Pakete verwalten**. Geben Sie im Suchfeld "ASP.net Identity" ein. Wählen Sie das Paket in der Liste der Ergebnisse aus, und klicken Sie auf installieren. Akzeptieren Sie den Lizenzvertrag, indem Sie auf die Schaltfläche "Ich stimme zu" klicken. Beachten Sie, dass mit diesem Paket die Abhängigkeits Pakete installiert werden: EntityFramework und Microsoft ASP.net Identity Core. Installieren Sie auf ähnliche Weise die folgenden Pakete (überspringen Sie die letzten vier owin-Pakete, wenn Sie die OAuth-Anmeldung nicht aktivieren möchten):

   - Microsoft.AspNet.Identity.Owin
   - Microsoft.Owin.Host.SystemWeb
   - Microsoft.Owin.Security.Facebook
   - Microsoft.Owin.Security.Google
   - Microsoft.Owin.Security.MicrosoftAccount
   - Microsoft.Owin.Security.Twitter

     ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image6.png)

### <a name="migrate-database-to-the-new-identity-system"></a>Migrieren der Datenbank zum neuen Identitätssystem

Der nächste Schritt besteht darin, die vorhandene Datenbank zu einem Schema zu migrieren, das für das ASP.net Identity System erforderlich ist. Um dies zu erreichen, führen wir ein SQL-Skript aus, das über eine Reihe von Befehlen zum Erstellen neuer Tabellen und zum Migrieren vorhandener Benutzerinformationen zu den neuen Tabellen verfügt. Die Skriptdatei finden Sie [hier](https://github.com/aspnet/samples/blob/master/samples/aspnet/Identity/SQLMembership-Identity-OWIN/Migrations.sql).

Diese Skriptdatei ist spezifisch für dieses Beispiel. Wenn das Schema für die mit der SQL-Mitgliedschaft erstellten Tabellen angepasst oder geändert wird, müssen die Skripts entsprechend geändert werden.

### <a name="how-to-generate-the-sql-script-for-schema-migration"></a>Generieren des SQL-Skripts für die Schema Migration

Damit ASP.net Identity Klassen mit den Daten vorhandener Benutzer standardmäßig arbeiten können, müssen wir das Datenbankschema zu dem ASP.net Identity migrieren, das von benötigt wird. Hierzu können Sie neue Tabellen hinzufügen und die vorhandenen Informationen in diese Tabellen kopieren. Standardmäßig verwendet ASP.net Identity EntityFramework, um die Identitäts Modellklassen wieder der Datenbank zuzuordnen, um Informationen zu speichern/abzurufen. Diese Modellklassen implementieren die wichtigsten Identitäts Schnittstellen, die Benutzer-und Rollen Objekte definieren. Die Tabellen und die Spalten in der Datenbank basieren auf diesen Modellklassen. Die EntityFramework-Modellklassen in Identity v 2.1.0 und deren Eigenschaften sind wie unten definiert definiert.

| **IdentityUser** | **Type** | **IdentityRole** | **IdentityUserRole** | **IdentityUserLogin** | **IdentityUserClaim** |
| --- | --- | --- | --- | --- | --- |
| ID | string | ID | RoleId | Providerkey | ID |
| Benutzername | string | -Name | UserId | UserId | ClaimType |
| PasswordHash | string |  |  | LoginProvider | ClaimValue |
| SecurityStamp | string |  |  |  | Benutzer\_-ID |
| E-Mail | string |  |  |  |  |
| Emailbestätigt | bool |  |  |  |  |
| Telefonnummer | string |  |  |  |  |
| PhoneNumberConfirmed | bool |  |  |  |  |
| LockoutEnabled | bool |  |  |  |  |
| LockoutEndDate | DateTime |  |  |  |  |
| AccessFailedCount | int |  |  |  |  |

Wir müssen für jedes dieser Modelle Tabellen mit Spalten aufweisen, die den Eigenschaften entsprechen. Die Zuordnung zwischen Klassen und Tabellen wird in der `OnModelCreating`-Methode der `IdentityDBContext`definiert. Dies wird als fließende API-Methode der Konfiguration bezeichnet. Weitere Informationen finden Sie [hier](https://msdn.microsoft.com/data/jj591617.aspx). Die Konfiguration für die Klassen ist wie unten beschrieben.

| **Klasse** | **Table** | **Primärschlüssel** | **Fremdschlüssel** |
| --- | --- | --- | --- |
| IdentityUser | AspnetUsers | ID |  |
| IdentityRole | AspnetRoles | ID |  |
| IdentityUserRole | AspnetUserRole | UserID + RoleID | Benutzer\_-ID-&gt;aspnettusers RoleID-&gt;aspnettroles |
| IdentityUserLogin | AspnetUserLogins | Providerkey + UserID und loginprovider | UserId-&gt;AspnetUsers |
| IdentityUserClaim | AspnetUserClaims | ID | Benutzer\_ID-&gt;aspnettusers |

Mit diesen Informationen können wir SQL-Anweisungen erstellen, um neue Tabellen zu erstellen. Wir können entweder jede Anweisung einzeln schreiben oder das gesamte Skript mithilfe von EntityFramework PowerShell-Befehlen generieren, die wir dann nach Bedarf bearbeiten können. Öffnen Sie hierzu in Visual Studio die **Paket-Manager-Konsole** über das Menü " **Ansicht** " oder " **Tools** ".

- Führen Sie den Befehl "Enable-Migrationen" zum Aktivieren von EntityFramework-Migrationen aus
- Führen Sie den Befehl "Add-Migration Initial" aus, mit dem der anfängliche Setup Code zum C#Erstellen der Datenbank in/VB. erstellt wird.
- Der letzte Schritt besteht darin, den Befehl "Update-Database – Script" auszuführen, der das SQL-Skript basierend auf den Modellklassen generiert.

[!INCLUDE[](../../../includes/identity/alter-command-exception.md)]

Dieses Daten Bank Generierungs Skript kann als Start verwendet werden, bei dem wir zusätzliche Änderungen vornehmen, um neue Spalten hinzuzufügen und Daten zu kopieren. Der Vorteil besteht darin, dass die `_MigrationHistory` Tabelle generiert wird, die von EntityFramework verwendet wird, um das Datenbankschema zu ändern, wenn sich die Modellklassen für zukünftige Versionen der Identitäts Releases ändern.

Die Informationen zu den SQL-Mitgliedschafts Benutzern waren zusätzlich zu den in der Identitäts Benutzer Modell-Klasse genannten Eigenschaften, z. b. e-Mail, Kenn Wort Versuche, letztes Anmeldungs Datum, letztes Sperr Datum usw. Dies sind nützliche Informationen, die wir auf das Identitätssystem übertragen möchten. Dies können Sie erreichen, indem Sie dem Benutzer Modell weitere Eigenschaften hinzufügen und diese den Tabellen Spalten in der Datenbank wieder zuordnen. Hierfür können Sie eine Klasse hinzufügen, die das `IdentityUser` Modell Unterklassen unterteilt. Wir können dieser benutzerdefinierten Klasse die Eigenschaften hinzufügen und das SQL-Skript bearbeiten, um beim Erstellen der Tabelle die entsprechenden Spalten hinzuzufügen. Der Code für diese Klasse wird weiter unten in diesem Artikel beschrieben. Das SQL-Skript zum Erstellen der `AspnetUsers` Tabelle nach dem Hinzufügen der neuen Eigenschaften wäre

[!code-sql[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample1.sql)]

Als nächstes müssen die vorhandenen Informationen aus der SQL-Mitgliedschafts Datenbank in die neu hinzugefügten Tabellen für die Identität kopiert werden. Dies kann mithilfe von SQL erreicht werden, indem Daten direkt aus einer Tabelle in eine andere kopiert werden. Zum Hinzufügen von Daten zu den Zeilen der Tabelle verwenden wir das `INSERT INTO [Table]`-Konstrukt. Zum Kopieren aus einer anderen Tabelle können wir die `INSERT INTO`-Anweisung zusammen mit der `SELECT`-Anweisung verwenden. Um alle Benutzerinformationen zu erhalten, müssen wir die *ASPNET-\_Benutzer* und *ASPNET\_Mitgliedschafts* Tabellen Abfragen und die Daten in die *aspnettusers* -Tabelle kopieren. Die `INSERT INTO` und `SELECT` werden zusammen mit `JOIN`-und `LEFT OUTER JOIN`-Anweisungen verwendet. Weitere Informationen zum Abfragen und Kopieren von Daten zwischen Tabellen finden Sie unter [diesem](https://technet.microsoft.com/library/ms190750%28v=sql.105%29.aspx) Link. Außerdem sind die aspnettuserlogins-Tabelle und die aspnettuserclaims-Tabelle leer, damit Sie mit beginnen, da in der SQL-Mitgliedschaft keine Informationen vorhanden sind, die dieser standardmäßig zugeordnet sind. Die einzigen kopierten Informationen sind für Benutzer und Rollen. Für das Projekt, das in den vorherigen Schritten erstellt wurde, würde die SQL-Abfrage zum Kopieren von Informationen in die Tabelle "Benutzer" lauten.

[!code-sql[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample2.sql)]

In der obigen SQL-Anweisung werden Informationen zu jedem Benutzer aus der *ASPNET-\_Benutzer* und *ASPNET\_Mitgliedschafts* Tabellen in die Spalten der *aspnettusers* -Tabelle kopiert. Die einzige Änderung, die hier vorgenommen wird, ist das Kopieren des Kennworts. Da der Verschlüsselungsalgorithmus für Kenn Wörter in der SQL-Mitgliedschaft ' PasswordSalt ' und ' PasswordFormat ' verwendet hat, kopieren wir das ebenfalls zusammen mit dem Hashwert-Kennwort, damit es zum Entschlüsseln des Kennworts nach Identität verwendet werden kann. Dies wird weiter unten in diesem Artikel erläutert, wenn Sie ein benutzerdefiniertes Kennwort für das Kennwort einbinden.

Diese Skriptdatei ist spezifisch für dieses Beispiel. Bei Anwendungen mit zusätzlichen Tabellen können Entwickler einem ähnlichen Ansatz folgen, um zusätzliche Eigenschaften für die Benutzer Modell Klasse hinzuzufügen und diese Spalten in der Tabelle "aspnettusers" zuzuordnen. So führen Sie das Skript aus

1. Öffnen Sie den Server-Explorer. Erweitern Sie die ApplicationServices-Verbindung, um die Tabellen anzuzeigen. Klicken Sie mit der rechten Maustaste auf den Knoten Tabellen, und wählen Sie die Option Neue Abfrage aus.

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image7.png)
2. Kopieren Sie im Abfragefenster das gesamte SQL-Skript, und fügen Sie es aus der Datei Migrationen. SQL ein. Führen Sie die Skriptdatei aus, indem Sie auf die Schaltfläche "ausführen" klicken.

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image4.jpg)

    Aktualisieren Sie das Fenster Server-Explorer. In der Datenbank werden fünf neue Tabellen erstellt.

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image8.png)

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image9.png)

    Im folgenden wird erläutert, wie die Informationen in den SQL-Mitgliedschafts Tabellen dem neuen Identitätssystem zugeordnet werden.

    ASPNET-\_Rollen--&gt; aspnettroles

    ASP\_nettusers und ASP\_netmembership--&gt; aspnettusers

    ASPNET\_userinrollen--&gt; aspnettuserrollen

    Wie im obigen Abschnitt erläutert, sind die aspnettuserclaims-Tabelle und die aspnettuserlogins-Tabelle leer. Das Feld "Diskriminator" in der Tabelle "aspnettuser" sollte dem Modellklassen Namen entsprechen, der als nächster Schritt definiert ist. Die Spalte "PasswordHash" hat außerdem das Format "verschlüsseltes Kennwort | Kennwort Salt | Kenn Wort Format". Dies ermöglicht Ihnen die Verwendung einer speziellen kryptografielogik der SQL-Mitgliedschaft, sodass Sie alte Kenn Wörter wieder verwenden können. Dies wird weiter unten in diesem Artikel erläutert.

### <a name="creating-models-and-membership-pages"></a>Erstellen von Modellen und Mitgliedschafts Seiten

Wie bereits erwähnt, verwendet die IDENTITY-Funktion Entity Framework, um mit der Datenbank zu kommunizieren, um standardmäßig Kontoinformationen zu speichern. Um mit vorhandenen Daten in der Tabelle arbeiten zu können, müssen Modellklassen erstellt werden, die den Tabellen wieder zugeordnet und im Identitätssystem eingebunden werden. Als Teil des Identitäts Vertrags sollten die Modellklassen entweder die in der Identity. Core-DLL definierten Schnittstellen implementieren oder die vorhandene Implementierung dieser Schnittstellen erweitern, die in Microsoft. Aspnet. Identity. EntityFramework verfügbar sind.

In unserem Beispiel verfügen die aspnettroles-, aspnettuserclaims-, aspnetlogins-und aspnettuserrole-Tabellen über Spalten, die der vorhandenen Implementierung des Identitäts Systems ähneln. Daher können wir die vorhandenen Klassen für die Zuordnung zu diesen Tabellen wieder verwenden. Die aspnettuser-Tabelle enthält einige zusätzliche Spalten, die zum Speichern zusätzlicher Informationen aus den SQL-Mitgliedschafts Tabellen verwendet werden. Dies kann durch Erstellen einer Modell Klasse, die die vorhandene Implementierung von ' identityuser ' erweitert, zugeordnet werden und die zusätzlichen Eigenschaften hinzufügen.

1. Erstellen Sie im Projekt einen Ordner Modelle, und fügen Sie einen Klassen Benutzer hinzu. Der Name der Klasse sollte mit den in der Spalte "Diskriminator" der Tabelle "aspnettusers" hinzugefügten Daten identisch sein.

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image10.png)

    Die User-Klasse sollte die identityuser-Klasse erweitern, die in der *Microsoft. Aspnet. Identity. EntityFramework* -dll enthalten ist. Deklarieren Sie die Eigenschaften in der Klasse, die den aspnettuser-Spalten wieder zugeordnet werden. Die Eigenschaften "ID", "username", "PasswordHash" und "securitystamp" sind im identityuser definiert und werden daher ausgelassen. Im folgenden finden Sie den Code für die User-Klasse, die alle Eigenschaften enthält.

    [!code-csharp[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample3.cs)]
2. Eine Entity Framework dbcontext-Klasse ist erforderlich, um Daten in Modellen zurück in Tabellen zu speichern und Daten aus Tabellen abzurufen, um die Modelle aufzufüllen. *Microsoft.AspNet.Identity.EntityFramework* dll defines the IdentityDbContext class which interacts with the Identity tables to retrieve and store information. Der identitydbcontext-&lt;tuser-&gt; nimmt eine tuser-Klasse an, die jede Klasse sein kann, die die identityuser-Klasse erweitert.

    Erstellen Sie eine neue applicationdbcontext-Klasse, die identitydbcontext im Ordner "Models" erweitert und die in Schritt 1 erstellte Benutzerklasse übergibt.

    [!code-csharp[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample4.cs)]
3. Die Benutzerverwaltung im neuen Identitätssystem erfolgt mithilfe der in der *Microsoft. Aspnet. Identity. EntityFramework* -dll definierten "usermanager"-&lt;tuser-&gt; Klasse. Wir müssen eine benutzerdefinierte Klasse erstellen, die den usermanager erweitert und die in Schritt 1 erstellte Benutzerklasse übergibt.

    Erstellen Sie im Ordner Models eine neue Klasse usermanager, die Benutzer-Manager&lt;Benutzer erweitert&gt;

    [!code-csharp[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample5.cs)]
4. Die Kenn Wörter der Benutzer der Anwendung werden verschlüsselt und in der Datenbank gespeichert. Der Kryptografiealgorithmus, der in der SQL-Mitgliedschaft verwendet wird, unterscheidet sich von dem in der neuen Identitätssystem Um alte Kenn Wörter wiederzuverwenden, müssen wir Kenn Wörter selektiv entschlüsseln, wenn sich alte Benutzer mithilfe des SQL-Mitgliedschafts Algorithmus anmelden, während der Kryptografiealgorithmus in der Identität für die neuen Benutzer verwendet wird.

    Die usermanager-Klasse verfügt über die Eigenschaft "passwordhasher", die eine Instanz einer Klasse speichert, die die Schnittstelle "ipasswordhasher" implementiert. Diese wird zum Verschlüsseln/Entschlüsseln von Kenn Wörtern bei Benutzer Authentifizierungs Transaktionen verwendet. Erstellen Sie in der in Schritt 3 definierten usermanager-Klasse eine neue Klasse sqlpasswordhasher, und kopieren Sie den folgenden Code.

    [!code-csharp[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample6.cs)]

    Beheben Sie die Kompilierungsfehler, indem Sie die System. Text-und System. Security. Cryptography-Namespaces importieren.

    Die encodepassword-Methode verschlüsselt das Kennwort gemäß der standardmäßigen kryptografieimplementierung der SQL-Mitgliedschaft. Dies wird aus der System. Web-dll entnommen. Wenn die alte App eine benutzerdefinierte Implementierung verwendet hat, sollte Sie hier reflektiert werden. Wir müssen zwei andere Methoden " *hashpassword* " und " *verifyhashedpassword* " definieren, die mit der *encodepassword* -Methode einen Hashwert für ein bestimmtes Kennwort oder ein nur-Text-Kennwort mit dem in der Datenbank vorhandenen Kennwort verwenden.

    Das SQL-Mitgliedschaftssystem hat PasswordHash, PasswordSalt und passwordFormat verwendet, um das Kennwort zu übertragen, das von Benutzern beim Registrieren oder Ändern des Kennworts eingegeben wurde. Während der Migration werden alle drei Felder in der Spalte "PasswordHash" in der Tabelle "aspnettuser" getrennt durch das Zeichen "|" gespeichert. Wenn sich ein Benutzer anmeldet und das Kennwort diese Felder enthält, wird das Kennwort mit der SQL-Mitgliedschafts Kryptografie überprüft. Andernfalls wird das Kennwort mit der standardmäßigen kryptografiesystemverschlüsselung überprüft. Auf diese Weise müssten alte Benutzer ihre Kenn Wörter nicht ändern, nachdem die APP migriert wurde.
5. Deklarieren Sie den Konstruktor für die usermanager-Klasse, und übergeben Sie diesen als sqlpasswordhasher an die-Eigenschaft im-Konstruktor.

    [!code-csharp[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample7.cs)]

### <a name="create-new-account-management-pages"></a>Erstellen von neuen Kontoverwaltungsseiten

Der nächste Schritt der Migration besteht darin, Kontoverwaltungsseiten hinzuzufügen, mit denen sich ein Benutzer registrieren und anmelden kann. Auf den alten Konto Seiten aus der SQL-Mitgliedschaft werden Steuerelemente verwendet, die nicht mit dem neuen Identitätssystem funktionieren. Wenn Sie die neuen Benutzer Verwaltungs Seiten hinzufügen möchten, befolgen Sie das Tutorial unter diesem Link [https://www.asp.net/identity/overview/getting-started/adding-aspnet-identity-to-an-empty-or-existing-web-forms-project](../getting-started/adding-aspnet-identity-to-an-empty-or-existing-web-forms-project.md) beginnend mit dem Schritt "Hinzufügen von Web Forms für die Registrierung von Benutzern bei Ihrer Anwendung", da wir das Projekt bereits erstellt und die nuget-Pakete hinzugefügt haben.

Wir müssen einige Änderungen vornehmen, damit das Beispiel mit dem hier aufgeführten Projekt funktioniert.

- Die Register.aspx.cs-und Login.aspx.cs-Code-Behind-Klassen verwenden den `UserManager` aus den Identitäts Paketen, um einen Benutzer zu erstellen. Verwenden Sie für dieses Beispiel den im Ordner Models hinzugefügten Benutzer-Manager, indem Sie die zuvor beschriebenen Schritte ausführen.
- Verwenden Sie die Benutzerklasse, die anstelle von identityuser in Register.aspx.cs-und Login.aspx.cs-Code Behind-Klassen erstellt wurde. Dadurch wird die benutzerdefinierte Benutzerklasse mit dem Identitätssystem verknüpft.
- Der Teil zum Erstellen der Datenbank kann übersprungen werden.
- Der Entwickler muss die ApplicationId für den neuen Benutzer entsprechend der aktuellen Anwendungs-ID festlegen. Hierzu können Sie die ApplicationId für diese Anwendung Abfragen, bevor ein Benutzerobjekt in der Register.aspx.cs-Klasse erstellt und vor dem Erstellen des Benutzers festgelegt wird.

    Beispiel:

    Definieren Sie eine Methode auf der Register.aspx.cs-Seite, um die ASPNET-\_Anwendungs Tabelle abzufragen und die Anwendungs-ID gemäß dem Anwendungsnamen zu erhalten.

    [!code-csharp[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample8.cs)]

    Legen Sie jetzt diese Einstellung für das User-Objekt fest.

    [!code-csharp[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample9.cs)]

Verwenden Sie den alten Benutzernamen und das Kennwort, um einen vorhandenen Benutzer anzumelden. Verwenden Sie die Seite registrieren, um einen neuen Benutzer zu erstellen. Vergewissern Sie sich außerdem, dass die Benutzer wie erwartet in den Rollen sind.

Das Portieren auf das Identitätssystem unterstützt den Benutzer beim Hinzufügen offener Authentifizierung (OAuth) zur Anwendung. Weitere Informationen finden Sie in [diesem Beispiel,](https://github.com/aspnet/samples/tree/master/samples/aspnet/Identity/SQLMembership-Identity-OWIN/) in dem OAuth aktiviert ist.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial wurde gezeigt, wie Sie Benutzer aus der SQL-Mitgliedschaft in ASP.net Identity portieren, aber keine Profildaten. Im nächsten Tutorial wird das Portieren von Profildaten aus der SQL-Mitgliedschaft in das neue Identitätssystem untersucht.

Sie können das Feedback am Ende dieses Artikels hinterlassen.

*Vielen Dank, dass Tom Dykstra und Rick Anderson den Artikel überprüft haben.*
