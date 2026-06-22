# marcle_hermes_skill_plugin

Marcle's personal collection of Hermes Agent skills and plugins.

## Standard path convention

All Marcle-created Hermes skills are stored under:

```text
skills/marcle/<category>/<skill-name>/SKILL.md
```

This mirrors the preferred local install path on each device:

```text
~/AppData/Local/hermes/skills/marcle/<category>/<skill-name>/SKILL.md
```

Using the `skills/marcle/` namespace keeps Marcle-created skills separate from bundled/community skills and makes cross-device installation and maintenance consistent.

## Skills

- `skills/marcle/profile-manage/custom-warm-start` — Chinese warm-start onboarding flow for personal/family Hermes profiles.
- `skills/marcle/creative/image-generation-openai` — Text-to-image and image-to-image generation via OpenAI-HK API (gpt-image-2).

## Plugin convention

Future Marcle-created plugins should use a similar namespace when practical:

```text
plugins/marcle/<plugin-type>/<plugin-name>/
```

## General layout

```text
skills/marcle/<category>/<skill-name>/SKILL.md
plugins/marcle/<plugin-type>/<plugin-name>/
```
