---
uid: mvc/overview/older-versions/using-oauth-providers-with-mvc
title: Verwenden von OAuth-Anbietern mit MVC 4 | Microsoft-Dokumentation
author: Rick-Anderson
description: In diesem Tutorial wird gezeigt, wie Sie eine ASP.NET MVC 4-Webanwendung erstellen, die es Benutzern ermöglicht, sich mit Anmelde Informationen eines externen Anbieters anzumelden, z. b. facebo...
ms.author: riande
ms.date: 06/19/2013
ms.assetid: 7a87f16f-0e19-4f15-a88a-094ae866c4a2
msc.legacyurl: /mvc/overview/older-versions/using-oauth-providers-with-mvc
msc.type: authoredcontent
ms.openlocfilehash: 5dfd1305376a62f4987caea242ca0f6aac1018e9
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78433365"
---
# <a name="using-oauth-providers-with-mvc-4"></a>Verwenden von OAuth-Anbietern mit MVC 4

von [Tom fitzmacken](https://github.com/tfitzmac)

> In diesem Tutorial wird gezeigt, wie Sie eine ASP.NET MVC 4-Webanwendung erstellen, die es Benutzern ermöglicht, sich mit Anmelde Informationen eines externen Anbieters anzumelden, wie z. b. Facebook, Twitter, Microsoft oder Google, und dann einige der Funktionen von diesen Anbietern in Ihre Webanwendung. Der Einfachheit halber konzentriert sich dieses Tutorial auf das Arbeiten mit Anmelde Informationen von Facebook.
> 
> Informationen zur Verwendung externer Anmelde Informationen in einer ASP.NET MVC 5-Webanwendung finden Sie [unter Erstellen einer ASP.NET MVC 5-App mit Facebook und Google OAuth2 und OpenID-Anmeldung](../security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).
> 
> Die Aktivierung dieser Anmelde Informationen auf ihren Websites bietet einen erheblichen Vorteil, da Millionen von Benutzern bereits über Konten mit diesen externen Anbietern verfügen. Diese Benutzer sind möglicherweise eher geneigt, sich für Ihre Website zu registrieren, wenn Sie keinen neuen Satz von Anmelde Informationen erstellen und speichern müssen. Nachdem sich ein Benutzer über einen dieser Anbieter angemeldet hat, können Sie auch soziale Vorgänge des Anbieters integrieren.

## <a name="what-youll-build"></a>Was Sie erstellen

In diesem Tutorial gibt es zwei Hauptziele:

1. Ermöglicht es Benutzern, sich mit Anmelde Informationen eines OAuth-Anbieters anzumelden.
2. Rufen Sie Kontoinformationen vom Anbieter ab, und integrieren Sie diese Informationen mit der Kontoregistrierung für Ihren Standort.

Obwohl sich die Beispiele in diesem Tutorial auf die Verwendung von Facebook als Authentifizierungs Anbieter konzentrieren, können Sie den Code so ändern, dass er einen beliebigen Anbieter verwendet. Die Schritte zum Implementieren eines beliebigen Anbieters ähneln den Schritten, die Sie in diesem Tutorial sehen werden. Sie werden nur signifikante Unterschiede bemerken, wenn Sie direkte Aufrufe an den API-Satz des Anbieters senden.

## <a name="prerequisites"></a>Voraussetzungen

- [Microsoft Visual Studio 2012](https://www.microsoft.com/visualstudio/eng/downloads#vs) oder [Microsoft Visual Studio Express 2012 für Web](https://www.microsoft.com/visualstudio/eng/downloads#d-2012-express)

Oder

- Microsoft Visual Studio 2010 SP1 oder [Visual Web Developer Express 2010 SP1](https://www.microsoft.com/visualstudio/eng/downloads#d-2010-express)
- [ASP.NET MVC 4](https://go.microsoft.com/fwlink/?LinkId=243392)

Außerdem wird in diesem Thema davon ausgegangen, dass Sie über grundlegende Kenntnisse über ASP.NET MVC und Visual Studio verfügen. Wenn Sie eine Einführung in ASP.NET MVC 4 benötigen, finden Sie weitere Informationen unter Einführung [in ASP.NET MVC 4](getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4.md).

## <a name="create-the-project"></a>Erstellen eines Projekts

Erstellen Sie in Visual Studio eine neue ASP.NET MVC 4-Webanwendung, und benennen Sie Sie &quot;oauthmvc-&quot;. Sie können entweder 4,5 oder 4 als Ziel .NET Framework.

![Erstellen des Projekts](using-oauth-providers-with-mvc/_static/image1.png)

Wählen Sie im Fenster Neues ASP.NET MVC 4-Projekt die Option **Internet Anwendung** aus, und lassen Sie **Razor** als Ansichts-Engine.

![Internet Anwendung auswählen](using-oauth-providers-with-mvc/_static/image2.png)

## <a name="enable-a-provider"></a>Aktivieren eines Anbieters

Wenn Sie eine MVC 4-Webanwendung mit der Internet Anwendungs Vorlage erstellen, wird das Projekt mit einer Datei namens AuthConfig.cs im Ordner App\_Start erstellt.

![AuthConfig-Datei](using-oauth-providers-with-mvc/_static/image3.png)

Die AuthConfig-Datei enthält Code zum Registrieren von Clients für externe Authentifizierungs Anbieter. Standardmäßig ist dieser Code auskommentiert, sodass keiner der externen Anbieter aktiviert ist.

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample1.cs)]

Sie müssen die Auskommentierung dieses Codes aufheben, um den externen Authentifizierungs Client zu verwenden. Entfernen Sie nur die Anbieter, die Sie in Ihre Website einschließen möchten. Für dieses Tutorial aktivieren Sie nur die Facebook-Anmelde Informationen.

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample2.cs)]

