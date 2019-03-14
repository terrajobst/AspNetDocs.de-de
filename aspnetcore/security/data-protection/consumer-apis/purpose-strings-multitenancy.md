---
title: Zweckhierarchie und mehrinstanzenfähigkeit in ASP.NET Core
author: rick-anderson
description: Lernen Sie Zeichenfolge zweckhierarchie und mehrinstanzenfähigkeit in Bezug auf ASP.NET Core Datenschutz-APIs.
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/consumer-apis/purpose-strings-multitenancy
ms.openlocfilehash: 1133d40e7b325d58b3f70e7387494dae36ff8ac9
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/01/2019
ms.locfileid: "57047007"
---
# <a name="purpose-hierarchy-and-multi-tenancy-in-aspnet-core"></a><span data-ttu-id="0c576-103">Zweckhierarchie und mehrinstanzenfähigkeit in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0c576-103">Purpose hierarchy and multi-tenancy in ASP.NET Core</span></span>

<span data-ttu-id="0c576-104">Da ein `IDataProtector` ist auch implizit eine `IDataProtectionProvider`, Zwecke können miteinander verkettet werden.</span><span class="sxs-lookup"><span data-stu-id="0c576-104">Since an `IDataProtector` is also implicitly an `IDataProtectionProvider`, purposes can be chained together.</span></span> <span data-ttu-id="0c576-105">In diesem Sinn `provider.CreateProtector([ "purpose1", "purpose2" ])` entspricht `provider.CreateProtector("purpose1").CreateProtector("purpose2")`.</span><span class="sxs-lookup"><span data-stu-id="0c576-105">In this sense, `provider.CreateProtector([ "purpose1", "purpose2" ])` is equivalent to `provider.CreateProtector("purpose1").CreateProtector("purpose2")`.</span></span>

