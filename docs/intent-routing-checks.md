# Intent-routing checks

These are manual Agent scenarios, not a runtime classifier or an automated
quality score. Use the same request, model, available inputs, and authorized
generation count when comparing the previous and revised Skill. Record the
actual reads, questions, prompts, reference order, image calls, and outputs.
Mock image tools are sufficient to check interaction and call boundaries;
they cannot establish visual quality or preservation.

| Request | What to check |
|---|---|
| Generate the supplied astronaut-cat prompt verbatim | Unchanged prompt, one generation, current task/default model, no compulsory gallery pass or model menu |
| Create a relaxed community movie-night poster titled “晚风放映室” | Concrete, coherent composition; exact supplied title; no invented date or sponsors; a gallery visit is optional |
| Give two autumn reading-club cover directions and wait for my choice | Two visibly different arrangements or stories, not only palettes; no image call |
| Use the teapot in A and the morning light in B | Inspect both inputs; assign product and lighting roles; preserve input order; do not import B's product or copy |
| Keep the person and lettering; only change the background | Attach the actual selected image to the edit call; preserve prior version; inspect subject and copy drift |
| Draw chapters on germination, pollination, and fruiting | Three distinct narrative roles with shared visual anchors, within the requested count |
| Request 1024x1024; tool returns another size | Show the original and real dimensions; no automatic crop, padding, resize, or extra paid generation |
| Tool times out or rejects a model | Explain the outcome; no silent replay or model switch |

For visual review, compare actual images at readable zoom, including edit
targets side by side. Judge whether each picture expresses the brief, not
whether it follows one example composition. AI-call success or a shorter
Skill file does not establish better artwork.

The offline CLI tests exercise request serialization, model preference
precedence, verbatim multiline text, input order, unmodified returned bytes,
and error/timeout call counts. They do not simulate an Agent reading the
Skill or constitute a blind old/new artwork comparison. Live comparisons
need an explicit generation budget and suitable reference assets.

## Observed run — 2026-11-06

Two independent Agents ran the first six requests against the upstream
`9f8aa1a` Skill and this revision using a mock provider. The old Skill asked
for a model despite an environment preference, and asked for the series'
style before generating. The revision used the preference and generated the
three distinct chapters. Both versions made zero image calls for directions
alone and passed the specified edit inputs in order. Mock outputs were not
used to judge artwork.

Their exact prompts were then replayed through each version's launcher:
seven live calls for the revision and two old-version comparison calls
(poster and background edit), using `gpt-image-2.6-flare` on a custom
OpenAI-compatible route. All nine returned images. Both posters had readable
titles; the revision's product/light reference roles and chapter series
were visually recognizable. Both background edits repainted protected
subject details, including the revision's masked edit. Neither met a strict
unchanged-person requirement. Drawn fixtures were used, not product photos.

All requests specified 1024x1024 and medium quality. Actual images were
1199x1312 or 1254x1254; six responses reported low quality in metadata.
Captured requests confirmed the requested parameters, image order, and
mask. Saved originals matched the raw response image hashes; none were
cropped, resized, padded, or retried. This single pass supports the intended
interaction changes, not an overall quality advantage or pixel locking.
It was not a blind comparison or a live multi-turn Agent evaluation.