Beachten Sie im obigen Beispiel, dass die-Methode leere Zeichen folgen für die Registrierungs Parameter enthält. Wenn Sie versuchen, die Anwendung jetzt auszuführen, löst die Anwendung eine Argument Ausnahme aus, da leere Zeichen folgen für die Parameter nicht zulässig sind. Um gültige Werte anzugeben, müssen Sie die Website bei den externen Anbietern registrieren, wie im nächsten Abschnitt gezeigt.

## <a name="registering-with-an-external-provider"></a>Registrieren bei einem externen Anbieter

Um Benutzer mit Anmelde Informationen eines externen Anbieters zu authentifizieren, müssen Sie die Website beim Anbieter registrieren. Wenn Sie Ihre Website registrieren, erhalten Sie die Parameter (z. b. Schlüssel oder ID und geheimer Schlüssel), die beim Registrieren des Clients eingeschlossen werden sollen. Sie müssen über ein Konto mit den Anbietern verfügen, die Sie verwenden möchten.

In diesem Tutorial werden nicht alle Schritte erläutert, die Sie ausführen müssen, um sich bei diesen Anbietern zu registrieren. Die Schritte sind in der Regel nicht schwierig. Befolgen Sie die Anweisungen auf diesen Standorten, um Ihre Website erfolgreich zu registrieren. Informationen zu den ersten Schritten beim Registrieren Ihrer Website finden Sie auf der Entwickler Website für:

