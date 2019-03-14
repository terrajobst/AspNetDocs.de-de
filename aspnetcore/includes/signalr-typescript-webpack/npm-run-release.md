---
ms.openlocfilehash: 939231583679d469d01724cc1a405bd45e46d2c5
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/01/2019
ms.locfileid: "57175885"
---
```console
npm run release
```

Dieser Befehl hält die clientseitigen Objekte an, die bereitgestellt werden, wenn die App ausgeführt wird. Die Objekte werden im Ordner *wwwroot* gespeichert.

Webpack hat die folgenden Aufgaben durchgeführt:

* Die Inhalte des Verzeichnis *wwwroot* wurden bereinigt.
* TypeScript wurde in JavaScript konvertiert. Dieser Prozess ist als *Transpilierung* bekannt.
* Der generierte JavaScript-Code wurde gekürzt, um die Dateigröße zu reduzieren. Dieser Prozess ist als *Minimierung* bekannt.
* Die verarbeiteten JavaScript-, CSS- und HTML-Dateien wurden aus *src* in das Verzeichnis *wwwroot* kopiert.
* Die folgenden Elemente wurden in die Datei *wwwroot/index.html* eingefügt:
    * Ein `<link>`-Tag, das auf die Datei *wwwroot/main.\<hash\>.css* verweist. Dieses Tag wird unmittelbar vor dem schließenden Tag `</head>` platziert.
    * Ein `<script>`-Tag, das auf die minimierte Datei *wwwroot/main.\<hash\>.js* verweist. Dieses Tag wird unmittelbar vor dem schließenden Tag `</body>` platziert.
