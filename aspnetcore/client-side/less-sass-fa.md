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
# <a name="less-sass-and-font-awesome-in-aspnet-core"></a><span data-ttu-id="364e5-103">Kleiner, Sass und Font Awesome in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="364e5-103">Less, Sass, and Font Awesome in ASP.NET Core</span></span>

<span data-ttu-id="364e5-104">Von [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="364e5-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="364e5-105">Benutzer von Webanwendungen haben zunehmend hohe Erwartungen, wenn es darum geht, formatieren und allgemeine.</span><span class="sxs-lookup"><span data-stu-id="364e5-105">Users of web applications have increasingly high expectations when it comes to style and overall experience.</span></span> <span data-ttu-id="364e5-106">Moderne Webanwendungen nutzen häufig umfassende Tools und Frameworks zum Definieren und verwalten ihre Aussehen und Verhalten konsistent.</span><span class="sxs-lookup"><span data-stu-id="364e5-106">Modern web applications frequently leverage rich tools and frameworks for defining and managing their look and feel in a consistent manner.</span></span> <span data-ttu-id="364e5-107">Frameworks wie [Bootstrap](http://getbootstrap.com/) gelangen einen wichtiger Beitrag über einen gemeinsamen Satz von Formatvorlagen und Layoutoptionen für Websites zu definieren.</span><span class="sxs-lookup"><span data-stu-id="364e5-107">Frameworks like [Bootstrap](http://getbootstrap.com/) can go a long way toward defining a common set of styles and layout options for web sites.</span></span> <span data-ttu-id="364e5-108">Die meisten nicht trivialen Sites profitieren jedoch auch von können effektiv zu definieren und zu verwalten, Stile und cascading Stylesheet (CSS)-Dateien als auch einfach den Zugriff auf nicht-Image-Symbole, mit deren Hilfe der Website-Benutzeroberfläche intuitiver zu gestalten.</span><span class="sxs-lookup"><span data-stu-id="364e5-108">However, most non-trivial sites also benefit from being able to effectively define and maintain styles and cascading style sheet (CSS) files, as well as having easy access to non-image icons that help make the site's interface more intuitive.</span></span> <span data-ttu-id="364e5-109">D. h. wo Sprachen und Tools, die unterstützen [weniger](http://lesscss.org/) und [Sass](http://sass-lang.com/), und -Bibliotheken wie [Font Awesome](http://fontawesome.io/), eingehen.</span><span class="sxs-lookup"><span data-stu-id="364e5-109">That's where languages and tools that support [Less](http://lesscss.org/) and [Sass](http://sass-lang.com/), and libraries like [Font Awesome](http://fontawesome.io/), come in.</span></span>

## <a name="css-preprocessor-languages"></a><span data-ttu-id="364e5-110">CSS-Präprozessor-Sprachen</span><span class="sxs-lookup"><span data-stu-id="364e5-110">CSS preprocessor languages</span></span>

<span data-ttu-id="364e5-111">Sprachen, die in anderen Sprachen, kompiliert werden, um die Arbeit erleichtern der Arbeit mit der zugrunde liegende Sprache werden als Präprozessoren bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="364e5-111">Languages that are compiled into other languages, in order to improve the experience of working with the underlying language, are referred to as preprocessors.</span></span> <span data-ttu-id="364e5-112">Es gibt zwei gängige Präprozessoren für CSS: Weniger und Sass.</span><span class="sxs-lookup"><span data-stu-id="364e5-112">There are two popular preprocessors for CSS: Less and Sass.</span></span>  <span data-ttu-id="364e5-113">Diese Präprozessoren Hinzufügen von Funktionen zu CSS, beispielsweise Unterstützung für Variablen und geschachtelte Regeln, die die Verwaltbarkeit großer, komplexer Stylesheets zu verbessern.</span><span class="sxs-lookup"><span data-stu-id="364e5-113">These preprocessors add features to CSS, such as support for variables and nested rules, which improve the maintainability of large, complex stylesheets.</span></span> <span data-ttu-id="364e5-114">CSS-Code als eine Sprache ist sehr einfach, fehlen, für die Unterstützung auch für etwas so einfaches wie Variablen, und dies bewirkt meist um CSS-Dateien wiederholten und sehr groß zu machen.</span><span class="sxs-lookup"><span data-stu-id="364e5-114">CSS as a language is very basic, lacking support even for something as simple as variables, and this tends to make CSS files repetitive and bloated.</span></span> <span data-ttu-id="364e5-115">Hinzufügen von echten Features von Programmiersprachen über Präprozessoren können Duplikate minimieren, und geben Sie eine bessere Organisation der Stilregeln.</span><span class="sxs-lookup"><span data-stu-id="364e5-115">Adding real programming language features via preprocessors can help reduce duplication and provide better organization of styling rules.</span></span> <span data-ttu-id="364e5-116">Visual Studio bietet integrierte Unterstützung für beide Less und Sass, als auch Erweiterungen, die für die Entwicklung weiter verbessern können, bei der Arbeit mit diesen Sprachen.</span><span class="sxs-lookup"><span data-stu-id="364e5-116">Visual Studio provides built-in support for both Less and Sass, as well as extensions that can further improve the development experience when working with these languages.</span></span>

<span data-ttu-id="364e5-117">Ein kurzes Beispiel wie Präprozessoren Lesbarkeit und verwaltbarkeit von Formatinformationen verbessern können betrachten Sie dieses CSS:</span><span class="sxs-lookup"><span data-stu-id="364e5-117">As a quick example of how preprocessors can improve readability and maintainability of style information, consider this CSS:</span></span>

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

<span data-ttu-id="364e5-118">Verwenden weniger, dies kann umgeschrieben werden zum Entfernen aller Duplikate, mit einer *Mixin* (so genannt, da es Ihnen ermöglicht, die "mix in" Eigenschaften aus einer Klasse oder in einen anderen Regelsatz):</span><span class="sxs-lookup"><span data-stu-id="364e5-118">Using Less, this can be rewritten to eliminate all of the duplication, using a *mixin* (so named because it allows you to "mix in" properties from one class or rule-set into another):</span></span>

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

## <a name="less"></a><span data-ttu-id="364e5-119">weniger</span><span class="sxs-lookup"><span data-stu-id="364e5-119">Less</span></span>

<span data-ttu-id="364e5-120">Die weniger CSS-vorverarbeitung wird ausgeführt, mithilfe von Node.js.</span><span class="sxs-lookup"><span data-stu-id="364e5-120">The Less CSS preprocessor runs using Node.js.</span></span> <span data-ttu-id="364e5-121">Um weniger zu installieren, verwenden Sie auf den Node Package Manager (Npm), über eine Eingabeaufforderung (-g bedeutet "global"):</span><span class="sxs-lookup"><span data-stu-id="364e5-121">To install Less, use Node Package Manager (npm) from a command prompt (-g means "global"):</span></span>

```console
npm install -g less
```

<span data-ttu-id="364e5-122">Wenn Sie Visual Studio verwenden, können Sie mit weniger Aufwand, indem Sie eine oder mehrere Less-Dateien zu Ihrem Projekt hinzufügen und dann konfigurieren Gulp (oder Grunt) zur Verarbeitung zum Zeitpunkt der Kompilierung beginnen.</span><span class="sxs-lookup"><span data-stu-id="364e5-122">If you're using Visual Studio, you can get started with Less by adding one or more Less files to your project, and then configuring Gulp (or Grunt) to process them at compile-time.</span></span> <span data-ttu-id="364e5-123">Hinzufügen einer *Stile* Ordner hinzu, und fügen Sie dann ein neues Less-Datei, die mit dem Namen *main.less* in diesen Ordner.</span><span class="sxs-lookup"><span data-stu-id="364e5-123">Add a *Styles* folder to your project, and then add a new Less file named *main.less* to this folder.</span></span>

![Less-Datei hinzufügen](less-sass-fa/_static/add-less-file.png)

<span data-ttu-id="364e5-125">Sobald hinzugefügt haben, sollte die Ordnerstruktur wie folgt aussehen:</span><span class="sxs-lookup"><span data-stu-id="364e5-125">Once added, your folder structure should look something like this:</span></span>

![Ordnerstruktur](less-sass-fa/_static/folder-structure.png)

<span data-ttu-id="364e5-127">Jetzt können Sie einige grundlegende Formatierungen in der Datei hinzufügen, die in CSS kompiliert und auf den Ordner "Wwwroot" Gulp bereitgestellt wird.</span><span class="sxs-lookup"><span data-stu-id="364e5-127">Now you can add some basic styling to the file, which will be compiled into CSS and deployed to the wwwroot folder by Gulp.</span></span>

<span data-ttu-id="364e5-128">Ändern Sie *main.less* sollen den folgenden Inhalt, die eine einfache Farbpalette aus einer einzelnen Basis Farbe erstellt.</span><span class="sxs-lookup"><span data-stu-id="364e5-128">Modify *main.less* to include the following content, which creates a simple color palette from a single base color.</span></span>

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

<span data-ttu-id="364e5-129">`@base` und die andere @-prefixed Elemente sind Variablen.</span><span class="sxs-lookup"><span data-stu-id="364e5-129">`@base` and the other @-prefixed items are variables.</span></span> <span data-ttu-id="364e5-130">Jeder davon stellt eine Farbe dar.</span><span class="sxs-lookup"><span data-stu-id="364e5-130">Each of them represents a color.</span></span> <span data-ttu-id="364e5-131">Mit Ausnahme von `@base`, erledigt diese Farbe-Funktionen verwenden: Aufhellen abgedunkelt werden soll, und starten.</span><span class="sxs-lookup"><span data-stu-id="364e5-131">Except for `@base`, they're set using color functions: lighten, darken, and spin.</span></span> <span data-ttu-id="364e5-132">Heller und dunkler sind ziemlich erwartet; Drehfeld passt den Farbton einer Farbe durch eine Reihe von Grad (in der Nähe der-Farbrad).</span><span class="sxs-lookup"><span data-stu-id="364e5-132">Lighten and darken do pretty much what you would expect; spin adjusts the hue of a color by a number of degrees (around the color wheel).</span></span> <span data-ttu-id="364e5-133">Der weniger Prozessor ist intelligent genug, um Variablen, die nicht verwendet werden, ignorieren Sie daher zur Veranschaulichung, wie diese Variablen zu arbeiten, wir sie an einer beliebigen Stelle verwenden müssen.</span><span class="sxs-lookup"><span data-stu-id="364e5-133">The Less processor is smart enough to ignore variables that aren't used, so to demonstrate how these variables work, we need to use them somewhere.</span></span> <span data-ttu-id="364e5-134">Die Klassen `.baseColor`, usw. wird gezeigt, das die berechneten Werte aller Variablen in der CSS-Datei, die erzeugt wird.</span><span class="sxs-lookup"><span data-stu-id="364e5-134">The classes `.baseColor`, etc. will demonstrate the calculated values of each of the variables in the CSS file that's produced.</span></span>

### <a name="get-started"></a><span data-ttu-id="364e5-135">Erste Schritte</span><span class="sxs-lookup"><span data-stu-id="364e5-135">Get started</span></span>

<span data-ttu-id="364e5-136">Erstellen einer **Npm-Konfigurationsdatei** (*"Package.JSON"*) in Ihrem Projektordner, und bearbeiten, damit Sie verweisen `gulp` und `gulp-less`:</span><span class="sxs-lookup"><span data-stu-id="364e5-136">Create an **npm Configuration File** (*package.json*) in your project folder and edit it to reference `gulp` and `gulp-less`:</span></span>

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

<span data-ttu-id="364e5-137">Installieren Sie die Abhängigkeiten, die entweder an einer Eingabeaufforderung aus, in Ihrem Projektordner befinden oder in Visual Studio **Projektmappen-Explorer** (**Abhängigkeiten > Npm > Wiederherstellen von Paketen**).</span><span class="sxs-lookup"><span data-stu-id="364e5-137">Install the dependencies either at a command prompt in your project folder, or in Visual Studio **Solution Explorer** (**Dependencies > npm > Restore packages**).</span></span>

```console
npm install
```

![Visual Studio stellen Pakete wieder her.](less-sass-fa/_static/restore-packages.png)

<span data-ttu-id="364e5-139">Erstellen Sie in den Projektordner ein **Gulp-Konfigurationsdatei** (*"gulpfile.js"*) um den automatisierten Prozess zu definieren.</span><span class="sxs-lookup"><span data-stu-id="364e5-139">In the project folder, create a **Gulp Configuration File** (*gulpfile.js*) to define the automated process.</span></span>  <span data-ttu-id="364e5-140">Fügen Sie am oberen Rand der Datei, die weniger darstellen, und einen Task ausführen kleiner eine Variable hinzu:</span><span class="sxs-lookup"><span data-stu-id="364e5-140">Add a variable at the top of the file to represent Less, and a task to run Less:</span></span>

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

<span data-ttu-id="364e5-141">Öffnen der **Task Runner Explorer** (**Ansicht > andere Windows > Task Runner Explorer**).</span><span class="sxs-lookup"><span data-stu-id="364e5-141">Open the **Task Runner Explorer** (**View > Other Windows > Task Runner Explorer**).</span></span> <span data-ttu-id="364e5-142">Für die Aufgaben, sollte eine neue Aufgabe, die mit dem Namen `less`.</span><span class="sxs-lookup"><span data-stu-id="364e5-142">Among the tasks, you should see a new task named `less`.</span></span> <span data-ttu-id="364e5-143">Sie müssen möglicherweise das Fenster zu aktualisieren.</span><span class="sxs-lookup"><span data-stu-id="364e5-143">You might have to refresh the window.</span></span>

<span data-ttu-id="364e5-144">Führen Sie die `less` Aufgabe und finden Sie eine Ausgabe ähnlich wie hier gezeigt:</span><span class="sxs-lookup"><span data-stu-id="364e5-144">Run the `less` task, and you see output similar to what is shown here:</span></span>

![Less-aufgabenausführung.](less-sass-fa/_static/less-task-runner.png)

<span data-ttu-id="364e5-146">Die *Wwwroot/Css* Ordner enthält jetzt eine neue Datei *main.css*:</span><span class="sxs-lookup"><span data-stu-id="364e5-146">The *wwwroot/css* folder now contains a new file, *main.css*:</span></span>

![Main Css erstellt](less-sass-fa/_static/main-css-created.png)

<span data-ttu-id="364e5-148">Open *main.css* und Sie sehen etwa wie folgt:</span><span class="sxs-lookup"><span data-stu-id="364e5-148">Open *main.css* and you see something like the following:</span></span>

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

<span data-ttu-id="364e5-149">Fügen Sie eine einfache HTML-Seite, um die *"Wwwroot"* Ordner und Verweis *main.css* auf der Farbpalette in Aktion finden Sie unter.</span><span class="sxs-lookup"><span data-stu-id="364e5-149">Add a simple HTML page to the *wwwroot* folder, and reference *main.css* to see the color palette in action.</span></span>

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

<span data-ttu-id="364e5-150">Sie sehen, starten die 180-Grad für `@base` zum Erzeugen von verwendet `@background` führte dazu, dass die Farbe des gegensätzlichen Farbkreis `@base`:</span><span class="sxs-lookup"><span data-stu-id="364e5-150">You can see that the 180 degree spin on `@base` used to produce `@background` resulted in the color wheel opposing color of `@base`:</span></span>

![Beispiel für weniger Tests](less-sass-fa/_static/less-test-screenshot.png)

<span data-ttu-id="364e5-152">Weniger bietet auch Unterstützung für geschachtelte Regeln als auch geschachtelte Medienabfragen.</span><span class="sxs-lookup"><span data-stu-id="364e5-152">Less also provides support for nested rules, as well as nested media queries.</span></span> <span data-ttu-id="364e5-153">Z. B. wie definierende geschachtelte Hierarchien wie Menüs ausführliche CSS-Regeln führen diese:</span><span class="sxs-lookup"><span data-stu-id="364e5-153">For example, defining nested hierarchies like menus can result in verbose CSS rules like these:</span></span>

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

<span data-ttu-id="364e5-154">Im Idealfall aller zugehörigen Stilregeln platziert werden zusammen in der CSS-Datei, aber in der Praxis ist "nothing" erzwingen diese Regel, mit Ausnahme von Konvention und vielleicht blockskommentaren aus.</span><span class="sxs-lookup"><span data-stu-id="364e5-154">Ideally all of the related style rules will be placed together within the CSS file, but in practice there's nothing enforcing this rule except convention and perhaps block comments.</span></span>

<span data-ttu-id="364e5-155">Definieren diese derselben Regeln, die mit weniger sieht folgendermaßen aus:</span><span class="sxs-lookup"><span data-stu-id="364e5-155">Defining these same rules using Less looks like this:</span></span>

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

<span data-ttu-id="364e5-156">Beachten Sie, dass in diesem Fall alle untergeordneten Elemente des `nav` innerhalb seines Bereichs befinden.</span><span class="sxs-lookup"><span data-stu-id="364e5-156">Note that in this case, all of the subordinate elements of `nav` are contained within its scope.</span></span> <span data-ttu-id="364e5-157">Es ist nicht mehr jeder Wiederholung der übergeordneten Elemente (`nav`, `li`, `a`), und die Anzahl der insgesamt Zeilen wurde ebenfalls gelöscht (obwohl einige der, die ist das Ergebnis Werte zu platzieren, auf die gleichen Zeilen im zweiten Beispiel).</span><span class="sxs-lookup"><span data-stu-id="364e5-157">There's no longer any repetition of parent elements (`nav`, `li`, `a`), and the total line count has dropped as well (though some of that's a result of putting values on the same lines in the second example).</span></span> <span data-ttu-id="364e5-158">Es kann sehr hilfreich, organisatorisch, um alle Regeln für ein angegebenes UI-Element in einem explizit begrenzten Bereich, in diesem Fall finden Sie unter vom Rest der Datei durch geschweifte Klammern festgelegt werden deaktiviert.</span><span class="sxs-lookup"><span data-stu-id="364e5-158">It can be very helpful, organizationally, to see all of the rules for a given UI element within an explicitly bounded scope, in this case set off from the rest of the file by curly braces.</span></span>

