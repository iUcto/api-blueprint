# Group Seznamy
## Měny [/currency]
<a name="currency"></a>
### Seznam dostupných měn [GET]
Seznam dostupných měn pro použití v dokladech.

Dostupnost se může měnit v závislosti na aktivaci rozšíření **Účtování v cizích měnách**

+ Request
    + Headers
    
            X-Auth-Key: api-key

+ Response 200 (application/json)
    + Attributes (CURRENCY)

## Metody plateb [/payment_type]
<a name="metody_plateb"></a>
### Seznam dostupných metod  [GET]
Seznam dostupných metod pro provedení platby používaných v dokladech.
+ Request
    + Headers

            X-Auth-Key: api-key

+ Response 200 (application/json)
    + Attributes (PAYMENT_TYPE)

## Způsoby zaokrouhlení [/rounding_type]
<a name="zaokrouhleni"></a>
### Seznam dostupných typů zaokrouhlení [GET]
Seznam metod pro zaokrouhlování částek v dokladech.
+ Request
    + Headers

            X-Auth-Key: api-key

+ Response 200 (application/json)
    + Attributes (ROUNDING_TYPE)
