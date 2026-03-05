# CLI Specification

### auditrs \<COMMAND\>

#### Control commands

- start
- stop
- reboot
- status

#### Query commands

- dump
- search
- report

#### Other

- help
- config

---

## Query

Commands for interacting with `auditrs` event logs.

### dump

```bash
auditrs dump \
  [--since <TIME>] \
  [--until <TIME>] \
  [--type <EVENT_TYPE>] \
  [--user <USER>] \
  [--result {success | failed}] \
  [--format {legacy | simple | json}] \
  [--follow] \
  [--limit <N>] \
  [<FILE>]
```

- **since \<TIME\>** / **until \<TIME\>**: Time range to include (e.g. `2026-03-04T10:00`, `-1h`).
- **type \<EVENT_TYPE\>**: Filter by event type (e.g. `exec`, `file`, `auth`).
- **user \<USER\>**: Filter by effective user name or ID.
- **result {success \| failed}**: Filter by outcome.
- **format {legacy \| simple \| json}**: Output format (default is `simple`).
- **follow**: Stream events as they arrive (like `tail -f`).
- **limit \<N\>**: Maximum number of events to output.
- **FILE**: Optional output file path; if omitted, writes to stdout.

### search

```bash
auditrs search <QUERY> \
  [--since <TIME>] \
  [--until <TIME>] \
  [--field <FIELD>] \
  [--type <EVENT_TYPE>] \
  [--user <USER>] \
  [--result {success | failed}] \
  [--format {table | json}] \
  [--limit <N>]
```

- **QUERY**: Free-text or key-value search expression.
- **since \<TIME\>** / **until \<TIME\>**: Limit results to a time range.
- **field \<FIELD\>**: Restrict the search to a specific field (e.g. `exe`, `path`, `syscall`).
- **type \<EVENT_TYPE\>**: Filter by event type.
- **user \<USER\>**: Filter by user.
- **result {success \| failed}**: Filter by outcome.
- **format {table \| json}**: Output as a human-readable table or JSON.
- **limit \<N\>**: Maximum number of matching events to print.

### report

```bash
auditrs report \
  [--since <TIME>] \
  [--until <TIME>] \
  [--by {user | result | syscall | exe | type}] \
  [--failed] \
  [--top <N>] \
  [--format {table | json}]
```

- **since \<TIME\>** / **until \<TIME\>**: Time window for the report.
- **by {user \| result \| syscall \| exe \| type}**: Aggregation dimension.
- **failed**: Only include failed events.
- **top \<N\>**: Only show the top-N buckets per aggregation.
- **format {table \| json}**: Report output format.

## Config

Commands for manipulating `auditrs` configuration.

```bash
auditrs config <COMMAND> { get | set | filter }
```

### Get

```bash
auditrs config get [COMMAND] { directory | size | format }
```

- **directory**: Get the current log directory.
- **size**: Get the current log size limit.
- **format**: Get the current output format

### Set

```bash
auditrs config set <COMMAND> \
  [directory <VALUE>] \
  [size <VALUE>] \
  [format <VALUE> {legacy | simple | json}]
```

- **directory \<VALUE\>**: Set the log directory.
- **size \<VALUE\>**: Set the log size limit.
- **format \<VALUE\>**: Set the output format.

### Filter

```bash
auditrs config filter <COMMAND> \
  [get [VALUE]] \
  [add]  \
  [remove <VALUE>] \
  [import <FILE>] \
  [update <VALUE> <ACTION> {block | allow}]
```

- **get [VALUE]**: Show current filters, optionally for a single value.
- **add \<VALUE\> \<ACTION\>**: Add a filter rule.
- **remove \<VALUE\>**: Remove a filter rule.
- **import \<FILE\>**: Import filter rules from a file.
- **update \<VALUE\> \<ACTION\>**: Update an existing rule.
