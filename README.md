[![Build status](https://img.shields.io/github/workflow/status/ibm-garage/node-garage-tools/Build)](https://github.com/ibm-garage/node-garage-tools/actions?query=workflow%3ABuild)
[![Code style: prettier](https://img.shields.io/badge/code_style-prettier-ff69b4.svg)](https://github.com/prettier/prettier)

# Garage Tools for Node.js

This package provides command-line tools from the IBM Garage for Cloud for use in developing Node.js applications.
It was split off from [Garage Utilities for Node.js](https://github.com/ibm-garage/node-garage-utils) and preserved here, but has not yet been published.

It requires Node.js 12.13.0 (LTS Erbium) or later.

## Contents

- [Installation](#installation)
- [CF Util](#cf-util)
- [License](#license)

## Installation

Normally, these tools are installed as a development dependency:

```
npm install -D garage-tools
```

They can then be run with `npx` or invoked directly from an npm run script.

## CF Util

```
$ npx cfutil -h
$ npx cfutil env -h
$ npx cfutil logs -h
```

### env

Saves information about the enviroment of a Cloud Foundry app to a file that can be used to replicate (or partially replicate) that environment when running locally.

```
$ npx cfutil env my-app
```

By default, a `.env` file is created, defining just the `VCAP_SERVICES` environment variable read from the specified app.
You can also opt to include user-provided environment variables with the `-u` option.
You can source this file before running locally to set up the environment.
This is also the same file format consumed by [dotenv](https://www.npmjs.com/package/dotenv), though its use is not recommended because it adds complexity and runtime dependencies.

Alternatively, with the `-j` option, a `services.json` file can be created, containing just the formatted JSON content of `VCAP_SERVICES`.
The advantage of this approach is that the file is easier to read and edit.
The disadvantage is that multiple environment variables cannot be defined.

Whether you opt for a `.env` or `services.json` file, you can also use the `-s` option to generate a tiny `env.sh` script that will consume it at run time.
For example, you might define an npm run script like this:

```
"start:local": ". env.sh && node server/server.js",
```

Here, sourcing `env.sh` adds the variables defined in `.env` or `services.json` to the environment.

Use the `-h` option to see all options supported by the `env` command.

### logs

Tails or shows recent logs for a Cloud Foundry app, trimming the content added by Loggregator from JSON messages to allow for formatting by the Bunyan CLI.
This command takes the place of `cf logs` when you are using the Bunyan-based `logger` API.

```
$ npx cfutil logs my-app | npx bunyan
```

By default, Loggregator content is removed from JSON messages.
All other messages pass through unchanged (except for the trimming of leading whitespace), and then pass through Bunyan as well.
Non-application and non-JSON messages can be excluded completely with the `-a` and `-j` options, respectively.

Use the `-h` option to see all options supported by the `logs` command.

## License

This package is licensed under the MIT License.
The full text is available in [LICENSE](LICENSE).
