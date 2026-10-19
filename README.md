# One User, One Agent — runnable starter submission

A small Python implementation of the assignment's decide → verify → act → measure loop.
It uses only the Python standard library. The bundled data is **synthetic demonstration data**,
not the hiring challenge's missing real data pack. Replace the files in `data/` with the supplied
pack and adapt `src/agent.py::load_inputs()` if its schemas differ.

## Requirements
- Python 3.10+
- No third-party packages required

## Run
From this directory:

```bash
python -m src.main --data-dir data --out-dir sample_run --db sample_run/agent.sqlite --reset-db
```

Run tests:

```bash
python -m unittest discover -s tests -v
```

## Input JSONL files
- `events.jsonl`: one event per line. Expected fields: `event_id`, `viewer_id`, `event_type`,
  `event_time`, optional `received_at`, `dialect`, `title_id`, `full_episode`, `paid`.
- `profiles.jsonl`: one profile per line: `viewer_id`, `signup_at`, `subscription_status`,
  `preferred_dialect`, `language`.
- `catalogue.jsonl`: title records with `title_id`, `dialect`, `available_from`, `available_until`,
  `active`, `genre`.
- `goals.json`: list of goal definitions.
- `interventions.json`: policy configuration.

## Safety/engineering notes
- Deterministic rules own eligibility, success checks and policy enforcement; no LLM is required.
- Duplicate events are idempotent by `event_id`.
- Current event app setting takes precedence for the current experience's dialect; profile is fallback.
- Messages are blocked during local quiet hours (22:00–08:00), without consent, or if a message was
  already sent that local day.
- This is a demonstration baseline, not production-ready. The real pack may have different schemas
  and the assignment's two underspecified rules must be explicitly documented after inspecting the pack.