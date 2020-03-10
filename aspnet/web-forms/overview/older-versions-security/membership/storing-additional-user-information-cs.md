---
uid: web-forms/overview/older-versions-security/membership/storing-additional-user-information-cs
title: Speichern zusätzlicher Benutzerinformationen (C#) | Microsoft-Dokumentation
author: rick-anderson
description: In diesem Tutorial beantworten wir diese Frage, indem wir eine sehr rudimentäre Gäste Buch Anwendung aufbauen. In diesem Fall werden wir verschiedene Optionen für modeli betrachten...
ms.author: riande
ms.date: 01/18/2008
ms.assetid: 1642132a-1ca5-4872-983f-ab59fc8865d3
msc.legacyurl: /web-forms/overview/older-versions-security/membership/storing-additional-user-information-cs
msc.type: authoredcontent
ms.openlocfilehash: 24b96e86bc93e03d2639b73e35ed1fd1271bac5a
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78515457"
---
# <a name="storing-additional-user-information-c"></a>Speichern von zusätzlichen Benutzerinformationen (C#)

von [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Code herunterladen](https://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/ASPNET_Security_Tutorial_08_CS.zip) oder [PDF herunterladen](https://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/aspnet_tutorial08_ExtraUserInfo_cs.pdf)

> In diesem Tutorial beantworten wir diese Frage, indem wir eine sehr rudimentäre Gäste Buch Anwendung aufbauen. Dabei werden verschiedene Optionen zum Modellieren von Benutzerinformationen in einer Datenbank erläutert. Anschließend wird erläutert, wie Sie diese Daten den Benutzerkonten zuordnen, die vom Mitgliedschafts Framework erstellt wurden.

## <a name="introduction"></a>Einführung

ASP. Das Mitgliedschafts Framework von NET bietet eine flexible Oberfläche zum Verwalten von Benutzern. Die Mitgliedschafts-API umfasst Methoden zum Überprüfen von Anmelde Informationen, zum Abrufen von Informationen über den aktuell angemeldeten Benutzer, zum Erstellen eines neuen Benutzerkontos und zum Löschen eines Benutzerkontos. Jedes Benutzerkonto im Mitgliedschafts Framework enthält nur die Eigenschaften, die zum Überprüfen von Anmelde Informationen und zum Ausführen wichtiger Benutzerkonto bezogenen Aufgaben erforderlich sind. Dies wird durch die Methoden und Eigenschaften der`MembershipUser`- [Klasse](https://msdn.microsoft.com/library/system.web.security.membershipuser.aspx)gezeigt, die ein Benutzerkonto im Mitgliedschafts Framework modelliert. Diese Klasse verfügt über Eigenschaften wie [`UserName`](https://msdn.microsoft.com/library/system.web.security.membershipuser.username.aspx), [`Email`](https://msdn.microsoft.com/library/system.web.security.membershipuser.email.aspx)und [`IsLockedOut`](https://msdn.microsoft.com/library/system.web.security.membershipuser.islockedout.aspx)sowie Methoden wie [`GetPassword`](https://msdn.microsoft.com/library/system.web.security.membershipuser.getpassword.aspx) und [`UnlockUser`](https://msdn.microsoft.com/library/system.web.security.membershipuser.unlockuser.aspx).

Oft müssen Anwendungen zusätzliche Benutzerinformationen speichern, die nicht im Mitgliedschafts Framework enthalten sind. Beispielsweise muss ein Onlinehändler möglicherweise jedem Benutzer die Versand-und abrechnungsadressen, Zahlungsinformationen, Übermittlungs Einstellungen und die Telefonnummer des Kontakts speichern. Außerdem ist jede Bestellung im System einem bestimmten Benutzerkonto zugeordnet.

Die `MembershipUser`-Klasse enthält keine Eigenschaften wie `PhoneNumber` oder `DeliveryPreferences` oder `PastOrders`. Wie werden die Benutzerinformationen, die von der Anwendung benötigt werden, nachverfolgt und in das Mitgliedschafts Framework integriert? In diesem Tutorial beantworten wir diese Frage, indem wir eine sehr rudimentäre Gäste Buch Anwendung aufbauen. Dabei werden verschiedene Optionen zum Modellieren von Benutzerinformationen in einer Datenbank erläutert. Anschließend wird erläutert, wie Sie diese Daten den Benutzerkonten zuordnen, die vom Mitgliedschafts Framework erstellt wurden. Erste Schritte

## <a name="step-1-creating-the-guestbook-applications-data-model"></a>Schritt 1: Erstellen des Datenmodells für die Gäste Buch Anwendung

Es gibt eine Vielzahl von Verfahren, die zum Erfassen von Benutzerinformationen in einer Datenbank verwendet werden können, und die Sie den Benutzerkonten zuordnen, die durch das Mitgliedschafts Framework erstellt wurden. Um diese Techniken zu veranschaulichen, muss die Tutorial-Webanwendung erweitert werden, damit Sie eine Art Benutzer bezogener Daten erfasst. (Derzeit enthält das Datenmodell der Anwendung nur die Anwendungs Dienst Tabellen, die vom `SqlMembershipProvider`benötigt werden.)

Erstellen Sie eine sehr einfache Gäste Buch Anwendung, bei der ein authentifizierter Benutzer einen Kommentar hinterlassen kann. Zusätzlich zum Speichern von Gäste Buch Kommentaren erlauben wir jedem Benutzer, seine Homepage, Homepage und Signatur zu speichern. Wenn bereitgestellt, werden die Homepage, die Homepage und die Signatur des Benutzers für jede Nachricht angezeigt, die er im Gäste Buch verbleiben muss.

### <a name="adding-theguestbookcommentstable"></a>Hinzufügen der`GuestbookComments`Tabelle

Um die Gäste Buch Kommentare zu erfassen, müssen wir eine Datenbanktabelle mit dem Namen `GuestbookComments` erstellen, die Spalten wie `CommentId`, `Subject`, `Body`und `CommentDate`enthält. Außerdem muss jeder Datensatz in der `GuestbookComments` Tabelle auf den Benutzer verweisen, der den Kommentar hinterlassen hat.

Um diese Tabelle zur Datenbank hinzuzufügen, wechseln Sie zum Datenbank-Explorer in Visual Studio, und führen Sie einen Drilldown in die `SecurityTutorials` Datenbank aus. Klicken Sie mit der rechten Maustaste auf den Ordner Tabellen, und wählen Sie neue Tabelle hinzufügen. Dadurch wird eine Schnittstelle geöffnet, mit der wir die Spalten für die neue Tabelle definieren können.

[![der securitytutorials-Datenbank eine neue Tabelle hinzufügen](storing-additional-user-information-cs/_static/image2.png)](storing-additional-user-information-cs/_static/image1.png)

**Abbildung 1**: Hinzufügen einer neuen Tabelle zur `SecurityTutorials` Datenbank ([Klicken Sie, um das Bild in voller Größe anzuzeigen](storing-additional-user-information-cs/_static/image3.png))

Definieren Sie als nächstes die Spalten des `GuestbookComments`. Fügen Sie zunächst eine Spalte mit dem Namen `CommentId` vom Typ `uniqueidentifier`hinzu. In dieser Spalte wird jeder Kommentar im Gäste Buch eindeutig identifiziert, sodass keine `NULL` s zugelassen und als Primärschlüssel der Tabelle gekennzeichnet werden kann. Anstatt einen Wert für das Feld `CommentId` für jede `INSERT`bereitzustellen, können wir angeben, dass ein neuer `uniqueidentifier` Wert automatisch für dieses Feld auf `INSERT` generiert werden soll, indem der Standardwert der Spalte auf `NEWID()`festgelegt wird. Nachdem Sie dieses erste Feld hinzugefügt, als Primärschlüssel markiert und seinen Standardwert eingestellt haben, sollte der Bildschirm in etwa wie in Abbildung 2 dargestellt aussehen.

[![eine primäre Spalte namens "CommentID" hinzufügen.](storing-additional-user-information-cs/_static/image5.png)](storing-additional-user-information-cs/_static/image4.png)

**Abbildung 2**: Hinzufügen einer primären Spalte mit dem Namen `CommentId` ([Klicken Sie, um das Bild in voller Größe anzuzeigen](storing-additional-user-information-cs/_static/image6.png))

Fügen Sie als nächstes eine Spalte mit dem Namen `Subject` vom Typ `nvarchar(50)` und eine Spalte mit dem Namen `Body` vom Typ `nvarchar(MAX)`hinzu, wobei `NULL` s in beiden Spalten nicht zulässig sind. Fügen Sie danach eine Spalte mit dem Namen `CommentDate` vom Typ `datetime`hinzu. Deaktivieren Sie `NULL` s, und legen Sie den Standardwert der `CommentDate` Spalte auf `getdate()`fest.

Nur noch müssen Sie eine Spalte hinzufügen, die jedem Gäste Buch Kommentar ein Benutzerkonto zuordnet. Eine Möglichkeit besteht darin, eine Spalte mit dem Namen `UserName` vom Typ `nvarchar(256)`hinzuzufügen. Dies ist eine geeignete Wahl, wenn Sie einen anderen Mitgliedschafts Anbieter als den `SqlMembershipProvider`verwenden. Wenn Sie die `SqlMembershipProvider`aber in dieser tutorialreihe verwenden, ist die `UserName` Spalte in der `aspnet_Users` Tabelle nicht garantiert eindeutig. Der Primärschlüssel der `aspnet_Users` Tabelle ist `UserId` und hat den Typ `uniqueidentifier`. Daher benötigt die `GuestbookComments` Tabelle eine Spalte mit dem Namen `UserId` vom Typ `uniqueidentifier` (`NULL` Werte werden nicht zugelassen). Fügen Sie diese Spalte hinzu.

> [!NOTE]
> Wie im Tutorial [*Erstellen des Mitgliedschafts Schemas in SQL Server*](creating-the-membership-schema-in-sql-server-cs.md) erläutert, ist das Mitgliedschafts Framework so konzipiert, dass mehrere Webanwendungen mit unterschiedlichen Benutzerkonten denselben Benutzerspeicher gemeinsam nutzen können. Dies erfolgt durch Partitionierung von Benutzerkonten in verschiedenen Anwendungen. Und obwohl jeder Benutzername innerhalb einer Anwendung eindeutig eindeutig ist, kann derselbe Benutzername in verschiedenen Anwendungen verwendet werden, die denselben Benutzerspeicher verwenden. Es gibt eine zusammengesetzte `UNIQUE` Einschränkung in der `aspnet_Users` Tabelle der Felder `UserName` und `ApplicationId`, aber nicht nur eines für das `UserName` Feld. Folglich ist es möglich, dass die ASPNET-\_Benutzertabelle über zwei (oder mehr) Datensätze mit demselben `UserName` Wert verfügt. Es gibt jedoch eine `UNIQUE` Einschränkung für das `UserId` Feld der `aspnet_Users` Tabelle (da es sich um den Primärschlüssel handelt). Eine `UNIQUE` Einschränkung ist wichtig, da keine FOREIGN KEY-Einschränkung zwischen den Tabellen "`GuestbookComments`" und "`aspnet_Users`" hergestellt werden kann.

Nachdem Sie die Spalte `UserId` hinzugefügt haben, speichern Sie die Tabelle, indem Sie auf das Symbol Speichern auf der Symbolleiste klicken. Benennen Sie die neue Tabelle `GuestbookComments`.

Bei der `GuestbookComments` Tabelle ist ein letztes Problem aufgetreten: Wir müssen eine [Foreign Key-Einschränkung](https://msdn.microsoft.com/library/ms175464.aspx) zwischen der `GuestbookComments.UserId` Spalte und der `aspnet_Users.UserId` Spalte erstellen. Um dies zu erreichen, klicken Sie auf das Beziehungs Symbol in der Symbolleiste, um das Dialogfeld Fremdschlüssel Beziehungen aufzurufen. (Alternativ können Sie dieses Dialogfeld starten, indem Sie zum Menü Tabellen-Designer wechseln und Beziehungen auswählen.)

Klicken Sie unten links im Dialogfeld Fremdschlüssel Beziehungen auf die Schaltfläche hinzufügen. Dadurch wird eine neue FOREIGN KEY-Einschränkung hinzugefügt, obwohl wir weiterhin die Tabellen definieren müssen, die an der Beziehung beteiligt sind.

[![das Dialog Feld Fremdschlüssel Beziehungen zum Verwalten der FOREIGN KEY-Einschränkungen einer Tabelle verwenden.](storing-additional-user-information-cs/_static/image8.png)](storing-additional-user-information-cs/_static/image7.png)

**Abbildung 3**: Verwenden des Dialog Felds Fremdschlüssel Beziehungen zum Verwalten der FOREIGN KEY-Einschränkungen einer Tabelle ([Klicken Sie, um das Bild in voller Größe anzuzeigen](storing-additional-user-information-cs/_static/image9.png))

Klicken Sie anschließend auf der rechten Seite in der Zeile "Tabellen-und Spalten Spezifikationen" auf das Symbol mit den Auslassungs Punkten. Dadurch wird das Dialogfeld Tabellen und Spalten geöffnet, in dem wir die Primärschlüssel Tabelle und-Spalte sowie die Fremdschlüssel Spalte aus der `GuestbookComments` Tabelle angeben können. Wählen Sie insbesondere `aspnet_Users` und `UserId` als Primärschlüssel Tabelle und-Spalte aus, und `UserId` aus der `GuestbookComments` Tabelle als Fremdschlüssel Spalte (siehe Abbildung 4). Nachdem Sie die Primär-und Fremdschlüssel Tabellen und-Spalten definiert haben, klicken Sie auf OK, um zum Dialogfeld Fremdschlüssel Beziehungen zurückzukehren.

[![eine FOREIGN KEY-Einschränkung zwischen den Tabellen aspnet_Users und guesbookcomments herzustellen.](storing-additional-user-information-cs/_static/image11.png)](storing-additional-user-information-cs/_static/image10.png)

**Abbildung 4**: Einrichten einer FOREIGN KEY-Einschränkung zwischen den Tabellen "`aspnet_Users`" und "`GuesbookComments`" ([Klicken Sie, um das Bild in voller Größe anzuzeigen](storing-additional-user-information-cs/_static/image12.png))

An diesem Punkt wurde die FOREIGN KEY-Einschränkung eingerichtet. Wenn diese Einschränkung vorhanden ist, wird die [relationale Integrität](http://en.wikipedia.org/wiki/Referential_integrity) zwischen den beiden Tabellen sichergestellt, indem sichergestellt wird, dass nie ein Gästebucheintrag auf ein nicht vorhandenes Benutzerkonto verweist. Standardmäßig lässt eine FOREIGN KEY-Einschränkung nicht zu, dass ein übergeordneter Datensatz gelöscht wird, wenn entsprechende untergeordnete Datensätze vorhanden sind. Das heißt, wenn ein Benutzer einen oder mehrere Gäste Buch Kommentare erstellt und dann versucht, das Benutzerkonto zu löschen, schlägt der Löschvorgang fehl, es sei denn, seine Gäste Buch Kommentare werden zuerst gelöscht.

Foreign Key-Einschränkungen können so konfiguriert werden, dass die zugehörigen untergeordneten Datensätze automatisch gelöscht werden, wenn ein übergeordneter Datensatz gelöscht wird. Anders ausgedrückt: Wir können diese Foreign Key-Einschränkung so einrichten, dass die Gästebucheinträge eines Benutzers automatisch gelöscht werden, wenn Ihr Benutzerkonto gelöscht wird. Erweitern Sie hierzu den Abschnitt "INSERT-und Update-Spezifikation", und legen Sie die Eigenschaft "Regel löschen" auf Cascade fest.

[![die FOREIGN KEY-Einschränkung So konfigurieren, dass Löschvorgänge gelöscht werden](storing-additional-user-information-cs/_static/image14.png)](storing-additional-user-information-cs/_static/image13.png)

**Abbildung 5**: Konfigurieren der FOREIGN KEY-Einschränkung zum Kaskadieren von Lösch Vorgängen ([Klicken Sie, um das Bild in voller Größe anzuzeigen](storing-additional-user-information-cs/_static/image15.png)

Um die FOREIGN KEY-Einschränkung zu speichern, klicken Sie auf die Schaltfläche Schließen, um die Fremdschlüssel Beziehungen zu beenden. Klicken Sie dann auf der Symbolleiste auf das Symbol speichern, um die Tabelle und die Beziehung zu speichern.

### <a name="storing-the-users-home-town-homepage-and-signature"></a>Speichern der Homepage, Homepage und Signatur des Benutzers

Die `GuestbookComments` Tabelle veranschaulicht, wie Informationen gespeichert werden, die eine 1: n-Beziehung mit Benutzerkonten gemeinsam haben. Da jedes Benutzerkonto über eine beliebige Anzahl zugeordneter Kommentare verfügen kann, wird diese Beziehung modelliert, indem eine Tabelle erstellt wird, in der die Kommentare enthalten sind, die eine Spalte enthalten, die jeden Kommentar mit einem bestimmten Benutzer verknüpft. Wenn Sie die `SqlMembershipProvider`verwenden, wird dieser Link am besten festgelegt, indem eine Spalte mit dem Namen `UserId` vom Typ `uniqueidentifier` und eine FOREIGN KEY-Einschränkung zwischen dieser Spalte und `aspnet_Users.UserId`erstellt wird.

Wir müssen nun jedem Benutzerkonto drei Spalten zuordnen, um die Homepage, die Homepage und die Signatur des Benutzers zu speichern, die in seinen Gäste Buch Kommentaren angezeigt werden. Hierfür gibt es verschiedene Möglichkeiten:

- <strong>Fügen Sie den</strong> Tabellen<strong>`aspnet_Users`</strong> <strong>oder</strong> <strong>`aspnet_Membership`</strong>neue Spalten hinzu <strong>.</strong> Diese Vorgehensweise wird nicht empfohlen, da das vom `SqlMembershipProvider`verwendete Schema geändert wird. Diese Entscheidung kann zurückkehren, um Sie auf den Weg zu bringen. Beispiel: Was ist, wenn in einer zukünftigen Version von ASP.net ein anderes `SqlMembershipProvider` Schema verwendet wird. Microsoft kann ein Tool zum Migrieren der ASP.NET 2,0-`SqlMembershipProvider` Daten zum neuen Schema enthalten. Wenn Sie jedoch das Schema "ASP.NET 2,0 `SqlMembershipProvider`" geändert haben, ist eine solche Konvertierung möglicherweise nicht möglich.

- **Verwenden Sie ASP. Das Profil Framework von NET, das eine Profil Eigenschaft für die Homepage, die Homepage und die Signatur definiert.** ASP.NET enthält ein Profil Framework, das zum Speichern zusätzlicher Benutzer spezifischer Daten konzipiert ist. Wie das Mitgliedschafts Framework wird das Profil Framework über das Anbieter Modell erstellt. Der .NET Framework wird mit einem `SqlProfileProvider` sausgeliefert, der Profildaten in einer SQL Server Datenbank speichert. Tatsächlich verfügt unsere Datenbank bereits über die Tabelle, die vom `SqlProfileProvider` (`aspnet_Profile`) verwendet wird, da Sie hinzugefügt wurde, als wir die Anwendungsdienste wieder <a id="_msoanchor_2"> </a>in das Tutorial [*Erstellen des Mitgliedschafts Schemas in SQL Server*](creating-the-membership-schema-in-sql-server-cs.md) eingefügt haben.   
  Der Hauptvorteil des Profil-Frameworks besteht darin, dass Entwickler die Profil Eigenschaften in `Web.config` definieren können – es muss kein Code geschrieben werden, um die Profildaten in den und aus dem zugrunde liegenden Datenspeicher zu serialisieren. Kurz gesagt, ist es unglaublich einfach, einen Satz von Profil Eigenschaften zu definieren und mit Ihnen im Code zu arbeiten. Das Profilsystem bleibt jedoch bei der Versionsverwaltung sehr viel zu wünschen. Wenn Sie also eine Anwendung haben, in der Sie erwarten, dass neue benutzerspezifische Eigenschaften zu einem späteren Zeitpunkt hinzugefügt werden, oder wenn vorhandene vorhanden sind, die entfernt oder geändert werden sollen, ist das Profil Framework möglicherweise nicht das  beste Option. Darüber hinaus speichert der `SqlProfileProvider` die Profil Eigenschaften in einer streng denormalisierten Weise, sodass es nicht möglich ist, Abfragen direkt für die Profildaten auszuführen (z. b., wie viele Benutzer eine Heim Stadt von New York haben).   
  Weitere Informationen zum Profil Framework finden Sie im Abschnitt "Weitere Messwerte" am Ende dieses Tutorials.

- <strong>Fügen Sie diese drei Spalten einer neuen Tabelle in der-Datenbank hinzu, und richten Sie eine 1:1-Beziehung zwischen dieser Tabelle und</strong> <strong>`aspnet_Users`</strong>ein <strong>.</strong> Diese Vorgehensweise umfasst etwas mehr Arbeit als das Profil Framework, bietet jedoch maximale Flexibilität bei der Modellierung der zusätzlichen Benutzereigenschaften in der Datenbank. Dies ist die Option, die in diesem Tutorial verwendet wird.

Wir erstellen eine neue Tabelle namens "`UserProfiles`", um die Homepage, die Homepage und die Signatur für jeden Benutzer zu speichern. Klicken Sie im Fenster Datenbank-Explorer mit der rechten Maustaste auf den Ordner Tabellen, und wählen Sie die Option zum Erstellen einer neuen Tabelle aus. Benennen Sie die erste Spalte `UserId` und legen Sie deren Typ auf `uniqueidentifier`fest. Lassen Sie `NULL` Werte nicht zu, und markieren Sie die Spalte als Primärschlüssel. Fügen Sie als nächstes Spalten mit dem Namen: `HomeTown` vom Typ `nvarchar(50)`; hinzu. `HomepageUrl` vom Typ `nvarchar(100)`; und Signatur vom Typ `nvarchar(500)`. Jede dieser drei Spalten kann einen `NULL` Wert akzeptieren.

[Erstellen der Tabelle "userprofiles" ![](storing-additional-user-information-cs/_static/image17.png)](storing-additional-user-information-cs/_static/image16.png)

**Abbildung 6**: Erstellen der `UserProfiles` Tabelle ([Klicken Sie, um das Bild in voller Größe anzuzeigen](storing-additional-user-information-cs/_static/image18.png))

Speichern Sie die Tabelle, und benennen Sie Sie `UserProfiles`. Richten Sie schließlich eine FOREIGN KEY-Einschränkung zwischen dem `UserId` Feld der `UserProfiles` Tabelle und dem `aspnet_Users.UserId` Feld ein. Wie bei der FOREIGN KEY-Einschränkung zwischen den Tabellen "`GuestbookComments`" und "`aspnet_Users`" wird diese Einschränkung gelöscht. Da das `UserId`-Feld in `UserProfiles` der Primärschlüssel ist, wird dadurch sichergestellt, dass für jedes Benutzerkonto nicht mehr als ein Datensatz in der `UserProfiles`-Tabelle vorhanden ist. Dieser Beziehungstyp wird als eins-zu-eins bezeichnet.

Nachdem wir nun das Datenmodell erstellt haben, können wir es verwenden. In den Schritten 2 und 3 wird erläutert, wie der aktuell angemeldete Benutzer seine Homepage-, Homepage-und Signatur Informationen anzeigen und bearbeiten kann. In Schritt 4 erstellen wir die Schnittstelle für authentifizierte Benutzer, um neue Kommentare an das Gäste Buch zu senden und die vorhandenen anzuzeigen.

## <a name="step-2-displaying-the-users-home-town-homepage-and-signature"></a>Schritt 2: Anzeigen der Homepage, der Startseite und der Signatur des Benutzers

Es gibt eine Vielzahl von Möglichkeiten, den derzeit angemeldeten Benutzer zu gestatten, seine Homepage, Homepage und Signatur Informationen anzuzeigen und zu bearbeiten. Wir könnten die Benutzeroberfläche manuell mit den Steuerelementen "TextBox" und "Label" erstellen, oder Sie können eines der datenweb Steuerelemente verwenden, z. b. das DetailsView-Steuerelement. Um die Daten Bank `SELECT` und `UPDATE` Anweisungen auszuführen, können wir ADO.NET-Code in der Code Behind-Klasse unserer Seite schreiben oder alternativ einen deklarativen Ansatz mit SqlDataSource einsetzen. Im Idealfall würde unsere Anwendung eine mehrstufige Architektur enthalten, die entweder Programm gesteuert aus der Code Behind-Klasse der Seite oder deklarativ über das ObjectDataSource-Steuerelement aufgerufen werden kann.

Da sich diese tutorialreihe auf Formular Authentifizierung, Autorisierung, Benutzerkonten und Rollen konzentriert, wird eine gründliche Erörterung dieser unterschiedlichen Datenzugriffs Optionen oder Gründe für die direkte Ausführung von SQL-Anweisungen durch eine mehrstufige Architektur bevorzugt. auf der Seite ASP.net. Ich werde Ihnen die Verwendung von DetailsView und SqlDataSource – die schnellste und einfachste Option – erläutern, aber die erörterten Konzepte können sicherlich auch auf Alternative websteuer Elemente und Datenzugriffs Logik angewendet werden. Weitere Informationen zum Arbeiten mit Daten in ASP.net finden Sie in der tutorialreihe *[Arbeiten mit Daten in ASP.NET 2,0](../../data-access/index.md)* .

Öffnen Sie die Seite `AdditionalUserInfo.aspx` im Ordner `Membership`, und fügen Sie der Seite ein DetailsView-Steuerelement hinzu, indem Sie die Eigenschaft `ID` auf `UserProfile` festlegen und deren `Width`-und `Height` Eigenschaften löschen. Erweitern Sie das Smarttag der DetailsView, und binden Sie es an ein neues Datenquellen-Steuerelement. Dadurch wird der Assistent zum Konfigurieren von Datenquellen gestartet (siehe Abbildung 7). Im ersten Schritt werden Sie aufgefordert, den Daten Quellentyp anzugeben. Da wir direkt eine Verbindung mit der `SecurityTutorials`-Datenbank herstellen werden, wählen Sie das Daten Bank Symbol aus, und geben Sie die `ID` als `UserProfileDataSource`an.

[![ein neues SqlDataSource-Steuerelement mit dem Namen userprofiledatasource hinzufügen.](storing-additional-user-information-cs/_static/image20.png)](storing-additional-user-information-cs/_static/image19.png)

**Abbildung 7**: Hinzufügen eines neuen SqlDataSource-Steuer Elements namens `UserProfileDataSource` ([Klicken Sie, um das Bild in voller Größe anzuzeigen](storing-additional-user-information-cs/_static/image21.png))

Im nächsten Bildschirm werden Sie aufgefordert, die Datenbank zu verwenden. Wir haben bereits eine Verbindungs Zeichenfolge in `Web.config` für die `SecurityTutorials`-Datenbank definiert. Der Name der Verbindungs Zeichenfolge – `SecurityTutorialsConnectionString` – sollte in der Dropdown Liste enthalten sein. Wählen Sie diese Option aus, und klicken Sie auf Weiter.

[Wählen Sie in der Dropdown Liste ![securitytutorialsconnectionstring aus.](storing-additional-user-information-cs/_static/image23.png)](storing-additional-user-information-cs/_static/image22.png)

**Abbildung 8**: Auswählen von `SecurityTutorialsConnectionString` in der Dropdown Liste ([Klicken Sie, um das Bild in voller Größe anzuzeigen](storing-additional-user-information-cs/_static/image24.png))

Der nachfolgende Bildschirm fordert Sie auf, die abzufragende Tabelle und die Spalten anzugeben. Wählen Sie in der Dropdown Liste die `UserProfiles` Tabelle aus, und überprüfen Sie alle Spalten.

[![alle Spalten aus der Tabelle "userprofiles" zurück.](storing-additional-user-information-cs/_static/image26.png)](storing-additional-user-information-cs/_static/image25.png)

**Abbildung 9**: zurückziehen aller Spalten aus der `UserProfiles` Tabelle ([Klicken Sie, um das Bild in voller Größe anzuzeigen](storing-additional-user-information-cs/_static/image27.png))

Die aktuelle Abfrage in Abbildung 9 gibt *alle* Datensätze in `UserProfiles`zurück, aber wir interessieren uns nur für den aktuell angemeldeten Benutzereintrag. Um eine `WHERE`-Klausel hinzuzufügen, klicken Sie auf die Schaltfläche `WHERE`, um das Dialogfeld `WHERE` Klausel hinzufügen anzuzeigen (siehe Abbildung 10). Hier können Sie die Spalte auswählen, nach der gefiltert werden soll, den Operator und die Quelle des Filter Parameters. Wählen Sie `UserId` als Spalte und "=" als Operator aus.

Leider gibt es keine integrierte Parameter Quelle, um den `UserId` Wert des aktuell angemeldeten Benutzers zurückzugeben. Dieser Wert muss Programm gesteuert verwendet werden. Legen Sie daher die Dropdown Liste Quelle auf "None" fest. Klicken Sie auf die Schaltfläche hinzufügen, um den Parameter hinzuzufügen, und klicken Sie dann auf OK.

[![einen Filter Parameter in der Spalte "UserID" hinzufügen.](storing-additional-user-information-cs/_static/image29.png)](storing-additional-user-information-cs/_static/image28.png)

**Abbildung 10**: Hinzufügen eines Filter Parameters in der Spalte "`UserId`" ([Klicken Sie, um das Bild in voller Größe anzuzeigen](storing-additional-user-information-cs/_static/image30.png))

Nachdem Sie auf OK geklickt haben, wird der Bildschirm zurückgegeben, der in Abbildung 9 angezeigt wird. Die SQL-Abfrage unten auf dem Bildschirm sollte jedoch eine `WHERE`-Klausel enthalten. Klicken Sie auf Weiter, um zum Bildschirm "Test Abfrage" zu gelangen. Hier können Sie die Abfrage ausführen und die Ergebnisse anzeigen. Klicken Sie auf Fertigstellen, um den Assistenten abzuschließen.

Nach dem Abschließen des Assistenten zum Konfigurieren von Datenquellen erstellt Visual Studio das SqlDataSource-Steuerelement basierend auf den im Assistenten angegebenen Einstellungen. Darüber hinaus fügt Sie der DetailsView für jede Spalte, die von der `SelectCommand`von SqlDataSource zurückgegeben wird, "boundfields" manuell hinzu. Es ist nicht erforderlich, das Feld "`UserId`" in der DetailsView anzuzeigen, da der Benutzer diesen Wert nicht kennen muss. Sie können dieses Feld direkt aus dem deklarativen Markup des DetailsView-Steuer Elements entfernen oder durch Klicken auf den Link "Felder bearbeiten" aus dem Smarttag.

An diesem Punkt sollte das deklarative Markup der Seite in etwa wie folgt aussehen:

[!code-aspx[Main](storing-additional-user-information-cs/samples/sample1.aspx)]

Der `UserId`-Parameter des SqlDataSource-Steuer Elements muss Programm gesteuert auf den `UserId` des aktuell angemeldeten Benutzers festgelegt werden, bevor die Daten ausgewählt werden. Dies kann erreicht werden, indem ein Ereignishandler für das `Selecting` Ereignis von SqlDataSource erstellt und dort der folgende Code hinzugefügt wird:

[!code-csharp[Main](storing-additional-user-information-cs/samples/sample2.cs)]

Der obige Code beginnt mit dem Abrufen eines Verweises auf den aktuell angemeldeten Benutzer durch Aufrufen der `GetUser`-Methode der `Membership`-Klasse. Dadurch wird ein `MembershipUser` Objekt zurückgegeben, dessen `ProviderUserKey`-Eigenschaft die `UserId`enthält. Der `UserId` Wert wird dann dem `@UserId` Parameter von SqlDataSource zugewiesen.

> [!NOTE]
> Die `Membership.GetUser()`-Methode gibt Informationen über den aktuell angemeldeten Benutzer zurück. Wenn ein anonymer Benutzer die Seite besucht, wird der Wert `null`zurückgegeben. In einem solchen Fall führt dies zu einer `NullReferenceException` in der folgenden Codezeile, wenn versucht wird, die `ProviderUserKey`-Eigenschaft zu lesen. Natürlich müssen Sie sich keine Gedanken darüber machen, `Membership.GetUser()` Sie einen `null` Wert auf der `AdditionalUserInfo.aspx` Seite zurückgeben, da wir die URL-Autorisierung in einem vorherigen Tutorial konfiguriert haben, damit nur authentifizierte Benutzer auf die ASP.NET-Ressourcen in diesem Ordner zugreifen können. Wenn Sie auf einer Seite, auf der anonymer Zugriff zulässig ist, auf Informationen über den aktuell angemeldeten Benutzer zugreifen müssen, stellen Sie sicher, dass ein Objekt, das nicht`null MembershipUser` ist, von der `GetUser()`-Methode zurückgegeben wird, bevor Sie auf dessen Eigenschaften verweisen.

Wenn Sie die `AdditionalUserInfo.aspx` Seite über einen Browser besuchen, wird eine leere Seite angezeigt, da wir der `UserProfiles` Tabelle noch Zeilen hinzufügen müssen. In Schritt 6 wird erläutert, wie das Steuerelement "kreateuserwizard" so angepasst wird, dass der `UserProfiles` Tabelle automatisch eine neue Zeile hinzugefügt wird, wenn ein neues Benutzerkonto erstellt wird. Vorerst müssen wir jedoch manuell einen Datensatz in der Tabelle erstellen.

Navigieren Sie zum Datenbank-Explorer in Visual Studio, und erweitern Sie den Ordner Tabellen. Klicken Sie mit der rechten Maustaste auf die `aspnet_Users` Tabelle, und wählen Sie "Tabellendaten anzeigen", um die Datensätze in der Tabelle anzuzeigen. Führen Sie die gleichen Schritte für die `UserProfiles` Tabelle aus. Abbildung 11 zeigt diese Ergebnisse, wenn Sie vertikal nebeneinander angeordnet sind. In meiner Datenbank gibt es zurzeit `aspnet_Users` Datensätze für Bruce, Fred und Tito, aber keine Datensätze in der `UserProfiles` Tabelle.

[![werden die Inhalte der Tabellen "aspnet_Users" und "userprofiles" angezeigt.](storing-additional-user-information-cs/_static/image32.png)](storing-additional-user-information-cs/_static/image31.png)

**Abbildung 11**: der Inhalt der Tabellen "`aspnet_Users`" und "`UserProfiles`" wird angezeigt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](storing-additional-user-information-cs/_static/image33.png))

Fügen Sie der `UserProfiles` Tabelle einen neuen Datensatz hinzu, indem Sie die Werte für die Felder `HomeTown`, `HomepageUrl`und `Signature` manuell eingeben. Die einfachste Möglichkeit, einen gültigen `UserId` Wert im neuen `UserProfiles` Datensatz zu erhalten, besteht darin, das Feld `UserId` aus einem bestimmten Benutzerkonto in der `aspnet_Users` Tabelle auszuwählen und es in `UserId` zu kopieren und in `UserProfiles`Feld einzufügen. In Abbildung 12 wird die `UserProfiles` Tabelle angezeigt, nachdem für Bruce ein neuer Datensatz hinzugefügt wurde.

[der Benutzerprofile für Bruce wurde ![ein Datensatz hinzugefügt.](storing-additional-user-information-cs/_static/image35.png)](storing-additional-user-information-cs/_static/image34.png)

**Abbildung 12**: ein Datensatz wurde `UserProfiles` für Bruce hinzugefügt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](storing-additional-user-information-cs/_static/image36.png))

Kehren Sie zur `AdditionalUserInfo.aspx` Seite zurück, die als "Bruce" angemeldet ist. Siehe Abbildung 13 zeigt, dass die Einstellungen von Bruce angezeigt werden.

[![dem aktuell besuchten Benutzer werden seine Einstellungen angezeigt.](storing-additional-user-information-cs/_static/image38.png)](storing-additional-user-information-cs/_static/image37.png)

**Abbildung 13**: der aktuell besuchte Benutzer zeigt seine Einstellungen an ([Klicken Sie, um das Bild in voller Größe anzuzeigen](storing-additional-user-information-cs/_static/image39.png))

> [!NOTE]
> Fügen Sie der `UserProfiles` Tabelle für jeden Mitgliedschafts Benutzer manuell Datensätze hinzu. In Schritt 6 wird erläutert, wie das Steuerelement "kreateuserwizard" so angepasst wird, dass der `UserProfiles` Tabelle automatisch eine neue Zeile hinzugefügt wird, wenn ein neues Benutzerkonto erstellt wird.

## <a name="step-3-allowing-the-user-to-edit-his-home-town-homepage-and-signature"></a>Schritt 3: zulassen, dass der Benutzer seine Homepage, Homepage und Signatur bearbeitet

An diesem Punkt kann der aktuell angemeldete Benutzer seine Homepage, die Homepage und die Signatur Einstellung anzeigen, Sie aber noch nicht ändern. Wir aktualisieren das DetailsView-Steuerelement, damit die Daten bearbeitet werden können.

Als erstes müssen wir eine `UpdateCommand` für SqlDataSource hinzufügen und dabei die auszuführende `UPDATE` Anweisung und die entsprechenden Parameter angeben. Wählen Sie die SqlDataSource aus, und klicken Sie im Eigenschaftenfenster auf die Auslassungs Punkte neben der UpdateQuery-Eigenschaft, um das Dialogfeld Befehls-und Parameter-Editor aufzurufende. Geben Sie die folgende `UPDATE`-Anweisung in das Textfeld ein:

[!code-sql[Main](storing-additional-user-information-cs/samples/sample3.sql)]

Klicken Sie anschließend auf die Schaltfläche "Parameter aktualisieren", die für jeden Parameter in der `UPDATE` Anweisung einen Parameter in der `UpdateParameters`-Sammlung des SqlDataSource-Steuer Elements erstellt. Lassen Sie die Quelle für alle Parameter auf None festgelegt, und klicken Sie auf die Schaltfläche OK, um das Dialogfeld abzuschließen.

[![den UpdateCommand und UpdateParameters von SqlDataSource angeben](storing-additional-user-information-cs/_static/image41.png)](storing-additional-user-information-cs/_static/image40.png)

**Abbildung 14**: Angeben der `UpdateCommand` und `UpdateParameters` von SqlDataSource ([Klicken Sie, um das Bild in voller Größe anzuzeigen](storing-additional-user-information-cs/_static/image42.png))

Aufgrund der Ergänzungen, die wir am SqlDataSource-Steuerelement vorgenommen haben, kann das DetailsView-Steuerelement jetzt die Bearbeitung unterstützen. Aktivieren Sie das Kontrollkästchen "Bearbeitung aktivieren" aus dem Smarttag der DetailsView. Dadurch wird der `Fields`-Auflistung des Steuer Elements ein CommandField hinzugefügt, dessen `ShowEditButton`-Eigenschaft auf true festgelegt ist. Dadurch wird eine Bearbeitungs Schaltfläche gerendert, wenn die DetailsView im schreibgeschützten Modus angezeigt wird und die Schaltflächen aktualisieren und Abbrechen, wenn Sie im Bearbeitungsmodus angezeigt werden. Anstatt dass der Benutzer auf "Bearbeiten" klicken muss, kann die DetailsView in einem "immer bearbeitbaren" Zustand dargestellt werden, indem die [`DefaultMode`-Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.detailsview.defaultmode.aspx) des DetailsView-Steuer Elements auf "`Edit`" festgelegt wird.

Mit diesen Änderungen sollte das deklarative Markup Ihres DetailsView-Steuer Elements in etwa wie folgt aussehen:

[!code-aspx[Main](storing-additional-user-information-cs/samples/sample4.aspx)]

Beachten Sie das Hinzufügen von CommandField und der `DefaultMode`-Eigenschaft.

Testen Sie diese Seite in einem Browser. Beim Besuch eines Benutzers, der über einen entsprechenden Datensatz in `UserProfiles`verfügt, werden die Einstellungen des Benutzers in einer editierbaren Schnittstelle angezeigt.

[![DetailsView eine bearbeitbare Schnittstelle rendert](storing-additional-user-information-cs/_static/image44.png)](storing-additional-user-information-cs/_static/image43.png)

**Abbildung 15**: die DetailsView rendert eine bearbeitbare Schnittstelle ([Klicken Sie, um das Bild in voller Größe anzuzeigen](storing-additional-user-information-cs/_static/image45.png))

Ändern Sie die Werte, und klicken Sie auf die Schaltfläche Aktualisieren. Es wird angezeigt, als ob nichts passiert. Es gibt ein Postback, und die Werte werden in der Datenbank gespeichert, aber es gibt kein visuelles Feedback, dass der Speicher aufgetreten ist.

Um dies zu beheben, kehren Sie zu Visual Studio zurück, und fügen Sie ein Label-Steuerelement oberhalb der DetailsView hinzu. Legen Sie den `ID` auf `SettingsUpdatedMessage`, seine `Text`-Eigenschaft auf "Ihre Einstellungen wurden aktualisiert" und dessen `Visible` und `EnableViewState` Eigenschaften auf `false`fest.

[!code-aspx[Main](storing-additional-user-information-cs/samples/sample5.aspx)]

Wir müssen die `SettingsUpdatedMessage` Bezeichnung immer dann anzeigen, wenn die DetailsView aktualisiert wird. Erstellen Sie hierzu einen Ereignishandler für das `ItemUpdated`-Ereignis der DetailsView, und fügen Sie den folgenden Code hinzu:

[!code-csharp[Main](storing-additional-user-information-cs/samples/sample6.cs)]

Kehren Sie über einen Browser zur Seite `AdditionalUserInfo.aspx` zurück, und aktualisieren Sie die Daten. Dieses Mal wird eine hilfreiche Statusmeldung angezeigt.

[![eine kurze Meldung angezeigt wird, wenn die Einstellungen aktualisiert werden.](storing-additional-user-information-cs/_static/image47.png)](storing-additional-user-information-cs/_static/image46.png)

**Abbildung 16**: eine kurze Meldung wird angezeigt, wenn die Einstellungen aktualisiert werden ([Klicken Sie, um das Bild in voller Größe anzuzeigen](storing-additional-user-information-cs/_static/image48.png))

> [!NOTE]
> Die Bearbeitungs Schnittstelle des DetailsView-Steuer Elements lässt sich sehr viel wünschen. Es verwendet Textfelder mit Standardgröße, das Signatur Feld sollte jedoch wahrscheinlich ein mehrzeilige Textfeld sein. Ein RegularExpressionValidator sollte verwendet werden, um sicherzustellen, dass die URL der Startseite, sofern eingegeben, mit "http://" oder "https://" beginnt. Da für das DetailsView-Steuerelement die `DefaultMode`-Eigenschaft auf `Edit`festgelegt ist, führt die Schaltfläche Abbrechen keine Aktion aus. Er sollte entweder entfernt werden, oder der Benutzer wird, wenn er darauf geklickt hat, an eine andere Seite umgeleitet (z. b. `~/Default.aspx`). Ich lasse diese Verbesserungen als Übung für den Reader aus.

### <a name="adding-a-link-to-theadditionaluserinfoaspxpage-in-the-master-page"></a>Hinzufügen eines Links zur Seite "`AdditionalUserInfo.aspx`" in der Master Seite

Derzeit stellt die Website keine Verknüpfungen zur Seite `AdditionalUserInfo.aspx` bereit. Die einzige Möglichkeit, Sie zu erreichen, besteht darin, die URL der Seite direkt in die Adressleiste des Browsers einzugeben. Fügen Sie auf der Seite "`Site.master` Master" einen Link zu dieser Seite hinzu.

Beachten Sie, dass die Master Seite ein LoginView-websteuer Element in der `LoginContent` contentplachalter enthält, das unterschiedliche Markup für authentifizierte und anonyme Besucher anzeigt. Aktualisieren Sie das `LoggedInTemplate` des LoginView-Steuer Elements, um einen Link zur `AdditionalUserInfo.aspx` Seite einzuschließen. Nachdem Sie diese Änderungen vorgenommen haben, sollte das deklarative Markup des LoginView-Steuer Elements in etwa wie folgt aussehen:

[!code-aspx[Main](storing-additional-user-information-cs/samples/sample7.aspx)]

Beachten Sie das Hinzufügen des `lnkUpdateSettings` Hyperlink-Steuer Elements zum `LoggedInTemplate`. Mit diesem Link können authentifizierte Benutzer schnell zu der Seite springen, um Ihre Home-, Homepage-und Signatur Einstellungen anzuzeigen und zu ändern.

## <a name="step-4-adding-new-guestbook-comments"></a>Schritt 4: Hinzufügen neuer Gäste Buch Kommentare

Auf der `Guestbook.aspx` Seite können authentifizierte Benutzer das Gäste Buch anzeigen und einen Kommentar hinterlassen. Beginnen wir mit dem Erstellen der-Schnittstelle, um neue Gäste Buch Kommentare hinzuzufügen.

Öffnen Sie die Seite "`Guestbook.aspx`" in Visual Studio, und erstellen Sie eine Benutzeroberfläche, die aus zwei TextBox-Steuerelementen besteht, eine für den Betreff des neuen Kommentars und eine für den Text. Legen Sie die `ID`-Eigenschaft des ersten TextBox-Steuer Elements auf `Subject` und dessen `Columns`-Eigenschaft auf 40 fest. Legen Sie den `ID` für das auf `Body`, dessen `TextMode` auf `MultiLine`und seine `Width`-und `Rows`-Eigenschaften auf "95%" bzw. 8 fest. Um die Benutzeroberfläche abzuschließen, fügen Sie ein Schaltflächen-websteuer Element mit dem Namen `PostCommentButton` hinzu, und legen Sie dessen `Text`-Eigenschaft auf "Post your comment"

Da für jeden Gäste Buch Kommentar ein Betreff und Text erforderlich ist, fügen Sie für jedes der Textfelder ein "Requirements dfieldvalidator"-Element hinzu. Legen Sie die `ValidationGroup`-Eigenschaft dieser Steuerelemente auf "entercomment" fest. Ebenso legen Sie die `ValidationGroup`-Eigenschaft des `PostCommentButton`-Steuer Elements auf "entercomment" fest. Weitere Informationen zu ASP. Die Validierungs Steuerelemente von NET, Auschecken der [Formular Validierung in ASP.net](http://www.4guysfromrolla.com/webtech/090200-1.shtml), über [Lagern der Validierungs Steuerelemente in ASP.NET 2,0](http://aspnet.4guysfromrolla.com/articles/112305-1.aspx)und des [Tutorials zur Validierung von Server Steuerelementen](http://www.w3schools.com/aspnet/aspnet_refvalidationcontrols.asp) auf [W3Schools](http://www.w3schools.com/).

Nach dem Erstellen der Benutzeroberfläche sollte das deklarative Markup der Seite in etwa wie folgt aussehen:

[!code-aspx[Main](storing-additional-user-information-cs/samples/sample8.aspx)]

Wenn die Benutzeroberfläche fertig ist, besteht die nächste Aufgabe darin, einen neuen Datensatz in die `GuestbookComments` Tabelle einzufügen, wenn auf die `PostCommentButton` geklickt wird. Dies kann auf verschiedene Weise erreicht werden: Wir können ADO.NET-Code in den `Click`-Ereignishandler der Schaltfläche schreiben. Wir können der Seite ein SqlDataSource-Steuerelement hinzufügen, dessen `InsertCommand`konfigurieren und dann seine `Insert`-Methode aus dem `Click`-Ereignishandler aufzurufen. oder wir könnten eine mittlere Ebene erstellen, die für das Einfügen neuer Gäste Buch Kommentare verantwortlich ist, und diese Funktionalität aus dem `Click`-Ereignishandler aufrufen. Da wir uns in Schritt 3 mit der Verwendung von SqlDataSource beschäftigt haben, verwenden wir hier ADO.NET-Code.

> [!NOTE]
> Die ADO.NET-Klassen, die für den programmgesteuerten Zugriff auf Daten aus einer Microsoft SQL Server Datenbank verwendet werden, befinden sich im `System.Data.SqlClient`-Namespace. Möglicherweise müssen Sie diesen Namespace in die Code Behind-Klasse Ihrer Seite importieren (z. b. `using System.Data.SqlClient;`).

Erstellen Sie einen Ereignishandler für das `Click` Ereignis des `PostCommentButton`, und fügen Sie den folgenden Code hinzu:

[!code-csharp[Main](storing-additional-user-information-cs/samples/sample9.cs)]

Der `Click` Ereignishandler prüft zunächst, ob die vom Benutzer bereitgestellten Daten gültig sind. Wenn dies nicht der Fall ist, wird der Ereignishandler beendet, bevor ein Datensatz eingefügt wird. Wenn die angegebenen Daten gültig sind, wird der `UserId` Wert des aktuell angemeldeten Benutzers abgerufen und in der `currentUserId` lokalen Variablen gespeichert. Dieser Wert ist erforderlich, da beim Einfügen eines Datensatzes in `GuestbookComments`ein `UserId` Wert angegeben werden muss.

Danach wird die Verbindungs Zeichenfolge für die `SecurityTutorials`-Datenbank aus `Web.config` abgerufen, und die `INSERT` SQL-Anweisung wird angegeben. Anschließend wird ein `SqlConnection` Objekt erstellt und geöffnet. Als nächstes wird ein `SqlCommand` Objekt erstellt, und die Werte für die Parameter, die in der `INSERT` Abfrage verwendet werden, werden zugewiesen. Die `INSERT`-Anweisung wird dann ausgeführt, und die Verbindung wurde geschlossen. Am Ende des Ereignis Handlers werden die `Text` Eigenschaften der `Subject` und `Body` Textfelder gelöscht, sodass die Werte des Benutzers nicht über das Postback hinweg beibehalten werden.

Testen Sie diese Seite in einem Browser. Da sich diese Seite im `Membership` Ordner befindet, ist Sie für anonyme Besucher nicht zugänglich. Daher müssen Sie sich zuerst anmelden (sofern Sie dies noch nicht getan haben). Geben Sie in die Textfelder `Subject` und `Body` einen Wert ein, und klicken Sie auf die Schaltfläche `PostCommentButton`. Dadurch wird `GuestbookComments`ein neuer Datensatz hinzugefügt. Beim Postback werden der von Ihnen angegebene Betreff und der Text aus den Textfeldern gelöscht.

Nachdem Sie auf die `PostCommentButton` Schaltfläche geklickt haben, ist kein visuelles Feedback vorhanden, dass der Kommentar dem Gäste Buch hinzugefügt wurde. Wir müssen diese Seite dennoch aktualisieren, damit Sie die vorhandenen Gäste Buch Kommentare anzeigt, die wir in Schritt 5 ausführen werden. Nachdem wir dies erreicht haben, wird der soeben hinzugefügte Kommentar in der Liste der Kommentare angezeigt, der entsprechendes visuelles Feedback bereitstellt. Vergewissern Sie sich vorerst, dass Ihr Gäste Buch Kommentar gespeichert wurde, indem Sie den Inhalt der `GuestbookComments` Tabelle untersuchen.

In Abbildung 17 wird der Inhalt der `GuestbookComments` Tabelle angezeigt, nachdem zwei Kommentare verbleiben.

[![Sie die Gäste Buch Kommentare in der Tabelle guestbookcomments sehen.](storing-additional-user-information-cs/_static/image50.png)](storing-additional-user-information-cs/_static/image49.png)

**Abbildung 17**: die Gäste Buch Kommentare werden in der `GuestbookComments` Tabelle angezeigt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](storing-additional-user-information-cs/_static/image51.png))

> [!NOTE]
> Wenn ein Benutzer versucht, einen Gäste Buch Kommentar einzufügen, der potenziell gefährliche Markup – wie z. b. html – enthält, löst ASP.net eine `HttpRequestValidationException`aus. Weitere Informationen zu dieser Ausnahme, warum Sie ausgelöst wird und wie Sie zulassen, dass Benutzer potenziell gefährliche Werte einreichen, finden Sie im [Whitepaper zur Anforderungs Validierung](../../../../whitepapers/request-validation.md).

## <a name="step-5-listing-the-existing-guestbook-comments"></a>Schritt 5: Auflisten der vorhandenen Gäste Buch Kommentare

Zusätzlich zum hinterlassen von Kommentaren sollte ein Benutzer, der die `Guestbook.aspx` Seite besucht, auch die vorhandenen Kommentare des Gästebuchs anzeigen können. Fügen Sie zu diesem Zweck dem unteren Rand der Seite ein ListView-Steuerelement mit dem Namen `CommentList` hinzu.

> [!NOTE]
> Das ListView-Steuerelement ist neu in ASP.net, Version 3,5. Es wurde entwickelt, um eine Liste von Elementen in einem sehr anpassbaren und flexiblen Layout anzuzeigen, bietet aber trotzdem integrierte Bearbeitungs-, Einfüge-, Lösch-, Paging-und Sortierungs Funktionen wie die GridView. Wenn Sie ASP.NET 2,0 verwenden, müssen Sie stattdessen das DataList-Steuerelement oder das Repeater-Steuerelement verwenden. Weitere Informationen zur Verwendung von ListView finden Sie im Blogeintrag von [Scott Guthrie](https://weblogs.asp.net/scottgu/), [dem ASP: ListView-Steuer](https://weblogs.asp.net/scottgu/archive/2007/08/10/the-asp-listview-control-part-1-building-a-product-listing-page-with-clean-css-ui.aspx)Element und meinem Artikel zum [Anzeigen von Daten mit dem ListView-Steuer](http://aspnet.4guysfromrolla.com/articles/122607-1.aspx)Element.

Öffnen Sie das Smarttag von ListView, und binden Sie das Steuerelement aus der Dropdown Liste Datenquelle auswählen an eine neue Datenquelle. Wie in Schritt 2 gezeigt, wird der Assistent zum Konfigurieren von Datenquellen gestartet. Wählen Sie das Daten Bank Symbol aus, benennen Sie das resultierende SqlDataSource-`CommentsDataSource`, und klicken Sie auf OK. Wählen Sie als nächstes die `SecurityTutorialsConnectionString` Verbindungs Zeichenfolge aus der Dropdown Liste aus, und klicken Sie auf Weiter.

An dieser Stelle in Schritt 2 haben wir die abzufragende Daten angegeben, indem wir die `UserProfiles` Tabelle aus der Dropdown Liste auswählen und die Spalten auswählen, die zurückgegeben werden sollen (siehe Abbildung 9). Diesmal möchten wir jedoch eine SQL-Anweisung erstellen, die nicht nur die Datensätze aus `GuestbookComments`abruft, sondern auch die Homepage, die Homepage, die Signatur und den Benutzernamen des kommentners. Wählen Sie daher die Options Schaltfläche "benutzerdefinierte SQL-Anweisung oder gespeicherte Prozedur angeben" aus, und klicken Sie auf Weiter.

Dadurch wird der Bildschirm "benutzerdefinierte Anweisungen oder gespeicherte Prozeduren definieren" angezeigt. Klicken Sie auf die Schaltfläche Abfrage-Generator, um die Abfrage grafisch zu erstellen. Der Abfrage-Generator beginnt mit der Aufforderung, die Tabellen anzugeben, von denen Abfragen ausgeführt werden sollen. Wählen Sie die Tabellen `GuestbookComments`, `UserProfiles`und `aspnet_Users` aus, und klicken Sie auf OK. Dadurch werden alle drei Tabellen zur Entwurfs Oberfläche hinzugefügt. Da es unter den Tabellen `GuestbookComments`, `UserProfiles`und `aspnet_Users` Foreign Key-Einschränkungen gibt, `JOIN` die Abfrage-Generator diese Tabellen automatisch.

Sie müssen nur noch die Spalten angeben, die zurückgegeben werden sollen. Wählen Sie in der `GuestbookComments` Tabelle die Spalten `Subject`, `Body`und `CommentDate` aus. Gibt die Spalten `HomeTown`, `HomepageUrl`und `Signature` aus der `UserProfiles` Tabelle zurück. und geben `UserName` aus `aspnet_Users`zurück. Fügen Sie dem Ende der `SELECT` Abfrage außerdem "`ORDER BY CommentDate DESC`" hinzu, damit die letzten Beiträge zuerst zurückgegeben werden. Nachdem Sie diese Auswahl getroffen haben, sollte die Abfrage-Generator-Schnittstelle ähnlich wie der Screenshot in Abbildung 18 aussehen.

[![die erstellte Abfrage mit den Tabellen guestbookcomments, userprofiles und aspnet_Users verbunden ist.](storing-additional-user-information-cs/_static/image53.png)](storing-additional-user-information-cs/_static/image52.png)

**Abbildung 18**: die erstellte Abfrage `JOIN` s die Tabellen "`GuestbookComments`", "`UserProfiles`" und "`aspnet_Users`" ([Klicken Sie, um das Bild in voller Größe anzuzeigen](storing-additional-user-information-cs/_static/image54.png))

Klicken Sie auf OK, um das Fenster Abfrage-Generator zu schließen und zum Bildschirm "benutzerdefinierte Anweisungen oder gespeicherte Prozeduren definieren" zurückzukehren. Klicken Sie auf Weiter, um zum Bildschirm "Test Abfrage" zu gelangen. dort können Sie die Abfrageergebnisse anzeigen, indem Sie auf die Schaltfläche Test Abfrage klicken. Wenn Sie fertig sind, klicken Sie auf Fertigstellen, um den Assistenten zum Konfigurieren von Datenquellen abzuschließen.

Wenn wir den Assistenten zum Konfigurieren von Datenquellen in Schritt 2 abgeschlossen haben, wurde die `Fields` Auflistung des zugeordneten DetailsView-Steuer Elements aktualisiert und enthält nun ein BoundField für jede Spalte, die vom `SelectCommand`zurückgegeben wurde. Die ListView bleibt jedoch unverändert. Wir müssen weiterhin das Layout definieren. Das ListView-Layout kann manuell über sein deklaratives Markup oder die Option "ListView konfigurieren" im Smarttags erstellt werden. Ich ziehe es in der Regel vor, das Markup per Hand zu definieren, aber ich verwende eine beliebige Methode für Sie.

Am Ende habe ich die folgenden `LayoutTemplate`, `ItemTemplate`und `ItemSeparatorTemplate` für das ListView-Steuerelement verwendet:

[!code-aspx[Main](storing-additional-user-information-cs/samples/sample10.aspx)]

Der `LayoutTemplate` definiert das vom Steuerelement ausgegebene Markup, während der `ItemTemplate` jedes von SqlDataSource zurückgegebene Element rendert. Das resultierende Markup des `ItemTemplate`wird in das `itemPlaceholder` Steuerelement des `LayoutTemplate`eingefügt. Zusätzlich zum `itemPlaceholder`enthält die `LayoutTemplate` ein DataPager-Steuerelement, das die ListView so einschränkt, dass nur 10 Gäste Buch Kommentare pro Seite (Standard) angezeigt werden, und es wird eine pagingschnittstelle gerendert.

Meine `ItemTemplate` zeigt den Betreff jedes Gäste Buch Kommentars in einem `<h4>`-Element mit dem Text an, der unterhalb des Subjekts liegt. Beachten Sie, dass die für die Anzeige des Texts verwendete Syntax die von der `Eval("Body")` Datenbindung-Anweisung zurückgegebenen Daten annimmt, Sie in eine Zeichenfolge konvertiert und Zeilenumbrüche durch das `<br />`-Element ersetzt. Diese Konvertierung wird benötigt, um die beim Einreichen des Kommentars eingegebenen Zeilenumbrüche anzuzeigen, da Leerräume von HTML ignoriert werden. Die Signatur des Benutzers wird unter dem Text in kursiv Schrift angezeigt, gefolgt von der Homepage des Benutzers, einem Link zu seiner Homepage, dem Datum und der Uhrzeit, zu der der Kommentar erstellt wurde, und dem Benutzernamen der Person, die den Kommentar hinterlassen hat.

Nehmen Sie sich einen Moment Zeit, um die Seite in einem Browser anzuzeigen. Hier werden die Kommentare angezeigt, die Sie dem-Gästebuch in Schritt 5 hinzugefügt haben.

[!["guest guest. aspx" zeigt nun die Kommentare des Gästebuchs an](storing-additional-user-information-cs/_static/image56.png)](storing-additional-user-information-cs/_static/image55.png)

**Abbildung 19**: `Guestbook.aspx` zeigt nun die Kommentare des Gästebuchs[an (Klicken Sie, um das Bild in voller Größe anzuzeigen](storing-additional-user-information-cs/_static/image57.png))

Versuchen Sie, dem Gäste Buch einen neuen Kommentar hinzuzufügen. Wenn Sie auf die `PostCommentButton` Schaltfläche klicken, wird die Seite zurückgesendet, und der Kommentar wird der Datenbank hinzugefügt, aber das ListView-Steuerelement wird nicht aktualisiert, um den neuen Kommentar anzuzeigen. Dies kann mit folgenden Aktionen korrigiert werden:

- Aktualisieren des `Click` Ereignis Handlers der `PostCommentButton` Schaltfläche, damit die `DataBind()` Methode des ListView-Steuer Elements aufgerufen wird, nachdem der neue Kommentar in die Datenbank eingefügt wurde, oder
- Festlegen der `EnableViewState`-Eigenschaft des ListView-Steuer Elements auf `false`. Dieser Ansatz funktioniert, da der Ansichts Zustand des Steuer Elements deaktiviert wird, muss er bei jedem Postback erneut an die zugrunde liegenden Daten gebunden werden.

Die in diesem Tutorial herunterladbare Tutorial-Website veranschaulicht beide Techniken. Die `EnableViewState`-Eigenschaft des ListView-Steuer Elements, die `false` werden soll, und der Code, der zum programmgesteuerten erneuten Binden der Daten an die ListView benötigt wird, ist im `Click`-Ereignishandler vorhanden, aber auskommentiert.

> [!NOTE]
> Die Seite "`AdditionalUserInfo.aspx`" ermöglicht dem Benutzer die Anzeige und Bearbeitung Ihrer Homepage-, Homepage-und Signatur Einstellungen. Es kann sinnvoll sein, `AdditionalUserInfo.aspx` zu aktualisieren, um die Gäste Buch Kommentare des angemeldeten Benutzers anzuzeigen. Das heißt, zusätzlich zur Untersuchung und Bearbeitung Ihrer Informationen kann ein Benutzer die `AdditionalUserInfo.aspx` Seite aufrufen, um zu sehen, welche Gäste Buch Kommentare Sie in der Vergangenheit gemacht haben. Ich lasse dies als Übung für den interessierten Reader aus.

## <a name="step-6-customizing-the-createuserwizard-control-to-include-an-interface-for-the-home-town-homepage-and-signature"></a>Schritt 6: Anpassen des Steuer Elements "kreateuserwizard", um eine Schnittstelle für die Homepage, die Homepage und die Signatur einzuschließen

Die von der `Guestbook.aspx` Seite verwendete `SELECT` Abfrage verwendet einen `INNER JOIN`, um die verknüpften Datensätze zwischen den Tabellen `GuestbookComments`, `UserProfiles`und `aspnet_Users` zu kombinieren. Wenn ein Benutzer, der keinen Datensatz in `UserProfiles` erstellt, einen Gäste Buch Kommentar erstellt, wird der Kommentar nicht in der ListView angezeigt, da der `INNER JOIN` nur `GuestbookComments` Datensätze zurückgibt, wenn in `UserProfiles` und `aspnet_Users`übereinstimmende Datensätze vorhanden sind. Und wie in Schritt 3 gezeigt, verfügt ein Benutzer nicht über einen Datensatz in `UserProfiles` er seine Einstellungen auf der Seite `AdditionalUserInfo.aspx` nicht anzeigen oder bearbeiten kann.

Aufgrund unserer Entwurfsentscheidungen ist es natürlich wichtig, dass jedes Benutzerkonto im Mitgliedschaftssystem über einen übereinstimmenden Datensatz in der `UserProfiles` Tabelle verfügt. Wir möchten, dass ein entsprechender Datensatz hinzugefügt wird, der `UserProfiles` wird, wenn ein neues Mitgliedschafts Benutzerkonto über den "feeuserwizard" erstellt wird.

Wie im Tutorial [*Erstellen von Benutzerkonten*](creating-user-accounts-cs.md) erläutert, löst das Steuerelement "kreateuserwizard" das [`CreatedUser`-Ereignis](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.createduser.aspx)aus, nachdem das neue Mitgliedschafts Benutzerkonto erstellt wurde. Wir können einen Ereignishandler für dieses Ereignis erstellen, die UserID für den soeben erstellten Benutzer erhalten und dann einen Datensatz in die `UserProfiles` Tabelle mit Standardwerten für die Spalten `HomeTown`, `HomepageUrl`und `Signature` einfügen. Darüber hinaus ist es möglich, den Benutzer zur Eingabe dieser Werte aufzufordern, indem er die Schnittstelle des Steuer Elements "kreateuserwizard" anpasst, um zusätzliche Textfelder einzubeziehen.

Sehen wir uns zunächst an, wie der `UserProfiles` Tabelle im `CreatedUser`-Ereignishandler eine neue Zeile mit Standardwerten hinzugefügt wird. Danach erfahren Sie, wie Sie die Benutzeroberfläche des Steuer Elements "anateuserwizard" so anpassen, dass es zusätzliche Formularfelder zum Erfassen der Homepage, der Homepage und der Signatur des neuen Benutzers enthält.

### <a name="adding-a-default-row-touserprofiles"></a>Hinzufügen einer Standardzeile zu`UserProfiles`

Im Tutorial [*Erstellen von Benutzerkonten*](creating-user-accounts-cs.md) haben wir der Seite "`CreatingUserAccounts.aspx`" im Ordner "`Membership`" ein "kreateuserwizard"-Steuerelement hinzugefügt. Damit das Steuerelement "kreateuserwizard" `UserProfiles` Tabelle bei der Erstellung des Benutzerkontos einen Datensatz hinzufügen kann, müssen wir die Funktionalität des Steuer Elements "kreateuserwizard" aktualisieren. Anstatt diese Änderungen auf der Seite "`CreatingUserAccounts.aspx`" vorzunehmen, fügen wir `EnhancedCreateUserWizard.aspx` Seite nun ein neues Steuerelement "kreateuserwizard" hinzu und nehmen dort die Änderungen für dieses Tutorial vor.

Öffnen Sie in Visual Studio die Seite `EnhancedCreateUserWizard.aspx`, und ziehen Sie das Steuerelement "kreateuserwizard" aus der Toolbox auf die Seite. Legen Sie die `ID`-Eigenschaft des-Steuer Elements für das-Steuerelement auf `NewUserWizard`fest. Wie im <a id="_msoanchor_5"> </a>Tutorial [*Erstellen von Benutzerkonten*](creating-user-accounts-cs.md) erläutert, wird der Besucher von der Standardbenutzer Oberfläche von "feateuserwizard" aufgefordert, die erforderlichen Informationen zu erhalten. Nachdem diese Informationen angegeben wurden, erstellt das Steuerelement intern ein neues Benutzerkonto im Mitgliedschafts Framework, ohne dass eine einzige Codezeile geschrieben werden muss.

Das Steuerelement "kreateuserwizard" löst während des Workflows eine Reihe von Ereignissen aus. Nachdem ein Besucher die Anforderungs Informationen bereitgestellt und das Formular übermittelt hat, löst das Steuerelement "kreateuserwizard" zunächst sein [`CreatingUser` Ereignis](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.creatinguser.aspx)aus. Wenn während des Erstellungs Vorgangs ein Problem auftritt, wird das [`CreateUserError` Ereignis](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.createusererror.aspx) ausgelöst. Wenn der Benutzer jedoch erfolgreich erstellt wurde, wird das [`CreatedUser`-Ereignis](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.createduser.aspx) ausgelöst. <a id="_msoanchor_6"> </a>Im Tutorial [*Erstellen von Benutzerkonten*](creating-user-accounts-cs.md) haben wir einen Ereignishandler für das `CreatingUser`-Ereignis erstellt, um sicherzustellen, dass der angegebene Benutzername keine führenden oder nachfolgenden Leerzeichen enthielt und dass der Benutzername nicht an einer beliebigen Stelle im Kennwort angezeigt wird.

Um der `UserProfiles` Tabelle für den soeben erstellten Benutzer eine Zeile hinzuzufügen, müssen wir einen Ereignishandler für das `CreatedUser`-Ereignis erstellen. Zum Zeitpunkt der Auslösung des `CreatedUser` Ereignisses wurde das Benutzerkonto bereits im Mitgliedschafts Framework erstellt, sodass wir den Wert "UserID" des Kontos abrufen können.

Erstellen Sie einen Ereignishandler für das `CreatedUser` Ereignis des `NewUserWizard`, und fügen Sie den folgenden Code hinzu:

[!code-csharp[Main](storing-additional-user-information-cs/samples/sample11.cs)]

Der obige Code ist durch Abrufen der UserID des soeben hinzugefügten Benutzerkontos. Dies wird erreicht, indem die `Membership.GetUser(username)`-Methode verwendet wird, um Informationen zu einem bestimmten Benutzer zurückzugeben, und anschließend die `ProviderUserKey`-Eigenschaft zum Abrufen Ihrer UserID zu verwenden. Der Benutzername, der vom Benutzer im Steuerelement "kreateuserwizard" eingegeben wurde, ist über die [`UserName`-Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.username.aspx)verfügbar.

Als nächstes wird die Verbindungs Zeichenfolge aus `Web.config` abgerufen, und die `INSERT`-Anweisung wird angegeben. Die erforderlichen ADO.NET-Objekte werden instanziiert und der Befehl ausgeführt. Der Code weist dem `@HomeTown`-, `@HomepageUrl`-und `@Signature`-Parameter eine [`DBNull`](https://msdn.microsoft.com/library/system.dbnull.aspx) Instanz zu. Dies hat Auswirkungen auf das Einfügen von Daten Bank `NULL` Werten für die `HomeTown`-, `HomepageUrl`-und `Signature`-Felder.

Besuchen Sie die Seite `EnhancedCreateUserWizard.aspx` über einen Browser, und erstellen Sie ein neues Benutzerkonto. Kehren Sie anschließend zu Visual Studio zurück, und untersuchen Sie den Inhalt der Tabellen "`aspnet_Users`" und "`UserProfiles`" (wie in Abbildung 12 dargestellt). Das neue Benutzerkonto sollte in `aspnet_Users` und eine entsprechende `UserProfiles` Zeile (mit `NULL` Werte für `HomeTown`, `HomepageUrl`und `Signature`) angezeigt werden.

[![ein neues Benutzerkonto und ein Benutzerprofil-Datensatz hinzugefügt wurden.](storing-additional-user-information-cs/_static/image59.png)](storing-additional-user-information-cs/_static/image58.png)

**Abbildung 20**: Es wurde ein neues Benutzerkonto und `UserProfiles` Datensatz hinzugefügt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](storing-additional-user-information-cs/_static/image60.png))

Nachdem der Besucher seine neuen Kontoinformationen eingegeben und auf die Schaltfläche "Benutzer erstellen" geklickt hat, wird das Benutzerkonto erstellt und der `UserProfiles` Tabelle eine Zeile hinzugefügt. Der Assistent zum Anzeigen der `CompleteWizardStep`zeigt dann die zugehörige an, in der eine Erfolgsmeldung und eine Schaltfläche "weiter" angezeigt werden. Wenn Sie auf die Schaltfläche "weiter" klicken, wird ein Postback ausgelöst, aber es wird keine Aktion ausgeführt, sodass der Benutzer auf der `EnhancedCreateUserWizard.aspx` Seite hängen bleibt

Wir können eine URL angeben, an die der Benutzer gesendet wird, wenn auf die Schaltfläche "weiter" mit der`ContinueDestinationPageUrl`- [Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.continuedestinationpageurl.aspx)des Steuer Elements "kreateuserwizard" geklickt wird Legen Sie die `ContinueDestinationPageUrl`-Eigenschaft auf "~/Membership/AdditionalUserInfo.aspx" fest. Dadurch wird der neue Benutzer `AdditionalUserInfo.aspx`, wo er seine Einstellungen anzeigen und aktualisieren kann.

### <a name="customizing-the-createuserwizards-interface-to-prompt-for-the-new-users-home-town-homepage-and-signature"></a>Anpassen der Benutzeroberfläche von "feateuserwizard", um zur Eingabe des Orts, der Homepage und der Signatur des neuen Benutzers aufzufordern

Die standardmäßige Oberfläche des Steuer Elements "kreateuserwizard" ist für einfache Konto Erstellungs Szenarien ausreichend, in denen nur wichtige Benutzerkontoinformationen wie Benutzername, Kennwort und e-Mail gesammelt werden müssen. Aber was ist, wenn wir den Besucher beim Erstellen Ihres Kontos zum Eingeben Ihrer Homepage, Homepage und Signatur auffordern möchten? Es ist möglich, die Schnittstelle des Steuer Elements "kreateuserwizard" anzupassen, um zusätzliche Informationen bei der Registrierung zu erfassen. diese Informationen können im `CreatedUser` Ereignishandler verwendet werden, um zusätzliche Datensätze in die zugrunde liegende Datenbank einzufügen.

Das Steuerelement "kreateuserwizard" erweitert das ASP.NET-Assistenten-Steuerelement, bei dem es sich um ein Steuerelement handelt, mit dem Seiten Entwickler eine Reihe geordneter `WizardSteps`definieren können. Das Assistenten-Steuerelement rendert den aktiven Schritt und stellt eine Navigationsschnittstelle bereit, die es dem Besucher ermöglicht, diese Schritte zu durchlaufen. Das Assistenten-Steuerelement ist ideal, um eine lange Aufgabe in mehrere kurze Schritte zu unterteilen. Weitere Informationen zum Assistenten-Steuerelement finden Sie unter [Erstellen einer Schritt-für-Schritt-Benutzeroberfläche mit dem ASP.NET 2,0-Assistenten-Steuer](http://aspnet.4guysfromrolla.com/articles/061406-1.aspx)Element.

Das Standard Markup des-Steuer Elements von "kreateuserwizard" definiert zwei `WizardSteps`: `CreateUserWizardStep` und `CompleteWizardStep`.

[!code-aspx[Main](storing-additional-user-information-cs/samples/sample12.aspx)]

Der erste `WizardStep`, `CreateUserWizardStep`, rendert die-Schnittstelle, die zur Eingabe von Benutzername, Kennwort, e-Mail-Adresse usw. auffordert. Nachdem der Besucher diese Informationen bereitgestellt und auf "Benutzer erstellen" geklickt hat, wird der `CompleteWizardStep`angezeigt, der die Erfolgsmeldung und die Schaltfläche "weiter" anzeigt.

Um die Schnittstelle des Steuer Elements "kreateuserwizard" so anzupassen, dass zusätzliche Formularfelder enthalten sind, können wir

- <strong>Erstellen Sie mindestens eine neue</strong> <strong>`WizardStep`</strong> <strong>s, um die zusätzlichen Benutzeroberflächen Elemente zu enthalten</strong>. Klicken Sie zum Hinzufügen eines neuen `WizardStep` zum Editor für "Debuggen" auf den Link "`WizardSteps`hinzufügen/entfernen" des Smarttags, um den `WizardStep` Auflistungs-Editor zu starten. Von dort aus können Sie die Schritte im Assistenten hinzufügen, entfernen oder neu anordnen. Dies ist der Ansatz, den wir für dieses Tutorial verwenden werden.

- <strong>Konvertieren Sie den</strong> <strong>`CreateUserWizardStep`</strong> <strong>in eine bearbeitbare</strong> <strong>`WizardStep`</strong> <strong>.</strong> Dadurch wird die `CreateUserWizardStep` durch eine äquivalente `WizardStep` ersetzt, deren Markup eine Benutzeroberfläche definiert, die dem `CreateUserWizardStep`entspricht. Indem die `CreateUserWizardStep` in eine `WizardStep` umgewandelt wird, können wir die Steuerelemente neu positionieren oder diesem Schritt zusätzliche Benutzeroberflächen Elemente hinzufügen. Wenn Sie die `CreateUserWizardStep` oder `CompleteWizardStep` in eine bearbeitbare `WizardStep`konvertieren möchten, klicken Sie auf den Link "Benutzer Schritt anpassen" oder "Schritt für Schritt anpassen" aus dem Smarttag des Steuer Elements.

- **Verwenden Sie eine Kombination der beiden oben genannten Optionen.**

Beachten Sie, dass das Steuerelement "" von "Create User" (Benutzerkonto erstellen) im `CreateUserWizardStep`das Erstellungs Verfahren für das Benutzerkonto ausführt, wenn Sie auf die Schaltfläche "Benutzer erstellen" klicken. Es spielt keine Rolle, ob nach dem `CreateUserWizardStep` weitere `WizardStep` s vorhanden sind.

Wenn Sie dem Steuerelement "kreateuserwizard" eine benutzerdefinierte `WizardStep` hinzufügen, um zusätzliche Benutzereingaben zu erfassen, können Sie die benutzerdefinierte `WizardStep` vor oder nach dem `CreateUserWizardStep`platzieren. Wenn Sie vor dem `CreateUserWizardStep` wird, sind die zusätzlichen Benutzereingaben, die vom benutzerdefinierten `WizardStep` gesammelt werden, für den `CreatedUser`-Ereignishandler verfügbar. Wenn die benutzerdefinierte `WizardStep` jedoch nach `CreateUserWizardStep` erfolgt, wenn die benutzerdefinierte `WizardStep` angezeigt wird, wurde das neue Benutzerkonto bereits erstellt, und das `CreatedUser` Ereignis wurde bereits ausgelöst.

Abbildung 21 zeigt den Workflow, wenn der hinzugefügte `WizardStep` dem `CreateUserWizardStep`vorangestellt ist. Da die zusätzlichen Benutzerinformationen zum Zeitpunkt der Auslösung des `CreatedUser` Ereignisses erfasst wurden, müssen Sie lediglich den `CreatedUser`-Ereignishandler aktualisieren, um diese Eingaben abzurufen und diese für die Parameterwerte der `INSERT` Anweisung (anstatt `DBNull.Value`) zu verwenden.

[![Sie den Workflow "feeuserwizard", wenn ein zusätzlicher WizardStep dem "feeudstep" vorangestellt ist.](storing-additional-user-information-cs/_static/image62.png)](storing-additional-user-information-cs/_static/image61.png)

**Abbildung 21**: der Workflow "buildeuserwizard", wenn ein zusätzlicher `WizardStep` dem `CreateUserWizardStep` vorangestellt ist ([Klicken Sie, um das Bild in voller Größe anzuzeigen](storing-additional-user-information-cs/_static/image63.png))

Wenn die benutzerdefinierte `WizardStep` *nach* der `CreateUserWizardStep`platziert wird, wird der Vorgang "Benutzerkonto erstellen" durchgeführt, bevor der Benutzer die Möglichkeit hat, Ihre Homepage, Homepage oder Signatur einzugeben. In diesem Fall müssen diese zusätzlichen Informationen in die Datenbank eingefügt werden, nachdem das Benutzerkonto erstellt wurde, wie in Abbildung 22 veranschaulicht.

[![Sie den Workflow "feeuserwizard", wenn ein zusätzlicher WizardStep nach dem "febereuserwizardstep"-Schritt ausgeführt wird.](storing-additional-user-information-cs/_static/image65.png)](storing-additional-user-information-cs/_static/image64.png)

**Abbildung 22**: der Workflow "buildeuserwizard", wenn ein zusätzliches `WizardStep` nach dem `CreateUserWizardStep` kommt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](storing-additional-user-information-cs/_static/image66.png))

