---
uid: web-pages/overview/api-reference/asp-net-web-pages-api-reference
title: ASP.net Web Pages (Razor) API-Kurzübersicht | Microsoft-Dokumentation
author: Rick-Anderson
description: Diese Seite enthält eine Liste mit kurzen Beispielen zu den am häufigsten verwendeten Objekten, Eigenschaften und Methoden für die Programmierung von ASP.net Web Pages mit Razor-Syntax.
ms.author: riande
ms.date: 02/10/2014
ms.assetid: 4001cb9b-3bfd-4ace-8a89-1561d8421e2c
msc.legacyurl: /web-pages/overview/api-reference/asp-net-web-pages-api-reference
msc.type: authoredcontent
ms.openlocfilehash: e010307fc0576e8b003fbfe665cae77618d9c9a5
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78463581"
---
# <a name="aspnet-web-pages-razor-api-quick-reference"></a>ASP.net Web Pages (Razor) API-kurz Referenz

von [Tom fitzmacken](https://github.com/tfitzmac)

> Diese Seite enthält eine Liste mit kurzen Beispielen zu den am häufigsten verwendeten Objekten, Eigenschaften und Methoden für die Programmierung von ASP.net Web Pages mit Razor-Syntax.
> 
> Mit "(v2)" markierte Beschreibungen wurden in ASP.net Web Pages Version 2 eingeführt.
> 
> Eine API-Referenz Dokumentation finden Sie in der [ASP.net Web Pages Referenz Dokumentation](https://go.microsoft.com/fwlink/?LinkId=208659) auf MSDN.
> 
> ## <a name="software-versions"></a>Software Versionen
> 
> 
> - ASP.net Web Pages (Razor) 3
>   
> 
> Dieses Tutorial funktioniert auch mit ASP.net Web Pages 2 und ASP.net Web Pages 1,0 (mit Ausnahme von Features, die als V2 gekennzeichnet sind).

Diese Seite enthält Referenzinformationen für Folgendes:

- [Klassen](#Classes)
- [Daten](#Data)
- [Hospiz](#Helpers)
- [Überprüfung](#Validation)

<a id="Classes"></a>
## <a name="classes"></a>Klassen

### `AppState[key], AppState[index],App`

Enthält Daten, die von beliebigen Seiten in der Anwendung gemeinsam verwendet werden können. Sie können die Eigenschaft dynamischer `App` für den Zugriff auf dieselben Daten verwenden, wie im folgenden Beispiel gezeigt:

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample1.html)]

### `AsBool(), AsBool(true|false)`

Konvertiert einen Zeichen folgen Wert in einen booleschen Wert (true/false). Gibt false oder den angegebenen Wert zurück, wenn die Zeichenfolge nicht true/false darstellt.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample2.cs)]

### `AsDateTime(), AsDateTime(value)`

Konvertiert einen Zeichen folgen Wert in Datum/Uhrzeit. Gibt `DateTime.MinValue` oder den angegebenen Wert zurück, wenn die Zeichenfolge keine Datums-/Uhrzeitangaben darstellt.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample3.cs)]

### `AsDecimal(), AsDecimal(value)`

Konvertiert einen Zeichen folgen Wert in einen Dezimalwert. Gibt 0,0 oder den angegebenen Wert zurück, wenn die Zeichenfolge keinen Dezimalwert darstellt.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample4.cs)]

### `AsFloat(), AsFloat(value)`

Konvertiert einen Zeichen folgen Wert in einen float-Wert. Gibt 0,0 oder den angegebenen Wert zurück, wenn die Zeichenfolge keinen Dezimalwert darstellt.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample5.cs)]

### `AsInt(), AsInt(value)`

Konvertiert einen Zeichen folgen Wert in eine ganze Zahl. Gibt 0 oder den angegebenen Wert zurück, wenn die Zeichenfolge keine Ganzzahl darstellt.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample6.cs)]

### `Href(path [, param1 [, param2]])`

Erstellt eine Browser kompatible URL aus einem lokalen Dateipfad mit optionalen zusätzlichen Pfad teilen.

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample7.cshtml)]

### `Html.Raw(value)`

Rendert den *Wert* als HTML-Markup, anstatt ihn als HTML-codierte Ausgabe zu rendern.

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample8.cshtml)]

### `IsBool(), IsDateTime(), IsDecimal(), IsFloat(), IsInt()`

