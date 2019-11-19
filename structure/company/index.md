## `BANK_ACCOUNT_READONLY` (object)
### Properties
- id: 456 (number, required) - ID účtu
- number: `19-123457/0100` (string, required) - Číslo účtu

## `BANK_ACCOUNT_BASE` (object)
### Properties
- name: `Komerční Banka` (string, required) - Název účtu
- currency: CZK (enum, sample, required) - [**Měna**](#currency)
- isdefault: true (boolean, required) - Nastaven jako výchozí

## `BANK_ACCOUNT_EXTRA` (object)
### Properties
- `account_prefix`: 19 (number, optional) - Předčíslí účtu
- `account_number`: 123457 (number, required) - Číslo účtu (bez předčíslí)
- `bank_code`: 0100 (string, required) - Kód banky
- swift: KOMBCZPPXXX (string, optional) - SWIFT
- iban: `CZ10INGB0697669236` (string, optional) - IBAN
- `initial_state`: 0 (number) - Počátenční stav účtu
- `chart_account_id`: 424 (number, optional) - [**Účet úč. osnovy**](#ucty_uc_osnovy)
- `chart_account_valid_from`: `01-01-2017` (string, required) - Účet úč. osnovy platný od
- visible: true (boolean) - Viditelnost účtu v seznamech

## `BANK_ACCOUNT_LIST` (object)
### Properties
- Include `BANK_ACCOUNT_READONLY`
- Include `BANK_ACCOUNT_BASE`


## `BANK_ACCOUNT_DETAIL` (object)
### Properties
- Include `BANK_ACCOUNT_READONLY`
- Include `BANK_ACCOUNT_BASE`
- Include `BANK_ACCOUNT_EXTRA`


## `BANK_ACCOUNT_PARAMS` (object)
### Properties
- Include `BANK_ACCOUNT_BASE`
- Include `BANK_ACCOUNT_EXTRA`


## `CASH_REGISTER` (object)
### Properties
- _link (object)
    - self (object)
        - href: /1.1/cash_register/475 (required)
- id: 475 (number, required) - ID účtu
- name: `EET pokladna` (string, required) - Název pokladny
- currency: CZK (enum, sample, required) - [**Měna**](#currency)
- isdefault: true (boolean, required) - Nastavena jako výchozí
- `initial_state`: 0 (number) - Počátenční stav pokladny
- iseet: true (boolean, required) - Zda je pokladna nastavená pro EET
- `business_premises` (`BUSINESS_PREMISES`, optional)  - [**Provozovna**](#business_premises)

## `BUSINESS_PREMISES` (object)
### Properties
- _link (object)
    - self (object)
        - href: /1.1/business_premises/897 (required)
- id: 897 (number, required) - ID provozovny
- name: `Restaurace u koníčka` (string, required) - Název provozovny
- isdefault: true (boolean) - Nastavena jako výchozí
