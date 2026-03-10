## Feature Parity

In order to have feature parity with auditd, we require a mechanism that enables users to produce log files with outputs based on a collection of rules. Auditd instills its rules at the kernel level, directly telling the kernel which audit items to retain and which to emit. With the filtration being deferred to the kernel, the items logged to `/var/log/audit/` are only those that have been subscribed to by the user.

### Pros of auditd's approach

- Less disk space is required for logs as unrequested audit items are never processed
- Users have a single interface for defining auditd output, `/etc/audit/audit.rules`
- Rotated logs are contain information relevant to users' rules configuration

### Cons of auditd's approach

- Potentially significant audit data is never accessible by users unless subscribed to
  - That is, rules must be predefined and updated proactively to be of value
- The burden of optimal rule management is placed entirely on the user

## A Hybrid Approach

To address the shortcomings of auditd's write path while retaining its benefits, a three-tiered log structure is proposed.

1. **Active Audit Stream**
   - This log contains all audit items emitted by the kernel regardless of user-defined filters or journal rules. It is a pure capture stream and is short-lived and ephemeral.
2. **Audit Journal**
   - The audit journal represents the next step in the evolution of the active stream. All audit items are still retained, and this structure serves as medium-term storage for recovering audit data retroactively and for replaying events under new or updated policies.
3. **Primary Audit Log** (user-facing, in lieu of an archive)
   - As events pass through the active audit stream, they are evaluated against the current filters and rules; events that match are written immediately to the current mutable segment of the primary audit log. In parallel, all events are retained in the audit journal so that the primary audit log (or alternate policy views) can be rebuilt or backfilled by replaying past events under new or updated policies. This tier is the canonical, long-term audit output, analogous to auditd's `/var/log/audit/audit.log`.

### Architectural and naming considerations

- The primary audit log should be treated as the authoritative, user-facing log, with a stable path and format, while the active audit stream and audit journal are internal implementation details that power richer workflows.
- Policy evaluation should support both streaming and batch modes: events can be written to the primary audit log as they pass through the active audit stream, while the audit journal provides a replayable source for retroactively materializing new policy views.
- The policy language and rule model should aim for compatibility with auditd's `audit.rules`, providing a migration path and allowing existing configurations to be translated into this three-tiered architecture.
- Introspection should make it clear why a given event does or does not appear in the primary audit log by tracing its path through the active audit stream, audit journal, and policy evaluation.
- The policy language should be able to express both activity-based and path-based predicates (for example, matching on specific files, directories, or directory trees with glob semantics) so that directory and file watches are first-class concepts rather than ad hoc extensions.
- Higher-level “watch” abstractions, such as a CLI interface for watching a path for a set of activities, should compile down to the same underlying policy engine as lower-level filters, ensuring that options exposed by commands like `auditrs filter` and `auditrs watch` are just different views over a unified rules model.

### Random thoughts on this approach

- Users should have the option to more quickly propagate the audit items defined by their rules and filters to the primary audit log. Maybe instead of having a purely rotation-based sequence, audit items that meet the users' criteria should be written to both the active audit stream and the primary audit log in parallel. Regardless of the details, the user should not be waiting for a large number of audit items to be ingested before the data they care about is flushed to the primary audit log.
- The primary audit log size must be defined in terms of bytes. Defining size in relation to the active audit stream size (as the audit journal may do) is no longer viable as the size of a primary audit log will not always be a factor of the active stream size.
- The CLI needs to intuitively depict the relationship between filters, rules, and the three tiers (active audit stream, audit journal, and primary audit log).