<span data-ttu-id="364e5-159">Die `&` Syntax ist eine weniger Selector-Funktion, mit &, die der aktuellen Auswahl übergeordneten Elements darstellt.</span><span class="sxs-lookup"><span data-stu-id="364e5-159">The `&` syntax is a Less selector feature, with & representing the current selector parent.</span></span> <span data-ttu-id="364e5-160">Ja, in dem ein {...}</span><span class="sxs-lookup"><span data-stu-id="364e5-160">So, within the a {...}</span></span> <span data-ttu-id="364e5-161">Block `&` stellt eine `a` -Tag, und somit `&:link` entspricht `a:link`.</span><span class="sxs-lookup"><span data-stu-id="364e5-161">block, `&` represents an `a` tag, and thus `&:link` is equivalent to `a:link`.</span></span>

<span data-ttu-id="364e5-162">Medienabfragen, äußerst nützlich bei der Erstellung von reaktionsfähiger Entwürfen, können auch stark zur Wiederholung sowie die Komplexität bei CSS beitragen.</span><span class="sxs-lookup"><span data-stu-id="364e5-162">Media queries, extremely useful in creating responsive designs, can also contribute heavily to repetition and complexity in CSS.</span></span> <span data-ttu-id="364e5-163">Weniger kann Medienabfragen innerhalb von Klassen, geschachtelt werden, damit die gesamte Klassendefinition nicht in anderen wiederholt werden muss der obersten Ebene `@media` Elemente.</span><span class="sxs-lookup"><span data-stu-id="364e5-163">Less allows media queries to be nested within classes, so that the entire class definition doesn't need to be repeated within different top-level `@media` elements.</span></span> <span data-ttu-id="364e5-164">Hier ist z. B. CSS für ein Menü reagiert:</span><span class="sxs-lookup"><span data-stu-id="364e5-164">For example, here is CSS for a responsive menu:</span></span>

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

