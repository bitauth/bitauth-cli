<!-- TODO: PR to https://github.com/bitcoincashorg/bitcoincash.org/blob/master/etc/protocols.csv -->

# Bitauth Key-Value Protocol (BKVP)

Bitauth Key-Value Protocol (BKVP) is a simple system for broadcasting structured metadata relating to a particular Bitauth identity.

## Declaring Metadata

Each time a Bitauth identity is migrated, the migration transaction may include one or more BKVP Metadata Outputs specifying the latest attributes of the identity's metadata. (While relay rules enforced by some node implementations require that transactions have no more than one `OP_RETURN` output, this specification includes no such requirement.)

Only the latest authhead is considered by BKVP. If previously-broadcasted metadata keys are no longer included in the new metadata output(s), or if no metadata output is present, the absent metadata is renounced.

## Trusting Metadata

Implementations must ensure that BKVP metadata is only trusted insofar as the declaring identity is trusted. BKVP metadata should be ignored for unknown identities.

## BKVP Metadata Output Format

Any transaction output with the prefix `OP_RETURN <'bkv' 0x00>` (`0x6a626b7600`) is considered to be a BKVP Metadata Output (i.e. a [Lokad ID](https://github.com/bitcoincashorg/bitcoincash.org/blob/master/spec/op_return-prefix-guideline.md) of `0x00766b62`). Future upgrades to this protocol may increment the `0x00` in this prefix to indicate protocol version.

Metadata is provided in alternating UTF8 key-value pairs: first the key is pushed, followed by the value, e.g.:

```btl
OP_RETURN <'bkv' 0x00> <'key'> <'value'> <'another-key'> <'another value'>
```

produces the BKVP metadata:

```json
{
  "key": "value",
  "another-key": "another value"
}
```

A properly-formed metadata output must minimally-push an odd number of items to the stack – the `0x6a626b7600` prefix, followed by a series of key-value pairs (such that each key is matched with a value). If an output pushes an even number of stack items, includes non-minimally encoded pushes, or contains unexpected opcodes, the entire output must be ignored. For example, the following scripts produce no BKVP metadata:

```btl
// Missing value for 'extra-key'
OP_RETURN <'bkv' 0x00> <'valid'> <'pair'> <'extra-key'>

// Unexpected opcode
OP_RETURN <'bkv' 0x00> <'valid'> <'pair'> OP_2DUP
```

When parsing an authhead transaction to determine BKVP Metadata, each BKVP Metadata Outputs is parsed in serialized transaction order, and any metadata key collisions overwrite previous values. For example:

```btl
// Output 1 (overridden)
OP_RETURN <"bkv" 0x00> <'key'> <'1'>

// Output 2 (first key is overridden)
OP_RETURN <"bkv" 0x00> <'key'> <'2'> <'key'> <'3'>

// Output 3 (malformed)
OP_RETURN <"bkv" 0x00> <'key'> <'4'> <'extra'>
```

must be parsed as the BKVP metadata:

```json
{
  "key": "3"
}
```

# Metadata Key Prefixes

To avoid collisions, keys not standardized in this specification should be prefixed with an identifier for the protocol by which they are standardized. Prefixes may be any valid UTF8 string, followed by a colon (`:` – `0x3A`).

For example, a software package release protocol may choose a prefix like `release-protocol`, where the standardized key for publishing a double sha256 hash is: `release-protocol:hash256`.

When grouping keys by prefix, clients should split each metadata key at the first instance of a colon (`:` – `0x3A`), e.g. for `protocol:nested:key`, the prefix is `protocol`, and the metadata key is `nested:key`.

### Known Metadata Key Prefixes

To assist with collision avoidance, a non-normative list of known BKVP metadata key prefixes is provided here.

| Prefix   | Topic                     | Description                                   | URL  |
| -------- | ------------------------- | --------------------------------------------- | ---- |
| `pay`    | Payment Information       | Current payment information for an identity   | TODO |
| `avatar` | Avatar                    | Avatar for an entity in various formats       | TODO |
| `bir`    | Bitauth Identity Registry | Update a registry of known Bitauth identities | TODO |
| `ca`     | CashAccount               | The canonical CashAccount for an identity     | TODO |
| `git`    | Git                       | Get the latest state of a Git repository      | TODO |
| `npm`    | Node Package Manager      | Verify version(s) of NPM package(s)           | TODO |

# Standard Metadata Keys

The following metadata keys are standardized by this specification: `n`, `c`, `e`, and `u`.

## `n` – Name

A name for this entity as it should be displayed in user interfaces.

In primary interfaces, implementations should truncate this value at 50 characters or less. (Extended contents may be displayed in secondary interfaces.)

### Examples

- `Jason Dreyzehner`
- `Bitauth CLI`

## `c` – Comment

A single-line comment, description, or bio for this entity as it should be displayed in user interfaces.

In primary interfaces, implementations should truncate this value at 100 characters or less. (Extended contents may be displayed in secondary interfaces.)

### Examples

- `Bitauth CLI Maintainer`
- `Project identity for Bitauth CLI. Releases signed using $RELEASE_PROTOCOL.`

## `e` – Email

An email address for this entity.

### Examples

- `contact@bitjson.com`
- `Bitauth CLI`

## `u` – URI

A full [Uniform Resource Identifier](https://en.wikipedia.org/wiki/Uniform_Resource_Identifier) for a user-navigable resource providing more information about this entity.

Typically, this should be a URL with the `http` or `https` scheme, an [onion address](https://en.wikipedia.org/wiki/.onion), an [IPFS address](https://en.wikipedia.org/wiki/InterPlanetary_File_System), or another URI representing a user-navigable resource.

- `https://bitjson.com/`
- `https://github.com/bitauth/bitauth-cli`
