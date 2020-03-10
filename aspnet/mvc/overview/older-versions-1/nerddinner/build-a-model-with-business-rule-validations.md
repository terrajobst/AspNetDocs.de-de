---
uid: mvc/overview/older-versions-1/nerddinner/build-a-model-with-business-rule-validations
title: Erstellen eines Modells mit Geschäftsregel Überprüfungen | Microsoft-Dokumentation
author: microsoft
description: In Schritt 3 wird gezeigt, wie ein Modell erstellt wird, das zum Abfragen und Aktualisieren der Datenbank für die "nerddinner"-Anwendung verwendet werden kann.
ms.author: riande
ms.date: 07/27/2010
ms.assetid: 0bc191b2-4311-479a-a83a-7f1b1c32e6fe
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/build-a-model-with-business-rule-validations
msc.type: authoredcontent
ms.openlocfilehash: 6ebf1b71c089229ba9139ff7dc788b8978724046
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78435579"
---
# <a name="build-a-model-with-business-rule-validations"></a>Erstellen eines Modells mit Geschäftsregelüberprüfungen

von [Microsoft](https://github.com/microsoft)

[PDF herunterladen](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Dies ist Schritt 3 des kostenlosen ["nerddinner"](introducing-the-nerddinner-tutorial.md) -Lernprogramms, in dem erläutert wird, wie eine kleine, aber komplette Webanwendung mit ASP.NET MVC 1 erstellt wird.
> 
> In Schritt 3 wird gezeigt, wie ein Modell erstellt wird, das zum Abfragen und Aktualisieren der Datenbank für die "nerddinner"-Anwendung verwendet werden kann.
> 
> Wenn Sie ASP.NET MVC 3 verwenden, empfiehlt es sich, die Tutorials " [Getting Started with MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) " oder " [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) " zu befolgen.

## <a name="nerddinner-step-3-building-the-model"></a>Nerddinner Step 3: Aufbau des Modells

In einem Model-View-Controller-Framework bezieht sich der Begriff "Model" auf die Objekte, die die Daten der Anwendung darstellen, sowie auf die entsprechende Domänen Logik, die Validierungs-und Geschäftsregeln damit integriert. Das Modell ist in vielerlei Hinsicht das "Herz" einer MVC-basierten Anwendung, und wie wir später sehen werden, wird das Verhalten der Anwendung im Grunde genommen.

Das ASP.NET-MVC-Framework unterstützt die Verwendung beliebiger Datenzugriffs Technologien, und Entwickler können aus einer Vielzahl von Rich .NET-Daten Optionen wählen, um Ihre Modelle zu implementieren. dazu gehören: LINQ to Entities, LINQ to SQL, NHibernate, llblgen pro, Subsonic, wilsonorm oder einfach RAW ADO. NET DataReaders oder Datasets.

Für unsere "nerddinner"-Anwendung verwenden wir LINQ to SQL, um ein einfaches Modell zu erstellen, das dem Daten bankentwurf relativ genau entspricht, und fügt einige benutzerdefinierte Validierungs Logik und Geschäftsregeln hinzu. Anschließend implementieren wir eine Repository-Klasse, mit der die Implementierung der Daten Persistenz aus dem Rest der Anwendung abstrahiert werden kann. so können wir den Komponenten Test problemlos durchführen.

### <a name="linq-to-sql"></a>LINQ to SQL

LINQ to SQL ist ein ORM (Object Relational Mapper), der als Teil von .NET 3,5 ausgeliefert wird.

LINQ to SQL bietet eine einfache Möglichkeit zum Zuordnen von Datenbanktabellen zu .NET-Klassen, mit denen Sie Code codieren können. Für unsere "nerddinner"-Anwendung verwenden wir diese, um die Dinner-und RSVP-Tabellen in der Datenbank den Dinner-und RSVP-Klassen zuzuordnen. Die Spalten der Dinner-und RSVP-Tabellen entsprechen den Eigenschaften der Dinner-und RSVP-Klassen. Jedes Dinner-und RSVP-Objekt stellt eine separate Zeile in den Dinner-oder RSVP-Tabellen in der Datenbank dar.

LINQ to SQL ermöglicht es uns, keine SQL-Anweisungen zum Abrufen und Aktualisieren von Dinner-und RSVP-Objekten mit Datenbankdaten manuell zu erstellen. Stattdessen definieren wir die Dinner-und RSVP-Klassen, ihre Zuordnung zu bzw. aus der Datenbank sowie die Beziehungen zwischen Ihnen. LINQ to SQL übernimmt dann die Erstellung der entsprechenden SQL-Ausführungs Logik, die zur Laufzeit verwendet werden soll, wenn wir interagieren und verwenden.

Wir können die LINQ-Sprachunterstützung in VB und C# verwenden, um ausdrucksstarke Abfragen zu schreiben, mit denen Dinner-und RSVP-Objekte aus der Datenbank abgerufen werden. Dies minimiert die Menge an Daten, die wir schreiben müssen, und ermöglicht es uns, wirklich saubere Anwendungen zu erstellen.

### <a name="adding-linq-to-sql-classes-to-our-project"></a>Hinzufügen von LINQ to SQL Klassen zum Projekt

Beginnen Sie mit der rechten Maustaste auf den Ordner "Models" in unserem Projekt, und wählen Sie den Menübefehl **Add-&gt;New Item** :

![](build-a-model-with-business-rule-validations/_static/image1.png)

Dadurch wird das Dialogfeld "Neues Element hinzufügen" angezeigt. Wir Filtern nach der Kategorie "Data" und wählen die Vorlage "LINQ to SQL Classes" aus:

![](build-a-model-with-business-rule-validations/_static/image2.png)

Wir geben dem Element den Namen "nerddinner" und klicken auf die Schaltfläche "Add" (hinzufügen). Visual Studio fügt eine Datei "nerddinner. dbml" in das Verzeichnis "\models" ein und öffnet dann den relationalen Designer des LINQ to SQL Objekts:

![](build-a-model-with-business-rule-validations/_static/image3.png)

### <a name="creating-data-model-classes-with-linq-to-sql"></a>Erstellen von Datenmodell Klassen mit LINQ to SQL

LINQ to SQL ermöglicht es uns, schnell Datenmodell Klassen aus einem vorhandenen Datenbankschema zu erstellen. Zu diesem Zweck öffnen Sie die Datenbank "nerddinner" im Server-Explorer und wählen die Tabellen aus, die in Ihr modelliert werden sollen:

![](build-a-model-with-business-rule-validations/_static/image4.png)

Anschließend können Sie die Tabellen auf die LINQ to SQL Designer-Oberfläche ziehen. Wenn wir dies tun, erstellt LINQ to SQL automatisch Dinner-und RSVP-Klassen mit dem Schema der Tabellen (mit Klasseneigenschaften, die den Spalten der Datenbanktabelle zugeordnet werden):

![](build-a-model-with-business-rule-validations/_static/image5.png)

Standardmäßig werden Tabellen-und Spaltennamen vom LINQ to SQL-Designer automatisch "pluralisiert", wenn Klassen basierend auf einem Datenbankschema erstellt werden. Beispiel: die Tabelle "Dinner" in unserem obigen Beispiel führte zu einer "Dinner"-Klasse. Diese benennungsklasse trägt dazu bei, dass unsere Modelle mit den .net-Benennungs Konventionen konsistent sind, und ich finde, dass der Designer dies bequem beheben kann (insbesondere beim Hinzufügen von vielen Tabellen). Wenn Sie den Namen einer Klasse oder Eigenschaft, die der Designer generiert, nicht kennen, können Sie Sie jederzeit überschreiben und in einen beliebigen Namen ändern. Hierzu können Sie entweder den Entitäts-/Eigenschaftennamen innerhalb des Designers inline bearbeiten oder ihn über das Eigenschaften Raster ändern.

Standardmäßig überprüft der LINQ to SQL-Designer auch die Primärschlüssel-/Fremdschlüssel Beziehungen der Tabellen und erstellt automatisch standardmäßige "Beziehungs Zuordnungen" zwischen den verschiedenen Modellklassen, die er erstellt. Wenn wir z. b. die Dinner-und RSVP-Tabellen auf den LINQ to SQL-Designer gezogen haben, wurde eine 1: n-Beziehungs Zuordnung zwischen den beiden abgeleitet, basierend darauf, dass die RSVP-Tabelle einen Fremdschlüssel für die Dinner-Tabelle enthielt (Dies wird durch den Pfeil im -Designer):

![](build-a-model-with-business-rule-validations/_static/image6.png)

Die obige Zuordnung bewirkt, dass LINQ to SQL eine stark typisierte "Dinner"-Eigenschaft zur RSVP-Klasse hinzufügt, die Entwickler verwenden können, um auf das Dinner zuzugreifen, das mit einer bestimmten RSVP verknüpft ist. Außerdem bewirkt dies, dass die Dinner-Klasse über eine "RSVPs"-Auflistungs Eigenschaft verfügt, mit der Entwickler RSVP-Objekte abrufen und aktualisieren können, die einem bestimmten Dinner zugeordnet sind.

Im folgenden finden Sie ein Beispiel für IntelliSense in Visual Studio, wenn wir ein neues RSVP-Objekt erstellen und es der RSVPs-Sammlung eines Dinner hinzufügen. Beachten Sie, dass LINQ to SQL automatisch eine "RSVPs"-Sammlung für das Dinner-Objekt hinzugefügt hat:

![](build-a-model-with-business-rule-validations/_static/image7.png)

Durch das Hinzufügen des RSVP-Objekts zur RSVPs-Auflistung des Dinner wird LINQ to SQL angewiesen, eine Fremdschlüssel Beziehung zwischen dem Dinner-und der RSVP-Zeile in unserer Datenbank zuzuordnen:

![](build-a-model-with-business-rule-validations/_static/image8.png)

Wenn Sie nicht möchten, wie der Designer eine Tabellen Zuordnung modelliert oder benannt hat, können Sie Sie überschreiben. Klicken Sie im Designer einfach auf den Zuordnungs Pfeil, und greifen Sie über das Eigenschaften Raster auf seine Eigenschaften zu, um es umzubenennen, zu löschen oder zu ändern. Für unsere "nerddinner"-Anwendung funktionieren die Standard Zuordnungs Regeln jedoch gut für die Datenmodell Klassen, die wir aufbauen, und wir können einfach das Standardverhalten verwenden.

### <a name="nerddinnerdatacontext-class"></a>Nerddinnerdatacontext-Klasse

Visual Studio erstellt automatisch .NET-Klassen, die die Modelle und Daten Bankbeziehungen darstellen, die mit dem LINQ to SQL-Designer definiert werden. Eine LINQ to SQL DataContext-Klasse wird auch für jede LINQ to SQL Designer-Datei generiert, die der Projekt Mappe hinzugefügt wird. Da wir unser LINQ to SQL-Klassen Element "nerddinner" genannt haben, wird die erstellte DataContext-Klasse als "nerddinnerdatacontext" bezeichnet. Diese nerddinnerdatacontext-Klasse ist die primäre Methode, mit der die Datenbank interagiert.

Unsere Klasse "nerddinnerdatacontext" macht zwei Eigenschaften verfügbar: "Dinner" und "RSVPs", die die beiden Tabellen darstellen, die in der Datenbank modelliert wurden. Wir können verwenden C# , um LINQ-Abfragen für diese Eigenschaften zu schreiben, um Dinner-und RSVP-Objekte aus der Datenbank abzufragen und abzurufen.

Der folgende Code veranschaulicht, wie ein "nerddinnerdatacontext"-Objekt instanziiert und eine LINQ-Abfrage ausgeführt wird, um eine Sequenz von Abendessen zu erhalten, die in der Zukunft auftreten. Visual Studio stellt beim Schreiben der LINQ-Abfrage eine vollständige IntelliSense-Funktion bereit, und die von ihr zurückgegebenen Objekte sind stark typisiert und unterstützen auch IntelliSense:

![](build-a-model-with-business-rule-validations/_static/image9.png)

Ein "nerddinnerdatacontext" ermöglicht nicht nur die Abfrage von Dinner-und RSVP-Objekten, sondern auch automatisch alle Änderungen, die wir anschließend an den Dinner-und RSVP-Objekten vornehmen, die wir durch die Tabelle abrufen. Wir können diese Funktion verwenden, um die Änderungen problemlos in der Datenbank zu speichern, ohne expliziten SQL-Update Code schreiben zu müssen.

Der folgende Code veranschaulicht z. b., wie eine LINQ-Abfrage verwendet wird, um ein einzelnes Dinner-Objekt aus der Datenbank abzurufen, zwei der Dinner-Eigenschaften zu aktualisieren und die Änderungen dann in der Datenbank wieder zu speichern:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample1.cs)]

