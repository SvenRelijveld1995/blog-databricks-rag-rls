# Diagrams

Five diagrams, each rendered as PNG and SVG under [`rendered/`](rendered/), as embedded in the
article.

| Diagram | What it shows |
| --- | --- |
| [`build-and-serve`](rendered/build-and-serve.png) | The two halves of the platform: what build writes and what serve reads. Start here. |
| [`governance-boundary`](rendered/governance-boundary.png) | Where Unity Catalog stops governing, and what the index does not inherit. |
| [`acl-flow`](rendered/acl-flow.png) | How the ACL resolves on one request, and where each branch ends. |
| [`architecture`](rendered/architecture.png) | The whole system on one page: identity, build, serve. |
| [`decision-tree`](rendered/decision-tree.png) | Which enforcement path to use, and the host constraint that follows. |

Alongside them, [`chat-response.png`](rendered/chat-response.png) shows the same question answered
for two callers on different projects — [`chat-response.md`](chat-response.md) explains what the
exchange demonstrates.

## Rebuilding them

The `.drawio` files are generated, not drawn by hand, because the brand rule ("lava marks
Databricks, oat marks everything else") and the enforcement rule ("who enforces this?") have to
hold on every one of ~100 cells, and doing that by hand is how inconsistencies creep in.

```bash
cd diagrams
python build.py          # regenerates all five .drawio files
```

`build.py` asserts as it goes: `fits()` fails the build if a label needs more height than its box
has, and `check_layout()` fails it if a connector waypoint lands inside a shape it does not
connect. Both turn a layout bug into a build error rather than something you notice in the render.

Exporting is a separate step, via the draw.io desktop CLI:

```bash
for f in *.drawio; do
  drawio --export --format svg --output "rendered/${f%.drawio}.svg" "$f"
  drawio --export --format png --scale 2 --output "rendered/${f%.drawio}.png" "$f"
done
```

Type sizes live in the `W_*` scale near the top of `build.py`. Changing a size there will usually trip `fits()` on a box or two, which is the
build telling you which ones need more room.

## Sizing

The diagrams keep the lanes side by side, which is how the argument reads best: identity above
build above serve, each as one horizontal band. The type is scaled up against the canvas, and the
boxes given the vertical room that needs, so it stays legible once the image is fitted to an article
column.

## Two colour rules, and they never disagree

All five follow the Databricks brand system, which has one rule at its centre:

> **Lava `#FF5F46` marks Databricks. Oat `#D9D7CE` marks everything that is not.**

On top of that, they use **border colour to say who enforces access control** — which is the
argument the article makes, rendered as a visual convention:

| Border | Meaning |
| --- | --- |
| **Green** `#00A972` | **Unity Catalog enforces.** Per caller, platform-side, gives notice when it goes wrong. |
| **Lava** `#FF5F46` | **Your code enforces.** Only as good as you wrote it. |
| **Navy** `#1B3139` | **Nothing enforces.** The security plane does not reach here. |
| **Oat** `#D9D7CE` | Outside Databricks entirely. |

"Nothing enforces" used to be deep lava `#FF3621`, one shade from the lava beside it — on screen
the two read as a single colour, so the legend distinguished nothing. Navy separates cleanly and is
already in the brand set.

Read the architecture diagram by border colour before you read it by arrow. Every silent failure
described in the article sits on a border the legend calls out — that is not decoration, it is the
whole point.

## Icons and typography

Every icon is official vendor artwork, embedded rather than fetched. The brand font is
[DM Sans](https://fonts.google.com/specimen/DM+Sans).

| Source | Used for |
| --- | --- |
| Databricks architecture icons | AI Search, Unity Catalog, ABAC, Genie, Model Serving, Databricks Apps, SQL Warehouse, Lakebase, Agent Bricks |
| Microsoft 365 content icons (Teams Purple) | the front end, the person mark |
| Azure Public Service Icons V24 | App Registrations, for the token exchange |

Two notes on what is deliberately absent. There is no plain "Entra ID" mark in the Azure V24 set,
so the RFC 8693 exchange is drawn with **App Registrations** — which is more accurate anyway, since
an app registration is exactly what the federation policy trusts. And the front-end box is labelled
"Front end · web UI, Teams, …" but still drawn with the Teams mark, because a recognisable product
reads faster than a generic client glyph. Teams is only an example of what sits there: a custom web
UI takes the same position and needs the same token federation to get a Databricks token.

The Databricks artwork comes from the community-maintained
[databricks-architecture-icons](https://github.com/oieduardorabelo/databricks-architecture-icons)
collection, which repackages official SVGs without redrawing them. That set is **unofficial** —
Databricks neither sponsors nor endorses it. Databricks, the Databricks logo and the product names
are trademarks of Databricks, Inc. Microsoft, Teams, Azure and Entra are trademarks of Microsoft
Corporation. Check both vendors' brand guidelines before commercial use.
