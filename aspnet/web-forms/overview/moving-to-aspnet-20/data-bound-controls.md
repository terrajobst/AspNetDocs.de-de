---
uid: web-forms/overview/moving-to-aspnet-20/data-bound-controls
title: Daten gebundene Steuerelemente | Microsoft-Dokumentation
author: microsoft
description: Die meisten ASP.NET-Anwendungen basieren auf einem gewissen Grad an Datendarstellung aus einer Back-End-Datenquelle. Daten gebundene Steuerelemente waren ein entscheidender Bestandteil der Interaktion von w...
ms.author: riande
ms.date: 02/20/2005
ms.assetid: 0e23ff32-646d-43f3-8bec-6b2313d3abd6
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/data-bound-controls
msc.type: authoredcontent
ms.openlocfilehash: 1154b38e0fa3d9d56cb407ae659c3b0d69871fda
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78488871"
---
# <a name="data-bound-controls"></a>Datengebundene Steuerelemente

von [Microsoft](https://github.com/microsoft)

> Die meisten ASP.NET-Anwendungen basieren auf einem gewissen Grad an Datendarstellung aus einer Back-End-Datenquelle. Daten gebundene Steuerelemente waren ein entscheidender Bestandteil der Interaktion mit Daten in dynamischen Webanwendungen. ASP.NET 2,0 führt einige wesentliche Verbesserungen für Daten gebundene Steuerelemente ein, einschließlich einer neuen BaseDataBoundControl-Klasse und deklarativer Syntax.

Die meisten ASP.NET-Anwendungen basieren auf einem gewissen Grad an Datendarstellung aus einer Back-End-Datenquelle. Daten gebundene Steuerelemente waren ein entscheidender Bestandteil der Interaktion mit Daten in dynamischen Webanwendungen. ASP.NET 2,0 führt einige wesentliche Verbesserungen für Daten gebundene Steuerelemente ein, einschließlich einer neuen BaseDataBoundControl-Klasse und deklarativer Syntax.

BaseDataBoundControl fungiert als Basisklasse für die DataBoundControl-Klasse und die hierarchcaldataboundcontrol-Klasse. In diesem Modul werden die folgenden Klassen erläutert, die von DataBoundControl abgeleitet werden:

- AdRotator
- Listen Steuerelemente
- GridView
- FormView
- DetailsView

Außerdem werden die folgenden Klassen erläutert, die von der hierarchcaldataboundcontrol-Klasse abgeleitet werden:

- TreeView
- Menü
- SiteMapPath

## <a name="databoundcontrol-class"></a>DataBoundControl-Klasse

Die DataBoundControl-Klasse ist eine abstrakte Klasse (in VB als MustInherit markiert), die für die Interaktion mit Tabellen-oder Listenformat Daten verwendet wird. Die folgenden Steuerelemente sind einige der Steuerelemente, die von DataBoundControl abgeleitet werden.

## <a name="adrotator"></a>AdRotator

Mit dem AdRotator-Steuerelement können Sie ein Grafik Banner auf einer Webseite anzeigen, die mit einer bestimmten URL verknüpft ist. Die Grafik, die angezeigt wird, wird mit den Eigenschaften für das-Steuerelement gedreht. Die Häufigkeit, mit der eine bestimmte Werbung auf einer Seite angezeigt wird, kann mithilfe der Eigenschaft " **Impressions** " konfiguriert werden, und die Anzeige kann mithilfe der Schlüsselwort Filterung gefiltert werden

AdRotator-Steuerelemente verwenden entweder eine XML-Datei oder eine Tabelle in einer Datenbank, um Daten zu verwenden. Die folgenden Attribute werden in XML-Dateien verwendet, um das AdRotator-Steuerelement zu konfigurieren.

### <a name="imageurl"></a>ImageUrl
Die URL eines Bilds, das für die Anzeige angezeigt werden soll.

### <a name="navigateurl"></a>NavigateUrl
Die URL, zu der der Benutzer beim Klicken auf die Werbeaktion gelangen soll. Dies sollte URL-codiert sein.

### <a name="alternatetext"></a>AlternateText
Der Alternative Text, der in einer QuickInfo angezeigt wird und von den Sprachausgaben gelesen wird. Wird auch angezeigt, wenn das von ImageUrl angegebene Bild nicht verfügbar ist.

### <a name="keyword"></a>Schlüsselwort
Definiert ein Schlüsselwort, das verwendet werden kann, wenn die Schlüsselwort Filterung verwendet wird. Wenn angegeben, werden nur die Werbeeinblendungen angezeigt, die mit dem Schlüsselwort Filter übereinstimmen.

### <a name="impressions"></a>Vermittelt
Eine Gewichtungs Zahl, die bestimmt, wie oft eine bestimmte Werbe eingeblendet wird. Es ist relativ zum Eindruck anderer anzeigen in derselben Datei. Der maximale Wert der kollektiven Eindrücke für alle Werbeeinblendungen in einer XML-Datei ist 2,048 Milliarden 1.

### <a name="height"></a>Höhe
Die Höhe der Werbeanzeige in Pixel.

### <a name="width"></a>Breite
Die Breite der Werbeanzeige in Pixel.

> [!NOTE]
> Die Attribute height und Width überschreiben die Höhe und Breite für das AdRotator-Steuerelement selbst.

Eine typische XML-Datei könnte wie folgt aussehen:

[!code-xml[Main](data-bound-controls/samples/sample1.xml)]

Im obigen Beispiel wird die Werbe einblendstellung für "Azure" aufgrund des Werts für das Attribut "Impressions" zweimal als AD für die Website "ASP.net" angezeigt.

Fügen Sie einer Seite ein AdRotator-Steuerelement hinzu, und legen Sie die Eigenschaft " **infosementfile** " so fest, dass Sie auf die XML-Datei zeigt, wie unten dargestellt:

[!code-aspx[Main](data-bound-controls/samples/sample2.aspx)]

Wenn Sie eine Datenbanktabelle als Datenquelle für das AdRotator-Steuerelement verwenden möchten, müssen Sie zunächst eine Datenbank mithilfe des folgenden Schemas einrichten:

| **Spaltenname** | **Datentyp** | **Beschreibung** |
| --- | --- | --- |
| ID | int | Der Primärschlüssel. Diese Spalte kann einen beliebigen Namen haben. |
| ImageUrl | nvarchar (*Länge*) | Der relative oder absolute URL des Bilds, das für die Anzeige angezeigt werden soll. |
| NavigateUrl | nvarchar (*Länge*) | Die Ziel-URL für die Werbeanzeige. Wenn Sie keinen Wert angeben, handelt es sich bei der Werbeanzeige nicht um einen Hyperlink. |
| AlternateText | nvarchar (*Länge*) | Der Text, der angezeigt wird, wenn das Bild nicht gefunden werden kann. In einigen Browsern wird der Text als QuickInfo angezeigt. Alternativer Text wird auch für den Zugriff verwendet, damit Benutzer, die die Grafik nicht sehen können, Ihre Beschreibung hören können. |
| Schlüsselwort | nvarchar (*Länge*) | Eine Kategorie für die Werbe einformationen, auf der die Seite filtern kann. |
| Vermittelt | int (4) | Eine Zahl, die die Wahrscheinlichkeit angibt, wie oft die Anzeige angezeigt wird. Je größer die Zahl ist, desto häufiger wird die Werbe eingeblendet angezeigt. Die Summe aller Impressions-Werte in der XML-Datei ist möglicherweise nicht größer als 2,048 Milliarden-1. |
| Breite | int (4) | Die Breite des Bilds in Pixel. |
| Höhe | int (4) | Die Höhe des Bilds in Pixel. |

In Fällen, in denen Sie bereits über eine Datenbank mit einem anderen Schema verfügen, können Sie die Eigenschaften " **Alternative Textfeld**", " **ImageUrlField**" und " **NavigateUrlField** " verwenden, um die AdRotator-Attribute Ihrer vorhandenen Datenbank zuzuordnen. Fügen Sie der Seite ein Datenquellen-Steuerelement hinzu, konfigurieren Sie die Verbindungs Zeichenfolge für das Datenquellen-Steuerelement, um auf Ihre Datenbank zu verweisen, und legen Sie die **DataSourceID** -Eigenschaft des AdRotator-Steuer Elements auf die ID des Datenquellen-Steuer Elements fest, um die Daten der Datenbank im AdRotator-Steuerelement anzuzeigen In Fällen, in denen Sie AdRotator-Werbeeinblendungen Programm gesteuert konfigurieren müssen, verwenden Sie das AdCreated-Ereignis. Das AdCreated-Ereignis nimmt zwei Parameter an: ein Objekt und das andere eine Instanz von adkreatedeventargs. Adkreatedeventargs ist ein Verweis auf das AD, das erstellt wird.

Der folgende Code Ausschnitt legt die Werte für "Image URL", "NavigateUrl" und "alternativen Text" für eine Werbeeinblendungen Programm gesteuert fest

[!code-csharp[Main](data-bound-controls/samples/sample3.cs)]

## <a name="list-controls"></a>Listen Steuerelemente

Listen Steuerelemente enthalten ListBox, DropDownList, CheckBoxList, RadioButtonList und BulletedList. Jedes dieser Steuerelemente kann an eine Datenquelle gebunden werden. Sie verwenden ein Feld in der Datenquelle als Anzeige Text und können optional ein zweites Feld als Wert eines Elements verwenden. Elemente können auch statisch zur Entwurfszeit hinzugefügt werden, und Sie können statische Elemente und dynamische Elemente mischen, die aus einer Datenquelle hinzugefügt werden.

Fügen Sie der Seite ein Datenquellen-Steuerelement hinzu, um ein Listen Steuerelement zu binden. Geben Sie einen SELECT-Befehl für das Datenquellen-Steuerelement an, und legen Sie dann die DataSourceID-Eigenschaft des Listen Steuer Elements auf die ID des Datenquellen-Steuer Elements fest. Verwenden Sie die Eigenschaften **DataTextField** und **DataValueField** , um den Anzeige Text und den Wert für das Steuerelement zu definieren. Darüber hinaus können Sie die **DataTextFormatString** -Eigenschaft verwenden, um die Darstellung des Anzeige Texts wie folgt zu steuern:

| **Ausdruck** | **Beschreibung** |
| --- | --- |
| Preis: {0:C} | Für numerische/Dezimal Daten. Zeigt die Literale "Price:" gefolgt von Zahlen im Währungs Format an. Das Währungs Format hängt von der Kultur Einstellung ab, die im Culture-Attribut der **Page** -Direktive oder in der Web. config-Datei angegeben ist. |
| {0:D4} | Für ganzzahlige Daten. Kann nicht mit Dezimalzahlen verwendet werden. Ganze Zahlen werden in einem NULL-aufgefüllten Feld angezeigt, das vier Zeichen breit ist. |
| {0:N2}% | Für numerische Daten. Zeigt die Zahl mit einer Genauigkeit von 2 Dezimalstellen an, gefolgt vom Literalzeichen "%". |
| {0:000.0} | Für numerische/Dezimal Daten. Zahlen werden auf eine Dezimalstelle gerundet. Zahlen mit weniger als drei Ziffern werden durch Nullen ergänzt. |
| {0:D} | Für Datums-/Uhrzeitdaten. Zeigt das lange Datumsformat an ("Donnerstag, 06. August, 1996"). Das Datumsformat hängt von der Kultureinstellung der Seite oder der Datei {1}Web.config{2} ab. |
| {0:d} | Für Datums-/Uhrzeitdaten. Zeigt das kurze Datumsformat an ("12/31/99"). |
| {0:JJ-MM-TT} | Für Datums-/Uhrzeitdaten. Zeigt das Datum im numerischen Jahr-Monat-Tag-Format an (96-08-06). |

## <a name="gridview"></a>GridView

Das GridView-Steuerelement ermöglicht die Anzeige und Bearbeitung von Tabellendaten mithilfe eines deklarativen Ansatzes und ist der Nachfolger des DataGrid-Steuer Elements. Die folgenden Funktionen sind im GridView-Steuerelement verfügbar.

- Binden an Datenquellen Steuerelemente, z. b. SqlDataSource.
- Integrierte Sortierfunktionen.
- Integrierte Aktualisierungs-und Löschfunktionen.
- Integrierte Paging-Funktionen.
- Integrierte Zeilenauswahl Funktionen.
- Programm gesteuerter Zugriff auf das GridView-Objektmodell, um Eigenschaften dynamisch festzulegen, Ereignisse zu behandeln usw.
- Mehrere Schlüsselfelder.
- Mehrere Datenfelder für die Hyperlink-Spalten.
- Anpassbare Darstellung durch Designs und Stile.

**Spalten Felder**

Jede Spalte im GridView-Steuerelement wird durch ein DataControlField-Objekt dargestellt. Standardmäßig ist die autogeneratecolenumns-Eigenschaft auf **true**festgelegt, wodurch ein AutoGeneratedField-Objekt für jedes Feld in der Datenquelle erstellt wird. Jedes Feld wird dann als Spalte im GridView-Steuerelement in der Reihenfolge gerendert, in der die einzelnen Felder in der Datenquelle angezeigt werden. Sie können auch manuell steuern, welche Spalten Felder im **GridView** -Steuerelement angezeigt werden, indem Sie die **autogeneratecolenns** -Eigenschaft auf **false** festlegen und dann eine eigene Spalten Feld Auflistung definieren. Verschiedene Spalten Feldtypen bestimmen das Verhalten der Spalten im-Steuerelement.

In der folgenden Tabelle werden die verschiedenen Spalten Feldtypen aufgelistet, die verwendet werden können.

| **Spalten Feldtyp** | **Beschreibung** |
| --- | --- |
| BoundField | Zeigt den Wert eines Felds in einer Datenquelle an. Dies ist der Standard Spaltentyp des GridView-Steuer Elements. |
| ButtonField | Zeigt eine Befehls Schaltfläche für jedes Element im GridView-Steuerelement an. Auf diese Weise können Sie eine Spalte mit benutzerdefinierten Schaltflächen-Steuerelementen erstellen, z. b. die Schaltfläche Hinzufügen oder entfernen. |
| CheckBoxField | Zeigt ein Kontrollkästchen für jedes Element im GridView-Steuerelement an. Dieser Spalten Feldtyp wird häufig verwendet, um Felder mit einem booleschen Wert anzuzeigen. |
| CommandField | Zeigt vordefinierte Befehls Schaltflächen zum Ausführen von Auswahl-, Bearbeitungs-oder Lösch Vorgängen. |
| HyperLinkField | Zeigt den Wert eines Felds in einer Datenquelle als Hyperlink an. Dieser Spalten Feldtyp ermöglicht es Ihnen, ein zweites Feld an die URL des Links zu binden. |
| ImageField | Zeigt für jedes Element im GridView-Steuerelement ein Bild an. |
| TemplateField | Zeigt einen benutzerdefinierten Inhalt für jedes Element im GridView-Steuerelement entsprechend einer angegebenen Vorlage an. Dieser Spalten Feldtyp ermöglicht das Erstellen eines benutzerdefinierten Spalten Felds. |

Um eine Spalten Feld Auflistung deklarativ zu definieren, fügen Sie zunächst öffnende und schließende **&lt;Spalten&gt;** Tags zwischen den öffnenden und schließenden Tags des GridView-Steuer Elements hinzu. Auflisten Sie als nächstes die Spalten Felder, die Sie zwischen den öffnenden und schließenden **&lt;Spalten&gt;** Tags einschließen möchten. Die angegebenen Spalten werden der Columns-Auflistung in der aufgelisteten Reihenfolge hinzugefügt. Die **Columns** -Auflistung speichert alle Spalten Felder im-Steuerelement und ermöglicht es Ihnen, die Spalten Felder im GridView-Steuerelement Programm gesteuert zu verwalten.

Explizit deklarierte Spalten Felder können in Kombination mit automatisch generierten Spalten Feldern angezeigt werden. Wenn beide verwendet werden, werden explizit deklarierte Spalten Felder zuerst gerendert, gefolgt von den automatisch generierten Spalten Feldern.

## <a name="binding-to-data"></a>Binden an Daten

Das GridView-Steuerelement kann an ein Datenquellen-Steuerelement (z. b. **SqlDataSource**, **ObjectDataSource**usw.) sowie an jede beliebige Datenquelle, die die System. Collections. IEnumerable-Schnittstelle implementiert (z. b. System. Data. DataView, System. Collections. ArrayList oder System. Collections Verwenden Sie eine der folgenden Methoden, um das GridView-Steuerelement an den entsprechenden Daten Quellentyp zu binden:

- Legen Sie zum Binden an ein Datenquellen-Steuerelement die DataSourceID-Eigenschaft des GridView-Steuer Elements auf den ID-Wert des Datenquellen-Steuer Elements fest. Das GridView-Steuerelement wird automatisch an das angegebene Datenquellen-Steuerelement gebunden und kann die Funktionen des Datenquellen-Steuer Elements nutzen, um Sortier-, Aktualisierungs-, Lösch-und Pagingfunktionen auszuführen. Dies ist die bevorzugte Methode, um Daten zu binden.
- Um eine Bindung an eine Datenquelle herzustellen, die die System. Collections. IEnumerable-Schnittstelle implementiert, legen Sie die DataSource-Eigenschaft des GridView-Steuer Elements Programm gesteuert auf die Datenquelle fest, und rufen Sie dann die DataBind-Methode auf. Wenn diese Methode verwendet wird, bietet das GridView-Steuerelement keine integrierten Funktionen zum Sortieren, aktualisieren, löschen und Paging. Diese Funktionalität müssen Sie selbst bereitstellen.

## <a name="data-operations"></a>Datenvorgänge

Das GridView-Steuerelement stellt viele integrierte Funktionen bereit, die es dem Benutzer ermöglichen, Elemente im Steuerelement zu sortieren, zu aktualisieren, zu löschen, auszuwählen und zu durchlaufen. Wenn das GridView-Steuerelement an ein Datenquellen Steuerelement gebunden ist, kann das GridView-Steuerelement die Funktionen des Datenquellen-Steuer Elements nutzen und automatische Sortier-, Aktualisierungs-und Löschfunktionen bereitstellen.

> [!NOTE]
> Das GridView-Steuerelement kann das Sortieren, aktualisieren und Löschen mit anderen Typen von Datenquellen unterstützen. Sie müssen jedoch einen geeigneten Ereignishandler mit der Implementierung für diese Vorgänge bereitstellen.

Das Sortieren ermöglicht dem Benutzer das Sortieren der Elemente im GridView-Steuerelement in Bezug auf eine bestimmte Spalte durch Klicken auf den Spaltenheader. Um die Sortierung zu aktivieren, legen Sie die allowsortier-Eigenschaft auf **true**fest.

Die automatischen Aktualisierungs-, Lösch-und Auswahl Funktionen werden aktiviert, wenn auf eine Schaltfläche mit dem Befehlsnamen "Bearbeiten", "Löschen" und "auswählen" in **einem Button-Feld oder einem** **TemplateField** -Spalten Feld geklickt wird. Das GridView-Steuerelement kann ein **CommandField** -Spalten Feld automatisch mit der Schaltfläche Bearbeiten, löschen oder auswählen hinzufügen, wenn die AutoGenerateEditButton-, AutoGenerateDeleteButton-oder AutoGenerateSelectButton-Eigenschaft auf **true**festgelegt ist.

> [!NOTE]
> Das Einfügen von Datensätzen in die Datenquelle wird nicht direkt vom GridView-Steuerelement unterstützt. Es ist jedoch möglich, Datensätze mit dem GridView-Steuerelement in Verbindung mit dem DetailsView-Steuerelement oder dem FormView-Steuerelement einzufügen.

Anstatt alle Datensätze in der Datenquelle gleichzeitig anzuzeigen, kann das GridView-Steuerelement die Datensätze automatisch in Seiten unterbrechen. Legen Sie zum Aktivieren von Paging die AllowPaging-Eigenschaft auf **true**fest.

## <a name="customizing-the-user-interface"></a>Anpassen der Benutzeroberfläche

Sie können die Darstellung des GridView-Steuer Elements anpassen, indem Sie die Stileigenschaften für die verschiedenen Teile des Steuer Elements festlegen. In der folgenden Tabelle werden die verschiedenen Stileigenschaften aufgelistet.

| **Style-Eigenschaft** | **Beschreibung** |
| --- | --- |
| Alternativen Stil | Die Stileinstellungen für die wechselnden Daten Zeilen im GridView-Steuerelement. Wenn diese Eigenschaft festgelegt ist, werden die Daten Zeilen abwechselnd zwischen den RowStyle-Einstellungen und den **Alternativen** Einstellungen angezeigt. |
| EditRowStyle | Die Stileinstellungen für die Zeile, die im GridView-Steuerelement bearbeitet wird. |
| EmptyDataRowStyle | Die Stileinstellungen für die leere Daten Zeile, die im GridView-Steuerelement angezeigt wird, wenn die Datenquelle keine Datensätze enthält. |
| FooterStyle | Die Stileinstellungen für die Fußzeile des GridView-Steuer Elements. |
| HeaderStyle | Die Stileinstellungen für die Kopfzeile des GridView-Steuer Elements. |
| PagerStyle | Die Stileinstellungen für die Pager-Zeile des GridView-Steuer Elements. |
| RowStyle | Die Stileinstellungen für die Daten Zeilen im GridView-Steuerelement. Wenn die Eigenschaft " **Alternative** Eigenschaft" ebenfalls festgelegt ist, werden die Daten Zeilen abwechselnd zwischen den **RowStyle** -Einstellungen und den **Alternativen** Einstellungen angezeigt. |
| SelectedRowStyle | Die Stileinstellungen für die ausgewählte Zeile im GridView-Steuerelement. |

Sie können auch unterschiedliche Teile des Steuer Elements anzeigen oder ausblenden. In der folgenden Tabelle werden die Eigenschaften aufgelistet, die Steuern, welche Teile angezeigt oder ausgeblendet werden.

| **Eigenschaft** | **Beschreibung** |
| --- | --- |
| ShowFooter | Zeigt den Footerbereich des GridView-Steuer Elements an oder blendet ihn aus. |
| ShowHeader | Blendet den Header Abschnitt des GridView-Steuer Elements ein oder aus. |

### <a name="events"></a>Events

Das GridView-Steuerelement stellt mehrere Ereignisse bereit, mit denen Sie programmieren können. Dies ermöglicht es Ihnen, eine benutzerdefinierte Routine immer dann auszuführen, wenn ein Ereignis auftritt. In der folgenden Tabelle sind die vom GridView-Steuerelement unterstützten Ereignisse aufgeführt.

| **Event** | **Beschreibung** |
| --- | --- |
| PageIndexChanged | Tritt auf, wenn auf eine der Pager-Schaltflächen geklickt wird, aber nachdem das GridView-Steuerelement den Pagingvorgang behandelt hat. Dieses Ereignis wird häufig verwendet, wenn Sie eine Aufgabe ausführen müssen, nachdem der Benutzer zu einer anderen Seite im Steuerelement navigiert ist. |
| PageIndexChanging | Tritt auf, wenn auf eine der Pager-Schaltflächen geklickt wird, jedoch bevor das GridView-Steuerelement den Pagingvorgang behandelt. Dieses Ereignis wird häufig verwendet, um den Pagingvorgang abzubrechen. |
| RowCancelingEdit | Tritt auf, wenn auf die Schaltfläche Abbrechen einer Zeile geklickt wird, jedoch bevor das GridView-Steuerelement den Bearbeitungsmodus verlässt. Dieses Ereignis wird häufig verwendet, um den Abbruch Vorgang zu verhindern. |
| RowCommand | Tritt ein, wenn im GridView-Steuerelement auf eine Schaltfläche geklickt wird. Dieses Ereignis wird häufig verwendet, um eine Aufgabe auszuführen, wenn im Steuerelement auf eine Schaltfläche geklickt wird. |
| RowCreated | Tritt auf, wenn im GridView-Steuerelement eine neue Zeile erstellt wird. Dieses Ereignis wird häufig verwendet, um den Inhalt einer Zeile zu ändern, wenn die Zeile erstellt wird. |
| RowDataBound | Tritt auf, wenn eine Daten Zeile an Daten im GridView-Steuerelement gebunden ist. Dieses Ereignis wird häufig verwendet, um den Inhalt einer Zeile zu ändern, wenn die Zeile an Daten gebunden ist. |
| RowDeleted | Tritt auf, wenn auf die Schaltfläche Löschen einer Zeile geklickt wird, aber nachdem das GridView-Steuerelement den Datensatz aus der Datenquelle gelöscht hat. Dieses Ereignis wird häufig verwendet, um die Ergebnisse des Löschvorgangs zu überprüfen. |
| Rowlösch Vorgang | Tritt auf, wenn auf die Schaltfläche Löschen einer Zeile geklickt wird, aber bevor das GridView-Steuerelement den Datensatz aus der Datenquelle löscht. Dieses Ereignis wird häufig verwendet, um den Löschvorgang abzubrechen. |
| RowEditing | Tritt auf, wenn auf die Bearbeitungs Schaltfläche einer Zeile geklickt wird, jedoch bevor das GridView-Steuerelement im Bearbeitungsmodus wechselt. Dieses Ereignis wird häufig verwendet, um den Bearbeitungsvorgang abzubrechen. |
| RowUpdated | Tritt auf, wenn auf die Schaltfläche Aktualisieren einer Zeile geklickt wird, aber nachdem das GridView-Steuerelement die Zeile aktualisiert hat. Dieses Ereignis wird häufig verwendet, um die Ergebnisse des Aktualisierungs Vorgangs zu überprüfen. |
| RowUpdating | Tritt auf, wenn auf die Schaltfläche Aktualisieren einer Zeile geklickt wird, aber bevor das GridView-Steuerelement die Zeile aktualisiert. Dieses Ereignis wird häufig verwendet, um den Aktualisierungs Vorgang abzubrechen. |
| SelectedIndexChanged | Tritt auf, wenn auf die Schaltfläche Auswählen einer Zeile geklickt wird, aber nachdem das GridView-Steuerelement den Select-Vorgang behandelt hat. Dieses Ereignis wird häufig verwendet, um eine Aufgabe auszuführen, nachdem eine Zeile im-Steuerelement ausgewählt wurde. |
| SelectedIndexChanging | Tritt auf, wenn auf die Schaltfläche Auswählen einer Zeile geklickt wird, jedoch bevor das GridView-Steuerelement den Select-Vorgang behandelt. Dieses Ereignis wird häufig verwendet, um den Auswahl Vorgang abzubrechen. |
| Sortiert | Tritt auf, wenn auf den Link zum Sortieren einer Spalte geklickt wird, aber nachdem das GridView-Steuerelement den Sortiervorgang behandelt hat. Dieses Ereignis wird häufig verwendet, um eine Aufgabe auszuführen, nachdem der Benutzer auf einen Link geklickt hat, um eine Spalte zu sortieren. |
| Sortierung | Tritt auf, wenn auf den Link zum Sortieren einer Spalte geklickt wird, jedoch bevor das GridView-Steuerelement den Sortiervorgang behandelt. Dieses Ereignis wird häufig verwendet, um den Sortiervorgang abzubrechen oder um eine benutzerdefinierte Sortier Routine auszuführen. |

## <a name="formview"></a>FormView

Das FormView-Steuerelement wird zum Anzeigen eines einzelnen Datensatzes aus einer Datenquelle verwendet. Dies ähnelt dem DetailsView-Steuerelement, mit dem Unterschied, dass es benutzerdefinierte Vorlagen anstelle von Zeilen Feldern anzeigt. Das Erstellen eigener Vorlagen bietet Ihnen mehr Flexibilität bei der Steuerung der Anzeige der Daten. Das FormView-Steuerelement unterstützt die folgenden Funktionen:

- Binden an Datenquellen-Steuerelemente, z. b. SqlDataSource und ObjectDataSource.
- Integrierte Einfügefunktionen.
- Integrierte Aktualisierungs-und Löschfunktionen.
- Integrierte Paging-Funktionen.
- Programm gesteuerter Zugriff auf das FormView-Objektmodell, um Eigenschaften dynamisch festzulegen, Ereignisse zu behandeln usw.
- Anpassbare Darstellung durch benutzerdefinierte Vorlagen, Designs und Stile.

## <a name="templates"></a>Vorlagen

Damit das FormView-Steuerelement Inhalt anzeigt, müssen Sie für die verschiedenen Teile des Steuer Elements Vorlagen erstellen. Die meisten Vorlagen sind optional. Sie müssen jedoch eine Vorlage für den Modus erstellen, in dem das Steuerelement konfiguriert ist. Beispielsweise muss ein FormView-Steuerelement, das das Einfügen von Datensätzen unterstützt, eine Einfügungs Vorlage definiert haben. In der folgenden Tabelle werden die verschiedenen Vorlagen aufgelistet, die Sie erstellen können.

| **Vorlagentyp** | **Beschreibung** |
| --- | --- |
| EditItemTemplate | Definiert den Inhalt für die Daten Zeile, wenn sich das FormView-Steuerelement im Bearbeitungsmodus befindet. Diese Vorlage enthält normalerweise Eingabe Steuerelemente und Befehls Schaltflächen, mit denen der Benutzer einen vorhandenen Datensatz bearbeiten kann. |
| EmptyDataTemplate | Definiert den Inhalt für die leere Daten Zeile, die angezeigt wird, wenn das FormView-Steuerelement an eine Datenquelle gebunden ist, die keine Datensätze enthält. Diese Vorlage enthält normalerweise Inhalt, um den Benutzer darüber zu benachrichtigen, dass die Datenquelle keine Datensätze enthält. |
| FooterTemplate | Definiert den Inhalt für die Footerzeile. Diese Vorlage enthält normalerweise alle zusätzlichen Inhalte, die Sie in der Footerzeile anzeigen möchten. Als Alternative können Sie einfach Text angeben, der in der Fußzeile angezeigt werden soll, indem Sie die FooterText-Eigenschaft festlegen. |
| Header Template | Definiert den Inhalt für die Kopfzeile. Diese Vorlage enthält normalerweise alle zusätzlichen Inhalte, die Sie in der Kopfzeile anzeigen möchten. Als Alternative können Sie einfach Text angeben, der in der Kopfzeile angezeigt werden soll, indem Sie die HeaderText-Eigenschaft festlegen. |
| ItemTemplate | Definiert den Inhalt für die Daten Zeile, wenn sich das FormView-Steuerelement im schreibgeschützten Modus befindet. Diese Vorlage enthält normalerweise Inhalt, um die Werte eines vorhandenen Datensatzes anzuzeigen. |
| InsertItemTemplate | Definiert den Inhalt für die Daten Zeile, wenn sich das FormView-Steuerelement im Einfügemodus befindet. Diese Vorlage enthält normalerweise Eingabe Steuerelemente und Befehls Schaltflächen, mit denen der Benutzer einen neuen Datensatz hinzufügen kann. |
| PagerTemplate | Definiert den Inhalt für die Pager-Zeile, die angezeigt wird, wenn das Pagingfeature aktiviert ist (wenn die AllowPaging-Eigenschaft auf **true**festgelegt ist). Diese Vorlage enthält normalerweise Steuerelemente, mit denen Benutzer zu einem anderen Datensatz navigieren können. |

Eingabe Steuerelemente in der Vorlage Element Vorlage bearbeiten und Element einfügen können mithilfe eines bidirektionalen Bindungs Ausdrucks an die Felder einer Datenquelle gebunden werden. Dies ermöglicht dem FormView-Steuerelement das automatische Extrahieren der Werte des Eingabe Steuer Elements für einen Aktualisierungs-oder Einfügevorgang. Bidirektionale Bindungs Ausdrücke ermöglichen auch Eingabe Steuerelementen in einer Edit-Element Vorlage, die ursprünglichen Feldwerte automatisch anzuzeigen.

### <a name="binding-to-data"></a>Binden an Daten

Das FormView-Steuerelement kann an ein Datenquellen-Steuerelement gebunden werden (z. b. **SqlDataSource**). AccessDataSource, **ObjectDataSource** usw.) oder auf eine beliebige Datenquelle, die die System. Collections. IEnumerable-Schnittstelle implementiert (z. b. System. Data. DataView, System. Collections. ArrayList und System. Collections. Hashtable). Verwenden Sie eine der folgenden Methoden, um das FormView-Steuerelement an den entsprechenden Daten Quellentyp zu binden:

- Legen Sie zum Binden an ein Datenquellen-Steuerelement die DataSourceID-Eigenschaft des FormView-Steuer Elements auf den ID-Wert des Datenquellen-Steuer Elements fest. Das FormView-Steuerelement wird automatisch an das angegebene Datenquellen-Steuerelement gebunden und kann die Funktionen des Datenquellen-Steuer Elements nutzen, um das Einfügen, aktualisieren, löschen und Paging auszuführen. Dies ist die bevorzugte Methode, um Daten zu binden.
- Um eine Bindung an eine Datenquelle herzustellen, die die **System. Collections. IEnumerable** -Schnittstelle implementiert, legen Sie die DataSource-Eigenschaft des FormView-Steuer Elements Programm gesteuert auf die Datenquelle fest, und rufen Sie dann die DataBind-Methode auf. Wenn diese Methode verwendet wird, bietet das FormView-Steuerelement keine integrierte Funktion zum Einfügen, aktualisieren, löschen und Paging. Sie müssen diese Funktion mithilfe des entsprechenden Ereignisses bereitstellen.

## <a name="data-operations"></a>Datenvorgänge

Das FormView-Steuerelement stellt viele integrierte Funktionen bereit, die es dem Benutzer ermöglichen, Elemente im Steuerelement zu aktualisieren, zu löschen, einzufügen und zu durchlaufen. Wenn das FormView-Steuerelement an ein Datenquellen Steuerelement gebunden ist, kann das FormView-Steuerelement die Funktionen des Datenquellen-Steuer Elements nutzen und automatische Aktualisierungs-, Lösch-, Einfüge-und Pagingfunktionen bereitstellen. Das FormView-Steuerelement kann Aktualisierungs-, Lösch-, Einfüge-und Pagingvorgänge mit anderen Typen von Datenquellen unterstützen. Sie müssen jedoch einen geeigneten Ereignishandler mit der-Implementierung für diese Vorgänge bereitstellen.

Da das FormView-Steuerelement Vorlagen verwendet, bietet es keine Möglichkeit, automatisch Befehls Schaltflächen zum Ausführen von Aktualisierungs-, Lösch-oder Einfügevorgängen zu generieren. Sie müssen diese Befehls Schaltflächen manuell in die entsprechende Vorlage einschließen. Das FormView-Steuerelement erkennt bestimmte Schaltflächen, deren **CommandName** -Eigenschaften auf bestimmte Werte festgelegt sind. In der folgenden Tabelle werden die Befehls Schaltflächen aufgelistet, die das FormView-Steuerelement erkennt

| **Button** (Schaltfläche) | **CommandName-Wert** | **Beschreibung** |
| --- | --- | --- |
| Abbrechen | Jederzeit | Wird beim Aktualisieren oder Einfügen von Vorgängen verwendet, um den Vorgang abzubrechen und die vom Benutzer eingegebenen Werte zu verwerfen. Das FormView-Steuerelement kehrt dann in den Modus zurück, der von der DefaultMode-Eigenschaft angegeben wird. |
| Löschen | "Löschen" | Wird beim Löschen von Vorgängen verwendet, um den angezeigten Datensatz aus der Datenquelle zu löschen. Löst die itemlösch-und ItemDeleted-Ereignisse aus. |
| Bearbeiten | Bearbeiten | Wird in Aktualisierungs Vorgängen verwendet, um das FormView-Steuerelement im Bearbeitungsmodus einzufügen. Der Inhalt, der in der **EditItemTemplate** -Eigenschaft angegeben ist, wird für die Daten Zeile angezeigt. |
| Einfügen | Setze | Wird beim Einfügen von Vorgängen verwendet, um mithilfe der vom Benutzer bereitgestellten Werte einen neuen Datensatz in die Datenquelle einzufügen. Löst die Ereignisse ItemInserting und ItemInserted aus. |
| Neu | Neubau | Wird beim Einfügen von Vorgängen verwendet, um das FormView-Steuerelement im Einfügemodus Der Inhalt, der in der **InsertItemTemplate** -Eigenschaft angegeben ist, wird für die Daten Zeile angezeigt. |
| Seite | S | Wird in Pagingvorgängen verwendet, um eine Schaltfläche in der Pager-Zeile darzustellen, die Paging ausführt. Legen Sie zum Angeben des Pagingvorgangs die **CommandArgument** -Eigenschaft der Schaltfläche auf "Next", "Prev", "First", "Last" oder den Index der Seite fest, zu der navigiert werden soll. Löst das pageidexchanging-Ereignis und das pageidexchanged-Ereignis aus. |
| Aktualisieren | Alisierungs | Wird in Aktualisierungs Vorgängen verwendet, um zu versuchen, den angezeigten Datensatz in der Datenquelle mit den vom Benutzer bereitgestellten Werten zu aktualisieren. Löst das ItemUpdating-Ereignis und das ItemUpdating-Ereignis aus. |

Anders als bei der Schaltfläche "Löschen" (wodurch der angezeigte Datensatz sofort gelöscht wird), wechselt das FormView-Steuerelement beim Klicken auf die Schaltfläche "Bearbeiten" oder "neu". Im Bearbeitungsmodus wird der Inhalt, der in der **EditItemTemplate** -Eigenschaft enthalten ist, für das aktuelle Datenelement angezeigt. In der Regel wird die Vorlage Element bearbeiten so definiert, dass die Schaltfläche Bearbeiten durch ein Update und eine Schaltfläche Abbrechen ersetzt wird. Eingabe Steuerelemente, die für den Datentyp des Felds geeignet sind (z. b. ein Textfeld oder ein CheckBox-Steuerelement), werden in der Regel auch mit dem Wert eines Felds angezeigt, das vom Benutzer geändert werden muss. Wenn Sie auf die Schaltfläche Aktualisieren klicken, wird der Datensatz in der Datenquelle aktualisiert. Wenn Sie auf die Schaltfläche Abbrechen klicken, werden alle Änderungen

Ebenso wird der Inhalt, der in der **InsertItemTemplate** -Eigenschaft enthalten ist, für das Datenelement angezeigt, wenn sich das Steuerelement im Einfügemodus befindet. Die Element Vorlage "Einfügen" wird in der Regel so definiert, dass die Schaltfläche "neu" durch eine Einfügung und eine Schaltfläche "Abbrechen" ersetzt wird. für den Benutzer werden leere Eingabe Steuerelemente angezeigt, um die Werte für den neuen Datensatz einzugeben Durch Klicken auf die Schaltfläche Einfügen wird der Datensatz in die Datenquelle eingefügt, während durch Klicken auf die Schaltfläche Abbrechen alle Änderungen abgebrochen werden.

Das FormView-Steuerelement bietet ein Pagingfeature, das es dem Benutzer ermöglicht, zu anderen Datensätzen in der Datenquelle zu navigieren. Wenn diese Option aktiviert ist, wird eine Pager-Zeile im FormView-Steuerelement angezeigt, das die Steuerelemente der Seitennavigation enthält. Legen Sie zum Aktivieren von Paging die **AllowPaging** -Eigenschaft auf **true**fest. Sie können die Zeile Pager anpassen, indem Sie die Eigenschaften der Objekte festlegen, die in der PagerStyle-Eigenschaft und der PagerSettings-Eigenschaft enthalten sind. Anstatt die integrierte Pager-Zeilen Schnittstelle zu verwenden, können Sie mit der **PagerTemplate** -Eigenschaft eine eigene Benutzeroberfläche erstellen.

## <a name="customizing-the-user-interface"></a>Anpassen der Benutzeroberfläche

Sie können die Darstellung des FormView-Steuer Elements anpassen, indem Sie die Stileigenschaften für die verschiedenen Teile des Steuer Elements festlegen. In der folgenden Tabelle werden die verschiedenen Stileigenschaften aufgelistet.

| **Style-Eigenschaft** | **Beschreibung** |
| --- | --- |
| EditRowStyle | Die Stileinstellungen für die Daten Zeile, wenn sich das FormView-Steuerelement im Bearbeitungsmodus befindet. |
| EmptyDataRowStyle | Die Stileinstellungen für die leere Daten Zeile, die im FormView-Steuerelement angezeigt wird, wenn die Datenquelle keine Datensätze enthält. |
| FooterStyle | Die Stileinstellungen für die Fußzeile des FormView-Steuer Elements. |
| HeaderStyle | Die Stileinstellungen für die Kopfzeile des FormView-Steuer Elements. |
| InsertRowStyle | Die Stileinstellungen für die Daten Zeile, wenn sich das FormView-Steuerelement im Einfügemodus befindet. |
| PagerStyle | Die Stileinstellungen für die Pager-Zeile, die im FormView-Steuerelement angezeigt wird, wenn das Pagingfeature aktiviert ist. |
| RowStyle | Die Stileinstellungen für die Daten Zeile, wenn sich das FormView-Steuerelement im schreibgeschützten Modus befindet. |

## <a name="events"></a>Events

Das FormView-Steuerelement stellt mehrere Ereignisse bereit, mit denen Sie programmieren können. Dies ermöglicht es Ihnen, eine benutzerdefinierte Routine immer dann auszuführen, wenn ein Ereignis auftritt. In der folgenden Tabelle sind die vom FormView-Steuerelement unterstützten Ereignisse aufgeführt.

| **Event** | **Beschreibung** |
| --- | --- |
| ItemCommand | Tritt auf, wenn in einem FormView-Steuerelement auf eine Schaltfläche geklickt wird. Dieses Ereignis wird häufig verwendet, um eine Aufgabe auszuführen, wenn im Steuerelement auf eine Schaltfläche geklickt wird. |
| ItemCreated | Tritt auf, nachdem alle FormViewRow-Objekte im FormView-Steuerelement erstellt wurden. Dieses Ereignis wird häufig verwendet, um die Werte eines Datensatzes zu ändern, bevor er angezeigt wird. |
| ItemDeleted | Tritt auf, wenn auf die Schaltfläche Löschen (eine Schaltfläche mit der Eigenschaft **CommandName** auf "Delete" festgelegt ist) geklickt wird, aber nachdem das FormView-Steuerelement den Datensatz aus der Datenquelle gelöscht hat. Dieses Ereignis wird häufig verwendet, um die Ergebnisse des Löschvorgangs zu überprüfen. |
| Itemlösch Vorgang | Tritt auf, wenn auf eine Lösch Schaltfläche geklickt wird, aber bevor das FormView-Steuerelement den Datensatz aus der Datenquelle löscht. Dieses Ereignis wird häufig verwendet, um den Löschvorgang abzubrechen. |
| ItemInserted | Tritt auf, wenn auf eine einfügeschaltfläche (eine Schaltfläche, bei der die **CommandName** -Eigenschaft auf "Insert" festgelegt ist) geklickt wird. Dieses Ereignis wird häufig verwendet, um die Ergebnisse des Einfügevorgangs zu überprüfen. |
| ItemInserting | Tritt auf, wenn auf eine Schaltfläche Einfügen geklickt wird, aber bevor das FormView-Steuerelement den Datensatz einfügt. Dieses Ereignis wird häufig verwendet, um den Einfügevorgang abzubrechen. |
| Itemupdatiert | Tritt auf, wenn auf eine Aktualisierungs Schaltfläche (eine Schaltfläche, deren **CommandName** -Eigenschaft auf "Update" festgelegt ist) geklickt wird, nachdem das FormView-Steuerelement die Zeile aktualisiert hat Dieses Ereignis wird häufig verwendet, um die Ergebnisse des Aktualisierungs Vorgangs zu überprüfen. |
| ItemUpdating | Tritt auf, wenn auf eine Update Schaltfläche geklickt wird, aber bevor das FormView-Steuerelement den Datensatz aktualisiert. Dieses Ereignis wird häufig verwendet, um den Aktualisierungs Vorgang abzubrechen. |
| ModeChanged | Tritt auf, nachdem das Formular View-Steuerelement den Modus geändert hat (zum Bearbeiten, einfügen oder schreibgeschützten Modus). Dieses Ereignis wird häufig verwendet, um eine Aufgabe auszuführen, wenn die Modi des FormView-Steuer Elements geändert werden. |
| ModeChanging | Tritt ein, bevor das FormView-Steuerelement den Modus ändert (zum Bearbeiten, einfügen oder schreibgeschützten Modus). Dieses Ereignis wird häufig verwendet, um eine Modusänderung abzubrechen. |
| PageIndexChanged | Tritt auf, wenn auf eine der Pager-Schaltflächen geklickt wird, aber nachdem das FormView-Steuerelement den Pagingvorgang behandelt hat. Dieses Ereignis wird häufig verwendet, wenn Sie eine Aufgabe ausführen müssen, nachdem der Benutzer zu einem anderen Datensatz im Steuerelement navigiert ist. |
| PageIndexChanging | Tritt auf, wenn auf eine der Pager-Schaltflächen geklickt wird, jedoch bevor das FormView-Steuerelement den Pagingvorgang behandelt. Dieses Ereignis wird häufig verwendet, um den Pagingvorgang abzubrechen. |

## <a name="detailsview"></a>DetailsView

Das DetailsView-Steuerelement wird zum Anzeigen eines einzelnen Datensatzes aus einer Datenquelle in einer Tabelle verwendet, in der jedes Feld des Datensatzes in einer Zeile der Tabelle angezeigt wird. Sie kann in Kombination mit einem GridView-Steuerelement für Master/Detail-Szenarien verwendet werden. Das DetailsView-Steuerelement unterstützt die folgenden Features:

- Binden an Datenquellen Steuerelemente, z. b. SqlDataSource.
- Integrierte Einfügefunktionen.
- Integrierte Aktualisierungs-und Löschfunktionen.
- Integrierte Paging-Funktionen.
- Programm gesteuerter Zugriff auf das DetailsView-Objektmodell, um Eigenschaften dynamisch festzulegen, Ereignisse zu behandeln usw.
- Anpassbare Darstellung durch Designs und Stile.

## <a name="row-fields"></a>Zeilen Felder

Jede Daten Zeile im DetailsView-Steuerelement wird durch Deklarieren eines Feld Steuer Elements erstellt. Verschiedene Zeilen Feldtypen bestimmen das Verhalten der Zeilen im Steuerelement. Feld Steuerelemente werden aus DataControlField abgeleitet. In der folgenden Tabelle werden die verschiedenen Zeilen Feldtypen aufgelistet, die verwendet werden können.

| **Spalten Feldtyp** | **Beschreibung** |
| --- | --- |
| BoundField | Zeigt den Wert eines Felds in einer Datenquelle als Text an. |
| ButtonField | Zeigt eine Befehls Schaltfläche im DetailsView-Steuerelement an. Dadurch können Sie eine Zeile mit einem benutzerdefinierten Schaltflächen-Steuerelement anzeigen, z. b. eine Schaltfläche zum Hinzufügen oder entfernen. |
| CheckBoxField | Zeigt ein Kontrollkästchen im DetailsView-Steuerelement an. Dieser Zeilen Feldtyp wird häufig verwendet, um Felder mit einem booleschen Wert anzuzeigen. |
| CommandField | Zeigt integrierte Befehls Schaltflächen zum Ausführen von Bearbeitungs-, Einfüge-oder Lösch Vorgängen im DetailsView-Steuerelement an. |
| HyperLinkField | Zeigt den Wert eines Felds in einer Datenquelle als Hyperlink an. Dieser Zeilen Feldtyp ermöglicht es Ihnen, ein zweites Feld an die URL des Links zu binden. |
| ImageField | Zeigt ein Bild im DetailsView-Steuerelement an. |
| TemplateField | Zeigt den benutzerdefinierten Inhalt einer Zeile im DetailsView-Steuerelement entsprechend einer angegebenen Vorlage an. Mit diesem Zeilen Feldtyp können Sie ein benutzerdefiniertes Zeilen Feld erstellen. |

Standardmäßig ist die AutoGenerateRows-Eigenschaft auf **true**festgelegt, wodurch automatisch ein gebundenes Zeilen Feld Objekt für jedes Feld eines bindbaren Typs in der Datenquelle generiert wird. Gültige bindbare Typen sind "String", "DateTime", "Decimal", "GUID" und der Satz primitiver Typen. Jedes Feld wird dann als Text in der Reihenfolge angezeigt, in der die einzelnen Felder in der Datenquelle angezeigt werden.

Das automatische Erstellen der Zeilen bietet eine schnelle und einfache Möglichkeit, jedes Feld im Datensatz anzuzeigen. Um jedoch die erweiterten Funktionen des DetailsView-Steuer Elements verwenden zu können, müssen Sie die Zeilen Felder, die in das DetailsView-Steuerelement eingeschlossen werden sollen, explizit deklarieren. Legen Sie zum Deklarieren der Zeilen Felder zunächst die **AutoGenerateRows** -Eigenschaft auf **false**fest. Fügen Sie als nächstes öffnende und schließende **&lt;Felder&gt;** Tags zwischen den öffnenden und schließenden Tags des DetailsView-Steuer Elements hinzu. Listen Sie schließlich die Zeilen Felder auf, die Sie zwischen den öffnenden und schließenden **&lt;Feldern&gt;** Tags einschließen möchten. Die angegebenen Zeilen Felder werden der Fields-Auflistung in der aufgelisteten Reihenfolge hinzugefügt. Die **Fields** -Auflistung ermöglicht Ihnen die programmgesteuerte Verwaltung der Zeilen Felder im DetailsView-Steuerelement.

> [!NOTE]
> Automatisch generierte Zeilen Felder werden der Fields-Auflistung nicht hinzugefügt.

## <a name="binding-to-data"></a>Binden an Daten

Das DetailsView-Steuerelement kann an ein Datenquellen Steuerelement wie **SqlDataSource** oder AccessDataSource oder an eine beliebige Datenquelle gebunden werden, die die System. Collections. IEnumerable-Schnittstelle implementiert, z. b. System. Data. DataView, System. Collections. ArrayList und System. Collections. Hashtable.

Verwenden Sie eine der folgenden Methoden, um das DetailsView-Steuerelement an den entsprechenden Daten Quellentyp zu binden:

- Legen Sie zum Binden an ein Datenquellen-Steuerelement die DataSourceID-Eigenschaft des DetailsView-Steuer Elements auf den ID-Wert des Datenquellen-Steuer Elements fest. Das DetailsView-Steuerelement bindet automatisch an das angegebene Datenquellen-Steuerelement. Dies ist die bevorzugte Methode, um Daten zu binden.
- Um eine Bindung an eine Datenquelle herzustellen, die die **System. Collections. IEnumerable** -Schnittstelle implementiert, legen Sie die DataSource-Eigenschaft des DetailsView-Steuer Elements Programm gesteuert auf die Datenquelle fest, und rufen Sie dann die DataBind-Methode auf.

## <a name="security"></a>Sicherheit

Dieses Steuerelement kann zum Anzeigen von Benutzereingaben verwendet werden, die möglicherweise böswillige Client Skripts enthalten. Überprüfen Sie alle Informationen, die von einem Client für ausführbare Skripts, SQL-Anweisungen oder anderen Code gesendet werden, bevor Sie ihn in der Anwendung anzeigen. ASP.net bietet eine Überprüfungs Funktion für die Eingabe Anforderung zum Blockieren von Skripts und HTML in Benutzereingaben.

## <a name="data-operations"></a>Datenvorgänge

Das DetailsView-Steuerelement stellt integrierte Funktionen bereit, die es dem Benutzer ermöglichen, Elemente im Steuerelement zu aktualisieren, zu löschen, einzufügen und zu durchlaufen. Wenn das DetailsView-Steuerelement an ein Datenquellen Steuerelement gebunden ist, kann das DetailsView-Steuerelement die Funktionen des Datenquellen-Steuer Elements nutzen und automatische Aktualisierungs-, Lösch-, Einfüge-und Pagingfunktionen bereitstellen.

Das DetailsView-Steuerelement kann Aktualisierungs-, Lösch-, Einfüge-und Pagingvorgänge mit anderen Typen von Datenquellen unterstützen. Allerdings müssen Sie die Implementierung für diese Vorgänge in einem geeigneten Ereignishandler bereitstellen.

Das DetailsView-Steuerelement kann automatisch ein **CommandField** -Zeilen Feld mit der Schaltfläche Bearbeiten, löschen oder neu hinzufügen, indem die Eigenschaften AutoGenerateEditButton, AutoGenerateDeleteButton oder AutoGenerateInsertButton auf **true**festgelegt werden. Anders als bei der Schaltfläche "Löschen" (wodurch der ausgewählte Datensatz sofort gelöscht wird), wechselt das DetailsView-Steuerelement in den Bearbeitungs-oder Einfügemodus. Im Bearbeitungsmodus wird die Schaltfläche Bearbeiten durch ein Update und eine Schaltfläche Abbrechen ersetzt. Eingabe Steuerelemente, die für den Datentyp des Felds geeignet sind (z. b. ein Textfeld oder ein CheckBox-Steuerelement), werden mit dem Wert eines Felds angezeigt, das vom Benutzer geändert werden soll. Wenn Sie auf die Schaltfläche Aktualisieren klicken, wird der Datensatz in der Datenquelle aktualisiert. Wenn Sie auf die Schaltfläche Abbrechen klicken, werden alle Änderungen Ebenso wird die Schaltfläche neu im Einfügemodus durch eine Einfügung und eine Schaltfläche Abbrechen ersetzt, und es werden leere Eingabe Steuerelemente angezeigt, damit der Benutzer die Werte für den neuen Datensatz eingeben kann.

Das DetailsView-Steuerelement bietet ein Pagingfeature, das es dem Benutzer ermöglicht, zu anderen Datensätzen in der Datenquelle zu navigieren. Wenn diese Option aktiviert ist, werden Steuerelemente zur Seitennavigation in einer Pager-Zeile angezeigt. Legen Sie zum Aktivieren von Paging die AllowPaging-Eigenschaft auf **true**fest. Die Pager-Zeile kann mithilfe der Eigenschaften PagerStyle und PagerSettings angepasst werden.

## <a name="customizing-the-user-interface"></a>Anpassen der Benutzeroberfläche

Sie können die Darstellung des DetailsView-Steuer Elements anpassen, indem Sie die Stileigenschaften für die verschiedenen Teile des Steuer Elements festlegen. In der folgenden Tabelle werden die verschiedenen Stileigenschaften aufgelistet.

| **Style-Eigenschaft** | **Beschreibung** |
| --- | --- |
| Alternativen Stil | Die Stileinstellungen für die wechselnden Daten Zeilen im DetailsView-Steuerelement. Wenn diese Eigenschaft festgelegt ist, werden die Daten Zeilen abwechselnd zwischen den RowStyle-Einstellungen und den **Alternativen** Einstellungen angezeigt. |
| CommandRowStyle | Die Stileinstellungen für die Zeile, die die integrierten Befehls Schaltflächen im DetailsView-Steuerelement enthält. |
| EditRowStyle | Die Stileinstellungen für die Daten Zeilen, wenn sich das DetailsView-Steuerelement im Bearbeitungsmodus befindet. |
| EmptyDataRowStyle | Die Stileinstellungen für die leere Daten Zeile, die im DetailsView-Steuerelement angezeigt wird, wenn die Datenquelle keine Datensätze enthält. |
| FooterStyle | Die Stileinstellungen für die Fußzeile des DetailsView-Steuer Elements. |
| HeaderStyle | Die Stileinstellungen für die Kopfzeile des DetailsView-Steuer Elements. |
| InsertRowStyle | Die Stileinstellungen für die Daten Zeilen, wenn sich das DetailsView-Steuerelement im Einfügemodus befindet. |
| PagerStyle | Die Stileinstellungen für die Pager-Zeile des DetailsView-Steuer Elements. |
| RowStyle | Die Stileinstellungen für die Daten Zeilen im DetailsView-Steuerelement. Wenn die Eigenschaft " **Alternative** Eigenschaft" ebenfalls festgelegt ist, werden die Daten Zeilen abwechselnd zwischen den **RowStyle** -Einstellungen und den **Alternativen** Einstellungen angezeigt. |
| FieldHeaderStyle | Die Stileinstellungen für die Header Spalte des DetailsView-Steuer Elements. |

## <a name="events"></a>Events

Das DetailsView-Steuerelement stellt mehrere Ereignisse bereit, mit denen Sie programmieren können. Dies ermöglicht es Ihnen, eine benutzerdefinierte Routine immer dann auszuführen, wenn ein Ereignis auftritt. In der folgenden Tabelle sind die vom DetailsView-Steuerelement unterstützten Ereignisse aufgeführt. Das DetailsView-Steuerelement erbt auch diese Ereignisse von seinen Basisklassen: DataBinding, datungebunden, verworfen, init, Load, PreRender und Rendering.

| **Event** | **Beschreibung** |
| --- | --- |
| ItemCommand | Tritt auf, wenn im DetailsView-Steuerelement auf eine Schaltfläche geklickt wird. |
| ItemCreated | Tritt auf, nachdem alle DetailsViewRow-Objekte im DetailsView-Steuerelement erstellt wurden. Dieses Ereignis wird häufig verwendet, um die Werte eines Datensatzes zu ändern, bevor er angezeigt wird. |
| ItemDeleted | Tritt auf, wenn auf eine Lösch Schaltfläche geklickt wird, aber nachdem das DetailsView-Steuerelement den Datensatz aus der Datenquelle gelöscht hat. Dieses Ereignis wird häufig verwendet, um die Ergebnisse des Löschvorgangs zu überprüfen. |
| Itemlösch Vorgang | Tritt auf, wenn auf eine Lösch Schaltfläche geklickt wird, aber bevor das DetailsView-Steuerelement den Datensatz aus der Datenquelle löscht. Dieses Ereignis wird häufig verwendet, um den Löschvorgang abzubrechen. |
| ItemInserted | Tritt auf, wenn auf eine Schaltfläche Einfügen geklickt wird, aber nachdem das DetailsView-Steuerelement den Datensatz eingefügt hat. Dieses Ereignis wird häufig verwendet, um die Ergebnisse des Einfügevorgangs zu überprüfen. |
| ItemInserting | Tritt auf, wenn auf eine Schaltfläche Einfügen geklickt wird, aber bevor das DetailsView-Steuerelement den Datensatz einfügt. Dieses Ereignis wird häufig verwendet, um den Einfügevorgang abzubrechen. |
| Itemupdatiert | Tritt auf, wenn auf eine Aktualisierungs Schaltfläche geklickt wird, nachdem das DetailsView-Steuerelement die Zeile aktualisiert hat. Dieses Ereignis wird häufig verwendet, um die Ergebnisse des Aktualisierungs Vorgangs zu überprüfen. |
| ItemUpdating | Tritt auf, wenn auf eine Aktualisierungs Schaltfläche geklickt wird, aber bevor das DetailsView-Steuerelement den Datensatz aktualisiert. Dieses Ereignis wird häufig verwendet, um den Aktualisierungs Vorgang abzubrechen. |
| ModeChanged | Tritt auf, nachdem das DetailsView-Steuerelement Modi geändert hat (bearbeiten, einfügen oder Schreib geschützter Modus). Dieses Ereignis wird häufig verwendet, um eine Aufgabe auszuführen, wenn die Modi des DetailsView-Steuer Elements geändert werden. |
| ModeChanging | Tritt ein, bevor das DetailsView-Steuerelement Modi ändert (bearbeiten, einfügen oder Schreib geschützter Modus). Dieses Ereignis wird häufig verwendet, um eine Modusänderung abzubrechen. |
| PageIndexChanged | Tritt auf, wenn auf eine der Pager-Schaltflächen geklickt wird, aber nachdem das DetailsView-Steuerelement den Pagingvorgang behandelt hat. Dieses Ereignis wird häufig verwendet, wenn Sie eine Aufgabe ausführen müssen, nachdem der Benutzer zu einem anderen Datensatz im Steuerelement navigiert ist. |
| PageIndexChanging | Tritt auf, wenn auf eine der Pager-Schaltflächen geklickt wird, jedoch bevor das DetailsView-Steuerelement den Pagingvorgang behandelt. Dieses Ereignis wird häufig verwendet, um den Pagingvorgang abzubrechen. |

## <a name="the-menu-control"></a>Das Menü Steuerelement

Das Menü Steuerelement in ASP.NET 2,0 ist als voll funktionsfähigen Navigationssystem konzipiert. Dies kann auf einfache Weise in hierarchische Datenquellen wie SiteMapDataSource erfolgen.

Eine Menü Steuerungsstruktur kann deklarativ oder dynamisch definiert werden und besteht aus einem einzelnen Stamm Knoten und einer beliebigen Anzahl von untergeordneten Knoten. Der folgende Code definiert deklarativ ein Menü für das Menü Steuerelement.

[!code-aspx[Main](data-bound-controls/samples/sample4.aspx)]

Im obigen Beispiel ist der Home. aspx-Knoten der Stamm Knoten. Alle anderen Knoten sind innerhalb des Stamm Knotens auf unterschiedlichen Ebenen geschachtelt.

Es gibt zwei Arten von Menüs, die das Menü Steuerelement Rendering haben kann. statische Menüs und dynamische Menüs. Statische Menüs bestehen aus Menü Elementen, die immer sichtbar sind. Dynamische Menüs bestehen aus Menü Elementen, die nur sichtbar sind, wenn der Benutzer mit der Maus darauf zeigt. Kunden können statische Menüs häufig mit Menüs, die deklarativ definiert sind, und dynamischen Menüs mit Menüs verwechseln, die zur Laufzeit Daten gebunden sind. Tatsächlich sind dynamische und statische Menüs nicht mit der auffüllungs Methode verknüpft. Die Begriffe *static* und *Dynamic* verweisen nur darauf, ob das Menü standardmäßig statisch angezeigt wird oder nur angezeigt wird, wenn der Benutzer eine Aktion durchführt.

Die **StaticDisplayLevels** -Eigenschaft wird verwendet, um zu konfigurieren, wie viele Ebenen des Menüs statisch und daher standardmäßig angezeigt werden. Im obigen Beispiel würde das Festlegen der **StaticDisplayLevels** -Eigenschaft auf den Wert 2 bewirken, dass im Menü der Startknoten, der Knoten "Musik" und der Knoten "Filme" statisch angezeigt werden. Alle anderen Knoten werden dynamisch angezeigt, wenn der Benutzer auf den übergeordneten Knoten zeigt.

Die **MaximumDynamicDisplayLevels** -Eigenschaft konfiguriert die maximale Anzahl dynamischer Ebenen, die das Menü anzeigen kann. Dynamische Menüs auf einer Ebene, die höher als der durch die **MaximumDynamicDisplayLevels** -Eigenschaft angegebene Wert ist, werden verworfen.

> [!NOTE]
> Es ist fast sicher, dass es Situationen gibt, in denen Menüs aufgrund der MaximumDynamicDisplayLevels-Eigenschaft nicht zum Rendering angezeigt werden. Vergewissern Sie sich in diesen Fällen, dass die-Eigenschaft ausreichend festgelegt ist, um die Anzeige der Kunden Menüs zuzulassen.

## <a name="data-binding-the-menu-control"></a>Datenbindung für das Menü Steuerelement

Das Menü Steuerelement kann an eine beliebige hierarchische Datenquelle wie SiteMapDataSource oder die XmlDataSource gebunden werden. Die SiteMapDataSource ist die am häufigsten verwendete Methode für die Datenbindung an ein Menü Steuerelement, da Sie sich aus der Datei "Web. Sitemap" speist und Ihr Schema eine bekannte API für das Menü Steuerelement bereitstellt. Die folgende Liste zeigt eine einfache Web. Sitemap-Datei.

[!code-xml[Main](data-bound-controls/samples/sample5.xml)]

Beachten Sie, dass es nur ein root siteMapNode-Element, in diesem Fall das Home-Element, gibt. Für jeden SiteMapNode können mehrere Attribute konfiguriert werden. Die am häufigsten verwendeten Attribute sind:

- **URL** Gibt die URL an, die angezeigt werden soll, wenn ein Benutzer auf das Menü Element klickt. Wenn dieses Attribut nicht vorhanden ist, sendet der Knoten einfach zurück, wenn darauf geklickt wird.
- **Titel** Gibt den Text an, der auf dem Menü Element angezeigt wird.
- **Beschreibung** Wird als Dokumentation für den Knoten verwendet. Wird auch als QuickInfo angezeigt, wenn mit dem Mauszeiger auf den Knoten gezeigt wird.
- **siteMapFile** Ermöglicht das Zulassen von Zuordnungs Zuordnungen. Dieses Attribut muss auf eine wohlgeformte ASP.net-Sitemap-Datei zeigen.
- **Rollen** Ermöglicht, dass die Darstellung eines Knotens durch ASP.net Security-Kürzung gesteuert wird.

Beachten Sie, dass zwar alle Attribute optional sind, das Verhalten des Menüs jedoch möglicherweise nicht erwartet wird, wenn Sie nicht angegeben werden. Wenn z. b. das *URL* -Attribut angegeben ist, das *Beschreibungs* Attribut jedoch nicht ist, ist der Knoten nicht sichtbar, und es gibt keine Möglichkeit, zur angegebenen URL zu navigieren.

## <a name="controlling-a-menus-operation"></a>Steuern eines Menüs Vorgangs

Es gibt mehrere Eigenschaften, die sich auf den Betrieb eines ASP.net-Menü Steuer Elements auswirken. die Eigenschaft **Ausrichtung** , die Eigenschaft **nach dem verwerfen nach** , die Eigenschaft **staticitemformatstring** und die Eigenschaft **StaticPopOutImageUrl** sind nur einige davon.

- Die **Ausrichtung** kann auf " *Horizontal* " oder " *vertikal* " festgelegt werden und steuert, ob statische Menü Elemente horizontal oder vertikal angeordnet und gestapelt werden. Diese Eigenschaft wirkt sich nicht auf dynamische Menüs aus.
- Mit der Eigenschaft **verwerfen nach** wird konfiguriert, wie lange ein dynamisches Menü sichtbar bleiben soll, nachdem die Maus von der Maus entfernt wurde. Der Wert wird in Millisekunden angegeben, und der Standardwert ist 500. Das Festlegen dieser Eigenschaft auf den Wert-1 bewirkt, dass das Menü nie automatisch verschwindet. In diesem Fall wird das Menü nur dann ausgeblendet, wenn der Benutzer außerhalb des Menüs klickt.
- Die **staticitemformatstring** -Eigenschaft vereinfacht die Beibehaltung konsistentes Sprache im Menüsystem. Wenn Sie diese Eigenschaft angeben, sollten *{0}* anstelle der Beschreibung eingegeben werden, die in der Datenquelle angezeigt wird. Um beispielsweise zu sehen, wie das Menü Element von Übung 1 auf der Seite Produkte besuchen usw. angezeigt wird, geben Sie besuchen Sie unsere {0} Seite für staticitemformatstring an. Zur Laufzeit ersetzt ASP.net jedes Vorkommen von {0} durch die richtige Beschreibung für das Menü Element.
- Die Eigenschaft **StaticPopOutImageUrl** gibt das Bild an, das verwendet wird, um anzugeben, dass ein bestimmter Menü Knoten über untergeordnete Knoten verfügt, auf die durch Bewegen des Mauszeigers zugegriffen werden kann. Dynamische Menüs verwenden weiterhin das Standardbild.

## <a name="templated-menu-controls"></a>Menü Steuerelemente mit Vorlagen

Das Menü Steuerelement ist ein Steuerelement mit Vorlagen und ermöglicht zwei verschiedene ItemTemplates. Das StaticItemTemplate-Element und das DynamicItemTemplate-Element. Mithilfe dieser Vorlagen können Sie Ihren Menüs problemlos Server Steuerelemente oder Benutzer Steuerelemente hinzufügen.

Um die Vorlagen in Visual Studio .net zu bearbeiten, klicken Sie im Menü auf die Smarttag-Schaltfläche, und wählen Sie entweder Vorlagen bearbeiten aus. Sie können dann zwischen der Bearbeitung von StaticItemTemplate oder DynamicItemTemplate wählen.

Alle Steuerelemente, die der StaticItemTemplate hinzugefügt werden, werden im statischen Menü angezeigt, wenn die Seite geladen wird. Alle der DynamicItemTemplate hinzugefügten Steuerelemente werden in allen Popup Menüs angezeigt.

## <a name="menu-events"></a>Menü Ereignisse

Das Menü Steuerelement verfügt über zwei Ereignisse, die dafür eindeutig sind. Das **menuitemgeklickt** -Ereignis und das **menuitemdatabo-** Ereignis.

Das menuitemgeklickt-Ereignis wird ausgelöst, wenn auf ein Menü Element geklickt wird. Das menuitemdatraised-Ereignis wird ausgelöst, wenn ein Menü Elementdaten gebunden ist. Die **MenuEventArgs** , die an den-Ereignishandler übermittelt werden, ermöglicht den Zugriff auf das Menü Element über die Item-Eigenschaft.

## <a name="controlling-a-menus-appearance"></a>Steuern der Darstellung eines Menüs

Sie können sich auch auf die Darstellung eines Menu-Steuer Elements auswirken, indem Sie einen oder mehrere der vielen Stile zum Formatieren von Menüs verwenden. Dazu zählen **StaticMenuStyle**, **DynamicMenuStyle**, **DynamicMenuItemStyle**, **DynamicSelectedStyle**und **DynamicHoverStyle**. Diese Eigenschaften werden mit einer HTML-Standardformat Zeichenfolge konfiguriert. Der folgende Code wirkt sich z. b. auf den Stil für dynamische Menüs aus.

[!code-aspx[Main](data-bound-controls/samples/sample6.aspx)]

> [!NOTE]
> Wenn Sie einen der Hover-Stile verwenden, müssen Sie der Seite ein &lt;Head&gt; Element hinzufügen, wobei das *runat* -Element auf *Server*festgelegt ist.

Menü Steuerelemente unterstützen auch die Verwendung von ASP.NET 2,0-Designs.

## <a name="the-treeview-control"></a>Das TreeView-Steuerelement

Das TreeView-Steuerelement zeigt Daten in einer Struktur ähnlichen Struktur an. Wie beim Menü Steuerelement kann es problemlos an eine beliebige hierarchische Datenquelle wie SiteMapDataSource gebunden werden.

Die erste Frage, die Kunden wahrscheinlich über das TreeView-Steuerelement in ASP.NET 2,0 Fragen, ist, ob es mit dem TreeView IE WebControl verknüpft ist, das für ASP.NET 1. x verfügbar war. Es ist nicht. Das ASP.NET 2,0 TreeView-Steuerelement wurde von Grund auf geschrieben und bietet deutliche Verbesserungen gegenüber dem zuvor verfügbaren IE TreeView WebControl.

Ich werde nicht ausführlich darauf eingehen, wie ein TreeView-Steuerelement an eine Site Map gebunden wird, da es auf genau dieselbe Weise wie das Menü Steuerelement ausgeführt wird. Das TreeView-Steuerelement weist jedoch einige Unterschiede in der Funktionsweise auf.

Standardmäßig wird ein TreeView-Steuerelement vollständig erweitert angezeigt. Um die Erweiterungs Ebene nach dem anfänglichen Laden zu ändern, ändern Sie die **expandtiefe** -Eigenschaft des Steuer Elements. Dies ist besonders wichtig, wenn TreeView beim Erweitern bestimmter Knoten Daten gebunden ist.

## <a name="databinding-the-treeview-control"></a>DataBinding des TreeView-Steuer Elements

Im Gegensatz zum Menü Steuerelement eignet sich die TreeView gut für die Verarbeitung großer Datenmengen. Daher ist die TreeView, zusätzlich zur Datenbindung an eine SiteMapDataSource oder eine XmlDataSource, häufig an ein DataSet oder andere relationale Daten gebunden. In Fällen, in denen das TreeView-Steuerelement an große Datenmengen gebunden ist, ist es am besten, nur an Daten zu binden, die im Steuerelement tatsächlich sichtbar sind. Sie können dann Daten an zusätzliche Daten binden, wenn TreeView-Knoten erweitert werden.

In diesen Fällen sollte die **PopulateOnDemand** -Eigenschaft der TreeView auf *true*festgelegt werden. Sie müssen dann eine Implementierung für die **TreeNodePopulate** -Methode bereitstellen.

## <a name="data-binding-without-postback"></a>Datenbindung ohne Postback

Beachten Sie, dass beim erstmaligen Erweitern eines Knotens im vorherigen Beispiel die Seite zurückgesendet und aktualisiert wird. In diesem Beispiel ist dies kein Problem, aber Sie können sich vorstellen, dass es sich in einer Produktionsumgebung mit einer großen Datenmenge befinden könnte. Ein besseres Szenario wäre eine, bei der die TreeView weiterhin dynamisch die Knoten auffüllen würde, ohne dass ein Postback an den Server durchführt.

Wenn Sie die Eigenschaften " **PopulateNodesFromClient** " und " **PopulateOnDemand** " auf "true" festlegen, füllt das ASP.NET TreeView-Steuerelement Knoten dynamisch ohne Postback. Wenn der übergeordnete Knoten erweitert ist, wird vom Client eine XMLHTTP-Anforderung erstellt, und das ontreenoentpopulate-Ereignis wird ausgelöst. Der Server antwortet mit einer XML-Daten Insel, die dann zum Binden der untergeordneten Knoten verwendet wird.

ASP.NET erstellt dynamisch den Client Code, der diese Funktion implementiert. Die &lt;Skripts&gt; Tags, die das Skript enthalten, werden generiert und zeigen auf eine axd-Datei. In der folgenden Liste werden z. b. die Skript Verknüpfungen für den Skriptcode angezeigt, der die XMLHTTP-Anforderung generiert.

[!code-html[Main](data-bound-controls/samples/sample7.html)]

Wenn Sie die oben stehende axd-Datei in Ihrem Browser durchsuchen und öffnen, wird der Code angezeigt, der die XMLHTTP-Anforderung implementiert. Diese Methode verhindert, dass Kunden die Skriptdatei ändern.

## <a name="controlling-the-operation-of-the-treeview-control"></a>Steuern des Vorgangs des TreeView-Steuer Elements

Das TreeView-Steuerelement verfügt über mehrere Eigenschaften, die sich auf die Ausführung des Steuer Elements auswirken. Die offensichtlichsten Eigenschaften sind die **ShowCheckBoxes**, **ShowExpandCollapse**und **ShowLines**.

Die **ShowCheckBoxes** -Eigenschaft wirkt sich darauf aus, ob Knoten beim Rendern ein Kontrollkästchen anzeigen. Gültige Werte für diese Eigenschaft sind **None**, **root**, **Parent**, **leaf**und **all**. Diese wirken sich wie folgt auf das TreeView-Steuerelement aus:

| **Eigenschafts Wert** | **Auswirkung** |
| --- | --- |
| Keine | Kontrollkästchen werden auf keinem Knoten angezeigt. Dies ist die Standardeinstellung. |
| Root | Ein Kontrollkästchen wird nur auf dem Stamm Knoten angezeigt. |
| Parent | Ein Kontrollkästchen wird nur auf den Knoten angezeigt, die über untergeordnete Knoten verfügen. Diese untergeordneten Knoten können übergeordnete Knoten oder Blattknoten sein. |
| Blatt | Ein Kontrollkästchen wird nur auf den Knoten angezeigt, die über keine untergeordneten Knoten verfügen. |
| Alle | Auf allen Knoten wird ein Kontrollkästchen angezeigt. |

Wenn Kontrollkästchen verwendet werden, gibt die **CheckedNodes** -Eigenschaft eine Auflistung von TreeView-Knoten zurück, die beim Postback überprüft werden.

Die **ShowExpandCollapse** -Eigenschaft steuert die Darstellung des Bilds/zureduzierens neben den Stamm-und übergeordneten Knoten. Wenn diese Eigenschaft auf **false**festgelegt ist, werden TreeView-Knoten als Hyperlinks gerendert und durch Klicken auf den Link erweitert bzw. reduziert.

Die **ShowLines** -Eigenschaft steuert, ob Zeilen angezeigt werden, die übergeordnete Knoten mit untergeordneten Knoten verbinden. Der Standardwert **false** gibt an, dass keine Zeilen angezeigt werden. Wenn **true**, verwendet das TreeView-Steuerelement Linienbilder in dem Ordner, der durch die **LineImagesFolder** -Eigenschaft angegeben wird.

Zum Anpassen der Darstellung von TreeView-Linien enthält Visual Studio .net 2005 ein Linien-Designer-Tool. Mithilfe der Smarttagschaltfläche im TreeView-Steuerelement können Sie wie unten beschrieben auf dieses Tool zugreifen.

![](data-bound-controls/_static/image1.jpg)

**Abbildung 1**

Wenn Sie die Menüoption **Linienbilder anpassen** auswählen, wird das Linien-Designer-Tool gestartet, mit dem Sie die Darstellung von TreeView-Zeilen konfigurieren können.

## <a name="treeview-events"></a>TreeView-Ereignisse

Das TreeView-Steuerelement verfügt über die folgenden eindeutigen Ereignisse:

- SelectedNodeChanged tritt auf, wenn ein Knoten basierend auf der **SelectAction** -Eigenschaft ausgewählt wird.
- Treenouncheckchanged tritt auf, wenn der Status eines Knoten checkboxs geändert wird.
- TreeNodeExpanded tritt auf, wenn ein Knoten basierend auf der **SelectAction** -Eigenschaft erweitert wird.
- Treenodebug tritt auf, wenn ein Knoten reduziert wird.
- TreeNode datbound tritt auf, wenn ein Knoten Daten gebunden ist.
- Treenoentpopulate tritt auf, wenn ein Knoten aufgefüllt wird.

Mit der **SelectAction** -Eigenschaft können Sie konfigurieren, welches Ereignis ausgelöst wird, wenn ein Knoten ausgewählt wird. Die SelectAction-Eigenschaft stellt die folgenden Aktionen bereit:

- TreeNodeSelectAction. Expand löst TreeNodeExpanded aus, wenn der Knoten ausgewählt wird.
- TreeNodeSelectAction. None löst kein Ereignis aus, wenn der Knoten ausgewählt wird.
- TreeNodeSelectAction. Select löst das SelectedNodeChanged-Ereignis aus, wenn der Knoten ausgewählt wird.
- TreeNodeSelectAction. SelectExpand löst sowohl das SelectedNodeChanged-Ereignis als auch das TreeNodeExpanded-Ereignis aus, wenn der Knoten ausgewählt wird.

## <a name="controlling-appearance-with-styles"></a>Steuern der Darstellung mit Stilen

Das TreeView-Steuerelement bietet viele Eigenschaften zum Steuern der Darstellung des Steuer Elements mit Stilen. Die folgenden Eigenschaften sind verfügbar:

| **Eigenschaftenname** | **Steuerelemente** |
| --- | --- |
| HoverNodeStyle | Steuert den Stil der Knoten, wenn mit dem Mauszeiger darauf gezeigt wird. |
| LeafNodeStyle | Steuert den Stil der Blattknoten. |
| NodeStyle | Steuert den Stil für alle Knoten. Bestimmte Knotenstile (z. b. LeafNodeStyle) überschreiben diesen Stil. |
| ParentNodeStyle | Steuert den Stil für alle übergeordneten Knoten. |
| RootNodeStyle | Steuert den Stil für den Stamm Knoten. |
| SelectedNodeStyle | Steuert den Stil für den ausgewählten Knoten. |

Jede dieser Eigenschaften ist schreibgeschützt. Allerdings wird jeweils ein **TreeNodeStyle** -Objekt zurückgegeben, und die Eigenschaften dieses Objekts können mit dem *Eigenschafts untereigenschaftenformat* geändert werden. Wenn Sie z. b. die **ForeColor** -Eigenschaft von **SelectedNodeStyle**festlegen möchten, verwenden Sie die folgende Syntax:

[!code-aspx[Main](data-bound-controls/samples/sample8.aspx)]

Beachten Sie, dass das obige Tag nicht geschlossen ist. Dies liegt daran, dass Sie bei Verwendung der hier gezeigten deklarativen Syntax auch die TreeViews-Knoten in den HTML-Code einschließen würden.

Die Stileigenschaften können auch im Code mithilfe des Properties *. Subproperty* -Formats angegeben werden. Wenn Sie z. b. die **ForeColor** -Eigenschaft von **RootNodeStyle** im Code festlegen möchten, verwenden Sie die folgende Syntax:

[!code-csharp[Main](data-bound-controls/samples/sample9.cs)]

> [!NOTE]
> Eine umfassende Liste der unterschiedlichen Stileigenschaften finden Sie in der MSDN-Dokumentation zum TreeNodeStyle-Objekt.

## <a name="the-sitemappath-control"></a>Das SiteMapPath-Steuerelement

Das SiteMapPath-Steuerelement stellt ein Brot Breadcrumb)-Navigations Steuerelement für ASP.NET-Entwickler bereit. Wie die anderen Navigations Steuerelemente kann es problemlos an hierarchische Datenquellen wie SiteMapDataSource oder XmlDataSource gebunden werden.

