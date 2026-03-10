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
- filter
- watch

#### Configuration

- config

---

## Query

Commands for interacting with `auditrs` event logs.

> These are not yet fully implemented, the flags for these commands are likely to be replaced with an interactive command line question set.

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

Commands for manipulating `auditrs` configuration. These settings control where each tier of the logging pipeline stores data and how much space each tier is allowed to consume, as described in the three-tiered write path (`Active Audit Stream`, `Audit Journal`, and `Primary Audit Log`).

```bash
auditrs config <COMMAND> { get | set }
```

### Get

```bash
auditrs config get <COMMAND> \
  { format \
  | active-directory | journal-directory | primary-directory \
  | active-size | journal-size | primary-size }
```

- **format**: Get the current output format for primary audit log records.
- **active-directory**: Get the current storage directory for the Active Audit Stream.
- **journal-directory**: Get the current storage directory for the Audit Journal (medium-term, full-fidelity storage).
- **primary-directory**: Get the current storage directory for the Primary Audit Log (user-facing, canonical log).
- **active-size**: Get the current size limit for the Active Audit Stream.
- **journal-size**: Get the current size limit for the Audit Journal.
- **primary-size**: Get the current size limit for the Primary Audit Log.

### Set

```bash
auditrs config set <COMMAND> \
  { format \
  | active-directory <VALUE> \
  | journal-directory <VALUE> \
  | primary-directory <VALUE> \
  | active-size \
  | journal-size \
  | primary-size }
```

- **format**: Set the output format for the Primary Audit Log (interactive).
- **active-directory \<VALUE\>**: Set the storage directory for the Active Audit Stream.
- **journal-directory \<VALUE\>**: Set the storage directory for the Audit Journal.
- **primary-directory \<VALUE\>**: Set the storage directory for the Primary Audit Log.
- **active-size**: Set the size limit for the Active Audit Stream (interactive).
- **journal-size**: Set the size limit for the Audit Journal (interactive).
- **primary-size**: Set the size limit for the Primary Audit Log (interactive).

---

## Filter

Commands for managing `auditrs` log filters. Filters are defined over record types and fields described in the reference documentation:
`https://github.com/Rowdy-Rustiles/docs/blob/main/Reference/Record%20Types.md`.

```bash
auditrs filter <COMMAND>
```

### get

```bash
auditrs filter get [VALUE]
```

- **VALUE** (optional): If provided, show only filters matching this value; otherwise, list all filters.

### add

```bash
auditrs filter add
```

- Starts an interactive sequence of prompts to define a new filter rule. The CLI will guide the user through choosing a record type, selecting fields and match conditions (including activity- and path-based predicates), and specifying whether matching events should be included in or excluded from the primary audit log.

### remove

```bash
auditrs filter remove [VALUE]
```

- **VALUE** (optional): Record type or filter value to remove. If omitted, `auditrs` will interactively present existing filters to remove.

### import

```bash
auditrs filter import <FILE>
```

- **FILE**: File to import filters from (`.ars`, `.toml`, `.rules`).

### dump

```bash
auditrs filter dump <FILE>
```

- **FILE**: File to dump filters to (omit file extension; the appropriate extension will be added automatically).

---

## Watch

Path- and directory-based watches are a higher-level interface built on top of the same underlying policy engine as filters. They provide an auditd-like way to say “always log certain activities on this file or directory.” Watches do not change the underlying storage configuration; they only influence which events are selected into the primary audit log.

```bash
auditrs watch <COMMAND>
```

### list

```bash
auditrs watch list
```

- Show all configured watches.

### add

```bash
auditrs watch add
```

- Starts an interactive sequence of prompts to define a new watch. The CLI will ask for the file or directory path, the activities to watch (e.g. `exec`, `chmod`, `chown`), whether the watch should apply recursively, and an optional human-readable key. Matching events are always written to the primary audit log.

### remove

```bash
auditrs watch remove <KEY>
```

- **KEY**: Remove the watch associated with the given key.
