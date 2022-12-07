# Repositorio de utilidades y recursos de funcionamiento 

## common

 Este es el nombre del repositorio que contiene las utilidades y recursos, dentro de este tenemos una carpeta llamda **Utils** la cual contiene subcarpetas tales como: Auth, Http y archivos de recursos.

```txt
Common                           
    |__Utils                     
         |__Auth
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

### AuthGroupV2

Esta es una clase para el manejo de los grupos de permisos. Contiene la lógica del servicio con ruta `/auth_group`. A continuación se describen los métodos principales de la clase.

### Métodos

1. **`create_auth_group.`** Se encarga de crear un grupo de permisos asociando varios permisos (`function_id`'s de la tabla `core.functions`). Estos pueden ser posteriormente asociados a un perfil con el servicio `/profile`.

2. **`update_auth_group.`** Se encarga de actualizar los permisos (`function_id`'s de la tabla `core.functions`) pertenecientes al grupo que se especifica.

3. **`get_auth_groups.`** Obtiene los grupos de permisos y la descripción de cada permiso que compone el grupo.

4. **`verify_function_auth.`** Se encarga de verificar si el grupo de permisos contiene un permiso específico (`function_id`).

5. **`delete_auth_group.`** Elimina un grupo de permisos. (Se marca como active = 0 en la tabla).