Ein SiteMapPath-Steuerelement besteht aus SiteMapNodeItem-Objekten. Es gibt drei Typen von Knoten: der Stamm Knoten, übergeordnete Knoten und der aktuelle Knoten. Der Stamm Knoten ist der Knoten am oberen Rand der hierarchischen Struktur. Der aktuelle Knoten stellt die aktuelle Seite dar. Alle anderen Knoten sind übergeordnete Knoten.

## <a name="controlling-the-operation-of-the-sitemappath-control"></a>Steuern des Vorgangs des SiteMapPath-Steuer Elements

Die Eigenschaften, die den Betrieb des SiteMapPath-Steuer Elements steuern, lauten wie folgt:

| **Eigenschaft** | **Beschreibung der Eigenschaft** |
| --- | --- |
| ParentLevelsDisplayed | Steuert, wie viele übergeordnete Knoten angezeigt werden. Der Standardwert ist-1. Dadurch wird die Anzahl der angezeigten übergeordneten Knoten nicht eingeschränkt. |
| PathDirection | Steuert die Richtung von SiteMapPath. Gültige Werte sind RootToCurrent (Standard) und currenttor oot. |
| PathSeparator | Eine Zeichenfolge, die das Zeichen steuert, mit dem Knoten in einem SiteMapPath-Steuerelement getrennt werden. Der Standardwert ist:. |
| RenderCurrentNodeAsLink | Ein boolescher Wert, der steuert, ob der aktuelle Knoten als Link gerendert wird. Die Standardeinstellung lautet „false“. |
| SkipLinkText | Unterstützt die Barrierefreiheit, wenn die Seite von Bildschirmlesern angezeigt wird. Diese Eigenschaft ermöglicht Bildschirmlesern das Überspringen des SiteMapPath-Steuer Elements. Um diese Funktion zu deaktivieren, legen Sie die-Eigenschaft auf String. Empty fest. |

