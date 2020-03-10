---
uid: web-pages/overview/security/using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site
title: Verwenden eines CAPTCHA-Diensts, um zu verhindern, dass Bots Ihre ASP.net Web Razor-Website verwenden | Microsoft-Dokumentation
author: microsoft
description: In diesem Artikel wird erläutert, wie Sie mit "reCAPTCHA (a Security Measure)" verhindern, dass automatisierte Programme (Bots) Aufgaben in einem ASP.net Web Pages (Razor) ausführen...
ms.author: riande
ms.date: 05/21/2012
ms.assetid: 2b381a41-2cb3-40c0-8545-1d393e22877f
msc.legacyurl: /web-pages/overview/security/using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site
msc.type: authoredcontent
ms.openlocfilehash: 2647a3155893a3dfb3214795a5f9cf1e8931fa91
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78440193"
---
# <a name="using-a-captcha-to-prevent-bots-from-using-your-aspnet-web-razor-site"></a>Verwenden eines CAPTCHA-Diensts, um zu verhindern, dass Bots Ihre ASP.net Web Razor-Website verwenden

von [Microsoft](https://github.com/microsoft)

> In diesem Artikel wird erläutert, wie Sie mit "reCAPTCHA" (einer Sicherheitsmaßnahme) verhindern, dass automatisierte Programme (Bots) Aufgaben auf einer ASP.net Web Pages-Website (Razor) ausführen.
> 
> **Lernen Sie Folgendes:** 
> 
> - Vorgehensweise beim Hinzufügen eines CAPTCHA-Tests zu Ihrer Website.
> 
> Dies sind die ASP.NET-Funktionen, die im Artikel eingeführt wurden:
> 
> - Das `ReCaptcha`-Hilfsprogramm.
> 
> > [!NOTE]
> > Die Informationen in diesem Artikel gelten für ASP.net Web Pages 1,0 und Web Pages 2.

## <a name="about-captchas"></a>Informationen zu Captchas

Jedes Mal, wenn Sie die Registrierung an Ihrer Website gestatten oder sogar einfach einen Namen und eine URL eingeben (z.b. für einen Blog Kommentar), erhalten Sie möglicherweise eine Flut von gefälschten Namen. Diese sind häufig von automatisierten Programmen (Bots) übrig, die versuchen, URLs auf jeder Website zu verlassen, die Sie finden. (Eine gängige Motivation besteht darin, die URLs von Produkten für den Verkauf zu veröffentlichen.)

Sie können sicherstellen, dass ein Benutzer echte Person und kein Computerprogramm ist, indem Sie eine *CAPTCHA* verwenden, um Benutzer zu überprüfen, wenn Sie Ihren Namen und Ihre Website registrieren oder anderweitig eingeben. CAPTCHA steht für vollständig automatisierte öffentliche Tests, um Computer und Menschen voneinander zu informieren. Ein CAPTCHA ist ein *Challenge-Response-* Test, bei dem der Benutzer aufgefordert wird, etwas zu tun, das für eine Person einfach ist, aber für ein automatisiertes Programm schwierig ist. Der häufigste Typ von CAPTCHA ist ein Typ, in dem Sie einige verzerrte Buchstaben sehen und aufgefordert werden, Sie einzugeben. (Die Verzerrung soll es für Bots schwierig machen, die Buchstaben zu entschlüsseln.)

## <a name="adding-a-recaptcha-test"></a>Hinzufügen eines "reCAPTCHA"-Tests

In ASP.NET-Seiten können Sie mithilfe des `ReCaptcha`-Hilfsprogramms einen CAPTCHA-Test, der auf dem "reCAPTCHA"-Dienst ([http://recaptcha.net](http://recaptcha.net)) basiert, darstellen. Das `ReCaptcha`-Hilfsprogramm zeigt ein Bild mit zwei verzerrten Wörtern an, die Benutzer vor dem Validieren der Seite ordnungsgemäß eingeben müssen. Die Benutzer Antwort wird vom reCAPTCHA.net-Dienst überprüft.

![](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/_static/image1.jpg)

1. Registrieren Sie Ihre Website unter reCAPTCHA.net ([http://recaptcha.net](http://recaptcha.net)). Wenn Sie die Registrierung abgeschlossen haben, erhalten Sie einen öffentlichen Schlüssel und einen privaten Schlüssel.
2. Fügen Sie die ASP.net-webhilfsbibliothek Ihrer Website hinzu, wie unter [Installieren von Hilfsprogrammen an einer ASP.net Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=252372)beschrieben, falls Sie dies noch nicht getan haben.
3. Wenn Sie nicht bereits über eine *\_AppStart. cshtml* -Datei verfügen, erstellen Sie im Stamm Ordner einer Website eine Datei mit dem Namen *\_AppStart. cshtml*.
4. Fügen Sie die folgenden `Recaptcha` Hilfsobjekte in der *\_AppStart. cshtml* -Datei hinzu: 

    [!code-cshtml[Main](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/samples/sample1.cshtml?highlight=6-7)]
5. Legen Sie die Eigenschaften "`PublicKey`" und "`PrivateKey`" mithilfe ihrer eigenen öffentlichen und privaten Schlüssel fest.
6. Speichern Sie die Datei *\_AppStart. cshtml* , und schließen Sie Sie.
7. Erstellen Sie im Stamm Ordner einer Website eine neue Seite mit dem Namen " *reCAPTCHA. cshtml*".
8. Ersetzen Sie den vorhandenen Inhalt durch Folgendes: 

    [!code-cshtml[Main](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/samples/sample2.cshtml)]
9. Führen Sie die Seite " *reCAPTCHA. cshtml* " in einem Browser aus. Wenn der `PrivateKey` Wert gültig ist, zeigt die Seite das Steuerelement "reCAPTCHA" und eine Schaltfläche an. Wenn Sie die Schlüssel nicht global in *\_AppStart. html*festgelegt haben, wird auf der Seite ein Fehler angezeigt. 

    ![](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/_static/image1.png)
10. Geben Sie die Wörter für den Test ein. Wenn Sie den "reCAPTCHA"-Test bestanden haben, wird eine entsprechende Meldung angezeigt. Andernfalls wird eine Fehlermeldung angezeigt, und das Steuerelement "reCAPTCHA" wird erneut angezeigt.

> [!NOTE]
> Wenn sich der Computer in einer Domäne befindet, die den Proxy Server verwendet, müssen Sie möglicherweise das `defaultproxy`-Element der Datei " *Web. config* " konfigurieren. Das folgende Beispiel zeigt eine *Web. config* -Datei mit dem-`defaultproxy`-Element, das so konfiguriert ist, dass der-Dienst für die Zusammenarbeit aktiviert wird.
> 
> [!code-xml[Main](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/samples/sample3.xml)]

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [Anpassen des Standort weiten Verhaltens für ASP.net Web Pages Websites](https://go.microsoft.com/fwlink/?LinkId=202906)
- [Website "reCAPTCHA"](https://www.google.com/recaptcha)
