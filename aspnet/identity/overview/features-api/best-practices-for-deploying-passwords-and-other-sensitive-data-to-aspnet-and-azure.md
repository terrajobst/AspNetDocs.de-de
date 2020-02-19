---
uid: identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure
title: Bereitstellen von Kenn Wörtern und anderen sensiblen Daten für ASP.net und Azure App Service-ASP.NET 4. x
author: Rick-Anderson
description: In diesem Tutorial wird gezeigt, wie Ihr Code sichere Informationen sicher speichern und darauf zugreifen kann. Der wichtigste Punkt ist, dass Sie niemals Kenn Wörter oder andere...
ms.author: riande
ms.date: 05/21/2015
ms.assetid: 97902c66-cb61-4d11-be52-73f962f2db0a
ms.custom: seoapril2019
msc.legacyurl: /identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure
msc.type: authoredcontent
ms.openlocfilehash: 8356a90611f791779cc4ff4730038d82cd76242f
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/19/2020
ms.locfileid: "77457049"
---
# <a name="best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure-app-service"></a>Bewährte Methoden für die Bereitstellung von Kennwörtern und anderen sensiblen Daten für ASP.NET und Azure App Service

von [Rick Anderson](https://twitter.com/RickAndMSFT)

> In diesem Tutorial wird gezeigt, wie Ihr Code sichere Informationen sicher speichern und darauf zugreifen kann. Der wichtigste Punkt ist, dass Sie niemals Kenn Wörter oder andere sensible Daten im Quellcode speichern sollten, und Sie sollten keine Produktionsgeheimnisse im Entwicklungs-und Testmodus verwenden.
> 
> Der Beispielcode ist eine einfache webjob-Konsolen-APP und eine ASP.NET MVC-APP, die Zugriff auf eine Datenbank-Verbindungs Zeichenfolge Password, twilio, Google und sendgrid Secure Keys benötigt.
> 
> Lokale Einstellungen und PHP werden ebenfalls erwähnt.

- [Arbeiten mit Kenn Wörtern in der Entwicklungsumgebung](#pwd)
- [Arbeiten mit Verbindungs Zeichenfolgen in der Entwicklungsumgebung](#con)
- [Webjobs-Konsolen-apps](#wj)
- [Bereitstellen von Geheimnissen in Azure](#da)
- [Hinweise für lokales und PHP](#not)
- [Weitere Ressourcen](#addRes)

<a id="pwd"></a>
## <a name="working-with-passwords-in-the-development-environment"></a>Arbeiten mit Kenn Wörtern in der Entwicklungsumgebung

In den Tutorials werden häufig sensible Daten im Quellcode angezeigt, und es wird empfohlen, dass Sie keine sensiblen Daten im Quellcode speichern. Im Tutorial "meine [ASP.NET MVC 5-App mit SMS und e-Mail 2FA](../../../mvc/overview/security/aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication.md) " wird beispielsweise Folgendes in der Datei " *Web. config* " angezeigt:

[!code-xml[Main](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure/samples/sample1.xml)]

Die Datei " *Web. config* " ist Quellcode, daher sollten diese geheimen Schlüssel niemals in dieser Datei gespeichert werden. Glücklicherweise verfügt das `<appSettings>`-Element über ein `file` Attribut, das es Ihnen ermöglicht, eine externe Datei anzugeben, die sensible App-Konfigurationseinstellungen enthält. Sie können alle geheimen Schlüssel in eine externe Datei verschieben, solange die externe Datei nicht in der Quell Struktur eingecheckt wird. Beispielsweise enthält die Datei " *appsettingssecrets. config* " im folgenden Markup alle App-Geheimnisse:

[!code-xml[Main](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure/samples/sample2.xml)]

Das Markup in der externen Datei ("*appsettingssecrets. config* " in diesem Beispiel) ist das gleiche Markup, das in der Datei " *Web. config* " gefunden wurde:

[!code-xml[Main](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure/samples/sample3.xml)]

Die ASP.NET-Laufzeit führt den Inhalt der externen Datei mit dem Markup in &lt;appSettings&gt;-Element zusammen. Falls die angegebene Datei nicht gefunden wird, wird das Dateiattribut ignoriert.

> [!WARNING]
> Sicherheit: Fügen Sie die Datei " *Secrets. config* " nicht zum Projekt hinzu, oder checken Sie Sie in die Quell Code Verwaltung ein. Standardmäßig legt Visual Studio die `Build Action` auf `Content`fest. Dies bedeutet, dass die Datei bereitgestellt wird. Weitere Informationen finden [Sie unter Warum werden nicht alle Dateien im Projektordner](https://msdn.microsoft.com/library/ee942158(v=vs.110).aspx#can_i_exclude_specific_files_or_folders_from_deployment) bereitgestellt? Obwohl Sie eine beliebige Erweiterung für die Datei " *Secrets. config* " verwenden können, ist es am besten, die Datei " *. config*" beizubehalten, da Konfigurationsdateien nicht von IIS bedient werden. Beachten Sie auch, dass die Datei " *appsettingssecrets. config* " aus der Datei " *Web. config* " aus zwei Verzeichnis Ebenen besteht, sodass Sie vollständig aus dem Projektmappenverzeichnis entfernt wird. Wenn Sie die Datei aus dem Projektmappenverzeichnis verschieben, &quot;git Add \*&quot; Sie nicht zu Ihrem Repository hinzufügen.

<a id="con"></a>
## <a name="working-with-connection-strings-in-the-development-environment"></a>Arbeiten mit Verbindungs Zeichenfolgen in der Entwicklungsumgebung

Visual Studio erstellt neue ASP.net-Projekte, die [localdb](https://blogs.msdn.com/b/sqlexpress/archive/2011/07/12/introducing-localdb-a-better-sql-express.aspx)verwenden. Localdb wurde speziell für die Entwicklungsumgebung erstellt. Es ist kein Kennwort erforderlich. Daher müssen Sie keine weiteren Schritte durchführen, um zu verhindern, dass Geheimnisse in Ihren Quellcode eingecheckt werden. Einige Entwicklungsteams verwenden die Vollversionen von SQL Server (oder einem anderen DBMS), die ein Kennwort erfordern.

Sie können das `configSource`-Attribut verwenden, um das gesamte `<connectionStrings>` Markup zu ersetzen. Im Gegensatz zum `<appSettings>` `file` Attributs, das das Markup zusammenfasst, ersetzt das Attribut `configSource` das Markup. Das folgende Markup zeigt das `configSource`-Attribut in der Datei " *Web. config* ":

[!code-xml[Main](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure/samples/sample4.xml?highlight=1)]

> [!NOTE]
> Wenn Sie das `configSource`-Attribut verwenden, wie oben gezeigt, um die Verbindungs Zeichenfolgen in eine externe Datei zu verschieben und Visual Studio eine neue Website zu erstellen, kann es nicht erkennen, dass Sie eine Datenbank verwenden, und Sie erhalten keine Möglichkeit, die Datenbank zu konfigurieren, wenn Sie von Visual Studio aus in Azure veröffentlichen. Wenn Sie das `configSource`-Attribut verwenden, können Sie PowerShell verwenden, um die Website und die Datenbank zu erstellen und bereitzustellen, oder Sie können die Website und die Datenbank im Portal erstellen, bevor Sie veröffentlichen. Mit dem Skript [New-AzureWebsitewithDB. ps1](https://gallery.technet.microsoft.com/scriptcenter/Ultimate-Create-Web-SQL-DB-9e0fdfd3) wird eine neue Website und eine neue Datenbank erstellt.

> [!WARNING]
> Sicherheit: im Gegensatz zur Datei " *appsettingssecrets. config* " muss sich die externe Verbindungs Zeichenfolgen-Datei im gleichen Verzeichnis befinden wie die *Web. config* -Stammdatei, sodass Sie Vorsichtsmaßnahmen treffen müssen, um sicherzustellen, dass Sie Sie nicht in Ihrem Quellrepository einchecken.

> [!NOTE]
> **Sicherheitswarnung bei Geheimnissen Datei:** Eine bewährte Vorgehensweise besteht darin, keine Produktionsgeheimnisse in Test-und Entwicklungsumgebungen zu verwenden. Diese Geheimnisse werden bei der Verwendung von Produktions Kennwörtern in Test-oder Entwicklungs

<a id="wj"></a>
## <a name="webjobs-console-apps"></a>Webjobs-Konsolen-apps

Die *app. config* -Datei, die von einer Konsolen-App verwendet wird, unterstützt keine relativen Pfade, Sie unterstützt jedoch absolute Pfade. Sie können einen absoluten Pfad verwenden, um Ihre geheimen Schlüssel aus dem Projektverzeichnis zu verschieben. Das folgende Markup zeigt die geheimen Schlüssel in der Datei *c:\secrez\appsettingssecret.config* und nicht vertrauliche Daten in der Datei *app. config* an.

[!code-xml[Main](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure/samples/sample5.xml?highlight=2)]

<a id="da"></a>
## <a name="deploying-secrets-to-azure"></a>Bereitstellen von Geheimnissen in Azure

Wenn Sie Ihre Web-App in Azure bereitstellen, wird die Datei " *appsettingssecrets. config* " nicht bereitgestellt (was Sie möchten). Sie können zum Azure- [Verwaltungsportal](https://azure.microsoft.com/services/management-portal/) wechseln und diese manuell festlegen:

1. Wechseln Sie zu [https://portal.azure.com](https://portal.azure.com), und melden Sie sich mit ihren Azure-Anmelde Informationen an.
2. Klicken Sie auf **Durchsuchen &gt; Web-Apps**, und klicken Sie dann auf den Namen Ihrer Web-App.
3. Klicken Sie auf **alle Einstellungen &gt; Anwendungseinstellungen**.

Die **App-Einstellungen** und **Verbindungs Zeichenfolgen** -Werte überschreiben die gleichen Einstellungen in der Datei " *Web. config* ". In unserem Beispiel haben wir diese Einstellungen nicht in Azure bereitgestellt, aber wenn sich diese Schlüssel in der Datei " *Web. config* " befinden, haben die im Portal angezeigten Einstellungen Vorrang.

Eine bewährte Vorgehensweise besteht darin, einen [devops-Workflow](../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/automate-everything.md) zu befolgen und [Azure PowerShell](https://azure.microsoft.com/documentation/articles/install-configure-powershell/) (oder ein anderes Framework wie [Chef](http://www.opscode.com/chef/) oder [Puppet](http://puppetlabs.com/puppet/what-is-puppet)) zu verwenden, um das Festlegen dieser Werte in Azure zu automatisieren. Das folgende PowerShell-Skript verwendet [Export-CliXML](http://www.powershellcookbook.com/recipe/PukO/securely-store-credentials-on-disk) , um die verschlüsselten Geheimnisse auf den Datenträger zu exportieren:

[!code-powershell[Main](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure/samples/sample6.ps1)]

Im obigen Skript ist "Name" der Name des geheimen Schlüssels, z. b. "&quot;FB\_appsecret&quot; oder" twittersecret ". Sie können die Datei ". Credential" anzeigen, die vom Skript in Ihrem Browser erstellt wurde. Der folgende Code Ausschnitt testet alle Anmelde Informationsdateien und legt die geheimen Schlüssel für die benannte Web-App fest:

[!code-powershell[Main](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure/samples/sample7.ps1)]

> [!WARNING]
> Sicherheit: Geben Sie keine Kenn Wörter oder anderen Geheimnisse in das PowerShell-Skript ein. Dadurch wird der Zweck der Verwendung eines PowerShell-Skripts zum Bereitstellen von sensiblen Daten zunichte gemacht. Das [Get-Credential](https://technet.microsoft.com/library/hh849815.aspx) -Cmdlet stellt einen sicheren Mechanismus zum Abrufen eines Kennworts bereit. Die Verwendung einer Benutzeroberflächen-Eingabeaufforderung kann das Verlust eines Kennworts verhindern

### <a name="deploying-db-connection-strings"></a>Datenbank-Verbindungs Zeichenfolgen

DB-Verbindungs Zeichenfolgen werden ähnlich wie App-Einstellungen behandelt. Wenn Sie Ihre Web-App über Visual Studio bereitstellen, wird die Verbindungs Zeichenfolge für Sie konfiguriert. Dies können Sie im Portal überprüfen. Die empfohlene Vorgehensweise zum Festlegen der Verbindungs Zeichenfolge ist PowerShell. Ein Beispiel für ein PowerShell-Skript erstellt eine Website und eine Datenbank und legt die Verbindungs Zeichenfolge auf der Website, [New-AzureWebsitewithDB. ps1](https://gallery.technet.microsoft.com/scriptcenter/Ultimate-Create-Web-SQL-DB-9e0fdfd3) , aus der [Azure-Skript Bibliothek](https://gallery.technet.microsoft.com/scriptcenter/site/search?f%5B0%5D.Type=RootCategory&amp;f%5B0%5D.Value=WindowsAzure)herunter.

<a id="not"></a>
## <a name="notes-for-php"></a>Hinweise für PHP

Da die Schlüssel-Wert-Paare für **App-Einstellungen** und **Verbindungs** Zeichenfolgen in Umgebungsvariablen auf Azure App Service gespeichert werden, können Entwickler, die beliebige Web-App-Frameworks (z. b. php) verwenden, diese Werte problemlos abrufen. Weitere Informationen finden Sie unter Stefan Schackow [Windows Azure-Websites: Funktionsweise von Anwendungs](https://azure.microsoft.com/blog/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work/) Zeichenfolgen und Verbindungs Zeichenfolgen, die einen PHP-Ausschnitt zum Lesen von App-Einstellungen und Verbindungs Zeichenfolgen anzeigen.

## <a name="notes-for-on-premises-servers"></a>Hinweise für lokale Server

Wenn Sie auf lokalen Webservern bereitstellen, können Sie Geheimnisse sichern, indem Sie [die Konfigurations Abschnitte von Konfigurationsdateien verschlüsseln](https://msdn.microsoft.com/library/ff647398.aspx). Als Alternative können Sie auch denselben Ansatz verwenden, der für Azure Websites empfohlen wird: behalten Sie Entwicklungseinstellungen in Konfigurationsdateien bei, und verwenden Sie Umgebungsvariablen Werte für Produktionseinstellungen. In diesem Fall müssen Sie jedoch Anwendungscode für die Funktion schreiben, die in Azure Websites automatisch erfolgt: Rufen Sie Einstellungen von Umgebungsvariablen ab, und verwenden Sie diese Werte anstelle von Einstellungen der Konfigurationsdatei, oder verwenden Sie die Einstellungen der Konfigurationsdatei, wenn Es wurden keine Umgebungsvariablen gefunden.

<a id="addRes"></a>
## <a name="additional-resources"></a>Weitere Ressourcen

Ein Beispiel für ein PowerShell-Skript, das eine Web-App +-Datenbank erstellt, legt die Verbindungs Zeichenfolge und die App-Einstellungen und den Download von [New-AzureWebsitewithDB. ps1](https://gallery.technet.microsoft.com/scriptcenter/Ultimate-Create-Web-SQL-DB-9e0fdfd3) aus der [Azure-Skript Bibliothek](https://gallery.technet.microsoft.com/scriptcenter/site/search?f%5B0%5D.Type=RootCategory&amp;f%5B0%5D.Value=WindowsAzure)fest. 

Die [Funktionsweise von Anwendungs-und Verbindungs](https://azure.microsoft.com/blog/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work/) Zeichenfolgen finden Sie unter den Windows Azure-Websites von Stefan Schackow.

Besonders vielen Dank an Barry dorrane ( [@blowdart](https://twitter.com/blowdart) ) und Carlos Farre zur Überprüfung.
