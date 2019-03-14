---
ms.openlocfilehash: 3965884c8e0d7116f5d8a42e6aefb0dc5be94f1e
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/01/2019
ms.locfileid: "57029657"
---
<a name="open-iconic-v111httpuseiconiccomopen"></a>[<span data-ttu-id="78c21-101">Open Iconic v1.1.1</span><span class="sxs-lookup"><span data-stu-id="78c21-101">Open Iconic v1.1.1</span></span>](http://useiconic.com/open)
===========

### <a name="open-iconic-is-the-open-source-sibling-of-iconichttpuseiconiccom-it-is-a-hyper-legible-collection-of-223-icons-with-a-tiny-footprintmdashready-to-use-with-bootstrap-and-foundation-view-the-collectionhttpuseiconiccomopenicons"></a><span data-ttu-id="78c21-102">Open Iconic ist das Pendant zu [Iconic](http://useiconic.com) in Open Source.</span><span class="sxs-lookup"><span data-stu-id="78c21-102">Open Iconic is the open source sibling of [Iconic](http://useiconic.com).</span></span> <span data-ttu-id="78c21-103">Es ist eine Sammlung von 223 extrem lesbaren Symbolen mit geringem Ressourcenbedarf&mdash;bereit zur Verwendung mit Bootstrap und Foundation.</span><span class="sxs-lookup"><span data-stu-id="78c21-103">It is a hyper-legible collection of 223 icons with a tiny footprint&mdash;ready to use with Bootstrap and Foundation.</span></span> [<span data-ttu-id="78c21-104">Sammlung anzeigen</span><span class="sxs-lookup"><span data-stu-id="78c21-104">View the collection</span></span>](http://useiconic.com/open#icons)



## <a name="whats-in-open-iconic"></a><span data-ttu-id="78c21-105">Was ist in Open Iconic enthalten?</span><span class="sxs-lookup"><span data-stu-id="78c21-105">What's in Open Iconic?</span></span>

* <span data-ttu-id="78c21-106">223 bis hinunter zu 8 Pixeln lesbare Symbole</span><span class="sxs-lookup"><span data-stu-id="78c21-106">223 icons designed to be legible down to 8 pixels</span></span>
* <span data-ttu-id="78c21-107">Extrem kompakte SVG-Dateien – 61,8 für den gesamten Satz</span><span class="sxs-lookup"><span data-stu-id="78c21-107">Super-light SVG files - 61.8 for the entire set</span></span> 
* <span data-ttu-id="78c21-108">SVG-Sprite&mdash;der moderne Ersatz für Symbolschriftarten</span><span class="sxs-lookup"><span data-stu-id="78c21-108">SVG sprite&mdash;the modern replacement for icon fonts</span></span>
* <span data-ttu-id="78c21-109">Webfont- (EOT, OTF, SVG, TTF, WOFF), PNG- und WebP-Formate</span><span class="sxs-lookup"><span data-stu-id="78c21-109">Webfont (EOT, OTF, SVG, TTF, WOFF), PNG and WebP formats</span></span>
* <span data-ttu-id="78c21-110">Webfontstylesheets (einschließlich der Versionen für Bootstrap und Foundation) in CSS-, LESS-, SCSS- und Stylus-Formaten</span><span class="sxs-lookup"><span data-stu-id="78c21-110">Webfont stylesheets (including versions for Bootstrap and Foundation) in CSS, LESS, SCSS and Stylus formats</span></span>
* <span data-ttu-id="78c21-111">PNG- und WebP-Rasterbilder in 8px, 16px, 24px, 32px, 48px und 64px.</span><span class="sxs-lookup"><span data-stu-id="78c21-111">PNG and WebP raster images in 8px, 16px, 24px, 32px, 48px and 64px.</span></span>


## <a name="getting-started"></a><span data-ttu-id="78c21-112">Erste Schritte</span><span class="sxs-lookup"><span data-stu-id="78c21-112">Getting Started</span></span>

#### <a name="for-code-samples-and-everything-else-you-need-to-get-started-with-open-iconic-check-out-our-iconshttpuseiconiccomopenicons-and-referencehttpuseiconiccomopenreference-sections"></a><span data-ttu-id="78c21-113">Codebeispiele und was Sie sonst noch zum Einstieg in Open Iconic benötigen, finden Sie in den Abschnitten zu [Symbolen](http://useiconic.com/open#icons) und [Referenz](http://useiconic.com/open#reference).</span><span class="sxs-lookup"><span data-stu-id="78c21-113">For code samples and everything else you need to get started with Open Iconic, check out our [Icons](http://useiconic.com/open#icons) and [Reference](http://useiconic.com/open#reference) sections.</span></span>

### <a name="general-usage"></a><span data-ttu-id="78c21-114">Allgemeine Verwendung</span><span class="sxs-lookup"><span data-stu-id="78c21-114">General Usage</span></span>

#### <a name="using-open-iconics-svgs"></a><span data-ttu-id="78c21-115">Verwenden der SVGs von Open Iconic</span><span class="sxs-lookup"><span data-stu-id="78c21-115">Using Open Iconic's SVGs</span></span>

<span data-ttu-id="78c21-116">Wir mögen SVGs und glauben, dass sie die ideale Möglichkeit zum Anzeigen von Symbolen im Internet sind.</span><span class="sxs-lookup"><span data-stu-id="78c21-116">We like SVGs and we think they're the way to display icons on the web.</span></span> <span data-ttu-id="78c21-117">Da Open Iconic im Wesentlichen aus SVGs besteht, sollten Sie sie so anzeigen wie jedes andere Bild (vergessen Sie nicht das `alt`-Attribut).</span><span class="sxs-lookup"><span data-stu-id="78c21-117">Since Open Iconic are just basic SVGs, we suggest you display them like you would any other image (don't forget the `alt` attribute).</span></span>

