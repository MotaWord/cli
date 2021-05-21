# cli-examples - Example 5

`.motaword` directory contains your configuration files for `motaword` CLI.

You can automatically create a new profile for authentication via `./bin/motaword auth add-profile`. The result of this command will be put inside `.motaword` directory.

## Explanation
`.motaword/config.json` demonstrates these features:

- source directory
- translations directory have `{locale}` parameter
- `*` does not include subdirectories, for that, you need to specify something like `pot/**/*.pot`

```json
{
  "source-directory": "src/main/resources/en",
  "translations-directory": "src/main/resources/{locale}",
  "rules": {
    "pot/*.pot": "{path}/{basename}",
    "md/*.md": "{path}/{filename}.md"
  },
  "exclude-rules": [
    "pot/languages*"
  ]
}
```