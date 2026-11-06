# Slides

Slide decks from talks by Alex Skrypnyk. Every deck is a single HTML file you can open in a browser, and the [index page](https://alexskrypnyk.github.io/slides/) links to each of them.

## Decks

| Deck | Folder |
|------|--------|
| Behat Steps 4: Drupal 12-ready testing in 1 package | [behat-steps-4](behat-steps-4/) |
| Making AI work for you: Minimising AI slop on Drupal projects | [making-ai-work-for-you](making-ai-work-for-you/) |
| From Prompt to Pull Request | [from-prompt-to-pull-request](from-prompt-to-pull-request/) |
| Five Years Late, Still On Time: mentalhealthonline.org.au Drupal 8 to 11 journey | [five-years-late-still-on-time](five-years-late-still-on-time/) |
| Setup projects and onboard developers in minutes with Vortex | [setup-projects-with-vortex](setup-projects-with-vortex/) |
| Local-CI-Production workflow with DrevOps | [local-ci-production-workflow-with-drevops](local-ci-production-workflow-with-drevops/) |

Each deck's `README.md` says where and when the talk was given.

## How the repository is laid out

- Each deck lives in its own folder at the repository root, named after the talk in lowercase with hyphens.
- A deck folder holds the deck as `index.html`, its images, a PDF export named after the folder, a `preview.png` of the title slide and a `README.md`.
- A deck built in Google Slides keeps 1 exported image per slide in `slides/`. Its `index.html` shows those images with the same keyboard controls as the HTML decks.
- The root `index.html` shows a card with the name and preview of every deck.
- [template/](template/) holds the template for new decks and describes its components.

## Add a deck

With Claude Code, run the `create-slide-deck` skill from the repository root. It creates the deck folder from the template, writes the deck's `README.md`, takes the preview, exports the PDF and adds the deck to the index page and to the [Decks](#decks) table.

To add a deck by hand:

1. Follow [the template's README](template/README.md) to create the deck folder and its `index.html`.
2. Write the deck's `README.md` with the talk's name, a summary and where and when it was given.
3. Save a 1600 × 900 screenshot of the title slide as `preview.png` in the deck folder.
4. Print the deck from the browser to a PDF named after the folder, such as `behat-steps-4/behat-steps-4.pdf`.
5. Add a card for the deck as the first `<a class="deck-card">` in the root `index.html`, and a row at the top of the [Decks](#decks) table.

Keep dates and venues off the slides and the index page. They belong in the deck's `README.md`.

## GitHub Pages

GitHub Pages publishes the `main` branch from the repository root at [alexskrypnyk.github.io/slides](https://alexskrypnyk.github.io/slides/). A deck's address is the site address plus its folder name, such as `https://alexskrypnyk.github.io/slides/behat-steps-4/`.
