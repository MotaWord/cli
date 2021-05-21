# cli-examples - Example 6

`.motaword` directory contains your configuration files for `motaword` CLI.

You can automatically create a new profile for authentication via `./bin/motaword auth add-profile`. The result of this command will be put inside `.motaword` directory.

## Explanation
`.motaword/config.json` demonstrates these features:

- source directory
- translations directory have `{locale}` parameter
- rules include all subdirectories and files with .pot and .md extensions

```json
{
  "source-directory": "src/main/resources/en",
  "translations-directory": "src/main/resources/{locale}",
  "rules": {
    "pot/**/*.pot": "{path}/{basename}",
    "md/**/*.md": "{path}/{basename}"
  },
  "exclude-rules": [
    "pot/languages*"
  ]
}
```