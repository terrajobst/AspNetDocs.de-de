---
title: Kryptografieerweiterbarkeit in Core in ASP.NET Core
author: rick-anderson
description: Informationen Sie zu IAuthenticatedEncryptor, IAuthenticatedEncryptorDescriptor, IAuthenticatedEncryptorDescriptorDeserializer und der obersten Ebene Factory.
ms.author: riande
ms.date: 8/11/2017
uid: security/data-protection/extensibility/core-crypto
ms.openlocfilehash: 47432cfefe0a52c9f815d717f7269ec68fdb6af3
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/01/2019
ms.locfileid: "57036177"
---
# <a name="core-cryptography-extensibility-in-aspnet-core"></a><span data-ttu-id="f7c3f-103">Kryptografieerweiterbarkeit in Core in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f7c3f-103">Core cryptography extensibility in ASP.NET Core</span></span>

<a name="data-protection-extensibility-core-crypto"></a>

>[!WARNING]
> <span data-ttu-id="f7c3f-104">Typen, die die folgenden Schnittstellen implementieren, sollten threadsicher werden für mehrere Aufrufer.</span><span class="sxs-lookup"><span data-stu-id="f7c3f-104">Types that implement any of the following interfaces should be thread-safe for multiple callers.</span></span>

<a name="data-protection-extensibility-core-crypto-iauthenticatedencryptor"></a>

## <a name="iauthenticatedencryptor"></a><span data-ttu-id="f7c3f-105">IAuthenticatedEncryptor</span><span class="sxs-lookup"><span data-stu-id="f7c3f-105">IAuthenticatedEncryptor</span></span>

<span data-ttu-id="f7c3f-106">Die **IAuthenticatedEncryptor** Schnittstelle ist der grundlegende Baustein des kryptografischen Subsystems.</span><span class="sxs-lookup"><span data-stu-id="f7c3f-106">The **IAuthenticatedEncryptor** interface is the basic building block of the cryptographic subsystem.</span></span> <span data-ttu-id="f7c3f-107">In der Regel eine IAuthenticatedEncryptor pro Schlüssel vorhanden ist, und die IAuthenticatedEncryptor Instanz dient als Wrapper für alle kryptografische Schlüsseldaten und algorithmische Informationen, die zum Ausführen kryptografischer Vorgänge erforderlich sind.</span><span class="sxs-lookup"><span data-stu-id="f7c3f-107">There's generally one IAuthenticatedEncryptor per key, and the IAuthenticatedEncryptor instance wraps all cryptographic key material and algorithmic information necessary to perform cryptographic operations.</span></span>

<span data-ttu-id="f7c3f-108">Wie der Name schon sagt, ist der Typ für die Bereitstellung des authentifizierten Ver- und Entschlüsselung Dienste verantwortlich.</span><span class="sxs-lookup"><span data-stu-id="f7c3f-108">As its name suggests, the type is responsible for providing authenticated encryption and decryption services.</span></span> <span data-ttu-id="f7c3f-109">Er macht die folgenden zwei APIs verfügbar.</span><span class="sxs-lookup"><span data-stu-id="f7c3f-109">It exposes the following two APIs.</span></span>

* <span data-ttu-id="f7c3f-110">Decrypt(ArraySegment<byte> ciphertext, ArraySegment<byte> additionalAuthenticatedData) : byte[]</span><span class="sxs-lookup"><span data-stu-id="f7c3f-110">Decrypt(ArraySegment<byte> ciphertext, ArraySegment<byte> additionalAuthenticatedData) : byte[]</span></span>

* <span data-ttu-id="f7c3f-111">Encrypt(ArraySegment<byte> plaintext, ArraySegment<byte> additionalAuthenticatedData) : byte[]</span><span class="sxs-lookup"><span data-stu-id="f7c3f-111">Encrypt(ArraySegment<byte> plaintext, ArraySegment<byte> additionalAuthenticatedData) : byte[]</span></span>

