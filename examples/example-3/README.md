# cli-examples - Example 3

`.motaword` directory contains your configuration files for `motaword` CLI.

You can automatically create a new profile for authentication via `./bin/motaword auth add-profile`. The result of this command will be put inside `.motaword` directory.

## Explanation
`.motaword/config.json` demonstrates these features:

- all language resources are in a subdirectory called `resources`
- source directory
- translations directory have `{locale}` parameter
- rule specifies "everything" with `*`, an alternative to `**/*`
- multiple exclusion rules, with directory exclusion

```json
{
  "source-directory": "resources/en",
  "translations-directory": "resources/{locale}",
  "rules": {
    "*": "{path}/{basename}"
  },
  "exclude-rules": [
    "md/excluded-pot/*",
    "pot/languages*"
  ]
}
```