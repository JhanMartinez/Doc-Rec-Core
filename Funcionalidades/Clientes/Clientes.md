# Clientes

Esta funcionalidad permite registrar, mostrar, actualizar, eliminar la información relacionada al propietario del crédito.

## Métodos

### def `create_customer( ):`

Permite la manipulación de la información relacionada al cliente con la finalidad de registrarlo en base de datos.

argumentos:

```json
{
    "customer_type": 1,
    "ciiu_code": "1234",
    "place_of_birth": 43002,
    "first_name": "critiano",
    "marital_status": 1,
    "middle_name": "livan",
    "first_lastname": "oliva",
    "second_lastname": "castro",
    "birthdate": "2001-05-15",
    "gender": 1,
    "email": "imperiootomano@gmail.com",
    "education_level_id":1,
    "mobile_phone": 3214545,
    "home_phone": 3214545,
    "document": {
        "document_number": "323232222222",
        "document_type_id": 2,
        "date_origin_document": "2019-05-20",
        "city_origin_document": 8139
    },
    "address": {
        "socioeconomic_strata": 1,
        "housing_type_id": 1,
        "city_id": 30096,
        "state_id": 3486,
        "country_id": 216,
        "address_line_1": "Calle 80 # 90 - 90",
        "address_line_2": "calle 90 # 78 - 78"
    },
    "financial": {
        "total_income": 150000000,
        "total_expenses": 8000000,
        "total_assets": 1000000000,
        "total_equity": 400000000,
        "total_liabilities": 100000000,
        "other_income": 2000000
    },
    "bank_info": {
        "bank_id": 32,
        "account_number": "-",
        "account_type": 3
    },
    "labors": {
        "city_of_company": 1,
        "occupation": 1,
        "salary": 0,
        "public_resource": true,
        "political_exposed": false,
        "company": "-",
        "company_type_id": 1,
        "company_address": "-",
        "company_phone": "-",
        "employee_incorporation_date": "2019-05-20",
        "company_identification": "e212121212121",
        "employment_contract_type": 3,
        "company_email": "imperiootomano@gmail.com"
    },
    "custom_fields": {}
}
```

## Atributos

`customer_type` (integer):
Tipo de cliente a registrar:

* cliente natural
* cliente jurídico

**`ciiu_code`** (obligatorio cliente jurídico):
Actividad comercial de la persona jurídica

**`place_of_birth`** (opcional):
Contiene un entero con el Lugar de nacimiento

**`first_name`** (obligatorio):
Contiene un string con la información del primer nombre

**`martital_status`** (opcional):
contiene un entero con la información referente al estado civil

* soltero
* unión libre
* casado
* divorciado
* separado
* viud(a)

**`middle_name`** (opcional):
Contiene un string con la información del segundo nombre

**`first_lastname`** (obligatorio):
Contiene un string con la información del primer apellido

**`second_lastname`** (opcional):
Contiene un string con la información del segundo apellido

**`Birthdate`** (obligatorio):
Contiene un String de tipo date la información referente a la fecha de nacimiento

**`gender`** (obligatorio):
Contiene un entero con la información del género:

* masculino
* Femenino

**`email`** (obligatorio):
Contiene un string con la información relacionada al correo electrónico

* education_level_id (opcional):
* Contiene un integer con la información relacionada al nivel educativo
* primaria
* bachiller
* técnico
* tecnólogo
* universitario
* especialización
* maestría
* doctorado
