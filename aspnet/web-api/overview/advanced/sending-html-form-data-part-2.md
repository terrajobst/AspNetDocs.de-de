---
uid: web-api/overview/advanced/sending-html-form-data-part-2
title: 'Senden von HTML-Formulardaten in ASP.net-Web-API: Datei Upload und mehrteilige MIME-ASP.NET 4. x'
author: MikeWasson
description: In diesem Tutorial wird gezeigt, wie Sie Dateien in eine Web-API hochladen. Außerdem wird beschrieben, wie mehrteilige MIME-Daten verarbeitet werden.
ms.author: riande
ms.date: 06/21/2012
ms.custom: seoapril2019
ms.assetid: a7f3c1b5-69d9-4261-b082-19ffafa5f16a
msc.legacyurl: /web-api/overview/advanced/sending-html-form-data-part-2
msc.type: authoredcontent
ms.openlocfilehash: f5aaebb96f631dfb6b0da1fbca96cd93a6a7fe2d
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78449211"
---
# <a name="sending-html-form-data-in-aspnet-web-api-file-upload-and-multipart-mime"></a><span data-ttu-id="ae387-104">Senden von HTML-Formulardaten in ASP.net-Web-API: Datei Upload und mehrteilige MIME</span><span class="sxs-lookup"><span data-stu-id="ae387-104">Sending HTML Form Data in ASP.NET Web API: File Upload and Multipart MIME</span></span>

