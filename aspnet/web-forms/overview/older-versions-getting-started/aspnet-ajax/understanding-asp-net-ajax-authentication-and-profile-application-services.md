---
uid: web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-authentication-and-profile-application-services
title: Grundlegendes zur ASP.NET AJAX-Authentifizierung und-Profil Anwendungsdienste | Microsoft-Dokumentation
author: scottcate
description: Der Authentifizierungsdienst ermöglicht es Benutzern, Anmelde Informationen bereitzustellen, um ein Authentifizierungs Cookie zu erhalten, und ist der Gatewaydienst zum zulassen benutzerdefinierter Benutzer...
ms.author: riande
ms.date: 03/14/2008
ms.assetid: 6ab4efb6-aab6-45ac-ad2c-bdec5848ef9e
msc.legacyurl: /web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-authentication-and-profile-application-services
msc.type: authoredcontent
ms.openlocfilehash: cab9acb1ffd75cca87f6c575a6abdd000235828e
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78520311"
---
# <a name="understanding-aspnet-ajax-authentication-and-profile-application-services"></a>Grundlegendes zu Authentifizierungs- und Profilanwendungsdiensten von ASP.NET AJAX

von [Scott Cate](https://github.com/scottcate)

[PDF herunterladen](https://download.microsoft.com/download/C/1/9/C19A3451-1D14-477C-B703-54EF22E197EE/AJAX_tutorial03_MSAjax_ASP.NET_Services_cs.pdf)

> Der Authentifizierungsdienst ermöglicht es Benutzern, Anmelde Informationen bereitzustellen, um ein Authentifizierungs Cookie zu erhalten, und ist der Gatewaydienst, mit dem benutzerdefinierte Benutzerprofile von ASP.NET bereitgestellt werden können. Die Verwendung des ASP.NET AJAX-Authentifizierungs diensdienstanbieter ist kompatibel mit der standardmäßigen ASP.NET-Formular Authentifizierung, sodass Anwendungen, die derzeit die Formular Authentifizierung verwenden (z. b. mit dem Anmelde Steuerelement), nicht durch ein Upgrade auf den AJAX-Authentifizierungsdienst

## <a name="introduction"></a>Einführung

Als Teil der .NET Framework 3,5 stellt Microsoft ein durch die Umgebung Aktualisier bares Umgebungs Upgrade bereit. Es ist nicht nur eine neue Entwicklungsumgebung verfügbar, sondern die neuen LINQ-Funktionen (Language-Integrated Query) und andere Verbesserungen in der Sprache. Außerdem werden einige vertraute Features anderer Toolsets, insbesondere die ASP.NET AJAX-Erweiterungen, als First-Class-Member der .NET Framework Basisklassen Bibliothek eingeschlossen. Diese Erweiterungen ermöglichen viele neue Rich Client-Features, einschließlich partielles Rendering von Seiten, ohne dass eine vollständige Seiten Aktualisierung erforderlich ist, die Möglichkeit des Zugriffs auf Webdienste über ein Client Skript (einschließlich der ASP.net-Profilerstellungs-API) und eine umfangreiche Client seitige API entworfen, um viele der Steuerelement Schemas zu spiegeln, die im serverseitigen ASP.NET-Steuerelement Satz angezeigt werden.

In diesem Whitepaper wird die Implementierung und Verwendung der ASP.NET-Profil Erstellungs-und Formular Authentifizierungsdienste erläutert, da diese von den Microsoft ASP.NET AJAX-Erweiterungen verfügbar gemacht werden. die AJAX-Erweiterungen machen die Formular Authentifizierung unglaublich einfach zu unterstützen, da Sie (und der Profil Erstellungs Dienst) wird über ein Webdienst-Proxy Skript verfügbar gemacht. Die AJAX-Erweiterungen unterstützen auch die benutzerdefinierte Authentifizierung über die AuthenticationServiceManager-Klasse.

Dieses Whitepaper basiert auf der Beta 2-Version von Visual Studio 2008 und der .NET Framework 3,5. In diesem Whitepaper wird auch davon ausgegangen, dass Sie mit Visual Studio 2008 Beta 2, nicht mit Visual Web Developer Express arbeiten und Exemplarische Vorgehensweisen entsprechend der Benutzeroberfläche von Visual Studio bereitstellen. Einige Codebeispiele verwenden möglicherweise Projektvorlagen, die in Visual Web Developer Express nicht verfügbar sind.

## <a name="profiles-and-authentication"></a>*Profile und Authentifizierung*

Die Microsoft ASP.NET Profile und Authentifizierungsdienste werden vom ASP.net Forms Authentication System bereitgestellt und sind Standardkomponenten von ASP.net. Die ASP.NET-AJAX-Erweiterungen bieten Skript Zugriff auf diese Dienste über Skript Proxys über ein relativ einfaches Modell im sys. Services-Namespace der Client AJAX-Bibliothek.

Der Authentifizierungsdienst ermöglicht es Benutzern, Anmelde Informationen bereitzustellen, um ein Authentifizierungs Cookie zu erhalten, und ist der Gatewaydienst, mit dem benutzerdefinierte Benutzerprofile von ASP.NET bereitgestellt werden können. Die Verwendung des ASP.NET AJAX-Authentifizierungs diensdienstanbieter ist kompatibel mit der standardmäßigen ASP.NET-Formular Authentifizierung, sodass Anwendungen, die derzeit die Formular Authentifizierung verwenden (z. b. mit dem Anmelde Steuerelement), nicht durch ein Upgrade auf den AJAX-Authentifizierungsdienst

Der Profil Dienst ermöglicht die automatische Integration und Speicherung von Benutzerdaten basierend auf der Mitgliedschaft, die vom Authentifizierungsdienst bereitgestellt wird. Die gespeicherten Daten werden durch die Datei "Web. config" angegeben, und die verschiedenen Profilerstellungs-Dienstanbieter verarbeiten die Datenverwaltung. Wie beim Authentifizierungsdienst ist der AJAX-Profil Dienst mit dem standardmäßigen ASP.NET-Profil Dienst kompatibel, sodass Seiten, die Funktionen des ASP.NET-Profil Dienstanbieter enthalten, nicht durch Einschließen von AJAX-Unterstützung beschädigt werden sollten.

Das Einbinden der ASP.net-Authentifizierungs-und Profil Erstellungs Dienste in eine Anwendung ist nicht in diesem Whitepaper enthalten. Weitere Informationen zu diesem Thema finden Sie im MSDN Library-Referenz Artikel Verwalten von Benutzern mithilfe der Mitgliedschaft in [https://msdn.microsoft.com/library/tw292whz.aspx](https://msdn.microsoft.com/library/tw292whz.aspx). ASP.NET umfasst auch ein Dienstprogramm zum automatischen Einrichten der Mitgliedschaft mit einem SQL Server, bei dem es sich um den Standard Authentifizierungs Dienstanbieter für die ASP.NET-Mitgliedschaft handelt. Weitere Informationen finden Sie im Artikel ASP.NET SQL Server Registration Tool (ASPNET\_RegSql. exe) unter [https://msdn.microsoft.com/library/ms229862(vs.80).aspx](https://msdn.microsoft.com/library/ms229862(vs.80).aspx).

## <a name="using-the-aspnet-ajax-authentication-service"></a>*Verwenden des ASP.NET AJAX-Authentifizierungs Dienstanbieter*

Der ASP.NET AJAX-Authentifizierungsdienst muss in der Datei "Web. config" aktiviert sein:

[!code-xml[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample1.xml)]

Der Authentifizierungsdienst erfordert, dass die ASP.NET-Formular Authentifizierung aktiviert ist, und erfordert, dass Cookies auf dem Client Browser aktiviert werden (ein Skript kann keine cookielose Sitzung aktivieren, da cookielose Sitzungen URL-Parameter erfordern).

Nachdem der AJAX-Authentifizierungsdienst aktiviert und konfiguriert wurde, kann das Client Skript das sys. Services. AuthenticationService-Objekt sofort nutzen. Vor allem möchte das Client Skript die `login`-Methode und `isLoggedIn`-Eigenschaft nutzen. Es gibt mehrere Eigenschaften, um Standardwerte für die Anmelde Methode bereitzustellen, die eine große Anzahl von Parametern annehmen kann.

*Sys. Services. AuthenticationService-Member*

*Anmelde Methode:*

Mit der Login ()-Methode wird eine Anforderung zum Authentifizieren der Anmelde Informationen des Benutzers gestartet. Diese Methode ist asynchron und blockiert nicht die Ausführung.

*Parameter:*

| **Parametername** | **Bedeutung** |
| --- | --- |
| userName | Erforderlich. Der zu authentifizier Ende Benutzername. |
| password | Optional (standardmäßig NULL). Das Kennwort des Benutzers. |
| isPersistent | Optional (Standardwert: false). Gibt an, ob das Authentifizierungs Cookie des Benutzers Sitzungs übergreifend beibehalten werden soll. False gibt an, dass der Benutzer sich abmeldet, wenn der Browser geschlossen wird oder die Sitzung abläuft. |
| redirectUrl | Optional (standardmäßig NULL). Die URL, an die der Browser nach erfolgreicher Authentifizierung umgeleitet werden soll. Wenn dieser Parameter NULL oder eine leere Zeichenfolge ist, erfolgt keine Umleitung. |
| customInfo | Optional (standardmäßig NULL). Dieser Parameter wird derzeit nicht verwendet und ist für die zukünftige Verwendung reserviert. |
| loginCompletedCallback | Optional (standardmäßig NULL). Die Funktion, die aufgerufen werden soll, wenn die Anmeldung erfolgreich abgeschlossen wurde. Wenn angegeben, überschreibt dieser Parameter die defaultloginabgeschlossene-Eigenschaft. |
| failedCallback | Optional (standardmäßig NULL). Die Funktion, die aufgerufen werden soll, wenn die Anmeldung fehlgeschlagen ist. Wenn angegeben, überschreibt dieser Parameter die defaultFailedCallback-Eigenschaft. |
| userContext | Optional (standardmäßig NULL). Benutzerdefinierte Benutzer Kontext Daten, die an die Rückruf Funktionen übermittelt werden sollen. |

*Return Value* (Rückgabewert):

Diese Funktion enthält keinen Rückgabewert. Beim Abschluss eines Aufrufes der Funktion werden jedoch einige Verhaltensweisen berücksichtigt:

- Die aktuelle Seite wird entweder aktualisiert oder geändert, wenn der `redirectUrl`-Parameter weder NULL noch eine leere Zeichenfolge ist.
- Wenn der-Parameter jedoch NULL oder eine leere Zeichenfolge ist, wird der `loginCompletedCallback`-Parameter oder `defaultLoginCompletedCallback`-Eigenschaft aufgerufen.
- Wenn der Aufruf des Webdiensts fehlschlägt, wird der `failedCallback`-Parameter der `defaultFailedCallback`-Eigenschaft aufgerufen.

*Abmelde-Methode:*

Die Logout ()-Methode entfernt das Cookie für Anmelde Informationen und meldet den aktuellen Benutzer aus der Webanwendung ab.

*Parameter:*

| **Parametername** | **Bedeutung** |
| --- | --- |
| redirectUrl | Optional (standardmäßig NULL). Die URL, an die der Browser nach erfolgreicher Authentifizierung umgeleitet werden soll. Wenn dieser Parameter NULL oder eine leere Zeichenfolge ist, erfolgt keine Umleitung. |
| logoutCompletedCallback | Optional (standardmäßig NULL). Die Funktion, die aufgerufen werden soll, wenn die Abmeldung erfolgreich abgeschlossen wurde. Wenn angegeben, überschreibt dieser Parameter die defaultlogoutabgeschlossene-Eigenschaft. |
| failedCallback | Optional (standardmäßig NULL). Die Funktion, die aufgerufen werden soll, wenn die Anmeldung fehlgeschlagen ist. Wenn angegeben, überschreibt dieser Parameter die defaultFailedCallback-Eigenschaft. |
| userContext | Optional (standardmäßig NULL). Benutzerdefinierte Benutzer Kontext Daten, die an die Rückruf Funktionen übermittelt werden sollen. |

*Return Value* (Rückgabewert):

Diese Funktion enthält keinen Rückgabewert. Beim Abschluss eines Aufrufes der Funktion werden jedoch einige Verhaltensweisen berücksichtigt:

- Die aktuelle Seite wird entweder aktualisiert oder geändert, wenn der `redirectUrl`-Parameter weder NULL noch eine leere Zeichenfolge ist.
- Wenn der-Parameter jedoch NULL oder eine leere Zeichenfolge ist, wird der `logoutCompletedCallback`-Parameter oder `defaultLogoutCompletedCallback`-Eigenschaft aufgerufen.
- Wenn der Aufruf des Webdiensts fehlschlägt, wird der `failedCallback`-Parameter der `defaultFailedCallback`-Eigenschaft aufgerufen.

*defaultFailedCallback-Eigenschaft (Get, Set):*

Diese Eigenschaft gibt eine Funktion an, die aufgerufen werden soll, wenn ein Fehler bei der Kommunikation mit dem Webdienst auftritt. Es sollte einen Delegaten (oder Funktions Verweis) erhalten.

Die von dieser Eigenschaft angegebene Funktionsreferenz sollte die folgende Signatur aufweisen:

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample2.js)]

*Parameter:*

| **Parametername** | **Bedeutung** |
| --- | --- |
| error | Gibt die Fehlerinformationen an. |
| userContext | Gibt die Benutzer Kontextinformationen an, die beim Aufrufen der Login-oder Abmelde-Funktion bereitgestellt wurden. |
| methodName | Der Name der aufrufenden Methode. |

*defaultLoginCompletedCallback-Eigenschaft (Get, Set):*

Diese Eigenschaft gibt eine Funktion an, die aufgerufen werden soll, wenn der Anmeldungs-Webdienst Aufruf abgeschlossen wurde. Es sollte einen Delegaten (oder Funktions Verweis) erhalten.

Die von dieser Eigenschaft angegebene Funktionsreferenz sollte die folgende Signatur aufweisen:

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample3.js)]

