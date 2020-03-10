---
uid: identity/overview/extensibility/implementing-a-custom-mysql-aspnet-identity-storage-provider
title: Implementieren eines benutzerdefinierten MySQL-ASP.net Identity Speicher Anbieters ASP.NET 4. x
author: raquelsa
description: ASP.net Identity ist ein erweiterbares System, mit dem Sie einen eigenen Speicher Anbieter erstellen und in Ihre Anwendung einbinden können, ohne den Löschvorgang erneut zu arbeiten...
ms.author: riande
ms.date: 05/22/2015
ms.assetid: 248f5fe7-39ba-40ea-ab1e-71a69b0bd649
ms.custom: seoapril2019
msc.legacyurl: /identity/overview/extensibility/implementing-a-custom-mysql-aspnet-identity-storage-provider
msc.type: authoredcontent
ms.openlocfilehash: 2f0b47d45bce82c71d1864536309f9e2ffed2d63
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78500067"
---
# <a name="implementing-a-custom-mysql-aspnet-identity-storage-provider"></a>Implementieren eines benutzerdefinierten MySQL ASP.NET Identity-Speicheranbieters

von [Raquel Soares de Almeida](https://github.com/raquelsa), [Suhas Joshi](https://github.com/suhasj), [Tom fitzmacken](https://github.com/tfitzmac)

> ASP.net Identity ist ein erweiterbares System, mit dem Sie einen eigenen Speicher Anbieter erstellen und in Ihre Anwendung einbinden können, ohne die Anwendung neu zu arbeiten. In diesem Thema wird beschrieben, wie ein MySQL-Speicher Anbieter für ASP.net Identity erstellt wird. Eine Übersicht über das Erstellen von benutzerdefinierten Speicheranbietern finden Sie unter [Übersicht über benutzerdefinierte Speicher Anbieter für ASP.net Identity](overview-of-custom-storage-providers-for-aspnet-identity.md).
> 
> Um dieses Tutorial abzuschließen, müssen Sie über Visual Studio 2013 mit Update 2 verfügen.
> 
> In diesem Tutorial wird Folgendes angezeigt:
> 
> - Zeigen Sie, wie Sie eine MySQL-Daten Bank Instanz in Azure erstellen.
> - Zeigen Sie, wie Sie mit einem MySQL-Client Tool (MySQL Workbench) Tabellen erstellen und Ihre Remote Datenbank in Azure verwalten.
> - Zeigen Sie, wie Sie die Standard Implementierung des ASP.net Identity Speichers durch unsere benutzerdefinierte Implementierung für ein MVC-Anwendungsprojekt ersetzen.
> 
> Dieses Tutorial wurde ursprünglich von Raquel Soares de Almeida und Rick Anderson ( [@RickAndMSFT](https://twitter.com/#!/RickAndMSFT) ) verfasst. Das Beispiel Projekt wurde für die Identität 2,0 von Suhas Joshi aktualisiert. Das Thema wurde für die Identität 2,0 von Tom fitzmacken aktualisiert.

## <a name="download-completed-project"></a>Herunterladen des abgeschlossenen Projekts

Am Ende dieses Tutorials verfügen Sie über ein MVC-Anwendungsprojekt, das ASP.net Identity mit einer MySQL-Datenbank arbeitet, die in Azure gehostet wird.

Sie können den abgeschlossenen MySQL-Speicher Anbieter unter [ASPNET. Identity. MySQL (GitHub)](https://github.com/aspnet/samples/tree/master/samples/aspnet/Identity/AspNet.Identity.MySQL)herunterladen.

## <a name="the-steps-you-will-perform"></a>Die Schritte, die Sie ausführen werden

In diesem Lernprogramm führen Sie folgende Schritte aus:

1. Erstellen einer MySQL-Datenbank in Azure
2. Erstellen der ASP.net Identity Tabellen in MySQL
3. Erstellen einer MVC-Anwendung und Konfigurieren der Anwendung für die Verwendung des MySQL-Anbieters
4. Ausführen der App

Dieses Thema behandelt nicht die Architektur der ASP.net Identity und die Entscheidungen, die Sie beim Implementieren eines Kunden Speicher Anbieters treffen müssen. Diese Informationen finden Sie unter [Übersicht über benutzerdefinierte Speicher Anbieter für ASP.net Identity](overview-of-custom-storage-providers-for-aspnet-identity.md).

## <a name="review-mysql-storage-provider-classes"></a>Überprüfen der MySQL-Speicher Anbieter Klassen

Bevor Sie mit den Schritten zum Erstellen des MySQL-Speicher Anbieters beginnen, betrachten wir die Klassen, die den Speicher Anbieter bilden. Sie benötigen Klassen, die die Daten Bank Vorgänge und-Klassen verwalten, die von der Anwendung aufgerufen werden, um Benutzer und Rollen zu verwalten.

### <a name="storage-classes"></a>Speicherklassen

- [Identityuser](https://github.com/aspnet/samples/blob/master/samples/aspnet/Identity/AspNet.Identity.MySQL/IdentityUser.cs) : enthält Eigenschaften für den Benutzer.
- [Userstore](https://github.com/aspnet/samples/blob/master/samples/aspnet/Identity/AspNet.Identity.MySQL/UserStore.cs) : enthält Vorgänge zum Hinzufügen, aktualisieren oder Abrufen von Benutzern.
- [Identityrole](https://github.com/aspnet/samples/blob/master/samples/aspnet/Identity/AspNet.Identity.MySQL/IdentityRole.cs) : enthält Eigenschaften für Rollen.
- [Rolestore](https://github.com/aspnet/samples/blob/master/samples/aspnet/Identity/AspNet.Identity.MySQL/RoleStore.cs) : enthält Vorgänge zum Hinzufügen, löschen, aktualisieren und Abrufen von Rollen.

### <a name="data-access-layer-classes"></a>Klassen für Datenzugriffsebenen

In diesem Beispiel enthalten die Klassen der Datenzugriffs Ebene SQL-Anweisungen zum Arbeiten mit den Tabellen. in Ihrem Code möchten Sie jedoch möglicherweise Objekt relationale Zuordnung (ORM) verwenden, wie z. b. Entity Framework oder NHibernate. Insbesondere kann es sein, dass Ihre Anwendung eine schlechte Leistung ohne ORM hat, die Lazy Loading und das Zwischenspeichern von Objekten umfasst. Weitere Informationen finden Sie unter [ASP.net Identity 2,0 ohne Entity Framework?](https://aspnetidentity.codeplex.com/discussions/561828)

- [Mysqldatabase](https://github.com/aspnet/samples/blob/master/samples/aspnet/Identity/AspNet.Identity.MySQL/MySQLDatabase.cs) : enthält die MySQL-Datenbankverbindung und Methoden zum Ausführen von Daten Bank Vorgängen. Userstore und rolestore werden beide mit einer Instanz dieser Klasse instanziiert.
- [Roletable](https://github.com/aspnet/samples/blob/master/samples/aspnet/Identity/AspNet.Identity.MySQL/RoleTable.cs) : enthält Daten Bank Vorgänge für die Tabelle, in der Rollen gespeichert werden.
- [Userclaimstable](https://github.com/aspnet/samples/blob/master/samples/aspnet/Identity/AspNet.Identity.MySQL/UserClaimsTable.cs) : enthält Daten Bank Vorgänge für die Tabelle, in der Benutzer Ansprüche gespeichert werden.
- [Userloginstable](https://github.com/aspnet/samples/blob/master/samples/aspnet/Identity/AspNet.Identity.MySQL/UserLoginsTable.cs) : enthält Daten Bank Vorgänge für die Tabelle, in der die Benutzer Anmelde Informationen gespeichert werden.
- [Userroletable](https://github.com/aspnet/samples/blob/master/samples/aspnet/Identity/AspNet.Identity.MySQL/UserRoleTable.cs) : enthält Daten Bank Vorgänge für die Tabelle, in der gespeichert wird, welche Benutzer welchen Rollen zugewiesen sind.
- [Usertable](https://github.com/aspnet/samples/blob/master/samples/aspnet/Identity/AspNet.Identity.MySQL/UserTable.cs) : enthält Daten Bank Vorgänge für die Tabelle, in der Benutzer gespeichert werden.

## <a name="create-a-mysql-database-instance-on-azure"></a>Erstellen einer MySQL-Daten Bank Instanz in Azure

1. Melden Sie sich beim [Azure-Portal](https://manage.windowsazure.com/)an.
2. Klicken Sie unten auf der Seite auf **+ neu** , und wählen Sie dann **Speichern**aus.  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image1.png)
3. Wählen Sie im Assistenten zum **auswählen und hinzufügen** von **cleardb MySQL-Datenbank** aus, und klicken Sie unten rechts im Dialogfeld auf den Pfeil "weiter".  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image2.png)
4. Behalten Sie den standardmäßigen **Free** -Plan bei, und ändern Sie den **Namen** in **identitymysqldatabase**. Wählen Sie die nächstgelegene Region aus, und klicken Sie dann auf den weiter-Pfeil.  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image3.png)
5. Aktivieren Sie das Häkchensymbol, um die Daten Bank Erstellung abzuschließen.  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image4.png)
6. Nachdem die Datenbank erstellt wurde, können Sie sie auf der Registerkarte **ADD-ONS** im Verwaltungsportal verwalten.   
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image5.png)
7. Sie können die Daten bankverbindungs Informationen erhalten, indem Sie unten auf der Seite auf **Verbindungs** Informationen klicken.  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image6.png)
8. Kopieren Sie die Verbindungs Zeichenfolge durch Klicken auf die Schaltfläche Kopieren, und speichern Sie Sie, damit Sie Sie später in ihrer MVC-Anwendung verwenden können.   
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image7.png)

## <a name="create-the-aspnet-identity-tables-in-a-mysql-database"></a>Erstellen der ASP.net Identity Tabellen in einer MySQL-Datenbank

### <a name="install-mysql-workbench-tool-to-connect-and-manage-mysql-database"></a>Installieren des MySQL Workbench-Tools zum Verbinden und Verwalten der MySQL-Datenbank

1. Installieren Sie das **MySQL Workbench** -Tool auf der [MySQL-Downloadseite](http://dev.mysql.com/downloads/windows/installer/) .
2. Starten Sie die APP, und klicken Sie auf die Schaltfläche **mysqlconnections +** , um eine neue Verbindung hinzuzufügen. Verwenden Sie die Verbindungs Zeichenfolgen-Daten, die Sie aus der zuvor in diesem Tutorial erstellten Azure MySQL-Datenbank kopiert haben.
3. Nachdem Sie die Verbindung hergestellt haben, öffnen Sie eine neue **Abfrage** Registerkarte. Fügen Sie die Befehle aus " [mysqlidentity. SQL](https://github.com/aspnet/samples/blob/master/samples/aspnet/Identity/AspNet.Identity.MySQL/MySQLIdentity.sql) " in die Abfrage ein, und führen Sie Sie aus, um die Datenbanktabellen zu erstellen.
4. Sie verfügen nun über alle ASP.net Identity notwendigen Tabellen, die in einer in Azure gehosteten MySQL-Datenbank erstellt wurden, wie unten gezeigt.  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image1.jpg)

## <a name="create-an-mvc-application-project-from-template-and-configure-it-to-use-mysql-provider"></a>Erstellen Sie ein MVC-Anwendungsprojekt aus einer Vorlage, und konfigurieren Sie es für die Verwendung von MySQL

Installieren Sie bei Bedarf entweder [Visual Studio Express 2013 für Web](https://go.microsoft.com/fwlink/?LinkId=299058) oder [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566) mit Update 2.

### <a name="download-the-aspnetidentitymysql-project-from-github"></a>Herunterladen des Projekts "ASP. net. Identity. MySQL" aus GitHub

1. Navigieren Sie zu der Repository-URL unter [ASPNET. Identity. MySQL (GitHub)](https://github.com/aspnet/samples/tree/master/samples/aspnet/Identity/AspNet.Identity.MySQL/).
2. Herunterladen des Quellcodes.
3. Extrahieren Sie die ZIP-Datei in einen lokalen Ordner.
4. Öffnen Sie die Projekt Mappe ASPNET. Identity. MySQL, und erstellen Sie Sie.

### <a name="create-a-new-mvc-application-project-from-template"></a>Erstellen Sie ein neues MVC-Anwendungsprojekt aus einer Vorlage.

1. Klicken Sie mit der rechten Maustaste auf die **Projekt** Mappe **ASPNET. Identity. MySQL** , und **fügen Sie hinzu**
2. Wählen Sie im Dialog Feld **Neues Projekt hinzufügen** auf der linken Seite die Option **Visualisierung C#**  und dann **Web** und dann **ASP.NET Webanwendung**aus. Benennen Sie Ihr Projekt **identitymysqldemo**; und klicken Sie dann auf OK.  
  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image2.jpg)
3. Wählen Sie im Dialogfeld **Neues ASP.net-Projekt** die MVC-Vorlage mit den Standardoptionen (die **einzelne Benutzerkonten** als Authentifizierungsmethode enthält) aus, und klicken Sie auf **OK**.![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image3.jpg)
4. Klicken Sie in Projektmappen-Explorer mit der rechten Maustaste auf das Projekt identitymysqldemo, und wählen Sie **nuget-Pakete verwalten**aus. Geben Sie im Dialogfeld Such Textfeld den Text **Identity. EntityFramework**ein. Wählen Sie dieses Paket in der Liste der Ergebnisse aus, und klicken Sie auf **deinstallieren**. Sie werden aufgefordert, das Abhängigkeits Paket EntityFramework zu deinstallieren. Klicken Sie auf Ja, da dieses Paket nicht mehr in dieser Anwendung angezeigt wird.
5. Klicken Sie mit der rechten Maustaste auf das Projekt identitymysqldemo, wählen Sie **Hinzufügen**, **Verweis, Projekt Mappe, Projekte** aus, und klicken **Sie auf**das Projekt ASPNET. Identity. MySQL.
6. Ersetzen Sie im Projekt identitymysqldemo alle Verweise auf.  
    `using Microsoft.AspNet.Identity.EntityFramework;`  
   with  
     `using AspNet.Identity.MySQL;`
7. Legen Sie in IdentityModels.cs **applicationdbcontext** auf Ableitung von **mysqldatabase** fest, und fügen Sie einen Konstruktor ein, der einen einzelnen Parameter mit dem Verbindungs Namen annimmt.  

    [!code-csharp[Main](implementing-a-custom-mysql-aspnet-identity-storage-provider/samples/sample1.cs)]
8. Öffnen Sie die Datei IdentityConfig.cs. Ersetzen Sie in der **applicationusermanager. Create** -Methode die Instanziierung von usermanager durch folgenden Code:  

    [!code-csharp[Main](implementing-a-custom-mysql-aspnet-identity-storage-provider/samples/sample2.cs)]
9. Öffnen Sie die Datei Web. config, und ersetzen Sie die DefaultConnection-Zeichenfolge durch diesen Eintrag. ersetzen Sie dabei die markierten Werte durch die Verbindungs Zeichenfolge der MySQL-Datenbank, die Sie in den vorherigen Schritten  

    [!code-xml[Main](implementing-a-custom-mysql-aspnet-identity-storage-provider/samples/sample3.xml?highlight=2)]

## <a name="run-the-app-and-connect-to-the-mysql-db"></a>Ausführen der APP und Herstellen einer Verbindung mit der MySQL-Datenbank

1. Klicken Sie mit der rechten Maustaste auf das Projekt **identitymysqldemo** , und wählen Sie **als Startprojekt festlegen**
2. Drücken Sie **STRG + F5** , um die APP zu erstellen und auszuführen.
3. Klicken Sie oben auf der Seite auf Registerkarte **registrieren** .
4. Geben Sie einen neuen Benutzernamen und ein Kennwort ein, und klicken Sie dann auf **registrieren**.  
  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image8.png)
5. Der neue Benutzer ist nun registriert und angemeldet.  
  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image9.png)
6. Wechseln Sie zurück zum MySQL Workbench-Tool, und überprüfen Sie den Inhalt der **identitymysqldatabase** -Tabelle. Überprüfen Sie die Tabelle "Benutzer" auf die Einträge, während Sie neue Benutzer registrieren.  
  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image10.png)

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Aktivieren anderer Authentifizierungsmethoden für diese APP finden Sie unter [Erstellen einer ASP.NET MVC 5-App mit Facebook und Google OAuth2 und OpenID-Anmeldung](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).

Informationen dazu, wie Sie Ihre Datenbank mit OAuth integrieren und Rollen einrichten, um den Benutzer Zugriff auf Ihre APP einzuschränken, finden Sie unter Bereitstellen [einer Secure ASP.NET MVC 5-App mit Mitgliedschaft, OAuth und SQL-Datenbank in Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).
