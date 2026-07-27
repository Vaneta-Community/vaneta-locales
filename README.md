# Vaneta language files

Every language [Vaneta](https://vaneta.xyz) speaks lives in this repo as a
folder of JSON files, named after its language code — `en/`, `tr/`, and so on.
**No code required**: a language is only text, so anyone can translate Vaneta
by copying the `en/` folder and translating the values.

`en/` is the source of truth. Anything a translation doesn't cover simply falls
back to English, so partial translations are fine — you can start with a single
file and grow the folder over time.

Merged translations are synced into the main bot automatically and go live in
Vaneta's next release — no separate release process to wait on.

## Folder structure

```
en/
  meta.json          # the language's native name
  messages/          # everything the bot says, split by domain
    common.json
    errors.json
    commands.json    # response texts of commands
    events.json      # response texts of events (welcomes, tickets, …)
  commands/          # slash command descriptions, split by category
    bot.json
    fun.json
    info.json
    moderation.json
    module.json
    utility.json
    voice.json
```

- **`meta.json`** — `{ "name": "English" }`: the language's native name, shown
  in `/config language`.
- **`messages/<domain>.json`** — the bot's response texts. Each file becomes one
  branch of the translation tree; the file name must match the English one
  exactly.
- **`commands/<category>.json`** — the slash command names and descriptions
  shown in Discord's command picker, keyed by full command name:

  ```json
  {
    "tag create": {
      "description": "Creates a tag.",
      "options": {
        "name": { "description": "The name of the tag." }
      }
    }
  }
  ```

## Adding a new language

1. Copy the `en/` folder to `<code>/`, where `<code>` is the two-letter
   [ISO 639-1](https://en.wikipedia.org/wiki/List_of_ISO_639_language_codes)
   language code (`de`, `fr`, `es`, `pt`, …). Use the plain language code even
   for languages Discord regionalises — a `pt/` folder automatically covers
   Discord's `pt-BR`, and `es/` covers `es-ES` and `es-419`.
2. Set `meta.json`'s `name` to the language's native name (e.g. `"Deutsch"`).
3. Translate the values. Never translate the keys or rename the files.
4. Open a pull request with your folder.

## Rules

- **`{placeholders}`** — values like `{member}` or `{count}` are filled in by
  the bot at runtime. Keep them exactly as they are in the English files (don't
  translate the word inside the braces), but move them around freely so the
  sentence reads naturally in your language.
- **Markdown** — texts may contain Discord markdown (`**bold**`, `` `code` ``)
  and mentions (`<@…>`). Keep the formatting, translate the words.
- **Arrays** — lists like the 8ball answers are response pools the bot picks
  from at random. Your language can have more or fewer entries than English.
- **Command descriptions** — must be 1–100 characters. Command and option
  `name`s must be lowercase and 1–32 characters with no spaces (Discord's
  rules); when in doubt, only translate `description`s and leave names alone.

## Validating your changes

This repo only holds the language files, so there's no local build to run
them against. Once your pull request merges, it's automatically synced into
the main bot repo, where the same checks that run in Vaneta's own test suite
validate it — unknown keys, broken placeholders, over-long command
descriptions, and untranslated keys are all caught there. If something's
wrong, we'll follow up on your PR or open a fix.
