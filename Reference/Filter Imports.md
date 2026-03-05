# Importing filters from files

Auditrs currently supports importing rules from two different file formats - **\*.ars** and **\*.toml**. Properly importing filters from each of these files requires their contents to follow specific formatting. Future development plans aim to bring backwards compatability with auditd's rules system.

### General Filter Arguments

- **Action types: {block | allow}**
- **Record types: any type defined in the [Record Types Reference](https://github.com/Rowdy-Rustiles/docs/blob/main/Reference/Record%20Types.md)**

> Both arguments are case-insensitive

## TOML

The formatting for an imported TOML filters file must match that of the Filters.toml that is generated when manually adding rules to auditrs via the CLI. The format follows as such:

```toml
[[filters]]
action = "block"
record_type = "syscall"

[[filters]]
action = "block"
record_type = "add_group"

[[filters]]
action = "block"
record_type = "cwd"

[[filters]]
...
```

## ARS

The ARS (Audit RS) format is meant to be an ergonomic, simplified representation of filters for easy readability and sharing. Filters are represented as key-value pairs separated by colons; `record_type: action`.

```ars
syscall: block
add_group: block
cwd: block
...
```

### Examples

Examples of both import formats are available for viewing [here](https://github.com/Rowdy-Rustiles/auditrs/tree/master/import).