<span data-ttu-id="f7c3f-112">Die Encrypt-Methode gibt ein Blob, das dazu nur-Text und ein authentifizierungstag enthält.</span><span class="sxs-lookup"><span data-stu-id="f7c3f-112">The Encrypt method returns a blob that includes the enciphered plaintext and an authentication tag.</span></span> <span data-ttu-id="f7c3f-113">Das authentifizierungstag muss die zusätzlichen authentifizierten Daten (AAD) umfassen, obwohl die AAD selbst nicht aus der letzten Nutzlast wiederhergestellt werden muss.</span><span class="sxs-lookup"><span data-stu-id="f7c3f-113">The authentication tag must encompass the additional authenticated data (AAD), though the AAD itself need not be recoverable from the final payload.</span></span> <span data-ttu-id="f7c3f-114">Die Decrypt-Methode überprüft das authentifizierungstag und die deciphered Nutzlast zurückgegeben.</span><span class="sxs-lookup"><span data-stu-id="f7c3f-114">The Decrypt method validates the authentication tag and returns the deciphered payload.</span></span> <span data-ttu-id="f7c3f-115">Alle Fehler auf (mit Ausnahme von "ArgumentNullException" und ähnlich) sollte auf CryptographicException antibiotischem sein.</span><span class="sxs-lookup"><span data-stu-id="f7c3f-115">All failures (except ArgumentNullException and similar) should be homogenized to CryptographicException.</span></span>

> [!NOTE]
> <span data-ttu-id="f7c3f-116">Die IAuthenticatedEncryptor-Instanz selbst gar nicht wirklich das Schlüsselmaterial enthält.</span><span class="sxs-lookup"><span data-stu-id="f7c3f-116">The IAuthenticatedEncryptor instance itself doesn't actually need to contain the key material.</span></span> <span data-ttu-id="f7c3f-117">Die Implementierung könnte z. B. in ein HSM für alle Vorgänge delegieren.</span><span class="sxs-lookup"><span data-stu-id="f7c3f-117">For example, the implementation could delegate to an HSM for all operations.</span></span>

<a name="data-protection-extensibility-core-crypto-iauthenticatedencryptorfactory"></a>
<a name="data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptor"></a>

## <a name="how-to-create-an-iauthenticatedencryptor"></a><span data-ttu-id="f7c3f-118">Vorgehensweise: Erstellen einer IAuthenticatedEncryptor</span><span class="sxs-lookup"><span data-stu-id="f7c3f-118">How to create an IAuthenticatedEncryptor</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="f7c3f-119">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="f7c3f-119">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="f7c3f-120">Die **IAuthenticatedEncryptorFactory** Schnittstelle darstellt, einen Typ, der weiß, wie Sie erstellen eine [IAuthenticatedEncryptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptor) Instanz.</span><span class="sxs-lookup"><span data-stu-id="f7c3f-120">The **IAuthenticatedEncryptorFactory** interface represents a type that knows how to create an [IAuthenticatedEncryptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptor) instance.</span></span> <span data-ttu-id="f7c3f-121">Die API lautet wie folgt aus:</span><span class="sxs-lookup"><span data-stu-id="f7c3f-121">Its API is as follows.</span></span>

* <span data-ttu-id="f7c3f-122">CreateEncryptorInstance ("iKey" Key): IAuthenticatedEncryptor</span><span class="sxs-lookup"><span data-stu-id="f7c3f-122">CreateEncryptorInstance(IKey key) : IAuthenticatedEncryptor</span></span>

<span data-ttu-id="f7c3f-123">Für jede bestimmte Instanz "iKey" alle authentifizierten lehnen die CreateEncryptorInstance-Methode erstellt sollte als äquivalent betrachtet, wie in der unten stehende Codebeispiel.</span><span class="sxs-lookup"><span data-stu-id="f7c3f-123">For any given IKey instance, any authenticated encryptors created by its CreateEncryptorInstance method should be considered equivalent, as in the below code sample.</span></span>

