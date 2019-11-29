---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/displaying-a-custom-error-page-cs
title: Anzeigen einer benutzerdefinierten FehlerseiteC#() | Microsoft-Dokumentation
author: rick-anderson
description: Was wird dem Benutzer angezeigt, wenn ein Laufzeitfehler in einer ASP.NET-Webanwendung auftritt? Die Antwort hängt davon ab, wie die &lt;customErrors-&gt; Konfiguration der Website...
ms.author: riande
ms.date: 06/09/2009
ms.assetid: cb061642-faf3-41b2-9372-69e13444d458
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/displaying-a-custom-error-page-cs
msc.type: authoredcontent
ms.openlocfilehash: c1ff4c112b9a489b8fb9ef3443663cd71eda7965
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2019
ms.locfileid: "74625492"
---
# <a name="displaying-a-custom-error-page-c"></a>Anzeigen einer benutzerdefinierten Fehlerseite (C#)

von [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Code herunterladen](https://download.microsoft.com/download/1/0/C/10CC829F-A808-4302-97D3-59989B8F9C01/ASPNET_Hosting_Tutorial_11_CS.zip) oder [PDF herunterladen](https://download.microsoft.com/download/5/C/5/5C57DB8C-5DEA-4B3A-92CA-4405544D313B/aspnet_tutorial11_CustomErrors_cs.pdf)

> Was wird dem Benutzer angezeigt, wenn ein Laufzeitfehler in einer ASP.NET-Webanwendung auftritt? Die Antwort hängt davon ab, wie die &lt;customErrors-&gt; Konfiguration der Website. Standardmäßig wird Benutzern ein unansehnlicher gelber Bildschirm angezeigt, der einen Laufzeitfehler verkündet. In diesem Tutorial wird gezeigt, wie Sie diese Einstellungen anpassen, um eine ästhetisch ansprechende benutzerdefinierte Fehlerseite anzuzeigen, die dem Aussehen und Erscheinungsbild Ihrer Website entspricht.

## <a name="introduction"></a>Einführung

In einer perfekten Welt gibt es keine Laufzeitfehler. Programmierer würden Code mit nary-Fehlern und mit stabiler Benutzereingabe Validierung schreiben, und externe Ressourcen wie Datenbankserver und e-Mail-Server würden niemals offline geschaltet werden. Natürlich sind Fehler in der Realität unvermeidlich. Die Klassen im .NET Framework signalisieren einen Fehler, indem eine Ausnahme ausgelöst wird. Wenn Sie z. b. die Open-Methode eines SqlConnection-Objekts aufrufen, wird eine Verbindung mit der durch eine Verbindungs Zeichenfolge angegebenen Datenbank hergestellt. Wenn die Datenbank jedoch nicht herunter ist oder die Anmelde Informationen in der Verbindungs Zeichenfolge ungültig sind, löst die Open-Methode eine `SqlException`aus. Ausnahmen können durch die Verwendung von `try/catch/finally`-Blöcken behandelt werden. Wenn Code in einem `try`-Block eine Ausnahme auslöst, wird die Steuerung an den entsprechenden catch-Block übertragen, in dem der Entwickler versuchen kann, den Fehler zu beheben. Wenn kein entsprechender catch-Block vorhanden ist, oder wenn sich der Code, der die Ausnahme ausgelöst hat, nicht in einem try-Block befindet, durchläuft die Ausnahme die aufrufsliste bei der Suche nach `try/catch/finally` Blöcken.

Wenn die Ausnahme bis zum ASP.net-Lauf Zeit Modul ohne Behandlung der Ausnahme erfolgt, wird das [`Error` Ereignis](https://msdn.microsoft.com/library/system.web.httpapplication.error.aspx) der [`HttpApplication` Klasse](https://msdn.microsoft.com/library/system.web.httpapplication.aspx)ausgelöst, und die konfigurierte *Fehlerseite* wird angezeigt. Standardmäßig wird in ASP.net eine Fehlerseite angezeigt, die als [gelber Bildschirm für Tod](http://en.wikipedia.org/wiki/Yellow_Screen_of_Death#Yellow) (Ysod) bezeichnet wird. Es gibt zwei Versionen von Ysod: eine zeigt die Ausnahme Details, eine Stapel Überwachung und weitere Informationen, die für Entwickler hilfreich sind, die die Anwendung Debuggen (siehe **Abbildung 1**). der andere gibt einfach an, dass ein Laufzeitfehler aufgetreten ist (siehe **Abbildung 2**).

Die Ausnahme Details Ysod ist für Entwickler, die die Anwendung debuggen, sehr hilfreich, aber das Anzeigen von Ysod für Endbenutzer ist bedienen und Unprofessional. Stattdessen sollten Endbenutzer auf eine Fehlerseite gelangen, auf der das Aussehen und das Erscheinungsbild der Website verwaltet werden, mit der benutzerfreundlichen Prosa, in der die Situation beschrieben wird. Die gute Nachricht ist, dass das Erstellen einer benutzerdefinierten Fehlerseite recht einfach ist. Dieses Tutorial beginnt mit einem Blick auf ASP. Die unterschiedlichen Fehlerseiten von net. Anschließend wird gezeigt, wie Sie die Webanwendung so konfigurieren, dass Benutzer eine benutzerdefinierte Fehlerseite bei einem Fehler anzeigt.

### <a name="examining-the-three-types-of-error-pages"></a>Untersuchen der drei Typen von Fehlerseiten

Wenn in einer ASP.NET-Anwendung eine nicht behandelte Ausnahme auftritt, wird einer der folgenden drei Arten von Fehlerseiten angezeigt:

- Auf der Seite mit der Ausnahme Details (gelber Bildschirm von Fehlerseite)
- Der gelbe Bildschirm "Laufzeitfehler" der Fehlerseite "Tod" oder
- Eine benutzerdefinierte Fehlerseite

Die Fehlerseite, die Entwicklern am häufigsten bekannt ist, ist die Ausnahme Details Ysod. Standardmäßig wird diese Seite für Benutzer angezeigt, die lokal besuchen. Daher ist dies die Seite, die angezeigt wird, wenn beim Testen der Website in der Entwicklungsumgebung ein Fehler auftritt. Wie der Name schon sagt, enthält die Ausnahme Details Ysod Details zur Ausnahme, den Typ, die Meldung und die Stapel Überwachung. Wenn die Ausnahme durch Code in der Code Behind-Klasse der ASP.NET-Seite ausgelöst wurde und die Anwendung für das Debuggen konfiguriert ist, wird in der Ausnahme Details Ysod ebenfalls diese Codezeile (und einige Codezeilen oberhalb und darunter) angezeigt.

**Abbildung 1** zeigt die Seite mit Ausnahme Details Ysod. Notieren Sie sich die URL im Adress Fenster des Browsers: `http://localhost:62275/Genre.aspx?ID=foo`. Beachten Sie, dass die `Genre.aspx` Seite die Buch Reviews in einem bestimmten Genre auflistet. Es erfordert, dass `GenreId` Wert (a `uniqueidentifier`) durch die Abfrage Zeichenfolge übermittelt wird. Beispielsweise ist die entsprechende URL zum Anzeigen der Überprüfung der Fiktion `Genre.aspx?ID=7683ab5d-4589-4f03-a139-1c26044d0146`. Wenn ein nicht`uniqueidentifier` Wert durch die QueryString (z. b. "foo") übermittelt wird, wird eine Ausnahme ausgelöst.

> [!NOTE]
> Wenn Sie diesen Fehler in der für den Download verfügbaren demowebanwendung reproduzieren möchten, können Sie entweder `Genre.aspx?ID=foo` direkt besuchen oder auf den Link "Laufzeitfehler generieren" in `Default.aspx`klicken.

Beachten Sie die in **Abbildung 1**dargestellten Ausnahme Informationen. Die Ausnahme Meldung "Fehler bei der Konvertierung bei der Konvertierung von einer Zeichenfolge in uniqueidentifier" oben auf der Seite. Der Typ der Ausnahme, `System.Data.SqlClient.SqlException`, wird ebenfalls aufgelistet. Außerdem gibt es die Stapel Überwachung.

[![](displaying-a-custom-error-page-cs/_static/image2.png)](displaying-a-custom-error-page-cs/_static/image1.png)

**Abbildung 1**: die Ausnahme Details Ysod enthält Informationen zur Ausnahme.  
 ([Klicken Sie, um das Bild in voller Größe anzuzeigen](displaying-a-custom-error-page-cs/_static/image3.png))

Der andere Typ von Ysod ist der Laufzeitfehler Ysod und ist in **Abbildung 2**dargestellt. Der Laufzeitfehler Ysod informiert den Besucher, dass ein Laufzeitfehler aufgetreten ist, enthält jedoch keine Informationen über die ausgelöste Ausnahme. (Es wird jedoch beschrieben, wie Sie die Fehlerdetails anzeigen können, indem Sie die `Web.config` Datei ändern. Dies ist ein Bestandteil der Art, in der ein solcher Ysod-Vorgang unprofessionell aussieht.)

Standardmäßig wird der Laufzeitfehler Ysod für Benutzer angezeigt, die Remote aufrufen (über http://www.yoursite.com), wie von der URL in der Adressleiste des Browsers in **Abbildung 2**: `http://httpruntime.web703.discountasp.net/Genre.aspx?ID=foo`. Die beiden unterschiedlichen Ysod-Bildschirme sind vorhanden, da Entwickler an der Kenntnis der Fehlerdetails interessiert sind. diese Informationen sollten jedoch auf einer Live Website nicht angezeigt werden, da Sie möglicherweise potenzielle Sicherheitsrisiken oder andere vertrauliche Informationen für alle Benutzer, die ihre Areal.

> [!NOTE]
> Wenn Sie das befolgen und DiscountASP.net als Webhost verwenden, bemerken Sie möglicherweise, dass der Laufzeitfehler Ysod beim Besuch der Live Website nicht angezeigt wird. Dies liegt daran, dass DiscountASP.net Ihre Server so konfiguriert hat, dass die Ausnahme Details Ysod standardmäßig angezeigt werden. Die gute Nachricht ist, dass Sie dieses Standardverhalten außer Kraft setzen können, indem Sie einen `<customErrors>` Abschnitt zu ihrer `Web.config`-Datei hinzufügen. Im Abschnitt "Konfigurieren der angezeigten Fehlerseite" wird der `<customErrors>` Abschnitt ausführlich untersucht.

[![](displaying-a-custom-error-page-cs/_static/image5.png)](displaying-a-custom-error-page-cs/_static/image4.png)

**Abbildung 2**: der Laufzeitfehler "Ysod" enthält keine Fehler Details.  
 ([Klicken Sie, um das Bild in voller Größe anzuzeigen](displaying-a-custom-error-page-cs/_static/image6.png))

Die dritte Fehlerseite ist die benutzerdefinierte Fehlerseite, die eine von Ihnen erstellte Webseite ist. Der Vorteil einer benutzerdefinierten Fehlerseite besteht darin, dass Sie über die gesamte Kontrolle über die Informationen verfügen, die dem Benutzer zusammen mit dem Aussehen und Gefühl der Seite angezeigt werden. die benutzerdefinierte Fehlerseite kann dieselbe Master Seite und dieselben Stile wie Ihre anderen Seiten verwenden. Der Abschnitt "Verwenden einer benutzerdefinierten Fehlerseite" führt Sie durch das Erstellen einer benutzerdefinierten Fehlerseite und deren Konfiguration für die Anzeige im Fall einer nicht behandelten Ausnahme. **Abbildung 3** bietet einen kurzen Spitzenwert für diese benutzerdefinierte Fehlerseite. Wie Sie sehen können, ist das Aussehen und das Gefühl der Fehlerseite weitaus professioneller als der in den Abbildungen 1 und 2 gezeigte gelbe Bildschirm.

[![](displaying-a-custom-error-page-cs/_static/image8.png)](displaying-a-custom-error-page-cs/_static/image7.png)

**Abbildung 3**: eine benutzerdefinierte Fehlerseite bietet ein individuanpasstes Aussehen und Gefühl  
 ([Klicken Sie, um das Bild in voller Größe anzuzeigen](displaying-a-custom-error-page-cs/_static/image9.png))

Nehmen Sie sich einen Moment Zeit, um die Adressleiste des Browsers in **Abbildung 3**zu überprüfen. Beachten Sie, dass die Adressleiste die URL der benutzerdefinierten Fehlerseite (`/ErrorPages/Oops.aspx`) anzeigt. In den Abbildungen 1 und 2 werden die gelben Bildschirme auf der gleichen Seite angezeigt, von der der Fehler stammt (`Genre.aspx`). Der benutzerdefinierten Fehlerseite wird die URL der Seite übergeben, auf der der Fehler über den `aspxerrorpath` QueryString-Parameter aufgetreten ist.

## <a name="configuring-which-error-page-is-displayed"></a>Konfigurieren der angezeigten Fehlerseite

Welche der drei möglichen Fehlerseiten angezeigt wird, basiert auf zwei Variablen:

- Die Konfigurationsinformationen im Abschnitt `<customErrors>` und
- Gibt an, ob der Benutzer die Website lokal oder Remote besucht.

Der [Abschnitt "`<customErrors>`](https://msdn.microsoft.com/library/h0hfz6fc.aspx) " in `Web.config` hat zwei Attribute, die sich darauf auswirken, welche Fehlerseite angezeigt wird: `defaultRedirect` und `mode`. Das `defaultRedirect`-Attribut ist optional. Wenn angegeben, wird die URL der benutzerdefinierten Fehlerseite angegeben, und es wird angegeben, dass die benutzerdefinierte Fehlerseite anstelle der Lauf Zeit Fehler-Ysod angezeigt werden soll. Das `mode`-Attribut ist erforderlich und akzeptiert einen von drei Werten: `On`, `Off`oder `RemoteOnly`. Diese Werte weisen folgendes Verhalten auf:

- `On`: gibt an, dass die benutzerdefinierte Fehlerseite oder der Laufzeitfehler Ysod für alle Besucher angezeigt wird, unabhängig davon, ob Sie lokal oder Remote sind.
- `Off`: gibt an, dass die Ausnahme Details Ysod für alle Besucher angezeigt werden, unabhängig davon, ob Sie lokal oder Remote sind.
- `RemoteOnly`: gibt an, dass die benutzerdefinierte Fehlerseite oder der Laufzeitfehler Ysod für Remote Besucher angezeigt wird, während die Ausnahme Details Ysod für die lokalen Besucher angezeigt werden.

Wenn Sie nicht anders angeben, verhält sich ASP.net so, als ob Sie das Mode-Attribut auf `RemoteOnly` festgelegt haben und keinen `defaultRedirect` Wert angegeben haben. Anders ausgedrückt bedeutet das Standardverhalten, dass die Ausnahme Details Ysod für lokale Besucher angezeigt werden, während der Laufzeitfehler Ysod für Remote Besucher angezeigt wird. Sie können dieses Standardverhalten überschreiben, indem Sie einen `<customErrors>` Abschnitt zu Ihrer Webanwendung hinzufügen `Web.config file.`

## <a name="using-a-custom-error-page"></a>Verwenden einer benutzerdefinierten Fehlerseite

Jede Webanwendung sollte über eine benutzerdefinierte Fehlerseite verfügen. Es bietet eine sicherere Alternative zum Lauf Zeit Fehler Ysod, es ist einfach zu erstellen, und das Konfigurieren der Anwendung für die Verwendung der benutzerdefinierten Fehlerseite nimmt nur einige Zeit in Anspruch. Der erste Schritt besteht darin, die benutzerdefinierte Fehlerseite zu erstellen. Ich habe der Anwendung "Book Reviews" einen neuen Ordner namens "`ErrorPages`" hinzugefügt und dieser eine neue ASP.NET-Seite mit dem Namen "`Oops.aspx`" hinzugefügt. Lassen Sie die Seite dieselbe Master Seite wie die übrigen Seiten auf Ihrer Website verwenden, damit Sie automatisch dasselbe Aussehen und Gefühl erbt.

[![](displaying-a-custom-error-page-cs/_static/image11.png)](displaying-a-custom-error-page-cs/_static/image10.png)

**Abbildung 4**: Erstellen einer benutzerdefinierten Fehlerseite

Geben Sie als nächstes einige Minuten ein, um den Inhalt für die Fehlerseite zu erstellen. Ich habe eine ziemlich einfache benutzerdefinierte Fehlerseite erstellt und eine Meldung mit dem Hinweis angezeigt, dass ein unerwarteter Fehler aufgetreten ist, und ein Link zurück zur Homepage der Site.

[![](displaying-a-custom-error-page-cs/_static/image13.png)](displaying-a-custom-error-page-cs/_static/image12.png)

**Abbildung 5**: Entwerfen der benutzerdefinierten Fehlerseite  
 ([Klicken Sie, um das Bild in voller Größe anzuzeigen](displaying-a-custom-error-page-cs/_static/image14.png))

Wenn die Fehlerseite abgeschlossen ist, konfigurieren Sie die Webanwendung so, dass die benutzerdefinierte Fehlerseite anstelle der Lauf Zeit Fehler-Ysod verwendet wird. Dies wird erreicht, indem die URL der Fehlerseite im `defaultRedirect` Attribut des `<customErrors>` Abschnitts angegeben wird. Fügen Sie der `Web.config` Datei Ihrer Anwendung das folgende Markup hinzu:

[!code-xml[Main](displaying-a-custom-error-page-cs/samples/sample1.xml)]

Das obige Markup konfiguriert die Anwendung so, dass die Ausnahme Details Ysod für Benutzer angezeigt werden, die lokal besuchen, während die benutzerdefinierte Fehlerseite oops. aspx für die Benutzer verwendet wird, die Remote besuchen. Um dies in Aktion zu sehen, stellen Sie Ihre Website in der Produktionsumgebung bereit, und besuchen Sie dann die Seite Genre. aspx auf der Live Website mit einem ungültigen QueryString-Wert. Die benutzerdefinierte Fehlerseite sollte angezeigt werden (siehe **Abbildung 3**).

Um zu überprüfen, ob die benutzerdefinierte Fehlerseite nur Remote Benutzern angezeigt wird, besuchen Sie die Seite `Genre.aspx` mit einer ungültigen Abfrage Zeichenfolge aus der Entwicklungsumgebung. Es sollte weiterhin die Ausnahme Details Ysod angezeigt werden (siehe **Abbildung 1**). Durch die `RemoteOnly` Einstellung wird sichergestellt, dass Benutzer, die die Website in der Produktionsumgebung besuchen, die benutzerdefinierte Fehlerseite sehen, während Entwickler, die lokal arbeiten, weiterhin die Details der Ausnahme sehen.

## <a name="notifying-developers-and-logging-error-details"></a>Benachrichtigen von Entwicklern und Protokollieren von Fehler Details

Fehler, die in der Entwicklungsumgebung auftreten, sind darauf zurückzuführen, dass sich der Entwickler auf dem Computer befindet. Ihnen werden die Informationen der Ausnahme in der Ausnahme Details Ysod angezeigt, und Sie weiß, welche Schritte Sie ausgeführt haben, als der Fehler aufgetreten ist. Wenn bei der Produktion jedoch ein Fehler auftritt, hat der Entwickler nicht wissen, dass ein Fehler aufgetreten ist, es sei denn, der Endbenutzer, der die Website besucht, benötigt die Zeit, den Fehler zu melden. Und selbst wenn der Benutzer seine Möglichkeit verlässt, das Entwicklungsteam darauf aufmerksam zu machen, dass ein Fehler aufgetreten ist, ohne den Ausnahmetyp, die Nachricht und die Stapel Überwachung zu kennen, kann es schwierig sein, die Ursache des Fehlers zu diagnostizieren.

Aus diesen Gründen ist es wichtig, dass alle Fehler in der Produktionsumgebung in einem permanenten Speicher (z. b. einer Datenbank) protokolliert werden und dass die Entwickler über diesen Fehler benachrichtigt werden. Die benutzerdefinierte Fehlerseite mag ein guter Ausgangspunkt für diese Protokollierung und Benachrichtigung sein. Die benutzerdefinierte Fehlerseite hat leider keinen Zugriff auf die Fehlerdetails und kann daher nicht zum Protokollieren dieser Informationen verwendet werden. Die gute Nachricht ist, dass es eine Reihe von Möglichkeiten gibt, die Fehlerdetails abzufangen und zu protokollieren. in den nächsten drei Tutorials wird dieses Thema ausführlicher erläutert.

## <a name="using-different-custom-error-pages-for-different-http-error-statuses"></a>Verwenden verschiedener benutzerdefinierter Fehlerseiten für verschiedene http-Fehlerstatus

Wenn eine Ausnahme von einer ASP.NET-Seite ausgelöst und nicht behandelt wird, wird die Ausnahme durch die ASP.NET-Laufzeit erhöht, die die konfigurierte Fehlerseite anzeigt. Wenn eine Anforderung in die ASP.net-Engine kommt, aber aus irgendeinem Grund nicht verarbeitet werden kann, kann es vorkommen, dass die angeforderte Datei nicht gefunden wurde oder Leseberechtigungen für die Datei deaktiviert wurden. die ASP.net-Engine löst dann eine `HttpException`aus. Diese Ausnahme, wie z. b. Ausnahmen, die von ASP.NET Seiten ausgelöst werden, wird zur Laufzeit hochskalieren, sodass die entsprechende Fehlerseite angezeigt wird.

Dies bedeutet, dass für die Webanwendung in der Produktion die benutzerdefinierte Fehlerseite angezeigt wird, wenn ein Benutzer eine Seite anfordert, die nicht gefunden wurde. **Abbildung 6** zeigt ein Beispiel. Da die Anforderung für eine nicht vorhandene Seite (`NoSuchPage.aspx`) gilt, wird ein `HttpException` ausgelöst, und die benutzerdefinierte Fehlerseite wird angezeigt (Beachten Sie den Verweis auf `NoSuchPage.aspx` im `aspxerrorpath` QueryString-Parameter).

[![](displaying-a-custom-error-page-cs/_static/image16.png)](displaying-a-custom-error-page-cs/_static/image15.png)

**Abbildung 6**: die ASP.NET-Laufzeit zeigt die konfigurierte Fehlerseite als Reaktion auf eine ungültige Anforderung an ([Klicken Sie, um das Bild in voller Größe anzuzeigen](displaying-a-custom-error-page-cs/_static/image17.png))

Standardmäßig bewirken alle Fehlertypen, dass die gleiche benutzerdefinierte Fehlerseite angezeigt wird. Sie können jedoch eine andere benutzerdefinierte Fehlerseite für einen bestimmten HTTP-Statuscode angeben, indem Sie `<error>` Children-Elemente innerhalb des `<customErrors>` Abschnitts verwenden. Um z. b. eine andere Fehlerseite anzuzeigen, die im Fall eines Fehlers der Seite "nicht gefunden" mit dem HTTP-Statuscode 404 angezeigt wird, aktualisieren Sie den `<customErrors>` Abschnitt, sodass er das folgende Markup enthält:

[!code-xml[Main](displaying-a-custom-error-page-cs/samples/sample2.xml)]

Wenn diese Änderung vorgenommen wird und ein Benutzer, der eine Remote Verbindung benötigt, eine ASP.NET-Ressource anfordert, die nicht vorhanden ist, wird er nicht `Oops.aspx`, sondern an die benutzerdefinierte Fehlerseite `404.aspx` umgeleitet. Wie in **Abbildung 7** dargestellt, kann die Seite `404.aspx` eine spezifischere Nachricht als die allgemeine benutzerdefinierte Fehlerseite enthalten.

> [!NOTE]
> Weitere Informationen zum Erstellen effektiver 404-Fehlerseiten finden Sie [unter 404-Fehlerseiten](http://www.smashingmagazine.com/2009/01/29/404-error-pages-one-more-time/) .

[![](displaying-a-custom-error-page-cs/_static/image19.png)](displaying-a-custom-error-page-cs/_static/image18.png)

**Abbildung 7**: auf der Seite "benutzerdefinierter 404-Fehler" wird eine gezieltere Nachricht angezeigt als `Oops.aspx`  
([Klicken Sie, um das Bild in voller Größe anzuzeigen](displaying-a-custom-error-page-cs/_static/image20.png)) 

Da Sie wissen, dass die `404.aspx` Seite nur erreicht wird, wenn der Benutzer eine Anforderung an eine Seite sendet, die nicht gefunden wurde, können Sie diese benutzerdefinierte Fehlerseite so erweitern, dass Sie Funktionen enthält, die dem Benutzer helfen, diesen spezifischen Fehlertyp zu beheben. Sie könnten z. b. eine Datenbanktabelle erstellen, die bekannte ungültige URLs guten URLs zuordnet, und dann auf der `404.aspx` Seite benutzerdefinierte Fehlerseite eine Abfrage für diese Tabelle ausführen und Seiten vorschlagen, die der Benutzer möglicherweise zu erreichen versucht.

> [!NOTE]
> Die benutzerdefinierte Fehlerseite wird nur angezeigt, wenn eine Anforderung an eine Ressource gerichtet ist, die von der ASP.net-Engine verarbeitet wird. Wie in den [grundlegenden Unterschieden zwischen IIS und dem ASP.NET Development Server-](core-differences-between-iis-and-the-asp-net-development-server-cs.md) Tutorial erläutert wurde, kann der Webserver bestimmte Anforderungen selbst verarbeiten. Standardmäßig verarbeitet der IIS-Webserver Anforderungen für statische Inhalte wie Bilder und HTML-Dateien, ohne dass die ASP.net-Engine aufgerufen wird. Wenn der Benutzer daher eine nicht vorhandene Bilddatei anfordert, wird die standardmäßige 404-Fehlermeldung von IIS anstelle von ASP zurückgegeben. Die konfigurierte Fehlerseite des Netzes.

## <a name="summary"></a>Summary

Wenn in einer ASP.NET-Anwendung eine nicht behandelte Ausnahme auftritt, wird dem Benutzer eine von drei Fehlerseiten angezeigt: die Ausnahme Details gelbe Bildschirm von Tod. Laufzeitfehler: gelber Bildschirm des Todes oder eine benutzerdefinierte Fehlerseite. Welche Fehlerseite angezeigt wird, hängt von der `<customErrors>` Konfiguration der Anwendung und davon ab, ob der Benutzer lokal oder Remote zu besuchen ist. Das Standardverhalten besteht darin, die Ausnahme Details Ysod an lokale Besucher und den Laufzeitfehler Ysod an Remote Besucher anzuzeigen.

Der Laufzeitfehler "Ysod" blendet potenziell vertrauliche Fehlerinformationen vom Benutzer aus, der die Website besucht, unterbricht aber das Erscheinungsbild Ihrer Website und bewirkt, dass die Anwendung fehlerhaft wird. Ein besserer Ansatz ist die Verwendung einer benutzerdefinierten Fehlerseite, bei der die benutzerdefinierte Fehlerseite erstellt und entworfen und die URL im `defaultRedirect`-Attribut des `<customErrors>` Abschnitts angegeben wird. Sie können sogar mehrere benutzerdefinierte Fehlerseiten für verschiedene http-Fehlerstatus verwenden.

Die Seite benutzerdefinierter Fehler ist der erste Schritt in einer umfassenden Strategie zur Fehlerbehandlung für eine Website in der Produktion. Außerdem sind wichtige Schritte erforderlich, um den Entwickler des Fehlers zu warnen und seine Details zu protokollieren. Die nächsten drei Tutorials untersuchen Techniken für Fehler Benachrichtigung und Protokollierung.

Fröhliche Programmierung!

### <a name="further-reading"></a>Weiterführende Themen

Weitere Informationen zu den in diesem Tutorial behandelten Themen finden Sie in den folgenden Ressourcen:

- [Fehlerseiten, eine weitere Zeit](http://www.smashingmagazine.com/2009/01/29/404-error-pages-one-more-time/)
- [Entwurfsrichtlinien für Ausnahmen](https://msdn.microsoft.com/library/ms229014.aspx)
- [Benutzerfreundliche Fehlerseiten](http://aspnet.4guysfromrolla.com/articles/090606-1.aspx)
- [Behandeln und Auslösen von Ausnahmen](https://msdn.microsoft.com/library/5b2yeyab.aspx)
- [Ordnungsgemäße Verwendung von benutzerdefinierten Fehlerseiten in ASP.net](http://professionalaspnet.com/archive/2007/09/30/Properly-Using-Custom-Error-Pages-in-ASP.NET.aspx)

> [!div class="step-by-step"]
> [Zurück](strategies-for-database-development-and-deployment-cs.md)
> [Weiter](processing-unhandled-exceptions-cs.md)