Das Objekt "nerddinnerdatacontext" im obigen Code hat die an dem Dinner-Objekt vorgenommenen Eigenschaften Änderungen, die wir daraus abgerufen haben, automatisch nachverfolgt. Wenn wir die Methode "SubmitChanges ()" aufgerufen haben, wird eine entsprechende SQL-Anweisung "Update" für die Datenbank ausgeführt, um die aktualisierten Werte wieder zu speichern.

### <a name="creating-a-dinnerrepository-class"></a>Erstellen einer dinnerrepository-Klasse

Bei kleinen Anwendungen ist es manchmal gut, dass Controller direkt für eine LINQ to SQL DataContext-Klasse funktionieren und LINQ-Abfragen innerhalb der Controller einbetten. Wenn Anwendungen jedoch größer werden, wird dieser Ansatz für die Wartung und den Test mühsam. Dies kann auch dazu führen, dass wir dieselben LINQ-Abfragen an mehreren Stellen duplizieren.

Ein Ansatz, mit dem Anwendungen leichter zu verwalten und zu testen sind, ist die Verwendung eines "Repository"-Musters. Eine Repository-Klasse unterstützt das Kapseln von Daten Abfragen und Persistenzlogik und abstrahiert die Implementierungsdetails der Daten Persistenz aus der Anwendung. Mit einem Repository-Muster können Sie nicht nur Anwendungs Codes bereinigen, sondern auch das Ändern von Daten Speicherungs Implementierungen in Zukunft vereinfachen und das Komponenten Testen einer Anwendung vereinfachen, ohne dass eine echte Datenbank erforderlich ist.