```csharp
// we have an IAuthenticatedEncryptorFactory instance and an IKey instance
IAuthenticatedEncryptorFactory factory = ...;
IKey key = ...;

// get an encryptor instance and perform an authenticated encryption operation
ArraySegment<byte> plaintext = new ArraySegment<byte>(Encoding.UTF8.GetBytes("plaintext"));
ArraySegment<byte> aad = new ArraySegment<byte>(Encoding.UTF8.GetBytes("AAD"));
var encryptor1 = factory.CreateEncryptorInstance(key);
byte[] ciphertext = encryptor1.Encrypt(plaintext, aad);

// get another encryptor instance and perform an authenticated decryption operation
var encryptor2 = factory.CreateEncryptorInstance(key);
byte[] roundTripped = encryptor2.Decrypt(new ArraySegment<byte>(ciphertext), aad);


// the 'roundTripped' and 'plaintext' buffers should be equivalent
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="f7c3f-124">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="f7c3f-124">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="f7c3f-125">Die **IAuthenticatedEncryptorDescriptor** Schnittstelle darstellt, einen Typ, der weiß, wie Sie erstellen eine [IAuthenticatedEncryptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptor) Instanz.</span><span class="sxs-lookup"><span data-stu-id="f7c3f-125">The **IAuthenticatedEncryptorDescriptor** interface represents a type that knows how to create an [IAuthenticatedEncryptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptor) instance.</span></span> <span data-ttu-id="f7c3f-126">Die API lautet wie folgt aus:</span><span class="sxs-lookup"><span data-stu-id="f7c3f-126">Its API is as follows.</span></span>

* <span data-ttu-id="f7c3f-127">CreateEncryptorInstance() : IAuthenticatedEncryptor</span><span class="sxs-lookup"><span data-stu-id="f7c3f-127">CreateEncryptorInstance() : IAuthenticatedEncryptor</span></span>

* <span data-ttu-id="f7c3f-128">ExportToXml() : XmlSerializedDescriptorInfo</span><span class="sxs-lookup"><span data-stu-id="f7c3f-128">ExportToXml() : XmlSerializedDescriptorInfo</span></span>

<span data-ttu-id="f7c3f-129">Wie IAuthenticatedEncryptor wird angenommen, dass eine Instanz von IAuthenticatedEncryptorDescriptor um einen bestimmten Schlüssel zu umschließen.</span><span class="sxs-lookup"><span data-stu-id="f7c3f-129">Like IAuthenticatedEncryptor, an instance of IAuthenticatedEncryptorDescriptor is assumed to wrap one specific key.</span></span> <span data-ttu-id="f7c3f-130">Dies bedeutet, dass für jede bestimmte Instanz IAuthenticatedEncryptorDescriptor alle authentifizierten lehnen die CreateEncryptorInstance-Methode erstellt wie im entsprechenden berücksichtigt werden sollte dem folgenden Codebeispiel.</span><span class="sxs-lookup"><span data-stu-id="f7c3f-130">This means that for any given IAuthenticatedEncryptorDescriptor instance, any authenticated encryptors created by its CreateEncryptorInstance method should be considered equivalent, as in the below code sample.</span></span>

```csharp
// we have an IAuthenticatedEncryptorDescriptor instance
IAuthenticatedEncryptorDescriptor descriptor = ...;

// get an encryptor instance and perform an authenticated encryption operation
ArraySegment<byte> plaintext = new ArraySegment<byte>(Encoding.UTF8.GetBytes("plaintext"));
ArraySegment<byte> aad = new ArraySegment<byte>(Encoding.UTF8.GetBytes("AAD"));
var encryptor1 = descriptor.CreateEncryptorInstance();
byte[] ciphertext = encryptor1.Encrypt(plaintext, aad);

// get another encryptor instance and perform an authenticated decryption operation
var encryptor2 = descriptor.CreateEncryptorInstance();
byte[] roundTripped = encryptor2.Decrypt(new ArraySegment<byte>(ciphertext), aad);