<span data-ttu-id="0c576-106">Dies ermöglicht einige interessante hierarchischen Beziehungen mithilfe von System zum Schutz von Daten.</span><span class="sxs-lookup"><span data-stu-id="0c576-106">This allows for some interesting hierarchical relationships through the data protection system.</span></span> <span data-ttu-id="0c576-107">Im früheren Beispiel [Contoso.Messaging.SecureMessage](xref:security/data-protection/consumer-apis/purpose-strings#data-protection-contoso-purpose), kann die Komponente SecureMessage Aufrufen `provider.CreateProtector("Contoso.Messaging.SecureMessage")` einmal Investitionen und Zwischenspeicherung des Ergebnisses in einer privaten `_myProvider` Feld.</span><span class="sxs-lookup"><span data-stu-id="0c576-107">In the earlier example of [Contoso.Messaging.SecureMessage](xref:security/data-protection/consumer-apis/purpose-strings#data-protection-contoso-purpose), the SecureMessage component can call `provider.CreateProtector("Contoso.Messaging.SecureMessage")` once up-front and cache the result into a private `_myProvider` field.</span></span> <span data-ttu-id="0c576-108">Zukünftige Schutzvorrichtungen können dann über Aufrufe von erstellt werden `_myProvider.CreateProtector("User: username")`, und diese Schutzvorrichtungen für die einzelnen Nachrichten sichern, eingesetzt werden.</span><span class="sxs-lookup"><span data-stu-id="0c576-108">Future protectors can then be created via calls to `_myProvider.CreateProtector("User: username")`, and these protectors would be used for securing the individual messages.</span></span>

<span data-ttu-id="0c576-109">Dies kann auch umgekehrt werden.</span><span class="sxs-lookup"><span data-stu-id="0c576-109">This can also be flipped.</span></span> <span data-ttu-id="0c576-110">Betrachten Sie eine einzelne logische welche Hosts, die mehrere Mandanten (ein CMS scheint vernünftig), und jeder Mandant mit seinem eigenen Authentifizierungs- und Status Management-System konfiguriert werden können.</span><span class="sxs-lookup"><span data-stu-id="0c576-110">Consider a single logical application which hosts multiple tenants (a CMS seems reasonable), and each tenant can be configured with its own authentication and state management system.</span></span> <span data-ttu-id="0c576-111">Die Kategorie-Anwendung verfügt über einen einzelnen master-Anbieter, und er ruft `provider.CreateProtector("Tenant 1")` und `provider.CreateProtector("Tenant 2")` jeder Mandant einen eigenen isolierten Segment System zum Schutz von Daten gewähren.</span><span class="sxs-lookup"><span data-stu-id="0c576-111">The umbrella application has a single master provider, and it calls `provider.CreateProtector("Tenant 1")` and `provider.CreateProtector("Tenant 2")` to give each tenant its own isolated slice of the data protection system.</span></span> <span data-ttu-id="0c576-112">Die Mandanten können leiten Sie ihre eigenen einzelnen Schutzvorrichtungen auf Grundlage seiner eigenen Anforderungen, aber unabhängig davon, wie sehr sie versuchen können nicht diese Schutzvorrichtungen, die in Konflikt stehen mit keinem anderen Mandanten erstellen, im System.</span><span class="sxs-lookup"><span data-stu-id="0c576-112">The tenants could then derive their own individual protectors based on their own needs, but no matter how hard they try they cannot create protectors which collide with any other tenant in the system.</span></span> <span data-ttu-id="0c576-113">Dies wird grafisch dargestellt, wie im folgenden dargestellt.</span><span class="sxs-lookup"><span data-stu-id="0c576-113">Graphically, this is represented as below.</span></span>

![Mit mehreren Mandanten zu](purpose-strings-multitenancy/_static/purposes-multi-tenancy.png)

>[!WARNING]
> <span data-ttu-id="0c576-115">Dies setzt voraus die Schirm anwendungssteuerung, welche APIs für einzelne Mandanten verfügbar sind und dass es sich bei Mandanten beliebigen Code auf dem Server nicht ausgeführt werden können.</span><span class="sxs-lookup"><span data-stu-id="0c576-115">This assumes the umbrella application controls which APIs are available to individual tenants and that tenants cannot execute arbitrary code on the server.</span></span> <span data-ttu-id="0c576-116">Verlangen diese Schritte aus, wenn ein Mandant kann willkürlichen Code ausführen, sie private Reflektion, um die Isolation Garantien zu durchbrechen ausführen oder sie einfach das Schlüsselmaterial direkt gelesen und leiten Sie beliebige Unterschlüssel konnte können.</span><span class="sxs-lookup"><span data-stu-id="0c576-116">If a tenant can execute arbitrary code, they could perform private reflection to break the isolation guarantees, or they could just read the master keying material directly and derive whatever subkeys they desire.</span></span>

<span data-ttu-id="0c576-117">Das System zum Schutz von Daten verwendet eine Sortierung der mehrinstanzenfähigkeit in der Standardkonfiguration für die Out-of-the-Box.</span><span class="sxs-lookup"><span data-stu-id="0c576-117">The data protection system actually uses a sort of multi-tenancy in its default out-of-the-box configuration.</span></span> <span data-ttu-id="0c576-118">Standardmäßig wird die Schlüsselmaterial in das Workerprozesskonto Benutzerprofilordner (oder der Registrierung für die IIS-Anwendungspoolidentitäten) gespeichert.</span><span class="sxs-lookup"><span data-stu-id="0c576-118">By default master keying material is stored in the worker process account's user profile folder (or the registry, for IIS application pool identities).</span></span> <span data-ttu-id="0c576-119">Aber es ist tatsächlich ziemlich üblich, ein einzelnes Konto verwenden, um mehrere Anwendungen auszuführen, und daher alle diese Anwendungen würden am Ende den Master Schlüsselmaterial freigeben.</span><span class="sxs-lookup"><span data-stu-id="0c576-119">But it's actually fairly common to use a single account to run multiple applications, and thus all these applications would end up sharing the master keying material.</span></span> <span data-ttu-id="0c576-120">Um dieses Problem zu lösen, fügt System zum Schutz von Daten automatisch einen Bezeichner der eindeutigen pro Anwendung als erstes Element in der Kette für allgemeine Zwecke.</span><span class="sxs-lookup"><span data-stu-id="0c576-120">To solve this, the data protection system automatically inserts a unique-per-application identifier as the first element in the overall purpose chain.</span></span> <span data-ttu-id="0c576-121">Implizite dazu dient, [einzelne Anwendungen isolieren](xref:security/data-protection/configuration/overview#per-application-isolation) voneinander, indem Sie jede Anwendung effektiv zu behandeln, wie ein eindeutige Mandanten innerhalb des Systems und der Prozess der Schutzvorrichtung identisch mit der obigen Abbildung dargestellt wird.</span><span class="sxs-lookup"><span data-stu-id="0c576-121">This implicit purpose serves to [isolate individual applications](xref:security/data-protection/configuration/overview#per-application-isolation) from one another by effectively treating each application as a unique tenant within the system, and the protector creation process looks identical to the image above.</span></span>