Der in Abbildung 22 dargestellte Workflow wartet auf das Einfügen eines Datensatzes in die `UserProfiles` Tabelle, bis Schritt 2 abgeschlossen ist. Wenn der Besucher seinen Browser nach Schritt 1 schließt, haben wir jedoch einen Zustand erreicht, in dem ein Benutzerkonto erstellt wurde, der `UserProfiles`jedoch kein Datensatz hinzugefügt wurde. Eine Problem Umgehung besteht darin, einen Datensatz mit `NULL`-oder Standardwerten in `UserProfiles` in den `CreatedUser`-Ereignishandler einzufügen (der nach Schritt 1 ausgelöst wird), und diesen Datensatz nach Abschluss von Schritt 2 zu aktualisieren. Dadurch wird sichergestellt, dass für das Benutzerkonto ein `UserProfiles` Datensatz hinzugefügt wird, auch wenn der Benutzer den Registrierungsprozess in der Mitte durchläuft.

In diesem Tutorial erstellen wir eine neue `WizardStep`, die nach dem `CreateUserWizardStep`, aber vor dem `CompleteWizardStep`auftritt. Zuerst soll der WizardStep eingerichtet und konfiguriert werden, und dann sehen wir uns den Code an.

Wählen Sie im smarttagfeld des Steuer Elements "kreateuserwizard" die Option "`WizardStep` s hinzufügen/entfernen" aus, die das Dialogfeld `WizardStep` Auflistungs-Editor öffnet. Fügen Sie einen neuen `WizardStep`hinzu, und legen Sie dessen `ID` auf `UserSettings`, dessen `Title` auf "Ihre Einstellungen" und `StepType` auf `Step`fest. Positionieren Sie diese dann so, dass Sie nach dem `CreateUserWizardStep` ("registrieren Sie sich für Ihr neues Konto") und vor dem `CompleteWizardStep` ("Complete") angezeigt wird, wie in Abbildung 23 dargestellt.