Gibt true zurück, wenn der Wert von einer Zeichenfolge in den angegebenen Typ konvertiert werden kann.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample9.cs)]

### `IsEmpty()`

Gibt "true" zurück, wenn das Objekt oder die Variable keinen Wert besitzt.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample10.cs)]

### `IsPost`

Gibt true zurück, wenn die Anforderung ein Post ist. (Anfängliche Anforderungen sind in der Regel eine GET-Anforderung.)

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample11.cs)]

### `Layout`

Gibt den Pfad einer Layoutseite an, die auf diese Seite angewendet werden soll.

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample12.html)]

### `PageData[key], PageData[index],Page`

Enthält Daten, die zwischen der Seite, den Layoutseiten und partiellen Seiten in der aktuellen Anforderung freigegeben werden. Sie können die Eigenschaft dynamischer `Page` für den Zugriff auf dieselben Daten verwenden, wie im folgenden Beispiel gezeigt:

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample13.html)]

### `RenderBody()`

(Layoutseiten) Rendert den Inhalt einer Inhaltsseite, die in keinem benannten Abschnitt vorhanden ist.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample14.cs)]

### `RenderPage(path, values)`  
`RenderPage(path[,param1 [, param2]])`

Rendert eine Inhaltsseite unter Verwendung des angegebenen Pfads und optionaler zusätzlicher Daten. Sie können die Werte der zusätzlichen Parameter von `PageData` nach Position (Beispiel 1) oder Schlüssel (Beispiel 2) erhalten.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample15.js)]

### `RenderSection(sectionName [, required = true|false])`

(Layoutseiten) Rendert einen Inhalts Abschnitt mit einem Namen. Legen Sie *erforderlich* auf false fest, um einen Abschnitt optional zu machen.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample16.js)]

### `Request.Cookies[key]`

Ruft den Wert eines HTTP-Cookies ab oder legt ihn fest.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample17.cs)]

### `Request.Files[key]`

Ruft die Dateien ab, die in der aktuellen Anforderung hochgeladen wurden.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample18.js)]

### `Request.Form[key]`

Ruft Daten ab, die in einem Formular (als Zeichen folgen) gepostet wurden. `Request[key]` überprüft sowohl die `Request.Form` als auch die `Request.QueryString` Sammlungen.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample19.cs)]

### `Request.QueryString[key]`

Ruft Daten ab, die in der URL-Abfrage Zeichenfolge angegeben wurden. `Request[key]` überprüft sowohl die `Request.Form` als auch die `Request.QueryString` Sammlungen.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample20.cs)]

### `Request.Unvalidated(key)`  
`Request.Unvalidated().QueryString|Form|Cookies|Headers[key]`

Deaktiviert die Anforderungs Validierung für ein Formular Element, einen Abfrage Zeichen folgen Wert, ein Cookie oder einen Header Wert selektiv. Die Anforderungs Validierung ist standardmäßig aktiviert und verhindert, dass Benutzer Markup oder andere potenziell gefährliche Inhalte veröffentlichen.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample21.cs)]

### `Response.AddHeader(name, value)`

Fügt der Antwort einen HTTP-Server Header hinzu.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample22.cs)]

### `Response.OutputCache(seconds [, sliding] [, varyByParams])`

Speichert die Seiten Ausgabe für einen angegebenen Zeitraum zwischen. Legen Sie optional einen *Gleit* enden Wert fest, um das Timeout auf den einzelnen Seiten Zugriffen zurückzusetzen, und *varybyparameams* , um unterschiedliche Versionen der Seite für jede andere Abfrage Zeichenfolge in der Seiten Anforderung

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample23.js)]

### `Response.Redirect(path)`

Leitet die Browser Anforderung an einen neuen Speicherort um.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample24.js)]

### `Response.SetStatus(httpStatusCode)`

Legt den HTTP-Statuscode fest, der an den Browser gesendet wird.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample25.cs)]

### `Response.WriteBinary(data [, mimetype])`

Schreibt den Inhalt der *Daten* in die Antwort mit einem optionalen MIME-Typ.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample26.js)]

### `Response.WriteFile(file)`

Schreibt den Inhalt einer Datei in die Antwort.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample27.cs)]

### `@section(sectionName) {content }`

(Layoutseiten) Definiert einen Inhalts Abschnitt mit einem Namen.

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample28.cshtml)]

### `Server.HtmlDecode(htmlText)`