*Parameter:*

| **Parametername** | **Bedeutung** |
| --- | --- |
| validCredentials | Gibt an, ob der Benutzer gültige Anmelde Informationen angegeben hat. `true`, wenn sich der Benutzer erfolgreich angemeldet hat. Andernfalls `false`. |
| userContext | Gibt die Benutzer Kontextinformationen an, die beim Aufrufen der Login-Funktion bereitgestellt wurden. |
| methodName | Der Name der aufrufenden Methode. |

*defaultLogoutCompletedCallback-Eigenschaft (Get, Set):*

Diese Eigenschaft gibt eine Funktion an, die aufgerufen werden soll, wenn der Abmelde-Webdienst Aufruf abgeschlossen wurde. Es sollte einen Delegaten (oder Funktions Verweis) erhalten.

Die von dieser Eigenschaft angegebene Funktionsreferenz sollte die folgende Signatur aufweisen:

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample4.js)]

*Parameter:*

| **Parametername** | **Bedeutung** |
| --- | --- |
| result | Dieser Parameter wird immer `null`. Es ist für die zukünftige Verwendung reserviert. |
| userContext | Gibt die Benutzer Kontextinformationen an, die beim Aufrufen der Login-Funktion bereitgestellt wurden. |
| methodName | Der Name der aufrufenden Methode. |

