# GPT Image model selection and API notes

Checked against official OpenAI documentation on 2026-09-09. This is the active
API reference for this skill; `openai-cookbook.md` remains an earlier GPT Image 2
cookbook, not a current model catalog. The [2.5 index](openai-image-2.5.md) is
optional; use [migration notes](openai-image-2.5-migration.md) only for comparisons.

## Resolve the model for this task

| Choice | Exact API model ID | Typical use |
|---|---|---|
| Flare | `gpt-image-2.5-flare` | Fast, everyday generation |
| Sunburst | `gpt-image-2.5-sunburst` | Precise edits and detail-sensitive work |
| Image 2 | `gpt-image-2` | Existing workflows and explicit compatibility requests |

The 2.5 models also have snapshots ending in `-2026-09-08`. There is no documented
bare `gpt-image-2.5` API alias. A name such as “GPT 2.5” names a family rather
than an exact API ID. Use an existing matching variant preference, or clarify
the variant when no preference resolves it; do not invent an alias.

Use the user's current explicit choice, then the model established for this
task, then `GPT_IMAGE_MODEL`, then the CLI compatibility default `gpt-image-2`.
An unspecified model alone does not require a question. If a named family has
several variants and no matching preference resolves it, ask about that variant
only. Clarify a probable typo rather than silently correcting a model ID.

`--model` overrides `GPT_IMAGE_MODEL`. The CLI loads process env, project `.env`,
then `~/.env` without replacing existing values, so a saved model preference can
be reused. This table describes the models covered by these dated notes, not
an exhaustive allowlist. Check newer model contracts when needed; do not
substitute an older model merely because this document does not list it.

Keep model selection outside the image prompt. User intent determines whether
to read creative references; the model determines applicable API parameters.
Do not generate extra variants or change models without authorization. On API
errors or uncertain timeouts, report the outcome rather than rewriting the
prompt or replaying a potentially billable request. SDK retries are disabled.

## Parameters

Both 2.5 models use the existing `/v1/images/generations` and `/v1/images/edits`
endpoints. No Responses API migration is needed.

| Parameter | Contract |
|---|---|
| `quality` | 2.5: `low`, `medium`, `high`, `xhigh`, `max`, `auto`. Image 2: through `high` plus `auto`. CLI default remains `high`. |
| `size` | `auto`, a CLI shortcut, or `WIDTHxHEIGHT`; see limits below. |
| `background` | `auto`, `opaque`, `transparent`. Transparency requires `png` or `webp`, not `jpeg`. |
| `output_format` | `png` (default), `jpeg`, `webp`. |
| `output_compression` | Integer 0–100 for JPEG/WebP. Do not request compression for PNG. |
| `n` | 1–10; default 1. More images require the user's authorization. |
| `user` | Optional end-user identifier, forwarded unchanged. |
| `moderation` | `auto` or `low`; this CLI exposes it for generation only. Existing CLI default remains `low`. |
| `input_fidelity` | Edits only, omitted by default. See the qualification below. |

For Image 2 and 2.5, both edges must be positive multiples of 16 and at most
3840 pixels, the long/short ratio at most 3:1, and total pixels between 655,360
and 8,294,400 inclusive. Above 2560×1440 is experimental. A size shortcut controls
the actual request; saying “4K” inside a prompt does not set the API dimensions.

For references, repeat `-i`; for a mask, use `-i` plus `-m`. Validate the actual
input image and mask requirements before a live call. The CLI checks file
existence; that alone does not prove valid format, alpha channel, or size.

### Parameters requiring care

- **Input fidelity:** official documentation explicitly requires omission for
  `gpt-image-2`. The CLI drops it only for Image 2 and its dated snapshots, not
  all names starting with `gpt-image-2`. Explicit `low`/`high` on 2.5 is passed
  through as an opt-in request, not advertised as verified API support. Prefer
  omission until the chosen model's behavior is confirmed.
- **Edit moderation:** the generic HTTP reference lists it, but the Python
  edit signature used here does not. Do not add it to edits merely because the
  generation method accepts it.
- **Cost:** equal per-token rates do not imply equal per-image cost. Compare
  returned usage for the selected model, size, and quality. Do not promise a
  fixed multiplier per quality level or assume Flare is always cheaper.
- **Output:** save `data[].b64_json`; a successful response is not proof that
  text, identity, or layout is correct. Check the actual image. Do not add
  `response_format` for GPT Image models.

## Validation boundary

The dependency floor is `openai>=2.32.0`. Offline request-serialization tests
pass on both 2.32.0 and the freshly resolved 3.10.0 (which uses httpx2). This is
a tested version pair, not a claim that 2.32.0 introduced every 2.5 feature or
that all intervening and future versions were tested.
Run `PYTHONPATH=src python -m unittest discover -s tests -v` for offline tests.
They use in-process HTTP responses, no credentials or paid image generation.

Live checks on 2026-09-09: the model list omitted 2.5 and
`models.retrieve` returned `404 model_not_found`, while the repository CLI
successfully completed three Sunburst generation requests and two reference edits
(HTTP 200, `quality=high`, PNG, one image per request). The initial three runs
did not record credential fingerprints. The two replacement-showcase runs
captured a private fingerprint and passed the verified credential to the CLI.
A separate read-only audit verified the current local credential and account
through `/v1/me` and `/v1/models` (HTTP 200). Model metadata lookup
alone cannot establish generation availability. See the
[samples, exact settings and visual checks](../../../docs/sunburst-samples.md).

These runs cover Sunburst generation and single-reference editing. Flare,
masks, transparency, `xhigh`/`max`, explicit 2.5 input fidelity, and the separate
community templates still require live validation. The poster's knife shadow
points in the wrong direction despite successful generation.

Before release testing, verify the effective key source, endpoint, project
and organization. Keep a non-secret credential fingerprint in private run
evidence; never publish keys or account details. Report blocking failures
before proposing a release. Run a separately authorized small live sample for
each selected model and endpoint. Record exact model, prompt, size, quality,
response/error, and output path. Check transparency, exact text, requested edits and preserved regions.
Do not treat mocked requests, model listings, or another author's examples as
successful local image generation.

## Official sources

- [Flare model](https://developers.openai.com/api/docs/models/gpt-image-2.5-flare)
- [Sunburst model](https://developers.openai.com/api/docs/models/gpt-image-2.5-sunburst)
- [Image generation guide](https://developers.openai.com/api/docs/guides/image-generation)
- [Python generation reference](https://developers.openai.com/api/reference/python/resources/images/methods/generate)
- [Python edit reference](https://developers.openai.com/api/reference/python/resources/images/methods/edit)
- [Image prompting guide](https://developers.openai.com/api/docs/guides/image-prompting)
