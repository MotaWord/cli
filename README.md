<br />
<p align="center">
  <a href="https://github.com/motaword/cli">
    <img src="https://dentycj2qhk72.cloudfront.net/new/images/logo_square.png" alt="Logo" width="80" height="80">
  </a>

<h3 align="center">motaword CLI</h3>

  <p align="center">
    Use MotaWord CLI to interact with your MotaWord account in many ways to manage your translation and localization needs.
    <br />
    <a href="https://motaword.readme.io"><strong>Explore the docs »</strong></a>
    <br />
    <br />
    <a href="https://github.com/motaword/cli/issues">Request Feature</a>
    ·
    <a href="https://github.com/motaword/cli/issues">Report Bug</a>
  </p>
</p>



## Table of Contents

- [About `motaword`](#about-the-project)
- [Getting Started](#getting-started)
   * [Download pre-built binary](#download-pre-built-binary)
   * [Install via Homebrew](#install-via-homebrew)
- [Usage](#usage)
   * [Command categories / parent commands](#command-categories---parent-commands)
   * [Subcommands](#subcommands)
   * [Command parameters](#command-parameters)
      + [Sending JSON payload](#sending-json-payload)
- [Configuration](#configuration)
- [Authenticating](#authenticating)
   * [TL;DR](#tl-dr)
   * [Authentication details](#authentication-details)
- [Translating](#translating)
   * [Configuring translations](#configuring-translations)
   * [Translation rules](#translation-rules)
   * [Available variables in rules](#available-variables-in-rules)
   * [Translation configuration examples](#translation-configuration-examples)
- [Debugging](#debugging)
- [Contact](#contact)



<!-- ABOUT THE PROJECT -->
## About `motaword`

[![motaword-cli screen shot][product-screenshot]](https://www.motaword.com)

Use MotaWord CLI to interact with your MotaWord account in many ways to manage your translation and localization needs.

You can use `motaword` locally or as part of your CI/CD for continuous localization of your software.

<!-- GETTING STARTED -->
## Getting Started

To get a local copy up and running follow these simple steps.

### Download pre-built binary

Use Releases in this GitHub repository and download the application for your respective platform. We release `motaword` for Linux, Windows and MacOS in various configurations.

Download here: [Releases](https://github.com/motaword/cli/releases)

### Install via bash installer

We provide an `installer.sh` bash script that you use with `curl` to install the latest version of `motaword` CLI in your working directory. You can find the `installer.sh` script in this repository.

You can install `motaword` by running this:
`curl -s https://i.jpillora.com/motaword/cli | bash`

### Install via Homebrew

Mac users can install `motaword` simply via homebrew:
1. Tap us
    ```sh
   brew tap motaword/homebrew-tap
    ```
2. Install `motaword`
    ```sh
   brew install motaword
    ```

Now you can run `motaword` anywhere in your system.


<!-- USAGE EXAMPLES -->
## Usage
### Command categories / parent commands
`motaword` allows you to access all aspects of MotaWord platform, from submitting files for translation to getting reports, from managing your glossary to style guides.

For all commands that `motaword` support, run:
```sh
> motaword help

Usage:
motaword [command]

Available Commands:
  async               Manage async operations
  auth                Authentication settings
  continuous-projects Manage your continuous projects
  debug               Debug your MotaWord configuration and environment
  documents           Manage documents
  get-access-token    Retrieve an access token to interact with the API.
  glossaries          Manage glossaries
  help                Help about any command
  help-config         Show CLI configuration help
  help-input          Show CLI input help
  projects            Manage projects
  reports             Manage reports
  search              Manage search operations
  static              Manage static endpoints
  stats               Manage stats
  strings             Manage strings
  styleguides         Manage styleguides
  translate           Instantly translate your content with your existing TM and MT resources. This is an alias to `continuous-projects translate`
  users               Manage users
  webhooks            Manage webhooks

Flags:
  -h, --help                   help for motaword
  -o, --output-format string   Output format [json, yaml] (default "json")
      --profile string         Credentials profile to use for authentication (default "default")
  -q, --query string           Filter / project results using JMESPath
      --raw                    Output result as raw rather than pretty JSON
      --server string          Override server URL
      --v                      Enable WARN level verbose log output
      --verbose                Enable DEBUG level verbose log output, same as --vvv flag
      --vv                     Enable INFO level verbose log output
      --vvv                    Enable DEBUG level verbose log output

Additional help topics:
  motaword blog                Manage blog articles
  motaword clients             Manage client account
  motaword token               Manage token operations

Use "motaword [command] --help" for more information about a command.
  ```

These are categories of commands. There are subcommands under each category.  

### Subcommands
Let's say we want to get some reports about my account:

```sh
> motaword reports
Usage:
  motaword reports [command]

Available Commands:
  get-language-pairs-report Returns a report of language pairs.
  get-projects-report       Returns a report of corporate account users.
  get-users-report          Returns a report of corporate account users.
```

We now see the available commands under `reports` category. Let's use `get-language-pairs-report` subcommand:

```sh
> motaword reports get-language-pairs-report

{
  "meta": {
    "paging": {
      "links": {
        "next": null,
        "previous": null,
        "self": {
          "href": "https://api.staging.motaword.com/v0/reports/language-pairs"
        }
      },
      "page": 1,
      "per_page": 2,
      "total_count": 2
    }
  },
  "report": [
    {
      "language_pair": {
        "source_language": "af",
        "target_language": "ak"
      },
      "spending": "343.42",
      "word_count": "2475"
    },
    {
      "language_pair": {
        "source_language": "en-US",
        "target_language": "fr"
      },
      "spending": "11.70",
      "word_count": "195"
    }
  ]
}
```

Most commands will return a JSON response. By using a tool like `jq`, you can process these JSON responses. Here is an example for getting a human-readable output from the above response:

```sh
> motaword reports get-language-pairs-report \
| jq '.report[] | "For \(.language_pair.source_language) into \(.language_pair.target_language), we spent $\(.spending)"'


"For af into ak, we spent $343.42"
"For en-US into fr, we spent $11.70"
```

### Command parameters
Some commands accept parameters and JSON payloads. We can view what kind of parameters or JSON payload they can accept, we can use the `--help` flag:

```sh
> motaword projects get-project --help

Get single project

Usage:
  motaword projects get-project id [flags]

Flags:
  -h, --help            help for get-project
      --with[] string   Include detailed information. Possible values 'client', 'vendor', 'score'
```

`get-project` command only accepts and `id` parameter and `with[]` flag.

Some commands will accept JSON payloads, these are usually create/update commands (e.g. POST/PUT requests in HTTP). 

We can view the shape of the request payload via `--help`. The request description is an OpenAPI specification. Here is an example *(removed descriptions for brevity)*:

```sh
> motaword reports get-projects-report --help

## Request Schema (application/json)

properties:
  budget_code:
    type: string
  date_from:
    format: date-time
    type: string
  date_to:
    format: date-time
    type: string
  source_languages:
    items:
      type: string
    type: array
  target_languages:
    items:
      type: string
    type: array
  users:
    items:
      format: int64
      type: integer
    type: array
type: object

Usage:
  motaword reports get-projects-report [flags]

Flags:
  -h, --help   help for get-projects-report
```

According to the request schema in the help output, you can send this payload with this command (which will generate your projects report within these dates):

```json
{
   "date_from":"2021-04-21T16:07:12.727Z",
   "date_to":"2021-05-21T16:07:12.727Z"
}
```

#### Sending JSON payload
JSON payloads like above can be sent by piping `STDIN` into the command.

If you have a simple payload, you can simply do this:
```sh
> echo '{"date_from":"2021-05-21T16:07:12.727Z","date_to":"2021-05-21T16:07:12.727Z"}' \
   | motaword reports get-projects-report
```

If you have a more complex payload, we recommend putting it in a file first, and then piping the file into the command:

```sh
> motaword reports get-projects-report < my-complex-payload.json
```

## Configuration
`motaword` CLI application looks for `.motaword/` directory to configure itself. It looks for configuration in this order:

1. Working directory
2. `$HOME/.motaword`
3. `/etc/.motaword`

We keep 2 files in this directory:
- credentials.json
- config.json

`credentials.json` holds your MotaWord API client ID and client secret. This file can be generated via `motaword` CLI itself. See **Authentication** below for more information.

`config.json` lets you configure all aspects of your `motaword` CLI, and especially tailored for `motaword translate` command which is typically used in development and CI/CD environments. You can learn more about `translate` command below.

Unless you are using `translate` command, `config.json` file is optional.

## Authenticating
### TL;DR
Add a default authentication profile to automatically create `.motaword/credentials.json`:
```shell
motaword auth add-profile client-credentials default MY-CLIENT-ID MY-CLIENT-SECRET
```

### Authentication details
`motaword` CLI requires a MotaWord developer account and an API client ("app" as we call them). You can create one here: [https://www.motaword.com/developer](https://www.motaword.com/developer)

Once you have a developer account, create a new app on your Developer Portal. Now you can access your API client ID and secret for this app. 

You need to use these keys with your `motaword` CLI by putting them in `.motaword/credentials.json` file. This is what `credentials.json` looks like:

```json
{
  "profiles": {
    "default": {
      "client_id": "my-client-id",
      "client_secret": "my-client-secret",
      "type": "client-credentials"
    }
  }
}
```

`motaword` CLI lets you have multiple **profiles** for authentication. `default` profile is sufficient in most use cases, but you can have multiple profiles with arbitrary names (e.g `secondary-profile` in place of `default` in the example above).

We provide a way to automatically manage your authentication profiles:

```shell
> motaword auth

Authentication settings

Usage:
  motaword auth [command]

Available Commands:
  add-profile   Add user profile for authentication
  list-profiles List available configured authentication profiles

Flags:
  -h, --help   help for auth
```

You can add a profile with this command:
```shell
> motaword auth add-profile client-credentials <name> <client-id> <client-secret>
```

Let's add a **default** profile:
```shell
motaword auth add-profile client-credentials default my-client-id my-client-secret
```

That's it! `motaword` CLI will manage the authentication for you including reusing and refreshing your API access tokens.

## Translating
Needless to say, translating your files is the most important thing that our CLI tool does. You can use any of the document upload, project download/package commands, **BUT**, there is one exclusive command that **solves everything for you**:

```sh
> motaword translate --help

Usage:
  motaword translate [target-language] [flags]

Flags:
  -h, --help   help for translate
```

This command is much smarter than it looks and comes with its own set of configuration to handle your file translations. This is especially used in code bases and CI/CD environments.

**What does it do exactly?** It uploads your language resource files, downloads the translated versions and puts them in the correct place in your directory structure. Let's dive in.

### Configuring translations
`translate` command comes with a highly flexible configuration to enable your translations in all use cases. Let's take a look at them:


| Config Key             | Type                         | Required | Default Value                          | Description                                                                                                                                                                                                                                                                                                                                                                      |
|------------------------|------------------------------|----------|----------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| project-id             | integer \| string            | yes      | `null`                                 | This is the continuous project ID given to you by MotaWord.                                                                                                                                                                                                                                                                                                                      |
| source-directory       | string                       | yes      | `null`                                 | The base directory for `motaword` CLI to start looking for source files. The actual source file detection can be managed by `rules` configuration. All rules are based on this source directory.                                                                                                                                                                                 |
| translations-directory | string                       | no       | `source-directory`/{locale}            | The base directory for `motaword` CLI to start placing your translated files. The translated file patterns `rules` configuration will be based on this directory. Can be same with `source-directory`.   By default, we will put translations in each `{locale}` directory under `source-directory`. **Beware, the default value is probably not the correct way in your case.** |
| rules                  | JSON Map  `{string: string}` | yes      | `{` `"**/*" : "{path}/{basename}"` `}` | List of `glob`-like file path patterns to specify your source files and how to place their translated versions. See below for more available variables, detailed explanation and examples.                                                                                                                                                                                       |
| exclude-rules          | JSON Array  [string]         | no       | []                                     | List of `glob`-like excluded source file patterns. We will skip these files during translation.                                                                                                                                                                                                                                                                                  |


### Translation rules
`rules` configuration is quite versatile to enable the correct directory structure in many use cases. Each rule uses `glob`-like path patterns to collect a list of source files. You can specify multiple rules to combine different directory structures into your MotaWord translations.

Let's look at the default value for `rules`:
```json
{
   "source-directory": "src/en",
   "translations-directory": "src/{locale}",
   "rules": {
      "**/*": "{path}/{basename}"
   }
}
```

This rule tells `motaword` CLI:

> take all the files in my `src/en` directory as source files and put their translations in the same path structure inside the `src/{locale}` directory where `{locale}` is each of your translation languages.

If your source and translations directories are organized in a simple manner, this can be sufficient for you. However, rules can handle very complex use cases. Let's take a look at one that is a little more complex than the default one:

> You should look at the section **Available variables in rules** before the complex example, it will make the example easier to digest.

```json
{
  "source-directory": "src/main/resources/strings",
  "translations-directory": "src/main/resources/translations",
  "rules": {
    "frontend/json/*.json": "{locale}/{path}/{basename}",
    "frontend/**/*.yml": "{locale}/{path}/{filename}.yaml",
    "pot/*.pot": "{locale}/{path}/{basename}",
    "md/*.md": "markdown-translations/{locale}/{path}/{filename}.md"
  },
  "exclude-rules": [
    "pot/passwords*"
  ]
}
```

A `rules` configuration like this says a lot :) 

**To debug and understand what your rules say,** you can always run `motaword debug`. It will tell you how your rules are interpreted. 

Let's go through the complex rules above:

> Given we are translating our files into French, which has the language code `fr`

1. Take `src/main/resources/strings` as the base directory for my source files.
2. Take `src/main/resources/translations` as the base directory for translated files. The last folder is different than the source path.
3. Translate all of `.json` files in `src/main/resources/strings/frontend/json/` directory, **without going down subdirectories**
   1. And put their (e.g. French) translations into `src/main/resources/translations/fr/frontend/json/`
4. Translate all of `.yml` files under `src/main/resources/strings/frontend/` directory, going **recursively in all subdirectories**
   1. And put their (e.g. French) translations under `src/main/resources/translations/fr/`, by using respective relative path of the source file.
5. Translate all of `.md` files in `src/main/resources/strings/md/` directory, **without going down subdirectories**
   1. And put their (e.g. French) translations into `src/main/resources/translations/fr/md/`
6. And finally, while doing all of that, exclude all files in `src/main/resources/strings/pot` directory whose file name starts with `passwords`.

If you are still here, you are an expert `motaword` configurer :)


### Available variables in rules

| Variable     | Description                                                                                                                                                               | Example                                                                                                                                                   |
|--------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------|
| `{basename}` | Full name of the file, including file extension                                                                                                                           | If source directory is: `./source-directory`<br><br>Given: `./source-directory/json-files/ui.json`<br><br>Then `{basename}`: `ui.json`                    |
| `{filename}` | Name of the file without the file extension.<br><br>This variable is usually used to change the extension of translated files.                                            | If source directory is: `./source-directory`<br><br>Given: `./source-directory/json-files/ui.json`<br><br>Then `{filename}`: `ui`                         |
| `{path}`     | The directory path from the source directory onto the file.<br><br>This is the most useful variable to modify your directory structure.                                   | If source directory is: `./source-directory`<br><br>Given: `./source-directory/json-files/v1/public/ui.json`<br><br>Then `{path}`: `json-files/v1/public` |
| `{locale}`   | The translation language code that is being processed.<br><br>This is most useful when your translation directories have different needs for target language directories. | `en-US`, `fr`, `zh-CN` etc...                                                                                                                             |


### Translation configuration examples

Look at the [`./examples`](https://github.com/motaword/cli/tree/master/examples) directory in this repository for various configuration examples.

## Debugging

All `motaword` CLI commands can get some debugging flags. Use `--help` to see the flags.

You can configure the level of output by using verbose flags:

| Flag                   |                  |
|------------------------|------------------|
| `--v`                  | WARN level logs  |
| `--vv`                 | INFO level logs  |
| `--vvv` or `--verbose` | DEBUG level logs |

There is also a `debug` command available for your `config.json` configuration file:

```sh
> motaword debug
```

This will output the detected source files and their corresponding translation paths.

## Contact

24/7 FREE technical or business live chat support: [https://www.motaword.com](https://www.motaword.com)


<!-- MARKDOWN LINKS & IMAGES -->
<!-- https://www.markdownguide.org/basic-syntax/#reference-style-links -->
[product-screenshot]: images/screenshot.png