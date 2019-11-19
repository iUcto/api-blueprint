# iÚčto
<a name="top"/>

`https://online.iucto.cz/api`

iÚčto API je založena na REST principu.
API umožnuje práci s různými typy dokladů pomocí HTTP požadavků zasílaných ve formátu popsaném níže.

`Ke každému HTTP požadavku je nutné přidat HTTP hlavičku X-Auth-key`

Vešekerá komunikace probíhá pomocí protokolu HTTPS.

Odpovědi serveru jsou ve formátu [HAL+JSON](http://stateless.co/hal_specification.html).
Tělo požadavku v případě POST a PUT musí být ve formátu JSON.

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

# Datové typy
| Datový typ | Popis                                                |
|------------|------------------------------------------------------|
| String     | Textoví řetězec v kódování UTF-8                     | 
| Number     | Celé číslo                                           |
| Boolean    | Hodnoty `true` a `false`                             |
| Date       | Datum ve formátu `YYYY-mm-dd`                        |
| Price      | Číslo se dvemi desetinými místy ve formátu `1500.10` |

Parametry uvedené jako volitelné, mohou nabývat hodnot `NULL`.

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