*isLoggedIn-Eigenschaft (Get):*

Diese Eigenschaft ruft den aktuellen Authentifizierungs Zustand des Benutzers ab. Sie wird während einer Seiten Anforderung vom ScriptManager-Objekt festgelegt.

Diese Eigenschaft gibt `true` zurück, wenn der Benutzer zurzeit angemeldet ist. Andernfalls wird `false`zurückgegeben.

*Path-Eigenschaft (Get, Set):*

Diese Eigenschaft bestimmt den Speicherort des authentifizierungsweb Diensts Programm gesteuert. Sie kann verwendet werden, um den Standard Authentifizierungs Anbieter und einen Satz deklarativ in der Path-Eigenschaft des untergeordneten AuthenticationService-Knotens des ScriptManager-Steuer Elements zu überschreiben. (Weitere Informationen finden Sie unter Verwenden eines benutzerdefinierten Authentifizierungs Dienstanbieters. weiter unten).

Beachten Sie, dass sich der Speicherort des Standard Authentifizierungs Dienes nicht ändert. Allerdings können Sie mit ASP.NET AJAX den Speicherort eines Webdiensts angeben, der dieselbe Klassen Schnittstelle wie der ASP.NET AJAX-Authentifizierungsdienst Proxy bereitstellt.

Beachten Sie auch, dass diese Eigenschaft nicht auf einen Wert festgelegt werden sollte, der die Skript Anforderung von der aktuellen Website ableitet. Da die aktuelle Anwendung nicht die Authentifizierungs Anmelde Informationen erhält, wäre sie nutzlos. Außerdem sollte die Technologie, die dem AJAX zugrunde liegt, keine standortübergreifenden Anforderungen bereitstellen und kann im Client Browser eine Sicherheits Ausnahme generieren.

