# Architecture

1. **Trigger ingestion**: read replay events in supplied order. `event_id` is the idempotency key.
2. **Viewer memory**: SQLite stores processed events, one current active plan per viewer, decisions,
   actions and outcome records.
3. **Evidence resolution**: each decision records event/profile evidence references. Current event's
   app dialect is used for the current experience; profile dialect is fallback. Conflicts are retained.
4. **Goal selection**: deterministic eligibility rules select at most one active goal. A completed or
   purchased viewer is not offered a goal that contradicts their state.
5. **Planning**: a home-page row is preferred when at least three available titles match the dialect.
   Messaging is a fallback only when explicit consent, a configured template, quiet hours and daily cap
   checks pass. Otherwise the agent abstains.
6. **Independent verification**: a separate verifier checks goal eligibility, content dialect/availability,
   row size, message consent, quiet hours, daily cap and duplicate action keys.
7. **Execution**: replay execution records approved actions rather than contacting real users.
8. **Outcome evaluation**: completion and paid events are checked against the selected goal deadline.
   Outcomes are marked succeeded, failed or inconclusive.
9. **Impact**: report intention-to-treat rates using `experiment_group` from profile data where present.
   A real causal report requires the supplied randomized assignment and complete outcome windows.
10. **Failure handling**: the implementation is deterministic and needs no model provider. Production
    adapters should use bounded timeouts/retries and fall back to no action. SQLite transaction locking
    should be strengthened for concurrent workers.

Untrusted catalogue text is treated as data only and never executed as instructions.