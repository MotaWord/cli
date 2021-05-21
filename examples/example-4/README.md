# cli-examples - Example 4

`.motaword` directory contains your configuration files for `motaword` CLI.

You can automatically create a new profile for authentication via `./bin/motaword auth add-profile`. The result of this command will be put inside `.motaword` directory.

## Explanation
`.motaword/config.json` demonstrates these features:

- all language resources are in a subdirectory called `src/main/resources`, more like a Java environment
- source directory
- translations directory have `{locale}` parameter
- rule specifies "everything" with `**/*`
- multiple exclusion rules, with directory exclusion

```json
{
  "source-directory": "src/main/resources/en",
  "translations-directory": "src/main/resources/{locale}",
  "rules": {
    "**/*": "{path}/{basename}"
  },
  "exclude-rules": [
    "md/excluded-pot/*",
    "pot/languages*"
  ]
}
```