Diese Eigenschaft ist ein `String` Objekt, das den Pfad zum authentifizierungsweb Dienst darstellt.

*Timeout-Eigenschaft (Get, Set):*

Diese Eigenschaft bestimmt die Zeitspanne, die auf den Authentifizierungsdienst gewartet werden soll, bevor angenommen wird, dass die Anmelde Anforderung fehlgeschlagen ist. Wenn das Timeout abläuft, während auf den Abschluss eines Aufrufs gewartet wird, wird der Rückruf für Anforderungs Fehler aufgerufen, und der Aufruf wird nicht beendet.

Diese Eigenschaft ist ein `Number` Objekt, das die Anzahl der Millisekunden darstellt, die auf Ergebnisse vom Authentifizierungsdienst gewartet werden soll.

*Code Beispiel: Anmelden beim Authentifizierungsdienst*

Das folgende Markup ist eine Beispiel-ASP.NET-Seite mit einem einfachen Skript Aufruf an die Anmelde-und Abmelde Methoden der AuthenticationService-Klasse.

[!code-aspx[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample5.aspx)]

## <a name="accessing-aspnet-profiling-data-via-ajax"></a>Zugreifen auf ASP.NET-Profil Erstellungs Daten über AJAX

Der ASP.NET-Profil Erstellungs Dienst wird auch über die ASP.NET AJAX-Erweiterungen verfügbar gemacht. Da der ASP.NET-Profil Erstellungs Dienst eine umfangreiche, differenzierte API bereitstellt, mit der Benutzerdaten gespeichert und abgerufen werden können, kann dies ein ausgezeichnetes Produktivitäts Tool sein.