Decodiert eine HTML-codierte Zeichenfolge.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample29.cs)]

### `Server.HtmlEncode(text)`

Codiert eine Zeichenfolge zum Rendern in HTML-Markup.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample30.cs)]

### `Server.MapPath(virtualPath)`

Gibt den physischen Pfad des Servers für den angegebenen virtuellen Pfad zurück.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample31.cs)]

### `Server.UrlDecode(urlText)`

Decodiert Text aus einer URL.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample32.cs)]

### `Server.UrlEncode(text)`

Codiert Text, der in eine URL eingefügt werden soll.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample33.cs)]

### `Session[key]`

Dient zum Abrufen oder Festlegen eines Werts, der vorhanden ist, bis der Benutzer den Browser schließt.

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample34.css)]

### `ToString()`

Zeigt eine Zeichen folgen Darstellung des Objekt Werts an.

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample35.html)]

### `UrlData[index]`

Ruft weitere Daten aus der URL ab (z. b. */MyPage/ExtraData*).

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample36.cs)]

### `WebSecurity.ChangePassword(userName,currentPassword,newPassword)`

Ändert das Kennwort für den angegebenen Benutzer.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample37.cs)]

### `WebSecurity.ConfirmAccount(accountConfirmationToken)`

Bestätigt ein Konto mit dem Konto Bestätigungs Token.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample38.cs)]

### `WebSecurity.CreateAccount(userName, password`  
 `[, requireConfirmationToken = true|false])`

Erstellt ein neues Benutzerkonto mit dem angegebenen Benutzernamen und Kennwort. Damit ein Bestätigungs Token erforderlich ist, übergeben Sie "true" für "Requirements *confirmationtoken".*

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample39.cs)]

### `WebSecurity.CurrentUserId`

Ruft den ganzzahligen Bezeichner für den aktuell angemeldeten Benutzer ab.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample40.cs)]

### `WebSecurity.CurrentUserName`

Ruft den Namen für den aktuell angemeldeten Benutzer ab.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample41.cs)]

### `WebSecurity.GeneratePasswordResetToken(username`  
 `[, tokenExpirationInMinutesFromNow])`

Generiert ein Token zum Zurücksetzen des Kennworts, das per e-Mail an einen Benutzer gesendet werden kann, sodass der Benutzer das Kennwort zurücksetzen kann.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample42.cs)]

### `WebSecurity.GetUserId(userName)`

Gibt die Benutzer-ID aus dem Benutzernamen zurück.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample43.cs)]

### `WebSecurity.IsAuthenticated`

Gibt true zurück, wenn der aktuelle Benutzer angemeldet ist.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample44.cs)]

### `WebSecurity.IsConfirmed(userName)`

Gibt "true" zurück, wenn der Benutzer bestätigt wurde (z. b. durch eine Bestätigungs-e-Mail).

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample45.cs)]

### `WebSecurity.IsCurrentUser(userName)`

Gibt true zurück, wenn der Name des aktuellen Benutzers mit dem angegebenen Benutzernamen übereinstimmt.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample46.cs)]

### `WebSecurity.Login(userName,password[, persistCookie])`

Protokolliert den Benutzer in, indem ein Authentifizierungs Token im Cookie festgelegt wird.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample47.cs)]

### `WebSecurity.Logout()`

Protokolliert den Benutzer, indem das Authentifizierungs Token-Cookie entfernt wird.

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample48.css)]

### `WebSecurity.RequireAuthenticatedUser()`

Wenn der Benutzer nicht authentifiziert ist, legt den HTTP-Status auf 401 (nicht autorisiert) fest.

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample49.css)]

### `WebSecurity.RequireRoles(roles)`

Wenn der aktuelle Benutzer kein Mitglied einer der angegebenen Rollen ist, wird der HTTP-Status auf 401 (nicht autorisiert) festgelegt.

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample50.html)]

### `WebSecurity.RequireUser(userId)`  
`WebSecurity.RequireUser(userName)`

Wenn der aktuelle Benutzer nicht der durch *username*angegebene Benutzer ist, legt den HTTP-Status auf 401 (nicht autorisiert) fest.

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample51.css)]

### `WebSecurity.ResetPassword(passwordResetToken,newPassword)`

Wenn das Token zum Zurücksetzen des Kennworts gültig ist, ändert das Kennwort des Benutzers in das neue Kennwort.

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample52.css)]