[![dem Steuerelement "kreateuserwizard" einen neuen WizardStep hinzufügen](storing-additional-user-information-cs/_static/image68.png)](storing-additional-user-information-cs/_static/image67.png)

**Abbildung 23**: Hinzufügen eines neuen `WizardStep` zum Steuerelement "kreateuserwizard" ([Klicken Sie, um das Bild in voller Größe anzuzeigen](storing-additional-user-information-cs/_static/image69.png))

Klicken Sie auf OK, um das Dialogfeld `WizardStep` Auflistungs-Editor Die neue `WizardStep` wird durch das aktualisierte deklarative Markup des-Steuer Elements von "upateuserwizard" angezeigt:

[!code-aspx[Main](storing-additional-user-information-cs/samples/sample13.aspx)]

Beachten Sie das neue `<asp:WizardStep>` Element. Wir müssen die Benutzeroberfläche hinzufügen, um die Homepage, Homepage und Signatur des neuen Benutzers zu erfassen. Sie können diesen Inhalt in der deklarativen Syntax oder über den Designer eingeben. Um den Designer zu verwenden, wählen Sie den Schritt "Ihre Einstellungen" aus der Dropdown Liste im smarttagtag aus, um den Schritt im Designer anzuzeigen.

> [!NOTE]
> Wenn Sie in der Dropdown Liste des Smarttags einen Schritt auswählen, wird die [`ActiveStepIndex`-Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.activestepindex.aspx)des Steuer Elements "" des-Steuer Elements aktualisiert, das den Index des Anfangs Schritts angibt. Wenn Sie diese Dropdown Liste verwenden, um den Schritt "Ihre Einstellungen" im Designer zu bearbeiten, müssen Sie Sie daher auf "für Ihr neues Konto registrieren" zurücksetzen, damit dieser Schritt angezeigt wird, wenn Benutzer die `EnhancedCreateUserWizard.aspx` Seite besuchen.

