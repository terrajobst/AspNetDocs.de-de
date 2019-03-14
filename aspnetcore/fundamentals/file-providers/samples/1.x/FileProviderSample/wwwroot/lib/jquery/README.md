---
ms.openlocfilehash: 610b16b181422978dc143b95f28658bbdf177fdd
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/01/2019
ms.locfileid: "57028277"
---
# <a name="jquery"></a>jQuery

> jQuery ist eine schnelle, kleine und funktionsreiche JavaScript-Bibliothek.

Informationen zum Einstieg und der Verwendung von jQuery finden Sie in der [jQuery-Dokumentation](http://api.jquery.com/).
Quelldateien und Probleme finden Sie im [jQuery-Repository](https://github.com/jquery/jquery).

Wenn ein Upgrade durchführen, lesen Sie [Blogbeitrag 3.3.1](https://blog.jquery.com/2017/03/20/jquery-3.3.1-now-available/). Dies beinhaltet deutliche Unterschiede zur Vorgängerversion und ein besser lesbares Änderungsprotokoll.

## <a name="including-jquery"></a>Einschließlich von jQuery

Im folgenden finden Sie einige der am häufigsten verwendeten Methoden zum Einschließen von jQuery.

### <a name="browser"></a>Browser

#### <a name="script-tag"></a>Skripttag

```html
<script src="https://code.jquery.com/jquery-3.3.1.min.js"></script>
```

#### <a name="babel"></a>Babel

[Babel](http://babeljs.io/) ist ein JavaScript-Compiler der nächsten Generation. Eines der Features ist die Möglichkeit, ES6/ES2015-Module zu verwenden, auch wenn Browser diese Funktion noch nicht nativ unterstützen.

```js
import $ from "jquery";
```

#### <a name="browserifywebpack"></a>Browserify/Webpack

Es gibt mehrere Möglichkeiten, [Browserify](http://browserify.org/) und [Webpack](https://webpack.github.io/) zu verwenden. Weitere Informationen zur Verwendung dieser Tools finden Sie in der Dokumentation des jeweiligen Projekts. In dem Skript sieht das Einschließen von jQuery in der Regel wie folgt aus....

```js
var $ = require("jquery");
```

#### <a name="amd-asynchronous-module-definition"></a>AMD (asynchrone Moduldefinition)

AMD ist ein für den Browser erstelltes Modulformat. Für weitere Informationen empfehlen wir die [require.js-Dokumentation](http://requirejs.org/docs/whyamd.html).

```js
define(["jquery"], function($) {

});
```

### <a name="node"></a>Knoten

Um jQuery in einem [Node](nodejs.org) einzuschließen, installieren Sie zunächst npm.

```sh
npm install jquery
```

Damit jQuery in Node funktioniert, ist ein Fenster mit einem Dokument erforderlich. Da kein solches Fenster nativ in Node vorhanden ist, kann es von Tools wie z.B. [Jsdom](https://github.com/tmpvar/jsdom) modelliert werden. Dies kann zu Testzwecken nützlich sein.

```js
require("jsdom").env("", function(err, window) {
    if (err) {
        console.error(err);
        return;
    }

    var $ = require("jquery")(window);
});
```
