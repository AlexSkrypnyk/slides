# Slide deck template

[index.html](index.html) is the starting point for every new deck in this repository. It's a single self-contained HTML file with the DrevOps design system and 1 example slide of every component. It needs no build step: open it in a browser to see every component.

## Start a deck from it

1. Copy `template/index.html` into a new folder at the repository root, named after the talk in lowercase with hyphens, such as `behat-steps-4/index.html`.
2. Replace the text on the title slide, the `<title>` and the `.byline`.
3. Replace `Event name` in each `.venue` footer label with the event's name.
4. Delete the example slides and build your own from the components listed under [Components](#components). Each `<section class="slide">` is 1 slide.
5. Put images and other assets in the deck's folder and reference them by relative path, such as `src="qr.png"`.

Keep dates and years off the slides. The deck's `README.md` records where and when the talk was given.

The `create-slide-deck` skill in `.claude/skills/` runs these steps with Claude Code and also adds the deck to the root index page.

## Design language

The deck is driven by about 15 CSS variables at the top of `index.html`. Every component references them, so editing those values reskins the whole deck.

- **Navy** (`#152235` and `#030D1E`) is the background of dark slides and section dividers.
- **Cool white** (`#F4F8FD`) is the background of light slides.
- **Teal** (`#1E7582` on light, `#96E7F4` on dark) is the accent for the point a slide makes.
- **Coral** (`#CD5B43` and `#FF9C86`) marks the problem, the warning or the "before". The coral utilities are `.kicker.slop`, `.slop-text`, `.punchline.slop`, `.tool-card.slop-card`, `.section-divider.slop-divider` and `.col-label.slop`.

The fonts are Plus Jakarta Sans for headings, Outfit for body text and Fira Code for code, loaded from Google Fonts. The DrevOps logo is inlined once as the SVG symbol `#drevops-logo` and takes its color from `currentColor`. Reference it with `<span class="deck-logo"><svg viewBox="0 0 5267 1247"><use href="#drevops-logo"/></svg></span>`.

## Components

Each component appears once in `index.html`:

- **Title** (`.slide.title`): a dark panel beside a brand-gradient panel.
- **Basic**: `.kicker`, `h2`, `.lede` and `p`. Add `.dark` for a dark slide.
- **Section divider** (`.slide.section-divider`): an oversized heading. Add `.slop-divider` for the coral version.
- **Tool grid** (`.tool-grid` with `.tool-card`): 3 cards in a row. Add `.slop-card` for a coral card.
- **2 columns** (`.two-col`): a before and after, labeled with `.col-label.slop` and `.col-label.good`.
- **Code split** (`.slide.code-split`): a claim beside a code block. Add `.code-split-reverse` to swap the sides.
- **Workflow** (`.workflow-list-wide`): numbered stages in a row.
- **Timeline** (`.timeline`): 6 points on a horizontal track.
- **Comparison bars** (`.comparison`): labeled percentage bars.
- **Image split** (`.slide.meme-split`): text beside an `img.meme-img`. Use `.img-placeholder` until the image exists.
- **Big statement** (`h2.big` with `.punchline`): the line you want quoted.
- **Stat grid** (`.stat-grid`) and **loop diagram** (`.loop-diagram`).
- **Tier stack** (`.tier-stack` with `.tier.variable` and `.tier.foundation`): a dashed layer resting on a solid one.
- **Layer list** (`.layer-list` with `.layer-row`): stacked rows. Add `.live` to draw a row dashed.
- **Thanks** (`.slide.thanks`).

The template also styles `pre.terminal` for command output, `pre.diff` for diffs and `.pr-mockup` for a GitHub-style pull request card, without an example slide for them. The inline helpers are `.accent` for teal text, `.slop-text` for coral text, `code` and `.quiet`.

## Present and export

Open the deck's `index.html` in a browser. These keys control it:

- <kbd>→</kbd>, <kbd>↓</kbd>, <kbd>Space</kbd> or <kbd>PageDown</kbd> moves to the next slide.
- <kbd>←</kbd>, <kbd>↑</kbd> or <kbd>PageUp</kbd> moves to the previous slide.
- <kbd>Home</kbd> and <kbd>End</kbd> jump to the first and last slides.
- <kbd>F</kbd> toggles fullscreen.

To export a PDF, print the page from the browser and save it as a PDF. The print styles put each slide on its own page.
