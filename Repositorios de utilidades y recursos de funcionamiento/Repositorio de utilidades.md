# Repositorio de utilidades y recursos de funcionamiento 

## common

 Este es el nombre del repositorio que contiene las utilidades y recursos, dentro de este tenemos una carpeta llamda **Utils** la cual contiene subcarpetas tales como: Auth, Http y archivos de recursos.

```txt
Common                           
    |__Utils                     
         |__Auth
         |    |__Authorization
         |__Http
         |
         | # Archivos de recursos
         |
         |__init__.py
         |__AuthGroupV2
         |__BaseModel.py
         |__CalculationTools.py
         |__CognitoManager.py
         |__CompanyCode.py
         |__CompanyTools.py
         |__EncrytionBase.py
         |__EndpointList.py
         |__EventTools.py
         |__ExcelManager.py
         |__ExceptionsTools.py
         |__ExternalRequest.py

```

## Auth

## Http

Carpeta que guarda una clase para manejar los statusCode a través de código.
Presenta utilidades como:
    **Validar si un statusCode es positivo**: Al usarlo de la manera StatusCode({statusCode}) se devuelve el resultado como un booleano.

## Archivos de recursos

### `__init__.py`

Este archivo se encarga de procesar los objetos json dump teniendo en cuenta los métodos establecidos, tales como:

* `__json__` : Este método convierte un objeto clase a un json serializable.
  
* `__as_dict__` : Este método permite convertir un objeto de tipo clase en un diccionario de python.
  
* `__dict__` : Este método almacena los atributos de lectura de un objeto

### Función  wrapped_default

Esta función convierte objetos en diccionarios, de otra manera si el objeto es un decimal lo convertirá en flotante.

#### Parametros

#### Estructura de la funcion wrapped_default

```python
def wrapped_default
    # Falta lo demas 
```

### `AuthGroupV2.py`

Esta es una clase para el manejo de los grupos de permisos. Contiene la lógica del servicio con ruta `/auth_group`. A continuación se describen los métodos principales de la clase.

#### Métodos

