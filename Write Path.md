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

To aaddress the shortcomings of auditd's write path while retaining its benefits, a three-tiered log structure is proposed.

1. **The Active Log**
   - This log contains all audit items emitted by the kernel regardless of user-defined filters or journal rules. Short-lived items.
2. **Journal Logs**
   - Journal logs represent the next step in the evolution of an active log. All audit items are still retained. This structure is effectively medium-term storage for recovering audit data retroactively (we need to think through what configurations can be made to the journal).
3. **User Logs** (in lieu of an archive)
   - After a journal log is rotated out of the journal via a FIFO policy, filters and rules are applied to its contents. By the end of this process, only the data explicitly desired by the user remains in the user log.

### Random thoughts on this approach

- Users should have the option to more quickly propagate the audit items defined by their rules and filters to the user log. Maybe instead of having a log rotation sequence, audit items that meet the users' criteria should be written to both the active log and the user log in parallel. Regardless of the details, the user should not be waiting for a large number of audit items to be ingested before the data they care about is flushed to the user log.
- The user log size must be defined in terms of bytes. Defining size in relation to the active log size (as the journal logs do) is no longer viable as the size of a user log will not always be a factor of the active log size.
- The CLI needs to intuitively depict the relationship between filters and rules.
