---
title: Skriv om HTTP-huvuden i Azure Application Gateway | Microsoft Docs
description: Den här artikeln innehåller en översikt över funktionen för att skriva om HTTP-huvuden i Azure Application Gateway
services: application-gateway
author: abshamsft
ms.service: application-gateway
ms.topic: article
ms.date: 12/20/2018
ms.author: absha
ms.openlocfilehash: 2babb6ff7b93ad9cf7c93565cadce9453a3b96ca
ms.sourcegitcommit: eecd816953c55df1671ffcf716cf975ba1b12e6b
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 01/28/2019
ms.locfileid: "55103436"
---
# <a name="rewrite-http-headers-with-application-gateway-public-preview"></a>Skriv om HTTP-huvuden med Application Gateway (offentlig förhandsversion)

Med HTTP-huvuden kan klienten och servern skicka ytterligare information med begäran eller svaret. Skriva om dessa HTTP-huvuden hjälper till att du utföra flera viktiga scenarier, till exempel att lägga till säkerhetsrelaterade rubrik fält som HSTS / X XSS skydd eller ta bort svar huvudfält, som kan röja känslig information som backend-servernamn.

Application Gateway stöder nu möjligheten att skriva om rubrikerna för inkommande HTTP-begäran samt utgående HTTP-svar. Du kommer att kunna lägga till, ta bort eller uppdatera HTTP-begäran och svarshuvuden medan begäran/svar-paketen flytta mellan klienten och backend-adresspooler. Du kan skriva om båda standard som inte är standard huvudfält.