## <a name="templated-sitemappath-controls"></a>Auf Vorlagen basierende SiteMapPath-Steuerelemente

Das sitemapcontrol-Steuerelement ist ein Steuerelement mit Vorlagen. Daher können Sie unterschiedliche Vorlagen für die Anzeige des Steuer Elements definieren. Um Vorlagen in einem SiteMapPath-Steuerelement zu bearbeiten, klicken Sie auf die Smarttag-Schaltfläche im Steuerelement, und wählen Sie im Menü Vorlagen bearbeiten aus. Dadurch wird das Menü sitemaptasks wie unten dargestellt angezeigt, in dem Sie zwischen den verschiedenen verfügbaren Vorlagen wählen können.

![](data-bound-controls/_static/image2.jpg)

**Abbildung 2**

Die **leiten Template** -Vorlage verweist auf jeden beliebigen Knoten in SiteMapPath. Wenn es sich bei dem Knoten um einen Stamm Knoten oder den aktuellen Knoten handelt und eine **RootNode Template** oder **currentnodebug Template** konfiguriert ist, wird die NoDebug-Vorlage überschrieben.

## <a name="sitemappath-events"></a>SiteMapPath-Ereignisse

Das SiteMapPath-Steuerelement verfügt über zwei Ereignisse, die nicht von der Control-Klasse abgeleitet sind. Das **ItemCreated** -Ereignis und das **itemdatdent** -Ereignis. Das ItemCreated-Ereignis wird ausgelöst, wenn ein SiteMapPath-Element erstellt wird. Itemdatraised wird ausgelöst, wenn die DataBind-Methode während der Datenbindung eines SiteMapPath-Knotens aufgerufen wird. Ein **SiteMapNodeItemEventArgs** -Objekt ermöglicht den Zugriff auf das spezifische SiteMapNodeItem über die Item-Eigenschaft.