<span data-ttu-id="364e5-165">Dies kann besser in weniger als definiert werden:</span><span class="sxs-lookup"><span data-stu-id="364e5-165">This can be better defined in Less as:</span></span>

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

<span data-ttu-id="364e5-166">Ein weiteres Feature von weniger als die wir bereits gesehen haben, ist die Unterstützung für mathematische Vorgänge, damit Stilattribute mit vordefinierten Variablen erstellt werden.</span><span class="sxs-lookup"><span data-stu-id="364e5-166">Another feature of Less that we have already seen is its support for mathematical operations, allowing style attributes to be constructed from pre-defined variables.</span></span> <span data-ttu-id="364e5-167">Dadurch wird das Aktualisieren von zugehörigen Formatvorlagen, die sehr viel einfacher, da die Basis-Variable geändert werden kann, und alle abhängigen Werte automatisch geändert.</span><span class="sxs-lookup"><span data-stu-id="364e5-167">This makes updating related styles much easier, since the base variable can be modified and all dependent values change automatically.</span></span>

<span data-ttu-id="364e5-168">CSS-Dateien, insbesondere für große Websites (und besonders, wenn Medienabfragen verwendet werden), in der Regel im Laufe der Zeit sehr groß lassen und mit der Verwendung sehr unhandlich.</span><span class="sxs-lookup"><span data-stu-id="364e5-168">CSS files, especially for large sites (and especially if media queries are being used), tend to get quite large over time, making working with them unwieldy.</span></span> <span data-ttu-id="364e5-169">Less-Dateien können separat definiert werden, klicken Sie dann abgerufen, mithilfe von `@import` Anweisungen.</span><span class="sxs-lookup"><span data-stu-id="364e5-169">Less files can be defined separately, then pulled together using `@import` directives.</span></span> <span data-ttu-id="364e5-170">Weniger kann auch verwendet werden zum Importieren der einzelnen CSS-Dateien, auch bei Bedarf.</span><span class="sxs-lookup"><span data-stu-id="364e5-170">Less can also be used to import individual CSS files, as well, if desired.</span></span>

