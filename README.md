# Row-level security on a Databricks RAG agent

Two colleagues ask the same chatbot the same question. Should they get the same answer, or
different ones because they are allowed to see different things?

Unity Catalog answers that well for tables — row filters and column masks, evaluated per caller.
It does not answer it for a vector index, which has grants but no row filters, so the access
decision moves out of the platform and into code you write. This is a write-up of building that
access control on Databricks, reachable from a front end outside the platform — a custom web UI or
a client like Microsoft Teams, once token federation is enabled — and of measuring whether it
actually holds.

## The article

**[Read the article](article/row-level-security.md)** (~21 min), also published on
[sven-relijveld.com](https://www.sven-relijveld.com/content/row-level-security-databricks-rag-agent).

## What else is here

| Directory | What it holds |
| --- | --- |
| [`examples/`](examples/) | runnable demonstrations of each failure the article describes, including a Unity Catalog RLS fixture with its negative tests |
| [`diagrams/`](diagrams/) | all five diagrams the article embeds, as PNG and SVG, drawn on one brand system |
| [`tests/`](tests/) | the ACL test suite, including the loose group-name matching it rejects |

Run the examples and the tests:

```bash
uv run --with pytest pytest        # 25 tests, no Databricks connection needed
python examples/03_acl_from_groups.py
```

## About

Written by [Sven Relijveld](https://www.sven-relijveld.com).

Identifiers, group names and project data are generalised for publication. Licensed
[MIT](LICENSE) — lift anything here into your own work.
