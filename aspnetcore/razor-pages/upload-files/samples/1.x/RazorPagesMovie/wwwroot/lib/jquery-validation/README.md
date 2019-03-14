---
ms.openlocfilehash: 1b563a8c0674d51f893415221ca8fab574b78265
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/01/2019
ms.locfileid: "57046547"
---
<a name="--"></a>--
================================

[![Buildstatus](https://secure.travis-ci.org/jzaefferer/jquery-validation.png)](http://travis-ci.org/jzaefferer/jquery-validation)
[![devDependency-Status](https://david-dm.org/jzaefferer/jquery-validation/dev-status.png?theme=shields.io)](https://david-dm.org/jzaefferer/jquery-validation#info=devDependencies)

Das jQuery-Überprüfungs-Plug-In bietet kurzfristige Überprüfung für Ihre vorhandenen Formulare und vereinfacht alle Arten von Anpassungen für Ihre Anwendung.

## <a name="help-the-projecthttppledgiecomcampaigns18159"></a>[Unterstützen Sie das Projekt](http://pledgie.com/campaigns/18159)

[![Unterstützen Sie das Projekt](http://www.pledgie.com/campaigns/18159.png?skin_name=chrome)](http://pledgie.com/campaigns/18159)

Dieses Projekt benötigt Unterstützung! [Sie können mit einer Spende die aktuelle Pledgie-Kampagne unterstützen](http://pledgie.com/campaigns/18159) und dafür sorgen, dass dem Projekt größere Aufmerksamkeit zuteil wird. Wenn Sie das Plug-In bereits verwendet haben oder seine Verwendung planen, sollten Sie eine Spende in Betracht ziehen. Jeder Betrag ist willkommen.

Sie finden den Plan zur Verwendung der Spenden auf der [Pledgie-Seite](http://pledgie.com/campaigns/18159).

## <a name="get-started"></a>Erste Schritte

### <a name="downloading-the-prebuilt-files"></a>Herunterladen der vorkompilierten Dateien

Vorkompilierte Dateien können hier heruntergeladen werden: http://jqueryvalidation.org/

### <a name="downloading-the-latest-changes"></a>Herunterladen der neuesten Änderungen

Die noch nicht veröffentlichten Entwicklungsdateien können wie folgt abgerufen werden:

 1. [Herunterladen](https://github.com/jzaefferer/jquery-validation/archive/master.zip) oder Forken dieses Repositorys
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

Weitere Informationen zum Einrichten von Regeln und Anpassungen [finden Sie in der Dokumentation](http://jqueryvalidation.org/documentation/).

## <a name="reporting-issues-and-contributing-code"></a>Melden von Problemen und Beitragen von Code

Details finden Sie in den [Richtlinien zu Beiträgen](CONTRIBUTING.md).

**WICHTIGER HINWEIS ZUR E-MAIL-ÜBERPRÜFUNG**. Ab Version 1.12.0 verwendet dieses Plug-In den gleichen regulären Ausdruck, den die [HTML5-Spezifikation für die Verwendung durch Browser vorschlägt](https://html.spec.whatwg.org/multipage/forms.html#valid-e-mail-address). Wir werden diesem Beispiel folgen und die gleiche Überprüfung verwenden. Wenn Sie der Meinung sind, dass die Spezifikation falsch ist, melden Sie das Problem bitte dort. Wenn Sie andere Anforderungen haben, ziehen Sie die [Verwendung einer benutzerdefinierten Methode](http://jqueryvalidation.org/jQuery.validator.addMethod/) in Betracht.

## <a name="license"></a>Lizenz
Copyright &copy; Jörn Zaefferer<br>
Lizenziert unter der MIT-Lizenz.