<span data-ttu-id="364e5-171">*Mixins* Parameter annehmen können und weniger bedingten Logik unterstützt, in Form von Mixin schützt, die eine deklarative Möglichkeit zum definieren, wann bestimmte Mixins wirksam bereitstellen.</span><span class="sxs-lookup"><span data-stu-id="364e5-171">*Mixins* can accept parameters, and Less supports conditional logic in the form of mixin guards, which provide a declarative way to define when certain mixins take effect.</span></span> <span data-ttu-id="364e5-172">Ein gängiges Szenario für Mixin schützt ist um Farben basierend auf wie Licht anzupassen oder dunkel Quellfarbe.</span><span class="sxs-lookup"><span data-stu-id="364e5-172">A common use for mixin guards is to adjust colors based on how light or dark the source color is.</span></span> <span data-ttu-id="364e5-173">Ein Mixin, die einen Parameter für die Farbe des akzeptiert wird angegeben, kann ein Mixin-Wächter so ändern Sie die Mixin basierend auf die entsprechende Farbe verwendet werden:</span><span class="sxs-lookup"><span data-stu-id="364e5-173">Given a mixin that accepts a parameter for color, a mixin guard can be used to modify the mixin based on that color:</span></span>

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

<span data-ttu-id="364e5-174">Erhalten unseren aktuellen `@base` Wert `#663333`, weniger dieses Skript erzeugt die folgende CSS:</span><span class="sxs-lookup"><span data-stu-id="364e5-174">Given our current `@base` value of `#663333`, this Less script will produce the following CSS:</span></span>

