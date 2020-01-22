---
uid: identity/overview/extensibility/change-primary-key-for-users-in-aspnet-identity
title: Ändern des Primärschlüssels für Benutzer in ASP.net Identity-ASP.NET 4. x
author: Rick-Anderson
description: In Visual Studio 2013 verwendet die Standardweb Anwendung einen Zeichen folgen Wert für den Schlüssel für Benutzerkonten. Mit ASP.net Identity können Sie den Typ des...
ms.author: riande
ms.date: 09/30/2014
ms.assetid: 44925849-5762-4504-a8cd-8f0cd06f6dc3
ms.custom: seoapril2019
msc.legacyurl: /identity/overview/extensibility/change-primary-key-for-users-in-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: 0afea8eacfc646f1489b87629fdb2d437815d88c
ms.sourcegitcommit: 88fc80e3f65aebdf61ec9414810ddbc31c543f04
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/22/2020
ms.locfileid: "76519140"
---
# <a name="change-primary-key-for-users-in-aspnet-identity"></a>Ändern des Primärschlüssels für Benutzer in ASP.NET Identity

durch [Tom FitzMacken](https://github.com/tfitzmac)

> In Visual Studio 2013 verwendet die Standardweb Anwendung einen Zeichen folgen Wert für den Schlüssel für Benutzerkonten. ASP.net Identity ermöglicht es Ihnen, den Typ des Schlüssels zu ändern, um Ihre Datenanforderungen zu erfüllen. Beispielsweise können Sie den Typ des Schlüssels von einer Zeichenfolge in eine ganze Zahl ändern.
> 
> In diesem Thema wird gezeigt, wie Sie mit der Standardweb Anwendung beginnen und den Benutzerkonto Schlüssel in eine ganze Zahl ändern. Sie können dieselben Änderungen verwenden, um beliebige Typen von Schlüsseln in Ihrem Projekt zu implementieren. Es wird gezeigt, wie diese Änderungen in der Standardweb Anwendung vorgenommen werden, aber Sie können ähnliche Änderungen auf eine angepasste Anwendung anwenden. Es zeigt die Änderungen, die bei der Arbeit mit MVC oder Web Forms erforderlich sind.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Im Tutorial verwendete Software Versionen
> 
> 
> - Visual Studio 2013 mit Update 2 (oder höher)
> - ASP.net Identity 2,1 oder höher

Damit Sie die Schritte in diesem Tutorial ausführen können, müssen Sie über Visual Studio 2013 Update 2 (oder höher) und eine Webanwendung verfügen, die aus der ASP.net-Webanwendungsvorlage erstellt wurde. Die Vorlage wurde in Update 3 geändert. In diesem Thema wird gezeigt, wie Sie die Vorlage in Update 2 und Update 3 ändern.

Dieses Thema enthält folgende Abschnitte:

- [Ändern Sie den Typ des Schlüssels in der Identity-Benutzerklasse.](#userclass)
- [Hinzufügen von angepassten Identitäts Klassen, die den Schlüsseltyp verwenden](#customclass)
- [Ändern der Kontext Klasse und des Benutzer-Managers, um den Schlüsseltyp zu verwenden](#context)
- [Ändern Sie die Startkonfiguration, sodass der Schlüsseltyp verwendet wird.](#startup)
- [Ändern Sie für MVC mit Update 2 den Kontotyp AccountController, um den Schlüsseltyp zu übergeben.](#mvcupdate2)
- [Ändern Sie für MVC mit Update 3 AccountController und managecontroller so, dass der Schlüsseltyp übergeben wird.](#mvcupdate3)
- [Ändern Sie für Web Forms mit Update 2 die Konto Seiten so, dass der Schlüsseltyp übergeben wird.](#webformsupdate2)
- [Ändern Sie für Web Forms mit Update 3 die Konto Seiten so, dass der Schlüsseltyp übergeben wird.](#webformsupdate3)
- [Anwendung ausführen](#run)
- [Andere Ressourcen](#other)

<a id="userclass"></a>
## <a name="change-the-type-of-the-key-in-the-identity-user-class"></a>Ändern Sie den Typ des Schlüssels in der Identity-Benutzerklasse.

Geben Sie in Ihrem Projekt, das aus der Vorlage ASP.NET-Webanwendung erstellt wurde, an, dass die ApplicationUser-Klasse eine Ganzzahl für den Schlüssel für Benutzerkonten verwendet. Ändern Sie in IdentityModels.cs die ApplicationUser-Klasse so, dass Sie von identityuser geerbt wird, der den Typ **int** für den generischen TKey-Parameter aufweist. Außerdem übergeben Sie die Namen von drei angepassten Klassen, die Sie noch nicht implementiert haben.

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample1.cs?highlight=1-2)]

Sie haben den Typ des Schlüssels geändert, aber standardmäßig geht der Rest der Anwendung davon aus, dass der Schlüssel eine Zeichenfolge ist. Sie müssen den Typ des Schlüssels in Code, der eine Zeichenfolge annimmt, explizit angeben.

Ändern Sie in der **ApplicationUser** -Klasse die **generateuseridentityasync** -Methode in "int", wie im folgenden hervorgehobenen Code gezeigt. Diese Änderung ist für Web Forms Projekte mit der Vorlage Update 3 nicht erforderlich.

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample2.cs?highlight=2)]

<a id="customclass"></a>
## <a name="add-customized-identity-classes-that-use-the-key-type"></a>Hinzufügen von angepassten Identitäts Klassen, die den Schlüsseltyp verwenden

Die anderen Identitäts Klassen, z. b. identityuserrole, identityuserclaim, identityuserlogin, identityrole, userstore, rolestore, sind immer noch für die Verwendung eines Zeichen folgen Schlüssels festgelegt. Erstellen Sie neue Versionen dieser Klassen, die eine ganze Zahl für den Schlüssel angeben. Sie müssen in diesen Klassen nicht viel Implementierungs Code bereitstellen, Sie legen primär einfach int als Schlüssel fest.

Fügen Sie der IdentityModels.cs-Datei die folgenden Klassen hinzu.

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample3.cs)]

<a id="context"></a>
## <a name="change-the-context-class-and-user-manager-to-use-the-key-type"></a>Ändern der Kontext Klasse und des Benutzer-Managers, um den Schlüsseltyp zu verwenden

Ändern Sie in IdentityModels.cs die Definition der **applicationdbcontext** -Klasse so, dass die neuen angepassten Klassen und ein **int** für den Schlüssel verwendet werden, wie im hervorgehobenen Code gezeigt.

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample4.cs?highlight=1-2)]

Der ThrowIfV1Schema-Parameter ist im Konstruktor nicht mehr gültig. Ändern Sie den Konstruktor, sodass er keinen ThrowIfV1Schema-Wert übergibt.

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample5.cs)]

Öffnen Sie IdentityConfig.cs, und ändern Sie die **applicationusermanager** -Klasse so, dass Ihre neue Benutzerspeicher Klasse zum Beibehalten von Daten und eine **int** -Taste für den Schlüssel verwendet wird.

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample6.cs?highlight=1,3,12,14,32,37,48)]

