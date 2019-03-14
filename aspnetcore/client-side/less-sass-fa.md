---
title: Kleiner, Sass und Font Awesome in ASP.NET Core
author: ardalis
description: Erfahren Sie, wie Sie kleiner, Sass und Font Awesome in ASP.NET Core-Anwendungen verwenden.
ms.author: tdykstra
ms.date: 10/14/2016
uid: client-side/less-sass-fa
ms.openlocfilehash: 2229c4e3b0238ff17c15e78f657b9acb10495c72
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/01/2019
ms.locfileid: "57065557"
---
# <a name="less-sass-and-font-awesome-in-aspnet-core"></a>Kleiner, Sass und Font Awesome in ASP.NET Core

Von [Steve Smith](https://ardalis.com/)

Benutzer von Webanwendungen haben zunehmend hohe Erwartungen, wenn es darum geht, formatieren und allgemeine. Moderne Webanwendungen nutzen häufig umfassende Tools und Frameworks zum Definieren und verwalten ihre Aussehen und Verhalten konsistent. Frameworks wie [Bootstrap](http://getbootstrap.com/) gelangen einen wichtiger Beitrag über einen gemeinsamen Satz von Formatvorlagen und Layoutoptionen für Websites zu definieren. Die meisten nicht trivialen Sites profitieren jedoch auch von können effektiv zu definieren und zu verwalten, Stile und cascading Stylesheet (CSS)-Dateien als auch einfach den Zugriff auf nicht-Image-Symbole, mit deren Hilfe der Website-Benutzeroberfläche intuitiver zu gestalten. D. h. wo Sprachen und Tools, die unterstützen [weniger](http://lesscss.org/) und [Sass](http://sass-lang.com/), und -Bibliotheken wie [Font Awesome](http://fontawesome.io/), eingehen.

## <a name="css-preprocessor-languages"></a>CSS-Präprozessor-Sprachen

Sprachen, die in anderen Sprachen, kompiliert werden, um die Arbeit erleichtern der Arbeit mit der zugrunde liegende Sprache werden als Präprozessoren bezeichnet. Es gibt zwei gängige Präprozessoren für CSS: Weniger und Sass.  Diese Präprozessoren Hinzufügen von Funktionen zu CSS, beispielsweise Unterstützung für Variablen und geschachtelte Regeln, die die Verwaltbarkeit großer, komplexer Stylesheets zu verbessern. CSS-Code als eine Sprache ist sehr einfach, fehlen, für die Unterstützung auch für etwas so einfaches wie Variablen, und dies bewirkt meist um CSS-Dateien wiederholten und sehr groß zu machen. Hinzufügen von echten Features von Programmiersprachen über Präprozessoren können Duplikate minimieren, und geben Sie eine bessere Organisation der Stilregeln. Visual Studio bietet integrierte Unterstützung für beide Less und Sass, als auch Erweiterungen, die für die Entwicklung weiter verbessern können, bei der Arbeit mit diesen Sprachen.

Ein kurzes Beispiel wie Präprozessoren Lesbarkeit und verwaltbarkeit von Formatinformationen verbessern können betrachten Sie dieses CSS:

```css
.header {
    color: black;
    font-weight: bold;
    font-size: 18px;
    font-family: Helvetica, Arial, sans-serif;
}

.small-header {
    color: black;
    font-weight: bold;
    font-size: 14px;
    font-family: Helvetica, Arial, sans-serif;
}
```

Verwenden weniger, dies kann umgeschrieben werden zum Entfernen aller Duplikate, mit einer *Mixin* (so genannt, da es Ihnen ermöglicht, die "mix in" Eigenschaften aus einer Klasse oder in einen anderen Regelsatz):

```less
.header {
    color: black;
    font-weight: bold;
    font-size: 18px;
    font-family: Helvetica, Arial, sans-serif;
}

.small-header {
    .header;
    font-size: 14px;
}
```

## <a name="less"></a>weniger

Die weniger CSS-vorverarbeitung wird ausgeführt, mithilfe von Node.js. Um weniger zu installieren, verwenden Sie auf den Node Package Manager (Npm), über eine Eingabeaufforderung (-g bedeutet "global"):

```console
npm install -g less
```

Wenn Sie Visual Studio verwenden, können Sie mit weniger Aufwand, indem Sie eine oder mehrere Less-Dateien zu Ihrem Projekt hinzufügen und dann konfigurieren Gulp (oder Grunt) zur Verarbeitung zum Zeitpunkt der Kompilierung beginnen. Hinzufügen einer *Stile* Ordner hinzu, und fügen Sie dann ein neues Less-Datei, die mit dem Namen *main.less* in diesen Ordner.

![Less-Datei hinzufügen](less-sass-fa/_static/add-less-file.png)

Sobald hinzugefügt haben, sollte die Ordnerstruktur wie folgt aussehen:

![Ordnerstruktur](less-sass-fa/_static/folder-structure.png)

Jetzt können Sie einige grundlegende Formatierungen in der Datei hinzufügen, die in CSS kompiliert und auf den Ordner "Wwwroot" Gulp bereitgestellt wird.

Ändern Sie *main.less* sollen den folgenden Inhalt, die eine einfache Farbpalette aus einer einzelnen Basis Farbe erstellt.

```less
@base: #663333;
@background: spin(@base, 180);
@lighter: lighten(spin(@base, 5), 10%);
@lighter2: lighten(spin(@base, 10), 20%);
@darker: darken(spin(@base, -5), 10%);
@darker2: darken(spin(@base, -10), 20%);

body {
    background-color:@background;
}
.baseColor  {color:@base}
.bgLight    {color:@lighter}
.bgLight2   {color:@lighter2}
.bgDark     {color:@darker}
.bgDark2    {color:@darker2}
```

`@base` und die andere @-prefixed Elemente sind Variablen. Jeder davon stellt eine Farbe dar. Mit Ausnahme von `@base`, erledigt diese Farbe-Funktionen verwenden: Aufhellen abgedunkelt werden soll, und starten. Heller und dunkler sind ziemlich erwartet; Drehfeld passt den Farbton einer Farbe durch eine Reihe von Grad (in der Nähe der-Farbrad). Der weniger Prozessor ist intelligent genug, um Variablen, die nicht verwendet werden, ignorieren Sie daher zur Veranschaulichung, wie diese Variablen zu arbeiten, wir sie an einer beliebigen Stelle verwenden müssen. Die Klassen `.baseColor`, usw. wird gezeigt, das die berechneten Werte aller Variablen in der CSS-Datei, die erzeugt wird.

### <a name="get-started"></a>Erste Schritte

Erstellen einer **Npm-Konfigurationsdatei** (*"Package.JSON"*) in Ihrem Projektordner, und bearbeiten, damit Sie verweisen `gulp` und `gulp-less`:

```json
{
  "version": "1.0.0",
  "name": "asp.net",
  "private": true,
  "devDependencies": {
    "gulp": "3.9.1",
    "gulp-less": "3.3.0"
  }
}
```

Installieren Sie die Abhängigkeiten, die entweder an einer Eingabeaufforderung aus, in Ihrem Projektordner befinden oder in Visual Studio **Projektmappen-Explorer** (**Abhängigkeiten > Npm > Wiederherstellen von Paketen**).

```console
npm install
```

![Visual Studio stellen Pakete wieder her.](less-sass-fa/_static/restore-packages.png)

Erstellen Sie in den Projektordner ein **Gulp-Konfigurationsdatei** (*"gulpfile.js"*) um den automatisierten Prozess zu definieren.  Fügen Sie am oberen Rand der Datei, die weniger darstellen, und einen Task ausführen kleiner eine Variable hinzu:

```javascript
var gulp = require("gulp"),
  fs = require("fs"),
  less = require("gulp-less");

gulp.task("less", function () {
  return gulp.src('Styles/main.less')
    .pipe(less())
    .pipe(gulp.dest('wwwroot/css'));
});
```

Öffnen der **Task Runner Explorer** (**Ansicht > andere Windows > Task Runner Explorer**). Für die Aufgaben, sollte eine neue Aufgabe, die mit dem Namen `less`. Sie müssen möglicherweise das Fenster zu aktualisieren.

Führen Sie die `less` Aufgabe und finden Sie eine Ausgabe ähnlich wie hier gezeigt:

![Less-aufgabenausführung.](less-sass-fa/_static/less-task-runner.png)

Die *Wwwroot/Css* Ordner enthält jetzt eine neue Datei *main.css*:

![Main Css erstellt](less-sass-fa/_static/main-css-created.png)

Open *main.css* und Sie sehen etwa wie folgt:

```css
body {
    background-color: #336666;
}
.baseColor {
    color: #663333;
}
.bgLight {
    color: #884a44;
}
.bgLight2 {
    color: #aa6355;
}
.bgDark {
    color: #442225;
}
.bgDark2 {
    color: #221114;
}
```

Fügen Sie eine einfache HTML-Seite, um die *"Wwwroot"* Ordner und Verweis *main.css* auf der Farbpalette in Aktion finden Sie unter.

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8" />
    <link href="css/main.css" rel="stylesheet" />
    <title></title>
</head>
<body>
    <div>
        <div class="baseColor">BaseColor</div>
        <div class="bgLight">Light</div>
        <div class="bgLight2">Light2</div>
        <div class="bgDark">Dark</div>
        <div class="bgDark2">Dark2</div>
    </div>
</body>
</html>
```

Sie sehen, starten die 180-Grad für `@base` zum Erzeugen von verwendet `@background` führte dazu, dass die Farbe des gegensätzlichen Farbkreis `@base`:

![Beispiel für weniger Tests](less-sass-fa/_static/less-test-screenshot.png)

Weniger bietet auch Unterstützung für geschachtelte Regeln als auch geschachtelte Medienabfragen. Z. B. wie definierende geschachtelte Hierarchien wie Menüs ausführliche CSS-Regeln führen diese:

```css
nav {
    height: 40px;
    width: 100%;
}
nav li {
    height: 38px;
    width: 100px;
}
nav li a:link {
    color: #000;
    text-decoration: none;
}
nav li a:visited {
    text-decoration: none;
    color: #CC3333;
}
nav li a:hover {
    text-decoration: underline;
    font-weight: bold;
}
nav li a:active {
    text-decoration: underline;
}
```

Im Idealfall aller zugehörigen Stilregeln platziert werden zusammen in der CSS-Datei, aber in der Praxis ist "nothing" erzwingen diese Regel, mit Ausnahme von Konvention und vielleicht blockskommentaren aus.

Definieren diese derselben Regeln, die mit weniger sieht folgendermaßen aus:

```less
nav {
    height: 40px;
    width: 100%;
    li {
        height: 38px;
        width: 100px;
        a {
            color: #000;
            &:link { text-decoration:none}
            &:visited { color: #CC3333; text-decoration:none}
            &:hover { text-decoration:underline; font-weight:bold}
            &:active {text-decoration:underline}
        }
    }
}
```

Beachten Sie, dass in diesem Fall alle untergeordneten Elemente des `nav` innerhalb seines Bereichs befinden. Es ist nicht mehr jeder Wiederholung der übergeordneten Elemente (`nav`, `li`, `a`), und die Anzahl der insgesamt Zeilen wurde ebenfalls gelöscht (obwohl einige der, die ist das Ergebnis Werte zu platzieren, auf die gleichen Zeilen im zweiten Beispiel). Es kann sehr hilfreich, organisatorisch, um alle Regeln für ein angegebenes UI-Element in einem explizit begrenzten Bereich, in diesem Fall finden Sie unter vom Rest der Datei durch geschweifte Klammern festgelegt werden deaktiviert.

Die `&` Syntax ist eine weniger Selector-Funktion, mit &, die der aktuellen Auswahl übergeordneten Elements darstellt. Ja, in dem ein {...} Block `&` stellt eine `a` -Tag, und somit `&:link` entspricht `a:link`.

Medienabfragen, äußerst nützlich bei der Erstellung von reaktionsfähiger Entwürfen, können auch stark zur Wiederholung sowie die Komplexität bei CSS beitragen. Weniger kann Medienabfragen innerhalb von Klassen, geschachtelt werden, damit die gesamte Klassendefinition nicht in anderen wiederholt werden muss der obersten Ebene `@media` Elemente. Hier ist z. B. CSS für ein Menü reagiert:

```css
.navigation {
    margin-top: 30%;
    width: 100%;
}
@media screen and (min-width: 40em) {
    .navigation {
        margin: 0;
    }
}
@media screen and (min-width: 62em) {
    .navigation {
        width: 960px;
        margin: 0;
    }
}
```

Dies kann besser in weniger als definiert werden:

```less
.navigation {
    margin-top: 30%;
    width: 100%;
    @media screen and (min-width: 40em) {
        margin: 0;
    }
    @media screen and (min-width: 62em) {
        width: 960px;
        margin: 0;
    }
}
```

Ein weiteres Feature von weniger als die wir bereits gesehen haben, ist die Unterstützung für mathematische Vorgänge, damit Stilattribute mit vordefinierten Variablen erstellt werden. Dadurch wird das Aktualisieren von zugehörigen Formatvorlagen, die sehr viel einfacher, da die Basis-Variable geändert werden kann, und alle abhängigen Werte automatisch geändert.

CSS-Dateien, insbesondere für große Websites (und besonders, wenn Medienabfragen verwendet werden), in der Regel im Laufe der Zeit sehr groß lassen und mit der Verwendung sehr unhandlich. Less-Dateien können separat definiert werden, klicken Sie dann abgerufen, mithilfe von `@import` Anweisungen. Weniger kann auch verwendet werden zum Importieren der einzelnen CSS-Dateien, auch bei Bedarf.

*Mixins* Parameter annehmen können und weniger bedingten Logik unterstützt, in Form von Mixin schützt, die eine deklarative Möglichkeit zum definieren, wann bestimmte Mixins wirksam bereitstellen. Ein gängiges Szenario für Mixin schützt ist um Farben basierend auf wie Licht anzupassen oder dunkel Quellfarbe. Ein Mixin, die einen Parameter für die Farbe des akzeptiert wird angegeben, kann ein Mixin-Wächter so ändern Sie die Mixin basierend auf die entsprechende Farbe verwendet werden:

```less
.box (@color) when (lightness(@color) >= 50%) {
    background-color: #000;
}
.box (@color) when (lightness(@color) < 50%) {
    background-color: #FFF;
}
.box (@color) {
    color: @color;
}

.feature {
    .box (@base);
}
```

Erhalten unseren aktuellen `@base` Wert `#663333`, weniger dieses Skript erzeugt die folgende CSS:

```css
.feature {
    background-color: #FFF;
    color: #663333;
}
```

Weniger bietet es sich um eine Reihe von zusätzlichen Funktionen, aber dies sollte Ihnen eine Vorstellung davon die Leistungsfähigkeit dieser Sprache vorverarbeitung.

## <a name="sass"></a>Sass

Sass ähnelt kleiner, bietet Unterstützung für viele der Funktionen, jedoch mit etwas andere Syntax. Es basiert auf Ruby, statt in JavaScript und besitzt somit unterschiedliche für das Setup. Die ursprüngliche Sass-Sprache nicht geschweiften Klammern oder Semikola als Trennzeichen verwendet, aber stattdessen definiert Bereichen mithilfe von Leerräumen und Einzügen. In Version 3 von Sass, eine neue Syntax eingeführt wurde, **SCSS** ("Sassy CSS"). SCSS ist ähnlich wie CSS, Einzugsebenen und Leerzeichen werden ignoriert, und verwendet stattdessen Semikolons und geschweifte Klammern.

Zum Installieren von Sass, würden in der Regel Sie zuerst installieren Sie Ruby (vorinstalliert unter MacOS), und klicken Sie dann ausführen:

```console
gem install sass
```

Jedoch wenn Sie Visual Studio ausführen, können Sie mit Sass im Wesentlichen die gleiche Weise beginnen wie mit weniger Aufwand. Open *"Package.JSON"* und fügen Sie das Paket "Gulp-Sass" `devDependencies`:

```json
"devDependencies": {
  "gulp": "3.9.1",
  "gulp-less": "3.3.0",
  "gulp-sass": "3.1.0"
}
```

Ändern Sie als Nächstes *"gulpfile.js"* einer Sass-Variablen und einer Aufgabe kompilieren Ihre Sass-Dateien und die Ergebnisse in den Ordner "Wwwroot" hinzufügen:

```javascript
var gulp = require("gulp"),
  fs = require("fs"),
  less = require("gulp-less"),
  sass = require("gulp-sass");

// other content removed

gulp.task("sass", function () {
  return gulp.src('Styles/main2.scss')
    .pipe(sass())
    .pipe(gulp.dest('wwwroot/css'));
});
```

Nun Sie die Datei Sass fügen *main2.scss* auf die *Stile* Ordner im Stammverzeichnis des Projekts:

![Scss-Datei hinzufügen](less-sass-fa/_static/add-scss-file.png)

Open *main2.scss* und fügen Sie Folgendes:

```sass
$base: #CC0000;
body {
    background-color: $base;
}
```

Speichern Sie all Ihre Dateien. Wenn Sie jetzt aktualisieren **Task Runner-Explorer**, Sie sehen eine `sass` Aufgabe. Führen Sie es aus, und suchen Sie der */Wwwroot/Css* Ordner. Es gibt jetzt eine *main2.css* -Datei mit dem folgenden Inhalt:

```css
body {
    background-color: #CC0000;
}
```

Sass unterstützt die Schachtelung im Wesentlichen, gleich, kleiner ist, bietet ähnliche Vorteile. Dateien können von Funktion aufgeteilt werden, und enthalten, mit der `@import` Richtlinie:

```sass
@import 'anotherfile';
```

Sass unterstützt Mixins auch die Verwendung der `@mixin` Schlüsselwort, um diese zu definieren und `@include` um sie einzuschließen, wie im folgenden Beispiel aus [Sass-lang.com](http://sass-lang.com):

```sass
@mixin border-radius($radius) {
    -webkit-border-radius: $radius;
    -moz-border-radius: $radius;
    -ms-border-radius: $radius;
    border-radius: $radius;
}

.box { @include border-radius(10px); }
```

Zusätzlich zu Mixins Sass unterstützt auch das Konzept der Vererbung, sodass eine Klasse für eine andere Schnittstelle erweitern. Es ist vom Konzept her ähnelt ein Mixin. allerdings führt zu weniger CSS-Code. Dies wird erreicht, mit der `@extend` Schlüsselwort. Um Mixins auszuprobieren, fügen Sie den folgenden Ihre *main2.scss* Datei:

```sass
@mixin alert {
    border: 1px solid black;
    padding: 5px;
    color: #333333;
}

.success {
    @include alert;
    border-color: green;
}

.error {
    @include alert;
    color: red;
    border-color: red;
    font-weight:bold;
}
```

Überprüfen Sie die Ausgabe in *main2.css* nach dem Ausführen der `sass` task **Task Runner-Explorer**:

```css
.success {
    border: 1px solid black;
    padding: 5px;
    color: #333333;
    border-color: green;
 }

.error {
    border: 1px solid black;
    padding: 5px;
    color: #333333;
    color: red;
    border-color: red;
    font-weight: bold;
}
```

Beachten Sie, dass alle die allgemeinen Eigenschaften der Warnung Mixin in jeder Klasse wiederholt werden. Die Mixin hat Ihnen hilft, Duplizierung, die zum Zeitpunkt der Entwicklung zu vermeiden, aber CSS wird weiterhin mit einer Vielzahl von Duplizierungen, größer als der erforderlichen CSS-Dateien – ein mögliches Leistungsproblem erstellt.

Ersetzen Sie jetzt die Warnung Mixin mit einem `.alert` Klasse, und ändern Sie `@include` zu `@extend` (Denken Sie daran, erweitern Sie `.alert`, nicht `alert`):

```sass
.alert {
    border: 1px solid black;
    padding: 5px;
    color: #333333;
}

.success {
    @extend .alert;
    border-color: green;
}

.error {
    @extend .alert;
    color: red;
    border-color: red;
    font-weight:bold;
}
```

Sass einmal ausgeführt, und überprüfen Sie die resultierende CSS:

```css
.alert, .success, .error {
    border: 1px solid black;
    padding: 5px;
    color: #333333;
}

.success {
    border-color: green;
}

.error {
    color: red;
    border-color: red;
    font-weight: bold;
}
```

Jetzt sind die Eigenschaften nur als häufig wie nötig definiert, und eine bessere CSS generiert wird.

Sass enthält auch Funktionen und Vorgänge für bedingte Logik, weniger ähnelt. Die beiden Sprachen Funktionen sind in der Tat sehr ähnlich.

## <a name="less-or-sass"></a>Weniger oder Sass?

Es ist immer noch keine Konsens gibt an, ob es im Allgemeinen besser, verwenden weniger oder Sass ist (oder sogar an, ob die ursprünglichen Sass oder die neuere SCSS-Syntax im Sass bevorzugen). Wahrscheinlich ist die wichtigste Entscheidung **verwenden Sie eines dieser Tools**, im Gegensatz zu nur-handcodierung CSS-Dateien. Nachdem Sie auf, die vorgenommen haben der Entscheidung, beide Less und Sass sind gute Optionen.

## <a name="font-awesome"></a>Font Awesome

Zusätzlich zur CSS-Präprozessoren ist eine weitere hervorragende Ressource für moderne Webanwendungen Formatierung Font Awesome. Font Awesome ist ein Toolkit, die mehr als 500 skalierbare Vektorgrafiken Symbole bereitstellt, die kostenlos in Ihren Webanwendungen verwendet werden kann. Es wurde ursprünglich entworfen, um die Arbeit mit Bootstrap, aber es besteht keine Abhängigkeit für dieses Framework oder für alle JavaScript-Bibliotheken.

Die einfachste Möglichkeit zum Einstieg in Font Awesome ist einen Verweis auf, mit der öffentlichen Content Delivery Network (CDN)-Speicherort hinzufügen:

```html
<link rel="stylesheet" href="//maxcdn.bootstrapcdn.com/font-awesome/4.3.0/css/font-awesome.min.css">
```

Sie können auch hinzufügen es zu Ihrem Visual Studio-Projekt zu der "Dependencies" hinzu, in *"bower.JSON"*:

```json
{
  "name": "ASP.NET",
  "private": true,
  "dependencies": {
    "bootstrap": "3.0.0",
    "jquery": "1.10.2",
    "jquery-validation": "1.11.1",
    "jquery-validation-unobtrusive": "3.2.2",
    "hammer.js": "2.0.4",
    "bootstrap-touch-carousel": "0.8.0",
    "Font-Awesome": "4.3.0"
  }
}
```

Nachdem Sie einen Verweis auf Font Awesome auf einer Seite haben, können Sie Symbole für Ihre Anwendung hinzufügen, indem Font Awesome Klassen, in der Regel mit dem Präfix "Fa-", um die Inline-HTML-Elemente anwenden (z. B. `<span>` oder `<i>`).  Beispielsweise können Sie Symbole einfache Listen und Menüs, die mithilfe von Code wie folgt hinzufügen:

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8" />
    <title></title>
    <link href="lib/font-awesome/css/font-awesome.css" rel="stylesheet" />
</head>
<body>
    <ul class="fa-ul">
        <li><i class="fa fa-li fa-home"></i> Home</li>
        <li><i class="fa fa-li fa-cog"></i> Settings</li>
    </ul>
</body>
</html>
```

Dies erzeugt die folgenden im Browser – Beachten Sie das Symbol neben dem jedes Element:

![Symbole](less-sass-fa/_static/list-icons-screenshot.png)

Sie können eine vollständige Liste der verfügbaren Symbole hier anzeigen:

http://fontawesome.io/icons/

## <a name="summary"></a>Zusammenfassung

Moderne Webanwendungen erfordern immer reaktionsfähig, flüssige-Entwürfe, die übersichtlich, intuitiv und einfach zu verwenden, aus einer Vielzahl von Geräten sind. Bewältigung der Komplexität von CSS-Stylesheets, die erforderlich, um diese Ziele erreichen, erfolgt am besten verwenden eine ähnliche Präprozessor weniger oder Sass. Darüber hinaus Toolkits wie Font Awesome geben schnell Well-known Text Navigationsmenüs Symbole und Schaltflächen, Verbessern des allgemeinen Benutzers Ihrer Anwendung auftreten.