```css
.feature {
    background-color: #FFF;
    color: #663333;
}
```

<span data-ttu-id="364e5-175">Weniger bietet es sich um eine Reihe von zusätzlichen Funktionen, aber dies sollte Ihnen eine Vorstellung davon die Leistungsfähigkeit dieser Sprache vorverarbeitung.</span><span class="sxs-lookup"><span data-stu-id="364e5-175">Less provides a number of additional features, but this should give you some idea of the power of this preprocessing language.</span></span>

## <a name="sass"></a><span data-ttu-id="364e5-176">Sass</span><span class="sxs-lookup"><span data-stu-id="364e5-176">Sass</span></span>

<span data-ttu-id="364e5-177">Sass ähnelt kleiner, bietet Unterstützung für viele der Funktionen, jedoch mit etwas andere Syntax.</span><span class="sxs-lookup"><span data-stu-id="364e5-177">Sass is similar to Less, providing support for many of the same features, but with slightly different syntax.</span></span> <span data-ttu-id="364e5-178">Es basiert auf Ruby, statt in JavaScript und besitzt somit unterschiedliche für das Setup.</span><span class="sxs-lookup"><span data-stu-id="364e5-178">It's built using Ruby, rather than JavaScript, and so has different setup requirements.</span></span> <span data-ttu-id="364e5-179">Die ursprüngliche Sass-Sprache nicht geschweiften Klammern oder Semikola als Trennzeichen verwendet, aber stattdessen definiert Bereichen mithilfe von Leerräumen und Einzügen.</span><span class="sxs-lookup"><span data-stu-id="364e5-179">The original Sass language didn't use curly braces or semicolons, but instead defined scope using white space and indentation.</span></span> <span data-ttu-id="364e5-180">In Version 3 von Sass, eine neue Syntax eingeführt wurde, **SCSS** ("Sassy CSS").</span><span class="sxs-lookup"><span data-stu-id="364e5-180">In version 3 of Sass, a new syntax was introduced, **SCSS** ("Sassy CSS").</span></span> <span data-ttu-id="364e5-181">SCSS ist ähnlich wie CSS, Einzugsebenen und Leerzeichen werden ignoriert, und verwendet stattdessen Semikolons und geschweifte Klammern.</span><span class="sxs-lookup"><span data-stu-id="364e5-181">SCSS is similar to CSS in that it ignores indentation levels and whitespace, and instead uses semicolons and curly braces.</span></span>

