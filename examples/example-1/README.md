# cli-examples - Example 1

`.motaword` directory contains your configuration files for `motaword` CLI.

You can automatically create a new profile for authentication via `./bin/motaword auth add-profile`. The result of this command will be put inside `.motaword` directory.

## Explanation
`.motaword/config.json` demonstrates these features:

- source directory
- translations directory does not have locale
- rules have `{locale}` parameter
- rules can specify all files with specified extensions
- rules can change extension by using `{filename}` instead of `{basename}`
- exclusion rule

```json
{
  "source-directory": ".example-files/en",
  "translations-directory": ".example-files",
  "rules": {
    "pot/json/*.pot": "{locale}/{path}/{filename}.po",
    "pot/*.pot": "{locale}/{path}/{basename}",
    "md/*.md": "{locale}/{path}/{filename}.md"
  },
  "exclude-rules": [
    "pot/languages*"
  ]
}
```