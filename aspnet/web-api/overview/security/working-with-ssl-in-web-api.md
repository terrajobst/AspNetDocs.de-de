---
uid: web-api/overview/security/working-with-ssl-in-web-api
title: Arbeiten mit SSL in der Web-API | Microsoft-Dokumentation
author: MikeWasson
description: Zeigt die Verwendung von SSL mit ASP.net-Web-API, einschließlich der Verwendung von SSL-Client Zertifikaten.
ms.author: riande
ms.date: 02/22/2019
ms.assetid: 97f6164f-59cf-45c0-b820-e4aa29b45396
msc.legacyurl: /web-api/overview/security/working-with-ssl-in-web-api
msc.type: authoredcontent
ms.openlocfilehash: 31589b3713b1f1a9b98d12906bfef81f8bf5e3f9
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78484413"
---
# <a name="working-with-ssl-in-web-api"></a><span data-ttu-id="e9643-103">Arbeiten mit SSL in der Web-API</span><span class="sxs-lookup"><span data-stu-id="e9643-103">Working with SSL in Web API</span></span>

<span data-ttu-id="e9643-104">von [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="e9643-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="e9643-105">Mehrere allgemeine Authentifizierungs Schemas sind über einfaches http nicht sicher.</span><span class="sxs-lookup"><span data-stu-id="e9643-105">Several common authentication schemes are not secure over plain HTTP.</span></span> <span data-ttu-id="e9643-106">Insbesondere senden die einfache Authentifizierung und Formularauthentifizierung unverschlüsselte Anmeldeinformationen.</span><span class="sxs-lookup"><span data-stu-id="e9643-106">In particular, Basic authentication and forms authentication send unencrypted credentials.</span></span> <span data-ttu-id="e9643-107">Um sicher zu sein, *müssen* diese Authentifizierungs Schemas SSL verwenden.</span><span class="sxs-lookup"><span data-stu-id="e9643-107">To be secure, these authentication schemes *must* use SSL.</span></span> <span data-ttu-id="e9643-108">Außerdem können SSL-Client Zertifikate verwendet werden, um Clients zu authentifizieren.</span><span class="sxs-lookup"><span data-stu-id="e9643-108">In addition, SSL client certificates can be used to authenticate clients.</span></span>

## <a name="enabling-ssl-on-the-server"></a><span data-ttu-id="e9643-109">Aktivieren von SSL auf dem Server</span><span class="sxs-lookup"><span data-stu-id="e9643-109">Enabling SSL on the Server</span></span>

<span data-ttu-id="e9643-110">Einrichten von SSL in IIS 7 oder höher:</span><span class="sxs-lookup"><span data-stu-id="e9643-110">To set up SSL in IIS 7 or later:</span></span>

- <span data-ttu-id="e9643-111">Erstellen oder rufen Sie ein Zertifikat ab.</span><span class="sxs-lookup"><span data-stu-id="e9643-111">Create or get a certificate.</span></span> <span data-ttu-id="e9643-112">Zum Testen können Sie ein selbst signiertes Zertifikat erstellen.</span><span class="sxs-lookup"><span data-stu-id="e9643-112">For testing, you can create a self-signed certificate.</span></span>
- <span data-ttu-id="e9643-113">Fügen Sie eine HTTPS-Bindung hinzu.</span><span class="sxs-lookup"><span data-stu-id="e9643-113">Add an HTTPS binding.</span></span>

<span data-ttu-id="e9643-114">Weitere Informationen finden [Sie unter Einrichten von SSL auf IIS 7](https://www.iis.net/learn/manage/configuring-security/how-to-set-up-ssl-on-iis).</span><span class="sxs-lookup"><span data-stu-id="e9643-114">For details, see [How to Set Up SSL on IIS 7](https://www.iis.net/learn/manage/configuring-security/how-to-set-up-ssl-on-iis).</span></span>

<span data-ttu-id="e9643-115">Für lokale Tests können Sie SSL in IIS Express aus Visual Studio aktivieren.</span><span class="sxs-lookup"><span data-stu-id="e9643-115">For local testing, you can enable SSL in IIS Express from Visual Studio.</span></span> <span data-ttu-id="e9643-116">Legen Sie in der Eigenschaftenfenster **SSL aktiviert** auf **true**fest.</span><span class="sxs-lookup"><span data-stu-id="e9643-116">In the Properties window, set **SSL Enabled** to **True**.</span></span> <span data-ttu-id="e9643-117">Notieren Sie sich den Wert der **SSL-URL**. Verwenden Sie diese URL zum Testen von HTTPS-Verbindungen.</span><span class="sxs-lookup"><span data-stu-id="e9643-117">Note the value of **SSL URL**; use this URL for testing HTTPS connections.</span></span>

![](working-with-ssl-in-web-api/_static/image1.png)

### <a name="enforcing-ssl-in-a-web-api-controller"></a><span data-ttu-id="e9643-118">Erzwingen von SSL in einem Web-API-Controller</span><span class="sxs-lookup"><span data-stu-id="e9643-118">Enforcing SSL in a Web API Controller</span></span>

<span data-ttu-id="e9643-119">Wenn Sie sowohl über eine HTTPS-als auch über eine HTTP-Bindung verfügen, können Clients weiterhin http für den Zugriff auf die Website verwenden.</span><span class="sxs-lookup"><span data-stu-id="e9643-119">If you have both an HTTPS and an HTTP binding, clients can still use HTTP to access the site.</span></span> <span data-ttu-id="e9643-120">Möglicherweise können einige Ressourcen über HTTP verfügbar sein, während für andere Ressourcen SSL erforderlich ist.</span><span class="sxs-lookup"><span data-stu-id="e9643-120">You might allow some resources to be available through HTTP, while other resources require SSL.</span></span> <span data-ttu-id="e9643-121">Verwenden Sie in diesem Fall einen Aktionsfilter, um SSL für die geschützten Ressourcen anzufordern.</span><span class="sxs-lookup"><span data-stu-id="e9643-121">In that case, use an action filter to require SSL for the protected resources.</span></span> <span data-ttu-id="e9643-122">Der folgende Code zeigt einen Web-API-Authentifizierungsfilter mit Überprüfung auf SSL:</span><span class="sxs-lookup"><span data-stu-id="e9643-122">The following code shows a Web API authentication filter that checks for SSL:</span></span>

[!code-csharp[Main](working-with-ssl-in-web-api/samples/sample1.cs)]

<span data-ttu-id="e9643-123">Fügen Sie diesen Filter allen Web-API-Aktionen hinzu, die SSL erfordern:</span><span class="sxs-lookup"><span data-stu-id="e9643-123">Add this filter to any Web API actions that require SSL:</span></span>

[!code-csharp[Main](working-with-ssl-in-web-api/samples/sample2.cs)]

## <a name="ssl-client-certificates"></a><span data-ttu-id="e9643-124">SSL-Client Zertifikate</span><span class="sxs-lookup"><span data-stu-id="e9643-124">SSL Client Certificates</span></span>

<span data-ttu-id="e9643-125">SSL ermöglicht die Authentifizierung mithilfe von Public Key-Infrastruktur Zertifikaten.</span><span class="sxs-lookup"><span data-stu-id="e9643-125">SSL provides authentication by using Public Key Infrastructure certificates.</span></span> <span data-ttu-id="e9643-126">Der Server muss ein Zertifikat bereitstellen, mit dem der Server beim Client authentifiziert wird.</span><span class="sxs-lookup"><span data-stu-id="e9643-126">The server must provide a certificate that authenticates the server to the client.</span></span> <span data-ttu-id="e9643-127">Es ist weniger üblich, dass der Client ein Zertifikat für den Server bereitstellt, aber dies ist eine Option zum Authentifizieren von Clients.</span><span class="sxs-lookup"><span data-stu-id="e9643-127">It is less common for the client to provide a certificate to the server, but this is one option for authenticating clients.</span></span> <span data-ttu-id="e9643-128">Zum Verwenden von Client Zertifikaten mit SSL benötigen Sie eine Möglichkeit, signierte Zertifikate an Ihre Benutzer zu verteilen.</span><span class="sxs-lookup"><span data-stu-id="e9643-128">To use client certificates with SSL, you need a way to distribute signed certificates to your users.</span></span> <span data-ttu-id="e9643-129">Bei vielen Anwendungs Typen ist dies keine gute Benutzerumgebung, aber in einigen Umgebungen (z. b. Enterprise) ist dies möglicherweise möglich.</span><span class="sxs-lookup"><span data-stu-id="e9643-129">For many application types, this will not be a good user experience, but in some environments (for example, enterprise) it may be feasible.</span></span>

| <span data-ttu-id="e9643-130">Vorteile</span><span class="sxs-lookup"><span data-stu-id="e9643-130">Advantages</span></span> | <span data-ttu-id="e9643-131">Nachteile</span><span class="sxs-lookup"><span data-stu-id="e9643-131">Disadvantages</span></span> |
| --- | --- |
| <span data-ttu-id="e9643-132">-Die Zertifikat Anmelde Informationen sind stärker als Benutzername/Kennwort.</span><span class="sxs-lookup"><span data-stu-id="e9643-132">- Certificate credentials are stronger than username/password.</span></span> <span data-ttu-id="e9643-133">-SSL bietet einen umfassenden sicheren Kanal mit Authentifizierung, Nachrichten Integrität und Nachrichten Verschlüsselung.</span><span class="sxs-lookup"><span data-stu-id="e9643-133">- SSL provides a complete secure channel, with authentication, message integrity, and message encryption.</span></span> | <span data-ttu-id="e9643-134">-Sie müssen PKI-Zertifikate abrufen und verwalten.</span><span class="sxs-lookup"><span data-stu-id="e9643-134">- You must obtain and manage PKI certificates.</span></span> <span data-ttu-id="e9643-135">-Die Client Plattform muss SSL-Client Zertifikate unterstützen.</span><span class="sxs-lookup"><span data-stu-id="e9643-135">- The client platform must support SSL client certificates.</span></span> |

<span data-ttu-id="e9643-136">Zum Konfigurieren von IIS für die Annahme von Client Zertifikaten öffnen Sie IIS-Manager, und führen Sie die folgenden Schritte aus:</span><span class="sxs-lookup"><span data-stu-id="e9643-136">To configure IIS to accept client certificates, open IIS Manager and perform the following steps:</span></span>

1. <span data-ttu-id="e9643-137">Klicken Sie in der Strukturansicht auf den Knoten Standort.</span><span class="sxs-lookup"><span data-stu-id="e9643-137">Click the site node in the tree view.</span></span>
2. <span data-ttu-id="e9643-138">Doppelklicken Sie im mittleren Bereich auf das Feature **SSL-Einstellungen** .</span><span class="sxs-lookup"><span data-stu-id="e9643-138">Double-click the **SSL Settings** feature in the middle pane.</span></span>
3. <span data-ttu-id="e9643-139">Wählen Sie unter **Client Zertifikate**eine der folgenden Optionen aus:</span><span class="sxs-lookup"><span data-stu-id="e9643-139">Under **Client Certificates**, select one of these options:</span></span> 

    - <span data-ttu-id="e9643-140">**Akzeptieren**: IIS akzeptiert ein Zertifikat vom Client, erfordert aber kein Zertifikat.</span><span class="sxs-lookup"><span data-stu-id="e9643-140">**Accept**: IIS will accept a certificate from the client, but does not require one.</span></span>
    - <span data-ttu-id="e9643-141">**Erforderlich**: ein Client Zertifikat erforderlich.</span><span class="sxs-lookup"><span data-stu-id="e9643-141">**Require**: Require a client certificate.</span></span> <span data-ttu-id="e9643-142">(Um diese Option zu aktivieren, müssen Sie auch "SSL erforderlich" auswählen).</span><span class="sxs-lookup"><span data-stu-id="e9643-142">(To enable this option, you must also select "Require SSL")</span></span>

<span data-ttu-id="e9643-143">Sie können diese Optionen auch in der Datei "applicationHost. config" festlegen:</span><span class="sxs-lookup"><span data-stu-id="e9643-143">You can also set these options in the ApplicationHost.config file:</span></span>

[!code-xml[Main](working-with-ssl-in-web-api/samples/sample3.xml)]

<span data-ttu-id="e9643-144">Das Flag " **sslaushandatecert** " bedeutet, dass IIS ein Zertifikat vom Client annimmt, aber keinen erfordert (entspricht der Option "Accept" im IIS-Manager).</span><span class="sxs-lookup"><span data-stu-id="e9643-144">The **SslNegotiateCert** flag means IIS will accept a certificate from the client, but does not require one (equivalent to the "Accept" option in IIS Manager).</span></span> <span data-ttu-id="e9643-145">Wenn Sie ein Zertifikat anfordern möchten, legen Sie das **SSLRequireCert** -Flag fest.</span><span class="sxs-lookup"><span data-stu-id="e9643-145">To require a certificate, set the **SslRequireCert** flag.</span></span> <span data-ttu-id="e9643-146">Zum Testen können Sie diese Optionen auch in IIS Express auf dem lokalen ApplicationHost festlegen. Config-Datei, die sich unter "documents\iisexpress\config" befindet.</span><span class="sxs-lookup"><span data-stu-id="e9643-146">For testing, you can also set these options in IIS Express, in the local applicationhost.Config file, located in "Documents\IISExpress\config".</span></span>

### <a name="creating-a-client-certificate-for-testing"></a><span data-ttu-id="e9643-147">Erstellen eines Client Zertifikats zum Testen</span><span class="sxs-lookup"><span data-stu-id="e9643-147">Creating a Client Certificate for Testing</span></span>

<span data-ttu-id="e9643-148">Zu Testzwecken können Sie [Makecert. exe](/windows/desktop/SecCrypto/makecert) verwenden, um ein Client Zertifikat zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="e9643-148">For testing purposes, you can use [MakeCert.exe](/windows/desktop/SecCrypto/makecert) to create a client certificate.</span></span> <span data-ttu-id="e9643-149">Erstellen Sie zunächst eine Test Stamm Zertifizierungsstelle:</span><span class="sxs-lookup"><span data-stu-id="e9643-149">First, create a test root authority:</span></span>

[!code-console[Main](working-with-ssl-in-web-api/samples/sample4.cmd)]

<span data-ttu-id="e9643-150">Mit Makecert werden Sie aufgefordert, ein Kennwort für den privaten Schlüssel einzugeben.</span><span class="sxs-lookup"><span data-stu-id="e9643-150">Makecert will prompt you to enter a password for the private key.</span></span>

<span data-ttu-id="e9643-151">Fügen Sie als nächstes das Zertifikat dem Speicher "Vertrauenswürdige Stamm Zertifizierungsstellen" des Test Servers wie folgt hinzu:</span><span class="sxs-lookup"><span data-stu-id="e9643-151">Next, add the certificate to the test server's "Trusted Root Certification Authorities" store, as follows:</span></span>

1. <span data-ttu-id="e9643-152">Öffnen Sie MMC.</span><span class="sxs-lookup"><span data-stu-id="e9643-152">Open MMC.</span></span>
2. <span data-ttu-id="e9643-153">Wählen Sie unter **Datei**die Option **Snap-in hinzufügen/entfernen**aus.</span><span class="sxs-lookup"><span data-stu-id="e9643-153">Under **File**, select **Add/Remove Snap-In**.</span></span>
3. <span data-ttu-id="e9643-154">Wählen Sie **Computer Konto**aus.</span><span class="sxs-lookup"><span data-stu-id="e9643-154">Select **Computer Account**.</span></span>
4. <span data-ttu-id="e9643-155">Wählen Sie **lokaler Computer** aus, und beenden Sie den Assistenten.</span><span class="sxs-lookup"><span data-stu-id="e9643-155">Select **Local computer** and complete the wizard.</span></span>
5. <span data-ttu-id="e9643-156">Erweitern Sie im Navigationsbereich den Knoten "Vertrauenswürdige Stamm Zertifizierungsstellen".</span><span class="sxs-lookup"><span data-stu-id="e9643-156">Under the navigation pane, expand the "Trusted Root Certification Authorities" node.</span></span>
6. <span data-ttu-id="e9643-157">Zeigen Sie im Menü **Aktion** auf **alle Aufgaben**, und klicken Sie dann auf **importieren** , um den Zertifikat Import-Assistenten zu starten.</span><span class="sxs-lookup"><span data-stu-id="e9643-157">On the **Action** menu, point to **All Tasks**, and then click **Import** to start the Certificate Import Wizard.</span></span>
7. <span data-ttu-id="e9643-158">Navigieren Sie zur Zertifikatsdatei TempCA. cer.</span><span class="sxs-lookup"><span data-stu-id="e9643-158">Browse to the certificate file, TempCA.cer.</span></span>
8. <span data-ttu-id="e9643-159">Klicken Sie auf **Öffnen**und dann auf **weiter** , und schließen Sie den Assistenten ab.</span><span class="sxs-lookup"><span data-stu-id="e9643-159">Click **Open**, then click **Next** and complete the wizard.</span></span> <span data-ttu-id="e9643-160">(Sie werden aufgefordert, das Kennwort erneut einzugeben.)</span><span class="sxs-lookup"><span data-stu-id="e9643-160">(You will be prompted to re-enter the password.)</span></span>

<span data-ttu-id="e9643-161">Erstellen Sie jetzt ein Client Zertifikat, das vom ersten Zertifikat signiert wurde:</span><span class="sxs-lookup"><span data-stu-id="e9643-161">Now create a client certificate that is signed by the first certificate:</span></span>

[!code-console[Main](working-with-ssl-in-web-api/samples/sample5.cmd)]

### <a name="using-client-certificates-in-web-api"></a><span data-ttu-id="e9643-162">Verwenden von Client Zertifikaten in der Web-API</span><span class="sxs-lookup"><span data-stu-id="e9643-162">Using Client Certificates in Web API</span></span>

<span data-ttu-id="e9643-163">Auf der Serverseite können Sie das Client Zertifikat abrufen, indem Sie [getclientcertificate](https://msdn.microsoft.com/library/system.net.http.httprequestmessageextensions.getclientcertificate.aspx) für die Anforderungs Nachricht aufrufen.</span><span class="sxs-lookup"><span data-stu-id="e9643-163">On the server side, you can get the client certificate by calling [GetClientCertificate](https://msdn.microsoft.com/library/system.net.http.httprequestmessageextensions.getclientcertificate.aspx) on the request message.</span></span> <span data-ttu-id="e9643-164">Die-Methode gibt NULL zurück, wenn kein Client Zertifikat vorhanden ist.</span><span class="sxs-lookup"><span data-stu-id="e9643-164">The method returns null if there is no client certificate.</span></span> <span data-ttu-id="e9643-165">Andernfalls wird eine **X509Certificate2** -Instanz zurückgegeben.</span><span class="sxs-lookup"><span data-stu-id="e9643-165">Otherwise, it returns an **X509Certificate2** instance.</span></span> <span data-ttu-id="e9643-166">Verwenden Sie dieses Objekt, um Informationen aus dem Zertifikat zu erhalten, z. b. den Aussteller und den Betreff.</span><span class="sxs-lookup"><span data-stu-id="e9643-166">Use this object to get information from the certificate, such as the issuer and subject.</span></span> <span data-ttu-id="e9643-167">Anschließend können Sie diese Informationen für die Authentifizierung und/oder Autorisierung verwenden.</span><span class="sxs-lookup"><span data-stu-id="e9643-167">Then you can use this information for authentication and/or authorization.</span></span>

[!code-csharp[Main](working-with-ssl-in-web-api/samples/sample6.cs)]