Für unsere "nerddinner"-Anwendung definieren wir eine dinnerrepository-Klasse mit der folgenden Signatur:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample2.cs)]

*Hinweis: später in diesem Kapitel extrahieren wir eine idinnerrepository-Schnittstelle aus dieser Klasse und aktivieren die Abhängigkeitsinjektion mit ihr auf unseren Controllern. Zunächst einmal beginnen wir mit der Verwendung von Simple und arbeiten einfach direkt mit der dinnerrepository-Klasse.*

Um diese Klasse zu implementieren, klicken Sie mit der rechten Maustaste auf den Ordner "Models", und wählen Sie den Menübefehl **Add-&gt;New Item** aus. Im Dialogfeld "Neues Element hinzufügen" wählen Sie die Vorlage "Class" aus und benennen die Datei "DinnerRepository.cs":

![](build-a-model-with-business-rule-validations/_static/image10.png)

Anschließend können wir unsere dinnerrepository-Klasse mithilfe des folgenden Codes implementieren:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample3.cs)]

### <a name="retrieving-updating-inserting-and-deleting-using-the-dinnerrepository-class"></a>Abrufen, aktualisieren, einfügen und löschen mithilfe der dinnerrepository-Klasse

Nachdem wir nun unsere dinnerrepository-Klasse erstellt haben, sehen wir uns einige Codebeispiele an, in denen häufige Aufgaben veranschaulicht werden, die wir damit ausführen können:

#### <a name="querying-examples"></a>Abfragen von Beispielen

Der folgende Code Ruft ein einzelnes Dinner mit dem dinnerid-Wert ab:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample4.cs)]

Der folgende Code Ruft alle bevorstehenden Abendessen ab und durchläuft Sie:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample5.cs)]

#### <a name="insert-and-update-examples"></a>INSERT-und Update-Beispiele

Der folgende Code veranschaulicht das Hinzufügen von zwei neuen Abendessen. Ergänzungen/Änderungen am Repository werden erst in der Datenbank ausgeführt, wenn die "Save ()"-Methode dafür aufgerufen wird. LINQ to SQL automatisch alle Änderungen in einer Datenbanktransaktion umschließt – so werden entweder alle Änderungen durchgeführt, oder keine von Ihnen, wenn unser Repository speichert:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample6.cs)]

Der folgende Code Ruft ein vorhandenes Dinner-Objekt ab und ändert zwei Eigenschaften. Die Änderungen werden an die Datenbank zurückgestellt, wenn die "Save ()"-Methode in unserem Repository aufgerufen wird:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample7.cs)]

Der folgende Code Ruft ein Dinner ab und fügt ihm dann eine RSVP hinzu. Dies erfolgt mithilfe der RSVPs-Auflistung auf dem Dinner-Objekt, das für uns erstellt LINQ to SQL (weil es eine Primärschlüssel-/Fremdschlüssel Beziehung zwischen den beiden in der Datenbank gibt). Diese Änderung wird als neue RSVP-Tabellenzeile wieder in der Datenbank gespeichert, wenn die "Save ()"-Methode für das Repository aufgerufen wird:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample8.cs)]