Der Profil Dienst muss in der Datei "Web. config" aktiviert sein. Dies ist nicht standardmäßig. Stellen Sie zu diesem Zweck sicher, dass das untergeordnete `profileService`-Element in der Datei "Web. config" aktiviert = true ist, und dass Sie festgelegt haben, welche Eigenschaften wie folgt gelesen oder geschrieben werden können:

[!code-xml[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample6.xml)]

Der Profil Dienst muss ebenfalls konfiguriert werden. Obwohl die Konfiguration des Profil Erstellungs Dienstanbieter außerhalb des Umfangs dieses Whitepaper liegt, ist es lohnenswert zu beachten, dass die in den Profil Konfigurationseinstellungen definierten Gruppen als untergeordnete Eigenschaften des Gruppennamens zugänglich sind. Beispielsweise mit folgendem Profilabschnitt angegeben:

[!code-xml[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample7.xml)]

Das Client Skript kann auf "Name", "Address. Zeile 1", "Address. Zeile 2", "Address. City", "Address. State", "Address. zip" und "BackgroundColor" als Eigenschaften des Felds "Properties" der ProfileService-Klasse zugreifen.

Nachdem der AJAX-Profil Erstellungs Dienst konfiguriert wurde, ist er sofort auf den Seiten verfügbar. Er muss jedoch einmal vor der Verwendung geladen werden.

