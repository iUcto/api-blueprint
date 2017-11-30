FORMAT: 1A
HOST: https://online.iucto.cz/api/1.1/

# iÚčto
<a name="top"/>

`https://online.iucto.cz/api`

iÚčto API je založena na REST principu.
API umožnuje práci s různými typy dokladů pomocí HTTP požadavků zasílaných ve formátu popsaném níže.

`Ke každému HTTP požadavku je nutné přidat HTTP hlavičku X-Auth-key`

Vešekerá komunikace probíhá pomocí protokolu HTTPS.

Odpovědi serveru jsou ve formátu [HAL+JSON](http://stateless.co/hal_specification.html).
Tělo požadavku v případě POST a PUT musí být ve formátu JSON.

[Přehled změn](https://raw.githubusercontent.com/PavelMaca/iUcto-API/master/changelog.txt)

#Povolené HTTP požadavky:
<a name="http_pozadavky"/>

- `GET` - Získání záznamu, nebo seznamu
- `POST` - Vytvoření nového záznamu
- `PUT` - Editace záznamu
- `DELETE` - Smazání záznamu

#Popis obvyklých odpovědí serveru
<a name="odpovedy_serveru"/>

- 200 `OK` - požadavek proběhl v pořádku (některé akce mohou vrátit 201).
- 201 `Created` - požadavek proběhl v pořádku a záznam byl vytvořen.
- 204 `No Content` - požadavek proběhl v pořádku, ale není nic co by se dalo vrátit (tělo odpovědi je prázdné).
- 400 `Bad Request` - neznámý formát požadavku, je možné že chybý nějaké parametry.
- 401 `Unauthorized` - špatný API klíč v požadavku.
- 403 `Forbidden` - přístup odepřen, uživatel nemá potřebná opravnění pro danou akci.
- 404 `Not Found` - záznam nenalezen.
- 405 `Method Not Allowed` - požadovaná metoda není podporována.

Odpověd může obsahovat popis chyby. Například při vkládání nového záznamu se špatnými daty se vypíší chybové zprávy validace.

## Výpis chybových hlášek

| Kód | Chyba                                         |
|:---:|-----------------------------------------------|
| 400 | Komunikace musí probíhat přes protokol HTTPS. |
| 400 | Neplatná verze API, nebo zdroj.               |
| 400 | Tělo požadavku je prázdné.                    |
| 400 | Neplatný JSON formát.                         |
| 400 | Parametr 'doctype' je povinný.                |
| 400 | Parametr 'doctype' není platný.               |
| 400 | Parametr 'date' je povinný.                   |
| 400 | Parametr 'date' není platný.                  |
| 403 | Nelze smazat záznam.                          |
| 403 | Účetní období, nebo období DPH je uzavřeno.   |

# Authentizace
<a name="authentizace"/>
K authentizace každého HTTP požadavku slouží HTTP hlavička **X-Auth-Key**.
    
Vzor HTTP hlavičky pro přihlášení:

    X-Auth-Key: 9f3ba84380c1a86b90f52bd46f0b6799
    
Parametrem této hlavička je přístupoví klíč.  
**Klíč lze vygenerovat v profilu uživatele** - [Stránka s profilem](https://online.iucto.cz/common/profile)  
  
Ke každé firmě, ke které máte přístup se generuje samostatný klíč.

![Vygenerování klíče](https://raw.githubusercontent.com/PavelMaca/iUcto-API/master/images/key_generation.PNG)

Uživatel musí mít v nastavené správcem firmy opraváněné pro přístup přes API (výchozí stav je povolen).
Při komunikaci prostřednictvím API má uživatel stejná oprávnění jako u webového rozhraní.
  

**[Správa ACL](https://online.iucto.cz/users)**  

# Typy dokladů
Seznam dostupných dokladů přes API a jejich interní identifikátory pro získání např. [Typů DPH](#typy_dph).

| Typ | Název                                         |
|:---:|-----------------------------------------------|
| FV  | [Faktury vydané](#faktury_vydane)             |
| FP  | [Faktury přijaté](#faktury_prijate)           |
| PV  | [Platby vydané](#platby-vydane)               |
| PP  | [Platby přijaté](#platby-prijate)             |
| OP  | [Objednávky přijaté](#objednavky-prijate)     |
| OV  | [Objednávky vydané](#objednavky-vydane)       |

# Group Datové typy
<a name="currency"></a>
## Měna [/currency]
### Seznam dostupných měn [GET]
Seznam dostupných měn pro použití v dokladech.

Dostupnost se může měnit v závislosti na aktivaci rozšíření **Účtování v cizích měnách**

+ Request
    + Headers

            X-Auth-Key: api-key

+ Response 200 (application/json)
    + Attributes (CURRENCY)

## Státy [/country]
<a name="country"></a>
Zde uvedený výpis není kompletní, jedná se pouze o vzor.
### Seznam dostupných států  [GET]
+ Request
    + Headers

            X-Auth-Key: api-key

+ Response 200 (application/json)
    + Attributes (COUNTRY)


## Preferované metody platby [/preferred_payment_method]
<a name="preferred_payment_method"></a>
### Seznam dostupných metod  [GET]
+ Request
    + Headers

            X-Auth-Key: api-key

+ Response 200 (application/json)
    + Attributes (PREFERRED_PAYMENT_METHOD)


# Group Firma


## Bankovní účty [/bank_account]
<a name="bank_account"></a>
### Seznam bankovních účtů [GET]
+ Request
    + Headers

            X-Auth-Key: api-key

+ Response 200 (application/hal+json)
    + Attributes (object)
        - _link (object)
            - self (object)
                - href: /1.1/bank_account (required)
        - _embedded (object)
            - `bank_account` (array[`BANK_ACCOUNT_LIST`], required) - Pole bankovních účtů
            
            
### Detail bankovního účtu [GET /bank_account/{id}]
+ Parameters
    + id: 123 (number, required)
        
+ Request
    + Headers

            X-Auth-Key: api-key
            
            
+ Response 200 (application/json)
    + Attributes (object)
        - _link (object)
            - self (object)
                - href: /1.1/bank_account/123 (required)
        - _embedded (object)
            - `bank_account` (`BANK_ACCOUNT_DETAIL`, required) - Detail bankovního účtu
            
            
### Editace bankovního účtu [PUT /bank_account/{id}]
+ Parameters
    + id: 123 (number, required)
    
+ Request (application/json)
    + Headers
    
            X-Auth-Key: api-key
        
    + Attributes (`BANK_ACCOUNT_PARAMS`)
    
+ Response 200 (application/json)
    + Attributes (object)
        - _link (object)
            - self (object)
                - href: /1.1/bank_account/123 (required)
        - _embedded (object)
            - `bank_account` (`BANK_ACCOUNT_DETAIL`, required) - Detail bankovního účtu
            
### Smazání bankovního účtu [DELETE /bank_account/{id}]
+ Parameters
    + id: 123 (number, required)
    
+ Request
    + Headers
    
            X-Auth-Key: api-key
        
+ Response 204


## Pokladny [/cash_register]
<a name="pokladny"></a>
### Seznam pokladen [GET]
+ Request
    + Headers

            X-Auth-Key: api-key

+ Response 200 (application/hal+json)
    + Attributes (object)
        - _link (object)
            - self (object)
                - href: /1.1/cash_register (required)
        - _embedded (object)
            - `cash_register` (array[`CASH_REGISTER`], required) - Pole dostupných pokladen

### Detail pokladny [GET /cash_register/{id}]
+ Parameters
    + id: 123 (number, required)
        
+ Request
    + Headers

            X-Auth-Key: api-key
            
            
+ Response 200 (application/json)
    + Attributes (object)
        - _link (object)
            - self (object)
                - href: /1.1/cash_register/123 (required)
        - _embedded (object)
            - `cash_register` (`CASH_REGISTER`, required) - Detail pokladny


## Provozovny [/business_premises]
<a name="business_premises"></a>
### Seznam provozoven [GET]
+ Request
    + Headers

            X-Auth-Key: api-key

+ Response 200 (application/hal+json)
    + Attributes (object)
        - _link (object)
            - self (object)
                - href: /1.1/business_premises (required)
        - _embedded (object)
            - `business_premises` (array[`BUSINESS_PREMISES`], required) - Pole dostupných provozoven
        
# Group Adresář
## Zákazníci [/customer]
### Seznam zákazníků [GET]
### Vytvoření zákazníka [POST /customer/{id}]
### Detail zakazníka [GET /customer/{id}]
### Editace zákazníka [PUT /customer/{id}]
### Smazání zákazníka [DELETE /customer/{id}]


## Dodavatelé [/supplier]
### Seznam dodavatelů [GET]
### Vytvoření dodavatele [POST /supplier/{id}]
### Detail dodavatele [GET /supplier/{id}]
### Editace dodavatele [PUT /supplier/{id}]
### Smazání dodavatele [DELETE /supplier/{id}]

# Group Platby

## Bankovní pohyby [/bank_transaction] 

Umožuje automatické spárování účetní dokladů s pohyby na bankovních účtech.

Po vložení nového pohybu se systém pokusí automaticky pohyb spárovat s existující fakturou.

### Seznam pohybů [GET]

Seznam bankovních pohybů, které jsou již imporotvané v systému.

+ Request
    + Headers

            X-Auth-Key: api-key
            
+ Response 200 (application/json)
    + Attributes (object)
        - _link (object)
            - self (object)
                - href: /1.1/bank_transaction (required)
        - _embedded (object)
            - `bank_transaction` (array[`BANK_STATEMENT_LIST`], required) - Pole transakcí

### Detail pohybu [GET /bank_transaction/{id}]

Podorbné inforamce k pohybu, včetně spárovaných dokladů.

+ Parameters
    + id: 123 (number, required)
        
+ Request
    + Headers

            X-Auth-Key: api-key
            
            
+ Response 200 (application/json)
    + Attributes (object)
        - _link (object)
            - self (object)
                - href: /1.1/bank_transaction/123 (required)
        - _embedded (object)
            - `bank_transaction` (BANK_STATEMENT_DETAIL, required) - Detail transakce

### Import pohybu [POST /bank_transaction/{id}]

Přenesení nového pohybu do systému.

Pohyb může být označen jedinečním idetifikátorm (`reference_number`) z výpisu, který zamezí duplicitám.

Součástí importu je automatický pokus o spárování dokladu s exitujícími doklady v systému.

**Pravidla pro spárování**
- Pohyb musí obsahovat položky `variable_symbol` a `bank_account`.
- Existuje právě jeden doklad s tímto variabilním symbolem a bankovním účtem, bude doklad spárován s pohybem.
- Nesmí být na pohybu větší částka než je na dokladu.

**Hodnoty závyslé na směru transakce**
Pokud je transakce příchozí je protistranou [Zákazník](#zakaznici) a s účetními parametry se zachází jako u [Platby přijaté](#platby-prijate).
- `coountraparty` 
- `account_entry_type` 
- `vat_type` 

+ Parameters
    + id: 123 (number, required)
    
+ Request (application/json)
    + Headers
    
            X-Auth-Key: api-key
        
    + Attributes (`BANK_STATEMENT_PARAMS`)
    
            
+ Response 200 (application/json)
    + Attributes (object)
        - _link (object)
            - self (object)
                - href: /1.1/bank_transaction/123 (required)
        - _embedded (object)
            - `bank_transaction` (`BANK_STATEMENT_DETAIL`, required) - Detail transakce
            
            
### Editace pohybu [PUT /bank_transaction/{id}]
Při editacy typu platby (`payment_type`) se musí také změnit hodnoty `counterparty` , `account_entry_type` a `vat_type`, tak aby odpovídali typu dokladu.

+ Parameters
    + id: 123 (number, required)
    
+ Request (application/json)
    + Headers
    
            X-Auth-Key: api-key
        
    + Attributes (`BANK_STATEMENT_PARAMS`)
    
+ Response 200 (application/json)
    + Attributes (object)
        - _link (object)
            - self (object)
                - href: /1.1/bank_transaction/123 (required)
        - _embedded (object)
            - `bank_transaction` (`BANK_STATEMENT_DETAIL`, required) - Detail transakce
            
### Smazání pohybu [DELETE /bank_transaction/{id}]
+ Parameters
    + id: 123 (number, required)
    
+ Request
    + Headers
    
            X-Auth-Key: api-key
        
+ Response 204
  
  
# Group Účetnictví
## Kurzy DPH [/vat_rates?date={date}]
<a name="kurz_dph"></a>
### Seznam kurzů DPH k danému datu [GET]
+ Parameters
    + date (string) ... Datum ve formátu yyy-mm-dd, pro které nás zajímají sazby
    
+ Request
    + Headers

            X-Auth-Key: api-key

+ Response 200 (application/json)
    
        [21,15,0]


## Typy účetních položek [/accountentry_type?doctype={doctype}]
<a name="typy_uc_polozek"></a>
### Seznam typů účetních položek [GET]
Odpověd obsahuje pole dostupných účetních položek v závislosti na parametru `doctype`.  

Formát odpovědi:
`{"id": "popis"}`

+ Parameters
    + doctype (string) ... Typ dokladu. Pro seznam dostupných hodnot si přejděte na [Typy dokladů](#typy_dokladu)
    
+ Request
    + Headers

            X-Auth-Key: api-key

+ Response 200 (application/json)
    
        {"141":"Bonusy od banky","163":"Da\u0148ov\u00fd bonus","167":"Da\u0148ov\u00fd doklad","62":"Dopravn\u00e9\/ p\u0159epravn\u00e9","149":"Dotace organiza\u010dn\u00ed slo\u017eky","166":"Kurzov\u00e9 zisky","176":"Kurzov\u00fd rozd\u00edl zisk","99":"Myln\u00e1 p\u0159\u00edchoz\u00ed platba","159":"Pen\u00edze na cest\u011b - p\u0159\u00edjem","136":"P\u0159evod mezi bankovn\u00edmi \u00fa\u010dty","126":"P\u0159ijat\u00e9 z\u00e1lohy ","10":"Prodej majetku","30":"Prodej materi\u00e1lu","7":"Prodej slu\u017eby","130":"Prodej slu\u017eby - extern\u00ed servis","171":"Prodej slu\u017eby - lokalizace","172":"Prodej slu\u017eby - marketing","142":"Prodej slu\u017eby - n\u00e1jem","170":"Prodej slu\u017eby - p\u0159eklady","144":"Prodej slu\u017eby - pron\u00e1jem zemn\u00edho stroje","131":"Prodej slu\u017eby - servis za\u0159\u00edzen\u00ed","173":"Prodej slu\u017eby - v\u00fduka","8":"Prodej v\u00fdrobky","3":"Prodej zbo\u017e\u00ed","168":"Smluvn\u00ed pokuty a \u00faroky z prodlen\u00ed","12":"\u00dahrada faktury vydan\u00e9","153":"\u00dahrada pohled\u00e1vky za spole\u010dn\u00edkem","187":"\u00dahrada z\u00e1lohov\u00e9 faktury vydan\u00e9","31":"\u00darok","1":"\u00darok z bankovn\u00edho \u00fa\u010dtu","162":"Vklad fyzick\u00e9 osoby do podnik\u00e1n\u00ed","134":"Vklad spole\u010dn\u00edka spole\u010dnosti","140":"Vr\u00e1cen\u00ed DPH finan\u010dn\u00edm \u00fa\u0159adem","146":"V\u00fdb\u011br z banky na pokladnu","132":"Vy\u00fa\u010dtov\u00e1n\u00ed odvodu hotovosti z pokladny","38":"Zaokrouhlen\u00ed ","139":"Zv\u00fd\u0161en\u00ed z\u00e1kladn\u00edho kapit\u00e1lu"}

## Typy DPH [/vat_type?doctype={doctype}]
<a name="typy_dph"></a>
### Seznam typů DPH [GET]
Odpověd obsahuje pole dostupných typů DPH v závislosti na parametru `doctype`.  

Formát odpovědi:
`{"id": "popis"}`

+ Parameters
    + doctype (string) ... Typ dokladu. Pro seznam dostupných hodnot si přejděte na [Typy dokladů](#typy_dokladu)
    
+ Request
    + Headers

            X-Auth-Key: api-key

+ Response 200 (application/json)
    
        {"20":"Bez DPH","11":"Dod\u00e1n\u00ed nov\u00e9ho dopravn\u00edho prost\u0159edku","3":"Dod\u00e1n\u00ed zbo\u017e\u00ed do EU","8":"Dod\u00e1n\u00ed zlata ","14":"Osvobozen\u00e9 pln\u011bn\u00ed bez odpo\u010dtu","13":"Osvobozen\u00e9 pln\u011bn\u00ed-n\u00e1rok ","4":"Poskytnut\u00ed slu\u017eby do EU","22":"P\u0159enesen\u00e1 da\u0148ov\u00e1 povinnost (dodavatel)","21":"P\u0159enesen\u00e1 da\u0148ov\u00e1 povinnost (odb\u011bratel)","15":"Slu\u017eby do zahrani\u010d\u00ed ","12":"V\u00fdvoz zbo\u017e\u00ed mimo EU ","23":"Zas\u00edl\u00e1n\u00ed zbo\u017e\u00ed","1":"Zdaniteln\u00e9 pln\u011bn\u00ed v \u010cR "}


## Účty účetní osnovy [/chart_account]
<a name="ucty_uc_osnovy"></a>
### Seznam dostupných účtů [GET]
Odpověd obsahuje pole dostupných účtů úč. osnovy pro použití v dokladech.

`{"id": "popis"}`

+ Request
    + Headers

            X-Auth-Key: api-key

+ Response 200 (application/json)
    
        {"3":"010000 - Dlouhodob\u00fd nehmotn\u00fd majetek ","4":"011000 - Z\u0159izovac\u00ed v\u00fddaje ","5":"012000 - Nehmotn\u00e9 v\u00fdsledky v\u00fdzkumu a v\u00fdvoje ","6":"013000 - Software ","7":"014000 - Oceniteln\u00e1 pr\u00e1va ","8":"015000 - Goodwill ","9":"019000 - Jin\u00fd dlouhodob\u00fd nehmotn\u00fd majetek ","10":"021000 - Stavby ","11":"022000 - Samostatn\u00e9 movit\u00e9 v\u011bci a soubory movit\u00fdch v\u011bc\u00ed ","12":"025000 - P\u011bstitelsk\u00e9 celky trval\u00fdch porost\u016f ","13":"026000 - Dosp\u011bl\u00e1 zv\u00ed\u0159ata a jejich skupiny ","14":"029000 - Jin\u00fd dlouhodob\u00fd hmotn\u00fd majetek ","15":"031000 - Pozemky ","16":"032000 - Um\u011bleck\u00e1 d\u00edla a sb\u00edrky ","17":"040000 - Nedokon\u010den\u00fd dlouhodob\u00fd nehmotn\u00fd a hmotn\u00fd majetek a po\u0159izovan\u00fd dlouhodob\u00fd finan\u010dn\u00ed majetek ","18":"041000 - Po\u0159\u00edzen\u00ed dlouhodob\u00e9ho nehmotn\u00e9ho majetku ","19":"042000 - Po\u0159\u00edzen\u00ed dlouhodob\u00e9ho hmotn\u00e9ho majetku ","20":"043000 - Po\u0159\u00edzen\u00ed dlouhodob\u00e9ho finan\u010dn\u00edho majetku ","21":"050000 - Poskytnut\u00e9 z\u00e1lohy na dlouhodob\u00fd majetek ","22":"051000 - Poskytnut\u00e9 z\u00e1lohy na dlouhodob\u00fd nehmotn\u00fd majetek ","23":"052000 - Poskytnut\u00e9 z\u00e1lohy na dlouhodob\u00fd hmotn\u00fd majetek ","24":"053000 - Poskytnut\u00e9 z\u00e1lohy na dlouhodob\u00fd finan\u010dn\u00ed majetek ","25":"061000 - Pod\u00edly v ovl\u00e1dan\u00fdch a \u0159\u00edzen\u00fdch osob\u00e1ch ","26":"062000 - Pod\u00edly v \u00fa\u010detn\u00edch jednotk\u00e1ch pod podstatn\u00fdm vlivem ","27":"063000 - Ostatn\u00ed cenn\u00e9 pap\u00edry a pod\u00edly ","28":"065000 - Dluhov\u00e9 cenn\u00e9 pap\u00edry dr\u017een\u00e9 do splatnosti ","29":"066000 - P\u016fj\u010dky a \u00fav\u011bry - ovl\u00e1daj\u00edc\u00ed a \u0159\u00edd\u00edc\u00ed osoby, podstatn\u00fd vliv ","30":"067000 - Ostatn\u00ed p\u016fj\u010dky ","31":"069000 - Jin\u00fd dlouhodob\u00fd finan\u010dn\u00ed majetek ","32":"070000 - Opr\u00e1vky k dlouhodob\u00e9mu nehmotn\u00e9mu majetku ","33":"071000 - Opr\u00e1vky ke z\u0159izovac\u00edm v\u00fddaj\u016fm ","34":"072000 - Opr\u00e1vky k nehmotn\u00fdm v\u00fdsledk\u016fm v\u00fdzkumu a v\u00fdvoje ","35":"073000 - Opr\u00e1vky k softwaru ","36":"074000 - Opr\u00e1vky k oceniteln\u00fdm pr\u00e1v\u016fm ","37":"075000 - Opr\u00e1vky ke goodwillu ","38":"079000 - Opr\u00e1vky k jin\u00e9mu dlouhodob\u00e9mu nehmotn\u00e9mu majetku ","39":"081000 - Opr\u00e1vky ke stavb\u00e1m ","40":"082000 - Opr\u00e1vky k samost. movit\u00fdm v\u011bcem a soubor\u016fm movit\u00fdch v\u011bc\u00ed ","41":"085000 - Opr\u00e1vky k p\u011bstitelsk\u00fdm celk\u016fm trval\u00fdch porost\u016f ","42":"086000 - Opr\u00e1vky k z\u00e1kladn\u00edmu st\u00e1du a ta\u017en\u00fdm zv\u00ed\u0159at\u016fm ","43":"089000 - Opr\u00e1vky k jin\u00e9mu dlouhodob\u00e9mu hmotn\u00e9mu majetku ","44":"091000 - Opravn\u00e1 polo\u017eka k dlouhodob\u00e9mu nehmotn\u00e9mu majetku ","45":"092000 - Opravn\u00e1 polo\u017eka k dlouhodob\u00e9mu hmotn\u00e9mu majetku ","46":"093000 - Opravn\u00e1 polo\u017eka k dlouhodob\u00e9mu nedokon\u010den\u00e9mu nehmotn\u00e9mu majetku ","47":"094000 - Opravn\u00e1 polo\u017eka k dlouhodob\u00e9mu nedokon\u010den\u00e9mu hmotn\u00e9mu majetku ","48":"095000 - Opravn\u00e1 polo\u017eka k poskytnut\u00fdm z\u00e1loh\u00e1m na dlouhodob\u00fd majetek ","49":"096000 - Opravn\u00e1 polo\u017eka k dlouhodob\u00e9mu finan\u010dn\u00edmu majetku ","50":"097000 - Oce\u0148ovac\u00ed rozd\u00edl k nabyt\u00e9mu majetku ","51":"098000 - Opr\u00e1vky k oce\u0148ovac\u00edmu rozd\u00edlu k nabyt\u00e9mu majetku ","102":"111000 - Po\u0159\u00edzen\u00ed materi\u00e1lu ","103":"112000 - Materi\u00e1l na sklad\u011b ","104":"119000 - Materi\u00e1l na cest\u011b ","105":"121000 - Nedokon\u010den\u00e1 v\u00fdroba ","106":"122000 - Polotovary vlastn\u00ed v\u00fdroby ","107":"123000 - V\u00fdrobky ","115":"124000 - Mlad\u00e1 a ostatn\u00ed zv\u00ed\u0159ata a jejich skupiny ","108":"131000 - Po\u0159\u00edzen\u00ed zbo\u017e\u00ed ","109":"132000 - Zbo\u017e\u00ed na sklad\u011b a v prodejn\u00e1ch ","110":"139000 - Zbo\u017e\u00ed na cest\u011b ","111":"151000 - Poskytnut\u00e9 z\u00e1lohy na materi\u00e1l ","112":"152000 - Poskytnut\u00e9 z\u00e1lohy na zv\u00ed\u0159ata ","113":"153000 - Poskytnut\u00e9 z\u00e1lohy na zbo\u017e\u00ed ","114":"191000 - Opravn\u00e1 polo\u017eka k materi\u00e1lu ","129":"192000 - Opravn\u00e1 polo\u017eka k nedokon\u010den\u00e9 v\u00fdrob\u011b ","130":"193000 - Opravn\u00e1 polo\u017eka k polotovar\u016fm vlastn\u00ed v\u00fdroby ","131":"194000 - Opravn\u00e1 polo\u017eka k v\u00fdrobk\u016fm ","132":"195000 - Opravn\u00e1 polo\u017eka ke zv\u00ed\u0159at\u016fm ","133":"196000 - Opravn\u00e1 polo\u017eka ke zbo\u017e\u00ed ","134":"197000 - Opravn\u00e1 polo\u017eka k z\u00e1loh\u00e1m na materi\u00e1l","135":"198000 - Opravn\u00e1 polo\u017eka k z\u00e1loh\u00e1m na zbo\u017e\u00ed ","136":"199000 - Opravn\u00e1 polo\u017eka k z\u00e1loh\u00e1m na zv\u00ed\u0159ata ","52":"211000 - Pokladna ","53":"213000 - Ceniny ","54":"221000 - Bankovn\u00ed \u00fa\u010dty ","55":"231000 - Kr\u00e1tkodob\u00e9 bankovn\u00ed \u00fav\u011bry ","56":"232000 - Eskontn\u00ed \u00fav\u011bry ","149":"241000 - Emitovan\u00e9 kr\u00e1tkodob\u00e9 dluhopisy","150":"249000 - Ostatn\u00ed kr\u00e1tkodob\u00e9 finan\u010dn\u00ed v\u00fdpomoci","151":"251000 - Registrovan\u00e9 majetkov\u00e9 cenn\u00e9 pap\u00edry k obchodov\u00e1n\u00ed","152":"252000 - Vlastn\u00ed akcie a vlastn\u00ed obchodn\u00ed pod\u00edly","153":"253000 - Registrovan\u00e9 dluhov\u00e9 cenn\u00e9 pap\u00edry k obchodov\u00e1n\u00ed","154":"255000 - Vlastn\u00ed dluhopisy","155":"256000 - Dluhov\u00e9 cenn\u00e9 pap\u00edry se splat. do 1 roku dr\u017een\u00e9 do splatnosti","156":"257000 - Ostatn\u00ed cenn\u00e9 pap\u00edry k obchodov\u00e1n\u00ed","157":"259000 - Po\u0159izov\u00e1n\u00ed kr\u00e1tkodob\u00e9ho finan\u010dn\u00edho majetku","139":"261000 - Pen\u00edze na cest\u011b ","158":"291000 - Opravn\u00e1 polo\u017eka ke kr\u00e1tkodob\u00e9mu finan\u010dn\u00edmu majetku","57":"311000 - Pohled\u00e1vky z obchodn\u00edch vztah\u016f ","159":"312000 - Sm\u011bnky k inkasu","160":"313000 - Pohled\u00e1vky za eskontovan\u00e9 cenn\u00e9 pap\u00edry","161":"314000 - Poskytnut\u00e9 z\u00e1lohy - dlouhodob\u00e9 a kr\u00e1tkodob\u00e9","162":"315000 - Ostatn\u00ed pohled\u00e1vky","140":"321000 - Z\u00e1vazky z obchodn\u00edch vztah\u016f ","163":"322000 - Sm\u011bnka k \u00fahrad\u011b","141":"324000 - P\u0159ijat\u00e9 provozn\u00ed z\u00e1lohy ","142":"325000 - Ostatn\u00ed z\u00e1vazky ","58":"331000 - Zam\u011bstnanci ","59":"333000 - Ostatn\u00ed z\u00e1vazky v\u016f\u010di zam\u011bstnanc\u016fm ","61":"335000 - Pohled\u00e1vky za zam\u011bstnanci ","60":"336000 - Z\u00fa\u010dtov\u00e1n\u00ed s institucemi soci\u00e1l. zabezpe\u010den\u00ed a zdravot. poji\u0161t\u011bn\u00ed ","62":"341000 - Da\u0148 z p\u0159\u00edjm\u016f ","63":"342000 - Ostatn\u00ed p\u0159\u00edm\u00e9 dan\u011b ","64":"343000 - Da\u0148 z p\u0159idan\u00e9 hodnoty ","65":"345000 - Ostatn\u00ed dan\u011b a poplatky ","166":"346000 - Dotace ze st\u00e1tn\u00edho rozpo\u010dtu","167":"347000 - Ostatn\u00ed dotace","66":"349000 - Vyrovn\u00e1vac\u00ed \u00fa\u010det pro DPH ","168":"351000 - Pohled\u00e1vky - ovl\u00e1daj\u00edc\u00ed a \u0159\u00edd\u00edc\u00ed osoba","169":"352000 - Pohled\u00e1vky - podstatn\u00fd vliv","170":"353000 - Pohled\u00e1vky za upsan\u00fd z\u00e1kladn\u00ed kapit\u00e1l","171":"354000 - Pohled\u00e1vky za spole\u010dn\u00edky p\u0159i \u00fahrad\u011b ztr\u00e1ty","172":"355000 - Ostatn\u00ed pohled\u00e1vky za spole\u010dn\u00edky a \u010dleny dru\u017estva","173":"358000 - Pohled\u00e1vky za \u00fa\u010dastn\u00edky sdru\u017een\u00ed","174":"361000 - Z\u00e1vazky - ovl\u00e1daj\u00edc\u00ed a \u0159\u00edd\u00edc\u00ed osoba","175":"362000 - Z\u00e1vazky - podstatn\u00fd vliv","176":"364000 - Z\u00e1vazky ke spole\u010dn\u00edk\u016fm p\u0159i rozd\u011blov\u00e1n\u00ed zisku","177":"365000 - Ostatn\u00ed z\u00e1vazky ke spole\u010dn\u00edk\u016fm a \u010dlen\u016fm dru\u017estva","178":"366000 - Z\u00e1vazky ke spole\u010dn\u00edk\u016fm a \u010dlen\u016fm dru\u017estva ze z\u00e1visl\u00e9 \u010dinnosti","179":"367000 - Z\u00e1vazky z upsan\u00fdch nesplacen\u00fdch cenn\u00fdch pap\u00edr\u016f a vklad\u016f","180":"368000 - Z\u00e1vazky k \u00fa\u010dastn\u00edk\u016fm sdru\u017een\u00ed","181":"371000 - Pohled\u00e1vky z prodeje podniku","182":"372000 - Z\u00e1vazky z koup\u011b podniku","183":"373000 - Pohled\u00e1vky a z\u00e1vazky z pevn\u00fdch term\u00ednov\u00fdch operac\u00ed","184":"374000 - Pohled\u00e1vky z pron\u00e1jmu","185":"375000 - Pohled\u00e1vky z emitovan\u00fdch dluhopis\u016f","186":"376000 - Nakoupen\u00e9 opce","187":"377000 - Prodan\u00e9 opce","67":"378000 - Jin\u00e9 pohled\u00e1vky ","68":"379000 - Jin\u00e9 z\u00e1vazky ","188":"381000 - N\u00e1klady p\u0159\u00ed\u0161t\u00edch obdob\u00ed","189":"382000 - Komplexn\u00ed n\u00e1klady p\u0159\u00ed\u0161t\u00edch obdob\u00ed","190":"383000 - V\u00fddaje p\u0159\u00ed\u0161t\u00edch obdob\u00ed","191":"384000 - V\u00fdnosy p\u0159\u00ed\u0161t\u00edch obdob\u00ed","192":"385000 - P\u0159\u00edjmy p\u0159\u00ed\u0161t\u00edch obdob\u00ed","193":"388000 - Dohadn\u00e9 \u00fa\u010dty aktivn\u00ed","194":"389000 - Dohadn\u00e9 \u00fa\u010dty pasivn\u00ed","195":"391000 - Opravn\u00e1 polo\u017eka k pohled\u00e1vk\u00e1m","196":"395000 - Vnit\u0159n\u00ed z\u00fa\u010dtov\u00e1n\u00ed","197":"398000 - Spojovac\u00ed \u00fa\u010det p\u0159i sdru\u017een\u00ed","69":"411000 - Z\u00e1kladn\u00ed kapit\u00e1l ","116":"412000 - Emisn\u00ed a\u017eio ","117":"413000 - Ostatn\u00ed kapit\u00e1lov\u00e9 fondy ","198":"414000 - Oce\u0148ovac\u00ed rozd\u00edly z p\u0159ecen\u011bn\u00ed majetku a z\u00e1vazk\u016f","199":"417000 - Rozd\u00edly z p\u0159em\u011bn spole\u010dnost\u00ed","200":"418000 - Oce\u0148ovac\u00ed rozd\u00edly z p\u0159ecen\u011bn\u00ed p\u0159i p\u0159em\u011bn\u00e1ch spole\u010dnost\u00ed","118":"419000 - Zm\u011bny z\u00e1kladn\u00edho kapit\u00e1lu ","119":"421000 - Z\u00e1konn\u00fd rezervn\u00ed fond ","120":"422000 - Ned\u011bliteln\u00fd fond ","121":"423000 - Statut\u00e1rn\u00ed fondy ","122":"427000 - Ostatn\u00ed fondy ","123":"428000 - Nerozd\u011blen\u00fd zisk minul\u00fdch let ","124":"429000 - Neuhrazen\u00e1 ztr\u00e1ta minul\u00fdch let ","125":"431000 - V\u00fdsledek hospoda\u0159en\u00ed ve schvalovac\u00edm \u0159\u00edzen\u00ed ","201":"451000 - Rezervy podle zvl\u00e1\u0161tn\u00edch pr\u00e1vn\u00edch p\u0159edpis\u016f","202":"452000 - Rezerva na d\u016fchody a podobn\u00e9 z\u00e1vazky","126":"453000 - Rezerva na da\u0148 z p\u0159\u00edjm\u016f ","127":"459000 - Ostatn\u00ed rezervy ","203":"461000 - Bankovn\u00ed \u00fav\u011bry","204":"471000 - Dlouhodob\u00e9 z\u00e1vazky - ovl\u00e1daj\u00edc\u00ed a \u0159\u00edd\u00edc\u00ed osoba","205":"472000 - Dlouhodob\u00e9 z\u00e1vazky - podstatn\u00fd vliv","206":"473000 - Emitovan\u00e9 dluhopisy","207":"474000 - Z\u00e1vazky z pron\u00e1jmu","128":"475000 - Dlouhodob\u00e9 p\u0159ijat\u00e9 z\u00e1lohy ","208":"478000 - Dlouhodob\u00e9 sm\u011bnky k \u00fahrad\u011b","209":"479000 - Jin\u00e9 dlouhodob\u00e9 z\u00e1vazky","210":"481000 - Odlo\u017een\u00fd da\u0148ov\u00fd z\u00e1vazek a pohled\u00e1vka","211":"491000 - \u00da\u010det individu\u00e1ln\u00edho podnikatele","70":"501000 - Spot\u0159eba materi\u00e1lu ","71":"502000 - Spot\u0159eba energie ","72":"503000 - Spot\u0159eba ostatn\u00edch neskladovateln\u00fdch dod\u00e1vek ","73":"504000 - Prodan\u00e9 zbo\u017e\u00ed ","74":"511000 - Opravy a udr\u017eov\u00e1n\u00ed ","75":"512000 - Cestovn\u00e9 ","76":"513000 - N\u00e1klady na reprezentaci ","77":"518000 - Ostatn\u00ed slu\u017eby ","213":"520000 - Osobn\u00ed n\u00e1klady","78":"521000 - Mzdov\u00e9 n\u00e1klady ","79":"522000 - P\u0159\u00edjmy spole\u010dn\u00edk\u016f a \u010dlen\u016f dru\u017estva ze z\u00e1visl\u00e9 \u010dinnosti ","80":"523000 - Odm\u011bny \u010dlen\u016fm org\u00e1n\u016f spole\u010dnosti a dru\u017estva ","81":"524000 - Z\u00e1konn\u00e9 soci\u00e1ln\u00ed poji\u0161t\u011bn\u00ed ","82":"525000 - Ostatn\u00ed soci\u00e1ln\u00ed poji\u0161t\u011bn\u00ed ","83":"526000 - Soci\u00e1ln\u00ed n\u00e1klady individu\u00e1ln\u00edho podnikatele ","84":"527000 - Z\u00e1konn\u00e9 soci\u00e1ln\u00ed n\u00e1klady ","85":"528000 - Ostatn\u00ed soci\u00e1ln\u00ed n\u00e1klady ","214":"530000 - Dan\u011b a poplatky","86":"531000 - Da\u0148 silni\u010dn\u00ed ","87":"532000 - Da\u0148 z nemovitost\u00ed ","88":"538000 - Ostatn\u00ed dan\u011b a poplatky ","215":"540000 - Jin\u00e9 provozn\u00ed n\u00e1klady","89":"541000 - Z\u016fstatkov\u00e1 cena prodan\u00e9ho dlouhodob\u00e9ho nehmotn\u00e9ho a hmotn\u00e9ho majetku ","90":"542000 - Prodan\u00fd materi\u00e1l ","91":"543000 - Dary ","92":"544000 - Smluvn\u00ed pokuty a \u00faroky z prodlen\u00ed ","143":"545000 - Ostatn\u00ed pokuty a pen\u00e1le ","144":"546000 - Odpis pohled\u00e1vky ","216":"548000 - Ostatn\u00ed provozn\u00ed n\u00e1klady","217":"549000 - Manka a \u0161kody z provozn\u00ed \u010dinnosti","93":"551000 - Odpisy dlouhodob\u00e9ho nehmotn\u00e9ho a hmotn\u00e9ho majetku ","218":"552000 - Tvorba a z\u00fa\u010dtov\u00e1n\u00ed rezerv podle zvl\u00e1\u0161tn\u00edch pr\u00e1vn\u00edch p\u0159edpis\u016f","219":"554000 - Tvorba a z\u00fa\u010dtov\u00e1n\u00ed ostatn\u00edch rezerv","220":"555000 - Tvorba a z\u00fa\u010dtov\u00e1n\u00ed komplexn\u00edch n\u00e1klad\u016f p\u0159\u00ed\u0161t\u00edch obdob\u00ed","221":"557000 - Z\u00fa\u010dtov\u00e1n\u00ed opr\u00e1vky k oce\u0148ovac\u00edmu rozd\u00edlu k nabyt\u00e9mu majetku","222":"558000 - Tvorba a z\u00fa\u010dtov\u00e1n\u00ed z\u00e1konn\u00fdch opravn\u00fdch polo\u017eek v provozn\u00ed \u010dinnosti","223":"559000 - Tvorba a z\u00fa\u010dtov\u00e1n\u00ed opravn\u00fdch polo\u017eek v provozn\u00ed \u010dinnosti","224":"560000 - Finan\u010dn\u00ed n\u00e1klady","225":"561000 - Prodan\u00e9 cenn\u00e9 pap\u00edry a pod\u00edly","145":"562000 - \u00daroky ","146":"563000 - Kurzov\u00e9 ztr\u00e1ty ","226":"564000 - N\u00e1klady z p\u0159ecen\u011bn\u00ed cenn\u00fdch pap\u00edr\u016f","227":"566000 - N\u00e1klady z finan\u010dn\u00edho majetku","228":"567000 - N\u00e1klady z deriv\u00e1tov\u00fdch operac\u00ed","229":"568000 - Ostatn\u00ed finan\u010dn\u00ed n\u00e1klady","230":"569000 - Manka a \u0161kody na finan\u010dn\u00edm majetku","231":"574000 - Tvorba a z\u00fa\u010dtov\u00e1n\u00ed finan\u010dn\u00edch rezerv","232":"579000 - Tvorba a z\u00fa\u010dtov\u00e1n\u00ed opravn\u00fdch polo\u017eek ve finan\u010dn\u00ed \u010dinnosti","233":"580000 - Mimo\u0159\u00e1dn\u00e9 n\u00e1klady","234":"581000 - N\u00e1klady na zm\u011bnu metody","235":"582000 - \u0160kody","236":"584000 - Tvorba a z\u00fa\u010dtov\u00e1n\u00ed mimo\u0159\u00e1dn\u00fdch rezerv","237":"588000 - Ostatn\u00ed mimo\u0159\u00e1dn\u00e9 n\u00e1klady","238":"589000 - Tvorba a z\u00fa\u010dtov\u00e1n\u00ed opravn\u00fdch polo\u017eek v mimo\u0159\u00e1dn\u00e9 \u010dinnosti","239":"591000 - Da\u0148 z p\u0159\u00edjm\u016f z b\u011b\u017en\u00e9 \u010dinnosti - splatn\u00e1","240":"592000 - Da\u0148 z p\u0159\u00edjm\u016f z b\u011b\u017en\u00e9 \u010dinnosti - odlo\u017een\u00e1","241":"593000 - Da\u0148 z p\u0159\u00edjm\u016f z mimo\u0159\u00e1dn\u00e9 \u010dinnosti - splatn\u00e1","242":"594000 - Da\u0148 z p\u0159\u00edjm\u016f z mimo\u0159\u00e1dn\u00e9 \u010dinnosti - odlo\u017een\u00e1","243":"595000 - Dodate\u010dn\u00e9 odvody dan\u011b z p\u0159\u00edjm\u016f","244":"596000 - P\u0159evod pod\u00edlu na v\u00fdsledku hospoda\u0159en\u00ed spole\u010dn\u00edk\u016fm","245":"597000 - P\u0159evod provozn\u00edch n\u00e1klad\u016f","246":"598000 - P\u0159evod finan\u010dn\u00edch n\u00e1klad\u016f","247":"599000 - Rezerva na da\u0148 z p\u0159\u00edjmu","94":"601000 - Tr\u017eby za vlastn\u00ed v\u00fdrobky ","1":"602000 - Tr\u017eby z prodeje slu\u017eeb ","2":"604000 - Tr\u017eby za zbo\u017e\u00ed ","248":"610000 - Zm\u011bny stavu z\u00e1sob vlastn\u00ed \u010dinnosti","249":"611000 - Zm\u011bna stavu nedokon\u010den\u00e9 v\u00fdroby","250":"612000 - Zm\u011bna stavu polotovar\u016f vlastn\u00ed v\u00fdroby","251":"613000 - Zm\u011bna stavu v\u00fdrobk\u016f","252":"614000 - Zm\u011bna stavu zv\u00ed\u0159at","253":"620000 - Aktivace","254":"621000 - Aktivace materi\u00e1lu a zbo\u017e\u00ed","255":"622000 - Aktivace vnitropodnikov\u00fdch slu\u017eeb","256":"623000 - Aktivace dlouhodob\u00e9ho nehmotn\u00e9ho majetku","257":"624000 - Aktivace dlouhodob\u00e9ho hmotn\u00e9ho majetku","258":"640000 - Jin\u00e9 provozn\u00ed v\u00fdnosy","100":"641000 - Tr\u017eby z prodeje dlouhodob\u00e9ho nehmotn\u00e9ho a hmotn\u00e9ho majetku ","101":"642000 - Tr\u017eby z prodeje materi\u00e1lu ","137":"644000 - Smluvn\u00ed pokuty a \u00faroky z prodlen\u00ed ","259":"646000 - V\u00fdnosy z odepsan\u00fdch pohled\u00e1vek","138":"648000 - Ostatn\u00ed provozn\u00ed v\u00fdnosy ","260":"660000 - Finan\u010dn\u00ed v\u00fdnosy","261":"661000 - Tr\u017eby z prodeje cenn\u00fdch pap\u00edr\u016f a pod\u00edl\u016f","95":"662000 - \u00daroky ","96":"663000 - Kursov\u00e9 zisky ","262":"664000 - V\u00fdnosy z p\u0159ecen\u011bn\u00ed cenn\u00fdch pap\u00edr\u016f","263":"665000 - V\u00fdnosy z dlouhodob\u00e9ho finan\u010dn\u00edho majetku","264":"666000 - V\u00fdnosy z kr\u00e1tkodob\u00e9ho finan\u010dn\u00edho majetku","265":"667000 - V\u00fdnosy z deriv\u00e1tov\u00fdch operac\u00ed","266":"668000 - Ostatn\u00ed finan\u010dn\u00ed v\u00fdnosy","267":"680000 - Mimo\u0159\u00e1dn\u00e9 v\u00fdnosy","268":"681000 - V\u00fdnosy ze zm\u011bny metody","591":"684000 - \u010clensk\u00e9 p\u0159\u00edsp\u011bvky","269":"688000 - Ostatn\u00ed mimo\u0159\u00e1dn\u00e9 v\u00fdnosy","270":"697000 - P\u0159evod provozn\u00edch v\u00fdnos\u016f","271":"698000 - P\u0159evod finan\u010dn\u00edch v\u00fdnos\u016f","97":"701000 - Po\u010d\u00e1te\u010dn\u00ed \u00fa\u010det rozva\u017en\u00fd ","98":"702000 - Kone\u010dn\u00fd \u00fa\u010det rozva\u017en\u00fd ","99":"710000 - \u00da\u010det zisk\u016f a ztr\u00e1t "}


## Účty DPH [/vat_chart]
<a name="ucty_dph"></a>
### Seznam dostupných účtů [GET]
Odpověd obsahuje pole dostupných Účty DPH pro použití v dokladech.

`{"id": "popis"}`

+ Request
    + Headers

            X-Auth-Key: api-key

+ Response 200 (application/json)
    
        {"281":"98006000 - Příjem DPH (vratka)","302":"96009000 - Platba DPH"}



## Data Structures

### `BANK_STATEMENT_BASE` (object)
#### Properties
- `payment_type` (PAYMENT_TYPE, required) - Typ pohybu
- price: 7845 (PRICE, required) - Částka
- currency: CZK (enum, sample, required) - [**Měna**](#currency)
- `date_payment`: `2017-01-24` (string, optional) - Datum platby
- `item_type`: Příchozí platba (string, optional) - Typ položky (popisek)
- `variable_symbol` - Variabilní symbol
- `constant_symbol` - Konstantní symbol
- `specific_symbol` - Specifický symbol
- `bank_account`: 544 (number, optional) - [**ID bankovního účtu**](#bank_account)
- `bank_account_contraparty`: `19-123457/0710` (string, optional) - Číslo bankovního účtu protistrany

### `BANK_STATEMENT_EXTRA` (object)

Tento objekt reprezentuje detail bankovní transakce.

#### Properties

- `variable_symbol2` (string, optional)
- `description_tome` (string, optional) - Poznámka pro mě
- `description_toyou` (string, optional) - Poznámka pro mě
- `reference_number` (string, optional) - Vlastní identifikátor pohybu
- `counterparty`:42 (number, optional) - ID protistrany, pro příchozí pohyb [Zákazník](#zakaznici), pro odchozí [Dodavatel](#dodavatele)
- `vat_type`:467 (number, optional) - [**Typ DPH**](#typy_dph)
- `account_entry_type`:687 (number, optional) - [**Typ účetní položky**](#položky typy_uc_polozek)
- `chart_account`:46 (number, optional)- [**Účet úč. osnovy**](#ucty_uc_osnovy)
- `vat_chart`:52 (number, optional) - [**Účet DPH**](#ucty_dph)

### `BANK_STATEMENT_READONLY` (object)
#### Properties
- id: 7745 (number, required) - Unikátní id transakce v systému.
- status (`BANK_STATEMENT_STATUS`, required) - Stav spárování
- price_czk (number) - Částka v CZK podle kurzu k datu pohybu


### `BANK_STATEMENT_PARAMS` (object)
#### Properties
- Include `BANK_STATEMENT_BASE`
- Include `BANK_STATEMENT_EXTRA`
- One Of
    - `account_entry_type`:687 (number, optional) - [**Typ účetní položky**](#položky typy_uc_polozek)
    - Properties
        - `chart_account`:46 (number, optional)- [**Účet úč. osnovy**](#ucty_uc_osnovy)
        - `vat_chart`:52 (number, optional) - [**Účet DPH**](#ucty_dph)

### `BANK_STATEMENT_DETAIL` (object)

Tento objekt reprezentuje zjednodušený náhled bankovní transakce.

#### Properties
- Include `BANK_STATEMENT_LIST`
- Include `BANK_STATEMENT_EXTRA`
- `linked_docs` (array[object], optional) - Seznam spárovaných dokladů

### `BANK_STATEMENT_LIST` (object)

Tento objekt reprezentuje zjednodušený náhled bankovní transakce.

#### Properties
- Include `BANK_STATEMENT_READONLY`
- Include `BANK_STATEMENT_BASE`
- `bank_account` (BANK_ACCOUNT_DETAIL, optional) - [**Bankovní účet**](#bank_account)
- `counterparty` (object, optional) - protistrany, pro příchozí pohyb [Zákazník](#zakaznici), pro odchozí [Dodavatel](#dodavatele)


### `PAYMENT_TYPE` (enum)

Směr platby.

#### Members

- in - Příchozí platba
- out - Odchozí platba

### PRICE (number)


### `BANK_STATEMENT_STATUS` (enum)

#### Members
- notprocessed - Nezpracováno
- unmatched - Nespárováno
- matched - Spárováno
- halfmatcheded - Částečně spárováno
- accounted - Zaúčtováno

### CURRENCY (array)
Seznam dostupných měn

## Sample
- CZK
- EUR
- USD

### COUNTRY (array)
Ukázkový seznam států

## Sample
- AD - Andorra
- AE - Spojené arabské emiráty
- AF - Afghánistán
- AG - Antigua a Barbuda

### PREFERRED_PAYMENT_METHOD (array)
Seznam dostupných metod platby

## Sample
- transfer - Bankovním převodem
- cash - V hotovosti
- proforma - Proforma
- check - Šekem
- creditcard - Platební kartou


### `BANK_ACCOUNT_READONLY` (object)
#### Properties
- id: 456 (number, required) - ID účtu)
- number: `19-123457/0100` (string, required) - Číslo účtu

### `BANK_ACCOUNT_BASE` (object)
#### Properties
- name: `Komerční Banka` (string, required) - Název účtu
- currency: CZK (enum, sample, required) - [**Měna**](#currency)
- isdefault: true (boolean, required) - Nastaven jako výchozí

### `BANK_ACCOUNT_EXTRA` (object)
#### Properties
- account_prefix: 19 (number, optional) - Předčíslí účtu
- account_number: 123457 (number, required) - Číslo účtu (bez předčíslí)
- bank_code: 0100 (string, required) - Kód banky
- swift: KOMBCZPPXXX (string, optional) - SWIFT
- iban: CZ10INGB0697669236 (string, optional) - IBAN
- initial_state: 0 (number) - Počátenční stav účtu
- chart_account_id: 424 (number, optional) - [**Účet úč. osnovy**](#ucty_uc_osnovy)
- chart_account_valid_from: `01-01-2017` (string, required) - Účet úč. osnovy platný od
- visible: true (boolean) - Viditelnost účtu v seznamech

### `BANK_ACCOUNT_LIST` (object)
#### Properties
- Include `BANK_ACCOUNT_READONLY`
- Include `BANK_ACCOUNT_BASE`


### `BANK_ACCOUNT_DETAIL` (object)
#### Properties
- Include `BANK_ACCOUNT_READONLY`
- Include `BANK_ACCOUNT_BASE`
- Include `BANK_ACCOUNT_EXTRA`


### `BANK_ACCOUNT_PARAMS` (object)
#### Properties
- Include `BANK_ACCOUNT_BASE`
- Include `BANK_ACCOUNT_EXTRA`


### `CASH_REGISTER` (object)
#### Properties
- _link (object)
    - self (object)
        - href: /1.1/cash_register/475 (required)
- id: 475 (number, required) - ID účtu
- name: `EET pokladna` (string, required) - Název pokladny
- currency: CZK (enum, sample, required) - [**Měna**](#currency)
- isdefault: true (boolean, required) - Nastavena jako výchozí
- initial_state: 0 (number) - Počátenční stav pokladny
- iseet: true (boolean, required) - Zda je pokladna nastavená pro EET
- `business_premises` (BUSINESS_PREMISES, optional)  - [**Provozovna**](#business_premises)


### `BUSINESS_PREMISES` (object)
#### Properties
- _link (object)
    - self (object)
        - href: /1.1/business_premises/897 (required)
- id: 897 (number, required) - ID provozovny
- name: `Restaurace u koníčka` (string, required) - Název provozovny
- isdefault: true (boolean) - Nastavena jako výchozí