```
<img src="/open-iconic/svg/icon-name.svg" alt="icon name">
```

#### <a name="using-open-iconics-svg-sprite"></a><span data-ttu-id="78c21-118">Verwenden des SVG-Sprite von Open Iconic</span><span class="sxs-lookup"><span data-stu-id="78c21-118">Using Open Iconic's SVG Sprite</span></span>

<span data-ttu-id="78c21-119">Open Iconic ist auch ein SVG-Sprite, sodass Sie alle Symbole in der Gruppe mit einer einzelnen Anforderung anzeigen können.</span><span class="sxs-lookup"><span data-stu-id="78c21-119">Open Iconic also comes in a SVG sprite which allows you to display all the icons in the set with a single request.</span></span> <span data-ttu-id="78c21-120">Es ist wie eine Symbolschriftart, ohne Nachteile mit sich zu bringen.</span><span class="sxs-lookup"><span data-stu-id="78c21-120">It's like an icon font, without being a hack.</span></span>

<span data-ttu-id="78c21-121">Das Hinzufügen eines Symbols aus einem SVG-Sprite ist ein wenig anders, als Sie es gewöhnt sind, aber immer noch kinderleicht.</span><span class="sxs-lookup"><span data-stu-id="78c21-121">Adding an icon from an SVG sprite is a little different than what you're used to, but it's still a piece of cake.</span></span> <span data-ttu-id="78c21-122">*Tipp: Um die Symbole einfach als Stil verwenden zu können, sollten Sie dem* `<svg>`*-Tag eine allgemeine Klasse und einen eindeutigen Klassennamen für jedes andere Symbol im* `<use>`*-Tag* hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="78c21-122">*Tip: To make your icons easily style able, we suggest adding a general class to the* `<svg>` *tag and a unique class name for each different icon in the* `<use>` *tag.*</span></span>  

```
<svg class="icon">
  <use xlink:href="open-iconic.svg#account-login" class="icon-account-login"></use>
</svg>
```

<span data-ttu-id="78c21-123">Für die Skalierung der Symbole ist nur grundlegendes CSS erforderlich.</span><span class="sxs-lookup"><span data-stu-id="78c21-123">Sizing icons only needs basic CSS.</span></span> <span data-ttu-id="78c21-124">Alle Symbole liegen in einem quadratischen Format vor, also legen Sie einfach das `<svg>`-Tag mit gleichem Breiten- und Höhenmaß fest.</span><span class="sxs-lookup"><span data-stu-id="78c21-124">All the icons are in a square format, so just set the `<svg>` tag with equal width and height dimensions.</span></span>

```
.icon {
  width: 16px;
  height: 16px;
}
```

<span data-ttu-id="78c21-125">Das Einfärben der Symbole ist sogar noch einfacher.</span><span class="sxs-lookup"><span data-stu-id="78c21-125">Coloring icons is even easier.</span></span> <span data-ttu-id="78c21-126">Sie müssen nur die `fill`-Regel auf dem `<use>`-Tag festlegen.</span><span class="sxs-lookup"><span data-stu-id="78c21-126">All you need to do is set the `fill` rule on the `<use>` tag.</span></span>