In der Vorlage Update 3 müssen Sie die applicationsigninmanager-Klasse ändern.

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample7.cs?highlight=1)]

<a id="startup"></a>
## <a name="change-start-up-configuration-to-use-the-key-type"></a>Ändern Sie die Startkonfiguration, sodass der Schlüsseltyp verwendet wird.

Ersetzen Sie in Startup.auth.cs den onvalidateidentity-Code, wie unten gezeigt. Beachten Sie, dass die getuseridcallback-Definition den Zeichen folgen Wert in eine ganze Zahl analysiert.

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample8.cs?highlight=7-12)]

Wenn das Projekt die generische Implementierung der **GetUserID** -Methode nicht erkennt, müssen Sie möglicherweise das ASP.net Identity nuget-Paket auf Version 2,1 aktualisieren.

Sie haben viele Änderungen an den von ASP.net Identity verwendeten Infrastruktur Klassen vorgenommen. Wenn Sie versuchen, das Projekt zu kompilieren, werden viele Fehler feststellen. Glücklicherweise sind die restlichen Fehler alle ähnlich. Die Identity-Klasse erwartet eine ganze Zahl für den Schlüssel, aber der Controller (oder das Webformular) übergibt einen Zeichen folgen Wert. In jedem Fall müssen Sie eine Konvertierung von einer Zeichenfolge in eine ganze Zahl und eine ganze Zahl durch Aufrufen von **GetUserID&lt;int&gt;** durchsetzen. Sie können entweder die Fehlerliste aus der Kompilierung bearbeiten oder die unten aufgeführten Änderungen befolgen.

