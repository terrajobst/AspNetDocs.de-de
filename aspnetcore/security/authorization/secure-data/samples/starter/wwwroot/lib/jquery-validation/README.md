---
ms.openlocfilehash: 83a958fe6a8d61becf9f9062e8ad0ff69108b435
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/01/2019
ms.locfileid: "57034987"
---
<a name="jquery-validation-pluginhttpjqueryvalidationorg---form-validation-made-easy"></a><span data-ttu-id="f523a-101">[jQuery Validation Plugin (jQuery-Überprüfungs-Plug-In):](http://jqueryvalidation.org/) Einfache Überprüfung von Formularen</span><span class="sxs-lookup"><span data-stu-id="f523a-101">[jQuery Validation Plugin](http://jqueryvalidation.org/) - Form validation made easy</span></span>
================================

<span data-ttu-id="f523a-102">[![Buildstatus](https://secure.travis-ci.org/jzaefferer/jquery-validation.png)](http://travis-ci.org/jzaefferer/jquery-validation)
[![devDependency-Status](https://david-dm.org/jzaefferer/jquery-validation/dev-status.png?theme=shields.io)](https://david-dm.org/jzaefferer/jquery-validation#info=devDependencies)</span><span class="sxs-lookup"><span data-stu-id="f523a-102">[![Build Status](https://secure.travis-ci.org/jzaefferer/jquery-validation.png)](http://travis-ci.org/jzaefferer/jquery-validation)
[![devDependency Status](https://david-dm.org/jzaefferer/jquery-validation/dev-status.png?theme=shields.io)](https://david-dm.org/jzaefferer/jquery-validation#info=devDependencies)</span></span>

<span data-ttu-id="f523a-103">Das jQuery-Überprüfungs-Plug-In bietet kurzfristige Überprüfung für Ihre vorhandenen Formulare und vereinfacht alle Arten von Anpassungen für Ihre Anwendung.</span><span class="sxs-lookup"><span data-stu-id="f523a-103">The jQuery Validation Plugin provides drop-in validation for your existing forms, while making all kinds of customizations to fit your application really easy.</span></span>

## <a name="help-the-projecthttppledgiecomcampaigns18159"></a>[<span data-ttu-id="f523a-104">Unterstützen Sie das Projekt</span><span class="sxs-lookup"><span data-stu-id="f523a-104">Help the project</span></span>](http://pledgie.com/campaigns/18159)

<span data-ttu-id="f523a-105">[![Unterstützen Sie das Projekt](http://www.pledgie.com/campaigns/18159.png?skin_name=chrome)](http://pledgie.com/campaigns/18159)</span><span class="sxs-lookup"><span data-stu-id="f523a-105">[![Help the project](http://www.pledgie.com/campaigns/18159.png?skin_name=chrome)](http://pledgie.com/campaigns/18159)</span></span>

<span data-ttu-id="f523a-106">Dieses Projekt benötigt Unterstützung!</span><span class="sxs-lookup"><span data-stu-id="f523a-106">This project is looking for help!</span></span> <span data-ttu-id="f523a-107">[Sie können mit einer Spende die aktuelle Pledgie-Kampagne unterstützen](http://pledgie.com/campaigns/18159) und dafür sorgen, dass dem Projekt größere Aufmerksamkeit zuteil wird.</span><span class="sxs-lookup"><span data-stu-id="f523a-107">[You can donate to the ongoing pledgie campaign](http://pledgie.com/campaigns/18159) and help spread the word.</span></span> <span data-ttu-id="f523a-108">Wenn Sie das Plug-In bereits verwendet haben oder seine Verwendung planen, sollten Sie eine Spende in Betracht ziehen. Jeder Betrag ist willkommen.</span><span class="sxs-lookup"><span data-stu-id="f523a-108">If you've used the plugin, or plan to use, consider a donation - any amount will help.</span></span>

<span data-ttu-id="f523a-109">Sie finden den Plan zur Verwendung der Spenden auf der [Pledgie-Seite](http://pledgie.com/campaigns/18159).</span><span class="sxs-lookup"><span data-stu-id="f523a-109">You can find the plan for how to spend the money on the [pledgie page](http://pledgie.com/campaigns/18159).</span></span>

## <a name="getting-started"></a><span data-ttu-id="f523a-110">Erste Schritte</span><span class="sxs-lookup"><span data-stu-id="f523a-110">Getting Started</span></span>

### <a name="downloading-the-prebuilt-files"></a><span data-ttu-id="f523a-111">Herunterladen der vorkompilierten Dateien</span><span class="sxs-lookup"><span data-stu-id="f523a-111">Downloading the prebuilt files</span></span>

<span data-ttu-id="f523a-112">Vorkompilierte Dateien können hier heruntergeladen werden: http://jqueryvalidation.org/</span><span class="sxs-lookup"><span data-stu-id="f523a-112">Prebuilt files can be downloaded from http://jqueryvalidation.org/</span></span>

### <a name="downloading-the-latest-changes"></a><span data-ttu-id="f523a-113">Herunterladen der neuesten Änderungen</span><span class="sxs-lookup"><span data-stu-id="f523a-113">Downloading the latest changes</span></span>

<span data-ttu-id="f523a-114">Die noch nicht veröffentlichten Entwicklungsdateien können wie folgt abgerufen werden:</span><span class="sxs-lookup"><span data-stu-id="f523a-114">The unreleased development files can be obtained by:</span></span>

 1. <span data-ttu-id="f523a-115">[Herunterladen](https://github.com/jzaefferer/jquery-validation/archive/master.zip) oder Forken dieses Repositorys</span><span class="sxs-lookup"><span data-stu-id="f523a-115">[Downloading](https://github.com/jzaefferer/jquery-validation/archive/master.zip) or Forking this repository</span></span>
 2. [<span data-ttu-id="f523a-116">Einrichten des Builds</span><span class="sxs-lookup"><span data-stu-id="f523a-116">Setup the build</span></span>](CONTRIBUTING.md#build-setup)
 3. <span data-ttu-id="f523a-117">Führen Sie `grunt` zum Erstellen der Builddateien im Verzeichnis „dist“ aus.</span><span class="sxs-lookup"><span data-stu-id="f523a-117">Run `grunt` to create the built files in the "dist" directory</span></span>

### <a name="including-it-on-your-page"></a><span data-ttu-id="f523a-118">Einbinden des Plug-Ins auf Ihrer Seite</span><span class="sxs-lookup"><span data-stu-id="f523a-118">Including it on your page</span></span>

<span data-ttu-id="f523a-119">Binden Sie jQuery und das Plug-In auf einer Seite ein.</span><span class="sxs-lookup"><span data-stu-id="f523a-119">Include jQuery and the plugin on a page.</span></span> <span data-ttu-id="f523a-120">Wählen Sie dann ein zu überprüfendes Formular aus, und rufen Sie die `validate`-Methode auf.</span><span class="sxs-lookup"><span data-stu-id="f523a-120">Then select a form to validate and call the `validate` method.</span></span>

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

<span data-ttu-id="f523a-121">Binden Sie alternativ dazu jQuery und das Plug-In über requirejs in Ihr Modul ein.</span><span class="sxs-lookup"><span data-stu-id="f523a-121">Alternatively include jQuery and the plugin via requirejs in your module.</span></span>

```js
define(["jquery", "jquery.validate"], function( $ ) {
    $("form").validate();
});
```

<span data-ttu-id="f523a-122">Weitere Informationen zum Einrichten von Regeln und Anpassungen [finden Sie in der Dokumentation](http://jqueryvalidation.org/documentation/).</span><span class="sxs-lookup"><span data-stu-id="f523a-122">For more information on how to setup a rules and customizations, [check the documentation](http://jqueryvalidation.org/documentation/).</span></span>

## <a name="reporting-issues-and-contributing-code"></a><span data-ttu-id="f523a-123">Melden von Problemen und Beitragen von Code</span><span class="sxs-lookup"><span data-stu-id="f523a-123">Reporting issues and contributing code</span></span>

<span data-ttu-id="f523a-124">Details finden Sie in den [Richtlinien zu Beiträgen](CONTRIBUTING.md).</span><span class="sxs-lookup"><span data-stu-id="f523a-124">See the [Contributing Guidelines](CONTRIBUTING.md) for details.</span></span>

<span data-ttu-id="f523a-125">**WICHTIGER HINWEIS ZUR E-MAIL-ÜBERPRÜFUNG**.</span><span class="sxs-lookup"><span data-stu-id="f523a-125">**IMPORTANT NOTE ABOUT EMAIL VALIDATION**.</span></span> <span data-ttu-id="f523a-126">Ab Version 1.12.0 verwendet dieses Plug-In den gleichen regulären Ausdruck, den die [HTML5-Spezifikation für die Verwendung durch Browser vorschlägt](https://html.spec.whatwg.org/multipage/forms.html#valid-e-mail-address).</span><span class="sxs-lookup"><span data-stu-id="f523a-126">As of version 1.12.0 this plugin is using the same regular expression that the [HTML5 specification suggests for browsers to use](https://html.spec.whatwg.org/multipage/forms.html#valid-e-mail-address).</span></span> <span data-ttu-id="f523a-127">Wir werden diesem Beispiel folgen und die gleiche Überprüfung verwenden.</span><span class="sxs-lookup"><span data-stu-id="f523a-127">We will follow their lead and use the same check.</span></span> <span data-ttu-id="f523a-128">Wenn Sie der Meinung sind, dass die Spezifikation falsch ist, melden Sie das Problem bitte dort.</span><span class="sxs-lookup"><span data-stu-id="f523a-128">If you think the specification is wrong, please report the issue to them.</span></span> <span data-ttu-id="f523a-129">Wenn Sie andere Anforderungen haben, ziehen Sie die [Verwendung einer benutzerdefinierten Methode](http://jqueryvalidation.org/jQuery.validator.addMethod/) in Betracht.</span><span class="sxs-lookup"><span data-stu-id="f523a-129">If you have different requirements, consider [using a custom method](http://jqueryvalidation.org/jQuery.validator.addMethod/).</span></span>

## <a name="license"></a><span data-ttu-id="f523a-130">Lizenz</span><span class="sxs-lookup"><span data-stu-id="f523a-130">License</span></span>
<span data-ttu-id="f523a-131">Copyright &copy; Jörn Zaefferer</span><span class="sxs-lookup"><span data-stu-id="f523a-131">Copyright &copy; Jörn Zaefferer</span></span><br>
<span data-ttu-id="f523a-132">Lizenziert unter der MIT-Lizenz.</span><span class="sxs-lookup"><span data-stu-id="f523a-132">Licensed under the MIT license.</span></span>
