---
uid: whitepapers/side-by-side-with-10
title: ASP.net parallele Ausführung von .NET Framework 1,0 und 1,1 | Microsoft-Dokumentation
author: rick-anderson
description: In diesem Whitepaper wird beschrieben, wie Sie .NET 1,0 und .NET 1,1 auf Ihrem Computer installieren, sodass eine ASP.NET-Webanwendung unter beiden Versionen von Fram ausgeführt werden kann...
ms.author: riande
ms.date: 02/10/2010
ms.assetid: bdea2003-e964-4db5-9092-d56cc7560616
msc.legacyurl: /whitepapers/side-by-side-with-10
msc.type: content
ms.openlocfilehash: c123545099013af71569bce4707f2b3eb732c344
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78513831"
---
# <a name="aspnet-side-by-side-execution-of-net-framework-10-and-11"></a>ASP.NET – Parallele Ausführung von .NET Framework 1.0 und 1.1

> In diesem Whitepaper wird beschrieben, wie Sie .NET 1,0 und .NET 1,1 auf Ihrem Computer installieren, sodass eine ASP.NET-Webanwendung in einer der beiden Versionen des Frameworks ausgeführt werden kann.
> 
> Gilt für ASP.NET 1,0 und ASP.NET 1,1.

In ASP.net werden Anwendungen so genannten, dass Sie parallel ausgeführt werden, wenn Sie auf demselben Computer installiert werden, aber unterschiedliche Versionen des .NET Framework verwenden. Im folgenden Thema wird beschrieben, wie ASP.NET-Anwendungen für die parallele Ausführung konfiguriert werden, und es werden ausführliche Schritte für Folgendes bereitgestellt:

- [Beibehalten der Zuordnung Ihrer Webanwendung zur .NET Framework Version 1,0 während der Installation](#1)
- [Ordnen Sie eine Webanwendung einer bestimmten Version des .NET Framework](#2)
- [Ermitteln der Version der .NET Framework, die von einer Website verwendet wird](#3)

Wenn eine Komponente oder eine Anwendung auf einem Computer aktualisiert wird, wird die ältere Version traditionell entfernt und durch die neuere Version ersetzt. Wenn die neue Version nicht mit der vorherigen Version kompatibel ist, unterbricht dies normalerweise andere Anwendungen, die die Komponente oder Anwendung verwenden. Der .NET Framework bietet Unterstützung für die parallele Ausführung, sodass mehrere Versionen einer Assembly oder Anwendung gleichzeitig auf demselben Computer installiert werden können. Da mehrere Versionen gleichzeitig installiert werden können, können verwaltete Anwendungen auswählen, welche Version verwendet werden soll, ohne dass Anwendungen betroffen sind, die eine andere Version verwenden.

Standardmäßig werden bei der Installation der .NET Framework Version 1,1 alle vorhandenen ASP.NET-Anwendungen automatisch neu konfiguriert, um die neueste Version der .NET Framework zu verwenden. Wenn Sie nicht möchten, dass Ihre ASP.NET-Anwendungen standardmäßig .NET Framework 1,1, klicken Sie [hier](#1) , um zu erfahren, wie Sie dies während der Installation verhindern können.

Wenn Sie Ihren Webserver auf .NET Framework 1,1 aktualisieren und eine oder mehrere Webanwendungen .NET Framework 1,0 ausführen möchten, müssen Sie die Skript Zuordnung Internetinformationsdienste (IIS) aktualisieren. Die Skript Zuordnung ist der Mechanismus zum Zuordnen der ASPX-Dateierweiterung für eine bestimmte Webanwendung zu einer Version der .NET Framework. Klicken Sie [hier](#2) , um zu erfahren, wie eine Webanwendung einer bestimmten Version des .NET Framework zugeordnet wird.

Mit dem Internet Information Manager oder dem ASP.NET IIS-Registrierungs Tool (ASPNET\_regiis. exe) können Sie feststellen, welche .NET Framework Version eine bestimmte Webanwendung ausgeführt hat. Klicken Sie [hier](#3) , um zu erfahren, wie Sie die Version der .NET Framework finden, die von einer Website verwendet wird.

Bei der Migration zu .NET Framework 1,1 ist eine Überlegungen zum Import zu beachten, dass jede Version des .NET Framework eine eigene Datei "Machine. config" verwendet. Wenn ein Webadministrator Änderungen an der Datei "Machine. config" vorgenommen hat, müssen diese Änderungen daher in die Datei ".NET Framework 1,1 Machine. config" migriert werden.

<a id="1"></a>

## <a name="maintaining-your-web-applications-mapping-to-net-framework-10-during-installation"></a>Beibehalten der Zuordnung Ihrer Webanwendung zu .NET Framework 1,0 während der Installation

Standardmäßig werden alle vorhandenen ASP.NET-Anwendungen während der Installation automatisch neu konfiguriert, um die neuere Version des .NET Framework zu verwenden. Mithilfe der neueren Version des .NET Framework können Anwendungen die Verbesserungen und neuen Features in der neuen Version in vollem Umfang nutzen. Gleichzeitig kann der Webadministrator, der die genaue Kontrolle über die aktualisierten Anwendungen hätte, die automatische Neuzuordnung aller vorhandenen ASP.NET-Anwendungen während der Installation des .NET Framework verhindern.

Um die automatische Neuzuordnung der gesamten ASP.NET-Anwendung zur neueren Version des .NET Framework zu verhindern, kann der Webadministrator die Befehlszeilenoption/noaspupgrade mit dem Setup Programm Dotnetfx. exe verwenden.

**So verhindern Sie die Neuzuordnung der ASP.NET-Anwendung zu einer neueren Version**

1. Navigieren Sie zu **Start**.
2. Klicken Sie auf **Ausführen**.
3. Geben Sie **cmd** ein.
4. Klicken Sie auf **OK**.  
  
    ![](side-by-side-with-10/_static/image1.gif)
5. Geben Sie an der Eingabeaufforderung die folgende Zeile ein, um die Installation des .NET Framework zu starten: **Dotnetfx. exe/c: "Install/noaspupgrade?** .  
  
    ![](side-by-side-with-10/_static/image2.gif)
6. Klicken Sie im Microsoft .NET Framework 1,1-Setup auf **Ja** . Dadurch wird der Setup Vorgang des .NET Framework 1,1 gestartet.  
  
    ![](side-by-side-with-10/_static/image3.gif)

<a id="2"></a>

## <a name="map-a-web-application-to-a-specific-version-of-the-net-framework"></a>Ordnen Sie eine Webanwendung einer bestimmten Version des .NET Framework

Jede Version der .NET Framework enthält eine Version des ASP.NET IIS-Registrierungs Tools (ASPNET\_regiis. exe). Mit diesem Tool können Administratoren angeben, dass eine Webanwendung unter einer bestimmten Version des .NET Framework ausgeführt werden soll. Dies wird als Zuordnung einer Webanwendung zu einer Version der .NET Framework bezeichnet. Administratoren müssen die ASPNET-\_regiis. exe auswählen, die der Version des .NET Framework entspricht, der der Webanwendung zugeordnet werden soll. Ein Administrator, der beispielsweise angeben möchte, dass eine Website .NET Framework 1,1 verwendet, muss die ASPNET-\_regiis. exe verwenden, die mit .NET Framework 1,1 ausgestattet ist.

Die ASPNET-\_regiis. exe für Version 1,0 befindet sich unter:

- C:\WINDOWS\Microsoft.NET\Framework\\**v 1.0.3705**\ASPNET\_regiis

Die ASPNET-\_regiis. exe für Version 1, 1 befindet sich unter:

- C:\WINDOWS\Microsoft.NET\Framework\\**v 1.1.4322**\ASPNET\_regiis

Die ASPNET-\_regiis. exe bietet zwei Optionen für die Skript Zuordnung für eine Webanwendung:

- **-s** legt die Skript Zuordnung im Pfad und in den zugehörigen untergeordneten Verzeichnissen fest.
- **-SN** legt die Skript Zuordnung nur im Pfad fest.

Der Pfad definiert den IIS-Metadatenpfad der Webanwendung, der in Form von W3SVC/root/{websitenreber}/{Application\_Name} definiert ist. Für eine Webanwendung namens "Portal" unter der Standard Website lautet der Metabasispfad beispielsweise W3SVC/1/root/Portal.

![](side-by-side-with-10/_static/image4.gif)

Hinweis Sie können auch ein Tool mit dem Namen Metabase Editor verwenden, um den Metabasispfad zu erhalten. Sie können dieses Tool von der Microsoft-Support-Website unter [https://support.microsoft.com/default.aspx?scid=kb; en-US; 232068](https://support.microsoft.com/default.aspx?scid=kb;en-us;232068) herunterladen.

- Führen Sie ASPNET\_regiis. exe-s W3SVC/1/root/Portal aus, um die Portal-IIS-Skript Map und deren unter Anwendung zu aktualisieren.  
  
    ![](side-by-side-with-10/_static/image5.gif)

- Führen Sie ASPNET\_regiis. exe-SN W3SVC/1/root/Portal aus, um die IIS-Skript Zuordnung des Portals zu aktualisieren, ohne dass sich dies auf die Anwendungen in den Unterverzeichnissen des Portals auswirkt.  
  
    ![](side-by-side-with-10/_static/image6.gif)

<a id="3"></a>

## <a name="find-the-net-framework-version-that-a-web-application-is-using"></a>Suchen der .NET Framework Version, die eine Webanwendung verwendet

Ein Administrator kann den Internet Service Manager verwenden, um zu ermitteln, welche Version des .NET Framework eine Website ausführt. Unterschiedliche Betriebssystemversionen starten das Internet Service Manager anders. Führen Sie die unten aufgeführten Schritte aus, um den Dienst-Manager zu starten.

**So starten Sie Internet Service Manager**

1. Navigieren Sie zu **Start**.
2. Klicken Sie auf **Ausführen**.
3. Geben Sie **inetmgr**ein.  
  
    ![](side-by-side-with-10/_static/image7.gif)
4. Wählen Sie im Internet Service Manager die Webanwendung aus, deren Version der .NET Framework Sie wissen möchten.  
  
    ![](side-by-side-with-10/_static/image8.gif)
5. Klicken Sie mit der rechten Maustaste auf die Webanwendung, und klicken Sie auf **Eigenschaften.**  
  
    ![](side-by-side-with-10/_static/image9.gif)
6. Wählen Sie im Eigenschaften Fenster die Option **Konfiguration aus.**  
  
    ![](side-by-side-with-10/_static/image10.gif)
7. Wählen Sie in der Tabelle Anwendungs Zuordnung die Option **. aspx**aus, und klicken Sie auf **Bearbeiten**.  
  
    ![](side-by-side-with-10/_static/image11.gif)
8. Überprüfen Sie im Textfeld **ausführbare** Datei das Versions Verzeichnis, indem Sie einen Bildlauf ausführen. Wenn das Versions Verzeichnis v. 1.1.4322 ist, wird die Anwendung .NET Framework 1,1 zugeordnet. Wenn das Versions Verzeichnis also v 1.0.3705 ist, wird die Anwendung .NET Framework 1,0 zugeordnet.  
  
    ![](side-by-side-with-10/_static/image12.gif)
