---
ms.openlocfilehash: 170d5ec71b0789bc1efb43fbdd4e24eab36a8cc4
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/01/2019
ms.locfileid: "57037367"
---
<a name="--"></a>--
==================

## <a name="core"></a>Kernspeicher
  * Entfernen nicht verwendeter removeAttrs-Methode.
  * Ersetzen von regex für url-Methode.
  * Entfernen des ungültigen url-Parameters in $.ajax, wird von $.extend überschrieben.
  * Ordnungsgemäße Verarbeitung der geschachtelten Schaltfläche „cancel submit“.
  * Korrigieren des Einzugs.
  * Refactoring von attributeRules und dataRules für gemeinsame Verwendung des Normalisierers.
  * dataRules-Methode zum Konvertieren des Werts in eine Zahl für Zahleneingaben.
  * Aktualisieren der url-Methode, um protokollrelative URLs zu ermöglichen.
  * Entfernen des veralteten $.format-Platzhalters.
  * Verwenden von on/off von JQuery 1.7 oder höher, Hinzufügen der destroy-Methode.
  * IE8-Kompatibilität, .indexOf in $.inArray geändert.
  * Konvertieren der NaN-Wertattribute in „undefined“ für Opera Mini.
  * Beenden der Wertkürzung in der required-Methode.
  * Verwenden des :disabled-Selektors zum Abgleichen deaktivierter Elemente.
  * Ausschließen einiger Tasten, um eine erneute Überprüfung des Felds zu verhindern.
  * Durchsuchen Sie nicht das gesamte DOM nach Optionsfeld- bzw. Kontrollkästchenelementen.
  * Auslösen besserer Fehler für ungültige rule-Methoden.
  * Korrigieren des Zahlenüberprüfungsfehlers.
  * Korrigieren des Verweises auf die whatwg-Spezifikation.
  * Fokussieren des ungültigen Elements bei der Überprüfung eines benutzerdefinierten Satzes von Eingaben.
  * Zurücksetzen der Elementformate bei Verwendung benutzerdefinierter highlight-Methoden.
  * Escapezeichen für Dollarzeichen in Fehler-ID.
  * Wiederherstellen von „Schreibgeschützte und deaktivierte Felder ignorieren“.
  * Aktualisieren des Links in Kommentar für Luhn-Algorithmus.

## <a name="additionals"></a>Zusätze
  * Aktualisieren von dateITA, um Zeitzonenproblem zu behandeln.
  * Korrigieren der extension-Methode nur auf den Methodenzeitraum.
  * Korrigieren der accept-Methode für die Übereinstimmung nur mit dem Zeitraum.
  * Aktualisieren der time-Methode, um einstellige Stundenangaben zu ermöglichen.
  * Löschen des fehlerhaften Tests für notEqualTo-Methode.
  * Hinzufügen der notEqualTo-Methode.
  * Verwenden des richtigen jQuery-Verweises über `$`.
  * Entfernen nutzloser regex-Überprüfung in iban-Methode.
  * Brasilianische CPF-Nummer

## <a name="localization"></a>Lokalisierung
  * Aktualisieren von messages_tr.js.
  * Aktualisieren von messages_sr_lat.js.
  * Hinzugefügt: Peruanisches Spanisch (ES PE).
  * Hinzugefügt: Georgisch (ქართული, ka).
  * Entfernen des Schreibfehlers in katalanischer Übersetzung.
  * Verbessern der finnischen Übersetzung (fi).
  * Hinzufügen des armenischen Gebietsschemas (hy_AM).
  * Erweitern der italienischen Übersetzung (it) mit currency-Methode.
  * Hinzufügen des bn_BD-Gebietsschemas.
  * Aktualisieren des zh-Gebietsschemas.
  * Entfernen des Punkts am Ende von italienischen Meldungen-

<a name="1131--2014-10-14"></a>1.13.1/14.10.2014
==================

## <a name="core"></a>Kernspeicher
  * Zulassen von 0 als Wert für autoCreateRanges.
  * Anwenden der ignore-Einstellung auf alle validationTargetFor-Elemente.
  * Wert in den min-/max-/rangelength-Methoden nicht kürzen.
  * Escapezeichen für ID/Name vor der Verwendung als Selektor in errorsFor verwenden.
  * Expliziter Standardwert für focusCleanup-Option.
  * Korrigieren von falschem regexp für describedby-Matcher.
  * Ignorieren schreibgeschützter und deaktivierter Felder.
  * Verbessern der id-Escapezeichen, Speichern der mit Escapezeichen versehenen ID in describedby.
  * Verwenden des Rückgabewerts von submitHandler, um die Formularübermittlung zu erlauben oder zu verhindern.

## <a name="additionals"></a>Zusätze
  * Hinzufügen der postalcodeBR-Methode.
  * Korrigieren der pattern-Methode, wenn Parameter eine Zeichenfolge ist.


<a name="1130--2014-07-01"></a>1.13.0/01.07.2014
==================

## <a name="all"></a>Alle
* Hinzufügen des Plug-In-UMD-Wrappers.

## <a name="core"></a>Kernspeicher
* Berücksichtigen von aria-describedby, wenn nicht fehlerhaft, und von leeren verborgenen Fehlern.
* Verbessern von dateISO-RegExp.
* Hinzufügen von Optionsfeld/Kontrollkästchen zum Delegieren von click-event.
* Verwenden von aria-describedby für Nichtbezeichnungselemente.
* Registrieren von focusin, focusout und keyup auch für Optionsfeld/Kontrollkästchen.
* Korrigieren der Normalisierung für rangelength-Attributwert.
* Aktualisieren der elementValue-Methode für die Verarbeitung von type="number"-Feldern.
* Verwenden von charAt anstelle der Arraynotation von Zeichenfolgen zur Unterstützung von IE8(?)

## <a name="localization"></a>Lokalisierung
* Korrigieren der sk-Übersetzung der rangelength-Methode.
* Hinzufügen finnischer Methoden.
* Korrigieren der Überprüfungsmeldung der GL-Nummer.
* Korrigieren der Methodenüberprüfungsmeldung der ES-Nummer.
* Hinzufügen von Galizisch (GL).
* Korrigieren der französischen Meldungen für min- und max-Methoden.

## <a name="additionals"></a>Zusätze
* Hinzufügen der statesUS-Methode.
* Korrigieren der dateITA-Methode für die Verarbeitung des DST-Fehlers.
* Hinzufügen der persischen date-Methode.
* Hinzufügen der postalCodeCA-Methode.
* Hinzufügen der postalcodeIT-Methode.

<a name="1120--2014-04-01"></a>1.12.0/01.04.2014
==================

