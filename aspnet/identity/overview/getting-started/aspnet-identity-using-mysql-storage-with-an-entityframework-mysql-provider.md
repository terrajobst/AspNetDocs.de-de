---
uid: identity/overview/getting-started/aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider
title: 'ASP.net Identity: Verwenden von MySQL-Speicher mit einem EntityFramework MySQLC#-Anbieter ()-ASP.NET 4. x'
author: maumar
description: In diesem Tutorial wird gezeigt, wie Sie den standardmäßigen Datenspeichermechanismus für ASP.net Identity mit EntityFramework (SQL-Client Anbieter) durch eine MySQL-Bereitstellung ersetzen...
ms.author: riande
ms.date: 12/10/2013
ms.assetid: 15253312-a92c-43ba-908e-b5dacd3d08b8
ms.custom: seoapril2019
msc.legacyurl: /identity/overview/getting-started/aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider
msc.type: authoredcontent
ms.openlocfilehash: e89ed139657c5ce9ddcc56879946c62038919483
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78471873"
---
# <a name="aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider-c"></a>ASP.net Identity: Verwenden von MySQL-Speicher mit einem EntityFramework MySQLC#-Anbieter ()

von [Maurycy Markowski](https://github.com/maumar), [Raquel de Almeida](https://github.com/raquelsa), [Robert McMurray](https://github.com/rmcmurray)

> In diesem Tutorial wird gezeigt, wie Sie den standardmäßigen Datenspeichermechanismus für [**ASP.net Identity**](introduction-to-aspnet-identity.md) mit EntityFramework (SQL-Client Anbieter) durch einen MySQL-Anbieter ersetzen.

In diesem Tutorial werden die folgenden Themen behandelt:

- Erstellen einer MySQL-Datenbank in Azure
- Erstellen einer MVC-Anwendung mit Visual Studio 2013 MVC-Vorlage
- Konfigurieren von EntityFramework für die Zusammenarbeit mit einem MySQL-Datenbankanbieter
- Ausführen der Anwendung zum Überprüfen der Ergebnisse

Am Ende dieses Tutorials verfügen Sie über eine MVC-Anwendung mit dem ASP.net Identity-Speicher, der eine MySQL-Datenbank verwendet, die in Azure gehostet wird.

## <a name="creating-a-mysql-database-instance-on-azure"></a>Erstellen einer MySQL-Daten Bank Instanz in Azure

1. Melden Sie sich beim [Azure-Portal](https://go.microsoft.com/fwlink/?linkid=529715&amp;clcid=0x409)an.
2. Klicken Sie unten auf der Seite auf **neu** , und wählen Sie dann **Speichern**aus:

    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image2.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image1.png)
3. Wählen Sie im Assistenten zum **auswählen und hinzufügen** von **cleardb MySQL-Datenbank**aus, und klicken Sie dann unten im Frame auf den Pfeil " **weiter** ":

   [Klicken Sie auf die folgende Abbildung, um Sie zu erweitern. ] [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image4.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image3.png)
4. Behalten Sie den standardmäßigen **Free** -Plan bei, ändern Sie den **Namen** in **identitymysqldatabase**, wählen Sie die Region aus, die Sie am nächsten ist, und klicken Sie dann unten im Frame auf den Pfeil " **weiter** ":

   [Klicken Sie auf die folgende Abbildung, um Sie zu erweitern. ] [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image6.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image5.png)
5. Klicken Sie **auf das** Häkchensymbol, um die Erstellung der Datenbank abzuschließen.

   [Klicken Sie auf die folgende Abbildung, um Sie zu erweitern. ] [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image8.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image7.png)
6. Nachdem die Datenbank erstellt wurde, können Sie sie auf der Registerkarte **ADD-ONS** im Verwaltungsportal verwalten. Zum Abrufen der Verbindungsinformationen für die Datenbank klicken Sie unten auf der Seite auf **Verbindungs** Informationen:

   [Klicken Sie auf die folgende Abbildung, um Sie zu erweitern. ] [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image10.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image9.png)
7. Kopieren Sie die Verbindungs Zeichenfolge, indem Sie auf die Schaltfläche "Kopieren" im Feld " **ConnectionString** " klicken und speichern. Diese Informationen werden später in diesem Tutorial für Ihre MVC-Anwendung verwendet:

   [Klicken Sie auf die folgende Abbildung, um Sie zu erweitern. ] [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image12.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image11.png)

## <a name="creating-an-mvc-application-project"></a>Erstellen eines MVC-Anwendungs Projekts

Um die Schritte in diesem Abschnitt des Tutorials ausführen zu können, müssen Sie zuerst [Visual Studio Express 2013 für Web](https://go.microsoft.com/fwlink/?LinkId=299058) -oder [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566)installieren. Nachdem Visual Studio installiert wurde, führen Sie die folgenden Schritte aus, um ein neues MVC-Anwendungsprojekt zu erstellen:

1. Öffnen Sie Visual Studio 2103.
2. Klicken Sie auf der **Start** Seite auf **Neues Projekt** , oder klicken Sie auf das Menü **Datei** und dann auf **Neues Projekt**:

   [Klicken Sie auf die folgende Abbildung, um Sie zu erweitern. ] [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image2.jpg)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image1.jpg)
3. Wenn das Dialogfeld **Neues Projekt** angezeigt wird, erweitern Sie in der Liste der Vorlagen die Option **Visualisierung C#**  , klicken Sie auf **Web**, und wählen Sie **ASP.NET Webanwendung**aus. Benennen Sie Ihr Projekt **identitymysqldemo** , und klicken Sie dann auf **OK**:

   [Klicken Sie auf die folgende Abbildung, um Sie zu erweitern. ] [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image14.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image13.png)
4. Wählen Sie im Dialogfeld **Neues ASP.net-Projekt** die Option **MVC** -vorlagenvorlage mit den Standardoptionen aus. Dadurch werden **einzelne Benutzerkonten** als Authentifizierungsmethode konfiguriert. Klicken Sie auf **OK**:

   [Klicken Sie auf die folgende Abbildung, um Sie zu erweitern. ] [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image16.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image15.png)

## <a name="configure-entityframework-to-work-with-a-mysql-database"></a>Konfigurieren von EntityFramework für die Arbeit mit einer MySQL-Datenbank

### <a name="update-the-entity-framework-assembly-for-your-project"></a>Aktualisieren Sie die Entity Framework Assembly für Ihr Projekt.

Die MVC-Anwendung, die aus der Visual Studio 2013 Vorlage erstellt wurde, enthält einen Verweis auf das 6.0.0-Paket " [EntityFramework](http://www.nuget.org/packages/EntityFramework) ", es wurden jedoch seit der Veröffentlichung Updates für diese Assembly erstellt, die bedeutende Leistungsverbesserungen enthalten. Führen Sie die folgenden Schritte aus, um die neuesten Updates in Ihrer Anwendung zu verwenden.

1. Öffnen Sie Ihr MVC-Projekt in Visual Studio.
2. Klicken **Sie auf Extras und dann**auf **nuget-Paket-Manager**und anschließend auf Paket-Manager- **Konsole**:

   [Klicken Sie auf die folgende Abbildung, um Sie zu erweitern. ] [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image18.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image17.png)
3. Die **Paket-Manager-Konsole** wird im unteren Abschnitt von Visual Studio angezeigt. Geben Sie &quot;**Update-Package EntityFramework**&quot; ein, und drücken Sie die EINGABETASTE:

   [Klicken Sie auf die folgende Abbildung, um Sie zu erweitern. ] [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image20.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image19.png)

### <a name="install-the-mysql-provider-for-entityframework"></a>Installieren des MySQL-Anbieters für EntityFramework

Damit von EntityFramework eine Verbindung mit der MySQL-Datenbank hergestellt werden kann, müssen Sie einen MySQL-Anbieter installieren. Öffnen Sie hierzu die Paket- **Manager-Konsole** , geben Sie &quot;**install-Package MySQL. Data. Entity-Pre**&quot;ein, und drücken Sie dann die EINGABETASTE.

> [!NOTE]
> Dabei handelt es sich um eine Vorabversion der Assembly, die möglicherweise Fehler enthält. Sie sollten keine Vorabversion des Anbieters in der Produktionsumgebung verwenden.

[Klicken Sie auf das folgende Bild, um es zu erweitern.]

[![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image22.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image21.png)

### <a name="making-project-configuration-changes-to-the-webconfig-file-for-your-application"></a>Vornehmen von Änderungen an der Projekt Konfiguration in der Datei "Web. config" für Ihre Anwendung

In diesem Abschnitt Konfigurieren Sie die Entity Framework für die Verwendung des soeben installierten MySQL-Anbieters, registrieren die MySQL-Anbieterfactory und fügen Ihre Verbindungs Zeichenfolge aus Azure hinzu.

> [!NOTE]
> Die folgenden Beispiele enthalten eine bestimmte Assemblyversion für MySQL. Data. dll. Wenn die Assemblyversion geändert wird, müssen Sie die entsprechenden Konfigurationseinstellungen mit der richtigen Version ändern.

1. Öffnen Sie die Datei "Web. config" für Ihr Projekt in Visual Studio 2013.
2. Suchen Sie die folgenden Konfigurationseinstellungen, mit denen der Standarddaten Bank Anbieter und die Factory für die Entity Framework definiert werden:

    [!code-xml[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample1.xml)]
3. Ersetzen Sie diese Konfigurationseinstellungen durch Folgendes, um die Entity Framework so zu konfigurieren, dass der MySQL-Anbieter verwendet wird:

    [!code-xml[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample2.xml)]
4. Suchen Sie den &lt;connectionStrings-&gt; Abschnitt, und ersetzen Sie ihn durch den folgenden Code, der die Verbindungs Zeichenfolge für Ihre MySQL-Datenbank definiert, die in Azure gehostet wird (Beachten Sie, dass der Wert für "ProviderName" auch aus dem ursprünglichen geändert wurde):

    [!code-xml[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample3.xml?highlight=3-4)]

### <a name="adding-custom-migrationhistory-context"></a>Benutzerdefinierten migrationhistory-Kontext wird hinzugefügt

Entity Framework Code First verwendet eine **migrationhistory** -Tabelle, um Modelländerungen nachzuverfolgen und die Konsistenz zwischen dem Datenbankschema und dem konzeptionellen Schema sicherzustellen. Diese Tabelle funktioniert jedoch nicht standardmäßig für MySQL, da der Primärschlüssel zu groß ist. Um diese Situation zu beheben, müssen Sie die Schlüsselgröße für diese Tabelle verkleinern. Führen Sie dazu die folgenden Schritte aus:

1. Die Schema Informationen für diese Tabelle werden in einem **historycontext**aufgezeichnet, der wie ein beliebiger anderer **dbcontext**geändert werden kann. Fügen Sie zu diesem Zweck dem Projekt eine neue Klassendatei mit dem Namen **MySqlHistoryContext.cs** hinzu, und ersetzen Sie den Inhalt durch den folgenden Code:

    [!code-csharp[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample4.cs)]
2. Als nächstes müssen Sie Entity Framework so konfigurieren, dass die geänderte **historycontext**anstelle der Standardeinstellung verwendet wird. Dies kann durch die Verwendung von Code basierten Konfigurationsfunktionen erreicht werden. Fügen Sie zu diesem Zweck dem Projekt eine neue Klassendatei mit dem Namen **MySqlConfiguration.cs** hinzu, und ersetzen Sie den Inhalt durch:

    [!code-csharp[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample5.cs)]

### <a name="creating-a-custom-entityframework-initializer-for-applicationdbcontext"></a>Erstellen eines benutzerdefinierten EntityFramework-Initialisierers für "applicationdbcontext"

Der in diesem Tutorial vorgestellte MySQL-Anbieter unterstützt derzeit keine Entity Framework Migrationen, sodass Sie modellinitialisierer verwenden müssen, um eine Verbindung mit der Datenbank herzustellen. Da in diesem Tutorial eine MySQL-Instanz in Azure verwendet wird, müssen Sie einen benutzerdefinierten Entity Framework Initialisierer erstellen.

> [!NOTE]
> Dieser Schritt ist nicht erforderlich, wenn Sie eine Verbindung mit einer SQL Server Instanz in Azure herstellen oder wenn Sie eine Datenbank verwenden, die lokal gehostet wird.

Führen Sie die folgenden Schritte aus, um einen benutzerdefinierten Entity Framework Initialisierer für MySQL zu erstellen:

1. Fügen Sie dem Projekt eine neue Klassendatei mit dem Namen **MySqlInitializer.cs** hinzu, und ersetzen Sie deren Inhalt durch den folgenden Code:

    [!code-csharp[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample6.cs?highlight=23)]
2. Öffnen Sie die Datei **IdentityModels.cs** für das Projekt, das sich im Verzeichnis **Models** befindet, und ersetzen Sie den Inhalt durch Folgendes:

    [!code-csharp[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample7.cs)]

## <a name="running-the-application-and-verifying-the-database"></a>Ausführen der Anwendung und Überprüfen der Datenbank

Nachdem Sie die Schritte in den vorherigen Abschnitten ausgeführt haben, sollten Sie Ihre Datenbank testen. Führen Sie dazu die folgenden Schritte aus:

1. Drücken Sie **STRG + F5** , um die Webanwendung zu erstellen und auszuführen.
2. Klicken Sie oben auf der Seite auf die Registerkarte **registrieren** :

   [Klicken Sie auf die folgende Abbildung, um Sie zu erweitern. ] [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image4.jpg)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image3.jpg)
3. Geben Sie einen neuen Benutzernamen und ein Kennwort ein, und klicken Sie dann auf **registrieren**:

   [Klicken Sie auf die folgende Abbildung, um Sie zu erweitern. ] [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image24.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image23.png)
4. An diesem Punkt werden die ASP.net Identity Tabellen in der MySQL-Datenbank erstellt, und der Benutzer wird registriert und bei der Anwendung angemeldet:

   [Klicken Sie auf die folgende Abbildung, um Sie zu erweitern. ] [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image6.jpg)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image5.jpg)