#### <a name="delete-example"></a>DELETE-Beispiel

Der folgende Code Ruft ein vorhandenes Dinner-Objekt ab und kennzeichnet es als Löschvorgang. Wenn die "Save ()"-Methode für das Repository aufgerufen wird, wird der Löschvorgang für den Löschvorgang wieder in die Datenbank durchführt:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample9.cs)]

### <a name="integrating-validation-and-business-rule-logic-with-model-classes"></a>Integrieren von Validierungs-und Geschäftsregel Logik in Modellklassen

Das Integrieren von Validierungs-und Geschäftsregel Logik ist ein wichtiger Bestandteil jeder Anwendung, die mit Daten arbeitet.

#### <a name="schema-validation"></a>Schema Validierung

Wenn Modellklassen mit dem LINQ to SQL-Designer definiert werden, entsprechen die Datentypen der Eigenschaften in den Datenmodell Klassen den Datentypen der Datenbanktabelle. Beispiel: Wenn die Spalte "eventdate" in der Tabelle "Dinners" ein "DateTime" ist, ist die von LINQ to SQL erstellte Datenmodell Klasse vom Typ "DateTime" (ein integrierter .NET-Datentyp). Dies bedeutet, dass Sie Kompilierungsfehler erhalten, wenn Sie versuchen, eine ganze Zahl oder einen booleschen Wert aus dem Code zuzuweisen, und es wird automatisch ein Fehler ausgegeben, wenn Sie versuchen, einen nicht gültigen Zeichen Folgentyp zur Laufzeit implizit zu konvertieren.

LINQ to SQL werden bei der Verwendung von Zeichen folgen auch automatisch Escapesequenzen von SQL für Sie behandelt, was Sie bei der Verwendung von SQL-Injection-Angriffen schützt.

#### <a name="validation-and-business-rule-logic"></a>Validierungs-und Geschäftsregel Logik

