---
name: create-slide-deck
description: Create a new slide deck in this repository from the template, export it to PDF, then add it to the root index page and README. Use when asked to "create a slide deck", "new slides for a talk", "start a deck", "make slides about X" or "/create-slide-deck".
---

# Create a slide deck

Create a new deck from `template/index.html` in its own folder at the repository root. Then register it on the root `index.html` and in the root `README.md`. A deck is 1 self-contained HTML file: no build step, assets referenced by relative path.

## Rules every deck follows

- Keep dates, years and event names with years off every slide, the `<title>` and the root index page. Where and when a talk was given goes only in the deck's `README.md`.
- Put 1 idea on each slide, and keep on-screen text short. The speaker says the detail.
- Write numbers as digits, including 1 to 9.
- Use hyphens. Never use em dashes or en dashes.
- Reference every image and asset by a relative path inside the deck folder, such as `src="qr.png"`, so the deck works locally and on GitHub Pages.
- Use only the components and CSS variables the template defines. `template/README.md` lists them.

## Steps

1. Collect what the deck needs. Ask the user for anything missing, in 1 question round:
   - The talk's title and a 1-line subtitle.
   - The event's name for the footer label, without a year, such as `Melbourne Drupal Meetup`.
   - Where and when the talk is or was given, for the deck's `README.md`.
   - The talk's content: an outline, an abstract or source material to build slides from.

2. Pick the folder name: the talk's title in lowercase with hyphens, short and without a date, such as `behat-steps-4`. Confirm that no folder with that name exists.

3. Create the deck from the template:

   ```bash
   mkdir <folder>
   cp template/index.html <folder>/index.html
   ```

4. Set the chrome in `<folder>/index.html`:
   - The `<title>` is the talk's title.
   - The title slide's `h1`, `.lede` and `.byline`.
   - Every `.venue` footer label reads the event's name in place of `Event name`.
   - When the talk is about a product with a logo, replace the `.title-orbit` in `.title-right` with the logo as inline SVG. `behat-steps-4/index.html` shows the markup and the `.title-right.logo` CSS that hides the orbit rings.

5. Delete the template's example slides and write the deck's own slides from the content, using the template's components. Keep the title slide first and the thanks slide last.

6. Check that every slide fits. Use an isolated `agent-browser` session so the shared browser stays untouched. Open the deck at a 1280 × 720 viewport:

   ```bash
   agent-browser --session deckcheck open file://$PWD/<folder>/index.html
   agent-browser --session deckcheck set viewport 1280 720
   ```

   Then run this check, which lists every slide whose content runs past the slide's bottom or right edge:

   ```bash
   agent-browser --session deckcheck eval "[...document.querySelectorAll('.slide')].map((s,i)=>{const r=s.getBoundingClientRect();let over=0;s.querySelectorAll('*').forEach(e=>{if(e.closest('.slide-footer'))return;const b=e.getBoundingClientRect();if(b.height&&b.bottom>r.bottom+1)over=Math.max(over,Math.round(b.bottom-r.bottom));if(b.width&&b.right>r.right+1)over=Math.max(over,Math.round(b.right-r.right))});return over?(i+1)+':'+over:null}).filter(Boolean).join(' ')||'no overflow'"
   ```

   Shorten the text on any slide it lists. Then screenshot each slide by scrolling to it with `document.querySelectorAll('.slide')[N].scrollIntoView()`, and fix any slide whose content crowds the footer.

7. Save the preview. Set a 1600 × 900 viewport, reload the deck at the title slide and hide the slide counter and the scrollbar:

   ```bash
   agent-browser --session deckcheck set viewport 1600 900
   agent-browser --session deckcheck eval "document.head.insertAdjacentHTML('beforeend','<style>.slide-number{display:none!important} ::-webkit-scrollbar{display:none} body{scrollbar-width:none}</style>'); 'ok'"
   agent-browser --session deckcheck screenshot <folder>/preview.png
   ```

   When the title slide shows a year, such as an event badge, hide that element with the same `eval` before the screenshot. Read the PNG and confirm it shows the title slide with no year on it. Then close the session with `agent-browser --session deckcheck close`.

8. Export the PDF with the local `convert` skill's HTML-to-PDF script. It renders through Gotenberg in Docker, 1 slide per A4 page:

   ```bash
   php ~/.claude/skills/convert/scripts/convert-html-to-pdf.php <folder>/index.html <folder>/<folder>.pdf --unbranded
   ```

   Confirm with `pdfinfo <folder>/<folder>.pdf` that the page count equals the number of `<section class="slide` elements in the deck. When the script doesn't exist or fails, such as when Docker isn't running, skip the PDF, leave its line out of the README and tell the user the PDF wasn't made.

9. Write `<folder>/README.md` in this shape:

   ```markdown
   # <Talk title>

   A talk by Alex Skrypnyk on <1-2 sentence summary>.

   ## Where it was presented

   - **Event:** <Event name>
   - **Date:** <Month Year>

   ## Slides

   - [Open the deck online](https://alexskrypnyk.github.io/slides/<folder>/).
   - To present from a local copy, open [index.html](index.html) in a browser and press <kbd>F</kbd> for fullscreen. The arrow keys move between slides.
   - [<folder>.pdf](<folder>.pdf) is the PDF export.
   ```

10. Add the deck to the root `index.html` as the first card in `.deck-grid`, so the newest deck comes first. The card carries the title and the preview only:

   ```html
   <a class="deck-card" href="<folder>/index.html">
     <img src="<folder>/preview.png" alt="Title slide of <Talk title>" loading="lazy">
     <h2><Talk title></h2>
   </a>
   ```

11. Add a row for the deck at the top of the **Decks** table in the root `README.md`.

12. Open the root `index.html` in the isolated session and confirm that the new card shows its preview and opens the deck.

13. Commit the deck folder, `index.html` and `README.md` with explicit paths. Push only when the user asks: a push to `main` publishes the deck on GitHub Pages.

## Present and export

<kbd>F</kbd> toggles fullscreen, and the arrow keys, <kbd>Space</kbd>, <kbd>PageUp</kbd> and <kbd>PageDown</kbd> move between slides. Step 8 exports the PDF. To export one without the script, print the deck from the browser and save it as a PDF; the print styles put each slide on its own page.