*Sys. Services. ProfileService-Member*

*Eigenschaften Feld:*

Das Feldeigenschaften macht alle konfigurierten Profildaten als untergeordnete Eigenschaften verfügbar, auf die in der "dot-Operator-Name"-Konvention verwiesen werden kann. Eigenschaften, die untergeordnete Elemente von Eigenschaften Gruppen sind, werden als GroupName. PropertyName bezeichnet. In der oben dargestellten Beispiel Profil Konfiguration können Sie den folgenden Bezeichner verwenden, um den Status des Benutzers zu erhalten:

[!code-csharp[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample8.cs)]

*Load-Methode:*

Lädt eine ausgewählte Liste oder alle Eigenschaften vom Server.

*Parameter:*

| **Parametername** | **Bedeutung** |
| --- | --- |
| propertyNames | Optional (standardmäßig NULL). Die Eigenschaften, die vom Server geladen werden sollen. |
| loadCompletedCallback | Optional (standardmäßig NULL). Die Funktion, die aufgerufen wird, wenn der Ladevorgang abgeschlossen ist. |
| failedCallback | Optional (standardmäßig NULL). Die Funktion, die aufgerufen werden soll, wenn ein Fehler auftritt. |
| userContext | Optional (standardmäßig NULL). Kontextinformationen, die an die Rückruffunktion übermittelt werden sollen. |

Die Load-Funktion weist keinen Rückgabewert auf. Wenn der-Befehl erfolgreich abgeschlossen wurde, wird entweder der `loadCompletedCallback`-Parameter oder `defaultLoadCompletedCallback`-Eigenschaft aufgerufen. Wenn der Aufruf fehlgeschlagen ist oder das Timeout abgelaufen ist, wird entweder der `failedCallback` Parameter oder `defaultFailedCallback` Eigenschaft aufgerufen.

Wenn der `propertyNames`-Parameter nicht angegeben wird, werden alle Lese konfigurierten Eigenschaften vom Server abgerufen.

*Save-Methode:*

Die Save ()-Methode speichert die angegebene Eigenschaften Liste (oder alle Eigenschaften) im ASP.NET Profil des Benutzers.

*Parameter:*

| **Parametername** | **Bedeutung** |
| --- | --- |
| propertyNames | Optional (standardmäßig NULL). Die Eigenschaften, die auf dem Server gespeichert werden sollen. |
| saveCompletedCallback | Optional (standardmäßig NULL). Die Funktion, die aufgerufen wird, wenn der Speichervorgang abgeschlossen ist. |
| failedCallback | Optional (standardmäßig NULL). Die Funktion, die aufgerufen werden soll, wenn ein Fehler auftritt. |
| userContext | Optional (standardmäßig NULL). Kontextinformationen, die an die Rückruffunktion übermittelt werden sollen. |

Die Funktion "Save" weist keinen Rückgabewert auf. Wenn der-Vorgang erfolgreich abgeschlossen wird, wird entweder der `saveCompletedCallback`-Parameter oder `defaultSaveCompletedCallback`-Eigenschaft aufgerufen. Wenn der Aufruf fehlgeschlagen ist oder das Timeout abgelaufen ist, wird entweder die Eigenschaft `failedCallback` oder `defaultFailedCallback` aufgerufen.

Wenn der `propertyNames`-Parameter NULL ist, werden alle Profil Eigenschaften an den Server gesendet, und der Server entscheidet, welche Eigenschaften gespeichert werden können und welche nicht.

*defaultFailedCallback-Eigenschaft (Get, Set):*

Diese Eigenschaft gibt eine Funktion an, die aufgerufen werden soll, wenn ein Fehler bei der Kommunikation mit dem Webdienst auftritt. Es sollte einen Delegaten (oder Funktions Verweis) erhalten.

