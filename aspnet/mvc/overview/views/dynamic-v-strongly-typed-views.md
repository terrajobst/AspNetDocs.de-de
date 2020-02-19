---
uid: mvc/overview/views/dynamic-v-strongly-typed-views
title: Dynamische im Vergleich zu Stark typisierte Ansichten | Microsoft-Dokumentation
author: Rick-Anderson
description: ''
ms.author: riande
ms.date: 01/27/2011
ms.assetid: 0cbd88da-0da6-4605-b222-2835c6478304
msc.legacyurl: /mvc/overview/views/dynamic-v-strongly-typed-views
msc.type: authoredcontent
ms.openlocfilehash: 3e81c6381b1e280e3b74cb7eb6ea6e6c3224e655
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/19/2020
ms.locfileid: "77455656"
---
# <a name="dynamic-v-strongly-typed-views"></a>Dynamische im Vergleich zu Stark typisierten Ansichten

von [Rick Anderson](https://twitter.com/RickAndMSFT)

Es gibt drei Möglichkeiten, Informationen von einem Controller an eine Ansicht in ASP.NET MVC 3 zu übergeben:

1. Als stark typisiertes Modell Objekt.
2. Als dynamischer Typ (mit @model Dynamic)
3. Verwenden der viewbag

Ich habe eine einfache Top-Blog Anwendung von MVC 3 geschrieben, um dynamische und stark typisierte Ansichten zu vergleichen und zu vergleichen. Der Controller beginnt mit einer einfachen Liste von Blogs:

[!code-csharp[Main](dynamic-v-strongly-typed-views/samples/sample1.cs)]

Klicken Sie mit der rechten Maustaste in die indexnotstonglytypi()-Methode, und fügen Sie eine Razor-Ansicht hinzu

[![8475. notstronglytypedview [1]](dynamic-v-strongly-typed-views/_static/image2.png)](dynamic-v-strongly-typed-views/_static/image1.png)

Stellen Sie sicher, dass das Feld **eine stark typisierte Ansicht erstellen** nicht aktiviert ist. Die resultierende Sicht enthält nicht viel:

[!code-cshtml[Main](dynamic-v-strongly-typed-views/samples/sample2.cshtml)]

[!code-cshtml[Main](dynamic-v-strongly-typed-views/samples/sample3.cshtml)]

Da wir eine dynamische und keine stark typisierte Ansicht verwenden, unterstützt IntelliSense uns nicht. Der abgeschlossene Code wird unten angezeigt:

[!code-cshtml[Main](dynamic-v-strongly-typed-views/samples/sample4.cshtml)]

[![6646. NotStronglyTypedView_5F00_IE [1]](dynamic-v-strongly-typed-views/_static/image4.png)](dynamic-v-strongly-typed-views/_static/image3.png)

Nun fügen wir eine stark typisierte Ansicht hinzu. Fügen Sie dem Controller den folgenden Code hinzu:

[!code-csharp[Main](dynamic-v-strongly-typed-views/samples/sample5.cs)]

Beachten Sie, dass es sich genau um dieselbe Rückgabe Ansicht (TopBlogs) handelt. als nicht stark typisierte Ansicht aufzurufen. Klicken Sie mit der rechten Maustaste in *stonglytypedindex ()* , und wählen Sie **Ansicht hinzufügen**aus. Wählen Sie diesmal die **Blog** Modell Klasse aus, und wählen Sie **Liste** als Gerüst Vorlage aus.

[![5658). strongview [1]](dynamic-v-strongly-typed-views/_static/image6.png)](dynamic-v-strongly-typed-views/_static/image5.png)

In der neuen Ansichts Vorlage erhalten Sie IntelliSense-Unterstützung.

[![7002. IntelliSense [1]](dynamic-v-strongly-typed-views/_static/image8.png)](dynamic-v-strongly-typed-views/_static/image7.png)

Das c#-Projekt kann [hier](https://blogs.msdn.com/cfs-file.ashx/__key/CommunityServer-Blogs-Components-WeblogFiles/00-00-01-11-73-SSMS/1817.Mvc3ViewDemo.zip)heruntergeladen werden.
