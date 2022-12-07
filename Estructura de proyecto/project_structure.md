# Estructura de proyecto

Los proyectos se estructuran generalmete de la siguiente manera:

```code
handlers                     // carpeta principal
    |__handler.py            // archivo manejador de la lambda
serverless.yml               // definición de funciones y recursos del proyecto
Classes
    |__Clase.py              // archivo con la lógica de negocios de la lambda
Models
    |__model.py              // modelo de base de datos con la estructura de sqlalchemy
.env                         // archivo de definición de credenciales
requirements-lock.txt        // archivo de definición de dependencias de python del repositorio
script.py                    // archivo de generación de requerimientos (requirements.txt)
setup.py                     // archivo de configuración del repositorio para ser importado como librería
pyproject.toml               // archivo de configuración del repositorio para ser importado como librería
```

## Flujo

```txt
serverless.yml --> handlers/handler.py --> Classes --> Models
```

## Serverless.yml

En este archivo, se encuentra la llave functions correspondiente a la definición de las lambdas.
En la definición de la lambda se define la ubicación del handler.

```yaml
functions:
    name: test-function
    handler: handlers/ClosureHandler.closure  # Ubicación del handler
    events:
    - http:
        path: /test
        method: post
```

La estructura de un handler comprende la siguiente lógica:

```python
# - Importación de la clase con la lógica de negocios
from Closure.Classes.Closure import Closure
# - Importación de funciones de alto orden (decoradores)
from Utils.EventTools import authorized
# - Imports requeridos de acuerdo a la lógica (varían por lambda)
from Utils.Auth.Authorization import TokenTools
from DataBase.db import Database

@authorized  # Se coloca el decorador para verificar siempre permisos de ejecición
def closure(event, context, conn):
    """Handler to make portfolio loans closure"""
    closure = Closure(conn)  # Instancia lógica de negocios

    # Se define la lógica por cada uno de los métodos http definidos en el serverless.yml
    methods = {
        "GET": closure.list_monthly_closures
    }

    # Lógica extra de acuerdo a la función
    tt = TokenTools(db=conn, company_id=event["company_id"])
    event["company_code"] = tt.company_info["code"]
    # Select the method to be executed based on the request.
    method_to_be_executed = methods.get(event['httpMethod'])

    # Se retorna la ejecución del método definido para el evento http
    return method_to_be_executed(event)
```

## Estructura de una clase

```python
# Librerías de manejo de base de datos
from sqlalchemy import select, insert, update
from datetime import date
# Utilidades
from Utils.TypingTools import ConnType, EventType, APIResponseType
from Utils.Validations import Validations, DATE_TYPE
from Utils.GeneralTools import get_input_data
from Utils.CalculationTools import extract_from_date, add_months, str_to_date
from Utils.ExceptionsTools import CustomException
# Constantes
from Constants.ErrorMessages import ERROR_MESSAGE_GENERIC
# Modelos de base de datos
from Closure.Models.ProductMonthlyClosure import (
    ProductMonthlyClosureModel, PENDING_S, CLOSED_S)
from Product.Models.Product import ProductModel
from Payments.Models.TodayDate import TodayDateModel
from User.Models.User import UserModel


class Closure:
    """Class responsible for handling the products monthly closures"""

    __periods = [
        "01", "02", "03", "04", "05", "06", "07", "08", "09", "10", "11", "12"
    ]

    def __init__(self, db: ConnType, product_id: int = None):
        """Class responsible for handling the products monthly closures"""
        self.__db = db
        self.validate = Validations(db)
        if type(product_id) is int:
            self.__product_id = product_id

    # Ejemplo de método GET que ejecuta el handler
    def list_monthly_closures(self, event: EventType) -> APIResponseType:
        """
        It lists monthly closures for a product

        Args:
          event (EventType): EventType

        Returns:
          A list of monthly closures for a product.
        """
        # 1. Se extrae la información de la petición a partir del event
        request = get_input_data(event)

        # 2. Se valida los parámetros requeridos para la lógica de negocios
        product_id = request["product_id"]

        val_result = Validations.validate([
            Validations.param('product_id', int, product_id)
        ])

        stmt = select(ProductModel).filter_by(
            product_id=product_id
        )
        product_exists = self.__db.read_session.query(stmt).first()

        if (not val_result["isValid"]) or (not product_exists):
            raise CustomException(ERROR_MESSAGE_GENERIC["generic400"])

        self.__product_id = product_id

        # 3. Ejecutar la lógica de negocios
        data = self.fetch_monthly_closures(self.__product_id)
        data = self.fill_product_closure_periods(data)

        return APIResponseType(statusCode=200, data=data)
```

## Estructura de requirements-lock.txt

### Añadir dependencias de python al repositorio

Ejemplo de `requirements-lock.txt`:

```txt
# Dependencias comunes
XlsxWriter==3.0.3
xlwings==0.25.0
xmltodict==0.12.0

# Dependencias de repositorio siguen la estructura:
git+https://USERNAME:PASSWORD@{REPOSITORY_URL}.git#egg={MODULE_NAME}
```

## Estructura de setup.py

```python
import setuptools

# Obtener credenciales a partir del .env
def get_environment():

"""
Construir el requirements.txt a partir del requirements-lock.txt
Reemplazando las variables USERNAME y PASSWORD
"""
def get_requirements(filename="requirements.txt", write=False):


if __name__ == "__main__":

    requirements = get_requirements()

    setuptools.setup(
        name="core-closure", # Este correspondería al nombre del módulo (MODULE_NAME) en el requirements-lock.txt
        version="0.0.1",
        author="Core",
        author_email="red5g@red5g.co",
        url="https://bitbucket.org/red5g-admin/redcore-closure/src/master/",  # Url del repositorio
        project_urls={
            "Bug Tracker": ("https://Sistemas22@bitbucket.org/red5g-admin/"
                            "redcore-closure.git"), # Url para clonar el repositorio (REPOSITORY_URL en el requiremets-lock.txt)
        },
        classifiers=[
            "Programming Language :: Python :: 3",
            "License :: Other/Proprietary License",
            "Operating System :: OS Independent",
        ],
        """
        El package dir debe ser armado de la siguiente manera para las
        carpetas que se quieran incluir en el repo como librería:

        NombreRepo.CarpetaPrincipal: Ruta desde la raiz

        """
        package_dir={
            'Closure.Classes': 'Classes',
            'Closure.Models': 'Models'
        },
        """
        La llave packages debe incluir las carpetas que se exportarán al momento de importar la librería (llaves del diccionario de package_dir)
        """
        packages=['Closure.Classes', 'Closure.Models'],
        python_requires=">=3.8",
        install_requires=requirements
    )
```

La configuración anterior de la llave **package_dir** indica que al momento de requerir el repositorio como librería, la librería tendría la siguiente estructura:

```txt
Closure                     // carpeta principal
     |
    Classes
        |__Closure.py
    Models
        |__Closure.py
```

Se termina incluyendo las carpetas indicadas dentro de una carpeta más que lleva el nombre del repositorio.

Entonces al momento de usarla en otro repo, se usaría de la siguiente manera:

```python
from Closure.Classes.Closure import Closure

closure = Closure()
```

## Generación de requirements.txt

Para generar el requirements.txt en caso de querer instalar localmente las librerías, se ejecuta el siguiente comando:

```cmd
$ python3 script.py
```