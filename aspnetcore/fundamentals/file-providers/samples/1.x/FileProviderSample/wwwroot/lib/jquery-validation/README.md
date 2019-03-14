---
ms.openlocfilehash: 24c36493284988aa45b15c78e2577c0ef96aba56
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/01/2019
ms.locfileid: "57029987"
---
<a name="jquery-validation-pluginhttpsjqueryvalidationorg---form-validation-made-easy"></a>[jQuery Validation Plugin (jQuery-Überprüfungs-Plug-In):](https://jqueryvalidation.org/) Einfache Überprüfung von Formularen
================================

[![Buildstatus](https://secure.travis-ci.org/jquery-validation/jquery-validation.svg)](https://travis-ci.org/jquery-validation/jquery-validation)
[![devDependency-Status](https://david-dm.org/jquery-validation/jquery-validation/dev-status.svg?theme=shields.io)](https://david-dm.org/jquery-validation/jquery-validation#info=devDependencies)

Das jQuery-Überprüfungs-Plug-In bietet kurzfristige Überprüfung für Ihre vorhandenen Formulare und vereinfacht alle Arten von Anpassungen für Ihre Anwendung.

## <a name="getting-started"></a>Erste Schritte

### <a name="downloading-the-prebuilt-files"></a>Herunterladen der vorkompilierten Dateien

Vorkompilierte Dateien können hier heruntergeladen werden: https://jqueryvalidation.org/

### <a name="downloading-the-latest-changes"></a>Herunterladen der neuesten Änderungen

Die noch nicht veröffentlichten Entwicklungsdateien können wie folgt abgerufen werden:

 1. [Herunterladen](https://github.com/jquery-validation/jquery-validation/archive/master.zip) oder Forken dieses Repositorys
 2. [Einrichten des Builds](CONTRIBUTING.md#build-setup)
 3. Führen Sie `grunt` zum Erstellen der Builddateien im Verzeichnis „dist“ aus.

### <a name="including-it-on-your-page"></a>Einbinden des Plug-Ins auf Ihrer Seite

Binden Sie jQuery und das Plug-In auf einer Seite ein. Wählen Sie dann ein zu überprüfendes Formular aus, und rufen Sie die `validate`-Methode auf.

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

Binden Sie alternativ dazu jQuery und das Plug-In über requirejs in Ihr Modul ein.

```js
define(["jquery", "jquery.validate"], function( $ ) {
    $("form").validate();
});
```

Weitere Informationen zum Einrichten von Regeln und Anpassungen [finden Sie in der Dokumentation](https://jqueryvalidation.org/documentation/).

## <a name="reporting-issues-and-contributing-code"></a>Melden von Problemen und Beitragen von Code

Details finden Sie in den [Richtlinien zu Beiträgen](CONTRIBUTING.md).

**WICHTIGER HINWEIS ZUR E-MAIL-ÜBERPRÜFUNG**. Ab Version 1.12.0 verwendet dieses Plug-In den gleichen regulären Ausdruck, den die [HTML5-Spezifikation für die Verwendung durch Browser vorschlägt](https://html.spec.whatwg.org/multipage/forms.html#valid-e-mail-address). Wir werden diesem Beispiel folgen und die gleiche Überprüfung verwenden. Wenn Sie der Meinung sind, dass die Spezifikation falsch ist, melden Sie das Problem bitte dort. Wenn Sie andere Anforderungen haben, ziehen Sie die [Verwendung einer benutzerdefinierten Methode](https://jqueryvalidation.org/jQuery.validator.addMethod/) in Betracht.
Falls Sie die eingebauten Validierungsmuster für reguläre Ausdrücke anpassen müssen, folgen Sie bitte [ der Dokumentation](https://jqueryvalidation.org/jQuery.validator.methods/).

**WICHTIGER HINWEIS ZUR ERFORDERLICHEN METHODE**. Ab Version 1.14.0 beendet dieses Plugin das Kürzen von Leerzeichen aus dem Wert des angefügten Elements. Wenn Sie das gleiche Ergebnis erzielen wollen, können Sie [`normalizer`](https://jqueryvalidation.org/normalizer/) verwenden, um den Wert eines Elements vor der Validierung zu transformieren. Dieses Feature ist seit `v1.15.0` verfügbar. Das heißt, Sie haben diese Möglichkeiten:
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

## <a name="license"></a>Lizenz
Copyright &copy; Jörn Zaefferer<br>
Lizenziert unter der MIT-Lizenz.