* Hinzufügen von ARIA-Tests ([3d5658e](https://github.com/jzaefferer/jquery-validation/commit/3d5658e9e4825fab27198c256beed86f0bd12577)).
* Hinzufügen von es-AR-Lokalisierungsmeldungen. ([7b30beb](https://github.com/jzaefferer/jquery-validation/commit/7b30beb8ebd218c38a55d26a63d529e16035c7a2))
* Hinzufügen fehlender Punkte zu es- und es_AR-Meldungen. ([a2a653c](https://github.com/jzaefferer/jquery-validation/commit/a2a653cb68926ca034b4b09d742d275db934d040))
* Hinzufügen indonesischer Lokalisierung (ID) ([1d348bd](https://github.com/jzaefferer/jquery-validation/commit/1d348bdcb65807c71da8d0bfc13a97663631cd3a)).
* Hinzufügen von spanischer NIF-, NIE- und CIF-Überprüfung von Dokumentnummern ([#830](https://github.com/jzaefferer/jquery-validation/issues/830), [317c20f](https://github.com/jzaefferer/jquery-validation/commit/317c20fa9bb772770bb9b70d46c7081d7cfc6545)).
* Hinzufügen des aktuelle Formulars zum Kontext der ajax-Remoteanforderung ([0a18ae6](https://github.com/jzaefferer/jquery-validation/commit/0a18ae65b9b6d877e3d15650a5c2617a9d2b11d5)).
* Zusätze: Aktualisieren der IBAN-Methode, kürzen nachfolgender Leerzeichen ([#970](https://github.com/jzaefferer/jquery-validation/issues/970), [347b04a](https://github.com/jzaefferer/jquery-validation/commit/347b04a7d4e798227405246a5de3fc57451d52e1))
* BIC-Methode: Verbessern von RegEx, {1} ist immer redundant. Schließt gh-744 ([5cad6b4](https://github.com/jzaefferer/jquery-validation/commit/5cad6b493575e8a9a82470d17e0900c881130873)).
* Bower: Hinzufügen von Bower.json für die paketregistrierung ([e86ccb0](https://github.com/jzaefferer/jquery-validation/commit/e86ccb06e301613172d472cf15dd4011ff71b398))
* Ändern der Dollarverweise auf „jQuery“ für Kompatibilität mit jQuery.noConflict. Schließt gh-754 ([2049afe](https://github.com/jzaefferer/jquery-validation/commit/2049afe46c1be7b3b89b1d9f0690f5bebf4fbf68)).
* Kern: Hinzufügen des Method-Felds zum fehlerlisteneintrag ([89a15c7](https://github.com/jzaefferer/jquery-validation/commit/89a15c7a4b17fa2caaf4ff817f09b04c094c3884))
* Kern: Unterstützung für generische Meldungen über Data-msg-Attribut ([5bebaa5](https://github.com/jzaefferer/jquery-validation/commit/5bebaa5c55c73f457c0e0181ec4e3b0c409e2a9d))
* Kern: Ermöglichen, dass Attribute den Wert NULL haben (z. B. min = '0') ([#854](https://github.com/jzaefferer/jquery-validation/issues/854), [9dc0d1d](https://github.com/jzaefferer/jquery-validation/commit/9dc0d1dd946b2c6178991fb16df0223c76162579))
* Kern: Deaktivieren von veraltetem $.format ([#755](https://github.com/jzaefferer/jquery-validation/issues/755), [bf3b350](https://github.com/jzaefferer/jquery-validation/commit/bf3b3509140ea8ab5d83d3ec58fd9f1d7822efc5))
* Kern: Korrigieren der Unterstützung für mehrere Fehlerklassen ([c1f0baf](https://github.com/jzaefferer/jquery-validation/commit/c1f0baf36c21ca175bbc05fb9345e5b44b094821))
* Kern: Ignorieren von Ereignissen für ignored-Elemente ([#700](https://github.com/jzaefferer/jquery-validation/issues/700), [a864211](https://github.com/jzaefferer/jquery-validation/commit/a86421131ea69786ee9e0d23a68a54a7658ccdbf))
* Kern: Verbessern der ElementValue-Methode ([6c041ed](https://github.com/jzaefferer/jquery-validation/commit/6c041edd21af1425d12d06cdd1e6e32a78263e82))
* Kern: Stellen Sie element() ignorierte Elemente ordnungsgemäß. ([3f464a8](https://github.com/jzaefferer/jquery-validation/commit/3f464a8da49dbb0e4881ada04165668e4a63cecb))
* Kern: DataRules-Analyse auf W3C HTML5-Spezifikation-Stil wechseln ([460fd22](https://github.com/jzaefferer/jquery-validation/commit/460fd22b6c84a74c825ce1fa860c0a9da20b56bb))
* Kern: Auslösen von Success on optional, aber andere erfolgreiche Validierungssteuerelemente ([#851](https://github.com/jzaefferer/jquery-validation/issues/851), [f93e1de](https://github.com/jzaefferer/jquery-validation/commit/f93e1deb48ec8b3a8a54e946a37db2de42d3aa2a))
* Kern: Verwenden des einfachen Elements anstelle von nicht umschließt das Element erneut ([03cd4c9](https://github.com/jzaefferer/jquery-validation/commit/03cd4c93069674db5415a0bf174a5870da47e5d2))
* Core: Sicherstellen, dass remote zuletzt ausgeführt wird ([#711](https://github.com/jzaefferer/jquery-validation/issues/711), [ad91b6f](https://github.com/jzaefferer/jquery-validation/commit/ad91b6f388b7fdfb03b74e78554cbab9fd8fca6f)).
* Demo: Verwenden der richtigen Option in mehrteiliger Demo. ([#1025](https://github.com/jzaefferer/jquery-validation/issues/1025), [070edc7](https://github.com/jzaefferer/jquery-validation/commit/070edc7be4de564cb74cfa9ee4e3f40b6b70b76f))
* Korrigieren der $/jQuery-Verwendung in additional-methods. Behebt #839 ([#839](https://github.com/jzaefferer/jquery-validation/issues/839), [59bc899](https://github.com/jzaefferer/jquery-validation/commit/59bc899e4586255a4251903712e813c21d25b3e1)).
* Verbessern der chinesische Übersetzungen ([1a0bfe3](https://github.com/jzaefferer/jquery-validation/commit/1a0bfe32b16f8912ddb57388882aa880fab04ffe)).
* Anfängliche ARIA-Required-Implementierung ([bf3cfb2](https://github.com/jzaefferer/jquery-validation/commit/bf3cfb234ede2891d3f7e19df02894797dd7ba5e)).
* Lokalisierung: Ändern der accept-Werte in extension. Behebt #771, schließt gh-793. ([#771](https://github.com/jzaefferer/jquery-validation/issues/771), [12edec6](https://github.com/jzaefferer/jquery-validation/commit/12edec66eb30dc7e86756222d455d49b34016f65))
* Meldungen: Hinzufügen isländischer Lokalisierung ([dc88575](https://github.com/jzaefferer/jquery-validation/commit/dc885753c8872044b0eaa1713cecd94c19d4c73d))
* Meldungen: Fügen Sie fehlender Punkte zu "bg", "fr" fr- und sr-Meldungen hinzu. ([adbc636](https://github.com/jzaefferer/jquery-validation/commit/adbc6361c377bf6b74c35df9782479b1115fbad7))
* Meldungen: Create messages_sr_lat.js ([f2f9007](https://github.com/jzaefferer/jquery-validation/commit/f2f90076518014d98495c2a9afb9a35d45d184e6))
* Meldungen: Create messages_tj.js ([de830b3](https://github.com/jzaefferer/jquery-validation/commit/de830b3fd8689a7384656c17565ee92c2878d8a5))
* Meldungen: Korrigieren der Sr_lat-Übersetzung, Hinzufügen des fehlenden Leerzeichens ([880ba1c](https://github.com/jzaefferer/jquery-validation/commit/880ba1ca545903a41d8c5332fc4038a7e9a580bd))
* Meldungen: Update messages_sr.js, fix missing space ([10313f4](https://github.com/jzaefferer/jquery-validation/commit/10313f418c18ea75f385248468c2d3600f136cfb))
* Methoden: Hinzufügen von zusätzlichen Methode für Währungsangaben ([1a981b4](https://github.com/jzaefferer/jquery-validation/commit/1a981b440346620964c87ebdd0fa03246348390e))
* Methoden: Hinzufügen typografischer Anführungszeichen zur StripHTML satzzeichenentfernung ([aa0d624](https://github.com/jzaefferer/jquery-validation/commit/aa0d6241c3ea04663edc1e45ed6e6134630bdd2f))
* Methoden: Korrigieren der DateITA-Methode, vermeiden von sommerzeitfehlern ([279 b 932](https://github.com/jzaefferer/jquery-validation/commit/279b932c1267b7238e6652880b7846ba3bbd2084))
* Methoden: Lokalisieren von Methoden für Chilenische Kultur (es-CL) ([cf36b93](https://github.com/jzaefferer/jquery-validation/commit/cf36b933499e435196d951401221d533a4811810))
* Methoden: Aktualisieren Sie die e-Mail-Adresse zur Verwendung von HTML5-Regex Entfernen der email2-Methode ([#828](https://github.com/jzaefferer/jquery-validation/issues/828), [dd162ae](https://github.com/jzaefferer/jquery-validation/commit/dd162ae360639f73edd2dcf7a256710b2f5a4e64))
* Pattern-Methode: Entfernen von Trennzeichen, da HTML5-Implementierungen diese auch nicht. ([37992c 1](https://github.com/jzaefferer/jquery-validation/commit/37992c1c9e2e0be8b315ccccc2acb74863439d3e))
* Einschränken des Kreditkarten-Validierungssteuerelements, um Längenüberprüfung einzuschließen. Schließt gh-772 ([f5f47c5](https://github.com/jzaefferer/jquery-validation/commit/f5f47c5c661da5b0c0c6d59d169e82230928a804)).
* Aktualisieren von messages_ko.js, schließt gh-715 ([5da3085](https://github.com/jzaefferer/jquery-validation/commit/5da3085ff02e0e6ecc955a8bfc3bb9a8d220581b)).
* Aktualisieren von messages_pt_BR.js. Schließt gh-782 ([4bf813b](https://github.com/jzaefferer/jquery-validation/commit/4bf813b751ce34fac3c04eaa2e80f75da3461124)).
* Aktualisieren von phonesUK und mobileUK, um neue Präfixe zu akzeptieren. Schließt gh-750 ([d447b41](https://github.com/jzaefferer/jquery-validation/commit/d447b41b830dee984be21d8281ec7b87a852001d)).
* Überprüfen neunstelliger Postleitzahlen. Schließt gh-726 ([165005d](https://github.com/jzaefferer/jquery-validation/commit/165005d4b5780e22d13d13189d107940c622a76f)).
* phoneUS: Hinzufügen von N11-Ausschlüssen. Schließt gh-861 ([519bbc6](https://github.com/jzaefferer/jquery-validation/commit/519bbc656bcb26e8aae5166d7b2e000014e0d12a)).
* resetForm sollte alle aria-invalid-Werte löschen([4f8a631](https://github.com/jzaefferer/jquery-validation/commit/4f8a631cbe84f496ec66260ada52db2aa0bb3733)).
* valid(): Überprüfen Sie alle Elemente an. Behebt #791: valid() überprüft nur das erste (ungültige) Element ([#791](https://github.com/jzaefferer/jquery-validation/issues/791), [6f26803](https://github.com/jzaefferer/jquery-validation/commit/6f268031afaf4e155424ee74dd11f6c47fbb8553)).

<a name="1111--2013-03-22"></a>1.11.1 / 2013-03-22
==================

  * Zurücksetzen auf die Konvertierung auch von Parametern der range-Methode in Zahlen. Schließt gh-702.
  * Ersetzen eines Großteils der Verwendung von PHP durch mockjax-Handler. Außerdem Ausführen von Demobereinigung, Aktualisieren auf neueres Plug-In für maskierte Eingabe. Beibehalten von Captchademo in PHP. Behebt #662.
  * Entfernen von Inlinecodehervorhebung aus milk-Demo. Anzeigen der Quelle funktioniert.
  * Korrigieren der dynamis-totals-Demo durch Kürzen von Leerzeichen aus Vorlageninhalt vor der Übergabe an jQuery-Konstruktor.
  * Korrigieren der min/max-Überprüfung. Schließt gh-666. Behebt #648.
  * Korrigieren, dass „messages“ als Regel ausgegeben werden und eine Ausnahme nach Aktualisierung durch rules("add") verursacht wird. Schließt gh 670, behebt #624.
  * Hinzufügen koreanischer Lokalisierung (ko). Schließt gh-671.
  * Verbessern der UKpostcode-Methode, um weitere ungültige Postleitzahlen auszufiltern. Schließt #682.
  * Aktualisieren von messages_sv.js. Schließt #683.
  * Ändern des Grunt-Links zur Projektwebsite. Schließt #684.
  * Verschieben der remote-Methode in der Liste nach unten, damit sie zuletzt nach allen anderen Methoden, die auf ein Feld angewendet werden, ausgeführt wird. Behebt #679.
  * Aktualisieren der plugin.json-Beschreibung, sollte das Wort „validate“ enthalten.
  * Korrigieren von Schreibfehlern.
  * Korrigieren des jQuery-Ladeprogramms, um eigenen Pfad zu verwenden. Korrigiert geschachtelte Demos.
  * Aktualisieren von grunt-contrib für die Verwendung von PhantomJS 1.8, wenn die Installation über das Knotenmodul „phantomjs“ erfolgt.
  * Veranlassen, dass valid() einen booleschen Wert anstelle von 0 oder 1 zurückgibt. Problem #109 behoben: „valid()“ gibt keinen booleschen Wert zurück.

<a name="1110--2013-02-04"></a>1.11.0/04.02.2013
==================

  * Entfernen des Löschens als Zahlen von `min`-, `max`- und `range`-Regeln. Behebt #455. Schließt gh-528.
  * Aktualisieren bereits vorhandener Bezeichnungen: behebt #430, schließt gh-436.
  * Korrigieren von $.validator.format, um Gruppeninterpolation zu vermeiden. Dabei ersetzt mindestens IE8/9 -bash durch die Übereinstimmung. Behebt #614.
  * Beheben von mimetype-regex.
  * Hinzufügen des Plug-In-Manifests und Aktualisieren der Header nur auf MIT-Lizenz, Löschen unnötiger Doppellizenzierung (wie jQuery).
  * Hebräische Meldungen: Entfernte Punkte am Ende von Sätzen, behebt gh-568
  * Französische Übersetzung für die require_from_group-Überprüfung. Behebt gh-573.
  * Ermöglichen von Gruppen als ein Array oder eine Zeichenfolge, behebt #479.
  * Entfernen von Leerteichen mit mehreren MIME-Typen.
  * Beheben einiger Datumsüberprüfungen, JS-Syntaxfehler.
  * Entfernen der Unterstützung für das Metadaten-Plug-In, Ersetzen durch data-rule- und data-msg-Eigenschaften (hinzugefügt in 907467e8).
  * Hinzufügen von sftp als gültiges url-pattern.
  * Hinzufügen malaiischer Lokalisierung (my).
  * Aktualisieren von localization/messages_hu.js.
  * Entfernen von focusin-/focusout-Polyfill. Behebt #542: Einschluss von jquery.validate führt zu Konflikten mit focusin- und focusout-Ereignissen in IE9.
  * Lokalisierung: Korrigieren des Schreibfehlers in finnischer Übersetzung.
  * Korrigieren von RTM-Demo zum Anzeigen des Symbols „Ungültig“ beim Wechseln aus dem gültigen zurück in den ungültigen Zustand.
  * Korrigieren der vorzeitigen Rückgabe in remote-Funktion, die verhindert, dass der Ajax-Aufruf ausgeführt wird, wenn eine Eingabe zu schnell erfolgt ist. Stellt sicher, dass die Remoteüberprüfung immer den neuesten Wert überprüft.
  * Rückgängigmachen der Fehlerbehebung für #244. Behebt #521: E-Mail-Überprüfung wird sofort ausgelöst, wenn sich Text im Feld befindet.

<a name="1100--2012-09-07"></a>1.10.0/07.09.2012
===================

  * Korrigieren der französischen Zeichenfolgen für nowhitespace, phoneUS, phoneUK und mobileUK basierend auf Communityfeedback.
  * Dateien für language_REGION wurden gemäß dem ISO_3166-1-Standard umbenannt (http://en.wikipedia.org/wiki/ISO_3166-1), für Taiwan ist die Sprache Chinesisch (zh) und die Region Taiwan (TW).
  * Optimieren von RegEx-Mustern, insbesondere für Telefonnummern des Vereinigten Königreichs.
  * Sprachennamen wurden für jede Datei hinzugefügt, und die Sprachcodes wurden gemäß ISO 639-Standard für Estnisch, Georgisch, Ukrainisch und Chinesisch umbenannt(http://en.wikipedia.org/wiki/List_of_ISO_639-1_codes).
  * Hinzufügten kroatischer Lokalisierung (HR).
  * Vorhandene französische Übersetzungen wurden bearbeitet, und französische Übersetzungen für die zusätzlichen Methoden wurden hinzugefügt.
  * Mergen von Änderungen zum Angeben von benutzerdefinierten Fehlermeldungen in Datenattributen.
  * Aktualisieren von UK Mobile Phone Number-Regex für neue Rufnummern. Behebt #154.
  * Hinzufügen eines Elements für success-Aufruf mit Test. Behebt #60.
  * Korrigieren von regex für zusätzliche time-Methode. Behebt #131.
  * resetForm löscht jetzt den alten previousValue für Formularelemente. Behebt #312.
  * Hinzufügen des Kontrollkästchentests zu require_from_group und Ändern, dass require_from_group elementValue verwendet. Behebt #359.
  * Korrigieren von dataFilter-Antwortproblemen in jQuery 1.5.2 oder höher. Behebt #405.
  * Hinzufügen von jQuery Mobile-Demo. Behebt #249.
  * Deoptimieren von findByName aus Gründen der Richtigkeit. Behebt #82: $.validator.prototype.findByName funktioniert in IE7 nicht.
  * Hinzufügen von Unterstützung für US-Postleitzahlen und Tests. Behebt #90.
  * Ändern von lastElement in LastActive in keyup, Überspringen der Überprüfung für Registerkarte oder leeres Element. Behebt #244.
  * Entfernen von Zahlenstripping aus stripHtml. Behebt #2.
  * Korrigieren der ungültigen Anzahl für invalid in gültige Remoteüberprüfung. Behebt #286.
  * Hinzufügen eines Links zu file_input zu Demoindex.
  * Verschieben der alten accept-Methode in additional-Methode der Erweiterung, Hinzufügen neuer accept-Methode zum Verarbeiten der Standardbrowser-mimetype-Filterung. Behebt #287 und ersetzt #369.
  * Deaktivieren des blur-Ereignisses, wenn onfocusout auf FALSE festgelegt ist. Hinzufügen von Test.
  * Korrigieren des Wertproblems für Optionsfelder und Kontrollkästchen. Behebt #363.
  * Hinzufügen eines Tests für rangeWords und Korrigieren von regex und Begrenzungen in der Methode. Behebt #308.
  * Korrigieren von tinyMCE-Demo und Hinzufügen von Link auf der Demoseite. Behebt #382.
  * Ändern der Lokalisierungsmeldung für min/max, behebt #273.
  * Hinzufügen von Pseudoselektor für Texteingabetypen, um Problem mit leerem Typstandardattribut zu beheben. Hinzufügen von Tests und Testmarkup. Behebt #217.
  * Korrigieren des Delegatenfehlers für dynamic-totals-Demo. Behebt #51.
  * Korrigieren falscher Meldung für alphanumerisches Validierungssteuerelement.
  * Entfernen falscher FALSE-Überprüfung für required-Attribut.
  * Korrigieren des required-Attributs für Nicht-HTML5-Browser. Behebt #301.
  * Hinzufügen von Methoden „require_from_group“ und „skip_or_fill_minimum“.
  * Verwenden des richtigen ISO-Codes für Schwedisch.
  * Aktualisieren der HTML-Demodateien für die Verwendung des HTML5-Dokumenttyps.
  * Korrigieren des regex-Problems für Dezimalzahlen ohne führende Nullen. Hinzufügen eines neuen Methodentests. Behebt #41.
  * Einführen einer elementValue-Methode, die nur Zeichenfolgenwerte normalisiert (Arraywert der Mehrfachauswahl nicht ändern). Behebt #116.
  * Unterstützung für dynamisch hinzugefügte submit-Schaltflächen und Aktualisieren des Testfalls. Verwenden von validateDelegate. Code aus PR #9
  * Korrigieren des ungültiges doppelten Anführungszeichens in Prüfvorrichtungen.
  * Korrigieren der maxWords-Methode, um die obere Begrenzung ein- und nicht auszuschließen. Behebt #284.
  * Korrigieren des Grammatikfehlers in der deutschen Meldung des range-Validierungssteuerelements. Behebt #315.
  * Korrigieren der Verarbeitung von mehreren Klassennamen für errorClass-Option. Test von Max Lynch. Behebt #280.
  * Korrigieren der Verwendung von jQuery.format, sollte $.validator.format lauten. Behebt #329.
  * Methoden für „alle“ UK-Telefonnummern und UK-Postleitzahlen.
  * Pattern-Methode: Konvertieren des abfragezeichenfolgeparameters in RegExp. Behebt Fehler #223.
  * Grammatikfehler in deutscher Lokalisierungsdatei.
  * Hinzufügen estnischer Lokalisierung für Meldungen.
  * Verbessern der QuickInfo-Verarbeitung für themerollered-Demo.
  * Hinzufügen von type="text" zu Eingabefeldern ohne Typattribut zugunsten von qSA.
  * Aktualisieren der themerollered-Demo für die Verwendung einer QuickInfo, um Fehler als Überlagerung anzuzeigen.
  * Aktualisieren der themerollered-Demo für die Verwendung der neuesten jQuery-Benutzeroberfläche (zusammen mit der neueren jQuery-Version). Verschieben von Code, um das Laden von Seiten zu beschleunigen.
  * Korrigieren der in Japanisch nicht funktionierenden min-Meldung.
  * Aktualisieren des Formular-Plug-Ins auf die neueste Version. Verbessern der ajaxSubmit-Demo.
  * Löschen der dateDE- und numberDE-Methoden aus classRuleSettings (durch Verschieben in lokalisierte Methoden überflüssig).
  * Übergeben des submit-Ereignisses an submitHandler-Rückruf.
  * Korrigieren von #219: Korrigieren von valid() für Elemente mit Abhängigkeitsrückruf oder Abhängigkeitsausdruck.
  * Verbessern des Builds zum Entfernen des dist-Verzeichnisses, um sicherzustellen, dass nur das aktuelle Release gezippt wird.

<a name="190"></a>1.9.0
---
* Hinzufügen baskischer Lokalisierung (EU).
* Hinzufügen slowenischer Lokalisierung (SL).
* Korrigieren von Problem #127: Finnische Übersetzungen verwenden : anstelle von ;.
* Korrigieren der russischen Lokalisierung, kleineres Syntaxproblem.
* Hinzufügen von Unterstützung für HTML5-Eingabetypen, behebt #97.
* Verbesserte Unterstützung für HTML5 durch Festlegen des novalidate-Attribut des Formulars und Lesen des type-Attributs.
* Korrigieren, dass showLabel() alle Klassen aus dem Fehlerelement entfernt. Entfernen nur von settings.validClass. Behebt #151.
* Hinzufügen von „pattern“ zu additional-methods, um die Überprüfung anhand beliebiger regulärer Ausdrücke auszuführen.
* Verbessern der email-Methode, um den Punkt am Ende nicht zuzulassen (nach RFC gültig, hier aber unerwünscht). Behebt #143.
* Korrigieren der schwedischen und norwegischen Übersetzungen, min-/max-Meldungen wurden vertauscht. Behebt #181.
* Korrigieren von #184, resetForm: sollte die Festlegung von lastElement aufheben.
* Korrigieren von #71: Verbessern der vorhandenen time-Methode und Hinzufügen der time12h-Methode für 12-Stunden-Zeitformat (AM und PM).
* Korrigieren von #177: Korrigieren der Überprüfung einer einzelnen Optionsfeld- oder Kontrollkästcheneingabe.
* Korrigieren von #189: Ausgeblendete Elemente werden jetzt standardmäßig ignoriert.
* Korrigieren von #194: Required als Attribut schlägt fehl, wenn jQuery>=1.6, Verwenden von .prop anstelle von .attr.
* Korrigieren von #47, #39, #32: Zulassen, dass Kreditkartennummern Leerzeichen und Bindestriche enthalten(Leerzeichen werden von Benutzern häufig eingegeben).

<a name="181"></a>1.8.1
---
* Hinzufügen von thailändischer Lokalisierung (TH), behebt #85.
* Hinzufügen von vietnamesischer Lokalisierung (VI) dank Ngoc.
* Korrigieren von Problem #78. Error-/Valid-Stil gilt für alle Optionsfelder der gleichen Gruppe für erforderliche Überprüfung.
* Verwenden Sie nicht „form.elements“, weil dies in jQuery 1.6 nicht mehr unterstützt wird. Sowieso schwerwiegend fehlerhaft (IE6-8: form.elements === form).

<a name="180"></a>1.8.0
---
* NL-Lokalisierung wurde verbessert (http://plugins.jquery.com/node/14120)
* Hinzufügen georgischer Lokalisierung (GE) dank Avtandil Kikabidze.
* Hinzufügen serbischer Lokalisierung (SR) dank Aleksandar Milovac.
* Hinzufügen von ipv4 und ipv6 zu zusätzlichen Methoden dank Natal Ngétal.
* Hinzufügen japanischer Lokalisierung (JA) dank Bryan Meyerovich.
* Hinzufügen katalanischer Lokalisierung (CA) dank Xavier de Pedro.
* Korrigieren fehlender var-Anweisungen in for-in-Schleifen.
* Remoteüberprüfung, bei der eine formatierte Meldung beschädigt wurde korrigiert (https://github.com/jzaefferer/jquery-validation/issues/11)
* Problembehandlungen für Kompatibilität mit jQuery 1.5.1 bei Aufrechterhaltung der Abwärtskompatibilität.

<a name="17"></a>1.7
---
* Hinzufügen litauischer Lokalisierung (LT).
* Griechische Lokalisierung (EL) wurde hinzugefügt (http://plugins.jquery.com/node/12319)
* Lettische Lokalisierung (LV) wurde hinzugefügt (http://plugins.jquery.com/node/12349)
* Hebräische Lokalisierung (HE) wurde hinzugefügt (http://plugins.jquery.com/node/12039)
* Spanische Lokalisierung (ES) wurde korrigiert (http://plugins.jquery.com/node/12696)
* Hinzufügen der themerolled-Demo der jQuery-Benutzeroberfläche.
* Entfernen von cmxform.js.
* Vier fehlende Semikolons wurden behoben (http://plugins.jquery.com/node/12639)
* Umbenennen von phone-method in additional-methods.js in phoneUS.
* Die Methoden phoneUK und mobileUK wurden „additional-methods.js“ hinzugefügt (http://plugins.jquery.com/node/12359)
* Optionen wurden tiefgreifend erweitert, um zu vermeiden, dass mehrere Formulare geändert werden, wenn die „rules-method“ für ein einzelnes Element verwendet wird (http://plugins.jquery.com/node/12411)
* Problembehandlungen für Kompatibilität mit jQuery 1.4.2 bei Aufrechterhaltung der Abwärtskompatibilität.

<a name="16"></a>1.6
---
* Hinzufügen von arabischer (AR), portugiesischer (PTPT), persischer (FA), finnischer (FI) und bulgarischer (BR) Lokalisierung.
* Aktualisieren der schwedischen Lokalisierung (SE) (einige fehlende HTML-ISO-Zeichen).
* Korrigieren von $.validator.addMethod, um leere Zeichenfolge ordnungsgemäß zu verarbeiten (im Vergleich zu nicht definiert für das message-Argument).
* Korrigieren von zwei versehentlichen globalen Variablen.
* Optimieren von min/max/rangeWords (in additional-methods.js), um HTML vor der Zählung zu entfernen (gut beim Zählen von Wörtern in einem Rich-Text-Editor).
* Hinzufügen lokalisierter Methoden für DE, NL und PT, Entfernen der dateDE- und numberDE-Methoden (stattdessen Verwendung von messages_de.js und methods_de.js mit Datums- und Zahlenmethoden).
* Korrigieren der Synchronisierung der Remoteformularübermittlung dank Matas Petrikas.
* Die interaktive select-Überprüfung wurde verbessert, nun wird auch bei click (über „option“ oder „select“, inkonsistent bei verschiedenen Browsern) überprüft. Das funktioniert nicht in Safari, weil dort kein click-Ereignis für select-Elemente ausgelöst wird. Damit wurde http://plugins.jquery.com/node/11520 behoben.
* Das aktuelle Formular-Plug-In (2.36) wurde aktualisiert, wodurch http://plugins.jquery.com/node/11487 behoben wurde.
* Anbindung an blur-Ereignis für equalTo-Ziel wurde integriert, um eine erneute Überprüfung auszuführen, wenn sich dieses Ziel ändert. Damit wurde http://plugins.jquery.com/node/11450 behoben.
* Die select-Überprüfung wurde vereinfacht. Das Delegieren an die val()-Methode von jQuery zum Abrufen des select-Werts sollte http://plugins.jquery.com/node/11239 beheben.
* Die Standardnachricht für Ziffern wurde behoben (http://plugins.jquery.com/node/9853)
* Ein Problem mit zwischengespeicherten Remotenachrichten wurde behoben (http://plugins.jquery.com/node/11029 und http://plugins.jquery.com/node/9351)
* Ein fehlendes Semikolon in „additional-methods.js“ wurde behoben (http://plugins.jquery.com/node/9233)
* Die automatische Erkennung von Ersetzungsparametern in Nachrichten wurde hinzugefügt, wodurch das Angeben von Formatfunktionen nicht mehr notwendig ist (http://plugins.jquery.com/node/11195)
* Ein Problem mit :filled/:blank wurde behoben, das durch Sizzle verursacht wurde (http://plugins.jquery.com/node/11144)
* Eine integer-Methode wurde in „additional-methods.js“ hinzugefügt (http://plugins.jquery.com/node/9612)
* Ein Problem mit der errorsFor-Methode wurde behoben, bei dem das for-Attribut Zeichen enthielt, die Escapezeichen erforderten, um innerhalb eines Selektors gültig zu sein (http://plugins.jquery.com/node/9611)

<a name="155"></a>1.5.5
---
* http://plugins.jquery.com/node/8659 wurde behoben
* Korrigieren des nachgestellten Kommas in messages_cs.js.

<a name="154"></a>1.5.4
---
* Ein Fehler mit der Remotemethode wurde behoben (http://plugins.jquery.com/node/8658)

<a name="153"></a>1.5.3
---
* Ein Fehler im Zusammenhang mit der „wrapper-option“ wurde behoben, bei dem alle Vorgängerelemente, die mit „wrapper-option“ übereinstimmen, ausgewählt wurden (http://plugins.jquery.com/node/7624)
* Aktualisieren der mehrteiligen Demo für die Verwendung des neuesten Accordion der jQuery-Benutzeroberfläche.
* Hinzufügen der dateNL- und time-Methoden zu additionalMethods.js.
* Hinzufügen von Lokalisierung für Chinesisch (traditionell) (Taiwan, tw) und Kasachisch (KK).
* jQuery.format (früher String.format) wurde in jQuery.validator.format verschoben, jQuery.format ist veraltet und wird in Version 1.6 entfernt (Ausführliche Informationen finden Sie unter http://code.google.com/p/jquery-utils/issues/detail?id=15)
* Bereinigen von messages_pl.js und messages_ptbr.js (weiterhin definierte Meldungen für max/min/rangeValue, die in Version 1.4 entfernt wurden).
* Fehlerhafte boolesche Logik in „valid-plugin-method“ wurde für mehrere Elemente korrigiert, nun müssen alle Elemente für ein boolean-true-Ergebnis gültig sein (http://plugins.jquery.com/node/8481)
* Enhancement $.validator.addMethod: Ein nicht definiertes drittes Message-Argument wird nicht zu einer vorhandenen Meldung (überschreiben http://plugins.jquery.com/node/8443)
* Verbesserung an SubmitHandler-Option: Wenn verwendet, klicken Sie auf die Ereignisse auf submit-Schaltflächen erfasst und die übermittelnde Schaltfläche in das Formular eingefügt werden, bevor SubmitHandler aufgerufen wird und anschließend entfernt; bleibt intakt () senden-Schaltflächen http://plugins.jquery.com/node/7183#comment-3585)
* Die Option validClass (Standardwert „valid“) wurde hinzugefügt, die diese Klasse allen gültigen Elementen nach der Überprüfung hinzufügt (http://dev.jquery.com/ticket/2205)
* Die creditcardtypes-Methode wurde in „additionalMethods.js“ hinzugefügt, einschließlich Tests (über http://dev.jquery.com/ticket/3635)
* Die Remotemethode zum Zulassen der serverseitigen Nachricht als Zeichenfolge oder von TRUE für gültig oder FALSE für ungültig unter Verwendung der auf der Clientseite definierten Nachricht wurde verbessert (http://dev.jquery.com/ticket/3807)
* Die accept-Methode wurde verbessert, um auch eine durch Trennzeichen getrennte Liste von Werten im Drupal-Stil anzunehmen (http://plugins.jquery.com/node/8580)

<a name="152"></a>1.5.2
---
* Korrigieren von Nachrichten in additional-methods.js für maxWords, minWords und rangeWords, um den Aufruf von $.format einzuschließen.
* Korrigieren des Werts, der an Methoden übergeben wird, um Wagenrücklauf (\r) auszuschließen (wie es bei val() von jQuery der Fall ist).
* Hinzufügen slowakischer Lokalisierung (sk).
* Hinzufügen von Demo für die Integration in Registerkarten der jQuery-Benutzeroberfläche.
* Hinzufügen des selects-grouping-Beispiels zur tabs-Demo (siehe zweite Registerkarte, birthdate-Feld).

<a name="151"></a>1.5.1
---
* Aktualisieren der marketo-Demo für die Verwendung der invalidHandler-Option, anstatt das invalidForm-Ereignis zu binden.
* Hinzufügen des TinyMCE-Integrationsbeispiels.
* Hinzufügen ukrainischer Lokalisierung (ua).
* Korrigieren der Längenüberprüfung für abgeschnittene Werte (Regression von Version 1.5, in der allgemeine Kürzung vor der Überprüfung entfernt wurde).
* Verschiedene kleine Korrekturen für die Kompatibilität mit den Versionen 1.2.6 und 1.3.

<a name="15"></a>1.5
---
* Optimieren der Basisdemo, Überprüfen des confirm-password-Felds nach Kennwortänderung.
* Korrigieren der Basisüberprüfung zum Übergeben des ungekürzten Eingabewerts als ersten Parameter an Überprüfungsmethoden, required entsprechend geändert. Vorhandene benutzerdefinierte Methoden werden abgebrochen, die Kürzung verwenden.
* Hinzufügen norwegischer (no), italienischer (it), ungarischer (hu) und rumänischer (ro) Lokalisierung.
* Korrigieren von #3195: Zwei Fehler in der schwedischen Lokalisierung.
* Feste #3503: Erweiterte dieser Option zum Annehmen der Messages-Eigenschaft: verwenden, um anzugeben Hinzufügen von benutzerdefinierten Meldungen in ein Element über Regeln ("hinzufügen", {Nachrichten: {erforderlich: "Required! " } });
* Korrigieren von #3356: Regression aus #2908 bei Verwendung des Meta-Option.
* Feste #3370: Hinzugefügte IgnoreTitle-Option zum Lesen von Nachrichten aus dem Title-Attribut zu überspringen ist hilfreich, um Probleme mit der Google-Symbolleiste zu vermeiden. Standardwert ist "false" für Anwendungskompatibilität
* Korrigieren von #3516: Trigger invalidform-Ereignis, wenn Remoteüberprüfung beteiligt ist.
* Die Option „invalidHandler“ wurde als Kurzform für bind("invalid-form", function() {}) hinzugefügt.
* Korrigieren des Safari-Problems zum Laden des Indikators in ajaxSubmit-integration-demo (zuerst an Hauptteil anfügen, dann ausblenden).
* Hinzufügen eines Tests für die Kreditkartenüberprüfung und verbesserte Standardnachricht.
* Optimieren der Remoteüberprüfung, Optionen für Übergabe an $.ajax als Parameter (URL-Zeichenfolge oder Optionen, einschließlich der url-Eigenschaft und aller anderen Elemente, die $.ajax unterstützt).

<a name="14"></a>1.4
---
* Korrigieren von #2931, Überprüfen von Elementen in Dokumentreihenfolge und Ignorieren von type=image-Eingaben.
* Korrigieren der Verwendung von $- und jQuery-Variablen, jetzt vollständig kompatibel mit allen Varianten der NoConflict-Syntax.
* Implementieren von #2908, Aktivieren benutzerdefinierter Nachrichten über Metadaten in der Form von class="{required:true,messages:{required:'required field'}}", Hinzufügen von demo/custom-messages-metadata-demo.html.
* Entfernen veralteter Methoden minValue (min), maxValue (max), rangeValue (rangevalue), minLength (minlength), maxLength (maxlength), rangeLength (rangelength).
* Feste #2215-Regression: Aufrufen von unhighlight nur für aktuelle Elemente, die nicht alles
* Implementieren von #2989, Aktivieren der image-Schaltfläche zum Abbrechen der Überprüfung.
* Korrigieren von Problem, dass IE eine falsche Überprüfung anhand von maxlength=0 ausführt.
* Hinzufügen von tschechischer Lokalisierung (cs).
* Zurücksetzen von validator.submitted für validator.resetForm(), Aktivieren einer vollständigen Zurücksetzung bei Bedarf.
* Korrigieren von #3035, Überspringen aller falschen Attribute beim Lesen von Regeln (0, nicht definiert, leere Zeichenfolge), Entfernen eines Teils der maxlength-Problemumgehung (für 0).
* Hinzufügen niederländischer Lokalisierung (nl) (#3201).

<a name="13"></a>1.3
---
* Korrigieren des invalid-form-Ereignisses, wird nun nur ausgelöst, wenn das Formular ungültig ist.
* Hinzufügen spanischer (es), russischer (ru), portugiesischer (Brasilien) (ptbr), türkischer (tr) und polnischer (pl) Lokalisierung.
* Hinzufügen des removeAttrs-Plug-Ins, um das Hinzufügen und Entfernen mehrerer Attribute zu ermöglichen.
* Hinzufügen der groups-Option, um eine einzelne Nachricht für mehrere Elemente über groups anzuzeigen: { arbitraryGroupName: "fieldName1 fieldName2[, fieldNameN" }.
* Optimieren von rules() zum Hinzufügen und Entfernen von (statischen) Regeln: rules("add", "method1[, methodN]"/{method1:param[, method_n:param]}) und rules("remove"[, "method1[, method_n]").
* Optimieren von rules-option, akzeptiert durch Leerzeichen getrennte Zeichenfolgenliste von Methoden, z. B. {birthdate: "required date"}.
* Behoben, der Überprüfung der Kontrollkästchengruppe mit Inlineregeln: Solange die Regeln für das erste Element angegeben werden, wird die Gruppe jetzt ordnungsgemäß auf überprüft
* Korrigieren von #2473, Ignorieren aller Regeln mit einem expliziten Parameter boolean-false. required:false entspricht z. B. dem Auslassen der Angabe required (wurde bisher als required:true behandelt).
* Feste #2424, mit einem geänderten Patch aus #2473: Methoden, die Dependency-Mismatch zurückgeben anhalten nicht andere Regeln ausgewertet wird, nicht mehr; Erfolg nicht dennoch für optionale Felder angewendet.
* Korrigieren der URL- und E-Mail-Überprüfung, damit keine abgeschnittenen Werte verwendet werden.
* Kreditkartenüberprüfung korrigiert: Nur Ziffern und Bindestriche werden angenommen („asdf“ ist keine gültige Kreditkartennummer).
* Zulassen von Schaltflächen- und Eingabeelementen für cancel-Schaltflächen (über class="cancel").
* Korrigieren von #2215: Korrigieren der Meldungsanzeige zum Aufrufen von unhighlight im Rahmen des ein- und Ausblendens von Meldungen, keine weiteren visuellen Nebeneffekte beim Überprüfen von einem Element und das Extrahieren von validator.checkForm zum Überprüfen eines Formulars ohne Benutzeroberflächen-Nebeneffekte.
* Umschreiben benutzerdefinierter Selektoren (:blank, :filled, :unchecked) mit Funktionen aus Gründen der Kompatibilität mit AIR.

<a name="121"></a>1.2.1
-----

* Bündeln des Delegaten-Plug-Ins mit dem Überprüfungs-Plug-In: sowieso immer erforderlich.
* Optimieren der Remoteüberprüfung. Sie enthält nun Teile aus dem ajaxQueue-Plug-In für die ordnungsgemäße Synchronisierung (kein weiteres Plug-In erforderlich).
* Korrigieren von stopRequest, um zu verhindern, dass PendingRequest < 0 ist.
* Hinzufügen der jQuery.validator.autoCreateRanges-Eigenschaft, Standarwert FALSE, Ermöglichen der Konvertierung von min/max in range und minlength/maxlength in rangelength. Dies behebt im Wesentlichen das Problem, das durch automatisches Erstellen von Bereiche in Version 1.2 entstanden ist.
* „Optional-methods“ korrigiert: Keine Hervorhebungen, wenn das Feld leer ist bzw. wenn kein Erfolg ausgelöst wird.
* Zulassen von FALSE/NULL für die Optionen highlight/unhighlight, anstatt selbst dann einen do-nothing-Rückruf zu erzwingen, wenn keine Hervorhebung erforderlich ist.
* Korrigieren des validate()-Aufrufs ohne ausgewählte Elemente. undefined wird zurückgegeben, anstatt einen Fehler auszulösen.
* Verbessern der Demo, Ersetzen von Metadaten durch Klassen/Attribute zum Angeben von Regeln.
* Korrigieren des Fehlers, wenn keine benutzerdefinierten Meldung für die Remoteüberprüfung verwendet wird.
* Ändern der E-Mail- und URL-Überprüfung, damit die Domänenbezeichnung und die Bezeichnung der obersten Ebene erforderlich ist.
* Ändern der URL- und E-Mail-Überprüfung, damit TLD (für die Domänenbezeichnung) erforderlich ist. Version 1.2 (TLD ist optional) wird als url2 und email2 in Zusätze verschoben.
* Korrigieren der dynamic-totals-Demo in IE6/7 und Optimieren der Vorlagenverwendung, Verwenden von textarea zum Speichern mehrzeiliger Vorlagen- und Zeichenfolgeninterpolation.
* Hinzufügen eines Anmeldeformularbeispiels mit Link für das E-Mail-Kennwort, durch den das Kennwortfeld optional wird.
* Optimieren der dynamic-totals-Demo durch ein Beispiel für eine einzelne Nachricht für zwei Felder.

<a name="12"></a>1.2
---

* Ein AJAX-captcha-Überprüfungsbeispiel wurde hinzugefügt (auf der Grundlage von http://psyrens.com/captcha/)
* Hinzufügen der remember-the-milk-Demo (Dank an das RTM-Team für die Erlaubnis).
* Hinzufügen der marketo-Demo (Dank an Glen Lipka).
* Hinzufügen von Unterstützung für ajax-validation, siehe Methode „remote“, Serverseite gibt JSON zurück, TRUE für gültige Elemente, FALSE oder eine Zeichenfolge für ungültige Elemente, Zeichenfolge wird als Nachricht verwendet.
* Hinzufügen der highlight- und unhighlight-Optionen, errorClass wird standardmäßig für Element umgeschaltet, ermöglicht benutzerdefinierte Hervorhebung.
* Hinzufügen der valid()-Plug-In-Methode für einfaches programmgesteuertes Überprüfen von Formularen und Feldern ohne Erfordernis, die Validierungssteuerelement-API zu verwenden.
* Hinzufügen der rules()-Plug-In-Methode zum Lesen und Schreiben von Regeln für ein Element (zurzeit nur schreibgeschützt).
* „regex“ wurde dank des Beitrags von Scott Gonzalez für die E-Mail-Methode ersetzt, siehe http://projects.scottsplayground.com/email_address_validation/
* Umstrukturieren der Ereignisarchitektur, um ausschließlich Delegierung zu verwenden, Optimieren der Leistung und einfacheren Verwendung für den Entwickler (erfordert jquery.delegate.js).
* Die lokale Dokumentation wurde nach http://docs.jquery.com/Plugins/Validation verschoben (einschließlich der interaktiven Beispiele für alle Methoden)
* Entfernen von validator.refresh(), Überprüfung ist jetzt vollständig dynamisch.
* Umbenennen von minValue in min, maxValue in max und rangeValue in range, die vorherigen Namen sind nun veraltet (sie werden in Version 1.3 entfernt).
* Umbenennen von minLength in minlength, maxLength in maxlength und rangeLength in rangelength, die vorherigen Namen sind nun veraltet (sie werden in Version 1.3 entfernt).
* Hinzufügen eines Features zum Mergen von min + max und range und minlength + maxlength in rangelength.
* Hinzufügen von Unterstützung für dynamische Regelparameter, Ermöglichen der Angabe einer Funktion als Parameter, z. B. für minlength, aufgerufen beim Überprüfen des Elements.
* Erlauben, dass null oder eine leere Zeichenfolge als Meldung angegeben wird, die nichts anzeigt (siehe marketo-Demo).
* Regelüberholung: Nun unterstützt die Kombination aus Regeln-Option, Metadaten, Klassen (neu) und Attributen (neu), finden Sie unter rules() details

<a name="112"></a>1.1.2
---

* „regex“ wurde dank des Beitrags von Scott Gonzalez für die URL-Methode ersetzt, siehe http://projects.scottsplayground.com/iri/
* Optimieren der email-Methode, um Unicode-Zeichen besser verarbeiten zu können.
* Korrigieren, dass der Fehlercontainer ausgeblendet wird, wenn alle Elemente gültig sind, und nicht nur beim Senden des Formulars.
* Korrigieren von String.format in jQuery.format (Verschieben in den jQuery-Namespace).
* Korrigieren der accept-Methode, um Erweiterungen in Groß- und Kleinbuchstaben zu akzeptieren.
* Korrigieren der validate()-Plug-In-Methode, um nur eine Validierungssteuerelementinstanz für ein bestimmtes Formular zu erstellen und stets diese Instanz zurückzugeben (vermeidet mehrfache Bindungsereignisse).
* Ändern des debug-mode-Konsolenprotokolls aus der Stufe „error“ in „warn“.

<a name="111"></a>1.1.1
-----

* Korrigieren von ungültigem XHTML, Verhindern der Fehlerbezeichnungserstellung in IE seit jQuery 1.1.4.
* Feste und verbesserte "String.Format": Globales suchen / ersetzen, bessere Verarbeitung von Arrayargumenten.
* Korrigieren der cancel-button-Verarbeitung für die Verwendung von validator-object zum Speichern des Zustands anstelle des Formularelements.
* Korrigieren von Namenselektoren für die Verarbeitung „komplexer“ Namen, z. B. von Namen, die Klammern enthalten („list[]“).
* Hinzufügen, dass button- und disabled-Elemente von der Überprüfung ausgeschlossen werden.
* Verschieben von Elementereignishandlern für die Aktualisierung, um neuen Elementen Handler hinzufügen zu können.
* Korrigieren der E-Mail-Überprüfung, um lange Domänen auf oberster Ebene zu ermöglichen (z. B. „.travel“).
* Verschieben von showErrors() aus valid() in form().
* Hinzufügen von validator.size(): Gibt die Anzahl der aktuellen Fehler zurück.
* Aufrufen von submitHandler mit Validierungssteuerelement als Bereich für einfacheren Zugriff auf dessen Methoden, z. B. zum Suchen nach Fehlerbezeichnungen mit errorsFor(Element).
* Kompatibel mit jQuery 1.1.x und 1.2.x.

<a name="11"></a>1.1
---

* Hinzufügen von Überprüfung bei blur, keyup und click (für Kontrollkästchen und radiobutton). Ersetzt event-option.
* Korrigieren von resetForm.
* Korrigieren der custom-methods-Demo.

<a name="10"></a>1.0
---

* Optimieren der number- und numberDE-Methoden, um die Überprüfung auf richtige Dezimalzahlen mit Trennzeichen zu ermöglichen.
* Nur Elemente mit Regeln werden überprüft (andernfalls wird success-option auf alle Elemente angewendet).
* Hinzufügten der creditcard number-Methode (dank Brian Klug).
* Hinzufügen von ignore-option, z. B. ignore: "[@type=hidden]". Dieser Ausdruck wird verwendet, um Elemente von der Überprüfung auszuschließen. Standardwert: keiner, obwohl submit- und reset-Schaltflächen immer ignoriert werden.
* Stark erweiterte Funktionen als Nachrichten durch Bereitstellen eines flexiblen String.format-Hilfsprogramms.
* Akzeptieren von Funktionen als Nachrichten, Bereitstellen von runtime-custom-messages.
* Korrigieren des Ausschlusses von Elementen ohne Regeln aus successList.
* Korrigieren von custom-method-demo, Ersetzen der Warnung durch eine Meldung, die die Anzahl der Fehler anzeigt.
* Korrigieren von form-submit-prevention bei Verwendung von submitHandler.
* Die Abhängigkeit von Element-IDs wurde vollständig entfernt, obwohl diese weiterhin verwendet werden (sofern vorhanden), um Fehlerbezeichnungen mit Eingaben zu verknüpfen. Dies wurde erreicht, indem ein Array mit {name, message, element} anstelle eines Objekts mit id:message-Paaren für die interne Fehlerliste verwendet wird.
* Hinzufügen von Unterstützung zum Angeben einfacher Regeln als einfache Zeichenfolgen, z. B. entspricht „required“ {required:true}.
* Hinzufügen eines Features: Ungültiges Feld s übergeordneten Elements, vereinfacht das Formatieren von das Bezeichnungsfeld/Container oder die Bezeichnung für das Feld wird ErrorClass hinzugefügt.
* Hinzufügen eines Features: focusCleanup. Wenn aktiviert, wird errorClass aus den ungültigen Elementen entfernt, und alle Fehlermeldungen werden ausgeblendet, wenn das Element den Fokus besitzt.
* Hinzufügen von success-option zum Anzeigen, dass ein Feld erfolgreich überprüft wurde.
* Korrigieren des select-issue von Opera (vermeiden von attribute-collision).
* Korrigieren von Problemen mit dem Fokus ausgeblendeter Elemente in IE.
* Hinzufügen eines Features zum Überspringen der Überprüfung für submit-Schaltflächen mit der Klasse „cancel“.
* Korrigieren potenzieller Probleme mit der Google-Symbolleiste, indem Plug-In-Optionsmeldungen dem title-Attribut vorgezogen werden.
* submitHandler wird nur aufgerufen, wenn ein tatsächliches submit-Ereignis verarbeitet wurde, validator.form() gibt FALSE nur für ungültige Formulare zurück.
* Ungültige Elemente erhalten nun den Fokus nur noch bei submit oder über validator.focusInvalid(), um alle Probleme mit focus-on-blur zu vermeiden.
* Problem mit dem IE6-Fehlercontainerlayout wurde gelöst.
* Anpassen des error-Elements über die errorElement-Option.
* Hinzufügen von validator.refresh(), um neue Eingaben im Formular zu ermitteln.
* Hinzufügen der accept validation-Methode, die Dateierweiterungen überprüft.
* Abhängigkeitsfeatures durch Hinzufügen von zwei benutzerdefinierten Ausdrücken wurden verbessert: „:blank“ zum Auswählen von Elementen mit einem leeren Wert und „:filled“ zum Auswählen von Elementen mit einem Wert. Beide Ausdrücke schließen Leerräume aus.
* Hinzufügen einer resetform()-Methode zum Validierungssteuerelement: Setzt jedes Formularelement (mithilfe der Formular-Plug-in, falls verfügbar), entfernt Klassen für ungültige Elemente und blendet alle Fehlermeldungen
* Korrigieren der Dokumentation für validator.showErrors().
* Korrigieren der Fehlerbezeichnungserstellung: immer html() anstelle von text() verwenden, ermöglicht das Übergeben von beliebigem HTML als Meldungen.
* Korrigieren der Fehlerbezeichnungserstellung für die Verwendung der angegebenen Fehlerklasse.
* Hinzufügen eines abhängigkeitsfeatures: Das requires-Methode nimmt String (jQuery-Ausdrücke) und Funktionen sowohl als Argument
* Stark verbesserte Anpassung der Fehlermeldungsanzeige: Verwenden Sie normale Nachrichten und ein-/ausblenden eines weiteren Containers; Ersetzen Sie die Meldungsanzeige vollständig durch einen eigenen Mechanismus (während Sie für die Delegierung an den Standardhandler möglich Anpassen der Platzierung der generierten Bezeichnungen (anstelle des Standardwerts below-Element)
* Korrigieren zweier wichtiger Fehler in IE (Fehlercontainer) und Opera (Metadaten).
* Überprüfungsmethoden wurden modifiziert, sodass sie nun leere Felder als gültig annehmen (Ausnahme: die Methoden „required“ und „equalTo“).
* Umbenennen von „min“ in „minLength“, „max“ in „maxLength“, „length“ in „rangeLength“.
* Hinzufügten von „minValue“, „maxValue“ und „rangeValue“.
* Optimierte API für die Unterstützung von verschiedenen Ereignissen. Die Standardeinstellung (submit) kann deaktiviert werden. Wenn ein Ereignis angegeben wird, wird dieses auf jedes Element (anstatt auf das gesamte Formular) angewendet. Das Kombinieren von keyup-validation mit submit-validation kann jetzt ausgesprochen einfach eingerichtet werden.
* Hinzufügen von Unterstützung für eine Meldung pro Regel (one-message-per-rule) beim Definieren von Meldungen über die Plug-In-Einstellungen.
* Hinzufügen von Unterstützung, um ein übergeordnetes Element als Wrapper für Metadaten zu verwenden. Sinnvoll, wenn Metadaten auch für andere Plug-Ins verwendet werden.
* Umgestaltete Tests und Demos: Weniger Dateien bessere demos
* Verbesserte Dokumentation: Weitere Beispiele für Methoden, Referenzmaterial mehr einige Grundlagen