## <a name="controlling-appearance-with-styles"></a>Steuern der Darstellung mit Stilen

Die folgenden Stile sind zum Formatieren eines SiteMapPath-Steuer Elements verfügbar.

| **Eigenschaftenname** | **Steuerelemente** |
| --- | --- |
| CurrentNodeStyle | Steuert den Text Stil für den aktuellen Knoten. |
| RootNodeStyle | Steuert den Text Stil für den Stamm Knoten. |
| NodeStyle | Steuert den Text Stil für alle Knoten, vorausgesetzt, dass ein CurrentNodeStyle oder RootNodeStyle nicht zutrifft. |

Die NodeStyle-Eigenschaft wird entweder von CurrentNodeStyle oder RootNodeStyle überschrieben. Jede dieser Eigenschaften ist schreibgeschützt und gibt ein **Style** -Objekt zurück. Wenn Sie die Darstellung eines Knotens mit einer dieser Eigenschaften beeinflussen möchten, müssen Sie die Eigenschaften des zurückgegebenen Stil Objekts festlegen. Der folgende Code ändert z. b. die ForeColor-Eigenschaft des aktuellen Knotens.

[!code-aspx[Main](data-bound-controls/samples/sample10.aspx)]

Die-Eigenschaft kann auch Programm gesteuert wie folgt angewendet werden:

[!code-csharp[Main](data-bound-controls/samples/sample11.cs)]

> [!NOTE]
> Wenn eine Vorlage angewendet wird, wird der Stil nicht angewendet.

## <a name="lab-1-configuring-an-aspnet-menu-control"></a>Lab 1: Konfigurieren eines ASP.net-Menü Steuer Elements

1. Erstellen Sie eine neue Website.
2. Fügen Sie eine Site Übersichts Datei hinzu, indem Sie Datei, neu und Datei auswählen und Site Map aus der Liste der Datei Vorlagen auswählen.
3. Öffnen Sie die Site Übersicht (standardmäßig "Web. Sitemap"), und ändern Sie Sie so, dass Sie wie in der folgenden Liste aussieht. Die Seiten, auf die Sie in der Site Übersichts Datei verweisen, sind nicht wirklich vorhanden, aber dies stellt für diese Übung kein Problem dar.

    [!code-xml[Main](data-bound-controls/samples/sample12.xml)]
4. Öffnen Sie das Standardweb Formular in Designansicht.
5. Fügen Sie der Seite im Navigationsbereich der Toolbox ein neues Menu-Steuerelement hinzu.
6. Fügen Sie im Abschnitt "Daten" der Toolbox eine neue SiteMapDataSource hinzu. Von SiteMapDataSource wird automatisch die Datei "Web. Sitemap" auf Ihrer Website verwendet. (Die Datei "Web. Sitemap" *muss* sich im Stamm Ordner der Website befinden.)
7. Klicken Sie auf das Menü Steuerelement, und klicken Sie dann auf die Smarttag-Schaltfläche, um das Dialogfeld Aufgaben
8. Wählen Sie in der Dropdown Liste Datenquelle auswählen die Option SiteMapDataSource1 aus.
9. Klicken Sie auf den Link Auto Format, und wählen Sie ein Format für das Menü aus.
10. Legen Sie im Bereich Eigenschaften für die Eigenschaft **StaticDisplayLevels** den Wert 2 fest. Das Menü Steuerelement sollte jetzt den Knoten "Home", "Products" und "Services" im Designer anzeigen.
11. Navigieren Sie in Ihrem Browser zur Seite, um das Menü zu verwenden. (Da die Seiten, die Sie der Site Übersicht hinzugefügt haben, nicht tatsächlich vorhanden sind, wird ein Fehler angezeigt, wenn Sie versuchen, zu diesen zu navigieren.)

