---
uid: whitepapers/aspnet4/breaking-changes
title: ASP.NET 4 wichtige Änderungen | Microsoft-Dokumentation
author: rick-anderson
description: In diesem Dokument werden die Änderungen beschrieben, die für die Version 4 von .NET Framework vorgenommen wurden, die sich potenziell auf Anwendungen auswirken kann, die mithilfe von erstellt wurden...
ms.author: riande
ms.date: 02/10/2010
ms.assetid: d601c540-f86b-4feb-890c-20c806b3da6c
msc.legacyurl: /whitepapers/aspnet4/breaking-changes
msc.type: content
ms.openlocfilehash: 8ccad3b40a723c92a3164de082e1f94577141008
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78439509"
---
# <a name="aspnet-4-breaking-changes"></a>ASP.NET 4: Bedeutende Änderungen

> In diesem Dokument werden die Änderungen beschrieben, die für die Version 4 von .NET Framework vorgenommen wurden, die sich potenziell auf Anwendungen auswirken kann, die mit früheren Releases erstellt wurden, einschließlich der Versionen ASP.NET 4 Beta 1 und Beta 2.
> 
> [Dieses Whitepaper herunterladen](https://download.microsoft.com/download/7/1/A/71A105A9-89D6-4201-9CC5-AD6A3B7E2F22/ASP_NET_4_Breaking_Changes.pdf)

<a id="0.1__Toc256768952"></a><a id="0.1__Toc256770056"></a>

## <a name="contents"></a>Inhalt

[ControlRenderingCompatibilityVersion-Einstellung in der Datei "Web. config"](#0.1__Toc256770141 "_Toc256770141")  
[ClientIDMode-Änderungen](#0.1__Toc256770142 "_Toc256770142")  
[HtmlEncode und UrlEncode codieren jetzt einfache Anführungszeichen.](#0.1__Toc256770143 "_Toc256770143")  
[Der ASP.net page (. aspx)-Parser ist strenger.](#0.1__Toc256770144 "_Toc256770144")  
[Aktualisierte Browser Definitions Dateien](#0.1__Toc256770145 "_Toc256770145")  
["System. Web. Mobile. dll" aus der Stamm-Webkonfigurationsdatei entfernt](#0.1__Toc256770146 "_Toc256770146")  
[ASP.net Anforderungs Validierung](#0.1__Toc256770147 "_Toc256770147")  
[Standard Hash Algorithmus ist jetzt HMACSHA256](#0.1__Toc256770148 "_Toc256770148")  
[Konfigurationsfehler im Zusammenhang mit der neuen ASP.NET 4-Stamm Konfiguration](#0.1__Toc256770149 "_Toc256770149")  
[ASP.NET 4 untergeordnete Anwendungen können nicht gestartet werden, wenn unter ASP.NET 2,0-oder ASP.NET 3,5-Anwendungen](#0.1__Toc256770150 "_Toc256770150")  
[ASP.NET 4 Websites können auf Computern, auf denen SharePoint installiert ist, nicht gestartet werden.](#0.1__Toc256770151 "_Toc256770151")  
[Die HttpRequest. FilePath-Eigenschaft enthält keine PathInfo-Werte mehr.](#0.1__Toc256770152 "_Toc256770152")  
[ASP.NET 2,0-Anwendungen generieren möglicherweise HttpException-Fehler, die auf "EURL. axd" verweisen](#0.1__Toc256770153 "_Toc256770153")  
[Ereignishandler werden möglicherweise nicht in einem Standarddokument in IIS 7 oder im integrierten IIS 7,5-Modus ausgelöst.](#0.1__Toc256770154 "_Toc256770154")  
[Änderungen an der Implementierung der ASP.NET-Code Zugriffssicherheit (CAS)](#0.1__Toc256770155 "_Toc256770155")  
[Mitgliedshipuser und andere Typen im System. Web. Security-Namespace wurden verschoben.](#0.1__Toc256770156 "_Toc256770156")  
[Änderungen der Ausgabe Zwischenspeicherung, um \* HTTP-Header variieren](#0.1__Toc256770157 "_Toc256770157")  
[System. Web. Sicherheits Typen für Passport sind veraltet.](#0.1__Toc256770158 "_Toc256770158")  
[Die MenuItem. PopOutImageUrl-Eigenschaft kann ein Bild in ASP.NET 4 nicht Rendering](#0.1__Toc256770159 "_Toc256770159")  
["Menu. StaticPopOutImageUrl" und "Menu. DynamicPopOutImageUrl" können keine Bilder darstellen, wenn Pfade umgekehrte Schrägstriche enthalten.](#0.1__Toc256770160 "_Toc256770160")  
[ERS](#0.1__Toc256770161 "_Toc256770161")

<a id="0.1__ControlRenderingCompatibilityVersio"></a><a id="0.1__Toc245724853"></a><a id="0.1__Toc255587630"></a><a id="0.1__Toc256770141"></a>

## <a name="controlrenderingcompatibilityversion-setting-in-the-webconfig-file"></a>ControlRenderingCompatibilityVersion-Einstellung in der Datei "Web. config"

ASP.NET-Steuerelemente wurden in der .NET Framework Version 4 geändert, damit Sie die Markup Darstellung genauer angeben können. In früheren Versionen der .NET Framework haben einige Steuerelemente Markup ausgegeben, das Sie nicht deaktivieren konnten. Standardmäßig wird ASP.NET 4 diese Art von Markup nicht mehr generiert.

Wenn Sie Visual Studio 2010 verwenden, um Ihre Anwendung von ASP.NET 2,0 oder ASP.NET 3,5 zu aktualisieren, fügt das Tool automatisch eine Einstellung zur `Web.config` Datei hinzu, die das Legacy Rendering beibehält. Wenn Sie eine Anwendung jedoch upgraden, indem Sie den Anwendungspool in IIS so ändern, dass er sich an .NET Framework 4 richtet, verwendet ASP.NET standardmäßig den neuen Renderingmodus. Um den neuen Renderingmodus zu deaktivieren, fügen Sie die folgende Einstellung in der `Web.config`-Datei hinzu:

[!code-xml[Main](breaking-changes/samples/sample1.xml)]

Die wichtigsten Renderingänderungen, die das neue Verhalten bringt, lauten wie folgt:

- Das **Image** -Steuerelement und das **ImageButton** -Steuerelement erzeugen nicht mehr ein `border="0"` Attribut.
- Die **BaseValidator** -Klasse und die Validierungs Steuerelemente, die von ihr abgeleitet werden, erzeugen standardmäßig keinen roten Text mehr.
- Das **HtmlForm** -Steuerelement rendert kein **Name** -Attribut.
- Das **Table** -Steuerelement rendert nicht mehr ein `border="0"` Attribut.
- Steuerelemente, die nicht für Benutzereingaben entworfen wurden (z. b. das **Label** -Steuerelement), geben das `disabled="disabled"` Attribut nicht mehr aus, wenn die **aktivierte** Eigenschaft auf **false** festgelegt ist (oder wenn Sie diese Einstellung von einem Container Steuerelement erben).

<a id="0.1__Toc245724854"></a><a id="0.1__Toc255587631"></a><a id="0.1__Toc256770142"></a>

## <a name="clientidmode-changes"></a>ClientIDMode-Änderungen

Mit der **ClientIDMode** -Einstellung in ASP.NET 4 können Sie angeben, wie ASP.NET das **ID** -Attribut für HTML-Elemente generiert. In früheren Versionen von ASP.net entsprach das Standardverhalten der **AutoID** -Einstellung von **ClientIDMode**. Die Standardeinstellung ist nun jedoch **vorhersagbar**.

Wenn Sie Visual Studio 2010 verwenden, um Ihre Anwendung von ASP.NET 2,0 oder ASP.NET 3,5 zu aktualisieren, fügt das Tool automatisch eine Einstellung zur `Web.config` Datei hinzu, die das Verhalten früherer Versionen der .NET Framework beibehält. Wenn Sie eine Anwendung jedoch upgraden, indem Sie den Anwendungspool in IIS so ändern, dass er sich an .NET Framework 4 richtet, verwendet ASP.NET standardmäßig den neuen Modus. Um den neuen Client-ID-Modus zu deaktivieren, fügen Sie die folgende Einstellung in der `Web.config`-Datei hinzu:

[!code-xml[Main](breaking-changes/samples/sample2.xml)]

<a id="0.1__Toc245724855"></a><a id="0.1__Toc255587632"></a><a id="0.1__Toc256770143"></a>

## <a name="htmlencode-and-urlencode-now-encode-single-quotation-marks"></a>HtmlEncode und UrlEncode codieren jetzt einfache Anführungszeichen.

In ASP.NET 4 wurden die Methoden **HtmlEncode** und **urlencode** der **HttpUtility** -Klasse und der **HttpServerUtility** -Klasse aktualisiert, um das einfache Anführungszeichen (') wie folgt zu codieren:

- Die **HtmlEncode** -Methode codiert Instanzen des einfachen Anführungs Zeichens als.
- Die **urlencode** -Methode codiert Instanzen des einfachen Anführungs Zeichens als %27.

<a id="0.1__Toc255587633"></a><a id="0.1__Toc256770144"></a><a id="0.1__Toc245724856"></a>

## <a name="aspnet-page-aspx-parser-is-stricter"></a>Der ASP.net page (. aspx)-Parser ist strenger.

Der Seiten Parser für ASP.net Pages (`.aspx`-Dateien) und Benutzer Steuerelemente (`.ascx` Dateien) ist in ASP.NET 4 strenger und lehnt weitere Instanzen von ungültigem Markup ab. Beispielsweise können die folgenden beiden Code Ausschnitte in früheren Versionen von ASP.net erfolgreich analysiert werden, aber jetzt werden Parser-Fehler in ASP.NET 4 ausgegeben.

[!code-aspx[Main](breaking-changes/samples/sample3.aspx)]

Beachten Sie das ungültige Semikolon am Ende des **HiddenField** -Tags.

[!code-aspx[Main](breaking-changes/samples/sample4.aspx)]

Beachten Sie das Attribut "nicht geschlossene **Style** ", das im **CssClass** -Attribut ausgeführt wird.

<a id="0.1__Toc255587634"></a><a id="0.1__Toc256770145"></a>

## <a name="browser-definition-files-updated"></a>Aktualisierte Browser Definitions Dateien

Die Browserdefinitionsdateien wurden aktualisiert und enthalten jetzt Informationen zu neuen und aktualisierten Browsern und Geräten. Ältere Browser und Geräte wie Netscape Navigator wurden entfernt und neuere Browser und Geräte wie Google Chrome und Apple iPhone hinzugefügt.

Wenn Ihre Anwendung benutzerdefinierte Browserdefinitionen enthält, die von einer der entfernten Browserdefinitionen erben, wird ein Fehler angezeigt. Wenn der `App_Browsers` Ordner z. b. eine Browser Definition enthält, die von der IE2-Browser Definition erbt, erhalten Sie die folgende Konfigurations Fehlermeldung:

- Das Browser-oder Gatewayelement mit der ID "IE2" wurde nicht gefunden.

> [!NOTE]
> Das **httpbrowserfunktionalitäten** -Objekt (das von der **Request. Browser** -Eigenschaft der Seite verfügbar gemacht wird) wird von den Browser Definitions Dateien gesteuert. Daher können sich die Informationen, die durch den Zugriff auf eine Eigenschaft dieses Objekts in ASP.NET 4 zurückgegeben werden, von den in einer früheren Version von ASP.net zurückgegebenen Informationen unterscheiden.

Sie können die alten Browser Definitions Dateien wiederherstellen, indem Sie die Browser Definitions Dateien aus folgendem Ordner kopieren:

[!code-console[Main](breaking-changes/samples/sample5.cmd)]

Kopieren Sie die Dateien in den entsprechenden `\CONFIG\Browsers` Ordner für ASP.NET 4. Nachdem Sie die Dateien kopiert haben, führen Sie das Befehlszeilen Tool ASPNET\_regbrowsers. exe aus.

<a id="0.1__Toc255587635"></a><a id="0.1__Toc256770146"></a>

## <a name="systemwebmobiledll-removed-from-root-web-configuration-file"></a>"System. Web. Mobile. dll" aus der Stamm-Webkonfigurationsdatei entfernt

In früheren Versionen von ASP.net war ein Verweis auf die Assembly "System. Web. Mobile. dll" `Web.config` **im Abschnitt "** Assemblys" unter enthalten. Der Verweis auf diese Assembly wurde entfernt, um die Leistung zu verbessern.

Die Assembly "System. Web. Mobile. dll" ist in ASP.NET 4 enthalten, ist jedoch veraltet. Wenn Sie Typen aus der System. Web. Mobile. dll-Assembly verwenden möchten, fügen Sie einen Verweis auf diese Assembly entweder der Stamm `Web.config` Datei oder einer Anwendungs `Web.config` Datei hinzu. Wenn Sie z. b. eines der (veralteten) ASP.NET Mobile-Steuerelemente verwenden möchten, müssen Sie der `Web.config` Datei einen Verweis auf die Assembly "System. Web. Mobile. dll" hinzufügen.

<a id="0.1__Toc245724857"></a><a id="0.1__Toc255587636"></a><a id="0.1__Toc256770147"></a>

## <a name="aspnet-request-validation"></a>ASP.net Anforderungs Validierung

Das Feature "Anforderungs Validierung" in ASP.net bietet eine bestimmte Ebene des Standard Schutzes vor Cross-Site Scripting (XSS)-Angriffen. In früheren Versionen von ASP.net war die Anforderungs Validierung standardmäßig aktiviert. Es wird jedoch nur auf ASP.NET Seiten (`.aspx` Dateien und deren Klassendateien) und nur auf die Ausführung dieser Seiten angewendet.

In ASP.NET 4 ist die Anforderungs Validierung standardmäßig für alle Anforderungen aktiviert, da Sie vor der **BeginRequest** -Phase einer HTTP-Anforderung aktiviert wird. Dies hat zur Folge, dass die Anforderungs Validierung auf Anforderungen für alle ASP.NET-Ressourcen, nicht nur auf ASPX-Seiten Anforderungen, angewendet wird. Dies schließt Anforderungen wie Webdienst Aufrufe und benutzerdefinierte HTTP-Handler ein. Die Anforderungs Validierung ist auch aktiv, wenn benutzerdefinierte HTTP-Module den Inhalt einer HTTP-Anforderung lesen.

Folglich treten möglicherweise Anforderungs Validierungs Fehler für Anforderungen auf, die zuvor keine Fehler ausgelöst haben. Fügen Sie die folgende Einstellung in der `Web.config`-Datei hinzu, um das Verhalten des 2,0 ASP.net-Anforderungs Überprüfungs Features wiederherzustellen:

[!code-xml[Main](breaking-changes/samples/sample6.xml)]

Es wird jedoch empfohlen, dass Sie alle Anforderungs Validierungs Fehler analysieren, um zu bestimmen, ob vorhandene Handler, Module oder anderer benutzerdefinierter Code potenziell unsichere HTTP-Eingaben zugreift, die XSS-Angriffsvektoren sein könnten.

<a id="0.1__Toc245724858"></a><a id="0.1__Toc255587637"></a><a id="0.1__Toc256770148"></a>

## <a name="default-hashing-algorithm-is-now-hmacsha256"></a>Standard Hash Algorithmus ist jetzt HMACSHA256

ASP.NET verwendet sowohl Verschlüsselung als auch Hashalgorithmen, um Daten wie Formularauthentifizierungscookies und den Ansichtsstatus zu sichern. Standardmäßig verwendet ASP.NET 4 jetzt den HMACSHA256-Algorithmus für Hash Vorgänge für Cookies und den Ansichts Zustand. In früheren Versionen von ASP.net wurde der ältere HMACSHA1-Algorithmus verwendet.

Ihre Anwendungen sind möglicherweise beeinträchtigt, wenn Sie gemischte ASP.NET 2.0/ASP. NET 4-Umgebungen ausführen, in denen Daten wie Formular Authentifizierungs Cookies across.net Frameworkversionen funktionieren müssen. Um eine ASP.NET 4-Webanwendung für die Verwendung des älteren HMACSHA1-Algorithmus zu konfigurieren, fügen Sie die folgende Einstellung in der `Web.config`-Datei hinzu:

[!code-xml[Main](breaking-changes/samples/sample7.xml)]

<a id="0.1__Toc245724859"></a><a id="0.1__Toc255587638"></a><a id="0.1__Toc256770149"></a>

## <a name="configuration-errors-related-to-new-aspnet-4-root-configuration"></a>Konfigurationsfehler im Zusammenhang mit der neuen ASP.NET 4-Stamm Konfiguration

Die Stamm Konfigurationsdateien (die `machine.config`-Datei und die Stamm `Web.config` Datei) für die .NET Framework 4 (und somit auch ASP.NET 4) wurden aktualisiert, sodass Sie die meisten der Konfigurationsinformationen für Bausteine enthalten, die in ASP.NET 3,5 in den Anwendungs `Web.config` Dateien gefunden wurden. Aufgrund der Komplexität der verwalteten IIS 7-und IIS 7,5-Konfigurations Systeme können bei der Ausführung von ASP.NET 3,5-Anwendungen unter ASP.NET 4 und unter IIS 7 und IIS 7,5 entweder ASP.net-oder IIS-Konfigurationsfehler auftreten.

Es wird empfohlen, ASP.NET 3,5-Anwendungen auf ASP.NET 4 zu aktualisieren, indem Sie die Projekt Aktualisierungs Tools in Visual Studio 2010 verwenden. Visual Studio 2010 ändert automatisch die `Web.config` Datei der ASP.NET 3,5-Anwendung, sodass Sie die entsprechenden Einstellungen für ASP.NET 4 enthält.

Es ist jedoch ein unterstütztes Szenario, ASP.NET 3,5-Anwendungen ohne Neukompilierung mit dem .NET Framework 4 auszuführen. In diesem Fall müssen Sie möglicherweise die `Web.config` Datei der Anwendung manuell ändern, bevor Sie die Anwendung unter dem .NET Framework 4 und unter IIS 7 oder IIS 7,5 ausführen.

In den nächsten beiden Abschnitten werden Änderungen beschrieben, die Sie möglicherweise für verschiedene Kombinationen von Software vornehmen müssen.

**Windows Vista SP1 oder Windows Server 2008 SP1, auf dem weder Hotfix KB958854 noch SP2 installiert ist.** In dieser Konfiguration führt das IIS 7-Konfigurationssystem eine nicht ordnungsgemäße Konfiguration der verwalteten Konfiguration einer Anwendung durch, indem Sie die `Web.config` Datei auf Anwendungsebene mit den ASP.NET 2,0 `machine.config`-Dateien vergleicht. Aus diesem Grund müssen `Web.config` Dateien auf Anwendungsebene aus dem .NET Framework 3,5 oder höher eine **System. Web. Extensions** -Konfigurations Abschnitts Definition (das-Element) aufweisen, um keinen IIS 7-Überprüfungs Fehler zu verursachen.

`Web.config` Datei Einträge auf Anwendungsebene, die nicht exakt mit den ursprünglichen Konfigurations Abschnitts Definitionen der Original Bausteine identisch sind, die mit Visual Studio 2008 eingeführt wurden, führen jedoch zu ASP.net-Konfigurationsfehlern. (Die von Visual Studio 2008 generierten Standard Konfigurationseinträge funktionieren ordnungsgemäß.) Ein häufiges Problem ist, dass manuell geänderte `Web.config` Dateien die Konfigurations Attribute **AllowDefinition** und Requirements- **Berechtigung** , die in den verschiedenen Konfigurations Abschnitts Definitionen gefunden werden, weglassen. Dies verursacht einen Konflikt zwischen dem abgekürzten Konfigurations Abschnitt in `Web.config` Dateien auf Anwendungsebene und der gesamten Definition in der Datei ASP.NET 4 `machine.config`. Folglich löst das ASP.NET 4-Konfigurationssystem zur Laufzeit einen Konfigurationsfehler aus.

**Windows Vista SP2, Windows Server 2008 SP2, Windows 7, Windows Server 2008 R2 sowie Windows Vista SP1 und Windows Server 2008 SP1, auf denen Hotfix KB958854 installiert ist.**

In diesem Szenario gibt das systemeigene Konfigurationssystem IIS 7 und IIS 7,5 einen Konfigurationsfehler zurück, da es einen Textvergleich für das **Type** -Attribut ausführt, das für einen verwalteten Konfigurations Abschnitts Handler definiert ist. Da alle `Web.config` Dateien, die von Visual Studio 2008 und Visual Studio 2008 SP1 generiert werden, in der Typzeichenfolge für das System den Wert "3,5" aufweisen **. Web. Extensions** (und verwandte) Konfigurations Abschnitts Handler, und da die Datei "ASP.NET 4 `machine.config`" im **Type** -Attribut für dieselben Konfigurations Abschnitts Handler "4,0" enthält, schlagen Anwendungen, die in Visual Studio 2008 oder Visual Studio 2008 SP1 generiert werden, bei der Konfigurations Validierung in IIS 7 und IIS 7,5 immer fehl.

<a id="0.1__Toc251910248"></a>

### <a name="resolving-these-issues"></a>Beheben dieser Probleme

Die Problem Umgehung für das erste Szenario besteht darin, die `Web.config` Datei auf Anwendungsebene zu aktualisieren, indem Sie den Konfigurations Text des Bausteine aus einer `Web.config` Datei einschließen, die automatisch von Visual Studio 2008 generiert wurde.

Eine alternative Lösung für das erste Szenario besteht darin, Service Pack 2 für Vista oder Windows Server 2008 auf Ihrem Computer zu installieren oder Hotfix KB958854 ([https://support.microsoft.com/kb/958854](https://support.microsoft.com/kb/958854)) zu installieren, um das falsche Konfigurations Verhalten des IIS-Konfigurations Systems zu beheben. Nachdem Sie jedoch eine dieser Aktionen durchgeführt haben, tritt bei der Anwendung wahrscheinlich aufgrund des für das zweite Szenario beschriebenen Problems ein Konfigurationsfehler auf.

Die Problem Umgehung für das zweite Szenario besteht darin, alle **System. Web. Extensions** -Konfigurations Abschnitts Definitionen und Konfigurations Abschnitts Gruppendefinitionen aus der `Web.config` Datei auf Anwendungsebene zu löschen oder auszukommentieren. Diese Definitionen befinden sich in der Regel am Anfang der Datei auf Anwendungsebene `Web.config` und können durch das **configabschnitts** -Element und seine untergeordneten Elemente identifiziert werden.

Für beide Szenarien wird empfohlen, dass Sie den Abschnitt " **System. CodeDom** " auch manuell löschen, obwohl dies nicht erforderlich ist.

<a id="0.1__Toc252995490"></a><a id="0.1__Toc255587639"></a><a id="0.1__Toc256770150"></a><a id="0.1__Toc245724860"></a>

## <a name="aspnet-4-child-applications-fail-to-start-when-under-aspnet-20-or-aspnet-35-applications"></a>ASP.NET 4 untergeordnete Anwendungen können nicht gestartet werden, wenn unter ASP.NET 2,0-oder ASP.NET 3,5-Anwendungen

ASP.NET 4-Anwendungen, die als untergeordnete Anwendungen konfiguriert wurden, die frühere Versionen von ASP.NET ausführen, können möglicherweise aufgrund von Konfigurations- oder Kompilierungsfehlern nicht starten. Im folgenden Beispiel wird eine Verzeichnisstruktur für eine betroffene Anwendung gezeigt.

`/parentwebapp` (für die Verwendung von ASP.NET 2,0 oder ASP.NET 3,5 konfiguriert)  
`/childwebapp` (für die Verwendung von ASP.NET 4 konfiguriert)

Die Anwendung im Ordner "`childwebapp`" kann unter IIS 7 oder IIS 7,5 nicht gestartet werden und meldet einen Konfigurationsfehler. Der Fehlertext enthält eine Meldung wie die folgende:

- `The requested page cannot be accessed because the related configuration data for the page is invalid.`

- `The configuration section 'configSections' cannot be read because it is missing a section declaration.`

Unter IIS 6 kann die Anwendung im Ordner "`childwebapp`" ebenfalls nicht gestartet werden, es wird jedoch ein anderer Fehler gemeldet. Der Fehlertext könnte z. b. Folgendes angeben:

- `The value for the 'compilerVersion' attribute in the provider options must be 'v4.0' or later if you are compiling for version 4.0 or later of the .NET Framework. To compile this Web application for version 3.5 or earlier of the .NET Framework, remove the 'targetFramework' attribute from the element of the Web.config file`

Diese Szenarios treten auf, weil die Konfigurationsinformationen aus der übergeordneten Anwendung im `parentwebapp` Ordner Teil der Hierarchie der Konfigurationsinformationen sind, die die endgültigen gemergten Konfigurationseinstellungen bestimmt, die von der untergeordneten Webanwendung im Ordner `childwebapp` verwendet werden. Abhängig davon, ob die ASP.NET 4-Webanwendung unter IIS 7 (oder IIS 7,5) oder IIS 6 ausgeführt wird, gibt entweder das IIS-Konfigurationssystem oder das ASP.NET 4-Kompilierungs System einen Fehler zurück.

Die Schritte, die Sie befolgen müssen, um dieses Problem zu beheben und die untergeordnete ASP.NET 4-Anwendung zu verwenden, hängen davon ab, ob die Anwendung ASP.NET 4 unter IIS 6 oder IIS 7 (oder IIS 7,5) ausgeführt wird.

### <a name="step-1-iis-7-or-iis-75-only"></a>Schritt 1 (nur IIS 7 oder IIS 7,5)

Dieser Schritt ist nur für Betriebssysteme erforderlich, die IIS 7 oder IIS 7,5 ausführen, darunter Windows Vista, Windows Server 2008, Windows 7 und Windows Server 2008 R2.

Verschieben Sie die **configabschnitts** -Definition in der `Web.config`-Datei der übergeordneten Anwendung (die Anwendung, die ASP.NET 2,0 oder ASP.NET 3,5 ausführt) in die Stamm `Web.config` Datei für The.NET Framework 2,0. Das systemeigene Konfigurationssystem IIS 7 und IIS 7,5 scannt das **configabschnitts** -Element bei der Zusammenführung der Hierarchie von Konfigurationsdateien. Wenn die **configabschnitts** -Definition aus der `Web.config` Datei der übergeordneten Webanwendung in die Stamm `Web.config` Datei verschoben wird, wird das Element aus dem Konfigurations Zusammenschluss Prozess, der für die untergeordnete Anwendung ASP.NET 4 ausgeführt wird, tatsächlich ausgeblendet.

Bei einem 32-Bit-Betriebssystem oder einem 32-Bit-Anwendungs Pool befindet sich die Stamm `Web.config` Datei für ASP.NET 2,0 und ASP.NET 3,5 normalerweise im folgenden Ordner:

`C:\Windows\Microsoft.NET\Framework\v2.0.50727\CONFIG`

Bei einem 64-Bit-Betriebssystem oder einem 64-Bit-Anwendungs Pool befindet sich die Stamm `Web.config` Datei für ASP.NET 2,0 und ASP.NET 3,5 normalerweise im folgenden Ordner:

`C:\Windows\Microsoft.NET\Framework64\v2.0.50727\CONFIG`

Wenn Sie sowohl 32-Bit-als auch 64-Bit-Webanwendungen auf einem 64-Bit-Computer ausführen, müssen Sie das **configabschnitts** -Element in Stamm `Web.config` Dateien für das 32-Bit-und das 64-Bit-System verschieben.

Wenn Sie das **configabschnitts** -Element in die Stamm `Web.config` Datei einfügen, fügen Sie den-Abschnitt direkt hinter das- **Konfigurations** Element ein. Im folgenden Beispiel wird gezeigt, wie der obere Teil der Stamm `Web.config` Datei aussehen sollte, wenn Sie das Verschieben der Elemente abgeschlossen haben.

> [!NOTE]
> Im folgenden Beispiel wurden Zeilen zur besseren Lesbarkeit umschließt.

[!code-xml[Main](breaking-changes/samples/sample8.xml)]

### <a name="step-2-all-versions-of-iis"></a>Schritt 2 (alle Versionen von IIS)

Dieser Schritt ist erforderlich, unabhängig davon, ob die untergeordnete Webanwendung ASP.NET 4 unter IIS 6 oder IIS 7 (oder IIS 7,5) ausgeführt wird.

Fügen Sie in der `Web.config`-Datei der übergeordneten Webanwendung, auf der ASP.NET 2 oder ASP.NET 3,5 ausgeführt wird, ein **Location** -Tag hinzu, das explizit (für das IIS-und das ASP.NET-Konfigurationssystem) angibt, dass die Konfigurationseinträge nur für die übergeordnete Webanwendung gelten. Das folgende Beispiel zeigt die Syntax des hinzu zufügenden **Location** -Elements:

[!code-xml[Main](breaking-changes/samples/sample9.xml)]

Das folgende Beispiel zeigt, wie das **Location** -Tag verwendet wird, um alle Konfigurations Abschnitte zu umschließen, beginnend mit dem **appSettings** -Abschnitt und mit dem **System. Webserver** -Abschnitt.

[!code-xml[Main](breaking-changes/samples/sample10.xml)]

Wenn Sie die Schritte 1 und 2 abgeschlossen haben, werden untergeordnete ASP.NET 4-Webanwendungen ohne Fehler gestartet.

<a id="0.1__Toc252995491"></a><a id="0.1__Toc255587640"></a><a id="0.1__Toc256770151"></a>

## <a name="aspnet-4-web-sites-fail-to-start-on-computers-where-sharepoint-is-installed"></a>ASP.NET 4 Websites können auf Computern, auf denen SharePoint installiert ist, nicht gestartet werden.

Webserver, auf denen SharePoint ausgeführt wird, verfügen über eine `Web.config`-Datei, die im Stammverzeichnis einer SharePoint-Website bereitgestellt wird (z. b. `c:\inetpub\wwwroot\web.config` für Standard Website). In dieser `Web.config` Datei legt SharePoint eine benutzerdefinierte teilweise vertrauenswürdige Ebene mit dem Namen WSS\_minimal fest.

Wenn Sie versuchen, eine ASP.NET 4-Website auszuführen, die als untergeordnetes Element dieses Typs von SharePoint-Website bereitgestellt wird, wird der folgende Fehler angezeigt:

`Could not find permission set named 'ASP.NET'.`

Dieser Fehler tritt auf, weil die ASP.NET 4-CAS-Infrastruktur (Code Access Security) nach einem Berechtigungs Satz mit dem Namen ASP.NET sucht. Die Konfigurationsdatei mit teilweiser Vertrauenswürdigkeit, auf die von WSS\_Minimum verwiesen wird, enthält jedoch keine Berechtigungs Sätze mit diesem Namen.

Zurzeit ist keine Version von SharePoint verfügbar, die mit ASP.NET kompatibel ist. Daher sollten Sie nicht versuchen, eine ASP.NET 4-Website als untergeordneten Standort unterhalb von SharePoint-Websites auszuführen.

<a id="0.1__Toc255587641"></a><a id="0.1__Toc256770152"></a>

## <a name="the-httprequestfilepath-property-no-longer-includes-pathinfo-values"></a>Die HttpRequest. FilePath-Eigenschaft enthält keine PathInfo-Werte mehr.

In früheren Versionen von ASP.net wurde ein **pathinfo** -Wert in den Wert eingefügt, der von verschiedenen Dateipfad bezogenen Eigenschaften zurückgegeben wurde, einschließlich **HttpRequest. FilePath**, **HttpRequest. AppRelativeCurrentExecutionFilePath**und **HttpRequest. Currency texecutionfilepath**. ASP.NET 4 enthält nicht mehr den **pathinfo** -Wert in den Rückgabe Werten aus diesen Eigenschaften. Stattdessen sind die **pathinfo** -Informationen in **HttpRequest. PathInfo**verfügbar. Nehmen wir z.B. das folgenden URL-Fragment:

`/testapp/Action.mvc/SomeAction`

In früheren Versionen von ASP.net haben **HttpRequest** -Eigenschaften die folgenden Werte:

**HttpRequest. FilePath**: `/testapp/Action.mvc/SomeAction`

**HttpRequest. PathInfo**: (leer)

In ASP.NET 4 haben die **HttpRequest** -Eigenschaften stattdessen die folgenden Werte:

**HttpRequest. FilePath**: `/testapp/Action.mvc`

**HttpRequest. PathInfo**: `SomeAction`

<a id="0.1__Toc252995493"></a><a id="0.1__Toc255587642"></a><a id="0.1__Toc256770153"></a><a id="0.1__Toc245724861"></a>

## <a name="aspnet-20-applications-might-generate-httpexception-errors-that-reference-eurlaxd"></a>ASP.NET 2,0-Anwendungen generieren möglicherweise HttpException-Fehler, die auf "EURL. axd" verweisen

Nachdem ASP.NET 4 für IIS 6 aktiviert wurde, generieren ASP.NET 2.0-Anwendungen, die auf IIS 6 (entweder unter Windows Server 2003 oder Windows Server 2003 R2) ausgeführt werden, möglicherweise Fehler wie den folgenden:

`System.Web.HttpException: Path '/[yourApplicationRoot]/eurl.axd/[Value]' was not found.`

Dieser Fehler tritt auf, weil ASP.net erkennt, dass eine Website für die Verwendung von ASP.NET 4 konfiguriert ist, eine native Komponente von ASP.NET 4 eine erweiterbare URL an den verwalteten Teil von ASP.net zur weiteren Verarbeitung übergibt. Wenn jedoch virtuelle Verzeichnisse unter einer ASP.NET 4-Website für die Verwendung von ASP.NET 2,0 konfiguriert sind, führt die Verarbeitung der Erweiterungs losen URL auf diese Weise zu einer geänderten URL, die die Zeichenfolge "EURL. axd" enthält. Diese geänderte URL wird dann an die Anwendung ASP.NET 2,0 gesendet. ASP.NET 2,0 kann das Format "EURL. axd" nicht erkennen. Daher versucht ASP.NET 2,0, eine Datei namens `eurl.axd` zu finden und auszuführen. Da keine solche Datei vorhanden ist, schlägt die Anforderung mit einer **HttpException** -Ausnahme fehl.

Sie können dieses Problem umgehen, indem Sie eine der folgenden Optionen verwenden.

### <a name="option-1"></a>Option 1:

Wenn ASP.NET 4 nicht erforderlich ist, um die Website auszuführen, ordnen Sie die Website stattdessen der Verwendung von ASP.NET 2,0 zu.

### <a name="option-2"></a>Option 2:

Wenn ASP.NET 4 erforderlich ist, um die Website auszuführen, verschieben Sie alle untergeordneten virtuellen Verzeichnisse ASP.NET 2,0 auf eine andere Website, die ASP.NET 2,0 zugeordnet ist.

### <a name="option-3"></a>Option 3

Wenn es nicht praktikabel ist, die Website zu ASP.NET 2,0 neu zuzuordnen oder den Speicherort eines virtuellen Verzeichnisses zu ändern, deaktivieren Sie die Erweiterungs freie URL-Verarbeitung in ASP.NET 4 explizit. Gehen Sie dazu wie folgt vor:

1. Öffnen Sie in der Windows-Registrierung den folgenden Knoten:

`HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\ASP.NET\4.0.30319.0`

1. Erstellen Sie einen neuen **DWORD** -Wert mit dem Namen **enableextensionlessurls**.
2. Legen Sie **enableextensionlessurls** auf 0 fest. Dadurch wird das URL-Verhalten ohne Erweiterungen deaktiviert.
3. Speichern Sie den Registrierungs Wert, und schließen Sie den Registrierungs-Editor.
4. Führen Sie das Befehlszeilen Tool **iisreset** aus, das IIS dazu veranlasst, den neuen Registrierungs Wert zu lesen.

> [!NOTE]
> Wenn **enableextensionlessurls** auf 1 festgelegt wird, wird das URL-Verhalten ohne Erweiterung aktiviert. Dies ist die Standardeinstellung, wenn kein Wert angegeben wird.

<a id="0.1__Toc252995494"></a><a id="0.1__Toc255587643"></a><a id="0.1__Toc256770154"></a><a id="0.1__Toc245724862"></a>

## <a name="event-handlers-might-not-be-not-raised-in-a-default-document-in-iis-7-or-iis-75-integrated-mode"></a>Ereignishandler werden möglicherweise nicht in einem Standarddokument in IIS 7 oder im integrierten IIS 7,5-Modus ausgelöst.

ASP.NET 4 enthält Änderungen, die ändern, wie das **Action** -Attribut des HTML- **Formular** Elements gerendert wird, wenn eine erweiterbare URL in ein Standarddokument aufgelöst wird. Ein Beispiel für eine erweiterbare URL, die in ein Standarddokument aufgelöst wird, wird [http://contoso.com/](http://contoso.com/), was zu einer Anforderung an [http://contoso.com/Default.aspx](http://contoso.com/Default.aspx)führt.

ASP.NET 4 rendert jetzt den **Aktions** Attribut Wert des HTML- **Formular** Elements als leere Zeichenfolge, wenn eine Anforderung an eine erweiterbare URL gerichtet ist, der ein Standarddokument zugeordnet ist. In früheren Versionen von ASP.net würde eine Anforderung an [http://contoso.com](http://contoso.com) z. b. zu einer Anforderung an `Default.aspx`führen. In diesem Dokument würde das öffnende **Formular** Tag wie im folgenden Beispiel gerendert werden:

`<form action="Default.aspx" />`

In ASP.NET 4 führt eine Anforderung an [http://contoso.com](http://contoso.com) auch zu einer Anforderung an `Default.aspx`. Allerdings rendert ASP.net jetzt das HTML-öffnende **Formular** Tag wie im folgenden Beispiel gezeigt:

`<form action="" />`

Dieser Unterschied, wie das **Action** -Attribut gerendert wird, kann zu geringfügigen Änderungen bei der Verarbeitung von Formular Post durch IIS und ASP.net führen. Wenn das **Action** -Attribut eine leere Zeichenfolge ist, erstellt das IIS **defaultdocumentmodule** -Objekt eine untergeordnete Anforderung zum `Default.aspx`. Unter den meisten Bedingungen ist diese untergeordnete Anforderung für Anwendungscode transparent, und die `Default.aspx` Seite wird normal ausgeführt.

Allerdings kann eine mögliche Interaktion zwischen verwaltetem Code und IIS 7 oder IIS 7.5 im integrierten Modus dazu führen, dass verwaltete ASPX-Seiten während der untergeordnete Anforderung nicht mehr ordnungsgemäß ausgeführt werden. Wenn die folgenden Bedingungen eintreten, führt die untergeordnete Anforderung an ein `Default.aspx` Dokument zu einem Fehler oder unerwartetem Verhalten:

1. Eine ASPX-Seite wird an den Browser gesendet, wobei das **Aktions** Attribut des **Formular** Elements auf "" festgelegt ist.
2. Das Formular wird zurück an ASP.NET gesendet.
3. Ein verwaltetes HTTP-Modul liest einen Teil des Entitäts Texts. Beispielsweise liest ein Modul " **Request. Form** " oder " **Request. para**". Aus diesem Grund wird der Einheitstextkörper der POST-Anforderung im verwalteten Arbeitsspeicher gelesen. Der Einheitstextkörper ist daher nicht mehr für native Codemodule verfügbar, die in IIS 7 oder IIS 7.5 im integrierten Modus ausgeführt werden.
4. Das IIS **defaultdocumentmodule** -Objekt wird schließlich ausgeführt, und es wird eine untergeordnete Anforderung an das `Default.aspx` Dokument erstellt. Da der Einheitstextkörper bereits von verwaltetem Code gelesen wurde, kann kein Einheitstextkörper an die untergeordnete Anforderung gesendet werden.
5. Wenn die HTTP-Pipeline für die untergeordnete Anforderung ausgeführt wird, wird der Handler für `.aspx` Dateien während der handlerausführungs-Phase ausgeführt.
6. Da es keinen entitätentext gibt, gibt es keine Formularvariablen und keinen Ansichts Zustand. Daher sind keine Informationen für den. aspx-Seiten Handler verfügbar, um zu bestimmen, welches Ereignis (sofern vorhanden) ausgelöst werden soll. Daher werden keine Postback-Ereignishandler für die betroffene ASPX-Seite ausgeführt.

Sie können dieses Verhalten auf folgende Weise umgehen:

- Identifizieren Sie das HTTP-Modul, das bei Standarddokument Anforderungen auf den Entitäts Text der Anforderung zugreift, und legen Sie fest, ob es für die nur für verwaltete Anforderungen ausgeführt werden kann. Im integrierten Modus für IIS 7 und IIS 7,5 können HTTP-Module so gekennzeichnet werden, dass Sie nur für verwaltete Anforderungen ausgeführt werden, indem das folgende Attribut dem **System. Webserver/modules** -Eintrag des Moduls hinzugefügt wird:

- `precondition="managedHandler"`

- Mit dieser Einstellung wird das Modul für Anforderungen deaktiviert, die von IIS 7 und IIS 7,5 als nicht verwaltete Anforderungen festgelegt werden. Bei Standarddokument Anforderungen erfolgt die erste Anforderung an eine erweiterbare URL. Daher führt IIS keine verwalteten Module aus, die während der anfänglichen Anforderungs Verarbeitung mit einer Vorbedingung des verwalteten Handlers gekennzeichnet sind. Daher wird der Entitäts Text von verwalteten Modulen nicht versehentlich gelesen. Folglich ist der Entitäts Text weiterhin verfügbar und wird an die untergeordnete Anforderung und an das Standarddokument weitergeleitet.

- Wenn die problematischen HTTP-Module für alle Anforderungen (bei statischen Dateien für erweiterbare URLs, die in das **defaultdocumentmodule** -Objekt aufgelöst werden, für verwaltete Anforderungen usw.) ausgeführt werden müssen, ändern Sie die betroffenen ASPX-Seiten, indem Sie die **Action** -Eigenschaft des **System. Web. UI. HtmlControls** Wenn das Standarddokument beispielsweise `Default.aspx`ist, ändern Sie den Code der Seite, um die **Action** -Eigenschaft des **HtmlForm** -Steuer Elements explizit auf "default. aspx" festzulegen.

<a id="0.1__Toc255587644"></a><a id="0.1__Toc256770155"></a>

## <a name="changes-to-the-aspnet-code-access-security-cas-implementation"></a>Änderungen an der Implementierung der ASP.NET-Code Zugriffssicherheit (CAS)

ASP.NET 2,0 und durch Erweiterung die ASP.NET-Features, die in 3,5 hinzugefügt wurden, verwenden das .NET Framework 1,1-und 2,0-Code Zugriffs Sicherheitsmodell (CAS). Die Implementierung von CAS wurde in ASP.NET 4 aber erheblich überholt. Demzufolge können teilweise vertrauenswürdige ASP.NET-Anwendungen, die auf vertrauenswürdigem Code basieren, der im globalen Assemblycache (GAC) ausgeführt wird, mit verschiedenen Sicherheits Ausnahmen fehlschlagen. Teilweise vertrauenswürdige Anwendungen, die umfangreiche Änderungen an der Richtlinie für die Computer Zertifizierungsstellen benötigen, können auch mit Sicherheits Ausnahmen fehlschlagen.

Sie können teilweise vertrauenswürdige ASP.NET 4-Anwendungen auf das Verhalten von ASP.NET 1,1 und 2,0 zurücksetzen, indem Sie das neue **legacycasmodel** -Attribut im **Trust** Configuration-Element verwenden, wie im folgenden Beispiel gezeigt:

`<trust level= "Medium" legacyCasModel="true" />`

Wenn Sie das Legacy-CAS-Modell zurücksetzen, sind die folgenden alten CAS-Verhaltensweisen aktiviert:

- Die Computer-CAS-Richtlinie wird berücksichtigt.
- Mehrere verschiedene Berechtigungs Sätze in einer einzelnen Anwendungsdomäne sind zulässig.
- Explizite Berechtigungs Assertionen sind nicht für Assemblys im GAC erforderlich, die aufgerufen werden, wenn sich nur ASP.net oder ein anderer .NET Framework Code im Stapel befindet.

Ein Szenario kann in der .NET Framework 4 nicht wieder hergestellt werden: nicht-Webanwendungen mit teilweiser Vertrauenswürdigkeit können bestimmte APIs in "System. Web. dll" und "System. Web. Extensions. dll" nicht mehr abrufen. In früheren Versionen der .NET Framework war es möglich, dass nicht-Webanwendungen mit teilweiser Vertrauenswürdigkeit explizit **aspnetthostingberechtigungs** Berechtigungen erteilt wurden. Diese Anwendungen können dann **System. Web. HttpUtility**, Typen in den **System. Web. Client Services.\*** -Namespaces und Typen verwenden, die mit der Mitgliedschaft, den Rollen und Profilen verknüpft sind. Das Aufrufen dieser Typen aus nicht-Webanwendungen mit teilweiser Vertrauenswürdigkeit wird in den .NET Framework 4 nicht mehr unterstützt.

> [!NOTE]
> Die Funktionen **HtmlEncode** und **HtmlDecode** der **System. Web. HttpUtility** -Klasse wurden in die neue **System .net. WebUtility** -Klasse von .NET Framework 4 verschoben. Wenn dies die einzige ASP.NET-Funktionalität war, die verwendet wurde, ändern Sie den Code der Anwendung so, dass stattdessen die neue **WebUtility** -Klasse verwendet wird.

Im folgenden finden Sie eine allgemeine Zusammenfassung der Änderungen an der standardmäßigen CAS-Implementierung in ASP.NET 4:

- ASP.NET Anwendungs Domänen sind nun homogene Anwendungs Domänen. In einer Anwendungsdomäne sind nur teilweise vertrauenswürdige und voll vertrauenswürdige Grant Sets verfügbar.
- ASP.net-Zuweisungs Sätze mit teilweiser Vertrauenswürdigkeit sind von allen CAS-Richtlinien auf Unternehmensebene, auf Computer Ebene oder auf Benutzerebene unabhängig.
- ASP.NET-Assemblys, die in 3,5 und 3,5 SP1 ausgeliefert wurden, wurden konvertiert, um das .NET Framework 4-Transparenz Modell zu verwenden.
- Die Verwendung des ASP.net **asparthostingberechtigung** -Attributs wurde erheblich reduziert. Die meisten Instanzen dieses Attributs wurden aus den öffentlichen ASP.NET-APIs entfernt.
- Dynamisch kompilierte Assemblys, die von ASP.net Build-Anbietern erstellt wurden, wurden aktualisiert, um Assemblys explizit als transparent zu markieren
- Alle ASP.NET-Assemblys sind nun so gekennzeichnet, dass das APTCA-Attribut nur in Webhostingumgebungen berücksichtigt wird. Teilweise vertrauenswürdige nicht-Webhostingumgebungen wie ClickOnce können nicht in ASP.NET-Assemblys aufgerufen werden.

Weitere Informationen zum neuen ASP.NET 4-Code Zugriffs Sicherheitsmodell finden Sie unter [Verwenden der Code Zugriffssicherheit in ASP.NET-Anwendungen](https://msdn.microsoft.com/library/dd984947%28VS.100%29.aspx) auf der MSDN-Website.

<a id="0.1__Toc256770156"></a><a id="0.1__Toc245724863"></a><a id="0.1__Toc252995496"></a><a id="0.1__Toc255587645"></a><a id="0.1__Toc245724864"></a>

## <a name="membershipuser-and-other-types-in-the-systemwebsecurity-namespace-have-been-moved"></a>Mitgliedshipuser und andere Typen im System. Web. Security-Namespace wurden verschoben.

Einige Typen, die in der ASP.NET-Mitgliedschaft verwendet werden, wurden aus `System.Web.dll` in die neue Assembly "System. Web. ApplicationServices. dll" verschoben. So sollten architektonische Schichtabhängigkeiten zwischen Typen im Client und erweiterten .NET Framework-SKUs gelöst werden.

Website Projekte haben aufgrund der Verschiebung dieser Typen keine Probleme, da "System. Web. ApplicationServices. dll" der Liste der referenzierten Assemblys hinzugefügt wurde, die standardmäßig vom ASP.net-Kompilierungs System verwendet werden. Wenn Sie ein-Website Projekt, das mit einer früheren Version von ASP.NET erstellt wurde, auf ASP.NET 4 aktualisieren, indem Sie es in Visual Studio 2010 öffnen, wird das Projekt ohne Fehler kompiliert.

Wenn Sie ein Webanwendungs Projekt, das in einer früheren Version von ASP.NET erstellt wurde, auf ASP.NET 4 aktualisieren, indem Sie es in Visual Studio 2010 öffnen, fügt der Upgradevorgang dem Projekt einen Verweis auf "System. Web. ApplicationServices. dll" hinzu. Daher werden aktualisierte Webanwendungs Projekte auch ohne Fehler kompiliert.

Kompilierte (binäre) Dateien, die mit früheren Versionen von ASP.NET erstellt wurden, werden auch ohne Fehler auf ASP.NET 4 ausgeführt, auch wenn die Mitgliedschafts Typen in eine andere Assembly verschoben wurden. Die Informationen zur Typweiterleitung wurden der ASP.NET 4-Version von `System.Web.dll` hinzugefügt, die automatisch Lauf Zeit Verweise für diese Typen an den neuen Speicherort für die Typen weiterleitet.

Klassenbibliotheken, die bestimmte Mitgliedschafts Typen verwenden und die von früheren Versionen von ASP.NET aktualisiert wurden, können jedoch nicht kompiliert werden, wenn Sie in einem ASP.NET 4-Projekt verwendet werden. Beispielsweise kann ein Klassen Bibliotheksprojekt möglicherweise nicht kompiliert werden, und es wird ein Fehler wie der folgende gemeldet:

- `The type 'System.Web.Security.MembershipUser' is defined in an assembly that is not referenced. You must add a reference to assembly 'System.Web.ApplicationServices, Version=4.0.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35'.`

- `The type name 'MembershipUser' could not be found. This type has been forwarded to assembly 'System.Web.ApplicationServices, Version=4.0.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35'. Consider adding a reference to that assembly.`

Sie können dieses Problem umgehen, indem Sie dem Klassen Bibliotheksprojekt einen Verweis auf "System. Web. ApplicationServices. dll" hinzufügen.

In der folgenden Liste sind die *System. Web. Security* -Typen aufgeführt, die von `System.Web.dll` in System. Web. ApplicationServices. dll verschoben wurden:

- *System. Web. Security. Membership shipkreatestatus*
- *System. Web. Security. Membership. kreateuserexception*
- *System. Web. Security. Membership shippasswordexception*
- *System. Web. Security. Membership shippasswordformat*
- *System. Web. Security. Membership shipprovider*
- *System. Web. Security. Membership shipprovidercollection*
- *System. Web. Security. Membership shipuser*
- *System. Web. Security. Membership shipusercollection*
- *System. Web. Security. mitgliedshipvalidatepassworreventhandler*
- *System. Web. Security. validatepassworabventargs*
- *System. Web. Security. RoleProvider*
- <a id="0.1_a"></a>*System. Web. Configuration. mitgliedshippasswordcompatibilitymode*

<a id="0.1__Toc256770157"></a>

## <a name="output-caching-changes-to-vary--http-header"></a>Änderungen der Ausgabe Zwischenspeicherung, um \* HTTP-Header variieren

In ASP.NET 1,0 verursachte ein Bug zwischengespeicherten Seiten, die `Location="ServerAndClient"` als Output – Cache-Einstellung angegeben haben, um einen `Vary:*`-HTTP-Header in der Antwort auszugeben. So wurde Clientbrowsern mitgeteilt, dass Seiten nie lokal zwischengespeichert werden sollen.

In ASP.NET 1,1 wurde die **System. Web. HttpCachePolicy. *-** Methode hinzugefügt, die aufgerufen werden konnte, um den `Vary:*`-Header zu unterdrücken. Diese Methode wurde ausgewählt, weil das Ändern des ausgegebenen HTTP-Headers zu diesem Zeitpunkt als potenziell Breaking Change galt. Entwickler sind jedoch durch das Verhalten in ASP.net verwirrt worden, und Fehlerberichte deuten darauf hin, dass Entwickler **das vorhandene Verhalten** von "" "" "" "" "" "" "" "" "

In ASP.NET 4 wurde die Entscheidung getroffen, das Problem zu beheben. Der `Vary:*`-HTTP-Header wird nicht mehr von Antworten ausgegeben, die die folgende Direktive angeben:

`<%@OutputCache Location="ServerAndClient" %>`

Daher wird **setmitvarystar** nicht mehr benötigt, um den `Vary:*`-Header zu unterdrücken.

In Anwendungen, die `Location="ServerAndClient"` in der **@ OutputCache** -Direktive auf einer Seite angeben, sehen Sie nun das Verhalten, das durch den Namen des **Location** -Attribut Werts impliziert wird – das heißt, dass Seiten im Browser zwischengespeichert werden können, ohne dass **Sie die Methode** "" der Methode "" mit dem Namen "" aufrufen müssen.

Wenn die Seiten in der Anwendung `Vary:*`ausgeben müssen, wenden Sie die **Appendheader** -Methode wie im folgenden Beispiel an:

`HttpResponse.AppendHeader("Vary","*");`

Alternativ können Sie den Wert **des Attributs** für das Ausgabe Zwischenspeichern in "Server" ändern.

<a id="0.1__Toc255587646"></a><a id="0.1__Toc256770158"></a>

## <a name="systemwebsecurity-types-for-passport-are-obsolete"></a>System. Web. Sicherheits Typen für Passport sind veraltet.

Die in ASP.NET 2,0 integrierte Passport-Unterstützung ist seit einigen Jahren veraltet und wird aufgrund von Änderungen in Passport (now LiveID) nicht mehr unterstützt. Folglich sind die fünf Typen, die sich auf Passport in **System. Web. Security** beziehen, jetzt mit dem **ObsoleteAttribute** -Attribut gekennzeichnet.

<a id="0.1__The_MenuItem.PopOutImageUrl_Propert"></a><a id="0.1__Toc256770159"></a>

## <a name="the-menuitempopoutimageurl-property-fails-to-render-an-image-in-aspnet-4"></a>Die MenuItem. PopOutImageUrl-Eigenschaft kann ein Bild in ASP.NET 4 nicht Rendering

In ASP.NET 3,5 können Sie mit der Eigenschaft *MenuItem. PopOutImageUrl* die URL für ein Bild angeben, das in einem Menü Element angezeigt wird, um anzugeben, dass das Menü Element über ein dynamisches Untermenü verfügt. Im folgenden Beispiel wird gezeigt, wie Sie diese Eigenschaft im Markup in ASP.NET 3,5 angeben.

[!code-aspx[Main](breaking-changes/samples/sample11.aspx)]

Aufgrund einer Entwurfs Änderung in ASP.NET 4 wird keine Ausgabe für die *PopOutImageUrl* gerendert, wenn die Eigenschaft für die *MenuItem* -Klasse festgelegt ist. Stattdessen müssen Sie eine Bild-URL direkt im *Menü* Steuerelement angeben, indem Sie entweder die *StaticPopOutImageUrl* -Eigenschaft oder die *DynamicPopOutImageUrl* -Eigenschaft verwenden. Wenn Sie mit einem statischen Menü arbeiten, gibt die Eigenschaft *Menu. StaticPopOutImageUrl* die URL für ein Bild an, das angezeigt wird, um anzugeben, dass das statische Menü Element über ein Untermenü verfügt, wie im folgenden Beispiel gezeigt:

[!code-aspx[Main](breaking-changes/samples/sample12.aspx)]

Wenn Sie mit einem dynamischen Menü arbeiten, verwenden Sie die *Menü. DynamicPopOutImageUrl* -Eigenschaft, um die URL für ein Bild anzugeben, das angibt, dass ein dynamisches Menü Element über ein Untermenü verfügt. Das folgende Beispiel ähnelt dem vorherigen, zeigt jedoch, wie die *DynamicPopOutImageUrl* -Eigenschaft für ein dynamisches Menü festgelegt wird.

[!code-aspx[Main](breaking-changes/samples/sample13.aspx)]

Wenn die Eigenschaft *Menu. DynamicPopOutImageUrl* nicht festgelegt ist und die Eigenschaft *Menu. DynamicEnableDefaultPopOutImage* auf *false*festgelegt ist, wird kein Bild angezeigt. Wenn die Eigenschaft *StaticPopOutImageUrl* nicht festgelegt ist und die Eigenschaft *StaticEnableDefaultPopOutImage* auf *false*festgelegt ist, wird kein Bild angezeigt.

Wenn Sie die Pfade für diese Eigenschaften festlegen, verwenden Sie einen Schrägstrich (/) anstelle eines umgekehrten Schrägstrichs (\). Weitere Informationen finden Sie unter [Menu. StaticPopOutImageUrl und Menu. DynamicPopOutImageUrl Fehler beim Rendering von Bildern, wenn Pfade](#0.1__Menu.StaticPopOutImageUrl_and_Menu. "_Menu. StaticPopOutImageUrl_and_Menu.") andere Links in diesem Dokument enthalten.

<a id="0.1__Menu.StaticPopOutImageUrl_and_Menu."></a><a id="0.1__Toc256770160"></a>

## <a name="menustaticpopoutimageurl-and-menudynamicpopoutimageurl-fail-to-render-images-when-paths-contain-backslashes"></a>"Menu. StaticPopOutImageUrl" und "Menu. DynamicPopOutImageUrl" können keine Bilder darstellen, wenn Pfade umgekehrte Schrägstriche enthalten.

In ASP.NET 4 werden die Bilder, die Sie mithilfe der Eigenschaften " *Menu. StaticPopOutImageUrl* " und " *Menu. DynamicPopOutImageUrl* " angeben, nicht angezeigt, wenn der Pfad Rück Striche (\)enthält. Dies ist eine Änderung gegenüber früheren Versionen von ASP.net.

Im folgenden Beispiel für *Menü* Steuerelement Markup wird die *StaticPopOutImageUrl* -Eigenschaft mit einem Pfad mit einem umgekehrten Schrägstrich angezeigt. In ASP.NET 4 wird das in der-Eigenschaft angegebene Bild nicht angezeigt.

[!code-aspx[Main](breaking-changes/samples/sample14.aspx)]

Um dieses Problem zu beheben, ändern Sie in den Eigenschaften *StaticPopOutImageUrl* und *DynamicPopOutImageUrl* angegebene Pfad Werte, um Schrägstriche (/) zu verwenden. Das folgende Beispiel zeigt diese Änderung:

[!code-aspx[Main](breaking-changes/samples/sample15.aspx)]

Beachten Sie, dass Anwendungen, die von früheren Versionen von ASP.net zu ASP.NET 4 migriert wurden, möglicherweise ebenfalls beeinträchtigt werden, da die *MenuItem. PopOutImageUrl* -Eigenschaft geändert wurde. Weitere Informationen finden Sie in [der MenuItem. PopOutImageUrl-Eigenschaft beim Rendering eines Bilds in ASP.NET 4](#0.1__The_MenuItem.PopOutImageUrl_Propert "_The_MenuItem. PopOutImageUrl_Propert") an anderer Stelle in diesem Dokument.

<a id="0.1__Toc224729061"></a><a id="0.1__Toc255587647"></a><a id="0.1__Toc256770161"></a>

## <a name="disclaimer"></a>Haftungsausschluss

Dies ist ein vorläufiges Dokument, das vor der kommerziellen Veröffentlichung der beschriebenen Software ggf. erheblich geändert wird.

Die in diesem Dokument enthaltenen Informationen stellen die Sicht der Microsoft Corporation der hier diskutierten Themen zum Zeitpunkt der Veröffentlichung dar. Da Microsoft auf wechselnde Marktbedingungen reagieren muss, sollten sie nicht als Verpflichtung seitens Microsoft interpretiert werden, und Microsoft kann die Genauigkeit der dargelegten Informationen nach dem Zeitpunkt der Veröffentlichung nicht garantieren.

Dieses Whitepaper dient ausschließlich Informationszwecken. MICROSOFT ÜBERNIMMT KEINE AUSDRÜCKLICHE, STILLSCHWEIGENDE ODER AUS GESETZ ERWACHSENDE GARANTIE IN BEZUG AUF DIE INFORMATIONEN IN DIESEM DOKUMENT.

Die Benutzer/innen sind verpflichtet, sich an alle anwendbaren Urheberrechtsgesetze zu halten. Unabhängig von der Anwendbarkeit der entsprechenden Urheberrechtsgesetze darf kein Teil dieses Dokuments ohne ausdrückliche schriftliche Erlaubnis der Microsoft Corporation für irgendwelche Zwecke vervielfältigt oder in einem Datenempfangssystem gespeichert oder darin eingelesen werden, unabhängig davon, auf welche Art und Weise oder mit welchen Mitteln (elektronisch, mechanisch, durch Fotokopieren, Aufzeichnen usw.) dies geschieht.

Microsoft Corporation kann Inhaber von Patenten oder Patentanträgen, Marken, Urheberrechten oder anderem geistigen Eigentum sein, die den Inhalt dieses Dokuments betreffen. Die Bereitstellung dieses Dokuments gewährt keinerlei Lizenzrechte an diesen Patenten, Marken, Urheberrechten oder anderem geistigen Eigentum, es sei denn, dies wurde ausdrücklich durch einen schriftlichen Lizenzvertrag mit der Microsoft Corporation vereinbart.

Sofern nicht anders angegeben, sind die hier dargestellten Beispiel Unternehmen, Organisationen, Produkte, Domänen Namen, e-Mail-Adressen, Logos, Personen, Orte und Ereignisse fiktiv und keine Zuordnung zu echten Unternehmen, Organisationen, Produkten, Domänen Namen, e-Mail-Adressen. Adresse, Logo, Person, Ort oder Ereignis ist beabsichtigt oder sollte abgeleitet werden.

© 2010 Microsoft Corporation. Alle Rechte vorbehalten.

Microsoft und Windows sind eingetragene Marken oder Marken der Microsoft Corporation in den USA und/oder anderen Ländern.

Die in diesem Dokument erwähnten Namen von Unternehmen und Produkten können Marken der jeweiligen Eigentümer sein.