<span data-ttu-id="ae387-105">von [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="ae387-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

## <a name="part-2-file-upload-and-multipart-mime"></a><span data-ttu-id="ae387-106">Teil 2: Datei Upload und mehrteilige MIME</span><span class="sxs-lookup"><span data-stu-id="ae387-106">Part 2: File Upload and Multipart MIME</span></span>

<span data-ttu-id="ae387-107">In diesem Tutorial wird gezeigt, wie Sie Dateien in eine Web-API hochladen.</span><span class="sxs-lookup"><span data-stu-id="ae387-107">This tutorial shows how to upload files to a web API.</span></span> <span data-ttu-id="ae387-108">Außerdem wird beschrieben, wie mehrteilige MIME-Daten verarbeitet werden.</span><span class="sxs-lookup"><span data-stu-id="ae387-108">It also describes how to process multipart MIME data.</span></span>

> [!NOTE]
> <span data-ttu-id="ae387-109">[Laden Sie das abgeschlossene Projekt herunter](https://code.msdn.microsoft.com/ASPNET-Web-API-File-Upload-a8c0fb0d).</span><span class="sxs-lookup"><span data-stu-id="ae387-109">[Download the completed project](https://code.msdn.microsoft.com/ASPNET-Web-API-File-Upload-a8c0fb0d).</span></span>

<span data-ttu-id="ae387-110">Im folgenden finden Sie ein Beispiel für ein HTML-Formular zum Hochladen einer Datei:</span><span class="sxs-lookup"><span data-stu-id="ae387-110">Here is an example of an HTML form for uploading a file:</span></span>

[!code-html[Main](sending-html-form-data-part-2/samples/sample1.html)]

![](sending-html-form-data-part-2/_static/image1.png)

<span data-ttu-id="ae387-111">Dieses Formular enthält ein Texteingabe-und ein Dateieingabe-Steuerelement.</span><span class="sxs-lookup"><span data-stu-id="ae387-111">This form contains a text input control and a file input control.</span></span> <span data-ttu-id="ae387-112">Wenn ein Formular ein Dateieingabe-Steuerelement enthält, sollte das **enctype** -Attribut immer &quot;mehrteiligen/Form-Data-&quot;sein, das angibt, dass das Formular als mehrteilige MIME-Nachricht gesendet wird.</span><span class="sxs-lookup"><span data-stu-id="ae387-112">When a form contains a file input control, the **enctype** attribute should always be &quot;multipart/form-data&quot;, which specifies that the form will be sent as a multipart MIME message.</span></span>

<span data-ttu-id="ae387-113">Das Format einer mehrteiligen MIME-Nachricht ist am einfachsten zu verstehen, indem Sie eine Beispiel Anforderung betrachten:</span><span class="sxs-lookup"><span data-stu-id="ae387-113">The format of a multipart MIME message is easiest to understand by looking at an example request:</span></span>

[!code-console[Main](sending-html-form-data-part-2/samples/sample2.cmd)]

<span data-ttu-id="ae387-114">Diese Meldung ist in zwei *Teile*unterteilt, eine für jedes Formular Steuerelement.</span><span class="sxs-lookup"><span data-stu-id="ae387-114">This message is divided into two *parts*, one for each form control.</span></span> <span data-ttu-id="ae387-115">Teile Grenzen werden durch die Zeilen angegeben, die mit Bindestrichen beginnen.</span><span class="sxs-lookup"><span data-stu-id="ae387-115">Part boundaries are indicated by the lines that start with dashes.</span></span>

> [!NOTE]
> <span data-ttu-id="ae387-116">Die teilgrenze schließt eine zufällige Komponente (&quot;41184676334&quot;) ein, um sicherzustellen, dass die Begrenzungs Zeichenfolge nicht versehentlich in einem Nachrichten Teil angezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="ae387-116">The part boundary includes a random component (&quot;41184676334&quot;) to ensure that the boundary string does not accidentally appear inside a message part.</span></span>

<span data-ttu-id="ae387-117">Jeder Nachrichten Teil enthält mindestens einen Header, gefolgt vom Teil Inhalt.</span><span class="sxs-lookup"><span data-stu-id="ae387-117">Each message part contains one or more headers, followed by the part contents.</span></span>

- <span data-ttu-id="ae387-118">Der Content-Disposition-Header enthält den Namen des Steuer Elements.</span><span class="sxs-lookup"><span data-stu-id="ae387-118">The Content-Disposition header includes the name of the control.</span></span> <span data-ttu-id="ae387-119">Für Dateien enthält Sie auch den Dateinamen.</span><span class="sxs-lookup"><span data-stu-id="ae387-119">For files, it also contains the file name.</span></span>
- <span data-ttu-id="ae387-120">Der Content-Type-Header beschreibt die Daten im-Teil.</span><span class="sxs-lookup"><span data-stu-id="ae387-120">The Content-Type header describes the data in the part.</span></span> <span data-ttu-id="ae387-121">Wenn dieser Header weggelassen wird, ist der Standardwert Text/Plain.</span><span class="sxs-lookup"><span data-stu-id="ae387-121">If this header is omitted, the default is text/plain.</span></span>

<span data-ttu-id="ae387-122">Im vorherigen Beispiel hat der Benutzer eine Datei mit dem Namen "grandcanyon. jpg" mit dem Inhaltstyp "Image/JPEG;" hochgeladen. und der Wert der Texteingabe war &quot;Sommerurlaub&quot;.</span><span class="sxs-lookup"><span data-stu-id="ae387-122">In the previous example, the user uploaded a file named GrandCanyon.jpg, with content type image/jpeg; and the value of the text input was &quot;Summer Vacation&quot;.</span></span>

## <a name="file-upload"></a><span data-ttu-id="ae387-123">Dateiupload</span><span class="sxs-lookup"><span data-stu-id="ae387-123">File Upload</span></span>

<span data-ttu-id="ae387-124">Betrachten wir nun einen Web-API-Controller, der Dateien aus einer mehrteiligen MIME-Nachricht liest.</span><span class="sxs-lookup"><span data-stu-id="ae387-124">Now let's look at a Web API controller that reads files from a multipart MIME message.</span></span> <span data-ttu-id="ae387-125">Der Controller liest die Dateien asynchron.</span><span class="sxs-lookup"><span data-stu-id="ae387-125">The controller will read the files asynchronously.</span></span> <span data-ttu-id="ae387-126">Die Web-API unterstützt asynchrone Aktionen mithilfe des [aufgabenbasierten Programmiermodells](https://msdn.microsoft.com/library/dd460693.aspx).</span><span class="sxs-lookup"><span data-stu-id="ae387-126">Web API supports asynchronous actions using the [task-based programming model](https://msdn.microsoft.com/library/dd460693.aspx).</span></span> <span data-ttu-id="ae387-127">Im folgenden finden Sie den Code, wenn Sie auf .NET Framework 4,5 abzielen, der die Schlüsselwörter **Async** und **erwartet** unterstützt.</span><span class="sxs-lookup"><span data-stu-id="ae387-127">First, here is the code if you are targeting .NET Framework 4.5, which supports the **async** and **await** keywords.</span></span>

[!code-csharp[Main](sending-html-form-data-part-2/samples/sample3.cs)]

<span data-ttu-id="ae387-128">Beachten Sie, dass die Controller Aktion keine Parameter annimmt.</span><span class="sxs-lookup"><span data-stu-id="ae387-128">Notice that the controller action does not take any parameters.</span></span> <span data-ttu-id="ae387-129">Der Grund hierfür ist, dass der Anforderungs Text innerhalb der Aktion verarbeitet wird, ohne dass ein Formatierer für den Medientyp aufgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="ae387-129">That's because we process the request body inside the action, without invoking a media-type formatter.</span></span>

<span data-ttu-id="ae387-130">Die **ismultipartcontent** -Methode überprüft, ob die Anforderung eine mehrteilige MIME-Nachricht enthält.</span><span class="sxs-lookup"><span data-stu-id="ae387-130">The **IsMultipartContent** method checks whether the request contains a multipart MIME message.</span></span> <span data-ttu-id="ae387-131">Andernfalls gibt der Controller den HTTP-Statuscode 415 (nicht unterstützter Medientyp) zurück.</span><span class="sxs-lookup"><span data-stu-id="ae387-131">If not, the controller returns HTTP status code 415 (Unsupported Media Type).</span></span>

<span data-ttu-id="ae387-132">Die **multipartformdatastreamprovider** -Klasse ist ein Hilfsobjekt, das Datei Datenströme für hochgeladene Dateien zuordnet.</span><span class="sxs-lookup"><span data-stu-id="ae387-132">The **MultipartFormDataStreamProvider** class is a helper object that allocates file streams for uploaded files.</span></span> <span data-ttu-id="ae387-133">Um die mehrteilige MIME-Nachricht zu lesen, müssen Sie die Methode " **leseasmultipartasync** " aufrufen.</span><span class="sxs-lookup"><span data-stu-id="ae387-133">To read the multipart MIME message, call the **ReadAsMultipartAsync** method.</span></span> <span data-ttu-id="ae387-134">Diese Methode extrahiert alle Nachrichten Teile und schreibt Sie in die Streams, die vom **multipartformdatastreamprovider**bereitgestellt werden.</span><span class="sxs-lookup"><span data-stu-id="ae387-134">This method extracts all of the message parts and writes them into the streams provided by the **MultipartFormDataStreamProvider**.</span></span>

<span data-ttu-id="ae387-135">Wenn die Methode abgeschlossen ist, können Sie Informationen zu den Dateien aus der **FileData** -Eigenschaft, die eine Sammlung von **multipartfiledata** -Objekten ist, erhalten.</span><span class="sxs-lookup"><span data-stu-id="ae387-135">When the method completes, you can get information about the files from the **FileData** property, which is a collection of **MultipartFileData** objects.</span></span>

- <span data-ttu-id="ae387-136">**Multipartfiledata. filename** ist der lokale Dateiname auf dem Server, auf dem die Datei gespeichert wurde.</span><span class="sxs-lookup"><span data-stu-id="ae387-136">**MultipartFileData.FileName** is the local file name on the server, where the file was saved.</span></span>
- <span data-ttu-id="ae387-137">**Multipartfiledata. Headers** enthält den Part-Header (*nicht* den-Anforderungs Header).</span><span class="sxs-lookup"><span data-stu-id="ae387-137">**MultipartFileData.Headers** contains the part header (*not* the request header).</span></span> <span data-ttu-id="ae387-138">Damit können Sie auf die Content-\_Disposition-und Content-Type-Header zugreifen.</span><span class="sxs-lookup"><span data-stu-id="ae387-138">You can use this to access the Content\_Disposition and Content-Type headers.</span></span>

<span data-ttu-id="ae387-139">Wie der Name schon sagt, handelt es sich bei "read **asmultipartasync** " um eine asynchrone Methode.</span><span class="sxs-lookup"><span data-stu-id="ae387-139">As the name suggests, **ReadAsMultipartAsync** is an asynchronous method.</span></span> <span data-ttu-id="ae387-140">Um nach Abschluss der-Methode Aufgaben auszuführen, verwenden Sie eine [Fortsetzungs Aufgabe](https://msdn.microsoft.com/library/ee372288.aspx) (.NET 4,0) oder das **erwartet** -Schlüsselwort (.NET 4,5).</span><span class="sxs-lookup"><span data-stu-id="ae387-140">To perform work after the method completes, use a [continuation task](https://msdn.microsoft.com/library/ee372288.aspx) (.NET 4.0) or the **await** keyword (.NET 4.5).</span></span>

<span data-ttu-id="ae387-141">Im folgenden finden Sie die .NET Framework 4,0-Version des vorherigen Codes:</span><span class="sxs-lookup"><span data-stu-id="ae387-141">Here is the .NET Framework 4.0 version of the previous code:</span></span>

[!code-csharp[Main](sending-html-form-data-part-2/samples/sample4.cs)]

## <a name="reading-form-control-data"></a><span data-ttu-id="ae387-142">Lesen von Formular Steuerungsdaten</span><span class="sxs-lookup"><span data-stu-id="ae387-142">Reading Form Control Data</span></span>

<span data-ttu-id="ae387-143">Das HTML-Formular, das ich zuvor gezeigt habe, hatte ein Texteingabe-Steuerelement.</span><span class="sxs-lookup"><span data-stu-id="ae387-143">The HTML form that I showed earlier had a text input control.</span></span>

[!code-html[Main](sending-html-form-data-part-2/samples/sample5.html)]

<span data-ttu-id="ae387-144">Sie können den Wert des Steuer Elements aus der **formdata** -Eigenschaft von **multipartformdatastreamprovider**erhalten.</span><span class="sxs-lookup"><span data-stu-id="ae387-144">You can get the value of the control from the **FormData** property of the **MultipartFormDataStreamProvider**.</span></span>

[!code-csharp[Main](sending-html-form-data-part-2/samples/sample6.cs?highlight=15)]

<span data-ttu-id="ae387-145">**Formdata** ist eine **NameValueCollection** , die Name-Wert-Paare für die Formular Steuerelemente enthält.</span><span class="sxs-lookup"><span data-stu-id="ae387-145">**FormData** is a **NameValueCollection** that contains name/value pairs for the form controls.</span></span> <span data-ttu-id="ae387-146">Die Sammlung kann doppelte Schlüssel enthalten.</span><span class="sxs-lookup"><span data-stu-id="ae387-146">The collection can contain duplicate keys.</span></span> <span data-ttu-id="ae387-147">Beachten Sie dieses Formular:</span><span class="sxs-lookup"><span data-stu-id="ae387-147">Consider this form:</span></span>

[!code-html[Main](sending-html-form-data-part-2/samples/sample7.html)]

![](sending-html-form-data-part-2/_static/image2.png)

<span data-ttu-id="ae387-148">Der Anforderungs Text könnte wie folgt aussehen:</span><span class="sxs-lookup"><span data-stu-id="ae387-148">The request body might look like this:</span></span>

[!code-console[Main](sending-html-form-data-part-2/samples/sample8.cmd)]

<span data-ttu-id="ae387-149">In diesem Fall enthält die **formdata** -Sammlung die folgenden Schlüssel-Wert-Paare:</span><span class="sxs-lookup"><span data-stu-id="ae387-149">In that case, the **FormData** collection would contain the following key/value pairs:</span></span>

- <span data-ttu-id="ae387-150">Fahrt: Roundtrip</span><span class="sxs-lookup"><span data-stu-id="ae387-150">trip: round-trip</span></span>
- <span data-ttu-id="ae387-151">Optionen: nonstop</span><span class="sxs-lookup"><span data-stu-id="ae387-151">options: nonstop</span></span>
- <span data-ttu-id="ae387-152">Optionen: Datumsangaben</span><span class="sxs-lookup"><span data-stu-id="ae387-152">options: dates</span></span>
- <span data-ttu-id="ae387-153">Arbeitsplatz: Fenster</span><span class="sxs-lookup"><span data-stu-id="ae387-153">seat: window</span></span>
