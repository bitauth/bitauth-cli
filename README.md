[![Version](https://img.shields.io/npm/v/@bitauth/cli.svg)](https://npmjs.org/package/@bitauth/cli)
[![CircleCI](https://circleci.com/gh/bitauth/bitauth-cli/tree/master.svg?style=shield)](https://circleci.com/gh/bitauth/bitauth-cli/tree/master)
[![Appveyor CI](https://ci.appveyor.com/api/projects/status/github/bitauth/bitauth-cli?branch=master&svg=true)](https://ci.appveyor.com/project/bitauth/bitauth-cli/branch/master)
[![Codecov](https://codecov.io/gh/bitauth/bitauth-cli/branch/master/graph/badge.svg)](https://codecov.io/gh/bitauth/bitauth-cli)
[![Downloads/week](https://img.shields.io/npm/dw/bitauth-cli.svg)](https://npmjs.org/package/bitauth-cli)

# bitauth-cli

### Work In Progress: Please note, the documentation below is for planning and design purposes, and most commands are not yet implemented.

Bitauth is a universal identity and message authentication standard. This CLI provides commands for tracking identities, verifying signatures, and managing your own wallets and identities.

# Quick Start

With [Node.js](https://nodejs.org/en/download/) v10 LTS or later, install the CLI globally:

```sh
npm i -g @bitauth/cli
```

For the best experience, run `bitauth autocomplete` and follow the instructions to enable autocompletion for your shell.

## Verify a File

```sh
bitauth verify path/to/file.zip.bitauth
```

(For details on the `.bitauth` file format, see [`docs/file-format.md`](/docs/file-format.md).)

# Usage Guide

This guide walks through key workflows using Bitauth CLI. A full [CLI Reference](#cli-reference) can be found below.

## Create a Wallet

To create transactions or identities, we'll need a wallet. Get started using the interactive setup:

```
bitauth wallet new
```

<!-- Outline:
- What kind of wallet would you like to create? (new wallet types can be added with `bitauth template`)
- (if template takes arguments, request them here)
- (if ambiguous:) Which role will be performed by this wallet?
- Enter a name for this wallet: (used in CLI output, e.g. "Personal Wallet")
- Choose an alias for this wallet: (to refer to this wallet in CLI commands, e.g. "personal")

(if entity requires variables which aren't keys:)
  - How many addresses should be pre-generated?
  - Please provide a value for `variable_name`: (First sentence of variable description) Footer: The remaining portion of the variable description as a hint. (Followed by) Result: [computed in hex and disassembled] | Error: [BTL compilation error]

(Print full CLI command with dim text:)
Creating wallet...
$ bitauth wallet:new "Personal Wallet" "personal" --template="m-of-n" --template-parameters="{m:2,n:2}" --entity="owner" --wallet-data="{wallet_var:'0x012345'}" --address-data="[{addr_var: '1'}, {addr_var: '2'}]"

New wallet "Personal Wallet" partially created at: $BITAUTH_DATA_DIR/wallets/personal

For details, run: `bitauth wallet personal`

--json: {
  path: '$BITAUTH_DATA_DIR/wallets/short-id',
  status: 'partial'
}
 -->

For our first wallet, let's choose the wallet type: `Single Signature (P2PKH)`. Follow the remaining prompts to choose a name and an alias for the wallet.

When the setup is complete, confirm that the new wallet is in our list of wallets:

```
bitauth wallet
```

<!--

$ bitauth wallet
Use "bitauth wallet [ALIAS]" to start interactive mode, or --help for details.

**Bitauth Wallets**:
Alias       Name              Role (if any differ)     Balance       Refreshed (if any differ)
personal    Personal Wallet   Owner                    1.000 BCH     2020-2-27 12:00 PM EST
second      Another Wallet    Owner                    0.050 BCH
(if Refreshed column is hidden:)
Refreshed: 2020-2-27 12:00 PM EST

-->

This lists all of our wallets and their current status. Most Bitauth CLI commands can also return results in JSON format using the `--json` flag, e.g.:

```sh
bitauth wallet --json
```

<!--

{
  "wallets": [
    {"alias": "personal", "name": "Personal Wallet", "role": "Owner", "balance": "1.000 BCH", Refreshed: [Date]}
  ]
}

 -->

This is helpful when using Bitauth CLI from scripts or other programs.

## Fund an Address

Now that the wallet has been created, we can generate an address to receive our first payment.

We can get the first unused address from the wallet interface – `[ALIAS]` is the identifier you chose during the interactive wallet setup:

```sh
bitauth wallet [ALIAS]
```

<!--
bold(Personal Wallet) (c.yellow("personal"))

list addresses

Commands:
- rename
- status (shown in interactive header)
- refresh
- address
- outputs
- listen

 -->

The interactive wallet interface also allows you to select from a list of actions to perform on the wallet. For now, you can simply exit out with `Ctrl+C` or by choosing the `Exit` option.

The address begins with `bitcoincash:`, and is followed by a long string of letters and numbers. Copy this address, paste it into another wallet, and send a small amount of BCH (or use a free faucet like the [Bitcoin.com faucet](https://free.bitcoin.com/)).

## Create an Identity

Next, let's create our first identity using the interactive setup:

```
bitauth id new
```

<!--
 - Enter a name for this Bitauth ID: (e.g. "Satoshi Nakamoto" or "Company Name")
   - warn if longer than 50 characters
 - Choose an alias for this Bitauth ID: (to refer to it in CLI commands, e.g. "me")
 - Enter a description for this identity: [placeholder: "Short bio or description for this identity"] (optional)
   - warn if longer than 100 characters
 - Would you like to broadcast this name and description on-chain? Make public / Don't share (by adding them to the public metadata for this identity)
 - Which wallet should hold the ownership output? (This wallet must sign to update the identity or change the signing output.)
 - Which wallet should hold the signing output? (This wallet may temporarily sign on behalf of the identity.)
 -->

Follow the prompts to choose a name, short identifier, and description for the Bitauth ID. When prompted, choose the wallet we created for both the identity and signing outputs.

When the setup is complete, confirm that the new ID is in our list of Bitauth IDs:

```
bitauth id
```

<!--
TODO: can we merge "track" into this?

example output:

Bitauth Identities
==================
Alias  Name               Wallet (/Ownership)    Signing (if any differ)    Updated
me     Jason Dreyzehner   Personal Wallet        Another Wallet             2020-2-27 12:00PM EST

 -->

Our new ID is listed, but its `Updated` column is marked as `Pending...`. Let's create our first update transaction to broadcast it – `[ALIAS]` is the alias you chose during the interactive setup:

```
bitauth id [ALIAS]
```

<!--
bold(Jason Dreyzehner) (c.yellow("me"))
Bitauth ID: [Pending first update] (later: `bitauth:qwtcxp42fcp06phz2xec6t5krau0ftew5efy50j9xyfxwa38df40zp58z6t5w`)
Comment: Short bio or description for this identity

Last Update: 2020-2-27 12:00PM EST (/ c.dim(never))
[Authhead TXID]

Current Status
==============
Ownership: c.dim(none)
Public Metadata: c.dim(none)

After Update
============
Ownership: Personal Wallet
Signing Output: Another Wallet (only shown if different than Ownership)
Public Metadata:
  Name: "Jason Dreyzehner"
  Comment: "Short bio or description for this identity"

**This identity has pending changes.**
Confirm the changes are correct, then create the update transaction.

- Create Update Transaction
---- (separator)
- Change Name, Alias, or Comment
  - (Display enquirer multi-input form)
  - (On confirm:) Would you like to update this identity's public metadata with these changes? (hint: The "name" and "comment" will be broadcasted on-chain.)
- Edit Public Metadata (hint: Metadata is shared via an OP_RETURN output using the Bitauth Key-Value Protocol.)
- Move Ownership Output (hint: This output must sign to create update transactions.)
- Move Signing Output (hint: This output can sign on behalf of the identity.)
- Exit (hint: You can also exit with Ctrl+C.)
 -->

The interactive identity interface allows us to select from a list of actions to perform on the identity. Choose `Create Update Transaction` to create our first update transaction.

<!--

doesn't have any options/interactivity – just creates the proposal.

$ bitauth id:update [ALIAS]
Update transaction proposal created for identity Jason Dreyzehner ("me").

Proposal ID: [uuid]
Alias: me-0

Use "bitauth tx me-0" to modify or broadcast.


--json: {
  "update": {
    "success": true,
    "proposal": "[ID]",
    "alias": "[ALIAS]"
  }
}

 -->

Now let's confirm the transaction proposal was created:

```sh
bitauth tx
```

<!--


flags:
filter by wallet: --wallet="ALIAS"
filter by identity: --id="ALIAS"
filter by address: --address="ADDR"
only proposals: --proposals
only history: --history
show both proposals and history: --proposals --history OR (neither flag)
limits to 20 items by default – no limit: --limit=0
--json should work as expected

● – signed
(? ◍ – partially signed , ○ – finalized, or:)
○ – partially signed

example output:

$ bitauth tx
Transaction Proposals                            ● signed   ○ partially signed
=====================
  Alias        Comment                                Created
○ me-0         Update TX for "Jason Dreyzehner"       2020-2-27 12:00PM EST

(if exists)
Transaction History
===================

 -->

Our new update transaction is now listed under `Transaction Proposals`. Let's fund it:

```
bitauth tx [ALIAS]
```

<!--

$ bitauth tx me-0
Transaction Proposal: me-0
Comment: Identity Update TX for "Jason Dreyzehner"
Created: 2020-2-27 12:00PM EST

Inputs: c.dim(none)

Outputs:
● 0: 0.001 BCH – Personal Wallet (:1)
  1: 0.001 BCH – Another Wallet (:1)
  2: 0 BCH – OP_RETURN (BKVP Metadata)
dim(● change output)

(blocking issues shown here:)
**This transaction has not been funded.** Add at least one input to continue.

- Edit Inputs/Outputs
---- (required/recommended actions shown above separator)
- Change Alias or Comment
- Export Proposal
- Import
- Exit (hint: You can also exit with Ctrl+C.)



------------------
(Inputs which include Ownership Outputs from tracked identities:)

Inputs:
  0: 0.01 BCH – Personal Wallet (:1) → bold(Jason Dreyzehner ("me")), bold(Another Merged One ("another"))

(Warnings section – non-blocking issues)
Warnings:
  - This transaction proposal modifies the identity: Jason Dreyzehner ("me")
  - This transaction proposal modifies the identity: Another Merged One ("another")


  - (OP_RETURN in 0th output + tracked identities:) This transaction permanently destroys and identity: Identity Name ("alias") - The Ownership (0th) output is un-spendable.
  -


 ({'n':'Jason Dreyzehner', 'c': ''})

 -->

The interactive transaction interface allows you to edit inputs and outputs

## Sign a File

```
bitauth sign [FILE_PATH]
```

## Migrate an Identity

[describe upgrading an identity to multisig]

### Create a Multisig Wallet

### Create the Migration Transaction

## Broadcast Metadata for an Identity

### Add Metadata to the Identity

### Create the Migration Transaction

## Lookup an Identity

Occasionally you'll need to quickly lookup an identity. Find the entity's Bitauth ID, e.g. `bitauth:qwtcxp42fcp06phz2xec6t5krau0ftew5efy50j9xyfxwa38df40zp58z6t5w`, then:

```sh
bitauth lookup [BITAUTH_ID]
```

Or:

```sh
bitauth lookup [BITAUTH_ID] --json
```

This will show the latest information for the specified identity. As with most commands, the `--json` flag causes results to be returned in JSON format. You can use a program like [`jq`](https://github.com/stedolan/jq) to manipulate the output.

## Track an Identity

For identities you'll lookup again in the future – like software signing identities – you should track the identity:

```sh
bitauth track [BITAUTH_ID]
```

This initiates a short setup process to track the identity. If the identity is a contact you already know or a public entity, you should use multiple sources to confirm it is the correct Bitauth ID, e.g. the entity's website, social media, or direct communications.

You'll now find the new identity in our list of tracked identities:

```
bitauth track
```

<!--
icon ideas:
● – ownership
◍ – confirming ownership output
○ – signing
◌ – confirming signing output


Tracked Bitauth Identities      (show when visible:)    ● ownership   ○ signing
==========================

  Name                  Comment                                       Alias  (default 7, up to first different char)
○ Another Identity      That one's description                        qwtcxp42...
● Jason Dreyzehner      Short bio or description for this identity    me

--json:

{
  "identities": [
    {"name": "Another Identity", "comment": "That one's description", "ownership": false, "signing": "personal"},
    {"name": "Jason Dreyzehner", "comment": "Short bio or description for this identity", "ownership": "personal", "signing": "personal"},
    {"name": "Someone Else", "comment": "the comment I added", "ownership": false, "signing": false}
  ]
}


 -->

## Switch Bitauth Data Directories

All wallets, data, and configuration are stored in your Bitauth data directory. To show the data directory currently in use, run:

```
bitauth config --data-dir
```

This can be configured using the `$BITAUTH_DATA_DIR` environment variable:

```sh
export BITAUTH_DATA_DIR='~/another/data-directory';
bitauth config --data-dir
# => /Users/me/another/data-directory
```

Using multiple data directories can be helpful for separating domains of wallets, e.g. "business" and "personal".

For more information on the contents of the Bitauth data directory, see [the readme](src/internal/defaults/data-dir-readme.ts) generated in your own:

```
cat $(bitauth config --data-dir)/readme.md
```

# CLI Reference

Bitauth CLI includes commands to create and manage Bitauth identities, sign files and messages, verify existing Bitauth signatures, and more.

<!-- disable:usage -->
<!-- disable:usagestop -->

<!-- disable:toc -->
<!-- disable:tocstop -->

<details>
<summary><strong>Commands</strong></summary>

Below you'll find the [`help`](#bitauth-help-command) output for all available commands.

<!-- commands -->

- [`bitauth autocomplete [SHELL]`](#bitauth-autocomplete-shell)
- [`bitauth config`](#bitauth-config)
- [`bitauth hello [FILE]`](#bitauth-hello-file)
- [`bitauth help [COMMAND]`](#bitauth-help-command)
- [`bitauth update [CHANNEL]`](#bitauth-update-channel)
- [`bitauth wallet [FILE]`](#bitauth-wallet-file)
- [`bitauth wallet:new [WALLET_ID] [TEMPLATE_ID]`](#bitauth-walletnew-wallet_id-template_id)

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

## `bitauth config`

Display current Bitauth configuration

```
USAGE
  $ bitauth config

OPTIONS
  -h, --help  show CLI help

DESCRIPTION
  Longer description here
```

_See code: [src/commands/config.ts](https://github.com/bitauth/bitauth-cli/blob/v0.0.0/src/commands/config.ts)_

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

## `bitauth wallet [FILE]`

short description here

```
USAGE
  $ bitauth wallet [FILE]

OPTIONS
  -h, --help  show CLI help

DESCRIPTION
  Longer description here

ALIASES
  $ bitauth wallets

EXAMPLE
  $ bitauth wallet
  hello world from ./src/hello.ts!
```

_See code: [src/commands/wallet/index.ts](https://github.com/bitauth/bitauth-cli/blob/v0.0.0/src/commands/wallet/index.ts)_

## `bitauth wallet:new [WALLET_ID] [TEMPLATE_ID]`

short description here

```
USAGE
  $ bitauth wallet:new [WALLET_ID] [TEMPLATE_ID]

ARGUMENTS
  WALLET_ID    wallet identifier
  TEMPLATE_ID  authentication template identifier

OPTIONS
  -h, --help  show CLI help

DESCRIPTION
  Longer description here

EXAMPLE
  $ bitauth wallet new
  hello world from ./src/hello.ts!
```

_See code: [src/commands/wallet/new.ts](https://github.com/bitauth/bitauth-cli/blob/v0.0.0/src/commands/wallet/new.ts)_

<!-- commandsstop -->

</details>

# Contributing

Pull Requests welcome! Please see [`CONTRIBUTING.md`](.github/CONTRIBUTING.md) for tips on getting started.