Experimentieren Sie mit dem Ändern der Eigenschaften StaticDisplayLevels und MaximumDynamicDisplayLevels, und sehen Sie sich an, wie sich diese auf die Darstellung des Menüs auswirken.

## <a name="lab-2-dynamically-binding-a-treeview-control"></a>Lab 2: Dynamisches Binden eines TreeView-Steuer Elements

Bei dieser Übung wird vorausgesetzt, dass SQL Server lokal ausgeführt werden und dass die Northwind-Datenbank in der SQL Server Instanz vorhanden ist. Wenn diese Bedingungen nicht erfüllt sind, ändern Sie die Verbindungs Zeichenfolge im Beispiel. Beachten Sie, dass Sie möglicherweise auch SQL Server Authentifizierung anstelle einer vertrauenswürdigen Verbindung angeben müssen.

1. Erstellen Sie eine neue Website.
2. Wechseln Sie zur Code Ansicht für "default. aspx", und ersetzen Sie den gesamten Code durch den unten aufgeführten Code. 

    [!code-aspx[Main](data-bound-controls/samples/sample13.aspx)]
3. Speichern Sie die Seite als TreeView. aspx.
4. Durchsuchen Sie die Seite.
5. Wenn die Seite zum ersten Mal angezeigt wird, können Sie die Quelle der Seite in Ihrem Browser anzeigen. Beachten Sie, dass nur die sichtbaren Knoten an den Client gesendet wurden.
6. Klicken Sie auf das Pluszeichen neben einem beliebigen Knoten.
7. Quelle auf der Seite erneut anzeigen. Beachten Sie, dass die neu angezeigten Knoten jetzt vorhanden sind.

