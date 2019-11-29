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

## Střediska [/department]
<a name="strediska"></a>

## Zakázky [/contract]
<a name="zakazky"></a>

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
