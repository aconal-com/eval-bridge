# Changelog

## Unreleased

- Ship `schemas/fixture.schema.json` inside the package (`eval_bridge/schemas/`).
  It lived at the repo root while `package-data` looked under `eval_bridge/`, so
  wheels shipped without it and every `load_fixture` / `capture` / `run` /
  `import` call crashed with `FileNotFoundError` on a pip-installed copy.
  Verified: `pip install` of the 0.5.0 wheel could not load a fixture; the same
  probe passes after the move.
- Fix the PR-comment step in `.github/workflows/ci.yml`: it read `junit.xml`
  while pytest writes `junit-<python-version>.xml`, so the action silently
  warned and exited without ever posting. Point it at the real file and run it
  on a single matrix leg (3.12) so one comment lands per PR.
- Fix `entrypoint.sh` fallback branch: `xml.etree.ElementTree` was imported as
  `sys`, so the "no suites found" path raised `AttributeError` on `sys.exit`.
- Drop `temperature: 0.0` from the LLM-judge request body. Current reasoning
  models reject non-default sampling parameters; the judge prompt already
  demands a single strict JSON verdict.
- Replace deprecated `gpt-4o-mini` (deprecated 2026-10-14, removed 2027-01-14)
  with `gpt-6-luna` — OpenAI's recommended replacement — in the provider,
  runner, config defaults, judge default, example config, README, and fixtures.
- README: install steps no longer claim `pip install eval-bridge` (the package
  is not on PyPI); document the real `import` CLI flags (`--output`, required);
  correct the exit-code summary (3 = hard baseline regression, not silent
  regression — silent regressions are reported without failing).
- `pyproject.toml` project URLs pointed at `aniketkarne-com/eval-bridge`
  (dead); now `aniketkarne/eval-bridge`.
- Add `tests/test_schema_packaging.py` pinning that the fixture schema lives
  inside the installed package.
