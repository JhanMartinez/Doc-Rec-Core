# `AuthGroupV2.py`

Esta es una clase para el manejo de los grupos de permisos. Contiene la lógica del servicio con ruta `/auth_group`. A continuación se describen los métodos principales de la clase.

## Métodos

1. **`create_auth_group.`** Se encarga de crear un grupo de permisos asociando varios permisos (`function_id`'s de la tabla `core.functions`). Estos pueden ser posteriormente asociados a un perfil con el servicio `/profile`.

2. **`update_auth_group.`** Se encarga de actualizar los permisos (`function_id`'s de la tabla `core.functions`) pertenecientes al grupo que se especifica.

3. **`get_auth_groups.`** Obtiene los grupos de permisos y la descripción de cada permiso que compone el grupo.

4. **`verify_function_auth.`** Se encarga de verificar si el grupo de permisos contiene un permiso específico (`function_id`).

5. **`delete_auth_group.`** Elimina un grupo de permisos. (Se marca como active = 0 en la tabla).

## Cognito Manager

Esta clase se encarga de crear, actualizar y deshabilitar un usuario en AWS Cognito.

### Métodos

1. **`create_user.`** Crea un nuevo usuario en el pool de usuarios de cognito con un username, password, email y company code.

2. **`disable_user.`** Deshabilita un usuario existente en el pool de usuarios de cognito con un username.

3. **`enable_user.`** Habilita un usuario existente en el pool de usuarios de cognito con un username.

4. **`update_password.`** Actualiza la contraseña de un usuario del pool de cognito usando su username y la nueva contraseña.

5. **`get_tokens.`** Obtiene los tokens de cognito (access token, id token y refresh token) con un username y contraseña.

6. **`refresh_tokens.`** Regenera los tokens de cognito (access token, id token) al recibir un refresh token.

[Volver a inicio](README.md)