- [Facebook](https://developers.facebook.com/)
- [Google](https://developers.google.com/)
- [Microsoft](http://manage.dev.live.com/)
- [Twitter](https://dev.twitter.com/)

Wenn Sie Ihre Website bei Facebook registrieren, können Sie &quot;localhost-&quot; für die Website Domäne bereitstellen und für die URL `&quot; http://localhost/&quot;`, wie in der folgenden Abbildung dargestellt. Die Verwendung von localhost funktioniert mit den meisten Anbietern, funktioniert aber zurzeit nicht mit dem Microsoft-Anbieter. Für den Microsoft-Anbieter müssen Sie eine gültige URL für die Website einschließen.

![Website registrieren](using-oauth-providers-with-mvc/_static/image4.png)

In der vorherigen Abbildung wurden die Werte für die APP-ID, den geheimen Schlüssel der APP und die Kontakt-e-Mail entfernt. Wenn Sie Ihre Website tatsächlich registrieren, werden diese Werte angezeigt. Sie sollten die Werte für die APP-ID und den geheimen App-Schlüssel notieren, da Sie Sie der Anwendung hinzufügen.

## <a name="creating-test-users"></a>Erstellen von Test Benutzern

Wenn Sie kein vorhandenes Facebook-Konto zum Testen Ihrer Website verwenden, können Sie diesen Abschnitt überspringen.

Auf der Seite für die Verwaltung von Facebook-Apps können Sie problemlos Test Benutzer für Ihre Anwendung erstellen. Sie können diese Testkonten verwenden, um sich bei Ihrer Website anzumelden. Sie erstellen Test Benutzer, indem Sie im linken Navigationsbereich auf den Link **Rollen** und dann auf den Link **Erstellen** klicken.

![Test Benutzer erstellen](using-oauth-providers-with-mvc/_static/image5.png)

Auf der Facebook-Website wird die Anzahl der von Ihnen angeforderten Testkonten automatisch erstellt.

## <a name="adding-application-id-and-secret-from-the-provider"></a>Hinzufügen von Anwendungs-ID und geheimer Schlüssel vom Anbieter

Nachdem Sie die ID und den geheimen Schlüssel von Facebook erhalten haben, wechseln Sie zurück zur AuthConfig-Datei, und fügen Sie Sie als Parameterwerte hinzu. Die unten gezeigten Werte sind keine echten Werte.

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample3.cs)]

## <a name="log-in-with-external-credentials"></a>Mit externen Anmelde Informationen anmelden

Das ist alles, was Sie tun müssen, um externe Anmelde Informationen auf Ihrer Website zu aktivieren. Führen Sie die Anwendung aus, und klicken Sie auf den Anmeldelink in der oberen rechten Ecke. Die Vorlage erkennt automatisch, dass Sie Facebook als Anbieter registriert haben, und schließt eine Schaltfläche für den Anbieter ein. Wenn Sie mehrere Anbieter registrieren, wird automatisch eine Schaltfläche eingefügt.

![externer Anmelde Name](using-oauth-providers-with-mvc/_static/image6.png)

In diesem Tutorial wird nicht erläutert, wie die Anmelde Schaltflächen für die externen Anbieter angepasst werden. Diese Informationen finden Sie unter [Anpassen der Anmelde Benutzeroberfläche bei Verwendung von OAuth/OpenID](https://blogs.msdn.com/b/pranav_rastogi/archive/2012/08/24/customizing-the-login-ui-when-using-oauth-openid.aspx).

Klicken Sie auf die Facebook-Schaltfläche, um sich mit Facebook-Anmelde Informationen anzumelden. Wenn Sie einen der externen Anbieter auswählen, werden Sie zu dieser Site umgeleitet und von diesem Dienst aufgefordert, sich anzumelden.

Die folgende Abbildung zeigt den Anmeldebildschirm für Facebook. Sie werden feststellen, dass Sie Ihr Facebook-Konto verwenden, um sich bei einer Website mit dem Namen oauthmvcexample anzumelden.

![Facebook-Authentifizierung](using-oauth-providers-with-mvc/_static/image7.png)

Nach der Anmeldung mit Facebook-Anmelde Informationen wird der Benutzer auf einer Seite informiert, dass die Website Zugriff auf grundlegende Informationen hat.

![Berechtigung anfordern](using-oauth-providers-with-mvc/_static/image8.png)

Nachdem Sie die Option **Gehe zu app**ausgewählt haben, muss sich der Benutzer für die Website registrieren. Die folgende Abbildung zeigt die Registrierungsseite, nachdem sich ein Benutzer mit Facebook-Anmelde Informationen angemeldet hat. Der Benutzername ist in der Regel mit dem Namen des Anbieters vorab ausgefüllt.

![Registrieren](using-oauth-providers-with-mvc/_static/image9.png)

Klicken Sie auf **registrieren** , um die Registrierung abzuschließen. Schließen Sie den Browser.

Sie können sehen, dass das neue Konto der Datenbank hinzugefügt wurde. Öffnen Sie in Server-Explorer die Datenbank **DefaultConnection** , und öffnen Sie den Ordner **Tabellen** .

![Datenbanktabellen](using-oauth-providers-with-mvc/_static/image10.png)

Klicken Sie mit der rechten Maustaste auf die Tabelle **UserProfile** , und wählen Sie **Tabellendaten anzeigen**.

![Daten anzeigen](using-oauth-providers-with-mvc/_static/image11.png)

Das neue Konto, das Sie hinzugefügt haben, wird angezeigt. Sehen Sie sich die Daten auf der **Webseite\_oauthmembership** -Tabelle an. Es werden weitere Daten im Zusammenhang mit dem externen Anbieter für das soeben hinzugefügte Konto angezeigt.

Wenn Sie nur die externe Authentifizierung aktivieren möchten, sind Sie damit abgeschlossen. Sie können jedoch weitere Informationen vom Anbieter in den neuen Benutzer Registrierungsprozess integrieren, wie in den folgenden Abschnitten dargestellt.

## <a name="create-models-for-additional-user-information"></a>Erstellen von Modellen für weitere Benutzerinformationen

Wie Sie in den vorherigen Abschnitten bemerkt haben, müssen Sie keine zusätzlichen Informationen abrufen, damit die integrierte Kontoregistrierung funktioniert. Bei den meisten externen Anbietern werden jedoch zusätzliche Informationen über den Benutzer zurückgegeben. In den folgenden Abschnitten wird gezeigt, wie Sie diese Informationen beibehalten und in einer-Datenbank speichern. Insbesondere behalten Sie Werte für den vollständigen Namen des Benutzers, den URI der persönlichen Webseite des Benutzers und einen Wert, der angibt, ob Facebook das Konto überprüft hat.

Sie verwenden [Code First-Migrationen](https://msdn.microsoft.com/data/jj591621) , um eine Tabelle zum Speichern zusätzlicher Benutzerinformationen hinzuzufügen. Wenn Sie die Tabelle einer vorhandenen Datenbank hinzufügen, müssen Sie zunächst eine Momentaufnahme der aktuellen Datenbank erstellen. Wenn Sie eine Momentaufnahme der aktuellen Datenbank erstellen, können Sie später eine Migration erstellen, die nur die neue Tabelle enthält. So erstellen Sie eine Momentaufnahme der aktuellen Datenbank:

1. Öffnen der **Paket-Manager-Konsole**
2. Führen Sie den Befehl **enable-Migrationen** aus.
3. Führen Sie den Befehl **Add-Migration Initial – ignorechanges** aus.
4. Ausführen des Befehls " **Update-Database** "

Nun fügen Sie die neuen Eigenschaften hinzu. Öffnen Sie im Ordner Models die Datei AccountModels.cs, und suchen Sie die registerexternzumodel-Klasse. Die registerexternzuweisung Model-Klasse enthält Werte, die vom Authentifizierungs Anbieter zurückgegeben werden. Fügen Sie die Eigenschaften "FullName" und "Link" wie unten gezeigt hinzu.

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample4.cs?highlight=9-13)]

Fügen Sie außerdem in AccountModels.cs eine neue Klasse mit dem Namen extrauserinformation hinzu. Diese Klasse stellt die neue Tabelle dar, die in der Datenbank erstellt wird.

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample5.cs)]

