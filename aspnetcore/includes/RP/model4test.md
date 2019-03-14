---
ms.openlocfilehash: 22c540058e0b3b9ae612f1b2612cecc08627ea2b
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/01/2019
ms.locfileid: "57063987"
---
<a name="test"></a>
### <a name="test-the-app"></a><span data-ttu-id="1fda4-101">Testen der App</span><span class="sxs-lookup"><span data-stu-id="1fda4-101">Test the app</span></span>

* <span data-ttu-id="1fda4-102">Führen Sie die App aus, und fügen Sie `/Movies` an die URL im Browser an (`http://localhost:port/movies`).</span><span class="sxs-lookup"><span data-stu-id="1fda4-102">Run the app and append `/Movies` to the URL in the browser (`http://localhost:port/movies`).</span></span>
* <span data-ttu-id="1fda4-103">Testen Sie den Link **Create** (Erstellen).</span><span class="sxs-lookup"><span data-stu-id="1fda4-103">Test the **Create** link.</span></span>

  ![Seite „Create“](../../tutorials/razor-pages/model/_static/conan.png)

<a name="scaffold"></a>

* <span data-ttu-id="1fda4-105">Testen Sie die Links **Edit** (Bearbeiten), **Details** und **Delete** (Löschen).</span><span class="sxs-lookup"><span data-stu-id="1fda4-105">Test the **Edit**, **Details**, and **Delete** links.</span></span>

<span data-ttu-id="1fda4-106">Wenn die folgende Fehlermeldung angezeigt wird, stellen Sie sicher, dass Migrationen ausgeführt wurden und die Datenbank aktualisiert ist:</span><span class="sxs-lookup"><span data-stu-id="1fda4-106">If you get the following error, verify you have run migrations and updated the database:</span></span>

```
An unhandled exception occurred while processing the request.
SqliteException: SQLite Error 1: 'no such table: Movie'.
Microsoft.Data.Sqlite.SqliteException.ThrowExceptionForRC(int rc, sqlite3 db)
```
