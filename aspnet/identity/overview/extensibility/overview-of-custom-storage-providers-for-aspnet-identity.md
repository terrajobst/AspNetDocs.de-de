---
uid: identity/overview/extensibility/overview-of-custom-storage-providers-for-aspnet-identity
title: Übersicht über benutzerdefinierte Speicher Anbieter für ASP.net Identity-ASP.NET 4. x
author: Rick-Anderson
description: ASP.net Identity ist ein erweiterbares System, mit dem Sie einen eigenen Speicher Anbieter erstellen und in Ihre Anwendung einbinden können, ohne den Löschvorgang erneut zu arbeiten...
ms.author: riande
ms.date: 10/13/2014
ms.assetid: 681a9204-462e-4260-9a0b-19f0644d6ad7
ms.custom: seoapril2019
msc.legacyurl: /identity/overview/extensibility/overview-of-custom-storage-providers-for-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: 21baedf6285b411f89627df9ca25d47a2a42e387
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78472221"
---
# <a name="overview-of-custom-storage-providers-for-aspnet-identity"></a>Übersicht über benutzerdefinierte Speicheranbieter für ASP.NET Identity

von [Tom fitzmacken](https://github.com/tfitzmac)

> ASP.net Identity ist ein erweiterbares System, mit dem Sie einen eigenen Speicher Anbieter erstellen und in Ihre Anwendung einbinden können, ohne die Anwendung neu zu arbeiten. In diesem Thema wird beschrieben, wie ein angepasster Speicher Anbieter für ASP.net Identity erstellt wird. Es behandelt die wichtigsten Konzepte zum Erstellen eines eigenen Speicher Anbieters, aber es handelt sich nicht um eine schrittweise exemplarische Vorgehensweise zum Implementieren eines benutzerdefinierten Speicher Anbieters.
> 
> Ein Beispiel für die Implementierung eines benutzerdefinierten Speicher Anbieters finden Sie unter [Implementieren eines benutzerdefinierten MySQL-ASP.net Identity Speicher Anbieters](implementing-a-custom-mysql-aspnet-identity-storage-provider.md).
> 
> Dieses Thema wurde für ASP.net Identity 2,0 aktualisiert.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Im Tutorial verwendete Software Versionen
> 
> 
> - Visual Studio 2013 mit Update 2
> - ASP.net Identity 2

## <a name="introduction"></a>Einführung

Standardmäßig speichert das ASP.net Identity Systembenutzer Informationen in einer SQL Server-Datenbank und verwendet Entity Framework Code First, um die Datenbank zu erstellen. Bei vielen Anwendungen funktioniert dieser Ansatz gut. Möglicherweise bevorzugen Sie jedoch eine andere Art von Persistenzmechanismus, wie z. b. Azure Table Storage, oder Sie verfügen möglicherweise bereits über Datenbanktabellen mit einer sehr anderen Struktur als die Standard Implementierung. In beiden Fällen können Sie einen benutzerdefinierten Anbieter für den Speichermechanismus schreiben und den Anbieter in Ihre Anwendung einbinden.

ASP.net Identity ist in vielen der Visual Studio 2013 Vorlagen Standardmäßig enthalten. Sie können ASP.net Identity über das [nuget-Paket Microsoft ASPNET Identity EntityFramework](http://www.nuget.org/packages/Microsoft.AspNet.Identity.EntityFramework/)Updates für erhalten.

Dieses Thema enthält die folgenden Abschnitte:

- [Grundlegendes zur Architektur](#architecture)
- [Verstehen der gespeicherten Daten](#data)
- [Erstellen der Datenzugriffs Ebene](#dal)
- [Benutzerklasse anpassen](#user)
- [Anpassen des Benutzer Stores](#userstore)
- [Anpassen der Rollen Klasse](#role)
- [Anpassen des Rollen Speicher](#rolestore)
- [Neukonfigurieren der Anwendung für die Verwendung eines neuen Speicher Anbieters](#reconfigure)
- [Andere Implementierungen von benutzerdefinierten Speicheranbietern](#other)

<a id="architecture"></a>
## <a name="understand-the-architecture"></a>Grundlegendes zur Architektur

ASP.net Identity besteht aus Klassen, die als Manager und Stores bezeichnet werden. Manager sind Klassen auf hoher Ebene, die von einem Anwendungsentwickler zum Ausführen von Vorgängen verwendet werden, z. b. das Erstellen eines Benutzers im ASP.net Identity System. Stores sind Klassen auf niedrigerer Ebene, die angeben, wie Entitäten, z. b. Benutzer und Rollen, beibehalten werden. Geschäfte sind eng mit dem Persistenzmechanismus verknüpft, aber die Manager sind von den Geschäften entkoppelt, sodass Sie den Persistenzmechanismus ersetzen können, ohne die gesamte Anwendung zu unterbrechen.

Das folgende Diagramm zeigt, wie Ihre Webanwendung mit den Managern interagiert, und speichert die Interaktion mit der Datenzugriffs Ebene.

![](overview-of-custom-storage-providers-for-aspnet-identity/_static/image1.png)

Um einen angepassten Speicher Anbieter für ASP.net Identity zu erstellen, müssen Sie die Datenquelle, die Datenzugriffs Ebene und die Speicher Klassen erstellen, die mit dieser Datenzugriffs Schicht interagieren. Die gleichen Manager-APIs können weiterhin zum Ausführen von Daten Vorgängen verwendet werden, die Daten werden jedoch jetzt in einem anderen Speichersystem gespeichert.

Sie müssen die Manager-Klassen nicht anpassen, da Sie beim Erstellen einer neuen Instanz von usermanager oder roleManager den Typ der Benutzerklasse angeben und eine Instanz der Store-Klasse als Argument übergeben. Mit diesem Ansatz können Sie die benutzerdefinierten Klassen in die vorhandene Struktur einbinden. Sie erfahren, wie Sie usermanager und roleManager mit Ihren angepassten Store-Klassen instanziieren [, indem Sie die Anwendung neu konfigurieren, um einen neuen Speicher Anbieter zu verwenden](#reconfigure).

<a id="data"></a>
## <a name="understand-the-data-that-is-stored"></a>Verstehen der gespeicherten Daten

Zum Implementieren eines benutzerdefinierten Speicher Anbieters müssen Sie die Datentypen verstehen, die mit ASP.net Identity verwendet werden, und entscheiden, welche Features für Ihre Anwendung relevant sind.

| Data | Beschreibung |
| --- | --- |
| Benutzer | Registrierte Benutzer Ihrer Website. Schließt die Benutzer-ID und den Benutzernamen ein. Kann ein Hashwert für das Kennwort enthalten, wenn sich Benutzer mit Anmelde Informationen anmelden, die für Ihren Standort spezifisch sind (anstatt Anmelde Informationen von einer externen Website wie Facebook zu verwenden), und den Sicherheits Stempel angeben, um anzugeben, ob etwas in den Anmelde Informationen des Benutzers geändert wurde. Kann auch die e-Mail-Adresse, Telefonnummer, ob die zweistufige Authentifizierung aktiviert ist, die aktuelle Anzahl der fehlgeschlagenen Anmeldungen und ob ein Konto gesperrt ist. |
| Benutzer Ansprüche | Ein Satz von-Anweisungen (oder-Ansprüchen) zum Benutzer, die die Identität des Benutzers darstellen. Kann einen größeren Ausdruck der Identität des Benutzers aktivieren, als durch Rollen erreicht werden kann. |
| Benutzeranmeldungen | Informationen zum externen Authentifizierungs Anbieter (z. b. Facebook), der bei der Anmeldung eines Benutzers verwendet werden soll. |
| Rollen | Autorisierungs Gruppen für Ihre Website. Enthält die Rollen-ID und den Rollennamen (z. b. "admin" oder "Employee"). |

<a id="dal"></a>
## <a name="create-the-data-access-layer"></a>Erstellen der Datenzugriffs Ebene

In diesem Thema wird davon ausgegangen, dass Sie mit dem Persistenzmechanismus vertraut sind, den Sie verwenden werden, und wie Sie Entitäten für diesen Mechanismus erstellen. In diesem Thema wird nicht erläutert, wie die-Depots oder Datenzugriffsklassen erstellt werden. Stattdessen finden Sie einige Vorschläge zu den Entwurfsentscheidungen, die Sie beim Arbeiten mit ASP.net Identity treffen müssen.

Beim Entwerfen der Depots für einen angepassten Speicher Anbieter haben Sie viel Freiheit. Sie müssen nur Repository für Funktionen erstellen, die Sie in Ihrer Anwendung verwenden möchten. Wenn Sie z. b. keine Rollen in der Anwendung verwenden, müssen Sie keinen Speicher für Rollen oder Benutzer Rollen erstellen. Ihre Technologie und vorhandene Infrastruktur benötigen möglicherweise eine Struktur, die sich stark von der Standard Implementierung ASP.net Identity unterscheidet. In ihrer Datenzugriffs Schicht stellen Sie die Logik bereit, um mit der Struktur ihrer Depots zu arbeiten.

Eine MySQL-Implementierung von Data Repositories für ASP.net Identity 2,0 finden Sie unter [mysqlidentity. SQL](https://github.com/aspnet/samples/blob/master/samples/aspnet/Identity/AspNet.Identity.MySQL/MySQLIdentity.sql).

In der Datenzugriffs Ebene stellen Sie die Logik bereit, um die Daten aus ASP.net Identity in der Datenquelle zu speichern. Die Datenzugriffs Ebene für Ihren benutzerdefinierten Speicher Anbieter kann die folgenden Klassen enthalten, um Benutzer-und Rollen Informationen zu speichern.

| Klasse | Beschreibung | Beispiel |
| --- | --- | --- |
| Kontext | Kapselt die Informationen zum Herstellen einer Verbindung mit dem Persistenzmechanismus und zum Ausführen von Abfragen. Diese Klasse ist für die Datenzugriffs Ebene von zentraler Bedeutung. Die anderen Daten Klassen erfordern eine Instanz dieser Klasse, um Ihre Vorgänge auszuführen. Außerdem initialisieren Sie Ihre Store-Klassen mit einer Instanz dieser Klasse. | [Mysqldatabase](https://github.com/aspnet/samples/blob/master/samples/aspnet/Identity/AspNet.Identity.MySQL/MySQLDatabase.cs) |
| Benutzerspeicher | Speichert Benutzerinformationen (z. b. Benutzername und Kenn Wort Hash) und ruft Sie ab. | [Usertable (MySQL)](https://github.com/aspnet/samples/blob/master/samples/aspnet/Identity/AspNet.Identity.MySQL/UserTable.cs) |
| Rollen Speicher | Speichert Rollen Informationen (z. b. den Rollennamen) und ruft Sie ab. | [Roletable (MySQL)](https://github.com/aspnet/samples/blob/master/samples/aspnet/Identity/AspNet.Identity.MySQL/RoleTable.cs) |
| Speicherung von Benutzer Ansprüchen | Speichert Benutzer Anspruchs Informationen (z. b. den Anspruchstyp und-Wert) und ruft Sie ab. | [Userclaimstable (MySQL)](https://github.com/aspnet/samples/blob/master/samples/aspnet/Identity/AspNet.Identity.MySQL/UserClaimsTable.cs) |
| Speicherung von Benutzeranmeldungen | Speichert und ruft Benutzer Anmelde Informationen ab (z. b. einen externen Authentifizierungs Anbieter). | [Userloginstable (MySQL)](https://github.com/aspnet/samples/blob/master/samples/aspnet/Identity/AspNet.Identity.MySQL/UserLoginsTable.cs) |
| Benutzer Rollen Speicher | Speichert und ruft ab, welchen Rollen ein Benutzer zugewiesen ist. | [Userroletable (MySQL)](https://github.com/aspnet/samples/blob/master/samples/aspnet/Identity/AspNet.Identity.MySQL/UserRoleTable.cs) |

Auch hier müssen Sie nur die Klassen implementieren, die Sie in Ihrer Anwendung verwenden möchten.

In den Datenzugriffsklassen stellen Sie Code zum Ausführen von Daten Vorgängen für Ihren bestimmten Persistenzmechanismus bereit. Beispielsweise enthält die usertable-Klasse innerhalb der MySQL-Implementierung eine-Methode, um einen neuen Datensatz in die Benutzerdaten Bank Tabelle einzufügen. Die Variable mit dem Namen `_database` ist eine Instanz der mysqldatabase-Klasse.

[!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample1.cs)]

Nachdem Sie die Datenzugriffsklassen erstellt haben, müssen Sie Speicher Klassen erstellen, die die spezifischen Methoden in der Datenzugriffs Ebene aufrufen.

<a id="user"></a>
## <a name="customize-the-user-class"></a>Benutzerklasse anpassen

Wenn Sie einen eigenen Speicher Anbieter implementieren, müssen Sie eine Benutzerklasse erstellen, die der [identityuser](https://msdn.microsoft.com/library/microsoft.aspnet.identity.entityframework.identityuser(v=vs.108).aspx) -Klasse im [Microsoft. ASP. net. Identity. EntityFramework](https://msdn.microsoft.com/library/microsoft.aspnet.identity.entityframework(v=vs.108).aspx) -Namespace entspricht:

Das folgende Diagramm zeigt die identityuser-Klasse, die Sie erstellen müssen, und die-Schnittstelle, die in dieser Klasse implementiert werden muss.

![](overview-of-custom-storage-providers-for-aspnet-identity/_static/image2.png)

Die [iuser&lt;TKey&gt;](https://msdn.microsoft.com/library/dn613291(v=vs.108).aspx) -Schnittstelle definiert die Eigenschaften, die der usermanager beim Ausführen angeforderter Vorgänge aufruft. Die Schnittstelle enthält zwei Eigenschaften: ID und Benutzername. Mit der [iuser&lt;TKey&gt;](https://msdn.microsoft.com/library/dn613291(v=vs.108).aspx) -Schnittstelle können Sie den Typ des Schlüssels für den Benutzer über den generischen **TKey** -Parameter angeben. Der Typ der ID-Eigenschaft stimmt mit dem Wert des TKey-Parameters überein.

Das Identitäts Framework stellt außerdem die [iuser](https://msdn.microsoft.com/library/microsoft.aspnet.identity.iuser(v=vs.108).aspx) -Schnittstelle (ohne den generischen Parameter) bereit, wenn Sie einen Zeichen folgen Wert für den Schlüssel verwenden möchten.

Die identityuser-Klasse implementiert iuser und enthält alle zusätzlichen Eigenschaften oder Konstruktoren für Benutzer auf Ihrer Website. Das folgende Beispiel zeigt eine identityuser-Klasse, die eine ganze Zahl für den Schlüssel verwendet. Das ID-Feld wird auf **int** festgelegt, um den Wert des generischen Parameters abzugleichen. 

[!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample2.cs)]

 Eine komplette Implementierung finden Sie unter [identityuser (MySQL)](https://github.com/aspnet/samples/blob/master/samples/aspnet/Identity/AspNet.Identity.MySQL/IdentityUser.cs). 

<a id="userstore"></a>
## <a name="customize-the-user-store"></a>Anpassen des Benutzer Stores

Außerdem erstellen Sie eine userstore-Klasse, die die Methoden für alle Daten Vorgänge für den Benutzer bereitstellt. Diese Klasse entspricht der Klasse [userstore&lt;tuser&gt;](https://msdn.microsoft.com/library/dn315446(v=vs.108).aspx) im [Microsoft. ASP. net. Identity. EntityFramework](https://msdn.microsoft.com/library/microsoft.aspnet.identity.entityframework(v=vs.108).aspx) -Namespace. In ihrer userstore-Klasse implementieren Sie den [iuserstore&lt;tuser, TKey&gt;](https://msdn.microsoft.com/library/dn613276(v=vs.108).aspx) und eine beliebige optionale Schnittstelle. Basierend auf der Funktionalität, die Sie in Ihrer Anwendung bereitstellen möchten, wählen Sie die optionalen Schnittstellen aus, die Sie implementieren möchten.

Die folgende Abbildung zeigt die userstore-Klasse, die Sie erstellen müssen, und die entsprechenden Schnittstellen.

![](overview-of-custom-storage-providers-for-aspnet-identity/_static/image3.png)

Die Standard Projektvorlage in Visual Studio enthält Code, der annimmt, dass viele optionale Schnittstellen im Benutzerspeicher implementiert wurden. Wenn Sie die Standardvorlage mit einem benutzerdefinierten Benutzerspeicher verwenden, müssen Sie entweder optionale Schnittstellen im Benutzerspeicher implementieren oder den Vorlagen Code ändern, sodass Methoden nicht mehr in den nicht implementierten Schnittstellen aufgerufen werden.

 Das folgende Beispiel zeigt eine einfache Benutzerspeicher Klasse. Der generische **tuser** -Parameter übernimmt den Typ der Benutzerklasse, bei der es sich normalerweise um die von Ihnen definierte identityuser-Klasse handelt. Der generische **TKey** -Parameter übernimmt den Typ des Benutzer Schlüssels. 

[!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample3.cs)]

 In diesem Beispiel ist der Konstruktor, der einen Parameter mit dem Namen " *Database* " vom Typ "exampledatabase" annimmt, nur eine Veranschaulichung, wie die Datenzugriffs Klasse übergeben werden kann. In der MySQL-Implementierung übernimmt dieser Konstruktor z. b. einen Parameter des Typs mysqldatabase. 

In ihrer userstore-Klasse verwenden Sie die Datenzugriffsklassen, die Sie erstellt haben, um Vorgänge auszuführen. Beispielsweise verfügt die userstore-Klasse in der MySQL-Implementierung über die Methode "Methode", die eine Instanz von "usertable" verwendet, um einen neuen Datensatz einzufügen. Die **Insert** -Methode für das **usertable** -Objekt ist dieselbe Methode, die im vorherigen Abschnitt gezeigt wurde. 

[!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample4.cs)]

### <a name="interfaces-to-implement-when-customizing-user-store"></a>Schnittstellen, die beim Anpassen des Benutzer Stores implementiert werden

In der nächsten Abbildung werden weitere Details zu den Funktionen angezeigt, die in jeder Schnittstelle definiert sind. Alle optionalen Schnittstellen erben von iuserstore.

![](overview-of-custom-storage-providers-for-aspnet-identity/_static/image4.png)

- **IUserStore**  
  Der [iuserstore&lt;tuser, TKey&gt;](https://msdn.microsoft.com/library/dn613278(v=vs.108).aspx) Interface ist die einzige Schnittstelle, die Sie in Ihrem Benutzerspeicher implementieren müssen. Es definiert Methoden zum Erstellen, aktualisieren, löschen und Abrufen von Benutzern.
- **Iuserclaimstore ist**  
  Das [iuserclaimstore-&lt;tuser, TKey&gt;](https://msdn.microsoft.com/library/dn613265(v=vs.108).aspx) -Schnittstelle definiert die Methoden, die Sie in Ihrem Benutzerspeicher implementieren müssen, um Benutzer Ansprüche zu aktivieren. Sie enthält Methoden oder das Hinzufügen, entfernen und Abrufen von Benutzer Ansprüchen.
- **Iuserloginstore ist**  
  Das [iuserloginstore-&lt;tuser, TKey&gt;](https://msdn.microsoft.com/library/dn613272(v=vs.108).aspx) definiert die Methoden, die Sie in Ihrem Benutzerspeicher implementieren müssen, um externe Authentifizierungs Anbieter zu aktivieren. Sie enthält Methoden zum Hinzufügen, entfernen und Abrufen von Benutzeranmeldungen sowie eine Methode zum Abrufen eines Benutzers basierend auf den Anmelde Informationen.
- **Iuserrolestore ist**  
  Das [iuserrolestore-&lt;TKey, die tuser-&gt;](https://msdn.microsoft.com/library/dn613276(v=vs.108).aspx) Schnittstelle definiert die Methoden, die Sie in Ihrem Benutzerspeicher implementieren müssen, um einen Benutzer einer Rolle zuzuordnen. Sie enthält Methoden zum Hinzufügen, entfernen und Abrufen der Rollen eines Benutzers sowie eine Methode, um zu überprüfen, ob ein Benutzer einer Rolle zugewiesen ist.
- **Iuserpasswordstore ist**  
  Das [iuserpasswordstore-&lt;tuser, TKey&gt;](https://msdn.microsoft.com/library/dn613273(v=vs.108).aspx) -Schnittstelle definiert die Methoden, die Sie in Ihrem Benutzerspeicher implementieren müssen, um Hash Kennwörter beizubehalten. Sie enthält Methoden zum erhalten und Festlegen des hashkennworts sowie eine Methode, die angibt, ob der Benutzer ein Kennwort festgelegt hat.
- **Iusersecuritystampstore**  
  Das [iusersecuritystampstore-&lt;tuser, TKey&gt;](https://msdn.microsoft.com/library/dn613277(v=vs.108).aspx) -Schnittstelle definiert die Methoden, die Sie in Ihrem Benutzerspeicher implementieren müssen, um mithilfe eines Sicherheits Stempels anzugeben, ob sich die Kontoinformationen des Benutzers geändert haben. Dieser Stempel wird aktualisiert, wenn ein Benutzer das Kennwort ändert oder Anmeldungen hinzufügt oder entfernt. Sie enthält Methoden zum erhalten und Festlegen des Sicherheits Stempels.
- **Iusertwofactorstore ist**  
  Das [iusertwofactorstore-&lt;tuser, TKey&gt;](https://msdn.microsoft.com/library/dn613279(v=vs.108).aspx) -Schnittstelle definiert die Methoden, die Sie implementieren müssen, um die zweistufige Authentifizierung zu implementieren. Sie enthält Methoden zum erhalten und festlegen, ob die zweistufige Authentifizierung für einen Benutzer aktiviert ist.
- **Iuserphonenumberstore ist**  
  Die [iuserphonenumberstore-&lt;tuser, TKey&gt;](https://msdn.microsoft.com/library/dn613275(v=vs.108).aspx) -Schnittstelle definiert die Methoden, die Sie implementieren müssen, um die Telefonnummern von Benutzern zu speichern. Sie enthält Methoden zum erhalten und Festlegen der Telefonnummer und darüber, ob die Telefonnummer bestätigt ist.
- **Iuseremailstore ist**  
  Der [iuseremailstore&lt;tuser, TKey&gt;](https://msdn.microsoft.com/library/dn613143(v=vs.108).aspx) Interface definiert die Methoden, die Sie implementieren müssen, um e-Mail-Adressen von Benutzern zu speichern. Sie enthält Methoden zum erhalten und Festlegen der e-Mail-Adresse und darüber, ob die e-Mail bestätigt ist.
- **Iuserlockoutstore ist**  
  Das [iuserlockoutstore-&lt;tuser, TKey&gt;](https://msdn.microsoft.com/library/dn613271(v=vs.108).aspx) -Schnittstelle definiert die Methoden, die Sie implementieren müssen, um Informationen zum Sperren eines Kontos zu speichern. Sie enthält Methoden zum Abrufen der aktuellen Anzahl fehlerhafter Zugriffsversuche, abrufen und festlegen, ob das Konto gesperrt werden kann, abrufen und Festlegen des Enddatums für die Sperrung, erhöhen der Anzahl fehlerhafter Versuche und Zurücksetzen der Anzahl fehlgeschlagener Versuche.
- **Iqueryableuserstore ist**  
  Die [iqueryableuserstore-&lt;tuser, TKey&gt;](https://msdn.microsoft.com/library/dn613267(v=vs.108).aspx) -Schnittstelle definiert die Elemente, die Sie implementieren müssen, um einen abfragbaren Benutzerspeicher bereitzustellen. Sie enthält eine Eigenschaft, die die abfragbaren Benutzer enthält.

  Sie implementieren die Schnittstellen, die in Ihrer Anwendung erforderlich sind. beispielsweise die Schnittstellen iuserclaimstore, iuserloginstore, iuserrolestore, iuserpasswordstore und iusersecuritystampstore, wie unten gezeigt. 

[!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample5.cs)]

Eine vollständige Implementierung (einschließlich aller Schnittstellen) finden Sie unter [userstore (MySQL)](https://github.com/aspnet/samples/blob/master/samples/aspnet/Identity/AspNet.Identity.MySQL/UserStore.cs).

### <a name="identityuserclaim-identityuserlogin-and-identityuserrole"></a>Identityuserclaim, identityuserlogin und identityuserrole

Der Microsoft. Aspnet. Identity. EntityFramework-Namespace enthält Implementierungen der Klassen [identityuserclaim](https://msdn.microsoft.com/library/dn613250(v=vs.108).aspx), [identityuserlogin](https://msdn.microsoft.com/library/dn613251(v=vs.108).aspx)und [identityuserrole](https://msdn.microsoft.com/library/dn613252(v=vs.108).aspx) . Wenn Sie diese Features verwenden, möchten Sie möglicherweise eigene Versionen dieser Klassen erstellen und die Eigenschaften für Ihre Anwendung definieren. Manchmal ist es jedoch effizienter, diese Entitäten beim Ausführen von grundlegenden Vorgängen (z. b. hinzufügen oder Entfernen eines Benutzer Anspruchs) nicht in den Arbeitsspeicher zu laden. Die Back-End-Speicher Klassen können diese Vorgänge stattdessen direkt für die Datenquelle ausführen. Beispielsweise kann die userstore. getclaimsasync ()-Methode die userclaimtable. findbyuserid (User) aufrufen. ID)-Methode, um eine Abfrage für diese Tabelle direkt auszuführen und eine Liste von Ansprüchen zurückzugeben.

[!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample6.cs)]

<a id="role"></a>
## <a name="customize-the-role-class"></a>Anpassen der Rollen Klasse

Wenn Sie einen eigenen Speicher Anbieter implementieren, müssen Sie eine Rollen Klasse erstellen, die der [identityrole](https://msdn.microsoft.com/library/microsoft.aspnet.identity.entityframework.identityrole(v=vs.108).aspx) -Klasse im [Microsoft. ASP. net. Identity. EntityFramework](https://msdn.microsoft.com/library/microsoft.aspnet.identity.entityframework(v=vs.108).aspx) -Namespace entspricht:

Das folgende Diagramm zeigt die identityrole-Klasse, die Sie erstellen müssen, und die-Schnittstelle, die in dieser Klasse implementiert werden muss.

![](overview-of-custom-storage-providers-for-aspnet-identity/_static/image5.png)

Die [IRole-&lt;TKey&gt;](https://msdn.microsoft.com/library/dn613268(v=vs.108).aspx) -Schnittstelle definiert die Eigenschaften, die der roleManager beim Ausführen angeforderter Vorgänge aufruft. Die Schnittstelle enthält zwei Eigenschaften: ID und Name. Mit der [IRole-&lt;TKey&gt;](https://msdn.microsoft.com/library/dn613268(v=vs.108).aspx) -Schnittstelle können Sie den Typ des Schlüssels für die Rolle über den generischen **TKey** -Parameter angeben. Der Typ der ID-Eigenschaft stimmt mit dem Wert des TKey-Parameters überein.

Das Identitäts Framework stellt außerdem die [IRole](https://msdn.microsoft.com/library/microsoft.aspnet.identity.irole(v=vs.108).aspx) -Schnittstelle (ohne den generischen Parameter) bereit, wenn Sie einen Zeichen folgen Wert für den Schlüssel verwenden möchten.

Das folgende Beispiel zeigt eine identityrole-Klasse, die eine ganze Zahl für den Schlüssel verwendet. Das ID-Feld wird auf int festgelegt, um den Wert des generischen Parameters abzugleichen. 

[!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample7.cs)]

 Eine komplette Implementierung finden Sie unter [identityrole (MySQL)](https://github.com/aspnet/samples/blob/master/samples/aspnet/Identity/AspNet.Identity.MySQL/IdentityRole.cs). 

<a id="rolestore"></a>
## <a name="customize-the-role-store"></a>Anpassen des Rollen Speicher

Außerdem erstellen Sie eine rolestore-Klasse, die die Methoden für alle Daten Vorgänge für Rollen bereitstellt. Diese Klasse entspricht der [rolestore-&lt;trole-&gt;](https://msdn.microsoft.com/library/dn468181(v=vs.108).aspx) -Klasse im Microsoft. ASP. net. Identity. EntityFramework-Namespace. In ihrer rolestore-Klasse implementieren Sie die [irolestore-&lt;trole, TKey&gt;](https://msdn.microsoft.com/library/dn613266(v=vs.108).aspx) und optional die [iqueryablerolestore&lt;trole, TKey&gt;](https://msdn.microsoft.com/library/dn613262(v=vs.108).aspx) -Schnittstelle.

![](overview-of-custom-storage-providers-for-aspnet-identity/_static/image6.png)

Das folgende Beispiel zeigt eine Rollen Speicher Klasse. Der generische trole-Parameter übernimmt den Typ der Rollen Klasse, bei der es sich normalerweise um die Klasse identityrole handelt, die Sie definiert haben. Der generische TKey-Parameter übernimmt den Typ Ihres Rollen Schlüssels. 

[!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample8.cs)]

- **Irolestore&lt;trole-&gt;**  
  Die [irolestore](https://msdn.microsoft.com/library/dn468195.aspx) -Schnittstelle definiert die Methoden, die in der Rollen Speicher Klasse implementiert werden müssen. Sie enthält Methoden zum Erstellen, aktualisieren, löschen und Abrufen von Rollen.
- **Rolestore&lt;trole&gt;**  
  Zum Anpassen von rolestore erstellen Sie eine Klasse, die die irolestore-Schnittstelle implementiert. Sie müssen diese Klasse nur implementieren, wenn Sie im System Rollen verwenden möchten. Der Konstruktor, der einen Parameter mit dem Namen " *Database* " vom Typ "exampledatabase" annimmt, ist nur ein Beispiel dafür, wie Sie die Datenzugriffs Klasse übergeben. In der MySQL-Implementierung übernimmt dieser Konstruktor z. b. einen Parameter des Typs mysqldatabase.  
  
  Eine komplette Implementierung finden Sie unter [rolestore (MySQL)](https://github.com/aspnet/samples/blob/master/samples/aspnet/Identity/AspNet.Identity.MySQL/RoleStore.cs) .

<a id="reconfigure"></a>
## <a name="reconfigure-application-to-use-new-storage-provider"></a>Neukonfigurieren der Anwendung für die Verwendung eines neuen Speicher Anbieters

Sie haben ihren neuen Speicher Anbieter implementiert. Nun müssen Sie Ihre Anwendung für die Verwendung dieses Speicher Anbieters konfigurieren. Wenn der Standard Speicher Anbieter in Ihrem Projekt enthalten ist, müssen Sie den Standardanbieter entfernen und ihn durch Ihren Anbieter ersetzen.

### <a name="replace-default-storage-provider-in-mvc-project"></a>Standard Speicher Anbieter im MVC-Projekt ersetzen

1. Deinstallieren Sie im Fenster " **nuget-Pakete verwalten** " das **Microsoft ASP.net Identity-EntityFramework** -Paket. Sie finden dieses Paket, indem Sie in den installierten Paketen nach Identity. EntityFramework suchen.  
    ![](overview-of-custom-storage-providers-for-aspnet-identity/_static/image7.png) Sie werden gefragt, ob Sie auch Entity Framework deinstallieren möchten. Wenn Sie es nicht in anderen Teilen der Anwendung benötigen, können Sie es deinstallieren.
2. Löschen Sie in der Datei IdentityModels.cs im Ordner Models die Klassen **ApplicationUser** und **applicationdbcontext** , oder kommentieren Sie Sie aus. In einer MVC-Anwendung können Sie die gesamte IdentityModels.cs-Datei löschen. Löschen Sie in einer Web Forms Anwendung die beiden Klassen, aber stellen Sie sicher, dass Sie die Hilfsklasse behalten, die sich auch in der IdentityModels.cs-Datei befindet.
3. Wenn sich der Speicher Anbieter in einem separaten Projekt befindet, fügen Sie in der Webanwendung einen Verweis darauf ein.
4. Ersetzen Sie alle Verweise auf `using Microsoft.AspNet.Identity.EntityFramework;` durch eine using-Anweisung für den Namespace Ihres Speicher Anbieters.
5. Ändern Sie in der **Startup.auth.cs** -Klasse die Methode " **konfigurierungsmethode** " so, dass eine einzelne Instanz des entsprechenden Kontexts verwendet wird. 

    [!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample9.cs?highlight=3)]
6. Öffnen Sie im Ordner App\_Start den Ordner **IdentityConfig.cs**. Ändern Sie in der applicationusermanager-Klasse die **Create** -Methode, um einen Benutzer-Manager zurückzugeben, der den benutzerdefinierten Benutzerspeicher verwendet. 

    [!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample10.cs?highlight=3)]
7. Ersetzen Sie alle Verweise auf **ApplicationUser** durch **identityuser**.
8. Das Standard Projekt enthält einige Member in der Benutzerklasse, die nicht in der iuser-Schnittstelle definiert sind. z. b. e-Mail, PasswordHash und generateuseridentityasync. Wenn Ihre Benutzerklasse nicht über diese Member verfügt, müssen Sie Sie entweder implementieren oder den Code ändern, der diese Member verwendet.
9. Wenn Sie Instanzen von roleManager erstellt haben, ändern Sie den Code so, dass die neue rolestore-Klasse verwendet wird.  

    [!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample11.cs)]
10. Das Standard Projekt ist für eine Benutzerklasse konzipiert, die über einen Zeichen folgen Wert für den Schlüssel verfügt. Wenn Ihre Benutzerklasse einen anderen Typ für den Schlüssel aufweist (z. b. eine ganze Zahl), müssen Sie das Projekt so ändern, dass es mit dem Typ funktioniert. Weitere Informationen finden Sie [unter Ändern des Primärschlüssels für Benutzer in ASP.net Identity](change-primary-key-for-users-in-aspnet-identity.md).
11. Fügen Sie bei Bedarf die Verbindungs Zeichenfolge der Datei "Web. config" hinzu.

<a id="other"></a>
## <a name="other-resources"></a>Weitere Ressourcen

- Blog: [Implementieren von ASP.net Identity](http://odetocode.com/blogs/scott/archive/2014/01/20/implementing-asp-net-identity.aspx)
- Tutorial und git-Code: [Simple. Data ASP.net Identitäts Anbieter](http://designcoderelease.blogspot.co.uk/2015/03/simpledata-aspnet-identity-provider.html)
- Tutorial:[Einrichten der grundlegenden Identitäts Konten und verweisen auf eine externe](http://typecastexception.com/post/2013/10/27/Configuring-Db-Connection-and-Code-First-Migration-for-Identity-Accounts-in-ASPNET-MVC-5-and-Visual-Studio-2013.aspx)Datenbank Nach [@xivSolutions](https://twitter.com/xivSolutions).
- Tutorial[: Implementieren eines benutzerdefinierten MySQL-ASP.net Identity Speicher Anbieters](implementing-a-custom-mysql-aspnet-identity-storage-provider.md)
- [Codefließend-Entitäten](http://blog.codefluententities.com/2014/04/30/asp-net-identity-v2-and-codefluent-entities/) durch [Software](http://www.softfluent.com/)
- [Azure Table Storage](https://www.nuget.org/packages/accidentalfish.aspnet.identity.azure/) von James Randall.
- Azure-Table Storage: [ASPNET. Identity. TableStorage](https://github.com/stuartleeks/leeksnet.AspNet.Identity.TableStorage) durch [@stuartleeks](https://twitter.com/stuartleeks).
- [Couchdb/cloudant von Daniel Wertheim.](https://github.com/danielwertheim/mycouch.aspnet.identity)
- Elastische Searc[h: elastische Identität](https://github.com/bmbsqd/elastic-identity) von bombsquad ab.
- [MongoDB](http://www.nuget.org/packages/MongoDB.AspNet.Identity/) von Jonathan sheely Jonathan sheely.
- [NHibernate. Aspnet. Identity](https://github.com/milesibastos/NHibernate.AspNet.Identity) von Antônio Milesi Bastos.
- [Ravendb](http://www.nuget.org/packages/AspNet.Identity.RavenDB/1.0.0) nach [@tourismgeek](https://twitter.com/tourismgeek).
- [Ravendb. Aspnet. Identity](https://github.com/ILMServices/RavenDB.AspNet.Identity) durch [ilmservices](http://www.ilmservice.com/).
- Redis: [redis. Aspnet. Identity](https://github.com/aminjam/Redis.AspNet.Identity)
- T4-Vorlagen zum Generieren von EF-Code für einen "Database First"-Benutzerspeicher: [ASPNET. Identity. EntityFramework](https://github.com/cbfrank/AspNet.Identity.EntityFramework)