Fügen Sie in der userscontext-Klasse den unten markierten Code hinzu, um eine dbset-Eigenschaft für die neue Klasse zu erstellen.

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample6.cs?highlight=9)]

Nun können Sie die neue Tabelle erstellen. Öffnen Sie die Paket-Manager-Konsole erneut und dieses Mal:

1. Führen Sie den Befehl **Add-Migration addextrauserinformation** aus.
2. Ausführen des Befehls " **Update-Database** "

Die neue Tabelle ist jetzt in der Datenbank vorhanden.

## <a name="retrieve-the-additional-data"></a>Abrufen der zusätzlichen Daten

Es gibt zwei Möglichkeiten, zusätzliche Benutzerdaten abzurufen. Die erste Möglichkeit besteht darin, Benutzerdaten beizubehalten, die während der Authentifizierungsanforderung standardmäßig zurückgegeben werden. Die zweite Möglichkeit besteht darin, die Anbieter-API speziell aufzurufen und weitere Informationen anzufordern. Werte für FullName und Link werden automatisch von Facebook zurückgegeben. Ein Wert, der angibt, ob Facebook überprüft hat, ob das Konto durch einen-Rückruf der Facebook-API abgerufen wird. Zuerst werden Werte für FullName und Link aufgefüllt, und später wird der überprüfte Wert angezeigt.

Öffnen Sie die Datei **AccountController.cs** im Ordner **Controllers** , um die zusätzlichen Benutzerdaten abzurufen.

Diese Datei enthält die Logik zum Protokollieren, registrieren und Verwalten von Konten. Beachten Sie insbesondere die Methoden **externzurückruf** und **externzugbestätigung**. Innerhalb dieser Methoden können Sie Code hinzufügen, um externe Anmeldevorgänge für Ihre Anwendung anzupassen. Die erste Zeile der **externzucallback** -Methode enthält Folgendes:

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample7.cs)]

