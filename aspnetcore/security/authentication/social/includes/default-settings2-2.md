---
ms.openlocfilehash: 733a41a76e289f8aed6ec2d246ed720e44808fec
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/01/2019
ms.locfileid: "57182760"
---
Der Aufruf von [AddIdentity](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionextensions.addidentity) konfiguriert die Einstellungen der Standard-Schema. Die [AddAuthentication(String)](/dotnet/api/microsoft.extensions.dependencyinjection.authenticationservicecollectionextensions.addauthentication#Microsoft_Extensions_DependencyInjection_AuthenticationServiceCollectionExtensions_AddAuthentication_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_String_) überladen, legt die [DefaultScheme](/dotnet/api/microsoft.aspnetcore.authentication.authenticationoptions.defaultscheme) Eigenschaft. Die [AddAuthentication (Aktion&lt;authenticationoptions&gt;)](/dotnet/api/microsoft.extensions.dependencyinjection.authenticationservicecollectionextensions.addauthentication#Microsoft_Extensions_DependencyInjection_AuthenticationServiceCollectionExtensions_AddAuthentication_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Action_Microsoft_AspNetCore_Authentication_AuthenticationOptions__) Überladung ermöglicht das Konfigurieren von Authentifizierungsoptionen, die zum Einrichten von standardmäßigen Authentifizierungsschemas für unterschiedliche Zwecke verwendet werden kann. Nachfolgende Aufrufe von `AddAuthentication` außer Kraft setzen, die zuvor konfigurierten [authenticationoptions](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions) Eigenschaften.

[AuthenticationBuilder](/dotnet/api/microsoft.aspnetcore.authentication.authenticationbuilder) Erweiterungsmethoden, die registriert einen authentifizierungshandler für die können nur einmal pro Authentifizierungsschema aufgerufen werden. Überladungen vorhanden sind, mit denen die Konfiguration von Eigenschaften für Schema, Name des Schemas und Anzeigename.
