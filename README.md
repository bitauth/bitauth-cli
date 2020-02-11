bitauth-cli
===========

universal identity and message authentication

[![oclif](https://img.shields.io/badge/cli-oclif-brightgreen.svg)](https://oclif.io)
[![Version](https://img.shields.io/npm/v/bitauth-cli.svg)](https://npmjs.org/package/bitauth-cli)
[![CircleCI](https://circleci.com/gh/bitauth/bitauth-cli/tree/master.svg?style=shield)](https://circleci.com/gh/bitauth/bitauth-cli/tree/master)
[![Appveyor CI](https://ci.appveyor.com/api/projects/status/github/bitauth/bitauth-cli?branch=master&svg=true)](https://ci.appveyor.com/project/bitauth/bitauth-cli/branch/master)
[![Codecov](https://codecov.io/gh/bitauth/bitauth-cli/branch/master/graph/badge.svg)](https://codecov.io/gh/bitauth/bitauth-cli)
[![Downloads/week](https://img.shields.io/npm/dw/bitauth-cli.svg)](https://npmjs.org/package/bitauth-cli)
[![License](https://img.shields.io/npm/l/bitauth-cli.svg)](https://github.com/bitauth/bitauth-cli/blob/master/package.json)

<!-- toc -->
* [Usage](#usage)
* [Commands](#commands)
<!-- tocstop -->
# Usage
<!-- usage -->
```sh-session
$ npm install -g bitauth-cli
$ bitauth COMMAND
running command...
$ bitauth (-v|--version|version)
bitauth-cli/0.0.0 darwin-x64 node-v12.14.1
$ bitauth --help [COMMAND]
USAGE
  $ bitauth COMMAND
...
```
<!-- usagestop -->
# Commands
<!-- commands -->
* [`bitauth hello [FILE]`](#bitauth-hello-file)
* [`bitauth help [COMMAND]`](#bitauth-help-command)

## `bitauth hello [FILE]`

describe the command here

```
USAGE
  $ bitauth hello [FILE]

OPTIONS
  -f, --force
  -h, --help       show CLI help
  -n, --name=name  name to print

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
<!-- commandsstop -->
