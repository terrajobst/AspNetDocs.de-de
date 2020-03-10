---
uid: web-pages/overview/getting-started/11-adding-email-to-your-web-site
title: Senden von e-Mails von einer ASP.net Web Pages (Razor-Website) | Microsoft-Dokumentation
author: Rick-Anderson
description: In diesem Kapitel wird erläutert, wie eine automatisierte e-Mail-Nachricht von einer Website gesendet wird.
ms.author: riande
ms.date: 02/20/2014
ms.assetid: fc49bcb9-f1a9-4048-8c3f-b60951853200
msc.legacyurl: /web-pages/overview/getting-started/11-adding-email-to-your-web-site
msc.type: authoredcontent
ms.openlocfilehash: 23e9717329525fb5a0ed505c9dc94505d4f9dbbe
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78515325"
---
# <a name="sending-email-from-an-aspnet-web-pages-razor-site"></a>Senden von e-Mails von einer ASP.net Web Pages-Website (Razor)

von [Tom fitzmacken](https://github.com/tfitzmac)

> In diesem Artikel wird erläutert, wie Sie eine e-Mail-Nachricht von einer Website senden, wenn Sie ASP.net Web Pages (Razor) verwenden.
> 
> Sie lernen Folgendes:
> 
> - So senden Sie eine e-Mail-Nachricht von Ihrer Website
> - Anfügen einer Datei an eine e-Mail-Nachricht.
> 
> Dies ist die im Artikel eingeführte ASP.net-Funktion:
> 
> - Das `WebMail`-Hilfsprogramm.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Im Tutorial verwendete Software Versionen
> 
> 
> - ASP.net Web Pages (Razor) 3
>   
> 
> Dieses Tutorial funktioniert auch mit ASP.net Web Pages 2.

<a id="Sending_Email_Messages"></a>
## <a name="sending-email-messages-from-your-website"></a>Senden von e-Mails von Ihrer Website

Es gibt verschiedene Gründe, warum Sie möglicherweise e-Mails von Ihrer Website senden müssen. Möglicherweise senden Sie Bestätigungsnachrichten an Benutzer, oder Sie senden Benachrichtigungen an sich selbst (z. b., wenn ein neuer Benutzer registriert wurde). Das `WebMail`-Hilfsprogramm erleichtert Ihnen das Senden von e-Mails.

Um das `WebMail`-Hilfsprogramm verwenden zu können, müssen Sie Zugriff auf einen SMTP-Server haben. (SMTP steht für *Simple Mail Transfer Protocol*.) Ein SMTP-Server ist ein e-Mail-Server, der nur Nachrichten &#8212; an den Server des Empfängers weiterleitet, d. h. als ausgehende e-Mail. Wenn Sie einen Hostinganbieter für Ihre Website verwenden, richten Sie wahrscheinlich eine e-Mail ein, und Sie können Ihnen mitteilen, wie Ihr SMTP-Servername lautet. Wenn Sie in einem Unternehmensnetzwerk arbeiten, können Administratoren oder Ihre IT-Abteilung Ihnen in der Regel die Informationen zu einem SMTP-Server zur Verfügung stellen, den Sie verwenden können. Wenn Sie zu Hause arbeiten, können Sie möglicherweise sogar mit Ihrem normalen e-Mail-Anbieter testen, der Ihnen den Namen des SMTP-Servers mitteilen kann. Normalerweise benötigen Sie Folgendes:

- Der Name des SMTP-Servers.
- Die Portnummer. Dies ist fast immer 25. Ihr ISP erfordert jedoch möglicherweise die Verwendung von Port 587. Wenn Sie SSL (Secure Sockets Layer) für e-Mail verwenden, benötigen Sie möglicherweise einen anderen Port. Informieren Sie sich bei Ihrem e-Mail-Anbieter
- Anmelde Informationen (Benutzername, Kennwort).

In diesem Verfahren erstellen Sie zwei Seiten. Die erste Seite verfügt über ein Formular, mit dem Benutzer eine Beschreibung eingeben können, als ob Sie ein Formular für technischen Support ausfüllen würden. Auf der ersten Seite werden die Informationen an eine zweite Seite übermittelt. Auf der zweiten Seite extrahiert der Code die Informationen des Benutzers und sendet eine e-Mail-Nachricht. Außerdem wird eine Meldung angezeigt, die bestätigt, dass der Problembericht empfangen wurde.

![Klang](11-adding-email-to-your-web-site/_static/image1.jpg)

> [!NOTE]
> Um dieses Beispiel einfach zu halten, initialisiert der Code das `WebMail` Helper direkt auf der Seite, auf der es verwendet wird. Bei echten Websites empfiehlt es sich jedoch, Initialisierungs Code wie diesen in eine globale Datei einzufügen, damit Sie das `WebMail`-Hilfsprogramm für alle Dateien auf Ihrer Website initialisieren können. Weitere Informationen finden Sie unter [Anpassen des Standort weiten Verhaltens für ASP.net Web Pages](https://go.microsoft.com/fwlink/?LinkId=202906#Setting_Values_For_Helpers).

1. Erstellen Sie eine neue Website.
2. Fügen Sie eine neue Seite mit dem Namen *emailrequest. cshtml* hinzu, und fügen Sie das folgende Markup hinzu: 

    [!code-html[Main](11-adding-email-to-your-web-site/samples/sample1.html)]

    Beachten Sie, dass das `action`-Attribut des Form-Elements auf *ProcessRequest. cshtml*festgelegt wurde. Dies bedeutet, dass das Formular an diese Seite und nicht an die aktuelle Seite zurückgesendet wird.
3. Fügen Sie der Website eine neue Seite mit dem Namen *ProcessRequest. cshtml* hinzu, und fügen Sie den folgenden Code und das folgende Markup hinzu:   

    [!code-cshtml[Main](11-adding-email-to-your-web-site/samples/sample2.cshtml)]

    Im Code werden die Werte der Formularfelder angezeigt, die an die Seite gesendet wurden. Anschließend rufen Sie die `Send`-Methode des `WebMail`-Hilfsprogramms auf, um die e-Mail zu erstellen und zu senden. In diesem Fall bestehen die zu verwendenden Werte aus Text, den Sie mit den Werten verketten, die aus dem Formular übermittelt wurden.

    Der Code für diese Seite befindet sich innerhalb eines `try/catch` Blocks. Wenn der Versuch, eine e-Mail zu senden, aus irgendeinem Grund nicht funktioniert (z. b. sind die Einstellungen nicht richtig), wird der Code im `catch`-Block ausgeführt, und die `errorMessage` Variable wird auf den aufgetretenen Fehler festgelegt. (Weitere Informationen zu `try/catch` Blöcken oder zum `<text>`-Tag finden [Sie unter Einführung in ASP.net Web Pages Programmieren mit der Razor-Syntax](https://go.microsoft.com/fwlink/?LinkID=251587#ID_HandlingErrors).)

    Wenn die `errorMessage`-Variable im Text der Seite leer ist (Standard), wird dem Benutzer eine Meldung angezeigt, dass die e-Mail-Nachricht gesendet wurde. Wenn die `errorMessage` Variable auf true festgelegt ist, wird dem Benutzer eine Meldung angezeigt, dass beim Senden der Nachricht ein Problem aufgetreten ist.

    Beachten Sie, dass im Bereich der Seite, auf dem eine Fehlermeldung angezeigt wird, ein zusätzlicher Test vorhanden ist: `if(debuggingFlag)`. Diese Variable kann auf "true" festgelegt werden, wenn beim Senden von e-Mails Probleme auftreten. Wenn `debuggingFlag` auf true festgelegt ist und beim Senden von e-Mails ein Problem vorliegt, wird eine zusätzliche Fehlermeldung angezeigt, die anzeigt, welche ASP.NET beim Versuch, die e-Mail-Nachricht zu senden, gemeldet hat. Eine faire Warnung: die Fehlermeldungen, die von ASP.net gemeldet werden, wenn keine e-Mail-Nachricht gesendet werden kann, können generisch sein. Wenn ASP.net beispielsweise keine Verbindung mit dem SMTP-Server aufnehmen kann (z. b. weil Sie einen Fehler im Servernamen ausgegeben haben), wird der Fehler `Failure sending mail`.

    > [!NOTE] 
    > 
    > **Wichtig** Wenn Sie eine Fehlermeldung von einem Ausnahme Objekt (`ex` im Code) erhalten, übergeben Sie diese Nachricht *nicht* routinemäßig an Benutzer. Ausnahme Objekte enthalten häufig Informationen, die Benutzern nicht angezeigt werden und die sogar ein Sicherheitsrisiko darstellen können. Der Grund hierfür ist, dass dieser Code die Variable `debuggingFlag` enthält, die als Schalter verwendet wird, um die Fehlermeldung anzuzeigen, und warum die Variable standardmäßig auf false festgelegt ist. Sie sollten diese Variable auf true festlegen (und daher die Fehlermeldung) *nur* dann anzeigen, wenn Sie ein Problem mit dem Senden von e-Mails haben und Debuggen müssen. Wenn Sie Probleme behoben haben, legen Sie `debuggingFlag` auf "false" fest.

    Ändern Sie die folgenden e-Mail-bezogenen Einstellungen im Code:

   - Legen Sie `your-SMTP-host` auf den Namen des SMTP-Servers fest, auf den Sie Zugriff haben.
   - Legen Sie `your-user-name-here` auf den Benutzernamen für Ihr SMTP-Server Konto fest.
   - Legen Sie `your-account-password` auf das Kennwort für Ihr SMTP-Server Konto fest.
   - Legen Sie `your-email-address-here` auf Ihre e-Mail-Adresse fest. Dies ist die e-Mail-Adresse, von der die Nachricht gesendet wird. (Bei einigen e-Mail-Anbietern ist es nicht möglich, eine andere `From` Adresse anzugeben, und der Benutzername wird als `From` Adresse verwendet.)

     > [!TIP] 
     > 
     > <a id="configuring_email_settings"></a>
     > ### <a name="configuring-email-settings"></a>E-Mail-Einstellungen
     > 
     > Manchmal kann es eine Herausforderung sein, sicherzustellen, dass Sie über die richtigen Einstellungen für den SMTP-Server, die Portnummer usw. verfügen. Im Folgenden einige Tipps:
     > 
     > - Der Name des SMTP-Servers ist oft wie `smtp.provider.com` oder `smtp.provider.net`. Wenn Sie Ihre Website jedoch in einem Hostinganbieter veröffentlichen, ist der Name des SMTP-Servers zu diesem Zeitpunkt möglicherweise `localhost`. Dies liegt daran, dass der e-Mail-Server, nachdem Sie veröffentlicht haben und die Website auf dem Server des Anbieters ausgeführt wird, lokal aus der Perspektive Ihrer Anwendung besteht. Diese Änderung der Servernamen kann bedeuten, dass Sie den SMTP-Servernamen im Rahmen Ihres Veröffentlichungsprozesses ändern müssen.
     > - Die Portnummer ist in der Regel 25. Bei manchen Anbietern ist es jedoch erforderlich, Port 587 oder einen anderen Port zu verwenden.
     > - Stellen Sie sicher, dass Sie die richtigen Anmelde Informationen verwenden. Wenn Sie Ihre Website auf einem Hosting-Anbieter veröffentlicht haben, verwenden Sie die Anmelde Informationen, die der Anbieter speziell für e-Mail angegeben hat. Diese können sich von den Anmelde Informationen unterscheiden, die Sie für die Veröffentlichung verwenden.
     > - Manchmal benötigen Sie keine Anmelde Informationen. Wenn Sie e-Mails mithilfe Ihres persönlichen ISP senden, kennt Ihr e-Mail-Anbieter ihre Anmelde Informationen möglicherweise bereits. Nach der Veröffentlichung müssen Sie möglicherweise andere Anmelde Informationen als beim Testen auf dem lokalen Computer verwenden.
     > - Wenn Ihr e-Mail-Anbieter Verschlüsselung verwendet, müssen Sie `WebMail.EnableSsl` auf `true`festlegen.
4. Führen Sie die Seite *emailrequest. cshtml* in einem Browser aus. (Stellen Sie sicher, dass die Seite im Arbeitsbereich " **Dateien** " ausgewählt ist, bevor Sie Sie ausführen.)
5. Geben Sie Ihren Namen und eine Problembeschreibung ein, und klicken Sie dann auf die Schaltfläche **senden** . Sie werden zur Seite " *ProcessRequest. cshtml* " umgeleitet, die Ihre Nachricht bestätigt und Ihnen eine e-Mail-Nachricht sendet. 

    ![Klang](11-adding-email-to-your-web-site/_static/image2.jpg)

<a id="Sending_a_File"></a>
## <a name="sending-a-file-using-email"></a>Senden einer Datei per e-Mail

Sie können auch Dateien senden, die an e-Mail-Nachrichten angefügt sind. In diesem Verfahren erstellen Sie eine Textdatei und zwei HTML-Seiten. Sie verwenden die Textdatei als e-Mail-Anhang.

1. Fügen Sie in der Website eine neue Textdatei hinzu, und nennen Sie Sie *MyFile. txt*.
2. Kopieren Sie den folgenden Text, und fügen Sie ihn in die Datei ein: 

    `Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat.`
3. Erstellen Sie eine Seite mit dem Namen *sendfile. cshtml* , und fügen Sie das folgende Markup hinzu: 

    [!code-html[Main](11-adding-email-to-your-web-site/samples/sample3.html)]
4. Erstellen Sie eine Seite mit dem Namen *processFile. cshtml* , und fügen Sie das folgende Markup hinzu: 

    [!code-cshtml[Main](11-adding-email-to-your-web-site/samples/sample4.cshtml)]
5. Ändern Sie die folgenden e-Mail-bezogenen Einstellungen im Code aus dem Beispiel:

    - Legen Sie `your-SMTP-host` auf den Namen eines SMTP-Servers fest, auf den Sie Zugriff haben.
    - Legen Sie `your-user-name-here` auf den Benutzernamen für Ihr SMTP-Server Konto fest.
    - Legen Sie `your-email-address-here` auf Ihre e-Mail-Adresse fest. Dies ist die e-Mail-Adresse, von der die Nachricht gesendet wird.
    - Legen Sie `your-account-password` auf das Kennwort für Ihr SMTP-Server Konto fest.
    - Legen Sie `target-email-address-here` auf Ihre e-Mail-Adresse fest. (Wie zuvor haben Sie normalerweise eine e-Mail an eine andere Person gesendet, aber zu Testzwecken können Sie Sie an sich selbst senden.)
6. Führen Sie die Seite *sendfile. cshtml* in einem Browser aus.
7. Geben Sie Ihren Namen, eine Betreffzeile und den Namen der anzufügenden Textdatei an (*MyFile. txt*).
8. Klicken Sie auf die Schaltfläche `Submit`. Wie zuvor werden Sie zur Seite *processFile. cshtml* umgeleitet, die Ihre Nachricht bestätigt und Ihnen eine e-Mail-Nachricht mit der angefügten Datei sendet.

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [Leitfaden zur Behandlung von Problemen mit ASP.NET Web Pages (Razor)](https://go.microsoft.com/fwlink/?LinkId=253001)
- [Simple Mail Transfer Protocol](https://msdn.microsoft.com/library/aa480435.aspx)
- [Anpassen des Standort weiten Verhaltens für ASP.net Web Pages](https://go.microsoft.com/fwlink/?LinkId=202906)
