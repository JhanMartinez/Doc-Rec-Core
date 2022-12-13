# `EventTools.py`

Contiene los decoradores (funciones de alto orden) que son usados en los handlers para formatear u obtener información antes y/o después de ejecutar la lógica de negocio. Contiene también funciones útiles para crear nuevos decoradores.

## Funciones

1. **`get_lambda_event_source.`** Con el evento recibido en el handler, obtiene la fuente que ejecuta la lambda. Por ejemplo la fuente puede ser un bucket, otra lambda, un sqs, una api gateway.

## Decoradores

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

[Volver a inicio](README.md)
