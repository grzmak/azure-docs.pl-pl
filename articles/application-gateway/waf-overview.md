---
title: Wprowadzenie do zapory aplikacji sieci web (WAF) w usłudze Azure Application Gateway
description: Ten artykuł zawiera omówienie zapory aplikacji sieci web (WAF) w usłudze Application Gateway
services: application-gateway
author: amsriva
ms.service: application-gateway
ms.date: 10/11/2018
ms.author: amsriva
ms.openlocfilehash: 10a67eab142287cf9303e54005b6b167e9890df0
ms.sourcegitcommit: 7b0778a1488e8fd70ee57e55bde783a69521c912
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/10/2018
ms.locfileid: "49068455"
---
# <a name="web-application-firewall-waf"></a>Zapora aplikacji internetowej

Zapora aplikacji internetowej (WAF) to funkcja usługi Application Gateway, która zapewnia scentralizowaną ochronę aplikacji internetowych przed typowymi programami wykorzystującymi luki i lukami w zabezpieczeniach. 

Aplikacje internetowe coraz częściej stają się obiektami złośliwych ataków wykorzystujących znane luki w zabezpieczeniach. Wśród nich często zdarzają się np. ataki polegające na iniekcji SQL i ataki z użyciem skryptów wykorzystywanych w wielu witrynach. Zapobieganie takim atakom z poziomu kodu aplikacji może być trudne. Może też wymagać rygorystycznego przestrzegania harmonogramu konserwacji, poprawek i monitorowania na poziomie wielu warstw topologii aplikacji. Scentralizowana zapora aplikacji internetowej ułatwia zarządzanie zabezpieczeniami oraz zapewnia lepszą ochronę administratorów aplikacji przed zagrożeniami i intruzami. Zapora aplikacji internetowej może reagować na zagrożenia bezpieczeństwa szybciej — poprzez wdrażanie poprawek zapobiegających wykorzystaniu znanych luk w zabezpieczeniach w centralnej lokalizacji zamiast w poszczególnych aplikacjach internetowych. Istniejące bramy Application Gateway można łatwo przekonwertować na bramę Application Gateway obsługującą zaporę aplikacji internetowej.

