---
title: "Anslutningsbibliotek för SQL-databas | Microsoft Docs"
description: "Länkar för nedladdning av moduler vilket gör anslutningen till SQL Server och SQL-databas från ett stort antal klienten programmeringsspråk."
services: sql-database
documentationcenter: 
author: MightyPen
manager: jhubbard
editor: genemi
ms.assetid: 13d899d3-cf46-4e4d-8919-cf4b41ca836d
ms.service: sql-database
ms.custom: develop apps
ms.workload: On Demand
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: genemi
ms.openlocfilehash: bdf83fac9bd0ac6790062f802748a18045c7a171
ms.sourcegitcommit: e5355615d11d69fc8d3101ca97067b3ebb3a45ef
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/31/2017
---
# <a name="connectivity-libraries-and-frameworks-for-microsoft-sql-server"></a>Anslutningen bibliotek och ramverk för Microsoft SQL Server

Kolla in våra [komma igång-självstudierna](http://aka.ms/sqldev) att snabbt komma igång med programmeringsspråk, t.ex C#, Java, Node.js, PHP och Python och skapa en app som använder SQL Server på Linux- eller Windows- eller Docker på macOS.

Tabellen nedan visar anslutning bibliotek eller *drivrutiner* att klientprogram kan använda från olika språk för att ansluta till och använda Microsoft SQL Server körs lokalt eller i molnet, på Linux-, Windows- eller Docker och att Azure SQL Database och Azure SQL Data Warehouse. 

| Språk | Plattform | Ytterligare resurser | Ladda ned | Kom igång |
| :-- | :-- | :-- | :-- | :-- |
| C# | Windows, Linux, macOS | [Microsoft ADO.NET för SQLServer](https://docs.microsoft.com/sql/connect/ado-net/microsoft-ado-net-for-sql-server) | [Ladda ned](https://www.microsoft.com/net/download/) | [Kom igång](https://www.microsoft.com/en-us/sql-server/developer-get-started/csharp/ubuntu)
| Java | Windows, Linux, macOS | [Microsoft JDBC-drivrutin för SQL Server](http://msdn.microsoft.com/library/mt484311.aspx) | [Ladda ned](https://go.microsoft.com/fwlink/?linkid=852460) |  [Kom igång](https://www.microsoft.com/en-us/sql-server/developer-get-started/java/ubuntu)
| PHP | Windows, Linux, macOS| [PHP-SQL-drivrutin för SQLServer](http://msdn.microsoft.com/library/dn865013.aspx) | Operativsystem: <br/> \*[Windows](https://www.microsoft.com/download/details.aspx?id=20098) <br/> \*[Linux](https://github.com/Microsoft/msphpsql/tree/dev#install-unix) <br/> \*[macOS](https://github.com/Microsoft/msphpsql/tree/dev#install-unix) |  [Kom igång](https://www.microsoft.com/en-us/sql-server/developer-get-started/php/ubuntu)
| Node.js | Windows, Linux, macOS | [Node.js-drivrutin för SQLServer](http://msdn.microsoft.com/library/mt652093.aspx) | [Installera](https://msdn.microsoft.com/library/mt652094.aspx) |  [Kom igång](https://www.microsoft.com/en-us/sql-server/developer-get-started/node/ubuntu)
| Python | Windows, Linux, macOS | [Python SQL-drivrutin](http://msdn.microsoft.com/library/mt652092.aspx) | Installera alternativ: <br/> \*[pymssql](https://msdn.microsoft.com/library/mt694094.aspx) <br/> \*[pyodbc](http://msdn.microsoft.com/library/mt763257.aspx) |  [Kom igång](https://www.microsoft.com/en-us/sql-server/developer-get-started/python/ubuntu)
| Ruby | Windows, Linux, macOS | [Ruby-drivrutin för SQLServer](http://msdn.microsoft.com/library/mt691981.aspx) | [Installera](https://msdn.microsoft.com/library/mt711041.aspx) | [Kom igång](https://www.microsoft.com/en-us/sql-server/developer-get-started/ruby/ubuntu)
| C++ | Windows, Linux, macOS | [Microsoft ODBC-drivrutin för SQLServer](https://msdn.microsoft.com/en-us/library/mt654048(v=sql.1).aspx) | [Ladda ned](https://msdn.microsoft.com/en-us/library/mt654048(v=sql.1).aspx) |  

I tabellen nedan visas några exempel på ramverk för objektet relationella mappning (ORM) och web ramverk som klientprogram kan använda med Microsoft SQL Server körs lokalt eller i molnet, på Linux-, Windows- eller Docker och att Azure SQL Database och Azure SQL Data Warehouse. 

| Språk | Plattform | ORM(s) |
| :-- | :-- | :-- |
| C# | Windows, Linux, macOS | [Entity Framework](https://docs.microsoft.com/en-us/ef)<br>[Entity Framework Core](https://docs.microsoft.com/en-us/ef/core/index) |
| Java | Windows, Linux, macOS |[Viloläge ORM](http://hibernate.org/orm)|
| PHP | Windows, Linux | [Laravel (Eloquent)](https://laravel.com/docs/5.0/eloquent) |
| Node.js | Windows, Linux, macOS | [Sequelize ORM](http://docs.sequelizejs.com) |
| Python | Windows, Linux, macOS |[Django](https://www.djangoproject.com/) |
| Ruby | Windows, Linux, macOS | [Ruby spår](http://rubyonrails.org/) |

## <a name="related-links"></a>Relaterade länkar
- [SQL Server-drivrutiner](http://msdn.microsoft.com/library/mt654049.aspx) för att ansluta från klientprogram
- [Ansluta till SQL Database med .NET (C#)](sql-database-connect-query-dotnet.md)
- [Ansluta till SQL Database med PHP](sql-database-connect-query-php.md)
- [Ansluta till SQL Database med Node.js](sql-database-connect-query-nodejs.md)
- [Ansluta till SQL Database med Java](sql-database-connect-query-java.md)
- [Anslut till SQL Database med Python](sql-database-connect-query-python.md)
- [Ansluta till SQL Database med Ruby](sql-database-connect-query-ruby.md)
