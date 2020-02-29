# Bitauth File Format

The Bitauth file format (`.bitauth`) is a simple format for sharing data and signatures between Bitauth CLI instances. The format requires a single-line, JSON-formatted header, followed by an optional newline (`\n`) and payload.

All `.bitauth` files **must** begin with a single-line, JSON header object with the following properties:

- `v` – **version** (required) – must be exactly `1`.
- `a` – **action** (required) – any string containing only lowercase characters, digits, or the `-` character. (RegExp: `/^[a-z0-9-]+$/`)
- `d` – **data** (optional) – a JSON value of the format required by the specified action.

For example:

```json
{ "v": 1, "a": "action-identifier", "d": { "hello": "world" } }
```

When parsing a `.bitauth` file, the client must read from the beginning of the file to the first newline (`\n`), parsing the result as JSON. (As with [`ndjson`](http://ndjson.org/), this ensures support for `\r\n` because trailing white space is ignored when parsing JSON values.)

The payload included after the newline may be of any format. This file layout makes it easy to strip the bitauth JSON header from the payload by removing the first line of the file, e.g.:

```sh
sed '1d' file.zip.bitauth > file.zip
```

## Actions

A number of actions are standardized. If a client encounters an unknown action, it should return with a non-zero exit code.

### `verify`

TODO
