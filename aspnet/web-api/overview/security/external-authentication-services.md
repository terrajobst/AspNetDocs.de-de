---
uid: web-api/overview/security/external-authentication-services
title: Externe Authentifizierungsdienste mit ASP.net-Web-API (C#) | Microsoft-Dokumentation
author: rmcmurray
description: Beschreibt die Verwendung externer Authentifizierungsdienste in ASP.net-Web-API.
ms.author: riande
ms.date: 01/28/2019
ms.assetid: 3bb8eb15-b518-44f5-a67d-a27e051aedc6
msc.legacyurl: /web-api/overview/security/external-authentication-services
msc.type: authoredcontent
ms.openlocfilehash: b2571552a3f8040ff42bfa0a9fa48981f71a1e4b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78447417"
---
# <a name="external-authentication-services-with-aspnet-web-api-c"></a>Externe Authentifizierungsdienste mit ASP.net-Web-API (C#)

Visual Studio 2017 und ASP.NET 4.7.2 erweitern Sie die Sicherheitsoptionen für [Single-Page-Anwendungen (Single Page Applications](../../../single-page-application/index.md) , Spa) und Web- [API](../../index.md) -Dienste für die Integration in externe Authentifizierungsdienste. dazu gehören verschiedene OAuth-/OpenID-und Social Media-Authentifizierungsdienste: Microsoft-Konten, Twitter, Facebook und Google.  

### <a name="in-this-walkthrough"></a>In dieser exemplarischen Vorgehensweise

- [Verwenden externer Authentifizierungsdienste](#USING)
- [Erstellen der beispielweb Anwendung](#SAMPLE)
- [Aktivieren der Facebook-Authentifizierung](#FACEBOOK)
- [Aktivieren der Google-Authentifizierung](#GOOGLE)
- [Aktivieren der Microsoft-Authentifizierung](#MICROSOFT)
- [Aktivieren der Twitter-Authentifizierung](#TWITTER)
- [Weitere Informationen](#MOREINFO)

    - [Kombinieren externer Authentifizierungsdienste](#COMBINE)
    - [Konfigurieren von IIS Express für die Verwendung eines voll qualifizierten Domänen Namens](#FQDN)
    - [Abrufen der Anwendungseinstellungen für die Microsoft-Authentifizierung](#OBTAIN)
    - [Optional: lokale Registrierung deaktivieren](#DISABLE)

### <a name="prerequisites"></a>Voraussetzungen

Um die Beispiele in dieser exemplarischen Vorgehensweise befolgen zu können, benötigen Sie Folgendes:

- Visual Studio 2017
- Ein Entwicklerkonto mit dem Anwendungs Bezeichner und dem geheimen Schlüssel für einen der folgenden Social Media-Authentifizierungsdienste:

  - Microsoft-Konten ([https://go.microsoft.com/fwlink/?LinkID=144070](https://go.microsoft.com/fwlink/?LinkID=144070))
  - Twitter ([https://dev.twitter.com/](https://dev.twitter.com/))
  - Facebook ([https://developers.facebook.com/](https://developers.facebook.com/))
  - Google ([https://developers.google.com/](https://developers.google.com))

<a id="USING"></a>
## <a name="using-external-authentication-services"></a>Verwenden externer Authentifizierungsdienste

Die Fülle externer Authentifizierungsdienste, die derzeit für Webentwickler verfügbar sind, trägt dazu bei, die Entwicklungszeit beim Erstellen neuer Webanwendungen zu verkürzen. Webbenutzer verfügen in der Regel über mehrere Konten für beliebte Webdienste und Social Media-Websites. Wenn also eine Webanwendung die Authentifizierungsdienste von einem externen Webdienst oder einer Website für soziale Medien implementiert, spart Sie die Entwicklungszeit, die hätte die Erstellung einer Authentifizierungs Implementierung aufgewendet. Wenn Sie einen externen Authentifizierungsdienst verwenden, müssen die Endbenutzer kein weiteres Konto für Ihre Webanwendung erstellen, sondern auch einen anderen Benutzernamen und ein Kennwort speichern.

In der Vergangenheit hatten Entwickler zwei Möglichkeiten: Erstellen Sie Ihre eigene Authentifizierungs Implementierung, oder erfahren Sie, wie Sie einen externen Authentifizierungsdienst in Ihre Anwendungen integrieren. Das folgende Diagramm veranschaulicht auf der grundlegendsten Ebene einen einfachen Anforderungs Fluss für einen Benutzer-Agent (Webbrowser), der Informationen aus einer Webanwendung anfordert, die für die Verwendung eines externen Authentifizierungs Diensts konfiguriert ist:

[![](external-authentication-services/_static/image2.png "Click to Expand the Image")](external-authentication-services/_static/image1.png)

Im vorangehenden Diagramm führt der Benutzer-Agent (oder Webbrowser in diesem Beispiel) eine Anforderung an eine Webanwendung aus, die den Webbrowser an einen externen Authentifizierungsdienst umleitet. Der Benutzer-Agent sendet seine Anmelde Informationen an den externen Authentifizierungsdienst, und wenn der Benutzer-Agent erfolgreich authentifiziert wurde, leitet der externe Authentifizierungsdienst den Benutzer-Agent zur ursprünglichen Webanwendung mit einer Form von Token weiter, die das der Benutzer-Agent sendet an die Webanwendung. Die Webanwendung verwendet das Token, um zu überprüfen, ob der Benutzer-Agent erfolgreich vom externen Authentifizierungsdienst authentifiziert wurde, und die Webanwendung kann das Token verwenden, um weitere Informationen über den Benutzer-Agent zu sammeln. Nachdem die Anwendung die Informationen des Benutzer-Agents verarbeitet hat, gibt die Webanwendung die entsprechende Antwort an den Benutzer-Agent zurück, basierend auf den Autorisierungs Einstellungen.

Im zweiten Beispiel wird der Benutzer-Agent mit der Webanwendung und dem externen autorisierungsserver verhandelt, und die Webanwendung führt zusätzliche Kommunikation mit dem externen autorisierungsserver durch, um zusätzliche Informationen über den Benutzer abzurufen. Büros

[![](external-authentication-services/_static/image4.png "Click to Expand the Image")](external-authentication-services/_static/image3.png)

Visual Studio 2017 und ASP.NET 4.7.2 vereinfachen Sie die Integration mit externen Authentifizierungsdiensten für Entwickler, indem Sie integrierte Integration für die folgenden Authentifizierungsdienste bereitstellen:

- Facebook
- Google
- Microsoft-Konten (Windows Live ID-Konten)
- Twitter

In den Beispielen in dieser exemplarischen Vorgehensweise wird veranschaulicht, wie die einzelnen unterstützten externen Authentifizierungsdienste mithilfe der neuen ASP.NET-Webanwendungs Vorlage konfiguriert werden, die mit Visual Studio 2017 ausgeliefert wird.

> [!NOTE]
> Gegebenenfalls müssen Sie Ihren FQDN den Einstellungen für den externen Authentifizierungsdienst hinzufügen. Diese Anforderung basiert auf Sicherheitseinschränkungen für einige externe Authentifizierungsdienste, die erfordern, dass der FQDN in ihren Anwendungseinstellungen mit dem FQDN übereinstimmt, der von ihren Clients verwendet wird. (Die Schritte hierfür sind für jeden externen Authentifizierungsdienst sehr unterschiedlich. Sie müssen sich die Dokumentation für jeden externen Authentifizierungsdienst ansehen, um festzustellen, ob dies erforderlich ist und wie diese Einstellungen konfiguriert werden.) Wenn Sie IIS Express für die Verwendung eines FQDN zum Testen dieser Umgebung konfigurieren müssen, lesen Sie den Abschnitt [Konfigurieren IIS Express, um einen voll qualifizierten Domänen Namen zu](#FQDN) verwenden, weiter unten in dieser exemplarischen Vorgehensweise.

<a id="SAMPLE"></a>
## <a name="create-a-sample-web-application"></a>Erstellen einer beispielweb Anwendung

Die folgenden Schritte führen Sie durch das Erstellen einer Beispielanwendung mithilfe der Vorlage ASP.NET-Webanwendung. diese Beispielanwendung wird später in dieser exemplarischen Vorgehensweise für jeden der externen Authentifizierungsdienste verwendet.

Starten Sie Visual Studio 2017, und wählen Sie auf der Start Seite die Option **Neues Projekt** aus. Oder wählen Sie im Menü **Datei** die Option **neu** und dann **Projekt**aus.

<!-- [![](external-authentication-services/_static/image6.png "Click to Expand the Image")](external-authentication-services/_static/image5.png) -->

Wenn das Dialogfeld **Neues Projekt** angezeigt wird, klicken Sie auf **installiert** , und erweitern Sie **Visual C#** . Wählen Sie unter **Visualisierung C#** die Option **Web**aus. Wählen Sie in der Liste der Projektvorlagen die Option **ASP.NET Webanwendung (.NET Framework)** aus. Geben Sie einen Namen für das Projekt ein, und klicken Sie auf **OK**.

[![](external-authentication-services/_static/image71.png "Click to Expand the Image")](external-authentication-services/_static/image71.png)

Wenn das **neue ASP.net-Projekt** angezeigt wird, wählen Sie die Vorlage für die **Einzelseiten Anwendung** aus, und klicken Sie auf **Projekt erstellen**.

[![](external-authentication-services/_static/image72.png "Click to Expand the Image")](external-authentication-services/_static/image72.png)

Warten Sie, bis Visual Studio 2017 Ihr Projekt erstellt hat.

<!-- [![](external-authentication-services/_static/image12.png "Click to Expand the Image")](external-authentication-services/_static/image11.png) -->

Wenn Visual Studio 2017 die Erstellung des Projekts abgeschlossen hat, öffnen Sie die Datei *Startup.auth.cs* , die sich im Ordner **App\_Start** befindet.

Wenn Sie das Projekt erstmalig erstellen, ist keiner der externen Authentifizierungsdienste in der *Startup.auth.cs* -Datei aktiviert; Im folgenden wird veranschaulicht, wie Ihr Code aussehen könnte, und in den Abschnitten, in denen Sie einen externen Authentifizierungsdienst und relevante Einstellungen aktivieren, um Microsoft-Konten, Twitter, Facebook oder die Google-Authentifizierung mit Ihrer ASP.NET-Anwendung zu verwenden:

[!code-csharp[Main](external-authentication-services/samples/sample1.cs)]

Wenn Sie F5 drücken, um Ihre Webanwendung zu erstellen und zu debuggen, wird ein Anmeldebildschirm angezeigt, auf dem Sie sehen, dass keine externen Authentifizierungsdienste definiert wurden.

[![](external-authentication-services/_static/image73.png "Click to Expand the Image")](external-authentication-services/_static/image73.png)

In den folgenden Abschnitten erfahren Sie, wie Sie die einzelnen externen Authentifizierungsdienste aktivieren, die mit ASP.net in Visual Studio 2017 bereitgestellt werden.

<a id="FACEBOOK"></a>
## <a name="enabling-facebook-authentication"></a>Aktivieren der Facebook-Authentifizierung

Für die Verwendung der Facebook-Authentifizierung müssen Sie ein Facebook-Entwicklerkonto erstellen, und Ihr Projekt erfordert eine Anwendungs-ID und einen geheimen Schlüssel von Facebook, damit Sie funktioniert. Weitere Informationen zum Erstellen eines Facebook-Entwickler Kontos und zum Abrufen der Anwendungs-ID und des geheimen Schlüssels finden Sie unter [https://go.microsoft.com/fwlink/?LinkID=252166](https://go.microsoft.com/fwlink/?LinkID=252166).

Nachdem Sie Ihre Anwendungs-ID und den geheimen Schlüssel erhalten haben, führen Sie die folgenden Schritte aus, um die Facebook-Authentifizierung für Ihre Webanwendung zu aktivieren:

1. Wenn das Projekt in Visual Studio 2017 geöffnet ist, öffnen Sie die Datei *Startup.auth.cs* .

2. Suchen Sie den Abschnitt Facebook-Authentifizierung im Code:

    [!code-csharp[Main](external-authentication-services/samples/sample2.cs)]
3. Entfernen Sie die &quot;//&quot; Zeichen, um die Auskommentierung der markierten Codezeilen aufzuheben, und fügen Sie dann die Anwendungs-ID und den geheimen Schlüssel hinzu. Nachdem Sie diese Parameter hinzugefügt haben, können Sie das Projekt neu kompilieren:

    [!code-csharp[Main](external-authentication-services/samples/sample3.cs)]
4. Wenn Sie F5 drücken, um die Webanwendung in Ihrem Webbrowser zu öffnen, sehen Sie, dass Facebook als externer Authentifizierungsdienst definiert wurde:

    [![](external-authentication-services/_static/image74.png "Click to Expand the Image")](external-authentication-services/_static/image74.png)
5. Wenn Sie auf die **Facebook** -Schaltfläche klicken, wird der Browser zur Facebook-Anmeldeseite umgeleitet:

    [![](external-authentication-services/_static/image22.png "Click to Expand the Image")](external-authentication-services/_static/image21.png)
6. Nachdem Sie Ihre Facebook-Anmelde Informationen eingegeben und auf **Anmelden**klicken, wird der Webbrowser zurück an Ihre Webanwendung umgeleitet, der Sie zur Eingabe des **Benutzernamens** auffordert, den Sie Ihrem Facebook-Konto zuordnen möchten:

    [![](external-authentication-services/_static/image24.png "Click to Expand the Image")](external-authentication-services/_static/image23.png)
7. Nachdem Sie Ihren Benutzernamen eingegeben und auf die Schaltfläche **registrieren** geklickt haben, zeigt Ihre Webanwendung die Standard **Startseite** für Ihr Facebook-Konto an:

    [![](external-authentication-services/_static/image26.png "Click to Expand the Image")](external-authentication-services/_static/image25.png)

<a id="GOOGLE"></a>
## <a name="enabling-google-authentication"></a>Aktivieren der Google-Authentifizierung

Wenn Sie die Google-Authentifizierung verwenden, müssen Sie ein Google Developer-Konto erstellen, und Ihr Projekt erfordert eine Anwendungs-ID und einen geheimen Schlüssel von Google, damit Sie funktioniert. Weitere Informationen zum Erstellen eines Google-Entwickler Kontos und zum Abrufen der Anwendungs-ID und des geheimen Schlüssels finden Sie unter [https://developers.google.com](https://developers.google.com).

Um die Google-Authentifizierung für Ihre Webanwendung zu aktivieren, führen Sie die folgenden Schritte aus:

1. Wenn das Projekt in Visual Studio 2017 geöffnet ist, öffnen Sie die Datei *Startup.auth.cs* .

2. Suchen Sie den Abschnitt zur Google-Authentifizierung im Code:

    [!code-csharp[Main](external-authentication-services/samples/sample4.cs)]
3. Entfernen Sie die &quot;//&quot; Zeichen, um die Auskommentierung der markierten Codezeilen aufzuheben, und fügen Sie dann die Anwendungs-ID und den geheimen Schlüssel hinzu. Nachdem Sie diese Parameter hinzugefügt haben, können Sie das Projekt neu kompilieren:

    [!code-csharp[Main](external-authentication-services/samples/sample5.cs)]
4. Wenn Sie F5 drücken, um die Webanwendung in Ihrem Webbrowser zu öffnen, sehen Sie, dass Google als externer Authentifizierungsdienst definiert wurde:

    [![](external-authentication-services/_static/image75.png "Click to Expand the Image")](external-authentication-services/_static/image75.png)
5. Wenn Sie auf die **Google** -Schaltfläche klicken, wird der Browser an die Google-Anmeldeseite umgeleitet:

    [![](external-authentication-services/_static/image32.png "Click to Expand the Image")](external-authentication-services/_static/image31.png)
6. Nachdem Sie Ihre Google-Anmelde Informationen eingegeben und auf **Anmelden**klicken, werden Sie von Google aufgefordert, sicherzustellen, dass Ihre Webanwendung über Berechtigungen für den Zugriff auf Ihr Google-Konto verfügt:

    [![](external-authentication-services/_static/image34.png "Click to Expand the Image")](external-authentication-services/_static/image33.png)
7. Wenn Sie auf **annehmen**klicken, wird der Webbrowser zurück an Ihre Webanwendung umgeleitet, der Sie zur Eingabe des **Benutzernamens** auffordert, den Sie Ihrem Google-Konto zuordnen möchten:

    [![](external-authentication-services/_static/image36.png "Click to Expand the Image")](external-authentication-services/_static/image35.png)
8. Nachdem Sie Ihren Benutzernamen eingegeben und auf die Schaltfläche **registrieren** geklickt haben, zeigt Ihre Webanwendung die Standard **Startseite** für Ihr Google-Konto an:

    [![](external-authentication-services/_static/image38.png "Click to Expand the Image")](external-authentication-services/_static/image37.png)

<a id="MICROSOFT"></a>
## <a name="enabling-microsoft-authentication"></a>Aktivieren der Microsoft-Authentifizierung

Die Microsoft-Authentifizierung erfordert, dass Sie ein Entwicklerkonto erstellen, und erfordert eine Client-ID und einen geheimen Client Schlüssel, damit Sie funktionieren. Weitere Informationen zum Erstellen eines Microsoft-Entwickler Kontos und zum Abrufen der Client-ID und des geheimen Client Schlüssels finden Sie unter [https://go.microsoft.com/fwlink/?LinkID=144070](https://go.microsoft.com/fwlink/?LinkID=144070).

Nachdem Sie den consumerschlüssel und das Kunden Geheimnis erhalten haben, führen Sie die folgenden Schritte aus, um die Microsoft-Authentifizierung für Ihre Webanwendung zu aktivieren:

1. Wenn das Projekt in Visual Studio 2017 geöffnet ist, öffnen Sie die Datei *Startup.auth.cs* .

2. Suchen Sie den Microsoft-Authentifizierungs Abschnitt des Codes:

    [!code-csharp[Main](external-authentication-services/samples/sample6.cs)]
3. Entfernen Sie die &quot;//&quot; Zeichen, um die Auskommentierung der markierten Codezeilen aufzuheben, und fügen Sie dann die Client-ID und den geheimen Client Schlüssel hinzu. Nachdem Sie diese Parameter hinzugefügt haben, können Sie das Projekt neu kompilieren:

    [!code-csharp[Main](external-authentication-services/samples/sample7.cs)]
4. Wenn Sie F5 drücken, um die Webanwendung in Ihrem Webbrowser zu öffnen, sehen Sie, dass Microsoft als externer Authentifizierungsdienst definiert wurde:

    [![](external-authentication-services/_static/image42.png "Click to Expand the Image")](external-authentication-services/_static/image41.png)
5. Wenn Sie auf die **Microsoft** -Schaltfläche klicken, wird der Browser auf die Microsoft-Anmeldeseite umgeleitet:

    [![](external-authentication-services/_static/image44.png "Click to Expand the Image")](external-authentication-services/_static/image43.png)
6. Nachdem Sie Ihre Microsoft-Anmelde Informationen eingegeben und auf **Anmelden**klicken, werden Sie aufgefordert, zu überprüfen, ob Ihre Webanwendung über Berechtigungen für den Zugriff auf Ihre Microsoft-Konto verfügt:

    [![](external-authentication-services/_static/image46.png "Click to Expand the Image")](external-authentication-services/_static/image45.png)
7. Wenn Sie auf **Ja**klicken, wird der Webbrowser zurück an Ihre Webanwendung umgeleitet, in der Sie aufgefordert werden, den **Benutzernamen** einzugeben, den Sie mit Ihrem Microsoft-Konto verknüpfen möchten:

    [![](external-authentication-services/_static/image48.png "Click to Expand the Image")](external-authentication-services/_static/image47.png)
8. Nachdem Sie Ihren Benutzernamen eingegeben und auf die Schaltfläche **registrieren** geklickt haben, zeigt Ihre Webanwendung die Standard **Startseite** für Ihre Microsoft-Konto an:

    [![](external-authentication-services/_static/image50.png "Click to Expand the Image")](external-authentication-services/_static/image49.png)

<a id="TWITTER"></a>
## <a name="enabling-twitter-authentication"></a>Aktivieren der Twitter-Authentifizierung

Die Twitter-Authentifizierung erfordert, dass Sie ein Entwicklerkonto erstellen, und erfordert einen consumerschlüssel und einen geheimen Kundenschlüssel, damit Sie funktionieren. Weitere Informationen zum Erstellen eines Twitter-Entwickler Kontos und zum Abrufen des consumerschlüssels und des geheimen Kunden Schlüssels finden Sie unter [https://go.microsoft.com/fwlink/?LinkID=252166](https://go.microsoft.com/fwlink/?LinkID=252166).

Nachdem Sie den consumerschlüssel und das Kunden Geheimnis erhalten haben, führen Sie die folgenden Schritte aus, um die Twitter-Authentifizierung für Ihre Webanwendung zu aktivieren:

1. Wenn das Projekt in Visual Studio 2017 geöffnet ist, öffnen Sie die Datei *Startup.auth.cs* .

2. Suchen Sie den Abschnitt Twitter-Authentifizierung im Code:

    [!code-csharp[Main](external-authentication-services/samples/sample8.cs)]
3. Entfernen Sie die &quot;//&quot; Zeichen, um die Auskommentierung der markierten Codezeilen aufzuheben, und fügen Sie dann Ihren consumerschlüssel und Ihr consumergeheimnis hinzu. Nachdem Sie diese Parameter hinzugefügt haben, können Sie das Projekt neu kompilieren:

    [!code-csharp[Main](external-authentication-services/samples/sample9.cs)]
4. Wenn Sie F5 drücken, um die Webanwendung in Ihrem Webbrowser zu öffnen, sehen Sie, dass Twitter als externer Authentifizierungsdienst definiert wurde:

    [![](external-authentication-services/_static/image54.png "Click to Expand the Image")](external-authentication-services/_static/image53.png)
5. Wenn Sie auf die Schaltfläche **Twitter** klicken, wird der Browser zur Twitter-Anmeldeseite umgeleitet:

    [![](external-authentication-services/_static/image56.png "Click to Expand the Image")](external-authentication-services/_static/image55.png)
6. Nachdem Sie Ihre Twitter-Anmelde Informationen eingegeben und auf " **App autorisieren**" klicken, wird der Webbrowser zurück an Ihre Webanwendung umgeleitet, der Sie zur Eingabe des **Benutzernamens** auffordert, den Sie Ihrem Twitter-Konto zuordnen möchten:

    [![](external-authentication-services/_static/image58.png "Click to Expand the Image")](external-authentication-services/_static/image57.png)
7. Nachdem Sie Ihren Benutzernamen eingegeben und auf die Schaltfläche **registrieren** geklickt haben, zeigt Ihre Webanwendung die Standard **Startseite** für Ihr Twitter-Konto an:

    [![](external-authentication-services/_static/image60.png "Click to Expand the Image")](external-authentication-services/_static/image59.png)

<a id="MOREINFO"></a>
## <a name="additional-information"></a>Zusätzliche Informationen

Weitere Informationen zum Erstellen von Anwendungen, die OAuth und OpenID verwenden, finden Sie unter den folgenden URLs:

- [https://go.microsoft.com/fwlink/?LinkID=252166](https://go.microsoft.com/fwlink/?LinkID=252166)
- [https://go.microsoft.com/fwlink/?LinkID=243995](https://go.microsoft.com/fwlink/?LinkID=243995)

<a id="COMBINE"></a>
### <a name="combining-external-authentication-services"></a>Kombinieren externer Authentifizierungsdienste

Um mehr Flexibilität zu erreichen, können Sie mehrere externe Authentifizierungsdienste gleichzeitig definieren. Dies ermöglicht es den Benutzern Ihrer Webanwendung, ein Konto von einem der aktivierten externen Authentifizierungsdienste zu verwenden:

[![](external-authentication-services/_static/image62.png "Click to Expand the Image")](external-authentication-services/_static/image61.png)

<a id="FQDN"></a>
### <a name="configure-iis-express-to-use-a-fully-qualified-domain-name"></a>Konfigurieren von IIS Express für die Verwendung eines voll qualifizierten Domänen Namens

Einige externe Authentifizierungs Anbieter unterstützen das Testen der Anwendung nicht, indem Sie eine HTTP-Adresse wie `http://localhost:port/`verwenden. Um dieses Problem zu umgehen, können Sie der Hostdatei eine statische vollständig qualifizierte Domänen Namen Zuordnung (FQDN) hinzufügen und ihre Projektoptionen in Visual Studio 2017 so konfigurieren, dass der FQDN zum Testen/Debuggen verwendet wird. Führen Sie dazu die folgenden Schritte aus:

- Fügen Sie einen statischen voll qualifizierten Namen für die Hostdatei hinzu:

  1. Öffnen Sie in Windows eine Eingabeaufforderung mit erhöhten Rechten.
  2. Geben Sie folgenden Befehl ein:

      <kbd>Editor%windir%\system32\drivers\etc\hosts</kbd>
  3. Fügen Sie der Datei Hosts einen Eintrag wie den folgenden hinzu:

      <kbd>127.0.0.1 www.wingtiptoys.com</kbd>
  4. Speichern und schließen Sie die Hostdatei.

- Konfigurieren Sie das Visual Studio-Projekt für die Verwendung des FQDN:

  1. Wenn das Projekt in Visual Studio 2017 geöffnet ist, klicken Sie auf das Menü **Projekt** , und wählen Sie dann die Eigenschaften Ihres Projekts aus. Sie können z. b. **WebApplication1-Eigenschaften**auswählen.
  2. Wählen Sie die Registerkarte **Web** aus.
  3. Geben Sie Ihren voll qualifizierten Namen für die <strong>Projekt-URL</strong>ein. Beispielsweise können Sie <kbd><http://www.wingtiptoys.com></kbd> eingeben, wenn dies die FQDN-Zuordnung ist, die Sie der Hostdatei hinzugefügt haben.

- Konfigurieren Sie IIS Express, um den FQDN für Ihre Anwendung zu verwenden:

    1. Öffnen Sie in Windows eine Eingabeaufforderung mit erhöhten Rechten.
    2. Geben Sie den folgenden Befehl ein, um zum IIS Express Ordner zu wechseln:

        <kbd>CD/d &quot;%ProgramFiles%\IIS Express&quot;</kbd>
    3. Geben Sie den folgenden Befehl ein, um den FQDN Ihrer Anwendung hinzuzufügen:

        <kbd>Appcmd. exe set config-section: System. applicationHost/Sites/+&quot;[Name = ' WebApplication1 ']. Bindungen. [Protocol = ' http ', bindingInformation = ' *: 80: www. wingtiptoys. com ']&quot;/Commit: Apphost</kbd>

  Dabei ist **WebApplication1** der Name Ihres Projekts, und **bindingInformation** enthält die Portnummer und den FQDN, die Sie für die Tests verwenden möchten.

<a id="OBTAIN"></a>
### <a name="how-to-obtain-your-application-settings-for-microsoft-authentication"></a>Abrufen der Anwendungseinstellungen für die Microsoft-Authentifizierung

Das Verknüpfen einer Anwendung mit Windows Live für die Microsoft-Authentifizierung ist ein einfacher Prozess. Wenn Sie eine Anwendung nicht bereits mit Windows Live verknüpft haben, können Sie die folgenden Schritte ausführen:

1. Navigieren Sie zu [https://go.microsoft.com/fwlink/?LinkID=144070](https://go.microsoft.com/fwlink/?LinkID=144070) , und geben Sie den Microsoft-Konto Namen und das Kennwort ein, **Wenn Sie dazu**aufgefordert werden.

   <!--  [![](external-authentication-services/_static/image64.png "Click to Expand the Image")](external-authentication-services/_static/image63.png) -->
2. Wählen Sie **app hinzufügen** aus, geben Sie den Namen der Anwendung ein, wenn Sie dazu aufgefordert werden, und klicken Sie auf **Erstellen**:

    [![](external-authentication-services/_static/image79.png "Click to Expand the Image")](external-authentication-services/_static/image79.png)
3. Wählen Sie Ihre APP unter **Name** aus, und die zugehörige Anwendungseigenschaften Seite wird angezeigt.

4. Geben Sie die Umleitungs Domäne für die Anwendung ein. Kopieren Sie die **Anwendungs-ID** , und klicken Sie unter **Anwendungs Geheimnisse**auf **Kennwort generieren**. Kopieren Sie das angezeigte Kennwort. Die Anwendungs-ID und das Kennwort sind Ihre Client-ID und Ihr geheimer Client Schlüssel. Wählen Sie **OK** und dann **Speichern**aus.

    [![](external-authentication-services/_static/image77.png "Click to Expand the Image")](external-authentication-services/_static/image77.png)

<a id="DISABLE"></a>
### <a name="optional-disable-local-registration"></a>Optional: lokale Registrierung deaktivieren

Die aktuelle ASP.net lokale Registrierungsfunktion verhindert nicht, dass automatisierte Programme (Bots) Mitgliedskonten erstellen. beispielsweise mithilfe einer bot-Prävention und Validierungstechnologie wie [CAPTCHA](../../../web-pages/overview/security/16-adding-security-and-membership.md). Aus diesem Grund sollten Sie das lokale Anmeldeformular und den Registrierungs Link auf der Anmeldeseite entfernen. Öffnen Sie hierzu die Seite " *\_Login. cshtml* " in Ihrem Projekt, und kommentieren Sie dann die Zeilen für den lokalen Anmeldebereich und den Registrierungs Link aus. Die resultierende Seite sollte wie im folgenden Codebeispiel aussehen:

[!code-html[Main](external-authentication-services/samples/sample10.html)]

Nachdem der lokale Anmeldebereich und der Registrierungs Link deaktiviert wurden, werden auf der Anmeldeseite nur die externen Authentifizierungs Anbieter angezeigt, die Sie aktiviert haben:

[![](external-authentication-services/_static/image70.png "Click to Expand the Image")](external-authentication-services/_static/image69.png)
