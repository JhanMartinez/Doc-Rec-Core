# `RedisManager.py`

Clase utilizada para guardar o consultar datos almacenados en caché. A continuación se mencionan los métodos principales:

## Métodos

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

[Volver a inicio](Repositorio%20de%20utilidades.md)