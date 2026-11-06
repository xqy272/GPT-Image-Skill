---
name: gpt-image
description: "Generate and edit images with the packaged GPT Image CLI, or develop visual directions and prompts without generating. Use for illustrations, posters, typography, reference edits, inpainting, and related image requests. Draw on the prompt gallery and task references when useful."
metadata: {"openclaw":{"requires":{"anyBins":["gpt-image","uv","uvx"]},"primaryEnv":"OPENAI_API_KEY","homepage":"https://github.com/xqy272/GPT-Image-Skill"}}
---

# GPT Image

Deliver the image, edit, visual direction, or prompt the user asked for. Use the existing CLI for image calls. Requires Python 3.11+ and the CLI dependencies; API calls use `OPENAI_API_KEY` and may incur charges.

## Work from the user's intent

- For a finished prompt requested verbatim, send the original text, including its language and exact copy. Keep execution settings separate. Do not translate, polish, or consult style examples to rewrite it.
- For a rough brief, make concrete creative choices that serve the subject and intended use: a readable composition, a telling action, a coherent visual medium. Preserve supplied text and constraints. Missing camera, palette, or setting choices are usually creative space; unreadable factual copy or an ambiguous edit target needs clarification.
- For directions or prompt-only work, deliver concrete ideas or prompts without calling the image API. Differences between directions should concern the picture's organization or story, not only its colors.
- For a follow-up edit, use the actual image the user refers to as the edit input. State what changes and what stays. Keep earlier versions available.
- Inspect supplied images and distinguish the base image from identity, composition, lighting, style, and object references. Keep their roles and repeated `-i` order consistent; borrowing light from a reference does not borrow its product or text.
- For a requested series, share appropriate visual anchors while giving each image its own subject, action, or narrative role.

## Use experience when it helps

A clear request can go straight to the CLI with any model. Reading references is optional and follows the visual problem, not the model version.

| Need | Read only the relevant material |
|---|---|
| A named case or visual technique | Its entry in [the gallery](references/gallery.md); go directly to a known case |
| Composition, lettering, visual hierarchy, or series consistency | The matching section of [craft](references/craft.md) |
| Photo, product, or illustration prompting | [Generation notes](references/openai-image-2.5-generation.md) |
| Text, UI, diagrams, or panels | [Layout and text](references/openai-image-2.5-layout-and-text.md) |
| Reference roles, preservation, masks, or cutouts | [Editing notes](references/openai-image-2.5-editing.md) |
| API parameters or model-specific limits | [Model notes](references/models.md) and the CLI's `--help` |

Read a selected case's original prompt when using it. Borrow the useful method, not its unrelated subjects, slogans, prices, or branding. Reading a sample does not authorize attaching its image. Templates are adaptable starting points; keep their attribution and stated limits. If nothing fits, continue from the brief.

## Execute with the existing CLI

Use the current explicit model choice, then the choice established for this task, then `GPT_IMAGE_MODEL`, then the CLI's compatibility default (`gpt-image-2`). Missing a model alone does not require a menu. If the user specifies a family with multiple variants and no matching preference resolves it, ask only which variant. Preserve an exact model ID rather than inventing or silently substituting one.

```bash
# Installed CLI; --model overrides GPT_IMAGE_MODEL
gpt-image -p "PROMPT" -f output.png [--model MODEL_ID]

# This skill's launcher; use its runtime-provided path
uv run "$SKILL_DIR/scripts/generate.py" -p "PROMPT" -f output.png

# Edit; additional -i inputs keep their stated roles. A mask applies to the first.
gpt-image -p "CHANGE; KEEP ..." -i base.png [-i reference.png] [-m mask.png] -f edited.png
```

No `-i` uses `/v1/images/generations`; `-i` uses `/v1/images/edits`. For this fork's checkout or an isolated verification, use its launcher so a different PATH installation cannot take precedence. Inspect `--help` for flags instead of writing another API client. Preserve the existing endpoint, credential, and proxy configuration. Installation and credential-file changes belong to setup requests, not ordinary image work; never print keys.

## Inspect and deliver

Show the saved image and report the path(s). Check what matters for this request: exact lettering, subject/reference identity, relationships, composition, and edit preservation. Describe visible deviations honestly; API success is not visual success.

Request the intended size and, when composing a prompt, express the intended ratio in it. Inspect and report the actual dimensions when size matters. Returned dimensions may differ with some providers or subscription-backed routes. Exact-size requirements alone do not authorize cropping, padding, resizing, or another paid generation; preserve the original and explain the difference. Image processing is a separate requested action.

Generate the requested count, one image when unspecified. Visual inspection does not authorize additional paid variants, a model switch, or automatic refinement. On an API error or an uncertain timeout, report it; do not silently resubmit, switch models, or rewrite the prompt to retry.
