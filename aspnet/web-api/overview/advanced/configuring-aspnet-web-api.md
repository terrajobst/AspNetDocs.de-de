---
uid: web-api/overview/advanced/configuring-aspnet-web-api
title: Konfigurieren von ASP.net-Web-API 2-ASP.NET 4. x
author: MikeWasson
description: 'Konfigurieren Sie ASP.net-Web-API 2 für ASP.NET 4. x: Konfigurieren Sie die Einstellungen, das ASP.NET 4. x-Hosting, das Self-Hosting von owin, globale Dienste und die Konfiguration vor dem Controller.'
ms.author: riande
ms.date: 03/31/2014
ms.custom: seoapril2019
ms.assetid: 9e10a700-8d91-4d2e-a31e-b8b569fe867c
msc.legacyurl: /web-api/overview/advanced/configuring-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 4f76728fa5e4602e35e1b7cb2d41b2245093cad8
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78449343"
---
# <a name="configuring-aspnet-web-api-2"></a>Konfigurieren von ASP.net-Web-API 2

von [Mike Wasson](https://github.com/MikeWasson)

In diesem Thema wird beschrieben, wie Sie ASP.net-Web-API konfigurieren.

- [Konfigurationseinstellungen](#settings)
- [Konfigurieren der Web-API mit ASP.net-Hosting](#webhost)
- [Konfigurieren der Web-API mit selbst Hosting von owin](#selfhost)
- [Globale Web-API-Dienste](#services)
- [Konfiguration pro Controller](#percontrollerconfig)

<a id="settings"></a>
## <a name="configuration-settings"></a>Konfigurationseinstellungen

Die Web-API-Konfigurationseinstellungen werden in der [httpconfiguration](https://msdn.microsoft.com/library/system.web.http.httpconfiguration.aspx) -Klasse definiert.

| Member | Beschreibung |
| --- | --- |
| **DependencyResolver** | Aktiviert die Abhängigkeitsinjektion für Controller. Siehe [Verwenden des Web-API-Abhängigkeits Konflikt Lösers](dependency-injection.md). |
| **Filter** | Aktionsfilter. |
| **Formatierungs Programme** | [Medientyp-Formatierer](../formats-and-model-binding/media-formatters.md). |
| **Includebug-detailpolicy** | Gibt an, ob der Server Fehlerdetails wie Ausnahme Meldungen und Stapel Überwachungen in HTTP-Antwort Nachrichten enthalten soll. Siehe [incluererrordetailpolicy](https://msdn.microsoft.com/library/system.web.http.includeerrordetailpolicy(v=vs.108)). |
| **Initialisierer** | Eine Funktion, die die endgültige Initialisierung der **httpconfiguration**ausführt. |
| **Messagehandlers** | [Http-Nachrichten Handler](http-message-handlers.md). |
| **Parameterbindingrules** | Eine Auflistung von Regeln für das Binden von Parametern für Controller Aktionen. |
| **Eigenschaften** | Ein generischer Eigenschaften Behälter. |
| **Routen** | Die Auflistung von Routen. Siehe [Routing in ASP.net-Web-API](../web-api-routing-and-actions/routing-in-aspnet-web-api.md). |
| **Dienste** | Die Sammlung von Diensten. Siehe [Dienste](#services). |

## <a name="prerequisites"></a>Voraussetzungen

[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017) – Community, Professional oder Enterprise Edition.

<a id="webhost"></a>
## <a name="configuring-web-api-with-aspnet-hosting"></a>Konfigurieren der Web-API mit ASP.net-Hosting

In einer ASP.NET-Anwendung konfigurieren Sie die Web-API, indem Sie [globalconfiguration. configure](https://msdn.microsoft.com/library/system.web.http.globalconfiguration.configure.aspx) in der **Anwendung\_Start** -Methode aufrufen. Die **configure** -Methode nimmt einen Delegaten mit einem einzelnen Parameter vom Typ " **httpconfiguration**" an. Führen Sie die gesamte Konfiguration innerhalb des Delegaten aus.

Im folgenden finden Sie ein Beispiel für die Verwendung eines anonymen Delegaten:

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample1.cs)]

In Visual Studio 2017 richtet die Projektvorlage "ASP.NET Webanwendung" den Konfigurations Code automatisch ein, wenn Sie im Dialogfeld " **Neues ASP.net-Projekt** " die Option "Web-API" auswählen.

[![](configuring-aspnet-web-api/_static/image2.png)](configuring-aspnet-web-api/_static/image1.png)

Die Projektvorlage erstellt eine Datei namens WebApiConfig.cs innerhalb des Ordners App\_Start. Mit dieser Codedatei wird der Delegat definiert, in dem Sie Ihren Web-API-Konfigurations Code platzieren sollten.

![](configuring-aspnet-web-api/_static/image3.png)

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample2.cs?highlight=12)]

Die Projektvorlage fügt auch den Code hinzu, der den Delegaten aus dem **Anwendungs\_Start**aufruft.

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample3.cs?highlight=5)]

<a id="selfhost"></a>
## <a name="configuring-web-api-with-owin-self-hosting"></a>Konfigurieren der Web-API mit selbst Hosting von owin

Wenn Sie selbst Hosting mit owin durchführen, erstellen Sie eine neue **httpconfiguration** -Instanz. Führen Sie eine beliebige Konfiguration für diese Instanz aus, und übergeben Sie die Instanz dann an die **owin. usewebapi** -Erweiterungsmethode.

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample4.cs)]

Im Tutorial [Verwenden von owin für den Self-Host-ASP.net-Web-API 2](../hosting-aspnet-web-api/use-owin-to-self-host-web-api.md) werden die gesamten Schritte angezeigt.

<a id="services"></a>
## <a name="global-web-api-services"></a>Globale Web-API-Dienste

Die **httpconfiguration. Services** -Auflistung enthält eine Reihe globaler Dienste, die die Web-API verwendet, um verschiedene Aufgaben auszuführen, z. b. die Controller Auswahl und die Inhaltsaushandlung.

> [!NOTE]
> Die- **Dienst** Sammlung ist kein allgemeiner Mechanismus für die Dienst Ermittlung oder die Abhängigkeitsinjektion. Es werden nur Dienst Typen gespeichert, die dem Web-API-Framework bekannt sind.

Die **Services** -Sammlung wird mit einem Standardsatz von Diensten initialisiert, und Sie können eigene benutzerdefinierte Implementierungen bereitstellen. Einige Dienste unterstützen mehrere Instanzen, während andere nur eine Instanz haben können. (Sie können jedoch auch Dienste auf Controller Ebene bereitstellen; Weitere Informationen finden Sie unter [Konfiguration pro Controller](#percontrollerconfig).

Einzelinstanzdienste

| Dienst | Beschreibung |
| --- | --- |
| **Iaktionvaluebinder** | Ruft eine Bindung für einen Parameter ab. |
| **Iapiexplorer** | Ruft Beschreibungen der APIs ab, die von der Anwendung verfügbar gemacht werden. Weitere Informationen finden Sie unter [Erstellen einer Hilfeseite für eine Web-API](../getting-started-with-aspnet-web-api/creating-api-help-pages.md). |
| **Iassembliesresolver** | Ruft eine Liste der Assemblys für die Anwendung ab. Weitere Informationen finden Sie unter [Routing und Aktions Auswahl](../web-api-routing-and-actions/routing-and-action-selection.md). |
| **Ibodymodelvalidator** | Überprüft ein Modell, das von einem Medientyp Formatierer aus dem Anforderungs Text gelesen wird. |
| **IContentNegotiator** | Führt Inhaltsaushandlung aus. |
| **Idocumentationprovider** | Stellt Dokumentation für-APIs bereit. Der Standardwert ist **null**. Weitere Informationen finden Sie unter [Erstellen einer Hilfeseite für eine Web-API](../getting-started-with-aspnet-web-api/creating-api-help-pages.md). |
| **Ihostbufferpolicyselector** | Gibt an, ob der Host HTTP-Nachrichten Entitäts Texte puffern soll. |
| **Ihttpactioninvoker** | Ruft eine Controller Aktion auf. Weitere Informationen finden Sie unter [Routing und Aktions Auswahl](../web-api-routing-and-actions/routing-and-action-selection.md). |
| **Ihttpactionselector** | Wählt eine Controller Aktion aus. Weitere Informationen finden Sie unter [Routing und Aktions Auswahl](../web-api-routing-and-actions/routing-and-action-selection.md). |
| **Ihttpcontrolleractivator** | Aktiviert einen Controller. Weitere Informationen finden Sie unter [Routing und Aktions Auswahl](../web-api-routing-and-actions/routing-and-action-selection.md). |
| **Ihttpcontrollerselector** | Wählt einen Controller aus. Weitere Informationen finden Sie unter [Routing und Aktions Auswahl](../web-api-routing-and-actions/routing-and-action-selection.md). |
| **Ihttpcontrollertyperesolver** | Stellt eine Liste der Web-API-Controller Typen in der Anwendung bereit. Weitere Informationen finden Sie unter [Routing und Aktions Auswahl](../web-api-routing-and-actions/routing-and-action-selection.md). |
| **Itracemanager** | Initialisiert das Ablaufverfolgungs-Framework. Siehe Ablauf [Verfolgung in ASP.net-Web-API](../testing-and-debugging/tracing-in-aspnet-web-api.md). |
| **Itracewriter** | Stellt einen Trace Writer bereit. Der Standardwert ist ein "No-op"-Ablaufverfolgungs-Writer. Siehe Ablauf [Verfolgung in ASP.net-Web-API](../testing-and-debugging/tracing-in-aspnet-web-api.md). |
| **Imodelvalidatorcache** | Stellt einen Cache von Modell Validierungs Steuerelementen bereit. |

Dienste mit mehreren Instanzen

|                 Dienst                 |                                                                                                              Beschreibung                                                                                                               |
|-----------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    <strong>Ifilterprovider</strong>     |                                                                                           Gibt eine Liste von Filtern für eine Controller Aktion zurück.                                                                                           |
|  <strong>Modelbinderprovider</strong>   |                                                                                                Gibt einen Modell Binder für einen angegebenen Typ zurück.                                                                                                |
| <strong>ModelMetadataProvider</strong>  |                                                                                                     Stellt Metadaten für ein Modell bereit.                                                                                                     |
| <strong>Modelvalidatorprovider</strong> |                                                                                                   Stellt ein Validierungs Steuerelement für ein Modell bereit.                                                                                                    |
|  <strong>Valueproviderfactory</strong>  | Erstellt einen Wert Anbieter. Weitere Informationen finden Sie im Blogbeitrag " [Erstellen eines benutzerdefinierten Wert Anbieters in WebAPI" im](https://blogs.msdn.com/b/jmstall/archive/2012/04/23/how-to-create-a-custom-value-provider-in-webapi.aspx) Blogbeitrag von Mike Stall. |

Zum Hinzufügen einer benutzerdefinierten-Implementierung zu einem Dienst mit mehreren Instanzen müssen **Sie Add** oder **Insert** für die **Services** -Sammlung abrufen:

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample5.cs)]

Um einen Einzelinstanzdienst durch eine benutzerdefinierte Implementierung zu ersetzen, nennen Sie **Replace** für die **Services** -Sammlung:

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample6.cs)]