<a id="Data"></a>
## <a name="data"></a>Data

### `Database.Execute(SQLstatement [,parameters]`

Führt *SQLStatement* (mit optionalen Parametern) aus, z. b. Insert, DELETE oder Update, und gibt die Anzahl der betroffenen Datensätze zurück.

[!code-sql[Main](asp-net-web-pages-api-reference/samples/sample53.sql)]

### `Database.GetLastInsertId()`

Gibt die Identitäts Spalte aus der zuletzt eingefügten Zeile zurück.

[!code-sql[Main](asp-net-web-pages-api-reference/samples/sample54.sql)]

### `Database.Open(filename)`  
`Database.Open(connectionStringName)`

Öffnet entweder die angegebene Datenbankdatei oder die angegebene Datenbank unter Verwendung einer benannten Verbindungs Zeichenfolge aus der Datei " *Web. config* ".

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample55.cs)]

### `Database.OpenConnectionString(connectionString)`

Öffnet eine Datenbank mit der Verbindungs Zeichenfolge. (Dies steht im Gegensatz zu `Database.Open`, die einen Verbindungs Zeichenfolgen-Namen verwendet.)

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample56.cs)]

### `Database.Query(SQLstatement[,parameters])`

Fragt die Datenbank mithilfe von *SQLStatement* ab (optional übergeben von Parametern) und gibt die Ergebnisse als Auflistung zurück.

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample57.html)]

### `Database.QuerySingle(SQLstatement [, parameters])`

Führt *SQLStatement* (mit optionalen Parametern) aus und gibt einen einzelnen Datensatz zurück.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample58.cs)]

### `Database.QueryValue(SQLstatement [, parameters])`

Führt *SQLStatement* (mit optionalen Parametern) aus und gibt einen einzelnen Wert zurück.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample59.cs)]

<a id="Helpers"></a>
## <a name="helpers"></a>Hilfsmethoden

### `Analytics.GetGoogleHtml(webPropertyId)`

Rendert den Google Analytics-JavaScript-Code für die angegebene ID.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample60.js)]

### `Analytics.GetStatCounterHtml(project,security)`

Rendert den JavaScript-Code der Status Counter Analytics für das angegebene Projekt.

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample61.css)]

### `Analytics.GetYahooHtml(account)`

Rendert den JavaScript-Code für Yahoo Analytics für das angegebene Konto.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample62.js)]

### `Bing.SearchBox([boxWidth])`

Übergibt eine Suche an den Dienst. Zum Angeben der zu durchsuchenden Site und eines Titels für das Suchfeld können Sie die Eigenschaften `Bing.SiteUrl` und `Bing.SiteTitle` festlegen. Normalerweise legen Sie diese Eigenschaften auf der *\_AppStart* -Seite fest.

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample63.html)]

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample64.cshtml)]

### `Chart(width,height [, template] [, templatePath])`

Initialisiert ein Diagramm.

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample65.cshtml)]

### `Chart.AddLegend([title] [, name])`

Fügt einem Diagramm eine Legende hinzu.

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample66.cshtml)]

### `Chart.AddSeries([name] [, chartType] [, chartArea]`  
 `[, axisLabel] [, legend] [, markerStep] [, xValue]`  
 `[, xField] [, yValues] [, yFields] [, options])`

Fügt dem Diagramm eine Reihe von Werten hinzu.

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample67.cshtml)]

### `Crypto.Hash(string [, algorithm])`  
`Crypto.Hash(bytes [, algorithm])`

Gibt einen Hash für die angegebenen Daten zurück. Der Standard Algorithmus ist `sha256`.

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample68.html)]

### `Facebook.LikeButton(href [, buttonLayout] [, showFaces] [, width] [, height]`   
 `[, action] [, font] [, colorScheme] [, refLabel])`

Ermöglicht Facebook-Benutzern das Herstellen einer Verbindung mit Seiten.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample69.js)]

### `FileUpload.GetHtml([initialNumberOfFiles] [, allowMoreFilesToBeAdded]`  
 `[, includeFormTag] [, addText] [, uploadText])`

Rendert die Benutzeroberfläche zum Hochladen von Dateien.

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample70.html)]

### `GamerCard.GetHtml(gamerTag)`

Rendert das angegebene Xbox-gamendtag.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample71.js)]

### `Gravatar.GetHtml(email [, imageSize] [, defaultImage] [, rating]`  
 `[, imageExtension] [, attributes])`