## <a name="lab-3-details-view-and-editing-data-using-a-gridview-and-detailsview"></a>Lab 3: Details zum Anzeigen und Bearbeiten von Daten mithilfe von GridView und DetailsView

1. Erstellen Sie eine neue Website.
2. Fügen Sie der Website eine neue Datei "Web. config" hinzu.
3. Fügen Sie der Datei "Web. config" wie unten dargestellt eine Verbindungs Zeichenfolge hinzu: 

    [!code-xml[Main](data-bound-controls/samples/sample14.xml)]

    > [!NOTE]
    > Möglicherweise müssen Sie die Verbindungs Zeichenfolge basierend auf Ihrer Umgebung ändern.
4. Speichern und schließen Sie die Datei "web.config".
5. Öffnen Sie Default. aspx, und fügen Sie ein neues SqlDataSource-Steuerelement hinzu.
6. Ändern Sie die ID des SqlDataSource-Steuer Elements in **Products**.
7. Klicken Sie im Menü **SqlDataSource-Aufgaben** auf **Datenquelle konfigurieren**.
8. Wählen Sie **Northwind** in der Dropdown Liste Verbindung aus, und klicken Sie auf Weiter.
9. Wählen Sie in der Dropdown Liste **Name** den Eintrag **Produkte** aus, und überprüfen Sie die Kontrollkästchen **ProductID**, **ProductName**, **UnitPrice**und **UnitsInStock** , wie unten dargestellt. 