Die von dieser Eigenschaft angegebene Funktionsreferenz sollte die folgende Signatur aufweisen:

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample9.js)]

*Parameter:*

| **Parametername** | **Bedeutung** |
| --- | --- |
| Fehler | Gibt die Fehlerinformationen an. |
| userContext | Gibt die Benutzer Kontextinformationen an, die beim Aufrufen der Load-oder Save-Funktion bereitgestellt wurden. |
| methodName | Der Name der aufrufenden Methode. |

*defaultsaveabgeschlossene-Eigenschaft (Get, Set):*

Diese Eigenschaft gibt eine Funktion an, die nach dem Abschluss der Speicherung der Profildaten des Benutzers aufgerufen werden soll. Es sollte einen Delegaten (oder Funktions Verweis) erhalten.

Die von dieser Eigenschaft angegebene Funktionsreferenz sollte die folgende Signatur aufweisen:

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample10.js)]

*Parameter:*

| **Parametername** | **Bedeutung** |
| --- | --- |
| numPropsSaved | Gibt die Anzahl der gespeicherten Eigenschaften an. |
| userContext | Gibt die Benutzer Kontextinformationen an, die beim Aufrufen der Load-oder Save-Funktion bereitgestellt wurden. |
| methodName | Der Name der aufrufenden Methode. |

*defaultloadabgeschlossene-Eigenschaft (Get, Set):*

Diese Eigenschaft gibt eine Funktion an, die nach dem Abschluss des Ladens der Profildaten des Benutzers aufgerufen werden soll. Es sollte einen Delegaten (oder Funktions Verweis) erhalten.

Die von dieser Eigenschaft angegebene Funktionsreferenz sollte die folgende Signatur aufweisen:

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample11.js)]

*Parameter:*

| **Parametername** | **Bedeutung** |
| --- | --- |
| numPropsLoaded | Gibt die Anzahl der geladenen Eigenschaften an. |
| userContext | Gibt die Benutzer Kontextinformationen an, die beim Aufrufen der Load-oder Save-Funktion bereitgestellt wurden. |
| methodName | Der Name der aufrufenden Methode. |

*Path-Eigenschaft (Get, Set):*

Diese Eigenschaft bestimmt Programm gesteuert den Speicherort des profilweb Diensts. Sie kann verwendet werden, um den Standard-Profil Dienstanbieter und einen Satz deklarativ in der Path-Eigenschaft des untergeordneten profilleservice-Steuer Elements des ScriptManager-Steuer Elements zu überschreiben.

Beachten Sie, dass sich der Speicherort des Standardprofil Dienes nicht ändert. Allerdings können Sie mit ASP.NET AJAX den Speicherort eines Webdiensts angeben, der dieselbe Klassen Schnittstelle wie der ASP.NET AJAX-Authentifizierungsdienst Proxy bereitstellt.

Beachten Sie auch, dass diese Eigenschaft nicht auf einen Wert festgelegt werden sollte, der die Skript Anforderung von der aktuellen Website ableitet. Die Technologie, die dem AJAX zugrunde liegt, sollte keine standortübergreifenden Anforderungen bereitstellen und im Client Browser eine Sicherheits Ausnahme generieren.

Diese Eigenschaft ist ein `String` Objekt, das den Pfad zum profilweb Dienst darstellt.

*Timeout-Eigenschaft (Get, Set):*

Diese Eigenschaft bestimmt die Zeitspanne, die auf den Profil Dienst gewartet werden soll, bevor davon ausgegangen wird, dass die Lade-oder Speicher Anforderung fehlgeschlagen ist. Wenn das Timeout abläuft, während auf den Abschluss eines Aufrufs gewartet wird, wird der Rückruf für Anforderungs Fehler aufgerufen, und der Aufruf wird nicht beendet.

Diese Eigenschaft ist ein `Number` Objekt, das die Anzahl der Millisekunden darstellt, die auf Ergebnisse vom Profil Dienst gewartet werden soll.

