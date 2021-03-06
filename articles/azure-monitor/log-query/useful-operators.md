---
title: Användbara operatörer i Azure Log Analytics-frågor | Microsoft Docs
description: Vanliga funktioner ska användas för olika scenarier i Log Analytics-frågor.
services: log-analytics
documentationcenter: ''
author: bwren
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 08/21/2018
ms.author: bwren
ms.openlocfilehash: 060b1e469a31c335f062ccd332157d13e64f9318
ms.sourcegitcommit: 5b869779fb99d51c1c288bc7122429a3d22a0363
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 12/10/2018
ms.locfileid: "53183990"
---
# <a name="useful-operators-in-log-analytics-queries"></a>Användbara operatörer i Log Analytics-frågor

Tabellen nedan innehåller några vanliga funktioner ska användas för olika scenarier i Log Analytics-frågor.

## <a name="useful-operators"></a>Användbara operatorer

Kategori                                |Relevanta Analytics-funktion
----------------------------------------|----------------------------------------
Val av och kolumnen alias            |`project`, `project-away`, `extend`
Temporära tabeller och konstanter          |`let scalar_alias_name = …;` <br> `let table_alias_name =  …  …  … ;`| 
Jämförelse och Strängoperatorerna         |`startswith`, `!startswith`, `has`, `!has` <br> `contains`, `!contains`, `containscs` <br> `hasprefix`, `!hasprefix`, `hassuffix`, `!hassuffix`, `in`, `!in` <br> `matches regex` <br> `==`, `=~`, `!=`, `!~`
Vanliga strängfunktioner                 |`strcat()`, `replace()`, `tolower()`, `toupper()`, `substring()`, `strlen()`
Vanliga matematiska funktioner                   |`sqrt()`, `abs()` <br> `exp()`, `exp2()`, `exp10()`, `log()`, `log2()`, `log10()`, `pow()` <br> `gamma()`, `gammaln()`
Parsning av text                            |`extract()`, `extractjson()`, `parse`, `split()`
Begränsa utdata                         |`take`, `limit`, `top`, `sample`
Datumfunktioner                          |`now()`, `ago()` <br> `datetime()`, `datepart()`, `timespan` <br> `startofday()`, `startofweek()`, `startofmonth()`, `startofyear()` <br> `endofday()`, `endofweek()`, `endofmonth()`, `endofyear()` <br> `dayofweek()`, `dayofmonth()`, `dayofyear()` <br> `getmonth()`, `getyear()`, `weekofyear()`, `monthofyear()`
Gruppering och sammanställning                |`summarize by` <br> `max()`, `min()`, `count()`, `dcount()`, `avg()`, `sum()` <br> `stddev()`, `countif()`, `dcountif()`, `argmax()`, `argmin()` <br> `percentiles()`, `percentile_array()`
Kopplingar och unioner                        |`join kind=leftouter`, `inner`, `rightouter`, `fullouter`, `leftanti` <br> `union`
Sortera, ordning                             |`sort`, `order` 
Dynamisk objekt (JSON och matris)         |`parsejson()` <br> `makeset()`, `makelist()` <br> `split()`, `arraylength()` <br> `zip()`, `pack()`
Logiska operatorer                       |`and`, `or`, `iff(condition, value_t, value_f)` <br> `binary_and()`, `binary_or()`, `binary_not()`, `binary_xor()`
Maskininlärning                        |`evaluate autocluster`, `basket`, `diffpatterns`, `extractcolumns`


## <a name="next-steps"></a>Nästa steg

- Gå igenom en lektion den [skriva frågor i Log Analytics](get-started-queries.md).
