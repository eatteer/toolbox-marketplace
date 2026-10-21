# Before / After Examples

## Spanish: delete flow

Before:

```
Botón:         Borrar Archivo
Confirmación:  ¿Está seguro de que desea eliminar el archivo?
Botones:       Sí / Cancelar
Éxito:         ¡El documento fue suprimido exitosamente!
```

After:

```
Botón:         Eliminar archivo
Confirmación:  ¿Eliminar este archivo? No podrás recuperarlo.
Botones:       Eliminar / Cancelar
Éxito:         Archivo eliminado
```

- One verb (`eliminar`) across the whole flow, and one noun (`archivo`).
- One form of address (`tú`), the same as the rest of the app.
- The confirm button repeats the action instead of a generic `Sí`.

## English: settings screen

Before:

```
Account Settings
  Edit Profile
  Change your Password
  Manage Notifications.
  Remove Account
[Save Changes]   [Discard]
Settings Updated Successfully!
```

After:

```
Account settings
  Edit profile
  Change password
  Manage notifications
  Delete account
[Save changes]   [Discard changes]
Changes saved
```

- Sentence case everywhere, no period on menu items.
- Parallel menu items (verb + object) without a stray "your".
- `Delete account`, because "remove" is used elsewhere for taking items out of a list.
- The success message repeats the button's terms (`changes`, `saved`).

## Spanish: list with a real difference in meaning

```
Quitar de favoritos     → the product stays in the catalog
Eliminar producto       → the product is gone for good
```

Two verbs are correct here because the actions are different. Each one is still used
only for its own meaning, everywhere in the app.

## Code: i18n strings

Before:

```json
{
  "addUser": "Añadir Usuario",
  "newClient": "Agregar cliente",
  "userCreated": "¡Usuario Adicionado!",
  "error": "Error"
}
```

After:

```json
{
  "users.create.button": "Agregar usuario",
  "users.create.success": "Usuario agregado",
  "users.create.error": "No pudimos agregar el usuario. Revisa los datos e intenta de nuevo."
}
```

- One verb (`agregar`) and one noun (`usuario`; `cliente` was the same concept with another name).
- Keys describe the meaning, grouped by feature.
- The error says what happened and how to fix it.