Erstellen Sie eine Benutzeroberfläche innerhalb des Schritts "Ihre Einstellungen", die drei Textfeld-Steuerelemente mit den Namen `HomeTown`, `HomepageUrl`und `Signature`enthält. Nachdem Sie diese Schnittstelle erstellt haben, sollte das deklarative Markup von "kreateuserwizard" in etwa wie folgt aussehen:

[!code-aspx[Main](storing-additional-user-information-cs/samples/sample14.aspx)]

Besuchen Sie diese Seite über einen Browser, und erstellen Sie ein neues Benutzerkonto, in dem Sie Werte für die Homepage, die Homepage und die Signatur angeben. Nach Abschluss des-`CreateUserWizardStep` wird das Benutzerkonto im Mitgliedschafts Framework erstellt, und der `CreatedUser`-Ereignishandler wird ausgeführt. Dadurch wird `UserProfiles`eine neue Zeile hinzugefügt, jedoch mit einem Datenbank-`NULL` Wert für `HomeTown`, `HomepageUrl`und `Signature`. Die für "Home", "Homepage" und "Signatur" eingegebenen Werte werden niemals verwendet. Das Ergebnis ist ein neues Benutzerkonto mit einem `UserProfiles` Datensatz, dessen `HomeTown`, `HomepageUrl`und `Signature` Felder noch angegeben werden müssen.

