---
ms.openlocfilehash: 1b563a8c0674d51f893415221ca8fab574b78265
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/01/2019
ms.locfileid: "57032947"
---
<a name="--"></a>--
================================

<span data-ttu-id="8b593-101">[![Buildstatus](https://secure.travis-ci.org/jzaefferer/jquery-validation.png)](http://travis-ci.org/jzaefferer/jquery-validation)
[![devDependency-Status](https://david-dm.org/jzaefferer/jquery-validation/dev-status.png?theme=shields.io)](https://david-dm.org/jzaefferer/jquery-validation#info=devDependencies)</span><span class="sxs-lookup"><span data-stu-id="8b593-101">[![Build Status](https://secure.travis-ci.org/jzaefferer/jquery-validation.png)](http://travis-ci.org/jzaefferer/jquery-validation)
[![devDependency Status](https://david-dm.org/jzaefferer/jquery-validation/dev-status.png?theme=shields.io)](https://david-dm.org/jzaefferer/jquery-validation#info=devDependencies)</span></span>

<span data-ttu-id="8b593-102">Das jQuery-Überprüfungs-Plug-In bietet kurzfristige Überprüfung für Ihre vorhandenen Formulare und vereinfacht alle Arten von Anpassungen für Ihre Anwendung.</span><span class="sxs-lookup"><span data-stu-id="8b593-102">The jQuery Validation Plugin provides drop-in validation for your existing forms, while making all kinds of customizations to fit your application really easy.</span></span>

## <a name="help-the-projecthttppledgiecomcampaigns18159"></a>[<span data-ttu-id="8b593-103">Unterstützen Sie das Projekt</span><span class="sxs-lookup"><span data-stu-id="8b593-103">Help the project</span></span>](http://pledgie.com/campaigns/18159)

<span data-ttu-id="8b593-104">[![Unterstützen Sie das Projekt](http://www.pledgie.com/campaigns/18159.png?skin_name=chrome)](http://pledgie.com/campaigns/18159)</span><span class="sxs-lookup"><span data-stu-id="8b593-104">[![Help the project](http://www.pledgie.com/campaigns/18159.png?skin_name=chrome)](http://pledgie.com/campaigns/18159)</span></span>

<span data-ttu-id="8b593-105">Dieses Projekt benötigt Unterstützung!</span><span class="sxs-lookup"><span data-stu-id="8b593-105">This project is looking for help!</span></span> <span data-ttu-id="8b593-106">[Sie können mit einer Spende die aktuelle Pledgie-Kampagne unterstützen](http://pledgie.com/campaigns/18159) und dafür sorgen, dass dem Projekt größere Aufmerksamkeit zuteil wird.</span><span class="sxs-lookup"><span data-stu-id="8b593-106">[You can donate to the ongoing pledgie campaign](http://pledgie.com/campaigns/18159) and help spread the word.</span></span> <span data-ttu-id="8b593-107">Wenn Sie das Plug-In bereits verwendet haben oder seine Verwendung planen, sollten Sie eine Spende in Betracht ziehen. Jeder Betrag ist willkommen.</span><span class="sxs-lookup"><span data-stu-id="8b593-107">If you've used the plugin, or plan to use, consider a donation - any amount will help.</span></span>

<span data-ttu-id="8b593-108">Sie finden den Plan zur Verwendung der Spenden auf der [Pledgie-Seite](http://pledgie.com/campaigns/18159).</span><span class="sxs-lookup"><span data-stu-id="8b593-108">You can find the plan for how to spend the money on the [pledgie page](http://pledgie.com/campaigns/18159).</span></span>

## <a name="get-started"></a><span data-ttu-id="8b593-109">Erste Schritte</span><span class="sxs-lookup"><span data-stu-id="8b593-109">Get Started</span></span>

### <a name="downloading-the-prebuilt-files"></a><span data-ttu-id="8b593-110">Herunterladen der vorkompilierten Dateien</span><span class="sxs-lookup"><span data-stu-id="8b593-110">Downloading the prebuilt files</span></span>

<span data-ttu-id="8b593-111">Vorkompilierte Dateien können hier heruntergeladen werden: http://jqueryvalidation.org/</span><span class="sxs-lookup"><span data-stu-id="8b593-111">Prebuilt files can be downloaded from http://jqueryvalidation.org/</span></span>

### <a name="downloading-the-latest-changes"></a><span data-ttu-id="8b593-112">Herunterladen der neuesten Änderungen</span><span class="sxs-lookup"><span data-stu-id="8b593-112">Downloading the latest changes</span></span>

<span data-ttu-id="8b593-113">Die noch nicht veröffentlichten Entwicklungsdateien können wie folgt abgerufen werden:</span><span class="sxs-lookup"><span data-stu-id="8b593-113">The unreleased development files can be obtained by:</span></span>

 1. <span data-ttu-id="8b593-114">[Herunterladen](https://github.com/jzaefferer/jquery-validation/archive/master.zip) oder Forken dieses Repositorys</span><span class="sxs-lookup"><span data-stu-id="8b593-114">[Downloading](https://github.com/jzaefferer/jquery-validation/archive/master.zip) or Forking this repository</span></span>
 2. [<span data-ttu-id="8b593-115">Einrichten des Builds</span><span class="sxs-lookup"><span data-stu-id="8b593-115">Setup the build</span></span>](CONTRIBUTING.md#build-setup)
 3. <span data-ttu-id="8b593-116">Führen Sie `grunt` zum Erstellen der Builddateien im Verzeichnis „dist“ aus.</span><span class="sxs-lookup"><span data-stu-id="8b593-116">Run `grunt` to create the built files in the "dist" directory</span></span>

### <a name="including-it-on-your-page"></a><span data-ttu-id="8b593-117">Einbinden des Plug-Ins auf Ihrer Seite</span><span class="sxs-lookup"><span data-stu-id="8b593-117">Including it on your page</span></span>

<span data-ttu-id="8b593-118">Binden Sie jQuery und das Plug-In auf einer Seite ein.</span><span class="sxs-lookup"><span data-stu-id="8b593-118">Include jQuery and the plugin on a page.</span></span> <span data-ttu-id="8b593-119">Wählen Sie dann ein zu überprüfendes Formular aus, und rufen Sie die `validate`-Methode auf.</span><span class="sxs-lookup"><span data-stu-id="8b593-119">Then select a form to validate and call the `validate` method.</span></span>

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

<span data-ttu-id="8b593-120">Binden Sie alternativ dazu jQuery und das Plug-In über requirejs in Ihr Modul ein.</span><span class="sxs-lookup"><span data-stu-id="8b593-120">Alternatively include jQuery and the plugin via requirejs in your module.</span></span>

```js
define(["jquery", "jquery.validate"], function( $ ) {
    $("form").validate();
});
```

<span data-ttu-id="8b593-121">Weitere Informationen zum Einrichten von Regeln und Anpassungen [finden Sie in der Dokumentation](http://jqueryvalidation.org/documentation/).</span><span class="sxs-lookup"><span data-stu-id="8b593-121">For more information on how to setup a rules and customizations, [check the documentation](http://jqueryvalidation.org/documentation/).</span></span>

## <a name="reporting-issues-and-contributing-code"></a><span data-ttu-id="8b593-122">Melden von Problemen und Beitragen von Code</span><span class="sxs-lookup"><span data-stu-id="8b593-122">Reporting issues and contributing code</span></span>

<span data-ttu-id="8b593-123">Details finden Sie in den [Richtlinien zu Beiträgen](CONTRIBUTING.md).</span><span class="sxs-lookup"><span data-stu-id="8b593-123">See the [Contributing Guidelines](CONTRIBUTING.md) for details.</span></span>

<span data-ttu-id="8b593-124">**WICHTIGER HINWEIS ZUR E-MAIL-ÜBERPRÜFUNG**.</span><span class="sxs-lookup"><span data-stu-id="8b593-124">**IMPORTANT NOTE ABOUT EMAIL VALIDATION**.</span></span> <span data-ttu-id="8b593-125">Ab Version 1.12.0 verwendet dieses Plug-In den gleichen regulären Ausdruck, den die [HTML5-Spezifikation für die Verwendung durch Browser vorschlägt](https://html.spec.whatwg.org/multipage/forms.html#valid-e-mail-address).</span><span class="sxs-lookup"><span data-stu-id="8b593-125">As of version 1.12.0 this plugin is using the same regular expression that the [HTML5 specification suggests for browsers to use](https://html.spec.whatwg.org/multipage/forms.html#valid-e-mail-address).</span></span> <span data-ttu-id="8b593-126">Wir werden diesem Beispiel folgen und die gleiche Überprüfung verwenden.</span><span class="sxs-lookup"><span data-stu-id="8b593-126">We will follow their lead and use the same check.</span></span> <span data-ttu-id="8b593-127">Wenn Sie der Meinung sind, dass die Spezifikation falsch ist, melden Sie das Problem bitte dort.</span><span class="sxs-lookup"><span data-stu-id="8b593-127">If you think the specification is wrong, please report the issue to them.</span></span> <span data-ttu-id="8b593-128">Wenn Sie andere Anforderungen haben, ziehen Sie die [Verwendung einer benutzerdefinierten Methode](http://jqueryvalidation.org/jQuery.validator.addMethod/) in Betracht.</span><span class="sxs-lookup"><span data-stu-id="8b593-128">If you have different requirements, consider [using a custom method](http://jqueryvalidation.org/jQuery.validator.addMethod/).</span></span>

## <a name="license"></a><span data-ttu-id="8b593-129">Lizenz</span><span class="sxs-lookup"><span data-stu-id="8b593-129">License</span></span>
<span data-ttu-id="8b593-130">Copyright &copy; Jörn Zaefferer</span><span class="sxs-lookup"><span data-stu-id="8b593-130">Copyright &copy; Jörn Zaefferer</span></span><br>
<span data-ttu-id="8b593-131">Lizenziert unter der MIT-Lizenz.</span><span class="sxs-lookup"><span data-stu-id="8b593-131">Licensed under the MIT license.</span></span>
