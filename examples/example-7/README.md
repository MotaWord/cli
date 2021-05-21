# cli-examples - Example 7

`.motaword` directory contains your configuration files for `motaword` CLI.

You can automatically create a new profile for authentication via `./bin/motaword auth add-profile`. The result of this command will be put inside `.motaword` directory.

## Explanation
`.motaword/config.json` demonstrates these features:

- source directory
- translations directory have `{locale}` parameter
- rules include all files in current (relative to source directory) and all subdirectories
- locale code mapping between your directories and MotaWord project's language codes

```json
{
  "source-directory": "src/main/resources/en",
  "translations-directory": "src/main/resources/{locale}",
  "rules": {
    "**/*": "{path}/{basename}"
  },
  "exclude-rules": [
    "pot/languages*"
  ],
  "locales-map": {
    "fr-FR": "fr"
  }
}
```