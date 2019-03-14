---
ms.openlocfilehash: 3965884c8e0d7116f5d8a42e6aefb0dc5be94f1e
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/01/2019
ms.locfileid: "57064847"
---
<a name="open-iconic-v111httpuseiconiccomopen"></a>[Open Iconic v1.1.1](http://useiconic.com/open)
===========

### <a name="open-iconic-is-the-open-source-sibling-of-iconichttpuseiconiccom-it-is-a-hyper-legible-collection-of-223-icons-with-a-tiny-footprintmdashready-to-use-with-bootstrap-and-foundation-view-the-collectionhttpuseiconiccomopenicons"></a>Open Iconic ist das Pendant zu [Iconic](http://useiconic.com) in Open Source. Es ist eine Sammlung von 223 extrem lesbaren Symbolen mit geringem Ressourcenbedarf&mdash;bereit zur Verwendung mit Bootstrap und Foundation. [Sammlung anzeigen](http://useiconic.com/open#icons)



## <a name="whats-in-open-iconic"></a>Was ist in Open Iconic enthalten?

* 223 bis hinunter zu 8 Pixeln lesbare Symbole
* Extrem kompakte SVG-Dateien – 61,8 für den gesamten Satz 
* SVG-Sprite&mdash;der moderne Ersatz für Symbolschriftarten
* Webfont- (EOT, OTF, SVG, TTF, WOFF), PNG- und WebP-Formate
* Webfontstylesheets (einschließlich der Versionen für Bootstrap und Foundation) in CSS-, LESS-, SCSS- und Stylus-Formaten
* PNG- und WebP-Rasterbilder in 8px, 16px, 24px, 32px, 48px und 64px.


## <a name="getting-started"></a>Erste Schritte

#### <a name="for-code-samples-and-everything-else-you-need-to-get-started-with-open-iconic-check-out-our-iconshttpuseiconiccomopenicons-and-referencehttpuseiconiccomopenreference-sections"></a>Codebeispiele und was Sie sonst noch zum Einstieg in Open Iconic benötigen, finden Sie in den Abschnitten zu [Symbolen](http://useiconic.com/open#icons) und [Referenz](http://useiconic.com/open#reference).

### <a name="general-usage"></a>Allgemeine Verwendung

#### <a name="using-open-iconics-svgs"></a>Verwenden der SVGs von Open Iconic

Wir mögen SVGs und glauben, dass sie die ideale Möglichkeit zum Anzeigen von Symbolen im Internet sind. Da Open Iconic im Wesentlichen aus SVGs besteht, sollten Sie sie so anzeigen wie jedes andere Bild (vergessen Sie nicht das `alt`-Attribut).

```
<img src="/open-iconic/svg/icon-name.svg" alt="icon name">
```

#### <a name="using-open-iconics-svg-sprite"></a>Verwenden des SVG-Sprite von Open Iconic

Open Iconic ist auch ein SVG-Sprite, sodass Sie alle Symbole in der Gruppe mit einer einzelnen Anforderung anzeigen können. Es ist wie eine Symbolschriftart, ohne Nachteile mit sich zu bringen.

Das Hinzufügen eines Symbols aus einem SVG-Sprite ist ein wenig anders, als Sie es gewöhnt sind, aber immer noch kinderleicht. *Tipp: Um die Symbole einfach als Stil verwenden zu können, sollten Sie dem* `<svg>`*-Tag eine allgemeine Klasse und einen eindeutigen Klassennamen für jedes andere Symbol im* `<use>`*-Tag* hinzufügen.  

```
<svg class="icon">
  <use xlink:href="open-iconic.svg#account-login" class="icon-account-login"></use>
</svg>
```

Für die Skalierung der Symbole ist nur grundlegendes CSS erforderlich. Alle Symbole liegen in einem quadratischen Format vor, also legen Sie einfach das `<svg>`-Tag mit gleichem Breiten- und Höhenmaß fest.

```
.icon {
  width: 16px;
  height: 16px;
}
```

Das Einfärben der Symbole ist sogar noch einfacher. Sie müssen nur die `fill`-Regel auf dem `<use>`-Tag festlegen.

```
.icon-account-login {
  fill: #f00;
}
```

Weitere Informationen zu SVG-Sprites finden Sie in dem [Handbuch von Chris Coyier](http://css-tricks.com/svg-sprites-use-better-icon-fonts/).

#### <a name="using-open-iconics-icon-font"></a>Verwenden der Symbolschriftart von Open Iconic...


##### <a name="with-bootstrap"></a>…mit Bootstrap

Sie finden unsere Bootstrap-Stylesheets in `font/css/open-iconic-bootstrap.{css, less, scss, styl}`


```
<link href="/open-iconic/font/css/open-iconic-bootstrap.css" rel="stylesheet">
```


```
<span class="oi oi-icon-name" title="icon name" aria-hidden="true"></span>
```

##### <a name="with-foundation"></a>…mit Foundation

Sie finden unsere Foundation-Stylesheets in `font/css/open-iconic-foundation.{css, less, scss, styl}`

```
<link href="/open-iconic/font/css/open-iconic-foundation.css" rel="stylesheet">
```


```
<span class="fi-icon-name" title="icon name" aria-hidden="true"></span>
```

##### <a name="on-its-own"></a>...eigenständig

Sie finden unsere Standardstylesheets in `font/css/open-iconic.{css, less, scss, styl}`

```
<link href="/open-iconic/font/css/open-iconic.css" rel="stylesheet">
```

```
<span class="oi" data-glyph="icon-name" title="icon name" aria-hidden="true"></span>
```


## <a name="license"></a>Lizenz

### <a name="icons"></a>Symbole

Der gesamte Code (einschließlich SVG-Markup) unterliegt der [MIT-Lizenz](http://opensource.org/licenses/MIT).

### <a name="fonts"></a>Schriftarten

Alle Schriftarten sind [SIL-lizenziert](http://scripts.sil.org/cms/scripts/page.php?item_id=OFL_web).
