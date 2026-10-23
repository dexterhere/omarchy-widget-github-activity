# GitHub activity

Two numbers from GitHub: how many pull requests you have open, and how many are waiting for your review. A **spec widget**: three JSON files, no code, no network, no files, no programs.

Widget id: `io.github.dexterhere.github-activity`

## Status (honest)

- **Planned, version 0.1.0.** Registry status should be **planned**, not alpha.
- It is planned because the GitHub provider and `ow connect github` are not built yet. Today the daemon cannot read a credential vault or contact GitHub, so nothing here can show real data.
- The desktop renderer for spec widgets does not exist yet either. The layout is only drawn by the website emulator, from sample data (4 open, 2 to review).
- Spec widget, no code. There is no code in this repository.
- This listing makes no claim that the widget is safe, reviewed or sandboxed.

## Your GitHub token (read this first)

- **The widget never receives your GitHub token.** It does not see it, name it or ask for it. It only declares that it reads the `github` provider's pull request counts.
- **The daemon holds the token and contacts api.github.com for you.** The widget has no network access at all (`capabilities.network` is empty). The daemon hands the widget the two numbers and nothing else.
- You create the token yourself: a **fine-grained, read-only** personal access token with **Pull requests: read** (GitHub adds Metadata: read automatically), a short expiry, and your own account as the resource owner. Nothing else. See the project docs (the auth plan) for the step-by-step recipe in plain words. Do not use a classic token.
- Honest limit: a token with Pull requests: read can read more than two counts (titles, diffs, comments of pull requests in scope). GitHub cannot narrow a token to counts only. The daemon narrows what it passes on to two numbers, but the token itself is broader than this widget needs. Limit it to the repositories you care about, and delete it on GitHub when you stop using the widget.
- A process running as you could still claim this widget's id and ask for the same two numbers. Grants protect the token, not the counts.
- Never paste a token into an issue, a layout file or a setting. This widget has none to paste into.

## What the daemon will serve

| Topic | Fields | Provider |
| --- | --- | --- |
| `github.prs` | `open`, `review` | `github` (personal, credential from the vault) |

These are the only fields the catalog has. The widget shows two numbers only.

## Variants

The default (`root`) is `counts`. Size classes: `chip` (bar), `compact`, `wide`.

| Variant | Shows |
| --- | --- |
| `counts` (default) | Two big numbers side by side: open, to review |
| `badge-review` | Title, open count, and the review count as a highlighted badge |
| `chip` | One-row bar chip: icon, open, divider, review |
| `minimal` | Numbers only: open / review |
| `detailed` | Title, divider, then a labelled row for each number |

### Not included: ring pair

A ring draws a percentage, and a pull request count is not one. Showing 4 open as a ring would be misleading, so there is no ring variant. Add one if the catalog gains a percentage field.

## Settings

None. The layout reads no setting, so none is declared. Customise by choosing a variant.

## Access

Reads `github.prs` fields `open` and `review`, with `auth: vault`. Network: none. Files: none. Programs: none.

## Install and removal

There is no installer yet (`ow install` is planned), and `ow connect github` is planned. To try the layout, run the website registry check against this folder; see "Verify". To remove it, delete this folder. Nothing is written elsewhere.

## Verify

The helper from the website docs ("Check and preview", option A), run from outside both repositories:

```bash
cd /tmp && WEB=/path/to/omarchy-widgets-web bun check-local.ts /path/to/github-activity
```

## License

MIT, see `LICENSE`.
