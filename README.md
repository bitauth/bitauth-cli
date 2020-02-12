[![Version](https://img.shields.io/npm/v/@bitauth/cli.svg)](https://npmjs.org/package/@bitauth/cli)
[![CircleCI](https://circleci.com/gh/bitauth/bitauth-cli/tree/master.svg?style=shield)](https://circleci.com/gh/bitauth/bitauth-cli/tree/master)
[![Appveyor CI](https://ci.appveyor.com/api/projects/status/github/bitauth/bitauth-cli?branch=master&svg=true)](https://ci.appveyor.com/project/bitauth/bitauth-cli/branch/master)
[![Codecov](https://codecov.io/gh/bitauth/bitauth-cli/branch/master/graph/badge.svg)](https://codecov.io/gh/bitauth/bitauth-cli)
[![Downloads/week](https://img.shields.io/npm/dw/bitauth-cli.svg)](https://npmjs.org/package/bitauth-cli)

# bitauth-cli

universal identity and message authentication

# Getting Started

With [Node.js](https://nodejs.org/en/download/) v10 LTS or later, install the CLI globally:

```sh
git clone https://github.com/bitauth/bitauth-cli.git
cd bitauth-cli
yarn && yarn link

# Note, not yet available on NPM
# npm i -g @bitauth/cli
```

# Reference

Bitauth CLI includes commands to create and manage Bitauth identities, sign files and messages, verify existing Bitauth signatures, and more.

Below you'll find the [`help`](#bitauth-help-command) output for all available commands.

<!-- disabled:toc -->
<!-- disabled:tocstop -->

<!-- disabled:usage -->
<!-- disabled:usagestop -->

# Commands

<!-- commands -->

- [`bitauth autocomplete [SHELL]`](#bitauth-autocomplete-shell)
- [`bitauth hello [FILE]`](#bitauth-hello-file)
- [`bitauth help [COMMAND]`](#bitauth-help-command)
- [`bitauth update [CHANNEL]`](#bitauth-update-channel)

## `bitauth autocomplete [SHELL]`

display autocomplete installation instructions

```
USAGE
  $ bitauth autocomplete [SHELL]

ARGUMENTS
  SHELL  shell type

OPTIONS
  -r, --refresh-cache  Refresh cache (ignores displaying instructions)

EXAMPLES
  $ bitauth autocomplete
  $ bitauth autocomplete bash
  $ bitauth autocomplete zsh
  $ bitauth autocomplete --refresh-cache
```

_See code: [@oclif/plugin-autocomplete](https://github.com/oclif/plugin-autocomplete/blob/v0.1.5/src/commands/autocomplete/index.ts)_

## `bitauth hello [FILE]`

short description here

```
USAGE
  $ bitauth hello [FILE]

OPTIONS
  -f, --force
  -h, --help       show CLI help
  -n, --name=name  name to print

DESCRIPTION
  Longer description here

EXAMPLE
  $ bitauth hello
  hello world from ./src/hello.ts!
```

_See code: [src/commands/hello.ts](https://github.com/bitauth/bitauth-cli/blob/v0.0.0/src/commands/hello.ts)_

## `bitauth help [COMMAND]`

display help for bitauth

```
USAGE
  $ bitauth help [COMMAND]

ARGUMENTS
  COMMAND  command to show help for

OPTIONS
  --all  see all commands in CLI
```

_See code: [@oclif/plugin-help](https://github.com/oclif/plugin-help/blob/v2.2.3/src/commands/help.ts)_

## `bitauth update [CHANNEL]`

update the bitauth CLI

```
USAGE
  $ bitauth update [CHANNEL]
```

_See code: [@oclif/plugin-update](https://github.com/oclif/plugin-update/blob/v1.3.9/src/commands/update.ts)_

<!-- commandsstop -->
