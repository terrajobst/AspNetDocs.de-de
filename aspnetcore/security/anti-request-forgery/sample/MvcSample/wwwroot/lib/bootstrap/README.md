---
ms.openlocfilehash: 3be420696add54094f24424285a905e7e7963bad
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/01/2019
ms.locfileid: "57032887"
---
# <a name="bootstraphttpgetbootstrapcom"></a>[Bootstrap](http://getbootstrap.com)

[![Slack](https://bootstrap-slack.herokuapp.com/badge.svg)](https://bootstrap-slack.herokuapp.com)
![Bower-Version](https://img.shields.io/bower/v/bootstrap.svg)
[![npm-Version](https://img.shields.io/npm/v/bootstrap.svg)](https://www.npmjs.com/package/bootstrap)
[![Buildstatus](https://img.shields.io/travis/twbs/bootstrap/master.svg)](https://travis-ci.org/twbs/bootstrap)
[![devDependency-Status](https://img.shields.io/david/dev/twbs/bootstrap.svg)](https://david-dm.org/twbs/bootstrap#info=devDependencies)
[![NuGet](https://img.shields.io/nuget/v/bootstrap.svg)](https://www.nuget.org/packages/Bootstrap)
[![Selenium-Teststatus](https://saucelabs.com/browser-matrix/bootstrap.svg)](https://saucelabs.com/u/bootstrap)

Bootstrap ist ein schlankes, intuitives und leistungsfähiges Front-End-Framework für schnellere und einfachere Webentwicklung. Es wurde von [Mark Otto](https://twitter.com/mdo) und [Jacob Thornton](https://twitter.com/fat) erstellt und wird durch das [Core-Team](https://github.com/orgs/twbs/people) mit massiver Unterstützung und Beteiligung durch die Community gepflegt.

Informationen zu den ersten Schritten finden Sie unter <http://getbootstrap.com>.


## <a name="table-of-contents"></a>Inhaltsverzeichnis

* [Schnellstart](#quick-start)
* [Fehler und Featureanforderungen](#bugs-and-feature-requests)
* [Dokumentation](#documentation)
* [Beitragen](#contributing)
* [Community](#community)
* [Versionskontrolle](#versioning)
* [Ersteller](#creators)
* [Copyright und Lizenz](#copyright-and-license)


## <a name="quick-start"></a>Schnellstart

Mehrere Schnellstartoptionen sind verfügbar:

* [Aktuelles Release herunterladen](https://github.com/twbs/bootstrap/archive/v3.3.7.zip).
* Repository klonen: `git clone https://github.com/twbs/bootstrap.git`.
* Installation mit [Bower](http://bower.io): `bower install bootstrap`.
* Installation mit [npm](https://www.npmjs.com): `npm install bootstrap@3`.
* Installation mit [Meteor](https://www.meteor.com): `meteor add twbs:bootstrap`.
* Installation mit [Composer](https://getcomposer.org): `composer require twbs/bootstrap`.

Lesen Sie die [Seite „Erste Schritte“](http://getbootstrap.com/getting-started/), um Informationen zum Inhalt des Frameworks zu erhalten und Vorlagen und Beispiele sowie vieles mehr abzurufen.

### <a name="whats-included"></a>Inhalt des Downloads

Im Download finden Sie die folgenden Verzeichnisse und Dateien. Dabei sind allgemeine Ressourcen logisch gruppiert, und es werden kompilierte und verkleinerte Varianten bereitgestellt. Folgendes (oder ähnlich) sollte angezeigt werden:

```
bootstrap/
├── css/
│   ├── bootstrap.css
│   ├── bootstrap.css.map
│   ├── bootstrap.min.css
│   ├── bootstrap.min.css.map
│   ├── bootstrap-theme.css
│   ├── bootstrap-theme.css.map
│   ├── bootstrap-theme.min.css
│   └── bootstrap-theme.min.css.map
├── js/
│   ├── bootstrap.js
│   └── bootstrap.min.js
└── fonts/
    ├── glyphicons-halflings-regular.eot
    ├── glyphicons-halflings-regular.svg
    ├── glyphicons-halflings-regular.ttf
    ├── glyphicons-halflings-regular.woff
    └── glyphicons-halflings-regular.woff2
```

Wir stellen kompilierte CSS- und JS- (`bootstrap.*`) sowie kompilierte und verkleinerte CSS- und JS-Dateien (`bootstrap.min.*`) bereit. CSS-[Quellzuordnungen](https://developer.chrome.com/devtools/docs/css-preprocessors) (`bootstrap.*.map`) sind zur Verwendung mit den Entwicklertools bestimmter Browser verfügbar. Schriftarten aus Glyphicons sind ebenso wie das optionale Bootstrap-Design enthalten.


## <a name="bugs-and-feature-requests"></a>Fehler und Featureanforderungen

Möchten Sie einen Fehler melden oder haben Sie eine Featureanforderung? Lesen Sie zuerst die [Problemleitlfäden](https://github.com/twbs/bootstrap/blob/master/CONTRIBUTING.md#using-the-issue-tracker), und suchen Sie nach vorhandenen und geschlossenen Problemen. Wenn Ihr Problem oder Ihre eine Idee noch nicht behandelt wurde, [öffnen Sie ein neues Problem](https://github.com/twbs/bootstrap/issues/new).

Beachten Sie, dass **Featureanforderungen als Ziel [Bootstrap v4](https://github.com/twbs/bootstrap/tree/v4-dev)** verwenden müssen, weil sich Bootstrap v3 nun im Wartungsmodus befindet und für neue Features geschlossen ist. Dies ist der Fall, damit wir unsere Bemühungen auf Bootstrap v4 konzentrieren können.


## <a name="documentation"></a>Dokumentation

Die Bootstrap-Dokumentation, die in diesem Repository im Stammverzeichnis enthalten ist, wird mit [Jekyll](http://jekyllrb.com) erstellt und auf GitHub-Seiten unter <http://getbootstrap.com> öffentlich gehostet. Die Dokumentation kann auch lokal ausgeführt werden.

### <a name="running-documentation-locally"></a>Lokales Ausführen der Dokumentation

1. Wenn erforderlich, [installieren Sie Jekyll](http://jekyllrb.com/docs/installation) und andere Ruby-Abhängigkeiten mit `bundle install`.
   **Hinweis für Windows-Benutzer:** Lesen [dieses inoffizielle Handbuch](http://jekyll-windows.juthilo.com/) um Jekyll ohne Probleme betriebsbereit zu machen.
2. Führen Sie `bundle exec jekyll serve` aus dem Stammverzeichnis `/bootstrap` in der Befehlszeile aus.
4. Öffnen Sie `http://localhost:9001` im Browser. Das war schon alles.

Weitere Informationen zur Verwendung von Jekyll finden Sie in der [Dokumentation](http://jekyllrb.com/docs/home/).

### <a name="documentation-for-previous-releases"></a>Dokumentation für vorherige Versionen

Die Dokumentation für v2.3.2 wird zurzeit unter <http://getbootstrap.com/2.3.2/> zur Verfügung gestellt, während Benutzer auf Bootstrap 3 umsteigen.

[Vorgängerversionen](https://github.com/twbs/bootstrap/releases) und deren Dokumentation stehen ebenfalls zum Download zur Verfügung.


## <a name="contributing"></a>Beitragen

Lesen Sie unsere [Richtlinien zu Beiträgen](https://github.com/twbs/bootstrap/blob/master/CONTRIBUTING.md). Sie enthalten Anleitungen zum Öffnen von Problemen, Codierungsstandards und Hinweise zur Entwicklung.

Darüber hinaus müssen Sie [relevante Komponententests](https://github.com/twbs/bootstrap/tree/master/js/tests) ausführen, wenn Ihr Pull Request JavaScript-Patches oder -Funktionen enthält. Der gesamte HTML- und CSS-Code sollte mit dem [Codeleitfaden](https://github.com/mdo/code-guide) konform sein, der von [Mark Otto](https://github.com/mdo) verwaltet wird.

**Bootstrap v3 ist jetzt für neue Features geschlossen.** Diese Version wurde in den Wartungsmodus versetzt, damit wir unsere Bemühungen auf [Bootstrap v4](https://github.com/twbs/bootstrap/tree/v4-dev) konzentrieren können, die Zukunft des Frameworks. Pull Requests, die neue Features hinzufügen (anstatt Fehler zu beheben), sollten als Ziel stattdessen [Bootstrap v4 (den Git-Branch `v4-dev`)](https://github.com/twbs/bootstrap/tree/v4-dev) verwenden.

Editor-Einstellungen für die einfache Verwendung in gängigen Text-Editoren finden Sie in der [Editor-Konfigurationsdatei](https://github.com/twbs/bootstrap/blob/master/.editorconfig). Lesen Sie mehr zum Thema, und laden Sie unter <http://editorconfig.org> Plug-Ins herunter.


## <a name="community"></a>Community

Rufen Sie Updates zur Entwicklung von Bootstrap ab, und chatten Sie mit den Projektverantwortlichen und Mitgliedern der Community.

* Folgen Sie [@getbootstrap auf Twitter](https://twitter.com/getbootstrap).
* Lesen und abonnieren Sie den [offiziellen Bootstrap-Blog](http://blog.getbootstrap.com).
* Werden Sie Mitglied im [offiziellen Slack Room](https://bootstrap-slack.herokuapp.com).
* Chatten Sie mit anderen Benutzern von Bootstrapper in IRC. Auf dem `irc.freenode.net`-Server im `##bootstrap`-Kanal.
* Hilfe zur Implementierung finden Sie ggf. unter Stack Overflow (Tag [ `twitter-bootstrap-3` ](https://stackoverflow.com/questions/tagged/twitter-bootstrap-3)).
* Entwickler sollten das Schlüsselwort `bootstrap` für Pakete verwenden, die die Funktionalität von Bootstrap ändern oder erweitern, wenn die Verteilung über [npm](https://www.npmjs.com/browse/keyword/bootstrap) oder ähnliche Mechanismen erfolgt, um eine maximale Auffindbarkeit zu erreichen.


## <a name="versioning"></a>Versionskontrolle

Für Transparenz in unserem Releasezyklus und im Bemühen um Abwärtskompatibilität wird Bootstrap gemäß den [Semantischen Versionierungsrichtlinien](http://semver.org/) gepflegt. In einigen Fällen gelingt uns dies nicht, aber wir halten uns nach Möglichkeit an diese Regeln.

Änderungsprotokolle für jede Releaseversion von Bootstrap finden Sie im [Abschnitt „Releases“ unseres GitHub-Projekts](https://github.com/twbs/bootstrap/releases). Releaseankündigungsbeiträge im [offiziellen Bootstrap-Blog](http://blog.getbootstrap.com) enthalten Zusammenfassungen der wichtigsten Änderungen, die in jedem Release vorgenommen werden.


## <a name="creators"></a>Ersteller

**Mark Otto**

* <https://twitter.com/mdo>
* <https://github.com/mdo>

**Jacob Thornton**

* <https://twitter.com/fat>
* <https://github.com/fat>


## <a name="copyright-and-license"></a>Copyright und Lizenz

Code und Dokumentation Copyright 2011-2016 Twitter, Inc. Code veröffentlicht unter der [MIT-Lizenz](https://github.com/twbs/bootstrap/blob/master/LICENSE). Dokumentation veröffentlicht unter [Creative Commons](https://github.com/twbs/bootstrap/blob/master/docs/LICENSE).