Die restlichen Änderungen hängen vom Projekttyp ab, den Sie erstellen, und von dem Update, das Sie in Visual Studio installiert haben. Sie können über die folgenden Links direkt zum entsprechenden Abschnitt wechseln.

- [Ändern Sie für MVC mit Update 2 den Kontotyp AccountController, um den Schlüsseltyp zu übergeben.](#mvcupdate2)
- [Ändern Sie für MVC mit Update 3 AccountController und managecontroller so, dass der Schlüsseltyp übergeben wird.](#mvcupdate3)
- [Ändern Sie für Web Forms mit Update 2 die Konto Seiten so, dass der Schlüsseltyp übergeben wird.](#webformsupdate2)
- [Ändern Sie für Web Forms mit Update 3 die Konto Seiten so, dass der Schlüsseltyp übergeben wird.](#webformsupdate3)

<a id="mvcupdate2"></a>
## <a name="for-mvc-with-update-2-change-the-accountcontroller-to-pass-the-key-type"></a>Ändern Sie für MVC mit Update 2 den Kontotyp AccountController, um den Schlüsseltyp zu übergeben.

Öffnen Sie die Datei AccountController.cs. Sie müssen die folgenden Methoden ändern.

**Confirmemail** -Methode

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample9.cs?highlight=1,3)]

Methode **diszuordnen**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample10.cs?highlight=5,9)]

**Manage (manageuserviewmodel)** -Methode

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample11.cs?highlight=11,17,41)]

**Linklogincallback** -Methode

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample12.cs?highlight=10)]

**Removeaccountlist** -Methode

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample13.cs?highlight=3)]

**HasPassword** -Methode

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample14.cs?highlight=3)]

Sie können jetzt [die Anwendung ausführen](#run) und einen neuen Benutzer registrieren.

<a id="mvcupdate3"></a>
## <a name="for-mvc-with-update-3-change-the-accountcontroller-and-managecontroller-to-pass-the-key-type"></a>Ändern Sie für MVC mit Update 3 AccountController und managecontroller so, dass der Schlüsseltyp übergeben wird.

Öffnen Sie die Datei AccountController.cs. Sie müssen die folgende Methode ändern.

**Confirmemail** -Methode

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample15.cs?highlight=1,3)]

**Sendcode** -Methode

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample16.cs?highlight=4)]

Öffnen Sie die Datei ManageController.cs. Sie müssen die folgenden Methoden ändern.

**Index** -Methode

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample17.cs?highlight=15-17)]

**Removelogin** -Methoden

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample18.cs?highlight=3,13,17)]

**Addphonenumber** -Methode

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample19.cs?highlight=9)]

**Enabletwofactorauthentication** -Methode

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample20.cs?highlight=3-4)]

**Disabletwofactorauthentication** -Methode

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample21.cs?highlight=3-4)]

**Verifyphonenumber** -Methoden

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample22.cs?highlight=4,18,21)]

**Removephonenumber** -Methode

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample23.cs?highlight=3,8)]

**ChangePassword** -Methode

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample24.cs?highlight=10,13)]

**SetPassword** -Methode

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample25.cs?highlight=5,8)]

**Managelogins** -Methode

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample26.cs?highlight=7,12)]

**Linklogincallback** -Methode

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample27.cs?highlight=8)]

**HasPassword** -Methode

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample28.cs?highlight=3)]