1. **`create_auth_group.`** Se encarga de crear un grupo de permisos asociando varios permisos (`function_id`'s de la tabla `core.functions`). Estos pueden ser posteriormente asociados a un perfil con el servicio `/profile`.

2. **`update_auth_group.`** Se encarga de actualizar los permisos (`function_id`'s de la tabla `core.functions`) pertenecientes al grupo que se especifica.

3. **`get_auth_groups.`** Obtiene los grupos de permisos y la descripción de cada permiso que compone el grupo.

4. **`verify_function_auth.`** Se encarga de verificar si el grupo de permisos contiene un permiso específico (`function_id`).

5. **`delete_auth_group.`** Elimina un grupo de permisos. (Se marca como active = 0 en la tabla).

### Cognito Manager

Esta clase se encarga de crear, actualizar y deshabilitar un usuario en AWS Cognito.

#### Métodos

1. **`create_user.`** Crea un nuevo usuario en el pool de usuarios de cognito con un username, password, email y company code.

2. **`disable_user.`** Deshabilita un usuario existente en el pool de usuarios de cognito con un username.

3. **`enable_user.`** Habilita un usuario existente en el pool de usuarios de cognito con un username.

4. **`update_password.`** Actualiza la contraseña de un usuario del pool de cognito usando su username y la nueva contraseña.

5. **`get_tokens.`** Obtiene los tokens de cognito (access token, id token y refresh token) con un username y contraseña.

6. **`refresh_tokens.`** Regenera los tokens de cognito (access token, id token) al recibir un refresh token.

### Company code

Esta clase se encarga de generar un identificador único para las empresas con el cual el usuario es identificado. Hace referencia a la tabla `core.company_codes`.

#### Métodos

1. **`get_code_info.`** Obtiene información relacionada a la empresa usando el `company_code` o el `company_id`(id de la empresa). Ej: secreto de la empresa, bucket de la empresa.

2. **`create_company_code.`** Genera el código único de la empresa y lo guarda en la tabla `core.company_codes`.

### `CalculationTools.py`

Esta es un conjunto de funciones esenciales para cálculo de fechas y utilidades de diccionarios. Incluye las siguientes funciones:

#### Funciones

1. **`add_months.`** Toma una fecha como parámetro y le suma el número de meses especificados. Puede ser usada también para convertir fechas a formato de días 360-365.

2. **`days360.`** Corresponde a la función de excel de dias360.

3. **`str_to_date.`** Castea la fecha recibida como `string` a tipo de dato `date`. Así mismo, recibe como segundo argumento el formato a validar para la fecha recibida como `string` el cual es por defecto `%Y-%m-%d`.

4. **`str_to_datetime.`** Castea la fecha recibida como `string` a tipo de dato `datetime`. Así mismo, recibe como segundo argumento el formato a validar para la fecha recibida como `string` el cual es por defecto `%Y-%m-%d %H:%M:%S`.

5. **`update_and_add_dict.`** Actualiza un diccionario con el conjunto de llaves pasadas como argumento. En caso de que la llave exista, el valor es sumado al original. **Debe usarse solo para valores numéricos.**

6. **`month_diff.`** Calcula el número de meses de diferencia entre dos fechas.

7. **`day_diff.`** Calcula el número de días de diferencia entre dos fechas.

8. **`extract_from_date.`** Extrae el día, mes o año de la fecha recibida.

9. **`calculate_age.`** Calcula la edad de acuerdo a la fecha de nacimiento recibida.

10. **`cast_dict_to_int.`** Convierte todos los valores de un diccionario a entero.

11. **`cast_dict_decimal_to_float.`** Convierte todos los valores de un diccionario a float siempre y cuando sean decimales.

### `RedisManager.py`

Clase utilizada para guardar o consultar datos almacenados en caché. A continuación se mencionan los métodos principales:

#### Métodos

1. **`set_prefix.`** Asigna un prefijo a las llaves que serán generadas para asociar los datos en la base de datos de redis. En la clase `DataBase.py` se asigna como el nombre de la tabla a la que se consulta.

2. **`_generate_key.`** Genera la llave con la cual se guarda el registro en el servidor de redis. La llave es generada de acuerdo a la consulta realizada a la base de datos y un prefijo previamente asignado, que por lo general corresponde al nombre de la tabla. Ej:

```python
from sqlalchemy import select
# El statement a continuación:
stmt = select(LoanModel).filter_by(active=1, loan_id=3)
# Generaría una llave como la siguiente:
key = "loans-active:1-loan_id:3"
```

1. **`get_dict.`** Obtiene el valor almacenado en caché de acuerdo al conjunto de llaves pasados como argumento.

### `EndpointList.py`

Esta clase se encarga de asociar un endpoint a una operación del sistema. Una operación del sistema es un proceso interno que es común en el flujo de las empresas, por ejemplo, la contabilidad.

#### Métodos

1. **`get_endpoints.`** Consulta los endpoints para todas las operaciones de sistema definidas para una empresa.

2. **`save_endpoint.`** Asocia un nuevo endpoint a una operación del sistema.

3. **`delete_endpoint.`** Elimina una asociación de endpoint a una operación del sistema.

4. **`get_endpoint_url.`** Consulta el endpoint a partir de una operación del sistema(`system_operation_id`).

### `EventTools.py`

Contiene los decoradores (funciones de alto orden) que son usados en los handlers para formatear u obtener información antes y/o después de ejecutar la lógica de negocio. Contiene también funciones útiles para crear nuevos decoradores.

#### Funciones

1. **`get_lambda_event_source.`** Con el evento recibido en el handler, obtiene la fuente que ejecuta la lambda. Por ejemplo la fuente puede ser un bucket, otra lambda, un sqs, una api gateway.

#### Decoradores

1. **`authorized.`** Valida si el usuario que consume un servicio tiene permiso de ejecución. Para esto el flujo es el siguiente:

    ```txt
    - Se extrae la información del token
    - Se setean las conexiones a base de datos
    - Se verifican los permisos con la clase Authorization.
    - Se ejecuta la lógica de negocio.
    - Se devuelve la respuesta formateada.
    ```

    ```python
    from Calculate.Classes.CostCalculation import CostCalculation
    from Utils.EventTools import authorized
    from Utils.LambdaTools import not_implemented


    @authorized
    def calculate_costs(event, context, conn):

        cc = CostCalculation(conn)
        methods = {
            'POST': cc.get_loan_costs
        }

        # Select the method to be executed based on the user's request.
        method_to_be_executed = methods.get(event['httpMethod'], not_implemented)

        return method_to_be_executed(event)

    ```

2. **`response_format.`** Este decorador formatea la respuesta retornada para la API Gateway.

    ```python
    from Utils.GeneralTools import get_post_data
    from Utils.CognitoManager import CognitoManager
    from Utils.EventTools import response_format
    from os import environ as os_environ


    @response_format
    def cognito_token(event, context):
        """Handler / function for get the token of Cognito service of AWS"""
        client_id = os_environ.get("COGNITO_CLIENT_ID", "")
        cognito = CognitoManager(client_id)
        body = get_post_data(event)
        return cognito.get_tokens(**body)
    ```

3. **`sqs_execution.`** Se usa para extraer el evento en el handler cuando una lambda es ejecutada por una cola AWS SQS.

4. **`lambda_execution.`** Se usa para extraer el evento en el handler cuando una lambda es ejecutada desde otra lambda. (Para ejecuciones desde otra lambda se recomienda utilizar la clase LambdaTools)

5. **`s3_execution.`** Se usa para extraer el evento en el handler cuando una lambda es ejecutada por un AWS S3 Bucket.

6. **`api_execution.`** Se usa para extraer el evento en el handler cuando una lambda es ejecutada por un AWS API Gateway.

7. **`multi_source.`** Identifica automáticamente la fuente de ejecución de la lambda y se encarga de validar permisos de ejecución. Sirve para cuando una lambda puede tener varias fuentes de ejecución, por ejemplo, ser ejecutada por una cola SQS o por la API Gateway. Tiene el parámetro `authorized_` para definir si se valida o no permisos de ejecución y el parámetro `alt_http_method` que define el método http (por defecto POST) que debe ejecutarse cuando la lambda es consumida por una fuente diferente a la API Gateway.
  
    ```python
    from Product.Classes.ProductCalculation import ProductCalculation
    from Utils.EventTools import multi_source
    from Utils.LambdaTools import not_implemented


    @multi_source(alt_http_method="PUT")
    def product_disbursement_range(event, context, conn):

        product_calculation = ProductCalculation(conn)
        methods = {
            "PUT": product_calculation.update_disbursement_range,
        }

        # Select the method to be executed based on the request.
        method_to_be_executed = methods.get(event['httpMethod'], not_implemented)

        return method_to_be_executed(event)

    ```

