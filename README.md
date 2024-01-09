# kotlin-diktat-action

This allows you to easily install and run [diKTat](https://github.com/saveourtool/diktat) in Kotlin projects.

## Example usage

```yaml
name: Run diKTat

on:
  push:
    branches: [ main ]

jobs:
  diktat:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: marcospereira/kotlin-diktat-action
```

## Inputs

The action accepts inputs based on what [diKTat CLI](https://diktat.saveourtool.com/diktat-cli/) supports, but filters out a few options.

The supported inputs are:

- `patterns`: A list of files to process by diktat
    - Default: "src/main/kotlin"
    - Examples:
      ```yaml
      patterns: "src/main/kotlin src/test/kotlin src/functionalTest/kotlin"
      ```
      ```yaml
      patterns: "src/**/*.kt"
      ```
- `config`: specify the location of the YAML configuration file. By default, diktat-analysis.yml in the current directory is used
  - Default: `"diktat-analysis.yml"`
  - Examples:
    ```yaml
    config: configs/diktat-analysis.yml
    ```
- `reporter`: The reporter to use to log errors to output. Value should be one of `checkstyle`, `html`, `json`, `plain`, `plain_group_by_file`, `sarif`
  - Default: empty, meaning it will use the CLI's default (`plain`)
  - Examples:
    ```yaml
    reporter: "html"
    output: diktat-report.html
    ```
    ```yaml
    reporter: "sarif"
    output: diktat-report.sarif
    ```
    > [!IMPORTANT]
    > When setting a `reporter`, it is also required to set `output`, and diktat CLI fails if one is set but not the other.

- `output`: Redirect the reporter output to a file. Must be provided when the reporter is provided
  - Default: empty
  - Examples: see `reporter` examples above.
- `group-by-file`: A flag to group found errors by files
  - Default: `"false"`
  - Examples:
    ```yaml
    group-by-file: true
    ```
- `log-level`: Control the log level. Value should be one of `error`, `warn`, `info`, `debug`, `trace`
  - Default: `"info"`
  - Examples:
     ```yaml
     log-level: "debug"
     ```
    ```yaml
     log-level: "error"
     ```
    
## A more complete example

```yaml
name: Run diKTat

on:
  push:
    branches: [ main ]

jobs:
  diktat:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: marcospereira/kotlin-diktat-action
        with:
          patterns: "src/main/kotlin src/test/kotlin"
          log-level: debug
          group-by-file: true
          reporter: sarif
          output: diktat.sarif
```