> [!NOTE] 
>
> HTTP-huvud omskrivning-stöd är endast tillgänglig för den [nya SKU: N [Standard_V2\]](https://docs.microsoft.com/azure/application-gateway/application-gateway-autoscaling-zone-redundant)

Stöd för Application Gateway-huvud omskrivning erbjuder:

- **Global rubrik omskrivning**: Du kan skriva om specifika rubriker för alla begäranden och svar som hör till platsen.
- **Sökvägsbaserad rubrik omskrivning**: den här typen av omarbetning gör det möjligt för rubriken omarbetning för dessa begäranden och svar som hör till endast på en viss plats, till exempel ett i kundvagnen område enligt/kundvagn / *.

Med den här ändringen måste du:

1. Skapa nya objekt som krävs för att skriva om http-huvuden: 
   - **RequestHeaderConfiguration**: det här objektet används för att ange fält i begärans-huvud som du avser att skriva om och det nya värdet som de ursprungliga sidhuvuden behöva skrivas till.
   - **ResponseHeaderConfiguration**: det här objektet används för att ange huvudfält för svar som du avser att skriva om och det nya värdet som de ursprungliga sidhuvuden behöva skrivas till.
   - **ActionSet**: det här objektet innehåller konfigurationerna för de begärande- och svarshuvuden som anges ovan. 
   - **RewriteRule**: det här objektet innehåller alla de *actionSets* angetts ovan. 
   - **RewriteRuleSet**– det här objektet innehåller alla de *rewriteRules* och måste vara kopplad till en begäran routningsregel - grundläggande eller -baserad.
2. Du kommer sedan att behöva koppla omskrivning regeluppsättningen med en regel för vidarebefordran. När du skapat är den här Skriv om konfigurationen kopplat till käll-lyssnaren via en routningsregel för. När du använder en grundläggande routningsregel rubrik Skriv om konfigurationen är associerad med en käll-lyssnare och är en global huvud-omskrivning. När en sökvägsbaserad regel används har rubrik Skriv om konfigurationen definierats för Webbadress för sökvägskarta. Därför gäller detta bara till området angiven sökväg för en plats.

Du kan skapa flera http-huvud omskrivning regeluppsättningar och varje regeluppsättning omskrivning kan tillämpas på flera lyssnare. Du kan dock använda endast en http-omskrivningsregel inställd på en viss lyssnare.

Du kan skriva om värdet i rubriker för att:

- Textvärdet. 

  *Exempel:* 

  ```azurepowershell-interactive
  $responseHeaderConfiguration = New-AzureRmApplicationGatewayRewriteRuleHeaderConfiguration -HeaderName "Strict-Transport-Security" -  HeaderValue "max-age=31536000")
  ```

- Värde från en annan rubrik. 

  *Exempel 1:* 

  ```azurepowershell-interactive
  $requestHeaderConfiguration= New-AzureRmApplicationGatewayRewriteRuleHeaderConfiguration -HeaderName "X-New-RequestHeader" -HeaderValue {http_req_oldHeader}
  ```

  > [!Note] 
  > För att ange en rubrik för begäran, måste du använda syntaxen: {http_req_headerName}

  *Exempel 2*:

  ```azurepowershell-interactive
  $responseHeaderConfiguration= New-AzureRmApplicationGatewayRewriteRuleHeaderConfiguration -HeaderName "X-New-ResponseHeader" -HeaderValue {http_resp_oldHeader}
  ```

  > [!Note] 
  > För att ange ett svarshuvud, måste du använda syntaxen: {http_resp_headerName}

- Värde från stöds servervariabler.

  *Exempel:* 

  ```azurepowershell-interactive
  $requestHeaderConfiguration = New-AzureRmApplicationGatewayRewriteRuleHeaderConfiguration -HeaderName "Ciphers-Used" -HeaderValue "{var_ciphers_used}"
  ```

  > [!Note] 
  > För att ange en servervariabel, måste du använda syntaxen: {var_serverVariable}

- En kombination av ovanstående.

## <a name="server-variables"></a>Servervariabler

Servervariabler lagra användbar information på en webbserver. Dessa variabler innehåller information om servern, anslutningen med klienten och den aktuella begäran i anslutningen, till exempel klientens IP-adress eller web webbläsartyp. De ändras dynamiskt, t.ex. när en ny sida läses eller en form publiceras.  Med dessa variabler-användare kan ange begärandehuvuden samt svarshuvuden. 

Den här funktionen stöder skriva om rubriker till följande servervariabler:

| Stöds servervariabler | Beskrivning                                                  |
| -------------------------- | :----------------------------------------------------------- |
| ciphers_supported          | Returnerar listan över chiffer som stöds av klienten          |
| ciphers_used               | Returnerar en sträng med chiffer som används för en etablerad SSL-anslutning |
| client_port                | klientport                                                  |
| client_tcp_rtt             | information om klienten TCP-anslutning. tillgängligt på system som stöder socketalternativet TCP_INFO |
| client_user                | När du använder HTTP-autentisering, användarnamnet som angetts för autentisering |
| värd                       | i den här rangordning: värdnamn från begäran linje- eller värdnamnet från fältet ”värd” begäran rubrik eller ett servernamn som matchar en begäran |
| http_method                | den metod som används för att utföra URL-begäran. Till exempel få POST osv. |
| http_status                | Sessionsstatus, t.ex.: 200, 400, 403 osv.                       |
| http_version               | begäran-protokollet, vanligtvis ”HTTP/1.0”, ”HTTP/1.1” eller ”HTTP/2.0” |
| query_string               | listan över variabeln / värde-par som följer den ””? i den begärda URL: en. |
| received_bytes             | begärandelängd (inklusive raden för kravet, rubrik och begärandetexten) |
| request_query              | argument i raden för kravet                                |
| request_scheme             | begäran-schema, ”http” eller ”https”                            |
| request_uri                | fullständiga ursprungliga begärande-URI (med argument)                   |
| sent_bytes                 | antalet byte som skickats till en klient                             |
| server_port                | port på den server som godkänt en begäran                 |
| ssl_connection_protocol    | Returnerar protokollet för en etablerad SSL-anslutning        |
| ssl_enabled                | ”on” om fungerar anslutningen i SSL-läge eller en tom sträng som på annat sätt |

## <a name="limitations"></a>Begränsningar

- Den här funktionen att skriva om HTTP-huvuden är för närvarande bara tillgänglig via Azure PowerShell, Azure API och Azure SDK. Support via portalen och Azure CLI är tillgänglig snart.

- HTTP-huvudet omskrivning stöd stöds bara på den nya SKU: N [Standard_V2](https://docs.microsoft.com/azure/application-gateway/application-gateway-autoscaling-zone-redundant). Funktionen stöds inte på den gamla SKU: N.

- Skriva om Connect, uppgradering och värd-rubriker stöds inte ännu.

- Två viktiga servervariabler, client_ip (IP-adressen för klienten som gör begäran) och cookie_*namn* (den *namn* cookie), stöds inte ännu. Servervariabeln client_ip är särskilt användbart i scenarier där kunderna vill skriva om rubriken x-vidarebefordrade-för som Application Gateway, så att huvudet innehåller endast IP-adressen för klienten och inte portinformationen.

  Båda dessa servervariabler kommer snart att stödjas.

- Möjlighet att skriva villkorligt om http-huvuden är tillgänglig snart.

- Rubriknamn får innehålla alla alfanumeriska tecken och specifika symboler som definierats i [RFC 7230](https://tools.ietf.org/html/rfc7230#page-27). Vi för närvarande stöder dock inte ”understreck” (\_) specialtecken i namnet. 

## <a name="need-help"></a>Behöver du hjälp?

Kontakta oss på [ AGHeaderRewriteHelp@microsoft.com ](mailto:AGHeaderRewriteHelp@microsoft.com) om du behöver hjälp med den här funktionen.

## <a name="next-steps"></a>Nästa steg

När du läst om möjligheten att skriva om HTTP-huvuden går du till [skapa en automatisk skalning och zonredundant application gateway som skriver om HTTP-huvuden](tutorial-http-header-rewrite-powershell.md) eller [skriva om HTTP-huvuden i befintliga automatisk skalning och zonredundant Programgateway](add-http-header-rewrite-rule-powershell.md)