Die Schema Validierung ist als erster Schritt nützlich, aber nur selten genug. In den meisten realen Szenarios ist es möglich, eine umfassendere Validierungs Logik anzugeben, die mehrere Eigenschaften umfassen, Code ausführen und häufig den Status eines Modells überprüfen kann (z. b., wenn er erstellt wird/updated/Deleted oder innerhalb eines domänenspezifischen Zustands wie "archiviert"). Es gibt eine Vielzahl von verschiedenen Mustern und Frameworks, die zum Definieren und Anwenden von Validierungsregeln für Modellklassen verwendet werden können. es gibt mehrere .NET-basierte Frameworks, die dazu verwendet werden können. Sie können in ASP.NET MVC-Anwendungen beliebig viele verwenden.

Für die Zwecke unserer "nerddinner"-Anwendung verwenden wir ein verhältnismäßig einfaches und geradliniges Muster, bei dem wir eine IsValid-Eigenschaft und eine getruleverletzungs ()-Methode für das Dinner Model-Objekt verfügbar machen. Die IsValid-Eigenschaft gibt "true" oder "false" zurück, je nachdem, ob die Validierungs-und Geschäftsregeln gültig sind. Die getruleverstöße ()-Methode gibt eine Liste aller Regel Fehler zurück.

Wir implementieren "IsValid" und "getruletemplates ()" für unser Dinner Model, indem wir dem Projekt eine "partielle Klasse" hinzufügen. Partielle Klassen können verwendet werden, um Methoden/Eigenschaften/Ereignisse zu Klassen hinzuzufügen, die von einem vs-Designer verwaltet werden (wie z. b. die Dinner-Klasse, die vom LINQ to SQL-Designer generiert wird) Wir können dem Projekt eine neue partielle Klasse hinzufügen, indem Sie mit der rechten Maustaste auf den Ordner "\models" klicken und dann den Menübefehl "Neues Element hinzufügen" auswählen. Anschließend können Sie im Dialogfeld "Neues Element hinzufügen" die Vorlage "Klasse" auswählen und ihr den Namen "Dinner.cs" geben.

![](build-a-model-with-business-rule-validations/_static/image11.png)

Wenn Sie auf die Schaltfläche "hinzufügen" klicken, wird dem Projekt eine Dinner.cs-Datei hinzugefügt und in der IDE geöffnet. Anschließend können wir mithilfe des folgenden Codes ein grundlegendes Framework für die Regel-/validierungsdurchsetzung implementieren:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample10.cs)]

Einige Hinweise zum obigen Code:

- Der Dinner-Klasse wird ein partielles Schlüsselwort vorangestellt – Dies bedeutet, dass der darin enthaltene Code mit der vom LINQ to SQL-Designer generierten/verwalteten Klasse kombiniert und in eine einzelne Klasse kompiliert wird.
- Die ruleverletzungs-Klasse ist eine Hilfsklasse, die wir dem Projekt hinzufügen, das es uns ermöglicht, weitere Details zu einer Regelverletzung bereitzustellen.
- Die Dinner. getruleverletzungs ()-Methode bewirkt, dass unsere Validierungs-und Geschäftsregeln ausgewertet werden (wir werden Sie in Kürze implementieren). Anschließend wird eine Sequenz von ruleverletzungs-Objekten zurückgegeben, die ausführlichere Informationen zu Regel Fehlern bereitstellen.
- Die Dinner. IsValid-Eigenschaft stellt eine bequeme Hilfseigenschaft bereit, die angibt, ob das Dinner-Objekt über aktive ruleverstöße verfügt. Sie kann von Entwicklern proaktiv überprüft werden, indem Sie das Dinner-Objekt jederzeit verwenden (und keine Ausnahme auslöst).
- Die partielle Dinner. OnValidate ()-Methode ist ein Hook, den LINQ to SQL bereitstellt, mit dem wir jederzeit benachrichtigt werden können, wenn das Dinner-Objekt in der Datenbank persistent gespeichert wird. Unsere oben genannte OnValidate ()-Implementierung stellt sicher, dass für das Dinner keine ruleverletzungs Verletzungen vorliegen, bevor es gespeichert wird. Wenn Sie sich in einem ungültigen Zustand befindet, wird eine Ausnahme ausgelöst, die dazu führt, dass LINQ to SQL die Transaktion abbricht.