Zusätzliche Benutzerdaten werden in der **ExtraData** -Eigenschaft des **authenticationresult** -Objekts zurückgegeben, das von der **verifyauthentication** -Methode zurückgegeben wird. Der Facebook-Client enthält die folgenden Werte in der **ExtraData** -Eigenschaft:

- ID
- name
- link
- gender
- Access Token

Andere Anbieter weisen ähnliche, aber etwas andere Daten in der ExtraData-Eigenschaft auf.

Wenn der Benutzer mit der Website neu ist, rufen Sie einige zusätzliche Daten ab, und übergeben Sie diese Daten an die Bestätigungs Ansicht. Der letzte Codeblock in der-Methode wird nur ausgeführt, wenn der Benutzer mit der Website neu ist. Ersetzen Sie die folgende Zeile:

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample8.cs)]

Durch diese Zeile:

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample9.cs)]

Diese Änderung schließt nur Werte für die Eigenschaften FullName und Link ein.

Ändern Sie in der Methode **externdepginconfirmation** den Code wie unten gezeigt, um die zusätzlichen Benutzerinformationen zu speichern.

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample10.cs?highlight=4,7-13)]

## <a name="adjusting-the-view"></a>Anpassen der Ansicht

Die zusätzlichen Benutzerdaten, die Sie vom Anbieter abrufen, werden auf der Registrierungsseite angezeigt.

