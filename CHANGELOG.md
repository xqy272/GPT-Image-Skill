# Changelog

## v0.3.0 — 2026-11-06

- Route generation, editing, directions, and prompt-only requests by user intent.
  Preserve finished prompts and consult creative references only when helpful.
- Add optional `GPT_IMAGE_MODEL`, including existing `.env` loading, with
  explicit `--model` taking precedence and `gpt-image-2` retained as fallback.
- Keep gallery numbers, sample images, original prompts, and attribution;
  clarify optional reading and add a small visual-problem index.
- Make image-processing and additional-generation boundaries explicit.
- Point fork installation metadata and launcher fallback to xqy272/GPT-Image-Skill.
  Add offline coverage and manual Agent/visual comparison scenarios.

## v0.2.0 — 2026-04-25

This release collects the recent documentation, gallery, skill, and discoverability updates after the initial public release.

### Highlights

- **Full split Reference Gallery Atlas** — expanded the skill references into category-level `gallery-*.md` files so agents can load the relevant prompt slice without filling the context window.
- **README selected showcase refresh** — compacted the README into representative visual panels while keeping the full 162-prompt catalog in the Reference Gallery.
- **New and reorganized gallery material** — added/updated examples for research paper figures, scientific education, screen photography, gaming HUDs, events maps, beauty/lifestyle, cinematic references, and more.
- **Codex installation clarity** — clarified that `$skill-installer` is a built-in Codex skill-management helper and added a manual Codex fallback path.
- **Bilingual README navigation** — added visible English / 中文 switching near the README top.
- **SEO and discovery polish** — added natural search terms, expanded package/plugin keywords, GitHub topics, and Star History.

### Merged PRs

- #1 — Sync the full prompt gallery into the skill reference atlas
- #3 — Polish README showcase and sync split gallery atlas
- #4 — Add README language switch
- #5 — Clarify Codex skill installation
- #6 — Improve README SEO and add star history