<span data-ttu-id="364e5-182">Zum Installieren von Sass, würden in der Regel Sie zuerst installieren Sie Ruby (vorinstalliert unter MacOS), und klicken Sie dann ausführen:</span><span class="sxs-lookup"><span data-stu-id="364e5-182">To install Sass, typically you would first install Ruby (pre-installed on macOS), and then run:</span></span>

```console
gem install sass
```

<span data-ttu-id="364e5-183">Jedoch wenn Sie Visual Studio ausführen, können Sie mit Sass im Wesentlichen die gleiche Weise beginnen wie mit weniger Aufwand.</span><span class="sxs-lookup"><span data-stu-id="364e5-183">However, if you're running Visual Studio, you can get started with Sass in much the same way as you would with Less.</span></span> <span data-ttu-id="364e5-184">Open *"Package.JSON"* und fügen Sie das Paket "Gulp-Sass" `devDependencies`:</span><span class="sxs-lookup"><span data-stu-id="364e5-184">Open *package.json* and add the "gulp-sass" package to `devDependencies`:</span></span>

```json
"devDependencies": {
  "gulp": "3.9.1",
  "gulp-less": "3.3.0",
  "gulp-sass": "3.1.0"
}
```

<span data-ttu-id="364e5-185">Ändern Sie als Nächstes *"gulpfile.js"* einer Sass-Variablen und einer Aufgabe kompilieren Ihre Sass-Dateien und die Ergebnisse in den Ordner "Wwwroot" hinzufügen:</span><span class="sxs-lookup"><span data-stu-id="364e5-185">Next, modify *gulpfile.js* to add a sass variable and a task to compile your Sass files and place the results in the wwwroot folder:</span></span>

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

<span data-ttu-id="364e5-186">Nun Sie die Datei Sass fügen *main2.scss* auf die *Stile* Ordner im Stammverzeichnis des Projekts:</span><span class="sxs-lookup"><span data-stu-id="364e5-186">Now you can add the Sass file *main2.scss* to the *Styles* folder in the root of the project:</span></span>

![Scss-Datei hinzufügen](less-sass-fa/_static/add-scss-file.png)

<span data-ttu-id="364e5-188">Open *main2.scss* und fügen Sie Folgendes:</span><span class="sxs-lookup"><span data-stu-id="364e5-188">Open *main2.scss* and add the following:</span></span>

```sass
$base: #CC0000;
body {
    background-color: $base;
}
```

<span data-ttu-id="364e5-189">Speichern Sie all Ihre Dateien.</span><span class="sxs-lookup"><span data-stu-id="364e5-189">Save all of your files.</span></span> <span data-ttu-id="364e5-190">Wenn Sie jetzt aktualisieren **Task Runner-Explorer**, Sie sehen eine `sass` Aufgabe.</span><span class="sxs-lookup"><span data-stu-id="364e5-190">Now when you refresh **Task Runner Explorer**, you see a `sass` task.</span></span> <span data-ttu-id="364e5-191">Führen Sie es aus, und suchen Sie der */Wwwroot/Css* Ordner.</span><span class="sxs-lookup"><span data-stu-id="364e5-191">Run it, and look in the */wwwroot/css* folder.</span></span> <span data-ttu-id="364e5-192">Es gibt jetzt eine *main2.css* -Datei mit dem folgenden Inhalt:</span><span class="sxs-lookup"><span data-stu-id="364e5-192">There's now a *main2.css* file, with these contents:</span></span>

```css
body {
    background-color: #CC0000;
}
```

<span data-ttu-id="364e5-193">Sass unterstützt die Schachtelung im Wesentlichen, gleich, kleiner ist, bietet ähnliche Vorteile.</span><span class="sxs-lookup"><span data-stu-id="364e5-193">Sass supports nesting in much the same was that Less does, providing similar benefits.</span></span> <span data-ttu-id="364e5-194">Dateien können von Funktion aufgeteilt werden, und enthalten, mit der `@import` Richtlinie:</span><span class="sxs-lookup"><span data-stu-id="364e5-194">Files can be split up by function and included using the `@import` directive:</span></span>

```sass
@import 'anotherfile';
```