<a id="percontrollerconfig"></a>
## <a name="per-controller-configuration"></a>Konfiguration pro Controller

Sie können die folgenden Einstellungen pro Controller überschreiben:

- Medientyp-Formatierer
- Parameter Bindungs Regeln
- Dienste

Definieren Sie zu diesem Zweck ein benutzerdefiniertes Attribut, das die **icontrollerconfiguration** -Schnittstelle implementiert. Wenden Sie dann das-Attribut auf den Controller an.

Im folgenden Beispiel werden die standardmäßigen Medientyp-Formatierer durch ein benutzerdefiniertes Formatierer ersetzt.

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample7.cs)]

Die **icontrollerconfiguration. Initialize** -Methode nimmt zwei Parameter an:

- Ein **httpcontrollersettings** -Objekt
- Ein **httpcontrollerdescriptor** -Objekt

Der **httpcontrollerdescriptor** enthält eine Beschreibung des Controllers, den Sie zu Informationszwecken überprüfen können (z.b., um zwischen zwei Controllern zu unterscheiden).

Verwenden Sie das **httpcontrollersettings** -Objekt, um den Controller zu konfigurieren. Dieses Objekt enthält die Teilmenge der Konfigurationsparameter, die Sie pro Controller überschreiben können. Alle Einstellungen, die Sie nicht ändern, werden standardmäßig in das globale **httpconfiguration** -Objekt geändert.