Diese Vorgehensweise bietet ein einfaches Framework, in das wir Validierungs-und Geschäftsregeln integrieren können. Nun fügen wir die unten aufgeführten Regeln zur getruleverletzungs ()-Methode hinzu:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample11.cs)]

Wir verwenden die Funktion "yield return" von C# , um eine Sequenz von ruleverstößen zurückzugeben. Die ersten sechs Regelprüfungen oben erzwingen einfach, dass die Zeichen folgen Eigenschaften in unserem Dinner nicht NULL oder leer sein dürfen. Die letzte Regel ist ein wenig interessanter und ruft eine phonevalidator. isvalidnumber ()-Hilfsmethode auf, die wir dem Projekt hinzufügen können, um zu überprüfen, ob das contactphone-Zahlenformat mit dem Land des Abendessens übereinstimmt.

Wir können verwenden. Unterstützung für reguläre Ausdrücke von NET zur Implementierung dieser Telefon Validierungs Unterstützung. Im folgenden finden Sie eine einfache phonevalidator-Implementierung, die wir dem Projekt hinzufügen können, mit der wir länderspezifische Regex-Muster Überprüfungen hinzufügen können:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample12.cs)]

#### <a name="handling-validation-and-business-logic-violations"></a>Behandeln von Validierungs-und Geschäftslogik Verstößen

Nachdem wir nun den obigen Validierungs-und Geschäftsregel Code hinzugefügt haben, werden wir jedes Mal, wenn wir versuchen, ein Dinner zu erstellen oder zu aktualisieren, die Regeln für die Validierungs Logik ausgewertet und erzwungen.

Entwickler können wie unten beschrieben Code schreiben, um proaktiv festzustellen, ob ein Dinner-Objekt gültig ist, und eine Liste aller Verstöße darin abrufen, ohne Ausnahmen zu verursachen:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample13.cs)]

Wenn Sie versuchen, ein Dinner in einem ungültigen Zustand zu speichern, wird eine Ausnahme ausgelöst, wenn die Save ()-Methode für das dinnerrepository aufgerufen wird. Der Grund hierfür ist, dass LINQ to SQL die partielle Dinner. OnValidate ()-Methode automatisch aufruft, bevor die Änderungen am Dinner gespeichert werden, und wir haben Code zu Dinner. OnValidate () hinzugefügt, um eine Ausnahme zu verursachen, wenn Regel Verletzungen im Abendessen vorhanden sind. Wir können diese Ausnahme abfangen und eine Liste der zu befolgenden Verstöße reaktiv abrufen:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample14.cs)]

Da unsere Validierungs-und Geschäftsregeln innerhalb unserer Modell Ebene und nicht innerhalb der UI-Schicht implementiert werden, werden Sie in allen Szenarien in unserer Anwendung angewendet und verwendet. Wir können später Geschäftsregeln ändern oder hinzufügen, sodass der gesamte Code, der mit unseren Dinner-Objekten zusammenarbeitet, Sie berücksichtigt.

Die Flexibilität, Geschäftsregeln an einem Ort zu ändern, ohne dass diese Änderungen in der gesamten Anwendungs-und Benutzeroberflächen Logik durchgeführt werden, ist ein Vorzeichen für eine gut geschriebene Anwendung und ein Vorteil, dass ein MVC-Framework ermutigt wird.

### <a name="next-step"></a>Nächster Schritt

Wir haben nun ein Modell, mit dem wir unsere Datenbank Abfragen und aktualisieren können.

Nun fügen wir dem Projekt einige Controller und Ansichten hinzu, die wir verwenden können, um eine HTML-Benutzeroberfläche für die Benutzeroberfläche zu erstellen.

> [!div class="step-by-step"]
> [Zurück](create-a-database.md)
> [Weiter](use-controllers-and-views-to-implement-a-listingdetails-ui.md)