Öffnen Sie im Ordner **views**/**Account** den Ordner **externzuweisungen Confirmation. cshtml**. Fügen Sie unter dem vorhandenen Feld für Benutzername Felder für FullName, Link und PictureLink hinzu.

[!code-cshtml[Main](using-oauth-providers-with-mvc/samples/sample11.cshtml)]

Sie sind jetzt fast bereit, die Anwendung auszuführen und einen neuen Benutzer mit den zusätzlich gespeicherten Informationen zu registrieren. Sie müssen über ein Konto verfügen, das nicht bereits bei dem Standort registriert ist. Sie können entweder ein anderes Testkonto verwenden oder die Zeilen in den Tabellen " **UserProfile** " und " **Webseiten"\_"oauthmembership** " für das Konto, das Sie wieder verwenden möchten, löschen. Wenn Sie diese Zeilen löschen, stellen Sie sicher, dass das Konto erneut registriert wird.

Führen Sie die Anwendung aus, und registrieren Sie den neuen Benutzer. Beachten Sie, dass die Bestätigungsseite dieses Mal mehr Werte enthält.

![Registrieren](using-oauth-providers-with-mvc/_static/image12.png)

Nachdem Sie die Registrierung abgeschlossen haben, schließen Sie den Browser. Suchen Sie in der Datenbank nach den neuen Werten in der Tabelle **extrauserinformation** .

## <a name="install-nuget-package-for-facebook-api"></a>Installieren des nuget-Pakets für die Facebook-API

Facebook stellt eine [API](https://developers.facebook.com/docs/reference/apis/) zur Verfügung, die Sie zum Ausführen von Vorgängen aufzurufen können. Sie können die Facebook-API aufrufen, indem Sie das Senden von HTTP-Anforderungen oder das Installieren eines nuget-Pakets, das das Senden dieser Anforderungen erleichtert, verwenden. Das Verwenden eines nuget-Pakets wird in diesem Tutorial gezeigt, aber das Installieren des nuget-Pakets ist nicht zwingend erforderlich. In diesem Tutorial wird gezeigt, wie das C# Facebook SDK-Paket verwendet wird. Es gibt andere nuget-Pakete, die beim Aufrufen der Facebook-API hilfreich sind.

Wählen Sie im Fenster **nuget-Pakete verwalten** das Facebook C# SDK-Paket aus.

![Paket installieren](using-oauth-providers-with-mvc/_static/image13.png)

Sie verwenden das Facebook C# -SDK zum Aufrufen eines Vorgangs, der das Zugriffs Token für den Benutzer erfordert. Im nächsten Abschnitt wird gezeigt, wie Sie das Zugriffs Token abrufen.

## <a name="retrieve-access-token"></a>Zugriffs Token abrufen

Die meisten externen Anbieter geben ein Zugriffs Token zurück, nachdem die Anmelde Informationen des Benutzers überprüft wurden. Dieses Zugriffs Token ist sehr wichtig, da es Ihnen ermöglicht, Vorgänge aufzurufen, die nur für authentifizierte Benutzer verfügbar sind. Daher ist es erforderlich, das Zugriffs Token abzurufen und zu speichern, wenn Sie mehr Funktionalität bereitstellen möchten.

Abhängig vom externen Anbieter ist das Zugriffs Token möglicherweise nur für einen begrenzten Zeitraum gültig. Um sicherzustellen, dass Sie über ein gültiges Zugriffs Token verfügen, rufen Sie es jedes Mal ab, wenn sich der Benutzer anmeldet, und speichern Sie es als Sitzungs Wert, anstatt es in einer Datenbank zu speichern.

In der **externzurückcallback** -Methode wird das Zugriffs Token auch in der **ExtraData** -Eigenschaft des **authenticationresult** -Objekts zurückgegeben. Fügen Sie den hervorgehobenen Code **externzurückcallback** hinzu, um das Zugriffs Token im **Sitzungs** Objekt zu speichern. Dieser Code wird jedes Mal ausgeführt, wenn sich der Benutzer mit einem Facebook-Konto anmeldet.

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample12.cs?highlight=11-14)]

Obwohl in diesem Beispiel ein Zugriffs Token von Facebook abgerufen wird, können Sie das Zugriffs Token von einem beliebigen externen Anbieter über denselben Schlüssel mit dem Namen &quot;Access Token&quot;abrufen.

## <a name="logging-off"></a>Abmelden

Mit der **Standardmethode** für die Abmeldung wird der Benutzer von der Anwendung abgemeldet, der Benutzer wird jedoch nicht vom externen Anbieter protokolliert. Um den Benutzer von Facebook zu protokollieren und zu verhindern, dass das Token beibehalten wird, nachdem sich der Benutzer abgemeldet hat, können Sie den folgenden hervorgehobenen Code der **Logoff** -Methode in AccountController hinzufügen.

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample13.cs?highlight=6-14)]

Der Wert, den Sie im `next`-Parameter angeben, ist die URL, die verwendet werden soll, nachdem sich der Benutzer von Facebook abgemeldet hat. Wenn Sie auf dem lokalen Computer testen, geben Sie die richtige Portnummer für den lokalen Standort ein. In der Produktionsumgebung geben Sie eine Standardseite an, z. b. contoso.com.

## <a name="retrieve-user-information-that-requires-the-access-token"></a>Abrufen von Benutzerinformationen, für die das Zugriffs Token erforderlich ist

Nachdem Sie nun das Zugriffs Token gespeichert und das Facebook C# SDK-Paket installiert haben, können Sie diese kombinieren, um weitere Benutzerinformationen von Facebook anzufordern. Erstellen Sie in der **externzuzubestätigungs** -Methode eine Instanz der **facebookclient** -Klasse, indem Sie den Wert des Zugriffs Tokens übergeben. Fordert den Wert der **verifizierten** Eigenschaft für den aktuellen authentifizierten Benutzer an. Mit der **verifizierten** Eigenschaft wird angegeben, ob Facebook das Konto auf andere Weise überprüft hat, z. b. das Senden einer Nachricht an ein Mobiltelefon. Speichern Sie diesen Wert in der Datenbank.

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample14.cs?highlight=7-18,25)]

Sie müssen die Datensätze in der Datenbank für den Benutzer entweder löschen oder ein anderes Facebook-Konto verwenden.

Führen Sie die Anwendung aus, und registrieren Sie den neuen Benutzer. Sehen Sie sich die Tabelle **extrauserinformation** an, um den Wert für die überprüfte Eigenschaft anzuzeigen.

## <a name="conclusion"></a>Zusammenfassung

In diesem Tutorial haben Sie eine Website erstellt, die für Benutzerauthentifizierung und Registrierungsdaten in Facebook integriert ist. Sie haben das Standardverhalten kennengelernt, das für die MVC 4-Webanwendung eingerichtet ist, und wie Sie dieses Standardverhalten anpassen.

## <a name="related-topics"></a>Verwandte Themen

- [Erstellen einer ASP.NET MVC-App mit Authentifizierung, SQL-Datenbank und Bereitstellung in Azure App Service](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data)
