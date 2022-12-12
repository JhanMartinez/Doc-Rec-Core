# Company code

Esta clase se encarga de generar un identificador único para las empresas con el cual el usuario es identificado. Hace referencia a la tabla `core.company_codes`.

## Métodos

1. **`get_code_info.`** Obtiene información relacionada a la empresa usando el `company_code` o el `company_id`(id de la empresa). Ej: secreto de la empresa, bucket de la empresa.

2. **`create_company_code.`** Genera el código único de la empresa y lo guarda en la tabla `core.company_codes`.

[Volver a inicio](Repositorio%20de%20utilidades.md)