### <a name="installing-mysql-workbench-tool-to-verify-the-data"></a>Installieren des MySQL Workbench-Tools zum Überprüfen der Daten

1. Installieren Sie das **MySQL Workbench** -Tool auf der [MySQL-Downloadseite](http://dev.mysql.com/downloads/windows/installer/) .
2. Wählen Sie im Installations-Assistenten für die Registerkarte **Funktionsauswahl** unter **Anwendungs** Abschnitt die Option **MySQL Workbench** aus.
3. Starten Sie die APP, und fügen Sie eine neue Verbindung mit den Verbindungs Zeichenfolgen-Daten aus der Azure MySQL-Datenbank hinzu, die Sie beim betteln dieses Tutorials erstellt haben.
4. Überprüfen Sie nach dem Herstellen der Verbindung die **ASP.net Identity** Tabellen, die für die **identitymysqldatabase** erstellt wurden.
5. Sie werden feststellen, dass alle ASP.net Identity erforderlichen Tabellen erstellt werden, wie in der folgenden Abbildung dargestellt:

   [Klicken Sie auf die folgende Abbildung, um Sie zu erweitern. ] [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image8.jpg)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image7.jpg)
6. Überprüfen Sie die **aspnettusers** -Tabelle auf die Instanz, um die Einträge beim Registrieren neuer Benutzer zu überprüfen.

   [Klicken Sie auf die folgende Abbildung, um Sie zu erweitern. ] [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image26.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image25.png)