Rendert das Gravatar-Bild für die angegebene e-Mail-Adresse.

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample72.css)]

### `Json.Encode(object)`

Konvertiert ein Datenobjekt in eine Zeichenfolge im JSON-Format (JavaScript Object Notation).

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample73.cs)]

### `Json.Decode(string)`

Konvertiert eine JSON-codierte Eingabe Zeichenfolge in ein Datenobjekt, das Sie durchlaufen oder in eine Datenbank einfügen können.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample74.cs)]

### `LinkShare.GetHtml(pageTitle[, pageLinkBack] [, twitterUserName]`  
 `[, additionalTweetText] [, linkSites])`

Rendert soziale Netzwerk Verknüpfungen unter Verwendung des angegebenen Titels und der optionalen URL.

[!code-xml[Main](asp-net-web-pages-api-reference/samples/sample75.xml)]

### `ModelStateDictionary.AddError(key, errorMessage)`

Ordnet eine Fehlermeldung einem Formularfeld zu. Verwenden Sie das `ModelState`-Hilfsprogramm, um auf diesen Member zuzugreifen.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample76.cs)]

### `ModelStateDictionary.AddFormError(errorMessage)`

Verknüpft eine Fehlermeldung mit einem Formular. Verwenden Sie das `ModelState`-Hilfsprogramm, um auf diesen Member zuzugreifen.

[!code-powershell[Main](asp-net-web-pages-api-reference/samples/sample77.ps1)]

### `ModelStateDictionary.IsValid`

Gibt true zurück, wenn keine Validierungs Fehler vorliegen. Verwenden Sie das `ModelState`-Hilfsprogramm, um auf diesen Member zuzugreifen.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample78.cs)]

### `ObjectInfo.Print(value [, depth] [, enumerationLength])`

Rendert die Eigenschaften und Werte eines Objekts und aller untergeordneten Objekte.

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample79.css)]

### `Recaptcha.GetHtml([, publicKey] [, theme] [, language] [, tabIndex])`

Rendert den reCAPTCHA-Überprüfungs Test.

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample80.css)]

### `ReCaptcha.PublicKey`  
 `ReCaptcha.PrivateKey`

Legt öffentliche und private Schlüssel für den "reCAPTCHA"-Dienst fest. Normalerweise legen Sie diese Eigenschaften auf der *\_AppStart* -Seite fest.

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample81.css)]

### `ReCaptcha.Validate([, privateKey])`

Gibt das Ergebnis des "reCAPTCHA"-Tests zurück.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample82.cs)]

### `ServerInfo.GetHtml()`

Rendert Statusinformationen über ASP.net Web Pages.

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample83.cshtml)]

### `Twitter.Profile(twitterUserName)`

Rendert einen Twitter-Stream für den angegebenen Benutzer.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample84.js)]

### `Twitter.Search(searchQuery)`

Rendert einen Twitter-Stream für den angegebenen Suchtext.

[!code-xml[Main](asp-net-web-pages-api-reference/samples/sample85.xml)]

### `Video.Flash(filename [, width, height])`

Rendert einen Flash-Video Player für die angegebene Datei mit optionaler Breite und Höhe.

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample86.cshtml)]

### `Video.MediaPlayer(filename [, width, height])`

Rendert einen Windows Media Player für die angegebene Datei mit optionaler Breite und Höhe.

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample87.cshtml)]

### `Video.Silverlight(filename, width, height)`

Rendert einen Silverlight-Player für die angegebene *XAP* -Datei mit der erforderlichen Breite und Höhe.

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample88.cshtml)]

### `WebCache.Get(key)`

Gibt das durch den *Schlüssel*angegebene Objekt oder NULL zurück, wenn das Objekt nicht gefunden wurde.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample89.cs)]

### `WebCache.Remove(key)`

Entfernt das durch den *Schlüssel* angegebene Objekt aus dem Cache.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample90.cs)]

### `WebCache.Set(key, value [, minutesToCache] [, slidingExpiration])`

Legt den *Wert* im Cache unter dem durch *Key*angegebenen Namen ab.

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample91.html)]

### `WebGrid(data)`

Erstellt ein neues `WebGrid`-Objekt mithilfe von Daten aus einer Abfrage.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample92.cs)]

### `WebGrid.GetHtml()`

