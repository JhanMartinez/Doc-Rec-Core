# `CalculationTools.py`

Esta es un conjunto de funciones esenciales para cálculo de fechas y utilidades de diccionarios. Incluye las siguientes funciones:

## Funciones

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

[Volver a inicio](Repositorio%20de%20utilidades.md)