<span data-ttu-id="364e5-195">Sass unterstützt Mixins auch die Verwendung der `@mixin` Schlüsselwort, um diese zu definieren und `@include` um sie einzuschließen, wie im folgenden Beispiel aus [Sass-lang.com](http://sass-lang.com):</span><span class="sxs-lookup"><span data-stu-id="364e5-195">Sass supports mixins as well, using the `@mixin` keyword to define them and `@include` to include them, as in this example from [sass-lang.com](http://sass-lang.com):</span></span>

```sass
@mixin border-radius($radius) {
    -webkit-border-radius: $radius;
    -moz-border-radius: $radius;
    -ms-border-radius: $radius;
    border-radius: $radius;
}

.box { @include border-radius(10px); }
```

<span data-ttu-id="364e5-196">Zusätzlich zu Mixins Sass unterstützt auch das Konzept der Vererbung, sodass eine Klasse für eine andere Schnittstelle erweitern.</span><span class="sxs-lookup"><span data-stu-id="364e5-196">In addition to mixins, Sass also supports the concept of inheritance, allowing one class to extend another.</span></span> <span data-ttu-id="364e5-197">Es ist vom Konzept her ähnelt ein Mixin. allerdings führt zu weniger CSS-Code.</span><span class="sxs-lookup"><span data-stu-id="364e5-197">It's conceptually similar to a mixin, but results in less CSS code.</span></span> <span data-ttu-id="364e5-198">Dies wird erreicht, mit der `@extend` Schlüsselwort.</span><span class="sxs-lookup"><span data-stu-id="364e5-198">It's accomplished using the `@extend` keyword.</span></span> <span data-ttu-id="364e5-199">Um Mixins auszuprobieren, fügen Sie den folgenden Ihre *main2.scss* Datei:</span><span class="sxs-lookup"><span data-stu-id="364e5-199">To try out mixins, add the following to your *main2.scss* file:</span></span>

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

<span data-ttu-id="364e5-200">Überprüfen Sie die Ausgabe in *main2.css* nach dem Ausführen der `sass` task **Task Runner-Explorer**:</span><span class="sxs-lookup"><span data-stu-id="364e5-200">Examine the output in *main2.css* after running the `sass` task in **Task Runner Explorer**:</span></span>

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

<span data-ttu-id="364e5-201">Beachten Sie, dass alle die allgemeinen Eigenschaften der Warnung Mixin in jeder Klasse wiederholt werden.</span><span class="sxs-lookup"><span data-stu-id="364e5-201">Notice that all of the common properties of the alert mixin are repeated in each class.</span></span> <span data-ttu-id="364e5-202">Die Mixin hat Ihnen hilft, Duplizierung, die zum Zeitpunkt der Entwicklung zu vermeiden, aber CSS wird weiterhin mit einer Vielzahl von Duplizierungen, größer als der erforderlichen CSS-Dateien – ein mögliches Leistungsproblem erstellt.</span><span class="sxs-lookup"><span data-stu-id="364e5-202">The mixin did a good job of helping eliminate duplication at development time, but it's still creating CSS with a lot of duplication in it, resulting in larger than necessary CSS files - a potential performance issue.</span></span>

<span data-ttu-id="364e5-203">Ersetzen Sie jetzt die Warnung Mixin mit einem `.alert` Klasse, und ändern Sie `@include` zu `@extend` (Denken Sie daran, erweitern Sie `.alert`, nicht `alert`):</span><span class="sxs-lookup"><span data-stu-id="364e5-203">Now replace the alert mixin with a `.alert` class, and change `@include` to `@extend` (remembering to extend `.alert`, not `alert`):</span></span>

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

<span data-ttu-id="364e5-204">Sass einmal ausgeführt, und überprüfen Sie die resultierende CSS:</span><span class="sxs-lookup"><span data-stu-id="364e5-204">Run Sass once more, and examine the resulting CSS:</span></span>

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

<span data-ttu-id="364e5-205">Jetzt sind die Eigenschaften nur als häufig wie nötig definiert, und eine bessere CSS generiert wird.</span><span class="sxs-lookup"><span data-stu-id="364e5-205">Now the properties are defined only as many times as needed, and better CSS is generated.</span></span>

<span data-ttu-id="364e5-206">Sass enthält auch Funktionen und Vorgänge für bedingte Logik, weniger ähnelt.</span><span class="sxs-lookup"><span data-stu-id="364e5-206">Sass also includes functions and conditional logic operations, similar to Less.</span></span> <span data-ttu-id="364e5-207">Die beiden Sprachen Funktionen sind in der Tat sehr ähnlich.</span><span class="sxs-lookup"><span data-stu-id="364e5-207">In fact, the two languages' capabilities are very similar.</span></span>

## <a name="less-or-sass"></a><span data-ttu-id="364e5-208">Weniger oder Sass?</span><span class="sxs-lookup"><span data-stu-id="364e5-208">Less or Sass?</span></span>

<span data-ttu-id="364e5-209">Es ist immer noch keine Konsens gibt an, ob es im Allgemeinen besser, verwenden weniger oder Sass ist (oder sogar an, ob die ursprünglichen Sass oder die neuere SCSS-Syntax im Sass bevorzugen).</span><span class="sxs-lookup"><span data-stu-id="364e5-209">There's still no consensus as to whether it's generally better to use Less or Sass (or even whether to prefer the original Sass or the newer SCSS syntax within Sass).</span></span> <span data-ttu-id="364e5-210">Wahrscheinlich ist die wichtigste Entscheidung **verwenden Sie eines dieser Tools**, im Gegensatz zu nur-handcodierung CSS-Dateien.</span><span class="sxs-lookup"><span data-stu-id="364e5-210">Probably the most important decision is to **use one of these tools**, as opposed to just hand-coding your CSS files.</span></span> <span data-ttu-id="364e5-211">Nachdem Sie auf, die vorgenommen haben der Entscheidung, beide Less und Sass sind gute Optionen.</span><span class="sxs-lookup"><span data-stu-id="364e5-211">Once you've made that decision, both Less and Sass are good choices.</span></span>

## <a name="font-awesome"></a><span data-ttu-id="364e5-212">Font Awesome</span><span class="sxs-lookup"><span data-stu-id="364e5-212">Font Awesome</span></span>

<span data-ttu-id="364e5-213">Zusätzlich zur CSS-Präprozessoren ist eine weitere hervorragende Ressource für moderne Webanwendungen Formatierung Font Awesome.</span><span class="sxs-lookup"><span data-stu-id="364e5-213">In addition to CSS preprocessors, another great resource for styling modern web applications is Font Awesome.</span></span> <span data-ttu-id="364e5-214">Font Awesome ist ein Toolkit, die mehr als 500 skalierbare Vektorgrafiken Symbole bereitstellt, die kostenlos in Ihren Webanwendungen verwendet werden kann.</span><span class="sxs-lookup"><span data-stu-id="364e5-214">Font Awesome is a toolkit that provides over 500 scalable vector icons that can be freely used in your web applications.</span></span> <span data-ttu-id="364e5-215">Es wurde ursprünglich entworfen, um die Arbeit mit Bootstrap, aber es besteht keine Abhängigkeit für dieses Framework oder für alle JavaScript-Bibliotheken.</span><span class="sxs-lookup"><span data-stu-id="364e5-215">It was originally designed to work with Bootstrap, but it has no dependency on that framework or on any JavaScript libraries.</span></span>

<span data-ttu-id="364e5-216">Die einfachste Möglichkeit zum Einstieg in Font Awesome ist einen Verweis auf, mit der öffentlichen Content Delivery Network (CDN)-Speicherort hinzufügen:</span><span class="sxs-lookup"><span data-stu-id="364e5-216">The easiest way to get started with Font Awesome is to add a reference to it, using its public content delivery network (CDN) location:</span></span>

```html
<link rel="stylesheet" href="//maxcdn.bootstrapcdn.com/font-awesome/4.3.0/css/font-awesome.min.css">
```

<span data-ttu-id="364e5-217">Sie können auch hinzufügen es zu Ihrem Visual Studio-Projekt zu der "Dependencies" hinzu, in *"bower.JSON"*:</span><span class="sxs-lookup"><span data-stu-id="364e5-217">You can also add it to your Visual Studio project by adding it to the "dependencies" in *bower.json*:</span></span>

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

<span data-ttu-id="364e5-218">Nachdem Sie einen Verweis auf Font Awesome auf einer Seite haben, können Sie Symbole für Ihre Anwendung hinzufügen, indem Font Awesome Klassen, in der Regel mit dem Präfix "Fa-", um die Inline-HTML-Elemente anwenden (z. B. `<span>` oder `<i>`).</span><span class="sxs-lookup"><span data-stu-id="364e5-218">Once you have a reference to Font Awesome on a page, you can add icons to your application by applying Font Awesome classes, typically prefixed with "fa-", to your inline HTML elements (such as `<span>` or `<i>`).</span></span>  <span data-ttu-id="364e5-219">Beispielsweise können Sie Symbole einfache Listen und Menüs, die mithilfe von Code wie folgt hinzufügen:</span><span class="sxs-lookup"><span data-stu-id="364e5-219">For example, you can add icons to simple lists and menus using code like this:</span></span>

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

<span data-ttu-id="364e5-220">Dies erzeugt die folgenden im Browser – Beachten Sie das Symbol neben dem jedes Element:</span><span class="sxs-lookup"><span data-stu-id="364e5-220">This produces the following in the browser - note the icon beside each item:</span></span>

![Symbole](less-sass-fa/_static/list-icons-screenshot.png)

<span data-ttu-id="364e5-222">Sie können eine vollständige Liste der verfügbaren Symbole hier anzeigen:</span><span class="sxs-lookup"><span data-stu-id="364e5-222">You can view a complete list of the available icons here:</span></span>

http://fontawesome.io/icons/

## <a name="summary"></a><span data-ttu-id="364e5-223">Zusammenfassung</span><span class="sxs-lookup"><span data-stu-id="364e5-223">Summary</span></span>

<span data-ttu-id="364e5-224">Moderne Webanwendungen erfordern immer reaktionsfähig, flüssige-Entwürfe, die übersichtlich, intuitiv und einfach zu verwenden, aus einer Vielzahl von Geräten sind.</span><span class="sxs-lookup"><span data-stu-id="364e5-224">Modern web applications increasingly demand responsive, fluid designs that are clean, intuitive, and easy to use from a variety of devices.</span></span> <span data-ttu-id="364e5-225">Bewältigung der Komplexität von CSS-Stylesheets, die erforderlich, um diese Ziele erreichen, erfolgt am besten verwenden eine ähnliche Präprozessor weniger oder Sass.</span><span class="sxs-lookup"><span data-stu-id="364e5-225">Managing the complexity of the CSS stylesheets required to achieve these goals is best done using a preprocessor like Less or Sass.</span></span> <span data-ttu-id="364e5-226">Darüber hinaus Toolkits wie Font Awesome geben schnell Well-known Text Navigationsmenüs Symbole und Schaltflächen, Verbessern des allgemeinen Benutzers Ihrer Anwendung auftreten.</span><span class="sxs-lookup"><span data-stu-id="364e5-226">In addition, toolkits like Font Awesome quickly provide well-known icons to textual navigation menus and buttons, improving the overall user experience of your application.</span></span>
