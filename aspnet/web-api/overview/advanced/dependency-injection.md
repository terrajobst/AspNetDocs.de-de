---
uid: web-api/overview/advanced/dependency-injection
title: Abhängigkeitsinjektion in ASP.net-Web-API 2-ASP.NET 4. x
author: MikeWasson
description: In diesem Tutorial wird gezeigt, wie Sie Abhängigkeiten in den ASP.net-Web-API Controller für ASP.NET 4. x einfügen.
ms.author: riande
ms.date: 01/20/2014
ms.custom: seoapril2019
ms.assetid: e3d3e7ba-87f0-4032-bdd3-31f3c1aa9d9c
msc.legacyurl: /web-api/overview/advanced/dependency-injection
msc.type: authoredcontent
ms.openlocfilehash: f9c212af92168ac02644625b9aa8ec1bef329cab
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2019
ms.locfileid: "74600409"
---
# <a name="dependency-injection-in-aspnet-web-api-2"></a>Abhängigkeitsinjektion in ASP.net-Web-API 2

von [Mike Wasson](https://github.com/MikeWasson)

[Herunterladen des abgeschlossenen Projekts](https://code.msdn.microsoft.com/ASP-NET-Web-API-Tutorial-468ee148)

> In diesem Tutorial wird gezeigt, wie Sie Abhängigkeiten in Ihren ASP.net-Web-API Controller einfügen.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Im Tutorial verwendete Software Versionen
> 
> 
> - Web-API 2
> - [Unity-Anwendungs Block](https://www.nuget.org/packages/Unity/)
> - Entity Framework 6 (Version 5 funktioniert auch)

## <a name="what-is-dependency-injection"></a>Was ist eine Abhängigkeitsinjektion?

Eine *Abhängigkeit* ist ein beliebiges Objekt, das ein anderes Objekt benötigt. Beispielsweise ist es üblich, ein [Repository](http://martinfowler.com/eaaCatalog/repository.html) zu definieren, das den Datenzugriff behandelt. Sehen wir uns ein Beispiel an. Zuerst definieren wir ein Domänen Modell:

[!code-csharp[Main](dependency-injection/samples/sample1.cs)]

Im folgenden finden Sie eine einfache Repository-Klasse, in der Elemente in einer Datenbank mithilfe von Entity Framework gespeichert werden.

[!code-csharp[Main](dependency-injection/samples/sample2.cs)]

Definieren wir nun einen Web-API-Controller, der Get-Anforderungen für `Product` Entitäten unterstützt. (Ich verlasse Post und andere Methoden zur Vereinfachung.) Hier ist ein erster Versuch:

[!code-csharp[Main](dependency-injection/samples/sample3.cs)]

Beachten Sie, dass die Controller Klasse von `ProductRepository`abhängt und dass der Controller die `ProductRepository` Instanz erstellen kann. Es ist jedoch eine gute Idee, die Abhängigkeit aus verschiedenen Gründen hart zu codieren.

- Wenn Sie `ProductRepository` durch eine andere Implementierung ersetzen möchten, müssen Sie auch die Controller Klasse ändern.
- Wenn die `ProductRepository` Abhängigkeiten aufweist, müssen Sie diese innerhalb des Controllers konfigurieren. Bei einem großen Projekt mit mehreren Controllern wird der Konfigurations Code in Ihrem Projekt verstreut.
- Der Komponenten Test ist schwierig, da der Controller hart codiert ist, um die Datenbank abzufragen. Für einen-Komponenten Test sollten Sie ein Mock-oder Stub-Repository verwenden, das mit dem aktuellen Entwurf nicht möglich ist.

Wir können diese Probleme beheben, indem Sie das Repository in den Controller *Einfügen* . Umgestalten Sie zunächst die `ProductRepository` Klasse in eine Schnittstelle:

[!code-csharp[Main](dependency-injection/samples/sample4.cs)]

Geben Sie dann die `IProductRepository` als Konstruktorparameter an:

[!code-csharp[Main](dependency-injection/samples/sample5.cs)]

In diesem Beispiel wird die [Konstruktorinjektion](http://www.martinfowler.com/articles/injection.html#FormsOfDependencyInjection)verwendet. Sie können auch *Setter Injection*verwenden, bei dem Sie die Abhängigkeit über eine Setter-Methode oder-Eigenschaft festlegen.

Doch jetzt liegt ein Problem vor, da Ihre Anwendung den Controller nicht direkt erstellt. Die Web-API erstellt den Controller, wenn Sie die Anforderung weiterleitet, und die Web-API weiß nichts über `IProductRepository`. An dieser Stelle kommt der Web-API-Abhängigkeits Konflikt Löser ins Spiel.

## <a name="the-web-api-dependency-resolver"></a>Der Web-API-Abhängigkeits Konflikt Löser

Die Web-API definiert die **idepdencyresolver** -Schnittstelle zum Auflösen von Abhängigkeiten. Hier ist die Definition der-Schnittstelle:

[!code-csharp[Main](dependency-injection/samples/sample6.cs)]

Die **idepdencyscope** -Schnittstelle verfügt über zwei Methoden:

- **GetService** erstellt eine Instanz eines Typs.
- **GetServices** erstellt eine Auflistung von Objekten eines angegebenen Typs.

Die **idepdencyresolver** -Methode erbt **idepdencyscope** und fügt die **BeginScope** -Methode hinzu. Später in diesem Tutorial werde ich über Bereiche sprechen.

Wenn die Web-API eine Controller Instanz erstellt, ruft Sie zuerst **idepdencyresolver. GetService**auf und übergibt dabei den Controllertyp. Mit diesem Erweiterbarkeits Hook können Sie den Controller erstellen und alle Abhängigkeiten auflösen. Wenn **GetService** NULL zurückgibt, sucht die Web-API in der Controller Klasse nach einem Parameter losen Konstruktor.

## <a name="dependency-resolution-with-the-unity-container"></a>Abhängigkeitsauflösung mit dem Unity-Container

Obwohl Sie eine vollständige **idepdencyresolver** -Implementierung von Grund auf neu schreiben könnten, ist die Schnittstelle eigentlich so konzipiert, dass Sie als Bridge zwischen Web-API und vorhandenen IOC-Containern fungiert.

Ein IOC-Container ist eine Softwarekomponente, die für die Verwaltung von Abhängigkeiten zuständig ist. Sie registrieren Typen beim Container und verwenden dann den Container, um Objekte zu erstellen. Der Container ermittelt automatisch die Abhängigkeitsbeziehungen. Viele IOC-Container ermöglichen es Ihnen außerdem, Dinge wie Objekt Lebensdauer und Bereich zu steuern.

> [!NOTE]
> "IOC" steht für "Inversion of Control", bei dem es sich um ein allgemeines Muster handelt, bei dem ein Framework Anwendungscode aufruft. Ein IOC-Container erstellt Ihre Objekte für Sie, was die übliche Ablauf Steuerung "Rück kehrt".

In diesem Tutorial verwenden wir [Unity](https://msdn.microsoft.com/library/ff647202.aspx) aus Microsoft Patterns &amp; Practices. (Weitere beliebte Bibliotheken sind [Castle Windsor](http://www.castleproject.org/), [Spring.net](http://www.springframework.net/), [autofac](https://code.google.com/p/autofac/), [Ninject](http://www.ninject.org/)und [StructureMap](http://structuremap.github.io/documentation/).) Sie können den nuget-Paket-Manager verwenden, um Unity zu installieren. Wählen Sie **im Menü Extras** in Visual Studio **nuget-Paket-Manager**und dann **Paket-Manager-Konsole**aus. Geben Sie im Fenster der Paket-Manager-Konsole den folgenden Befehl ein:

[!code-console[Main](dependency-injection/samples/sample7.cmd)]

Im folgenden finden Sie eine Implementierung von **idepdencyresolver** , der einen Unity-Container umschließt.

[!code-csharp[Main](dependency-injection/samples/sample8.cs)]

> [!NOTE]
> Wenn die **GetService** -Methode einen Typ nicht auflösen kann, sollte Sie **null**zurückgeben. Wenn die **GetServices** -Methode einen Typ nicht auflösen kann, sollte ein leeres Auflistungs Objekt zurückgegeben werden. Lösen Sie keine Ausnahmen für unbekannte Typen aus.

## <a name="configuring-the-dependency-resolver"></a>Konfigurieren des Abhängigkeits Konflikt Lösers

Legen Sie den Abhängigkeits Konflikt Löser für die **DependencyResolver** -Eigenschaft des globalen **httpconfiguration** -Objekts fest.

Der folgende Code registriert die `IProductRepository`-Schnittstelle bei Unity und erstellt dann eine `UnityResolver`.

[!code-csharp[Main](dependency-injection/samples/sample9.cs)]

## <a name="dependency-scope-and-controller-lifetime"></a>Abhängigkeits Bereich und Controller Lebensdauer

Controller werden pro Anforderung erstellt. Zum Verwalten der Objekt Lebensdauer verwendet **idepdencyresolver** das Konzept eines *Bereichs*.

Der an das **httpconfiguration** -Objekt angefügte Abhängigkeits Konflikt Löser verfügt über einen globalen Gültigkeitsbereich. Wenn die Web-API einen Controller erstellt, wird **BeginScope**aufgerufen. Diese Methode gibt einen **idepdencyscope** -Wert zurück, der einen untergeordneten Bereich darstellt.

Die Web-API ruft dann **GetService** für den untergeordneten Bereich auf, um den Controller zu erstellen. Wenn die Anforderung erfüllt ist, löschen Web-API **-Aufrufe den untergeordneten** Bereich. Verwenden Sie die verwerfen **-Methode, um die Abhängigkeiten** des Controllers zu verwerfen.

Wie Sie **BeginScope** implementieren, hängt vom IOC-Container ab. Für Unity entspricht der Gültigkeitsbereich einem untergeordneten Container:

[!code-csharp[Main](dependency-injection/samples/sample10.cs)]

Die meisten IOC-Container verfügen über ähnliche Entsprechungen.
