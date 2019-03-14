---
ms.openlocfilehash: 610b16b181422978dc143b95f28658bbdf177fdd
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/01/2019
ms.locfileid: "57028277"
---
# <a name="jquery"></a><span data-ttu-id="6f34c-101">jQuery</span><span class="sxs-lookup"><span data-stu-id="6f34c-101">jQuery</span></span>

> <span data-ttu-id="6f34c-102">jQuery ist eine schnelle, kleine und funktionsreiche JavaScript-Bibliothek.</span><span class="sxs-lookup"><span data-stu-id="6f34c-102">jQuery is a fast, small, and feature-rich JavaScript library.</span></span>

<span data-ttu-id="6f34c-103">Informationen zum Einstieg und der Verwendung von jQuery finden Sie in der [jQuery-Dokumentation](http://api.jquery.com/).</span><span class="sxs-lookup"><span data-stu-id="6f34c-103">For information on how to get started and how to use jQuery, please see [jQuery's documentation](http://api.jquery.com/).</span></span>
<span data-ttu-id="6f34c-104">Quelldateien und Probleme finden Sie im [jQuery-Repository](https://github.com/jquery/jquery).</span><span class="sxs-lookup"><span data-stu-id="6f34c-104">For source files and issues, please visit the [jQuery repo](https://github.com/jquery/jquery).</span></span>

<span data-ttu-id="6f34c-105">Wenn ein Upgrade durchführen, lesen Sie [Blogbeitrag 3.3.1](https://blog.jquery.com/2017/03/20/jquery-3.3.1-now-available/).</span><span class="sxs-lookup"><span data-stu-id="6f34c-105">If upgrading, please see the [blog post for 3.3.1](https://blog.jquery.com/2017/03/20/jquery-3.3.1-now-available/).</span></span> <span data-ttu-id="6f34c-106">Dies beinhaltet deutliche Unterschiede zur Vorgängerversion und ein besser lesbares Änderungsprotokoll.</span><span class="sxs-lookup"><span data-stu-id="6f34c-106">This includes notable differences from the previous version and a more readable changelog.</span></span>

## <a name="including-jquery"></a><span data-ttu-id="6f34c-107">Einschließlich von jQuery</span><span class="sxs-lookup"><span data-stu-id="6f34c-107">Including jQuery</span></span>

<span data-ttu-id="6f34c-108">Im folgenden finden Sie einige der am häufigsten verwendeten Methoden zum Einschließen von jQuery.</span><span class="sxs-lookup"><span data-stu-id="6f34c-108">Below are some of the most common ways to include jQuery.</span></span>

### <a name="browser"></a><span data-ttu-id="6f34c-109">Browser</span><span class="sxs-lookup"><span data-stu-id="6f34c-109">Browser</span></span>

#### <a name="script-tag"></a><span data-ttu-id="6f34c-110">Skripttag</span><span class="sxs-lookup"><span data-stu-id="6f34c-110">Script tag</span></span>

```html
<script src="https://code.jquery.com/jquery-3.3.1.min.js"></script>
```

#### <a name="babel"></a><span data-ttu-id="6f34c-111">Babel</span><span class="sxs-lookup"><span data-stu-id="6f34c-111">Babel</span></span>

<span data-ttu-id="6f34c-112">[Babel](http://babeljs.io/) ist ein JavaScript-Compiler der nächsten Generation.</span><span class="sxs-lookup"><span data-stu-id="6f34c-112">[Babel](http://babeljs.io/) is a next generation JavaScript compiler.</span></span> <span data-ttu-id="6f34c-113">Eines der Features ist die Möglichkeit, ES6/ES2015-Module zu verwenden, auch wenn Browser diese Funktion noch nicht nativ unterstützen.</span><span class="sxs-lookup"><span data-stu-id="6f34c-113">One of the features is the ability to use ES6/ES2015 modules now, even though browsers do not yet support this feature natively.</span></span>

```js
import $ from "jquery";
```

#### <a name="browserifywebpack"></a><span data-ttu-id="6f34c-114">Browserify/Webpack</span><span class="sxs-lookup"><span data-stu-id="6f34c-114">Browserify/Webpack</span></span>

<span data-ttu-id="6f34c-115">Es gibt mehrere Möglichkeiten, [Browserify](http://browserify.org/) und [Webpack](https://webpack.github.io/) zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="6f34c-115">There are several ways to use [Browserify](http://browserify.org/) and [Webpack](https://webpack.github.io/).</span></span> <span data-ttu-id="6f34c-116">Weitere Informationen zur Verwendung dieser Tools finden Sie in der Dokumentation des jeweiligen Projekts.</span><span class="sxs-lookup"><span data-stu-id="6f34c-116">For more information on using these tools, please refer to the corresponding project's documention.</span></span> <span data-ttu-id="6f34c-117">In dem Skript sieht das Einschließen von jQuery in der Regel wie folgt aus....</span><span class="sxs-lookup"><span data-stu-id="6f34c-117">In the script, including jQuery will usually look like this...</span></span>

```js
var $ = require("jquery");
```

#### <a name="amd-asynchronous-module-definition"></a><span data-ttu-id="6f34c-118">AMD (asynchrone Moduldefinition)</span><span class="sxs-lookup"><span data-stu-id="6f34c-118">AMD (Asynchronous Module Definition)</span></span>

<span data-ttu-id="6f34c-119">AMD ist ein für den Browser erstelltes Modulformat.</span><span class="sxs-lookup"><span data-stu-id="6f34c-119">AMD is a module format built for the browser.</span></span> <span data-ttu-id="6f34c-120">Für weitere Informationen empfehlen wir die [require.js-Dokumentation](http://requirejs.org/docs/whyamd.html).</span><span class="sxs-lookup"><span data-stu-id="6f34c-120">For more information, we recommend [require.js' documentation](http://requirejs.org/docs/whyamd.html).</span></span>

```js
define(["jquery"], function($) {

});
```

### <a name="node"></a><span data-ttu-id="6f34c-121">Knoten</span><span class="sxs-lookup"><span data-stu-id="6f34c-121">Node</span></span>

<span data-ttu-id="6f34c-122">Um jQuery in einem [Node](nodejs.org) einzuschließen, installieren Sie zunächst npm.</span><span class="sxs-lookup"><span data-stu-id="6f34c-122">To include jQuery in [Node](nodejs.org), first install with npm.</span></span>

```sh
npm install jquery
```

<span data-ttu-id="6f34c-123">Damit jQuery in Node funktioniert, ist ein Fenster mit einem Dokument erforderlich.</span><span class="sxs-lookup"><span data-stu-id="6f34c-123">For jQuery to work in Node, a window with a document is required.</span></span> <span data-ttu-id="6f34c-124">Da kein solches Fenster nativ in Node vorhanden ist, kann es von Tools wie z.B. [Jsdom](https://github.com/tmpvar/jsdom) modelliert werden.</span><span class="sxs-lookup"><span data-stu-id="6f34c-124">Since no such window exists natively in Node, one can be mocked by tools such as [jsdom](https://github.com/tmpvar/jsdom).</span></span> <span data-ttu-id="6f34c-125">Dies kann zu Testzwecken nützlich sein.</span><span class="sxs-lookup"><span data-stu-id="6f34c-125">This can be useful for testing purposes.</span></span>

```js
require("jsdom").env("", function(err, window) {
    if (err) {
        console.error(err);
        return;
    }

    var $ = require("jquery")(window);
});
```
