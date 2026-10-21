---
name: copy-style
description: Write consistent user-facing text - sentence case instead of Title Case, one term per concept (never "add" here, "insert" there and "append" elsewhere), and the same tone, verb form and punctuation everywhere. Use this skill whenever you write or edit copy in any language - UI labels, buttons, headings, menus, error and empty-state messages, notifications, emails, docs or strings inside code - even if the user doesn't explicitly ask for consistency.
---

# Copy style

The goal: every piece of text should look like it was written by the same person, on the same
day, following the same rules. Readers notice inconsistency long before they notice good writing,
and every variation makes them wonder whether two words mean two different things.

## 0. First, respect the project

Before writing any text:

- If the project has a style guide, glossary or design system with copy rules, it wins.
- Look at the existing copy: translation files (`*.json`, `*.po`, `*.yaml`, `*.arb`,
  `*.strings`...), components, emails and docs. Reuse the terms, tone and patterns already there.
- If the existing copy is inconsistent (e.g. both "Agregar" and "Añadir"), use the most common
  variant and point out the inconsistency to the user. Don't silently add a third.
- This guide fills in the gaps the project leaves undefined.

## 1. Sentence case

Capitalize only the first word and proper nouns. This applies to headings, titles, buttons,
menu items, tabs, labels, placeholders, tooltips and table headers.

| Avoid                           | Prefer                          |
| ------------------------------- | ------------------------------- |
| `Hola, Cómo Estás`              | `Hola, cómo estás`              |
| `Crear Nueva Cuenta`            | `Crear nueva cuenta`            |
| `Configuración De Notificaciones` | `Configuración de notificaciones` |
| `Save Changes`                  | `Save changes`                  |
| `Forgot Your Password?`         | `Forgot your password?`         |

- Proper nouns, brand and product names keep their own spelling (`GitHub`, `iPhone`, `Google Drive`).
- Acronyms stay in capitals (`PDF`, `API`, `URL`).
- No ALL CAPS for emphasis or style. If it must look uppercase, use CSS (`text-transform`)
  so the source text stays in sentence case.
- In Spanish, days, months and languages are lowercase (`lunes`, `marzo`, `español`), and so is
  the word after a colon, except in a salutation (`Hola, Ana:`) or a quotation.

## 2. One term per concept

Pick one word for each concept and each action, and use it everywhere: buttons, titles,
messages, emails and docs.

Bad, in the same app:

```
[Agregar producto]        "Añade un comentario"        "Producto adicionado correctamente"
[Delete]                  "Remove this file?"          "The file was erased"
```

Good:

```
[Agregar producto]        "Agrega un comentario"       "Producto agregado"
[Delete]                  "Delete this file?"          "File deleted"
```

Rules:

- **Actions**: one verb per action (`agregar`, not `añadir` / `adicionar` / `insertar`).
- **Objects**: one noun per object (`usuario` or `cliente`, `pedido` or `orden`, `file` or `document`).
- **Opposites come in fixed pairs**, and the pair stays the same everywhere:
  `agregar / quitar`, `crear / eliminar`, `activar / desactivar`, `add / remove`,
  `create / delete`, `show / hide`, `enable / disable`, `sign in / sign out`.
- **The confirmation repeats the action's verb**: a button `Eliminar` leads to
  `¿Eliminar este archivo?` and then `Archivo eliminado`, not `¿Borrar...?` and `Archivo suprimido`.
- **Different words only for different meanings.** It's correct to use two terms when the
  actions really differ, e.g. `quitar` (removes it from a list, it still exists) vs `eliminar`
  (it's gone for good), or `archive` vs `delete`. Then use each one only for its own meaning.
- Don't use synonyms to "avoid repetition". In copy, repetition is clarity.

When you introduce a new term, check whether the project already has a word for that concept.

## 3. Consistent voice and form

- **One form of address**: `tú` or `usted` (`vos` if that is the project's choice), never mixed.
  In English, address the user as "you" consistently.
- **One possessive perspective**: `Tu perfil` or `Mi perfil`, not both.
- **Buttons and menu items use the same verb form**. In Spanish, the infinitive is the usual
  choice (`Guardar`, `Enviar`, `Crear cuenta`); don't mix it with imperatives (`Guarda`, `Envía`).
  In English, use the plain verb (`Save`, `Send`, `Create account`).
- **Specific labels over generic ones**: `Guardar cambios`, `Crear pedido` instead of `Aceptar`,
  `OK` or `Enviar` when the action can be named.
- **The same message type always has the same shape**:
  - Errors: what happened + how to fix it (`No pudimos guardar el archivo. Revisa tu conexión e intenta de nuevo.`).
  - Success: object + past participle, short (`Pedido creado`, `Changes saved`).
  - Empty states: what is missing + the action to start (`Aún no tienes pedidos. Crea el primero.`).

## 4. Consistent punctuation and formatting

- No final period on buttons, labels, headings, menu items or tabs.
- Full sentences (messages, descriptions, help text) end with a period. Choose one rule
  for single-sentence tooltips and keep it.
- Spanish questions and exclamations open and close: `¿Eliminar este archivo?`, `¡Listo!`.
- Avoid exclamation marks except in a truly celebratory moment.
- Use the ellipsis `…` only on actions that need more input before they run
  (`Exportar…` opens a dialog), and for progress (`Guardando…`). Use one form, `…` or `...`, everywhere.
- Same format everywhere for numbers, dates, times and currency, following the locale
  (`1.250,50 €` in es-ES, `$1,250.50` in en-US). Don't mix `21/10/2026` and `Oct 21, 2026`.
- Keep the spelling of technical terms consistent: `email` or `correo electrónico`,
  `login` or `inicio de sesión`, never both.

## 5. Copy inside code

- Strings in code follow the same rules: sentence case, the same terms, the same tone.
- If the project uses i18n, add new strings to the translation files. Don't hardcode them.
- Translation keys describe the meaning, not the text (`orders.create.button`, not `crear_pedido_ahora`).
- Don't concatenate fragments to build sentences (`"Tienes " + n + " pedidos"`); use the i18n
  library's interpolation and pluralization so every language can be correct.

## Checklist before delivering text

- [ ] Is everything in sentence case, except proper nouns and acronyms?
- [ ] Does each action and concept use a single term, the same one the project already uses?
- [ ] Are opposite actions in their fixed pairs, and do confirmations repeat the action's verb?
- [ ] Is the form of address (`tú` / `usted`) and the button verb form the same everywhere?
- [ ] Do errors, success messages and empty states follow the same shape?
- [ ] Are punctuation and number/date formats consistent?
- [ ] Did you flag any inconsistency you found in the existing copy?

For more examples, see `references/examples.md`.
