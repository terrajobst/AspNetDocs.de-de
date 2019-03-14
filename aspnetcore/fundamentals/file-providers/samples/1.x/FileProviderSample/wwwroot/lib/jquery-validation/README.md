---
ms.openlocfilehash: 24c36493284988aa45b15c78e2577c0ef96aba56
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/01/2019
ms.locfileid: "57029987"
---
<a name="jquery-validation-pluginhttpsjqueryvalidationorg---form-validation-made-easy"></a><span data-ttu-id="2a364-101">[jQuery Validation Plugin (jQuery-Überprüfungs-Plug-In):](https://jqueryvalidation.org/) Einfache Überprüfung von Formularen</span><span class="sxs-lookup"><span data-stu-id="2a364-101">[jQuery Validation Plugin](https://jqueryvalidation.org/) - Form validation made easy</span></span>
================================

<span data-ttu-id="2a364-102">[![Buildstatus](https://secure.travis-ci.org/jquery-validation/jquery-validation.svg)](https://travis-ci.org/jquery-validation/jquery-validation)
[![devDependency-Status](https://david-dm.org/jquery-validation/jquery-validation/dev-status.svg?theme=shields.io)](https://david-dm.org/jquery-validation/jquery-validation#info=devDependencies)</span><span class="sxs-lookup"><span data-stu-id="2a364-102">[![Build Status](https://secure.travis-ci.org/jquery-validation/jquery-validation.svg)](https://travis-ci.org/jquery-validation/jquery-validation)
[![devDependency Status](https://david-dm.org/jquery-validation/jquery-validation/dev-status.svg?theme=shields.io)](https://david-dm.org/jquery-validation/jquery-validation#info=devDependencies)</span></span>

<span data-ttu-id="2a364-103">Das jQuery-Überprüfungs-Plug-In bietet kurzfristige Überprüfung für Ihre vorhandenen Formulare und vereinfacht alle Arten von Anpassungen für Ihre Anwendung.</span><span class="sxs-lookup"><span data-stu-id="2a364-103">The jQuery Validation Plugin provides drop-in validation for your existing forms, while making all kinds of customizations to fit your application really easy.</span></span>

## <a name="getting-started"></a><span data-ttu-id="2a364-104">Erste Schritte</span><span class="sxs-lookup"><span data-stu-id="2a364-104">Getting Started</span></span>

### <a name="downloading-the-prebuilt-files"></a><span data-ttu-id="2a364-105">Herunterladen der vorkompilierten Dateien</span><span class="sxs-lookup"><span data-stu-id="2a364-105">Downloading the prebuilt files</span></span>

<span data-ttu-id="2a364-106">Vorkompilierte Dateien können hier heruntergeladen werden: https://jqueryvalidation.org/</span><span class="sxs-lookup"><span data-stu-id="2a364-106">Prebuilt files can be downloaded from https://jqueryvalidation.org/</span></span>

### <a name="downloading-the-latest-changes"></a><span data-ttu-id="2a364-107">Herunterladen der neuesten Änderungen</span><span class="sxs-lookup"><span data-stu-id="2a364-107">Downloading the latest changes</span></span>

<span data-ttu-id="2a364-108">Die noch nicht veröffentlichten Entwicklungsdateien können wie folgt abgerufen werden:</span><span class="sxs-lookup"><span data-stu-id="2a364-108">The unreleased development files can be obtained by:</span></span>

 1. <span data-ttu-id="2a364-109">[Herunterladen](https://github.com/jquery-validation/jquery-validation/archive/master.zip) oder Forken dieses Repositorys</span><span class="sxs-lookup"><span data-stu-id="2a364-109">[Downloading](https://github.com/jquery-validation/jquery-validation/archive/master.zip) or Forking this repository</span></span>
 2. [<span data-ttu-id="2a364-110">Einrichten des Builds</span><span class="sxs-lookup"><span data-stu-id="2a364-110">Setup the build</span></span>](CONTRIBUTING.md#build-setup)
 3. <span data-ttu-id="2a364-111">Führen Sie `grunt` zum Erstellen der Builddateien im Verzeichnis „dist“ aus.</span><span class="sxs-lookup"><span data-stu-id="2a364-111">Run `grunt` to create the built files in the "dist" directory</span></span>

### <a name="including-it-on-your-page"></a><span data-ttu-id="2a364-112">Einbinden des Plug-Ins auf Ihrer Seite</span><span class="sxs-lookup"><span data-stu-id="2a364-112">Including it on your page</span></span>

<span data-ttu-id="2a364-113">Binden Sie jQuery und das Plug-In auf einer Seite ein.</span><span class="sxs-lookup"><span data-stu-id="2a364-113">Include jQuery and the plugin on a page.</span></span> <span data-ttu-id="2a364-114">Wählen Sie dann ein zu überprüfendes Formular aus, und rufen Sie die `validate`-Methode auf.</span><span class="sxs-lookup"><span data-stu-id="2a364-114">Then select a form to validate and call the `validate` method.</span></span>

```html
<form>
    <input required>
</form>
<script src="jquery.js"></script>
<script src="jquery.validate.js"></script>
<script>
$("form").validate();
</script>
```

<span data-ttu-id="2a364-115">Binden Sie alternativ dazu jQuery und das Plug-In über requirejs in Ihr Modul ein.</span><span class="sxs-lookup"><span data-stu-id="2a364-115">Alternatively include jQuery and the plugin via requirejs in your module.</span></span>

```js
define(["jquery", "jquery.validate"], function( $ ) {
    $("form").validate();
});
```

<span data-ttu-id="2a364-116">Weitere Informationen zum Einrichten von Regeln und Anpassungen [finden Sie in der Dokumentation](https://jqueryvalidation.org/documentation/).</span><span class="sxs-lookup"><span data-stu-id="2a364-116">For more information on how to setup a rules and customizations, [check the documentation](https://jqueryvalidation.org/documentation/).</span></span>

## <a name="reporting-issues-and-contributing-code"></a><span data-ttu-id="2a364-117">Melden von Problemen und Beitragen von Code</span><span class="sxs-lookup"><span data-stu-id="2a364-117">Reporting issues and contributing code</span></span>

<span data-ttu-id="2a364-118">Details finden Sie in den [Richtlinien zu Beiträgen](CONTRIBUTING.md).</span><span class="sxs-lookup"><span data-stu-id="2a364-118">See the [Contributing Guidelines](CONTRIBUTING.md) for details.</span></span>

<span data-ttu-id="2a364-119">**WICHTIGER HINWEIS ZUR E-MAIL-ÜBERPRÜFUNG**.</span><span class="sxs-lookup"><span data-stu-id="2a364-119">**IMPORTANT NOTE ABOUT EMAIL VALIDATION**.</span></span> <span data-ttu-id="2a364-120">Ab Version 1.12.0 verwendet dieses Plug-In den gleichen regulären Ausdruck, den die [HTML5-Spezifikation für die Verwendung durch Browser vorschlägt](https://html.spec.whatwg.org/multipage/forms.html#valid-e-mail-address).</span><span class="sxs-lookup"><span data-stu-id="2a364-120">As of version 1.12.0 this plugin is using the same regular expression that the [HTML5 specification suggests for browsers to use](https://html.spec.whatwg.org/multipage/forms.html#valid-e-mail-address).</span></span> <span data-ttu-id="2a364-121">Wir werden diesem Beispiel folgen und die gleiche Überprüfung verwenden.</span><span class="sxs-lookup"><span data-stu-id="2a364-121">We will follow their lead and use the same check.</span></span> <span data-ttu-id="2a364-122">Wenn Sie der Meinung sind, dass die Spezifikation falsch ist, melden Sie das Problem bitte dort.</span><span class="sxs-lookup"><span data-stu-id="2a364-122">If you think the specification is wrong, please report the issue to them.</span></span> <span data-ttu-id="2a364-123">Wenn Sie andere Anforderungen haben, ziehen Sie die [Verwendung einer benutzerdefinierten Methode](https://jqueryvalidation.org/jQuery.validator.addMethod/) in Betracht.</span><span class="sxs-lookup"><span data-stu-id="2a364-123">If you have different requirements, consider [using a custom method](https://jqueryvalidation.org/jQuery.validator.addMethod/).</span></span>
<span data-ttu-id="2a364-124">Falls Sie die eingebauten Validierungsmuster für reguläre Ausdrücke anpassen müssen, folgen Sie bitte [ der Dokumentation](https://jqueryvalidation.org/jQuery.validator.methods/).</span><span class="sxs-lookup"><span data-stu-id="2a364-124">In case you need to adjust the built-in validation regular expression patterns, please [follow the documentation](https://jqueryvalidation.org/jQuery.validator.methods/).</span></span>

<span data-ttu-id="2a364-125">**WICHTIGER HINWEIS ZUR ERFORDERLICHEN METHODE**.</span><span class="sxs-lookup"><span data-stu-id="2a364-125">**IMPORTANT NOTE ABOUT REQUIRED METHOD**.</span></span> <span data-ttu-id="2a364-126">Ab Version 1.14.0 beendet dieses Plugin das Kürzen von Leerzeichen aus dem Wert des angefügten Elements.</span><span class="sxs-lookup"><span data-stu-id="2a364-126">As of version 1.14.0 this plugin stops trimming white spaces from the value of the attached element.</span></span> <span data-ttu-id="2a364-127">Wenn Sie das gleiche Ergebnis erzielen wollen, können Sie [`normalizer`](https://jqueryvalidation.org/normalizer/) verwenden, um den Wert eines Elements vor der Validierung zu transformieren.</span><span class="sxs-lookup"><span data-stu-id="2a364-127">If you want to achieve the same result, you can use the [`normalizer`](https://jqueryvalidation.org/normalizer/) that can be used to transform the value of an element before validation.</span></span> <span data-ttu-id="2a364-128">Dieses Feature ist seit `v1.15.0` verfügbar.</span><span class="sxs-lookup"><span data-stu-id="2a364-128">This feature was available since `v1.15.0`.</span></span> <span data-ttu-id="2a364-129">Das heißt, Sie haben diese Möglichkeiten:</span><span class="sxs-lookup"><span data-stu-id="2a364-129">In other words, you can do something like this:</span></span>
``` js
$("#myForm").validate({
    rules: {
        username: {
            required: true,
            // Using the normalizer to trim the value of the element
            // before validating it.
            //
            // The value of `this` inside the `normalizer` is the corresponding
            // DOMElement. In this example, `this` references the `username` element.
            normalizer: function(value) {
                return $.trim(value);
            }
        }
    }
});
```

## <a name="license"></a><span data-ttu-id="2a364-130">Lizenz</span><span class="sxs-lookup"><span data-stu-id="2a364-130">License</span></span>
<span data-ttu-id="2a364-131">Copyright &copy; Jörn Zaefferer</span><span class="sxs-lookup"><span data-stu-id="2a364-131">Copyright &copy; Jörn Zaefferer</span></span><br>
<span data-ttu-id="2a364-132">Lizenziert unter der MIT-Lizenz.</span><span class="sxs-lookup"><span data-stu-id="2a364-132">Licensed under the MIT license.</span></span>