Wir müssen nach dem Schritt "Ihre Einstellungen" Code ausführen, der die vom Benutzer eingegebenen Werte Home, honepage und Signature annimmt und den entsprechenden `UserProfiles` Datensatz aktualisiert. Jedes Mal, wenn der Benutzer zwischen den Schritten in einem Assistenten-Steuerelement wechselt, wird das [`ActiveStepChanged` Ereignis](https://msdn.microsoft.com/library/system.web.ui.webcontrols.wizard.activestepchanged.aspx) des Assistenten ausgelöst. Wir können einen Ereignishandler für dieses Ereignis erstellen und die `UserProfiles` Tabelle aktualisieren, wenn der Schritt "Ihre Einstellungen" abgeschlossen ist.

Fügen Sie einen Ereignishandler für das `ActiveStepChanged`-Ereignis von "feateuserwizard" hinzu, und fügen Sie den folgenden Code hinzu:

[!code-csharp[Main](storing-additional-user-information-cs/samples/sample15.cs)]

Der obige Code beginnt mit der Bestimmung, ob wir soeben den Schritt "Complete" (fertig) erreicht haben. Da der Schritt "abschließen" unmittelbar nach dem Schritt "Ihre Einstellungen" erfolgt, wenn der Besucher den Schritt "abschließen" erreicht, bedeutet dies, dass Sie den Schritt "Ihre Einstellungen" gerade abgeschlossen haben.

In einem solchen Fall müssen wir Programm gesteuert auf die Textfeld-Steuerelemente innerhalb der `UserSettings WizardStep`verweisen. Dies wird erreicht, indem zuerst die `FindControl`-Methode verwendet wird, um Programm gesteuert auf die `UserSettings WizardStep`zu verweisen, und dann erneut auf die Textfelder innerhalb des `WizardStep`verwiesen wird. Nachdem auf die Textfelder verwiesen wurde, können wir die `UPDATE`-Anweisung ausführen. Die `UPDATE`-Anweisung verfügt über die gleiche Anzahl von Parametern wie die `INSERT`-Anweisung im `CreatedUser`-Ereignishandler, aber hier werden die vom Benutzer bereitgestellten Home-, Homepage-und Signatur Werte verwendet.

Wenn dieser Ereignishandler vorhanden ist, besuchen Sie die Seite `EnhancedCreateUserWizard.aspx` über einen Browser, und erstellen Sie ein neues Benutzerkonto, in dem die Werte für Home, Homepage und Signature angegeben werden. Nachdem Sie das neue Konto erstellt haben, sollten Sie auf die Seite `AdditionalUserInfo.aspx` umgeleitet werden, auf der die soeben eingegebenen Informationen zu Homepage, Homepage und Signatur angezeigt werden.

> [!NOTE]
> Unsere Website verfügt zurzeit über zwei Seiten, von denen ein Besucher ein neues Konto erstellen kann: `CreatingUserAccounts.aspx` und `EnhancedCreateUserWizard.aspx`. Die Seite "Sitemap und Anmeldung" der Website verweist auf die Seite "`CreatingUserAccounts.aspx`", aber die `CreatingUserAccounts.aspx` Seite fordert den Benutzer nicht zur Eingabe von Orts-, Homepage-und Signatur Informationen auf und fügt `UserProfiles`keine entsprechende Zeile hinzu. Aktualisieren Sie daher die `CreatingUserAccounts.aspx` Seite so, dass Sie diese Funktion bietet, oder aktualisieren Sie die Seite "Sitemap" und "Login" so, dass Sie anstelle `CreatingUserAccounts.aspx`auf `EnhancedCreateUserWizard.aspx` verweist. Wenn Sie die zweite Option auswählen, achten Sie darauf, dass Sie die `Web.config` Datei des `Membership` Ordners aktualisieren, damit anonyme Benutzer auf die `EnhancedCreateUserWizard.aspx` Seite zugreifen können.

## <a name="summary"></a>Zusammenfassung

In diesem Tutorial haben wir Techniken zum Modellieren von Daten im Zusammenhang mit Benutzerkonten innerhalb des Mitgliedschafts-Frameworks behandelt. Insbesondere haben wir uns mit der Modellierung von Entitäten beschäftigt, die eine 1: n-Beziehung mit Benutzerkonten und Daten gemeinsam nutzen, die eine 1:1-Beziehung gemeinsam haben. Außerdem haben wir gesehen, wie diese verwandten Informationen angezeigt, eingefügt und aktualisiert werden können. Beispiele hierfür sind das SqlDataSource-Steuerelement und andere Benutzer, die ADO.NET-Code verwenden.

In diesem Tutorial wird die Betrachtung von Benutzerkonten abgeschlossen. Beginnend mit dem nächsten Tutorial werden wir uns mit den Rollen befassen. In den nächsten Tutorials wird das Rollen Framework erläutert, das Erstellen neuer Rollen, das Zuweisen von Rollen zu Benutzern, das Ermitteln der Rollen, zu denen ein Benutzer gehört, und das Anwenden der rollenbasierten Autorisierung.

Fröhliche Programmierung!

### <a name="further-reading"></a>Weitere nützliche Informationen

Weitere Informationen zu den in diesem Tutorial behandelten Themen finden Sie in den folgenden Ressourcen:

- [Zugreifen auf und Aktualisieren von Daten in ASP.NET 2,0](http://aspnet.4guysfromrolla.com/articles/011106-1.aspx)
- [ASP.NET 2,0-Assistenten-Steuerelement](https://weblogs.asp.net/scottgu/archive/2006/02/21/438732.aspx)
- [Erstellen einer Schritt-für-Schritt-Benutzeroberfläche mit dem ASP.NET 2,0-Assistenten-Steuerelement](http://aspnet.4guysfromrolla.com/articles/061406-1.aspx)
- [Erstellen von benutzerdefinierten DataSource-Steuerelement Parametern](http://aspnet.4guysfromrolla.com/articles/110106-1.aspx)
- [Anpassen des Steuer Elements "kreateuserwizard"](http://aspnet.4guysfromrolla.com/articles/070506-1.aspx)
- [Schnellstarts für DetailsView-Steuerelemente](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/data/detailsview.aspx)
- [Anzeigen von Daten mit dem ListView-Steuerelement](http://aspnet.4guysfromrolla.com/articles/122607-1.aspx)
- [Dissecting der Validierungs Steuerelemente in ASP.NET 2,0](http://aspnet.4guysfromrolla.com/articles/112305-1.aspx)
- [Bearbeiten und Löschen von Daten](../../data-access/editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md)
- [Formular Validierung in ASP.net](http://www.4guysfromrolla.com/webtech/090200-1.shtml)
- [Sammeln von benutzerdefinierten Benutzer Registrierungsinformationen](https://weblogs.asp.net/scottgu/archive/2006/07/05/Tip_2F00_Trick_3A00_-Gathering-Custom-User-Registration-Information.aspx)
- [Profile in ASP.NET 2,0](http://www.odetocode.com/Articles/440.aspx)
- [Das ASP: ListView-Steuerelement](https://weblogs.asp.net/scottgu/archive/2007/08/10/the-asp-listview-control-part-1-building-a-product-listing-page-with-clean-css-ui.aspx)
- [Schnellstart für Benutzerprofile](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/profile/default.aspx)

### <a name="about-the-author"></a>Zum Autor

Scott Mitchell, Autor mehrerer ASP/ASP. net-Bücher und Gründer von 4GuysFromRolla.com, hat seit 1998 mit Microsoft-Webtechnologien gearbeitet. Scott arbeitet als unabhängiger Berater, Ausbilder und Writer. Sein letztes Buch ist *[Sams Teach Yourself ASP.NET 2,0 in 24 Stunden](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)* . Scott kann über [mitchell@4guysfromrolla.com](mailto:mitchell@4guysfromrolla.com) oder über seinen Blog unter [http://ScottOnWriting.NET](http://scottonwriting.net/)erreicht werden.

### <a name="special-thanks-to"></a>Besonders vielen Dank...

Diese tutorialreihe wurde von vielen hilfreichen Reviewern geprüft. Möchten Sie meine bevorstehenden MSDN-Artikel überprüfen? Wenn dies der Fall ist, können Sie eine Zeile in [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)ablegen.

> [!div class="step-by-step"]
> [Zurück](user-based-authorization-cs.md)
> [Weiter](creating-the-membership-schema-in-sql-server-vb.md)