```
.icon-account-login {
  fill: #f00;
}
```

<span data-ttu-id="78c21-127">Weitere Informationen zu SVG-Sprites finden Sie in dem [Handbuch von Chris Coyier](http://css-tricks.com/svg-sprites-use-better-icon-fonts/).</span><span class="sxs-lookup"><span data-stu-id="78c21-127">To learn more about SVG Sprites, read [Chris Coyier's guide](http://css-tricks.com/svg-sprites-use-better-icon-fonts/).</span></span>

#### <a name="using-open-iconics-icon-font"></a><span data-ttu-id="78c21-128">Verwenden der Symbolschriftart von Open Iconic...</span><span class="sxs-lookup"><span data-stu-id="78c21-128">Using Open Iconic's Icon Font...</span></span>


##### <a name="with-bootstrap"></a><span data-ttu-id="78c21-129">…mit Bootstrap</span><span class="sxs-lookup"><span data-stu-id="78c21-129">…with Bootstrap</span></span>

<span data-ttu-id="78c21-130">Sie finden unsere Bootstrap-Stylesheets in `font/css/open-iconic-bootstrap.{css, less, scss, styl}`</span><span class="sxs-lookup"><span data-stu-id="78c21-130">You can find our Bootstrap stylesheets in `font/css/open-iconic-bootstrap.{css, less, scss, styl}`</span></span>


```
<link href="/open-iconic/font/css/open-iconic-bootstrap.css" rel="stylesheet">
```


```
<span class="oi oi-icon-name" title="icon name" aria-hidden="true"></span>
```

##### <a name="with-foundation"></a><span data-ttu-id="78c21-131">…mit Foundation</span><span class="sxs-lookup"><span data-stu-id="78c21-131">…with Foundation</span></span>

<span data-ttu-id="78c21-132">Sie finden unsere Foundation-Stylesheets in `font/css/open-iconic-foundation.{css, less, scss, styl}`</span><span class="sxs-lookup"><span data-stu-id="78c21-132">You can find our Foundation stylesheets in `font/css/open-iconic-foundation.{css, less, scss, styl}`</span></span>

```
<link href="/open-iconic/font/css/open-iconic-foundation.css" rel="stylesheet">
```


```
<span class="fi-icon-name" title="icon name" aria-hidden="true"></span>
```

##### <a name="on-its-own"></a><span data-ttu-id="78c21-133">...eigenständig</span><span class="sxs-lookup"><span data-stu-id="78c21-133">…on its own</span></span>

<span data-ttu-id="78c21-134">Sie finden unsere Standardstylesheets in `font/css/open-iconic.{css, less, scss, styl}`</span><span class="sxs-lookup"><span data-stu-id="78c21-134">You can find our default stylesheets in `font/css/open-iconic.{css, less, scss, styl}`</span></span>

```
<link href="/open-iconic/font/css/open-iconic.css" rel="stylesheet">
```

```
<span class="oi" data-glyph="icon-name" title="icon name" aria-hidden="true"></span>
```


## <a name="license"></a><span data-ttu-id="78c21-135">Lizenz</span><span class="sxs-lookup"><span data-stu-id="78c21-135">License</span></span>

### <a name="icons"></a><span data-ttu-id="78c21-136">Symbole</span><span class="sxs-lookup"><span data-stu-id="78c21-136">Icons</span></span>

<span data-ttu-id="78c21-137">Der gesamte Code (einschließlich SVG-Markup) unterliegt der [MIT-Lizenz](http://opensource.org/licenses/MIT).</span><span class="sxs-lookup"><span data-stu-id="78c21-137">All code (including SVG markup) is under the [MIT License](http://opensource.org/licenses/MIT).</span></span>

### <a name="fonts"></a><span data-ttu-id="78c21-138">Schriftarten</span><span class="sxs-lookup"><span data-stu-id="78c21-138">Fonts</span></span>

<span data-ttu-id="78c21-139">Alle Schriftarten sind [SIL-lizenziert](http://scripts.sil.org/cms/scripts/page.php?item_id=OFL_web).</span><span class="sxs-lookup"><span data-stu-id="78c21-139">All fonts are under the [SIL Licensed](http://scripts.sil.org/cms/scripts/page.php?item_id=OFL_web).</span></span>
