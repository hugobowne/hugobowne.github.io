---
name: create-agent-skills-guest-page
description: Create a static Show Us Your Agent Skills guest dossier page from guest field notes, episode context, avatar thumbnails, and optional 8-bit guest videos. Use when asked to prototype or generate a per-guest page for this site, especially from episode-field-notes markdown, YouTube timestamps, skills/workflows, and guest media assets.
---

# Create Agent Skills Guest Page

## Core Rule

Create an unlinked prototype unless the user explicitly asks to add navigation, roster links, index links, or main-page links. Do not wire the page into `agent-skills.html` by default.

## Inputs

Use some or all of:

- Guest field-notes markdown, usually from `episode-field-notes/ep-N/<guest>.md`.
- Existing avatar thumbnail, usually under `show/avatars/<guest>.png|jpg`.
- 8-bit video, usually placed in the guest page folder or supplied by the user.
- Episode metadata from `agent-skills.html`: episode number, title, YouTube URL, guest roster, skill links.

If the 8-bit video is mentioned but not present, search local likely locations first. If still missing, ask where the user wants to place it. Prefer this destination:

```text
agent-skills/guests/<guest-slug>/<guest-slug>-8bit.mp4
```

Use the avatar as the poster/fallback:

```html
<video poster="../../../show/avatars/<avatar>" autoplay muted loop playsinline controls preload="metadata">
  <source src="<video-file>.mp4" type="video/mp4">
  <img src="../../../show/avatars/<avatar>" alt="<Guest Name>">
</video>
```

## Page Location

Create one directory per guest:

```text
agent-skills/guests/<guest-slug>/index.html
```

Use clean relative paths from that nested page:

- `../../../agent-skills`
- `../../../index.html`
- `../../../show/avatars/<avatar>`
- `../../../shared/shared.css`

## Page Shape

Build a compact dossier, not a generic bio page and not a transcript dump.

Recommended sections:

- Hero: guest name, affiliation, episode, topic chips, static mini-avatar, main 8-bit video.
- Short deck: what the guest did with agents, in plain language.
- Primary CTAs: `Watch segment`, skill/repo link if available.
- `What He/She/They Showed`: 2 direct paragraphs grounded in the notes.
- Key moments: 4-8 timestamp cards.
- Skills and workflows: cards for concrete reusable artifacts or methods.
- Principles: concise transferable ideas from the notes.
- Tools mentioned: compact grid.
- Pull quotes: short, high-signal excerpts with timestamps.
- Footer: show page, full field notes, episode link.

## Copy Standards

Ground copy in the field notes. Do not invent broad framing that is not supported.

Avoid:

- Analyst/source narration: “the field notes frame this as...”, “the document suggests...”
- Synthetic contrast formulas: “not X but Y”, “the key move is...”, “the code is just the substrate...”
- Generic AI copy: “unlocking”, “seamless”, “magic”, “transformative”, “the real value is...”
- Unrequested interpretation of stats, packaging counts, or installability.

Prefer:

- Direct claims from the guest’s actual workflow.
- Specific nouns from the field notes: tools, artifacts, verification loops, timestamps, constraints.
- Plain descriptions of agent behavior and human verification.

Example for Alan Nichol:

```text
Alan uses Claude and Remotion to generate videos as code, then checks the rendered video rather than reading every line of generated code. The skill gives Claude rules for animation, layout, text treatment, timing, and visual inspection.
```

## Media Treatment

Use the 8-bit video as the main hero media when available. Keep the static avatar thumbnail for:

- mini identity badge
- video poster/fallback
- `og:image` / `twitter:image`

Do not use multiple large hero images. If the video has controls visible, label the caption clearly as `8-BIT VIDEO`.

## Verification

After creating or editing:

1. Check `git diff` and confirm only intended files changed.
2. If previewing locally, use a local server when possible; otherwise remind the user to refresh the `file://` page after edits.
3. Verify the guest page loads, the avatar path works, the video file exists, and there is no horizontal overflow.
4. Confirm `agent-skills.html` has no diff unless the user explicitly requested linking.

Useful checks:

```bash
rg -n "field notes frame|key move|substrate|not .* but|WATCH MOMENT" agent-skills/guests/<guest-slug>/index.html
find agent-skills/guests/<guest-slug> -maxdepth 1 -type f -print -exec ls -lh {} \;
git diff -- agent-skills.html
```