![](data-bound-controls/_static/image3.jpg)

    **Figure 3**
10. Klicken Sie auf **Weiter**.
11. Klicken Sie auf **Fertig stellen**.
12. Wechseln Sie zur Quell Ansicht, und überprüfen Sie den generierten Code. Beachten Sie die Elemente **SelectCommand**, **DeleteCommand**, **InsertCommand**und **UpdateCommand** , die dem SqlDataSource-Steuerelement hinzugefügt wurden. Beachten Sie auch die Parameter, die hinzugefügt wurden.
13. Wechseln Sie zu Designansicht, und fügen Sie der Seite ein neues GridView-Steuerelement hinzu.
14. Wählen Sie in der Dropdown Liste **Datenquelle auswählen** die Option **Produkte** aus.
15. Aktivieren Sie **Paging aktivieren** und **Auswahl aktivieren** , wie unten gezeigt. 

![](data-bound-controls/_static/image4.jpg)

    **Figure 4**
16. Klicken Sie auf den Link **Spalten bearbeiten** , und vergewissern Sie sich, dass **Felder automatisch generieren** aktiviert ist.
17. Klicken Sie auf **OK**.
18. Wenn Sie das GridView-Steuerelement ausgewählt haben, klicken Sie auf die Schaltfläche neben der **DataKeyNames** -Eigenschaft im Bereich Eigenschaften.
19. Wählen Sie **ProductID** in der Liste **Verfügbare Datenfelder** aus, und klicken Sie auf die Schaltfläche **&gt;** , um Sie hinzuzufügen
20. Klicken Sie auf OK.
21. Fügen Sie der Seite ein neues SqlDataSource-Steuerelement hinzu.
22. Ändern Sie die ID des SqlDataSource-Steuer Elements in **Details**.
23. Wählen Sie im Menü SqlDataSource-Aufgaben die Option **Datenquelle konfigurieren**aus.
24. Wählen Sie in der Dropdown Liste **Northwind** aus, und klicken Sie auf **weiter**
25. Wählen Sie in der Dropdown Liste <strong>Name</strong> den Eintrag <strong>Products</strong> aus, und aktivieren Sie das Kontrollkästchen <strong>\</strong > * im Listenfeld <strong>Spalten</strong> .
26. Klicken Sie auf die Schaltfläche **Where** .
27. Wählen Sie **ProductID** aus der Dropdown Liste **Spalte** aus.
28. Wählen Sie **=** in der Dropdown Liste Operator aus.
29. Wählen Sie in der Dropdown Liste **Quelle** die Option **Steuer** Element
30. Wählen Sie **GridView1** aus der Dropdown Liste Steuerelement- **ID** aus.
31. Klicken Sie auf **Hinzufügen** , um die WHERE-Klausel hinzuzufügen.
32. Klicken Sie auf **OK**.
33. Aktivieren Sie die Schaltfläche **erweitert** , und aktivieren Sie das Kontrollkästchen **INSERT-, Update-und DELETE-Anweisungen generieren** .
34. Klicken Sie auf **OK**.
35. Klicken Sie auf **weiter und dann** auf **Fertig**stellen.
36. Fügen Sie der Seite ein DetailsView-Steuerelement hinzu.
37. Wählen Sie in der Dropdown Liste **Datenquelle auswählen** die Option **Details**aus.
38. Aktivieren Sie das Kontrollkästchen **Bearbeitung aktivieren** , wie unten gezeigt. 

![](data-bound-controls/_static/image1.gif)

    **Figure 5**
39. Speichern Sie die Seite, und suchen Sie "default. aspx".
40. Klicken Sie auf den Link **auswählen** neben verschiedene Datensätze, um das DetailsView-Update automatisch anzuzeigen.
41. Klicken Sie im DetailsView-Steuerelement auf den Link **Bearbeiten** .
42. Nehmen Sie eine Änderung am Datensatz vor, und klicken Sie auf **Aktualisieren**.
