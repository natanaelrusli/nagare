---
name: backend-testing
description: Table-driven testing rules for backend code and jobs (handlers, services, repositories, schedulers, consumers). Use when writing or reviewing backend tests.
---

1. Use a table when cases share one behavior and one assertion shape. A case with its own setup, mocks or step sequence gets a separate test.
2. Every row has a unique `name` stating the behavior. 3-15 rows per table, one behavior each. No branching on row fields in the test body.
3. Assert whole values with diff output (Go: `cmp.Diff`, `errors.Is`; pytest: `pytest.param(..., id=)`; vitest: `test.each` with `$name`).
4. Inject clock and IDs; never call `time.Now()` in a row. Go 1.25+: `testing/synctest` for timers.
5. Own tests, not rows: idempotency (run twice), retry exhaustion, timeout, partial failure mid-batch.
6. Repository SQL runs against a real database; the table only varies inputs.
7. Go: go.mod at 1.22+ or copy the loop var before `t.Parallel()`; use slices, not maps, when order matters.

Pure-logic fits: parsers, validators, mappers, money/date math, retry/backoff policy, boundary data (empty, zero, max, DST).
Sources: go.dev/wiki/TableDrivenTests, google.github.io/styleguide/go/best-practices.html, docs.pytest.org parametrize, vitest.dev/api.
