---
uid: web-forms/overview/ajax-control-toolkit/modalpopup/handling-postbacks-from-a-modalpopup-cs
title: Verarbeiten von Postbacks über ein ModalPopupC#-Attribut () | Microsoft-Dokumentation
author: wenz
description: Das ModalPopup-Steuerelement im AJAX Control Toolkit bietet eine einfache Möglichkeit zum Erstellen eines modalen Popups mithilfe Client seitiger Mittel. Es muss besonders darauf geachtet werden, wenn ein POS...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 7963890b-4ea3-4a1c-b65d-6098a3d56f62
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/modalpopup/handling-postbacks-from-a-modalpopup-cs
msc.type: authoredcontent
ms.openlocfilehash: 20073d156b4bd5ce67a47d2511b28594b70ce260
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78446205"
---
# <a name="handling-postbacks-from-a-modalpopup-c"></a>Verarbeiten von Postbacks über ein ModalPopup-Steuerelement (C#)

von [Christian Wenz](https://github.com/wenz)

[Code herunterladen](https://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup3.cs.zip) oder [PDF herunterladen](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup3CS.pdf)

> Das ModalPopup-Steuerelement im AJAX Control Toolkit bietet eine einfache Möglichkeit zum Erstellen eines modalen Popups mithilfe Client seitiger Mittel. Es muss besonders darauf geachtet werden, wenn ein Postback aus dem Popup erstellt wird.

## <a name="overview"></a>Übersicht

Das ModalPopup-Steuerelement im AJAX Control Toolkit bietet eine einfache Möglichkeit zum Erstellen eines modalen Popups mithilfe Client seitiger Mittel. Es muss besonders darauf geachtet werden, wenn ein Postback aus dem Popup erstellt wird.

## <a name="steps"></a>Schritte

Um die Funktionalität von ASP.NET AJAX und dem Steuerelement-Toolkit zu aktivieren, muss das `ScriptManager` Steuerelement an einer beliebigen Stelle auf der Seite platziert werden (innerhalb des `<form>` Elements):

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-cs/samples/sample1.aspx)]

Fügen Sie als nächstes ein Panel hinzu, das als modales Popup fungiert. Dort kann der Benutzer einen Namen und eine e-Mail-Adresse eingeben. Eine Schaltfläche wird verwendet, um das Popup Fenster zu schließen und die Informationen zu speichern. Beachten Sie, dass das `OnClick`-Attribut festgelegt ist, sodass ein Postback auftritt, wenn auf diese Schaltfläche geklickt wird:

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-cs/samples/sample2.aspx)]

Die Seite selbst besteht aus zwei Bezeichnungen für genau dieselben Informationen: Name und e-Mail-Adresse. Eine Schaltfläche wird verwendet, um das modale Popup zu starten:

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-cs/samples/sample3.aspx)]

Fügen Sie das `ModalPopupExtender`-Steuerelement hinzu, damit das Popup angezeigt wird. Legen Sie das `PopupControlID`-Attribut auf die ID des Panels fest, und `TargetControlID` auf die ID der Schaltfläche:

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-cs/samples/sample4.aspx)]

Wenn nun auf die Schaltfläche "`Save`" innerhalb des modalen Popups geklickt wird, wird die serverseitige `SaveData()` Methode ausgeführt. Dort können Sie die eingegebenen Daten in einem Datenspeicher speichern. Der Einfachheit halber werden die neuen Daten nur in der Bezeichnung ausgegeben:

[!code-csharp[Main](handling-postbacks-from-a-modalpopup-cs/samples/sample5.cs)]

Außerdem sollten die Textfeld-Steuerelemente im modalen Popup mit dem aktuellen Namen und der aktuellen e-Mail-Adresse ausgefüllt werden. Dies ist jedoch nur erforderlich, wenn kein Postback stattfindet. Wenn ein Postback vorliegt, werden die Textfelder von der ASP.NET ViewState-Funktion automatisch mit den entsprechenden Werten aufgefüllt.

[!code-csharp[Main](handling-postbacks-from-a-modalpopup-cs/samples/sample6.cs)]

[![das modale Popup ein Postback verursacht.](handling-postbacks-from-a-modalpopup-cs/_static/image2.png)](handling-postbacks-from-a-modalpopup-cs/_static/image1.png)

Das modale Popup bewirkt ein Postback ([Klicken Sie, um das Bild in voller Größe anzuzeigen](handling-postbacks-from-a-modalpopup-cs/_static/image3.png)).

> [!div class="step-by-step"]
> [Zurück](using-modalpopup-with-a-repeater-control-cs.md)
> [Weiter](positioning-a-modalpopup-cs.md)