**Hasphonenumber** -Methode

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample29.cs?highlight=3)]

Sie können jetzt [die Anwendung ausführen](#run) und einen neuen Benutzer registrieren.

<a id="webformsupdate2"></a>
## <a name="for-web-forms-with-update-2-change-account-pages-to-pass-the-key-type"></a>Ändern Sie für Web Forms mit Update 2 die Konto Seiten so, dass der Schlüsseltyp übergeben wird.

Für Web Forms mit Update 2 müssen Sie die folgenden Seiten ändern.

**Confirm.aspx.cx**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample30.cs?highlight=8)]

**RegisterExternalLogin.aspx.cs**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample31.cs?highlight=36)]

**Manage.aspx.cs**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample32.cs?highlight=3,22,47,52,69,85,93,98)]

Sie können jetzt [die Anwendung ausführen](#run) und einen neuen Benutzer registrieren.

<a id="webformsupdate3"></a>
## <a name="for-web-forms-with-update-3-change-account-pages-to-pass-the-key-type"></a>Ändern Sie für Web Forms mit Update 3 die Konto Seiten so, dass der Schlüsseltyp übergeben wird.

Für Web Forms mit Update 3 müssen Sie die folgenden Seiten ändern.

**Confirm.aspx.cx**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample33.cs?highlight=8)]

**RegisterExternalLogin.aspx.cs**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample34.cs?highlight=36)]

**Manage.aspx.cs**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample35.cs?highlight=11,27,32,34,82,87,99,108)]

**VerifyPhoneNumber.aspx.cs**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample36.cs?highlight=8,23,27)]

**AddPhoneNumber.aspx.cs**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample37.cs?highlight=7)]

**ManagePassword.aspx.cs**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample38.cs?highlight=11,47,50,68)]

**ManageLogins.aspx.cs**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample39.cs?highlight=16,23,32,41,45)]

**TwoFactorAuthenticationSignIn.aspx.cs**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample40.cs?highlight=14-15,29,53)]

<a id="run"></a>
## <a name="run-application"></a>Ausführen der Anwendung

Sie haben alle erforderlichen Änderungen an der Standardweb Application-Vorlage abgeschlossen. Führen Sie die Anwendung aus, und registrieren Sie einen neuen Benutzer. Nachdem Sie den Benutzer registriert haben, werden Sie bemerken, dass die aspnettusers-Tabelle eine ID-Spalte aufweist, die eine ganze Zahl ist.

![neuer Primärschlüssel](change-primary-key-for-users-in-aspnet-identity/_static/image1.png)

Wenn Sie zuvor die ASP.net Identity Tabellen mit einem anderen Primärschlüssel erstellt haben, müssen Sie einige zusätzliche Änderungen vornehmen. Löschen Sie die vorhandene Datenbank, wenn möglich. Die Datenbank wird mit dem richtigen Entwurf neu erstellt, wenn Sie die Webanwendung ausführen und einen neuen Benutzer hinzufügen. Wenn das Löschen nicht möglich ist, führen Sie Code First-Migrationen aus, um die Tabellen zu ändern. Der neue ganzzahlige Primärschlüssel wird jedoch nicht als SQL-Identitäts Eigenschaft in der Datenbank eingerichtet. Sie müssen die ID-Spalte manuell als Identität festlegen.

<a id="other"></a>
## <a name="other-resources"></a>Weitere Ressourcen

- [Übersicht über benutzerdefinierte Speicheranbieter für ASP.NET Identity](overview-of-custom-storage-providers-for-aspnet-identity.md)
- [Migrieren einer vorhandenen Website von einem SQL-Mitgliedschaftsanbieter nach ASP.NET Identity](../migrations/migrating-an-existing-website-from-sql-membership-to-aspnet-identity.md)
- [Migrieren von universellen Anbieter Daten für Mitgliedschafts-und Benutzerprofile zu ASP.net Identity](../migrations/migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity.md)
- [Beispielanwendung](https://github.com/aspnet/samples/tree/master/samples/aspnet/Identity/ChangePK) mit geändertem Primärschlüssel
