# Domain docs

## Before exploring

Read `GLOSSARY.md` at the repo root and any ADRs in `docs/adr/` relevant
to the work.

If these files do not exist, proceed silently. `/domain-modeling` creates
them lazily when terminology or decisions are resolved.

## Layout

This is a single-context repository:

- `GLOSSARY.md`: shared domain vocabulary.
- `docs/adr/`: architecture decision records.

## Vocabulary

Use the glossary's terms in issue titles, proposals, hypotheses, and tests.
If a needed concept is missing, reconsider the terminology or note the gap
for `/domain-modeling`.

## ADR conflicts

Surface contradictions with existing ADRs explicitly, naming the ADR and
explaining why reopening the decision may be warranted.
