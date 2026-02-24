# Record Lifecycle

An audit event emitted by the Linux kernel goes through various stages in auditrs before they are ultimately presented to users. The details of those stages and the rationale behind them are described in this document.

### Overview

```
Kernel -> RawAuditRecord -> ParsedAuditRecord -> AuditEvent -> Output
```

### Definitions

- **Kernel:** When referencing the kernel in auditrs, we are usually referencing the Netlink socket. Netlink provides an interface for interprocess communication between kernel space and user space (where auditrs sits). Specifically, we rely on a Netlink socket to the Linux audit subsystem, which is the operating system's built-in auditing tool.

- **Raw Audit Record:** Audit records received from the kernel prior to modifications by auditrs.

  ```rust
  struct RawAuditRecord {
      record_id: u16,
      data: String,
  }
  ```

- **Parsed Audit Record:** The finalized version of individual records following a series of transformations. Some examples of potential transformations include:
  - Record enrichment
  - Typed data fields
  - Syscall name resolution
  - Quality of life improvements

  ```rust
  struct ParsedAuditRecord {
       record_type: RecordType,
       timestamp: SystemTime,
       serial: u16,
       fields: HashMap<String, String>,
   }
  ```

- **Audit Event:** A group of correlated, parsed audit records. For any given action that triggers auditing by the kernel, the kernel usually does not emit a single record. Instead, it creates several, often overlapping, audit records that share a common source and identical timestamps and serial ids.

## Implementation Details