Rendert Markup zum Anzeigen von Daten in einer HTML-Tabelle.

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample93.html)]

### `WebGrid.Pager()`

Rendert einen Pager für das `WebGrid` Objekt.

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample94.html)]

### `WebImage(path)`

Lädt ein Bild aus dem angegebenen Pfad.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample95.cs)]

### `WebImage.AddImagesWatermark(image)`

Fügt das angegebene Bild als Wasserzeichen hinzu.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample96.cs)]

### `WebImage.AddTextWatermark(text)`

Fügt dem Bild den angegebenen Text hinzu.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample97.cs)]

### `WebImage.FlipHorizontal()`  
`WebImage.FlipVertical()`

Flippt das Bild horizontal oder vertikal.

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample98.css)]

### `WebImage.GetImageFromRequest()`

Lädt ein Bild, wenn ein Bild während eines Datei Uploads an eine Seite gesendet wird.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample99.cs)]

### `WebImage.Resize(width,height)`

Ändert die Größe des Bilds.

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample100.css)]

### `WebImage.RotateLeft()`  
`WebImage.RotateRight()`

Dreht das Bild nach links oder nach rechts.

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample101.css)]

### `WebImage.Save(path [, imageFormat])`

Speichert das Bild im angegebenen Pfad.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample102.js)]

### `WebMail.Password`

Legt das Kennwort für den SMTP-Server fest. Normalerweise legen Sie diese Eigenschaft auf der *\_AppStart* -Seite fest.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample103.cs)]

### `WebMail.Send(to, subject, body [, from] [, cc] [, filesToAttach] [, isBodyHtml]`  
 `[, additionalHeaders])`

Sendet eine E-Mail.

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample104.css)]

### `WebMail.SmtpServer`

Legt den Namen des SMTP-Servers fest. Normalerweise legen Sie diese Eigenschaft auf der *\_AppStart* -Seite fest.

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample105.html)]

### `WebMail.UserName`

Legt den Benutzernamen für den SMTP-Server fest. Normalerweise sollten Sie diese Eigenschaft auf der *\_AppStart* -Seite festlegen.

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample106.html)]

<a id="Validation"></a>
## <a name="validation"></a>Überprüfen

### `Html.ValidationMessage(field)`

v2 Rendert eine Validierungs Fehlermeldung für das angegebene Feld.

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample107.cshtml)]

### `Html.ValidationSummary([message])`

v2 Zeigt eine Liste aller Validierungs Fehler an.

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample108.cshtml)]

### `Validation.Add(field, validationType)`

v2 Registriert ein Benutzereingabe Element für den angegebenen Validierungstyp.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample109.js)]

### `Validation.ClassFor(field)`

v2 Rendert dynamisch CSS-Klassenattribute für die Client seitige Validierung, sodass Sie Validierungs Fehlermeldungen formatieren können. (Erfordert, dass Sie auf die entsprechenden Client Skript Bibliotheken verweisen und CSS-Klassen definieren.)

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample110.html)]

### `Validation.For(field)`

v2 Aktiviert die Client seitige Validierung für das Benutzereingabe Feld. (Erfordert, dass Sie auf die entsprechenden Client Skript Bibliotheken verweisen.)

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample111.html)]

### `Validation.IsValid()`

v2 Gibt true zurück, wenn alle Benutzereingabe Elemente, die für die Validierung registriert sind, gültige Werte enthalten.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample112.cs)]

### `Validation.RequireField(field[, errorMessage])`

v2 Gibt an, dass Benutzer einen Wert für das User Input-Element angeben müssen.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample113.cs)]

### `Validation.RequireFields(field1[, field12, field3, ...])`

v2 Gibt an, dass Benutzer Werte für jedes Benutzereingabe Element angeben müssen. Diese Methode ermöglicht keine Angabe einer benutzerdefinierten Fehlermeldung.

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample114.html)]

### `Validator.DateTime ([error message])`  
`Validator.Decimal([error message])`  
`Validator.EqualsTo(otherField,[error message])`  
`Validator.Float([error message])`  
`Validator.Integer([error message])`  
`Validator.Range(min,max [, error message])`  
`Validator.RegEx(pattern[, error message])`  
`Validator.Required([error message])`  
`Validator.StringLength(length)`  
`Validator.Url([error message])`

v2 Gibt einen Validierungstest an, wenn Sie die `Validation.Add`-Methode verwenden.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample115.js)]