// the 'roundTripped' and 'plaintext' buffers should be equivalent
```

---

<a name="data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptor"></a>

## <a name="iauthenticatedencryptordescriptor-aspnet-core-2x-only"></a><span data-ttu-id="f7c3f-131">IAuthenticatedEncryptorDescriptor ((ASP.NET Core 2.x nur)</span><span class="sxs-lookup"><span data-stu-id="f7c3f-131">IAuthenticatedEncryptorDescriptor (ASP.NET Core 2.x only)</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="f7c3f-132">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="f7c3f-132">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="f7c3f-133">Die **IAuthenticatedEncryptorDescriptor** Schnittstelle darstellt, einen Typ, der weiß, wie Sie selbst nach XML exportieren.</span><span class="sxs-lookup"><span data-stu-id="f7c3f-133">The **IAuthenticatedEncryptorDescriptor** interface represents a type that knows how to export itself to XML.</span></span> <span data-ttu-id="f7c3f-134">Die API lautet wie folgt aus:</span><span class="sxs-lookup"><span data-stu-id="f7c3f-134">Its API is as follows.</span></span>

* <span data-ttu-id="f7c3f-135">ExportToXml() : XmlSerializedDescriptorInfo</span><span class="sxs-lookup"><span data-stu-id="f7c3f-135">ExportToXml() : XmlSerializedDescriptorInfo</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="f7c3f-136">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="f7c3f-136">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

---

## <a name="xml-serialization"></a><span data-ttu-id="f7c3f-137">XML-Serialisierung</span><span class="sxs-lookup"><span data-stu-id="f7c3f-137">XML Serialization</span></span>

<span data-ttu-id="f7c3f-138">Der Hauptunterschied zwischen IAuthenticatedEncryptor und IAuthenticatedEncryptorDescriptor ist, dass der Deskriptor weiß, wie die Verschlüsselung zu erstellen, und klicken Sie mit gültigen Argumente bereitstellen.</span><span class="sxs-lookup"><span data-stu-id="f7c3f-138">The primary difference between IAuthenticatedEncryptor and IAuthenticatedEncryptorDescriptor is that the descriptor knows how to create the encryptor and supply it with valid arguments.</span></span> <span data-ttu-id="f7c3f-139">Erwägen Sie eine IAuthenticatedEncryptor SymmetricAlgorithm und KeyedHashAlgorithm, deren Implementierung abhängig.</span><span class="sxs-lookup"><span data-stu-id="f7c3f-139">Consider an IAuthenticatedEncryptor whose implementation relies on SymmetricAlgorithm and KeyedHashAlgorithm.</span></span> <span data-ttu-id="f7c3f-140">Die Verschlüsselungsmethode für die Aufgabe besteht darin, diese Typen nutzen, aber er weiß nicht unbedingt, in denen diese Typen stammen, damit sie wirklich keiner entsprechenden Beschreibung zum selbst neu zu erstellen schreiben kann, wenn die Anwendung neu gestartet wird.</span><span class="sxs-lookup"><span data-stu-id="f7c3f-140">The encryptor's job is to consume these types, but it doesn't necessarily know where these types came from, so it can't really write out a proper description of how to recreate itself if the application restarts.</span></span> <span data-ttu-id="f7c3f-141">Der Deskriptor fungiert als eine höhere Sicherheitsstufe als Ergänzung.</span><span class="sxs-lookup"><span data-stu-id="f7c3f-141">The descriptor acts as a higher level on top of this.</span></span> <span data-ttu-id="f7c3f-142">Da der Deskriptor weiß, wie die Verschlüsselung-Instanz zu erstellen (z. B. er weiß, wie die erforderlichen Algorithmen zu erstellen), können sie dieses Wissen im XML-Format serialisieren, damit die Verschlüsselung-Instanz wiederhergestellt werden kann, nachdem eine Anwendung zurückgesetzt.</span><span class="sxs-lookup"><span data-stu-id="f7c3f-142">Since the descriptor knows how to create the encryptor instance (e.g., it knows how to create the required algorithms), it can serialize that knowledge in XML form so that the encryptor instance can be recreated after an application reset.</span></span>

<a name="data-protection-extensibility-core-crypto-exporttoxml"></a>

<span data-ttu-id="f7c3f-143">Der Deskriptor kann über die Routine ExportToXml serialisiert werden.</span><span class="sxs-lookup"><span data-stu-id="f7c3f-143">The descriptor can be serialized via its ExportToXml routine.</span></span> <span data-ttu-id="f7c3f-144">Diese Routine gibt ein XmlSerializedDescriptorInfo enthält zwei Eigenschaften: die "XElement"-Darstellung des Deskriptors und den Typ steht für ein [IAuthenticatedEncryptorDescriptorDeserializer](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptordeserializer) kann verwendet, um diesen Deskriptor, erhält der entsprechende "XElement" erwecken.</span><span class="sxs-lookup"><span data-stu-id="f7c3f-144">This routine returns an XmlSerializedDescriptorInfo which contains two properties: the XElement representation of the descriptor and the Type which represents an [IAuthenticatedEncryptorDescriptorDeserializer](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptordeserializer) which can be used to resurrect this descriptor given the corresponding XElement.</span></span>

<span data-ttu-id="f7c3f-145">Der serialisierte Deskriptor enthalten möglicherweise vertrauliche Informationen wie z. B. kryptografische Schlüsseldaten.</span><span class="sxs-lookup"><span data-stu-id="f7c3f-145">The serialized descriptor may contain sensitive information such as cryptographic key material.</span></span> <span data-ttu-id="f7c3f-146">Das System zum Schutz von Daten verfügt über integrierte Unterstützung für das Verschlüsseln von Informationen, bevor es in den Speicher dauerhaft gespeichert wird.</span><span class="sxs-lookup"><span data-stu-id="f7c3f-146">The data protection system has built-in support for encrypting information before it's persisted to storage.</span></span> <span data-ttu-id="f7c3f-147">Um diesen Vorteil nutzen zu können, sollte der Deskriptor markieren Sie das Element enthält sensiblen Informationen mit dem Attribut namens "" RequiresEncryption"" (Xmlns "<http://schemas.asp.net/2015/03/dataProtection>"), Wert "true".</span><span class="sxs-lookup"><span data-stu-id="f7c3f-147">To take advantage of this, the descriptor should mark the element which contains sensitive information with the attribute name "requiresEncryption" (xmlns "<http://schemas.asp.net/2015/03/dataProtection>"), value "true".</span></span>

>[!TIP]
> <span data-ttu-id="f7c3f-148">Für das Festlegen dieses Attributs ist ein Hilfs-API verfügbar.</span><span class="sxs-lookup"><span data-stu-id="f7c3f-148">There's a helper API for setting this attribute.</span></span> <span data-ttu-id="f7c3f-149">Rufen Sie die Erweiterungsmethode XElement.MarkAsRequiresEncryption() im Namespace Microsoft.AspNetCore.DataProtection.AuthenticatedEncryption.ConfigurationModel befindet.</span><span class="sxs-lookup"><span data-stu-id="f7c3f-149">Call the extension method XElement.MarkAsRequiresEncryption() located in namespace Microsoft.AspNetCore.DataProtection.AuthenticatedEncryption.ConfigurationModel.</span></span>

<span data-ttu-id="f7c3f-150">Es kann auch Fälle, in dem der serialisierte Deskriptor vertraulichen Informationen enthalten, nicht, vorhanden sein.</span><span class="sxs-lookup"><span data-stu-id="f7c3f-150">There can also be cases where the serialized descriptor doesn't contain sensitive information.</span></span> <span data-ttu-id="f7c3f-151">Betrachten Sie erneut den Fall eines kryptografischen Schlüssels, der in einem HSM gespeichert.</span><span class="sxs-lookup"><span data-stu-id="f7c3f-151">Consider again the case of a cryptographic key stored in an HSM.</span></span> <span data-ttu-id="f7c3f-152">Der Deskriptor kann nicht beim selbst serialisieren, da das HSM für das Material in unverschlüsselter Form verfügbar machen wird nicht das Schlüsselmaterial geschrieben.</span><span class="sxs-lookup"><span data-stu-id="f7c3f-152">The descriptor cannot write out the key material when serializing itself since the HSM won't expose the material in plaintext form.</span></span> <span data-ttu-id="f7c3f-153">Stattdessen kann der Deskriptor schreiben, die Schlüssel Wrapper-Version des Schlüssels (wenn das HSM Export auf diese Weise kann) oder den HSMs-Bezeichner für den Schlüssel.</span><span class="sxs-lookup"><span data-stu-id="f7c3f-153">Instead, the descriptor might write out the key-wrapped version of the key (if the HSM allows export in this fashion) or the HSM's own unique identifier for the key.</span></span>

<a name="data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptordeserializer"></a>

## <a name="iauthenticatedencryptordescriptordeserializer"></a><span data-ttu-id="f7c3f-154">IAuthenticatedEncryptorDescriptorDeserializer</span><span class="sxs-lookup"><span data-stu-id="f7c3f-154">IAuthenticatedEncryptorDescriptorDeserializer</span></span>

<span data-ttu-id="f7c3f-155">Die **IAuthenticatedEncryptorDescriptorDeserializer** Schnittstelle darstellt, einen Typ, der weiß, wie Sie eine Instanz eines "XElement" IAuthenticatedEncryptorDescriptor zu deserialisieren.</span><span class="sxs-lookup"><span data-stu-id="f7c3f-155">The **IAuthenticatedEncryptorDescriptorDeserializer** interface represents a type that knows how to deserialize an IAuthenticatedEncryptorDescriptor instance from an XElement.</span></span> <span data-ttu-id="f7c3f-156">Sie macht eine einzelne Methode verfügbar:</span><span class="sxs-lookup"><span data-stu-id="f7c3f-156">It exposes a single method:</span></span>

* <span data-ttu-id="f7c3f-157">ImportFromXml ("XElement"-Element): IAuthenticatedEncryptorDescriptor</span><span class="sxs-lookup"><span data-stu-id="f7c3f-157">ImportFromXml(XElement element) : IAuthenticatedEncryptorDescriptor</span></span>

<span data-ttu-id="f7c3f-158">Die ImportFromXml-Methode akzeptiert die "XElement", die von zurückgegeben wurde [IAuthenticatedEncryptorDescriptor.ExportToXml](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-exporttoxml) und eine entsprechende von der ursprünglichen IAuthenticatedEncryptorDescriptor erstellt.</span><span class="sxs-lookup"><span data-stu-id="f7c3f-158">The ImportFromXml method takes the XElement that was returned by [IAuthenticatedEncryptorDescriptor.ExportToXml](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-exporttoxml) and creates an equivalent of the original IAuthenticatedEncryptorDescriptor.</span></span>

<span data-ttu-id="f7c3f-159">Die IAuthenticatedEncryptorDescriptorDeserializer implementieren sollte eine der folgenden zwei öffentliche Konstruktoren aufweisen:</span><span class="sxs-lookup"><span data-stu-id="f7c3f-159">Types which implement IAuthenticatedEncryptorDescriptorDeserializer should have one of the following two public constructors:</span></span>

* <span data-ttu-id="f7c3f-160">.ctor(IServiceProvider)</span><span class="sxs-lookup"><span data-stu-id="f7c3f-160">.ctor(IServiceProvider)</span></span>

* <span data-ttu-id="f7c3f-161">.ctor()</span><span class="sxs-lookup"><span data-stu-id="f7c3f-161">.ctor()</span></span>

> [!NOTE]
> <span data-ttu-id="f7c3f-162">Der an den Konstruktor übergebene IServiceProvider kann null sein.</span><span class="sxs-lookup"><span data-stu-id="f7c3f-162">The IServiceProvider passed to the constructor may be null.</span></span>

## <a name="the-top-level-factory"></a><span data-ttu-id="f7c3f-163">Die Factory der obersten Ebene</span><span class="sxs-lookup"><span data-stu-id="f7c3f-163">The top-level factory</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="f7c3f-164">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="f7c3f-164">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="f7c3f-165">Die **AlgorithmConfiguration** Klasse stellt einen Typ, der weiß, wie Sie erstellen [IAuthenticatedEncryptorDescriptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptor) Instanzen.</span><span class="sxs-lookup"><span data-stu-id="f7c3f-165">The **AlgorithmConfiguration** class represents a type which knows how to create [IAuthenticatedEncryptorDescriptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptor) instances.</span></span> <span data-ttu-id="f7c3f-166">Er macht eine einzelne API verfügbar.</span><span class="sxs-lookup"><span data-stu-id="f7c3f-166">It exposes a single API.</span></span>

* <span data-ttu-id="f7c3f-167">CreateNewDescriptor() : IAuthenticatedEncryptorDescriptor</span><span class="sxs-lookup"><span data-stu-id="f7c3f-167">CreateNewDescriptor() : IAuthenticatedEncryptorDescriptor</span></span>

<span data-ttu-id="f7c3f-168">Stellen Sie sich AlgorithmConfiguration als Factory der obersten Ebene.</span><span class="sxs-lookup"><span data-stu-id="f7c3f-168">Think of AlgorithmConfiguration as the top-level factory.</span></span> <span data-ttu-id="f7c3f-169">Die Konfiguration dient als Vorlage.</span><span class="sxs-lookup"><span data-stu-id="f7c3f-169">The configuration serves as a template.</span></span> <span data-ttu-id="f7c3f-170">Umschlossene algorithmische Informationen (z. B. diese Konfiguration erzeugt Deskriptoren mit einem Hauptschlüssel AES-128-GCM), aber es wurde noch nicht mit einem bestimmten Schlüssel verknüpft.</span><span class="sxs-lookup"><span data-stu-id="f7c3f-170">It wraps algorithmic information (e.g., this configuration produces descriptors with an AES-128-GCM master key), but it's not yet associated with a specific key.</span></span>

<span data-ttu-id="f7c3f-171">Wenn CreateNewDescriptor aufgerufen wird, neues des Schlüsselmaterials ausschließlich für diesen Aufruf erstellt wird und eine neue IAuthenticatedEncryptorDescriptor erzeugt wird umschließt die diesem Schlüsselmaterial sowie die algorithmische Informationen erforderlich, um das Material zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="f7c3f-171">When CreateNewDescriptor is called, fresh key material is created solely for this call, and a new IAuthenticatedEncryptorDescriptor is produced which wraps this key material and the algorithmic information required to consume the material.</span></span> <span data-ttu-id="f7c3f-172">Das Schlüsselmaterial konnte in der Software erstellt (und im Arbeitsspeicher gespeichert werden), es erstellt und in einem HSM usw. gespeichert werden konnte.</span><span class="sxs-lookup"><span data-stu-id="f7c3f-172">The key material could be created in software (and held in memory), it could be created and held within an HSM, and so on.</span></span> <span data-ttu-id="f7c3f-173">Der entscheidende Punkt ist, dass zwei Aufrufe CreateNewDescriptor nie entsprechende IAuthenticatedEncryptorDescriptor-Instanzen erstellen soll.</span><span class="sxs-lookup"><span data-stu-id="f7c3f-173">The crucial point is that any two calls to CreateNewDescriptor should never create equivalent IAuthenticatedEncryptorDescriptor instances.</span></span>

<span data-ttu-id="f7c3f-174">Welche AlgorithmConfiguration dient als Einstiegspunkt für die schlüsselerstellung Routinen wie z. B. [automatische schlüsselrollover](xref:security/data-protection/implementation/key-management#key-expiration-and-rolling).</span><span class="sxs-lookup"><span data-stu-id="f7c3f-174">The AlgorithmConfiguration type serves as the entry point for key creation routines such as [automatic key rolling](xref:security/data-protection/implementation/key-management#key-expiration-and-rolling).</span></span> <span data-ttu-id="f7c3f-175">Um die Implementierung für alle zukünftigen Schlüssel zu ändern, legen Sie die AuthenticatedEncryptorConfiguration-Eigenschaft in KeyManagementOptions aus.</span><span class="sxs-lookup"><span data-stu-id="f7c3f-175">To change the implementation for all future keys, set the AuthenticatedEncryptorConfiguration property in KeyManagementOptions.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="f7c3f-176">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="f7c3f-176">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="f7c3f-177">Die **IAuthenticatedEncryptorConfiguration** Schnittstelle darstellt, einen Typ, der weiß, wie Sie erstellen [IAuthenticatedEncryptorDescriptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptor) Instanzen.</span><span class="sxs-lookup"><span data-stu-id="f7c3f-177">The **IAuthenticatedEncryptorConfiguration** interface represents a type which knows how to create [IAuthenticatedEncryptorDescriptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptor) instances.</span></span> <span data-ttu-id="f7c3f-178">Er macht eine einzelne API verfügbar.</span><span class="sxs-lookup"><span data-stu-id="f7c3f-178">It exposes a single API.</span></span>

* <span data-ttu-id="f7c3f-179">CreateNewDescriptor() : IAuthenticatedEncryptorDescriptor</span><span class="sxs-lookup"><span data-stu-id="f7c3f-179">CreateNewDescriptor() : IAuthenticatedEncryptorDescriptor</span></span>

<span data-ttu-id="f7c3f-180">Stellen Sie sich IAuthenticatedEncryptorConfiguration als Factory der obersten Ebene.</span><span class="sxs-lookup"><span data-stu-id="f7c3f-180">Think of IAuthenticatedEncryptorConfiguration as the top-level factory.</span></span> <span data-ttu-id="f7c3f-181">Die Konfiguration dient als Vorlage.</span><span class="sxs-lookup"><span data-stu-id="f7c3f-181">The configuration serves as a template.</span></span> <span data-ttu-id="f7c3f-182">Umschlossene algorithmische Informationen (z. B. diese Konfiguration erzeugt Deskriptoren mit einem Hauptschlüssel AES-128-GCM), aber es wurde noch nicht mit einem bestimmten Schlüssel verknüpft.</span><span class="sxs-lookup"><span data-stu-id="f7c3f-182">It wraps algorithmic information (e.g., this configuration produces descriptors with an AES-128-GCM master key), but it's not yet associated with a specific key.</span></span>

<span data-ttu-id="f7c3f-183">Wenn CreateNewDescriptor aufgerufen wird, neues des Schlüsselmaterials ausschließlich für diesen Aufruf erstellt wird und eine neue IAuthenticatedEncryptorDescriptor erzeugt wird umschließt die diesem Schlüsselmaterial sowie die algorithmische Informationen erforderlich, um das Material zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="f7c3f-183">When CreateNewDescriptor is called, fresh key material is created solely for this call, and a new IAuthenticatedEncryptorDescriptor is produced which wraps this key material and the algorithmic information required to consume the material.</span></span> <span data-ttu-id="f7c3f-184">Das Schlüsselmaterial konnte in der Software erstellt (und im Arbeitsspeicher gespeichert werden), es erstellt und in einem HSM usw. gespeichert werden konnte.</span><span class="sxs-lookup"><span data-stu-id="f7c3f-184">The key material could be created in software (and held in memory), it could be created and held within an HSM, and so on.</span></span> <span data-ttu-id="f7c3f-185">Der entscheidende Punkt ist, dass zwei Aufrufe CreateNewDescriptor nie entsprechende IAuthenticatedEncryptorDescriptor-Instanzen erstellen soll.</span><span class="sxs-lookup"><span data-stu-id="f7c3f-185">The crucial point is that any two calls to CreateNewDescriptor should never create equivalent IAuthenticatedEncryptorDescriptor instances.</span></span>

<span data-ttu-id="f7c3f-186">Welche IAuthenticatedEncryptorConfiguration dient als Einstiegspunkt für die schlüsselerstellung Routinen wie z. B. [automatische schlüsselrollover](xref:security/data-protection/implementation/key-management#key-expiration-and-rolling).</span><span class="sxs-lookup"><span data-stu-id="f7c3f-186">The IAuthenticatedEncryptorConfiguration type serves as the entry point for key creation routines such as [automatic key rolling](xref:security/data-protection/implementation/key-management#key-expiration-and-rolling).</span></span> <span data-ttu-id="f7c3f-187">Um die Implementierung für alle zukünftigen Schlüssel zu ändern, registrieren Sie einen Singleton IAuthenticatedEncryptorConfiguration im Dienstcontainer.</span><span class="sxs-lookup"><span data-stu-id="f7c3f-187">To change the implementation for all future keys, register a singleton IAuthenticatedEncryptorConfiguration in the service container.</span></span>

---