*Code Beispiel: Laden von Profildaten beim Laden der Seite*

Im folgenden Code wird überprüft, ob ein Benutzer authentifiziert ist. wenn dies der Fall ist, wird die bevorzugte Hintergrundfarbe des Benutzers als der der Seite geladen.

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample12.js)]

## <a name="using-a-custom-authentication-service-provider"></a>*Verwenden eines benutzerdefinierten Authentifizierungs Dienstanbieters*

Die ASP.NET AJAX-Erweiterungen ermöglichen es Ihnen, einen benutzerdefinierten Skript Authentifizierungsdienst-Anbieter zu erstellen, indem Sie Ihre Funktionalität über einen benutzerdefinierten Webdienst verfügbar machen. Um verwendet werden zu können, muss der Webdienst zwei Methoden, `Login` und `Logout`verfügbar machen. Diese Methoden müssen mit denselben Methoden Signaturen wie der Standardweb Dienst für die ASP.NET-AJAX-Authentifizierung angegeben werden.

Nachdem Sie den benutzerdefinierten Webdienst erstellt haben, müssen Sie den Pfad zu ihm entweder deklarativ auf der Seite, Programm gesteuert im Code oder über das Client Skript angeben.

*So legen Sie den Pfad deklarativ fest:*

Um den Pfad deklarativ festzulegen, schließen Sie das untergeordnete AuthenticationService-Objekt des ScriptManager-Objekts auf der ASP.NET-Seite ein:

[!code-aspx[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample13.aspx)]

*So legen Sie den Pfad im Code fest:*

Um den Pfad Programm gesteuert festzulegen, geben Sie den Pfad über die Instanz Ihres Skript-Managers an:

[!code-csharp[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample14.cs)]

*So legen Sie den Pfad im Skript fest:*

Um den Pfad Programm gesteuert im Skript festzulegen, verwenden Sie die `path`-Eigenschaft der AuthenticationService-Klasse:

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample15.js)]

*Beispielweb Dienst für benutzerdefinierte Authentifizierung*

[!code-aspx[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample16.aspx)]

## <a name="summary"></a>Zusammenfassung

ASP.NET Services (insbesondere die Profilerstellung, die Mitgliedschaft und die Authentifizierungsdienste) lassen sich problemlos im Client Browser für JavaScript verfügbar machen. Dies ermöglicht es Entwicklern, Ihren Client seitigen Code nahtlos in den Authentifizierungsmechanismus zu integrieren, ohne abhängig von Steuerelementen wie z. b. Update Panel, um die Arbeit zu erledigen. Profildaten können auch über den Client geschützt werden, indem Webkonfigurations Einstellungen genutzt werden. Standardmäßig sind keine Daten verfügbar, und Entwickler müssen sich für Profil Eigenschaften entscheiden.

Außerdem können Entwickler benutzerdefinierte Skript Anbieter für diese intrinsischen ASP.NET-Dienste erstellen, indem Sie vereinfachte Webdienst Implementierungen mit äquivalenten Methoden Signaturen erstellen. Die Unterstützung dieser Techniken vereinfacht die Entwicklung von Rich Client-Anwendungen und bietet Entwicklern gleichzeitig eine große Bandbreite an Flexibilität, um bestimmte Anforderungen zu erfüllen.

## <a name="bio"></a>*Basierten*

Scott Cate arbeitet seit 1997 mit Microsoft-Webtechnologien und ist der Präsident von myKB.com ([www.myKB.com](http://www.myKB.com)), wo er sich darauf spezialisiert hat, ASP.NET basierte Anwendungen zu schreiben, die sich auf die Software Lösungen der Wissensdatenbank konzentrieren. Scott kann über [scott.cate@myKB.com](mailto:scott.cate@myKB.com) oder seinen Blog unter [ScottCate.com](http://ScottCate.com) per e-Mail kontaktiert werden.

> [!div class="step-by-step"]
> [Zurück](understanding-asp-net-ajax-updatepanel-triggers.md)
> [Weiter](understanding-asp-net-ajax-localization.md)
