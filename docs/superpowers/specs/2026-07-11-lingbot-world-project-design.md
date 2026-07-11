# LingBot-World 2.0 publication card design

## Goal

Add LingBot-World 2.0 to the top of the homepage's **Selected Publications** list while preserving the site's existing visual and interaction patterns.

## Source of truth

Use the public `Robbyant/lingbot-world-v2` repository and arXiv record `2607.07534` as the authoritative sources:

- Paper title: **Infinite Worlds with Versatile Interactions**
- Project name: **LingBot-World 2.0** / **LingBot-World-Infinity**
- Venue label: **arXiv 2026**
- Project page: `https://technology.robbyant.com/lingbot-world-v2`
- Code: `https://github.com/Robbyant/lingbot-world-v2`
- Paper: `https://arxiv.org/pdf/2607.07534`
- Thumbnail source: `assets/teaser.png` from the project repository
- Authors: use the arXiv ordering and render Yichong Lu as the highlighted homepage owner

## Approaches considered

1. **Homepage publication card only — approved.** Add one card matching the existing cards, with no additional navigation or page. This is the smallest change and keeps the list chronological.
2. **Homepage card plus a standalone project page.** This would provide more space for demos, but duplicates the external project page and adds unnecessary maintenance.
3. **Homepage card plus a News item.** This would increase prominence, but the request is specifically to add a project and does not ask for a news announcement.

## Approved design

- Copy the upstream teaser unchanged to `assets/teaser/lingbot-world-v2.png` so the homepage does not depend on GitHub's raw-file availability.
- Insert a new `<li>` before the current first publication in `index.html`.
- Follow the existing three-column/nine-column card structure and button styles.
- Use a unique card id, `Gao2026Arxiv`.
- Render the exact paper title, complete author list, and `arXiv 2026` periodical label.
- Highlight `Lu, Yichong` with `<em>`; render the other authors as plain author links without inventing personal-page URLs.
- Provide the existing `Abs`, `PDF`, `Code`, and `Website` buttons in that order.
- Populate the hidden abstract block from the arXiv abstract.
- Use descriptive thumbnail alt text matching the paper title.
- Do not add an independent page under `projects/`, a News row, new CSS, or new JavaScript.

## Verification

- Confirm the copied thumbnail is a valid non-empty PNG.
- Parse `index.html` and verify exactly one `Gao2026Arxiv` card exists before the previous first publication.
- Verify the card contains the expected title, all four links, `arXiv 2026`, the highlighted `Lu, Yichong`, and the local teaser path.
- Confirm the repository diff is limited to the design document, `index.html`, and the teaser image.

## Acceptance criteria

The homepage shows LingBot-World 2.0 first in Selected Publications, visually consistent with existing entries; its thumbnail loads locally; and its PDF, code, project-page, and abstract controls are all present with the correct content.
