---
title: Einführung in die Autorisierung in ASP.NET Core
author: rick-anderson
description: Enthält die Grundlagen der Autorisierung und die Funktionsweise der Autorisierung in ASP.NET Core-apps.
ms.author: riande
ms.date: 10/14/2016
uid: security/authorization/introduction
ms.openlocfilehash: 5465eb7875ebecd77b628376ef886db0ddd05025
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/01/2019
ms.locfileid: "57046367"
---
# <a name="introduction-to-authorization-in-aspnet-core"></a>Einführung in die Autorisierung in ASP.NET Core

<a name="security-authorization-introduction"></a>

Autorisierung bezieht sich auf den Prozess, der bestimmt, was ein Benutzer möglich ist. Beispielsweise kann ein Administrator eine Dokumentbibliothek erstellen, Dokumente hinzufügen, Bearbeiten von Dokumenten und löschen Sie sie. Ein Benutzer ohne Administratorrechte arbeiten mit der Bibliothek wird nur autorisierte, um die Dokumente zu lesen.

Autorisierung ist orthogonal und unabhängig von der Authentifizierung. Allerdings erfordert eine Autorisierung einen Authentifizierungsmechanismus. Authentifizierung ist der Prozess der Feststellung, wer ein Benutzer ist. Authentifizierung kann eine oder mehrere Identitäten für den aktuellen Benutzer erstellen.

## <a name="authorization-types"></a>Autorisierungstypen

ASP.NET Core-Autorisierung stellt eine einfache, deklarative [Rolle](xref:security/authorization/roles) und einen umfangreichen [richtlinienbasierten](xref:security/authorization/policies) Modell. Autorisierung Anforderungen ausgedrückt wird, und Handler auswerten, die Benutzeransprüche im Hinblick auf Anforderungen. Imperative Überprüfungen können auf einfache Richtlinien oder Richtlinien ausgewertet werden, sowohl die Identität des Benutzers als auch die Eigenschaften der Ressource, die der Benutzer zugreifen möchte, basieren.

## <a name="namespaces"></a>Namespaces

Authorization-Komponenten, einschließlich der `AuthorizeAttribute` und `AllowAnonymousAttribute` Attribute befinden sich die `Microsoft.AspNetCore.Authorization` Namespace.

In der Dokumentation auf [einfache Autorisierung](xref:security/authorization/simple).