Zapora aplikacji sieci Web na podstawie reguł z [podstawowych zestawów reguł OWASP](https://www.owasp.org/index.php/Category:OWASP_ModSecurity_Core_Rule_Set_Project) wersji 3.0 lub 2.2.9. Automatycznie aktualizuje obejmujący ochronę przed nowymi zagrożeniami dzięki nie wykonywania dodatkowych czynności konfiguracyjnych.

![imageURLroute](./media/waf-overview/WAF1.png)

Usługa Application Gateway działa jak kontroler dostarczania aplikacji (ADC) i oferuje możliwość kończenia żądań SSL, koligacja sesji na podstawie plików cookie, dystrybucji obciążenia z działaniem okrężnym, routingu opartego na zawartości, możliwość hostowania wielu witryn sieci Web i stosowania rozszerzonych zabezpieczeń. Rozszerzenia zabezpieczeń oferowane przez usługę Application Gateway obejmują zarządzanie zasadami protokołu SSL i pełne wsparcie w zakresie protokołu SSL. Zabezpieczenia aplikacji są teraz silniejsze dzięki bezpośredniej integracji zapory aplikacji internetowej z ofertą kontrolera ADC. Dzięki temu możliwa jest łatwa konfiguracja w centralnej lokalizacji. Dodatkowo wprowadzenie tej nowej funkcji sprawia, że zarządzanie aplikacjami internetowymi i ich ochrona przed znanymi lukami są niezwykle proste.

## <a name="benefits"></a>Korzyści

Poniżej przedstawiono główne korzyści wynikające ze stosowania usługi Application Gateway i zapory aplikacji internetowej:

### <a name="protection"></a>Ochrona

* Ochrona aplikacji internetowej przed atakami wykorzystującymi luki w zabezpieczeniach oraz atakami niewymagającymi modyfikacji kodu zaplecza.

* Jednoczesna ochrona wielu aplikacji internetowych za bramą aplikacji. Brama Application Gateway umożliwia hostowanie maksymalnie 20 witryn sieci Web. Wszystkie witryny za bramą są chronione przed atakami z sieci Web przy użyciu zapory aplikacji sieci Web.

### <a name="monitoring"></a>Monitorowanie

* Monitoruj aplikację internetową pod kątem ataków przy użyciu dziennika zapory aplikacji internetowej w czasie rzeczywistym. Ten dziennik jest zintegrowany z usługą [Azure Monitor](../monitoring-and-diagnostics/monitoring-overview.md) w celu śledzenia alertów i dzienników zapory aplikacji sieci Web oraz łatwego monitorowania trendów.

* Zapora aplikacji sieci Web zostanie wkrótce zintegrowana z usługą Azure Security Center. Usługa Azure Security Center daje pełny widok stanu bezpieczeństwa wszystkich Twoich zasobów platformy Azure.

### <a name="customization"></a>Dostosowywanie

* Możliwość dostosowywania reguł i grup reguł zapory aplikacji sieci Web do własnych wymagań dotyczących aplikacji i w celu wyeliminowania wyników fałszywie dodatnich.

## <a name="features"></a>Funkcje

- Ochrona przed atakami polegającymi na iniekcji SQL
- Ochrona przed atakami z użyciem skryptów wykorzystywanych w obrębie wielu witryn
- Częste ataki w ramach sieci Web polegające na iniekcji poleceń, przemycaniu żądań HTTP, rozdzielaniu odpowiedzi HTTP i zdalnym dołączaniu plików
- Ochrona przed naruszeniami protokołu HTTP
- Ochrona przed nieprawidłowościami protokołu HTTP, takimi jak brakujące powiązania agenta i użytkownika hosta oraz akceptowanie nagłówków
- Zapobieganie atakom z użyciem robotów, przeszukiwarek i skanerów
- Wykrywanie typowych błędów konfiguracji aplikacji (czyli Apache, usługi IIS itp.)

### <a name="public-preview-features"></a>Funkcje w publicznej wersji zapoznawczej

Bieżący publiczny zapory aplikacji sieci Web w wersji zapoznawczej jednostki SKU incudes następujące funkcje:

- **Limity rozmiaru żądań** — Zapora aplikacji sieci Web pozwala użytkownikom na Konfigurowanie ograniczeń rozmiar żądania w ramach dolną i górną granicę.
- **Listy wykluczeń** — Zapora aplikacji sieci Web listy wykluczeń Zezwalaj użytkownikom na pominięcie niektórych atrybutów żądania oceny zapory aplikacji sieci Web. Typowym przykładem jest, że usługi Active Directory włożony tokenów, które są używane do uwierzytelniania lub pola hasła.

Aby uzyskać więcej informacji na temat publicznej wersji zapoznawczej zapory aplikacji sieci Web, zobacz [internetowych limity rozmiaru żądanie zapory aplikacji oraz listy wykluczeń (publiczna wersja zapoznawcza)](application-gateway-waf-configuration.md).





### <a name="core-rule-sets"></a>Podstawowe zestawy reguł

Usługa Application Gateway obsługuje dwa zestawy reguł: CRS 3.0 i CRS 2.2.9. Są to kolekcje reguł, które chronią aplikacje internetowe przed złośliwymi działaniami.

Zapora aplikacji internetowej jest domyślnie wstępnie skonfigurowana przy użyciu zestawu CRS 3.0, ale możesz używać wersji 2.2.9. Zestaw CRS 3.0 oferuje mniejszą liczbę wyników fałszywie dodatnich niż wersja 2.2.9. Umożliwiono [dostosowywanie reguł do określonych wymagań](application-gateway-customize-waf-rules-portal.md). Oto niektóre typowe luki w zabezpieczeniach w Internecie, przed którymi chroni zapora aplikacji internetowej:

- Ochrona przed atakami polegającymi na iniekcji SQL
- Ochrona przed atakami z użyciem skryptów wykorzystywanych w obrębie wielu witryn
- Częste ataki w ramach sieci Web polegające na iniekcji poleceń, przemycaniu żądań HTTP, rozdzielaniu odpowiedzi HTTP i zdalnym dołączaniu plików
- Ochrona przed naruszeniami protokołu HTTP
- Ochrona przed nieprawidłowościami protokołu HTTP, takimi jak brakujące powiązania agenta i użytkownika hosta oraz akceptowanie nagłówków
- Zapobieganie atakom z użyciem robotów, przeszukiwarek i skanerów
- Wykrywanie typowych błędów konfiguracji aplikacji (np. Apache, usługi IIS itp.)

Bardziej szczegółowe listę reguł i ich ochrony w artykule [podstawowych zestawów reguł](#core-rule-sets).


#### <a name="owasp30"></a>OWASP_3.0

Udostępniony podstawowy zestaw reguł w wersji 3.0 zawiera 13 grup reguł, jak pokazano w poniższej tabeli. Każda z tych grup reguł zawiera wiele reguł, które można wyłączyć.

|Grupa reguł|Opis|
|---|---|
|**[REQUEST-911-METHOD-ENFORCEMENT](application-gateway-crs-rulegroups-rules.md#crs911)**|Zawiera reguły blokowania metod (PUT, PATCH).|
|**[REQUEST-913-SCANNER-DETECTION](application-gateway-crs-rulegroups-rules.md#crs913)**| Zawiera reguły ochrony przed skanerami portów i środowiska.|
|**[REQUEST-920-PROTOCOL-ENFORCEMENT](application-gateway-crs-rulegroups-rules.md#crs920)**|Zawiera reguły ochrony przed problemami z protokołami i kodowaniem.|
|**[REQUEST-921-PROTOCOL-ATTACK](application-gateway-crs-rulegroups-rules.md#crs921)**|Zawiera reguły ochrony przed iniekcjami nagłówków, przemycaniem żądań i rozdzielaniem odpowiedzi|
|**[REQUEST-930-APPLICATION-ATTACK-LFI](application-gateway-crs-rulegroups-rules.md#crs930)**|Zawiera reguły ochrony przed atakami na pliki i ścieżki.|
|**[REQUEST-931-APPLICATION-ATTACK-RFI](application-gateway-crs-rulegroups-rules.md#crs931)**|Zawiera reguły ochrony przed zdalnym dołączaniem plików (RFI)|
|**[REQUEST-932-APPLICATION-ATTACK-RCE](application-gateway-crs-rulegroups-rules.md#crs932)**|Zawiera reguły ochrony przed zdalnym wykonywaniem kodu.|
|**[REQUEST-933-APPLICATION-ATTACK-PHP](application-gateway-crs-rulegroups-rules.md#crs933)**|Zawiera reguły ochrony przed atakami polegającymi na iniekcji PHP.|
|**[REQUEST-941-APPLICATION-ATTACK-XSS](application-gateway-crs-rulegroups-rules.md#crs941)**|Zawiera reguły ochrony przed atakami z użyciem skryptów w obrębie wielu witryn.|
|**[REQUEST-942-APPLICATION-ATTACK-SQLI](application-gateway-crs-rulegroups-rules.md#crs942)**|Zawiera reguły ochrony przed atakami polegającymi na iniekcji SQL.|
|**[REQUEST-943-APPLICATION-ATTACK-SESSION-FIXATION](application-gateway-crs-rulegroups-rules.md#crs943)**|Zawiera reguły ochrony przed atakami z użyciem spreparowanych stałych identyfikatorów sesji.|

#### <a name="owasp229"></a>OWASP_2.2.9

Udostępniony podstawowy zestaw reguł w wersji 2.2.9 zawiera 10 grup reguł, jak pokazano w poniższej tabeli. Każda z tych grup reguł zawiera wiele reguł, które można wyłączyć.

|Grupa reguł|Opis|
|---|---|
|**[crs_20_protocol_violations](application-gateway-crs-rulegroups-rules.md#crs20)**|Zawiera reguły ochrony przed naruszeniami protokołu (nieprawidłowe znaki, metoda GET z treścią żądania itp.)|
|**[crs_21_protocol_anomalies](application-gateway-crs-rulegroups-rules.md#crs21)**|Zawiera reguły ochrony przed nieprawidłowymi informacjami nagłówka.|
|**[crs_23_request_limits](application-gateway-crs-rulegroups-rules.md#crs23)**|Zawiera reguły ochrony przed argumentami lub plikami przekraczającymi ograniczenia.|
|**[crs_30_http_policy](application-gateway-crs-rulegroups-rules.md#crs30)**|Zawiera reguły ochrony przed ograniczonymi metodami, nagłówkami i typami plików. |
|**[crs_35_bad_robots](application-gateway-crs-rulegroups-rules.md#crs35)**|Zawiera reguły ochrony przed przeszukiwarkami i skanerami sieci Web.|
|**[crs_40_generic_attacks](application-gateway-crs-rulegroups-rules.md#crs40)**|Zawiera reguły ochrony przed atakami ogólnymi (używaniem spreparowanych stałych identyfikatorów sesji, zdalnym dołączaniu plików, iniekcją PHP itp.)|
|**[crs_41_sql_injection_attacks](application-gateway-crs-rulegroups-rules.md#crs41sql)**|Zawiera reguły ochrony przed atakami polegającymi na iniekcji SQL|
|**[crs_41_xss_attacks](application-gateway-crs-rulegroups-rules.md#crs41xss)**|Zawiera reguły ochrony przed atakami z użyciem skryptów w obrębie wielu witryn.|
|**[crs_42_tight_security](application-gateway-crs-rulegroups-rules.md#crs42)**|Zawiera reguły ochrony przed atakami polegającymi na przechodzeniu przez ścieżki|
|**[crs_45_trojans](application-gateway-crs-rulegroups-rules.md#crs45)**|Zawiera reguły ochrony przed końmi trojańskimi otwierającymi tylne wejście.|

### <a name="waf-modes"></a>Tryby zapory aplikacji sieci Web

Zapora aplikacji sieci Web bramy aplikacji może zostać skonfigurowana pod kątem jej uruchamiania w następujących dwóch trybach:

* **Tryb wykrywania** — po skonfigurowaniu do pracy w trybie wykrywania ZAPORA monitoruje i loguje się wszystkie alerty zagrożenia w pliku dziennika. Rejestrowanie danych diagnostycznych usługi Application Gateway musi być włączone w sekcji **Diagnostyka**. Należy również upewnić się, że opcja dziennika zapory aplikacji sieci Web jest zaznaczona i włączona. Podczas pracy w trybie wykrywania zapora aplikacji internetowej nie blokuje żądań przychodzących.
* **Tryb zapobiegania** — kiedy zapora jest skonfigurowana do działania w tym trybie, usługa Application Gateway aktywnie blokuje próby włamań i ataki wykryte na podstawie reguł. Osoba atakująca odbiera wyjątek 403 odnoszący się do nieautoryzowanego dostępu, a jej połączenie zostaje zakończone. W trybie zapobiegania tego rodzaju ataki są rejestrowane w dziennikach zapory aplikacji sieci Web w sposób ciągły.

### <a name="application-gateway-waf-reports"></a>Monitorowanie zapory aplikacji sieci Web

Monitorowanie kondycji bramy Application Gateway jest ważne. Monitorowanie kondycji zapory aplikacji internetowej i chronionych przez nią aplikacji jest możliwe dzięki rejestrowaniu i integracji z usługami Azure Monitor, Azure Security Center (wkrótce) oraz Log Analytics.

![Diagnostyka](./media/waf-overview/diagnostics.png)

#### <a name="azure-monitor"></a>Azure Monitor

Każdy dziennik bramy Application Gateway jest zintegrowany z usługą [Azure Monitor](../monitoring-and-diagnostics/monitoring-overview.md).  Dzięki temu można śledzić informacje diagnostyczne, w tym alerty i dzienniki zapory aplikacji sieci Web.  Ta funkcja jest dostępna w ramach zasobu usługi Application Gateway w portalu na karcie **Diagnostyka** lub bezpośrednio w usłudze Azure Monitor. Aby dowiedzieć się więcej na temat włączania dzienników diagnostycznych usługi application gateway, zobacz [diagnostyki usługi Application Gateway](application-gateway-diagnostics.md)

#### <a name="azure-security-center"></a>Azure Security Center

Usługa [Azure Security Center](../security-center/security-center-intro.md) ułatwia zapobieganie zagrożeniom, ich wykrywanie i reagowanie na nie, a przy tym zapewnia lepszy wgląd i większą kontrolę w zakresie bezpieczeństwa zasobów na platformie Azure. Brama aplikacji jest teraz [zintegrowana z usługą Azure Security Center](application-gateway-integration-security-center.md). Usługa Azure Security Center skanuje środowisko w celu wykrycia niechronionych aplikacji internetowych. Usługa ta może udostępnić zalecenie dotyczące zapory aplikacji internetowej bramy aplikacji służące do ochrony tych narażonych na ataki zasobów. Zapory aplikacji internetowej bramy aplikacji można tworzyć bezpośrednio z poziomu usługi Azure Security Center.  Te wystąpienia zapory aplikacji internetowej są zintegrowane z usługą Azure Security Center i za ich pomocą będą wysyłane alerty i informacje o kondycji do usługi Azure Security Center na potrzeby raportowania.

![rysunek 1](./media/waf-overview/figure1.png)

#### <a name="logging"></a>Rejestrowanie

Zapora aplikacji sieci Web w usłudze Application Gateway dostarcza szczegółowe raporty w zakresie każdego wykrytego zagrożenia. Rejestrowanie jest zintegrowane z dziennikami diagnostycznymi platformy Azure, a alerty są zapisywane w formacie json. Te dzienniki można zintegrować z usługą [Log Analytics](../log-analytics/log-analytics-azure-networking-analytics.md).

![imageURLroute](./media/waf-overview/waf2.png)

```json
{
  "resourceId": "/SUBSCRIPTIONS/{subscriptionId}/RESOURCEGROUPS/{resourceGroupId}/PROVIDERS/MICROSOFT.NETWORK/APPLICATIONGATEWAYS/{appGatewayName}",
  "operationName": "ApplicationGatewayFirewall",
  "time": "2017-03-20T15:52:09.1494499Z",
  "category": "ApplicationGatewayFirewallLog",
  "properties": {
    "instanceId": "ApplicationGatewayRole_IN_0",
    "clientIp": "104.210.252.3",
    "clientPort": "4835",
    "requestUri": "/?a=%3Cscript%3Ealert(%22Hello%22);%3C/script%3E",
    "ruleSetType": "OWASP",
    "ruleSetVersion": "3.0",
    "ruleId": "941320",
    "message": "Possible XSS Attack Detected - HTML Tag Handler",
    "action": "Blocked",
    "site": "Global",
    "details": {
      "message": "Warning. Pattern match \"<(a|abbr|acronym|address|applet|area|audioscope|b|base|basefront|bdo|bgsound|big|blackface|blink|blockquote|body|bq|br|button|caption|center|cite|code|col|colgroup|comment|dd|del|dfn|dir|div|dl|dt|em|embed|fieldset|fn|font|form|frame|frameset|h1|head|h ...\" at ARGS:a.",
      "data": "Matched Data: <script> found within ARGS:a: <script>alert(\\x22hello\\x22);</script>",
      "file": "rules/REQUEST-941-APPLICATION-ATTACK-XSS.conf",
      "line": "865"
    }
  }
} 

```

## <a name="application-gateway-waf-sku-pricing"></a>Cena jednostki SKU zapory aplikacji sieci Web w usłudze Application Gateway

Zapora aplikacji internetowej jest dostępna w ramach nowej jednostki SKU zapory aplikacji internetowej. Ta jednostka SKU jest dostępna tylko w modelu aprowizacji usługi Azure Resource Manager, a nie w klasycznym modelu wdrażania. Ponadto jednostka SKU zapory aplikacji sieci Web jest oferowana tylko w średnich i dużych rozmiarach wystąpień usługi Application Gateway. Wszystkie limity dotyczące usługi Application Gateway dotyczą również jednostki SKU zapory aplikacji sieci Web. Ceny zależą od opłaty godzinnej za wystąpienie bramy i opłaty za przetwarzanie danych. Opłata godzinna za bramę w przypadku jednostki SKU zapory aplikacji sieci Web różni się od opłat za standardową jednostkę SKU. Ceny można znaleźć na stronie [szczegółowego cennika usługi Application Gateway](https://azure.microsoft.com/pricing/details/application-gateway/). Opłaty za przetwarzanie danych pozostają bez zmian. Nie ma opłat za regułę lub grupę reguł. Możesz chronić wiele aplikacji internetowych za tą samą zaporą aplikacji internetowej i nie ma dodatkowych opłat za obsługę wielu aplikacji. 

## <a name="next-steps"></a>Kolejne kroki

Po zapoznaniu się zapory aplikacji sieci Web, zobacz [jak skonfigurować zaporę aplikacji sieci web w usłudze Application Gateway](tutorial-restrict-web-traffic-powershell.md).

