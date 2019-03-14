---
ms.openlocfilehash: 3b33bb9bcab1aa7e5438c6fe3f2d7ba0833e7756
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/01/2019
ms.locfileid: "57054727"
---
--

## <a name="reporting-an-issue"></a>Melden eines Problems

1. Stellen Sie sicher, dass das Problem, das Sie melden möchten, reproduzierbar ist.
2. Verwenden Sie http://jsbin.com oder http://jsfiddle.net, um eine Testseite bereitzustellen.
3. Geben Sie an, in welchem Browser das Problem reproduziert werden kann. **Hinweis: Probleme des IE-Kompatibilitätsmodus werden nicht behandelt. Stellen Sie sicher, dass Sie in einem echten Browser testen!**
4. In welcher Version des Plug-Ins kann das Problem reproduziert werden? Ist es reproduzierbar, nachdem ein Update auf die neueste Version ausgeführt wurde?

Dokumentationsprobleme werden ebenfalls mit der Problemverfolgung [jQuery-Überprüfung](https://github.com/jzaefferer/jquery-validation/issues) nachverfolgt.
Pull Requests zur Verbesserung der Dokumentation sind hingegen im Repository für die [Dokumentation zur jQuery-Überprüfung](https://github.com/jzaefferer/validation-content) willkommen.

**WICHTIGER HINWEIS ZUR E-MAIL-ÜBERPRÜFUNG**. Ab Version 1.12.0 verwendet dieses Plug-In den gleichen regulären Ausdruck, den die [HTML5-Spezifikation für die Verwendung durch Browser vorschlägt](https://html.spec.whatwg.org/multipage/forms.html#valid-e-mail-address). Wir werden diesem Beispiel folgen und die gleiche Überprüfung verwenden. Wenn Sie der Meinung sind, dass die Spezifikation falsch ist, melden Sie das Problem bitte dort. Wenn Sie andere Anforderungen haben, ziehen Sie die [Verwendung einer benutzerdefinierten Methode](http://jqueryvalidation.org/jQuery.validator.addMethod/) in Betracht.

## <a name="contributing-code"></a>Beitragen von Code

Vielen Dank für Ihren Beitrag. Hier sind ein paar Richtlinien für einen erfolgreichen Beitrag.

1. Stellen Sie sicher, dass das Problem, das Sie melden möchten, reproduzierbar ist. Verwenden Sie jsbin.com oder jsfiddle.net, um eine Testseite bereitzustellen.
2. Befolgen des [jQuery-Styleguides](http://contribute.jquery.com/style-guides/js)
3. Fügen Sie Komponententests zusammen mit Ihrem Patch hinzu, oder aktualisieren Sie diese. Führen Sie die Komponententests in mindestens einem Browser aus (siehe unten).
4. Führen Sie `grunt` aus (siehe unten), um Linting und einige andere Probleme zu überprüfen.
5. Beschreiben Sie die Änderung in Ihrer commitnachricht, und verweisen auf das Ticket, wie folgt: "Demos: Korrigieren des Delegatenfehlers für dynamic-totals-Demo. Fixes #51“. Wenn Sie eine neue Lokalisierungsdatei hinzufügen, verwenden Sie etwa wie folgt: "Localization: Hinzugefügte Kroatisch (HR) Localization"

## <a name="build-setup"></a>Buildeinrichtung

1. Installieren Sie [NodeJS](http://nodejs.org).
2. Installieren Sie die zu installierende Grunt CLI, indem Sie `npm install -g grunt-cli` ausführen. Weitere Informationen finden Sie auf der Website http://gruntjs.com/getting-started.
3. Installieren Sie die npm-Abhängigkeiten, indem Sie `npm install` ausführen.
4. Der Build kann nun aufgerufen werden, indem `grunt` ausgeführt wird.

## <a name="creating-a-new-additional-method"></a>Erstellen einer neuen zusätzlichen Methode

Wenn Sie benutzerdefinierte Methoden geschrieben haben, die Sie zu additional-methods.js betragen möchten, gehen Sie folgendermaßen vor:

1. Erstellen einer Verzweigung
2. Hinzufügen der Methode als neue Datei in `src/additional`
3. (Optional) Hinzufügen von Übersetzungen zu `src/localization`
4. Senden Sie einen Pull Request an den Masterbranch.

## <a name="unit-tests"></a>Komponententests

Öffnen Sie zum Ausführen von Komponententests einfach `test/index.html` in Ihrem Browser. Stellen Sie sicher, dass Sie zuvor `npm install` ausgeführt haben, damit alle erforderlichen Abhängigkeiten zur Verfügung stehen.
Beginnen Sie mit einem Browser zum Entwickeln der Problembehandlung, und führen Sie diese dann mit anderen Browsern aus, bevor Sie sie committen. In der Regel werden die neuesten Versionen von Chrome, Firefox, Safari und Opera sowie einige IEs verwendet.

## <a name="documentation"></a>Dokumentation

Melden Sie Dokumentationsprobleme bei der Problemverfolgung [jQuery-Überprüfung](https://github.com/jzaefferer/jquery-validation/issues).
Falls Ihr Pull Request die öffentliche API implementiert oder ändert, wäre es von Vorteil, wenn Sie einen Pull Request für das Repository für [Dokumentation zur jQuery-Überprüfung](https://github.com/jzaefferer/validation-content) bereitstellen würden.

## <a name="linting"></a>Linting

Verwenden Sie `grunt`, um JSHint und andere Tools auszuführen.
