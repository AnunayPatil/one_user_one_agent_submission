# Known limitations

- The included data is synthetic and only demonstrates the expected schema; it is not the challenge pack.
- Goal rules are illustrative. Inspect the supplied goal catalogue and document the two underspecified
  rules and the unavailable success signal before final submission.
- The demonstration uses simple goal rules and title selection, not a learned ranking model.
- It does not implement distributed locking, a production message provider, or horizontal scaling.
- Timestamps are expected in ISO-8601. Event time and receipt time are stored separately, but extensive
  clock-skew correction is not implemented.
- Outcome attribution is basic; causal conclusions require the real randomized holdout fields and
  complete observation windows.