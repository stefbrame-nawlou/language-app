# Parole

A personal language-practice website, built with React, TypeScript, and Vite. It runs locally and builds into a static website suitable for GitHub Pages. No backend, account, or Python installation is required.

## Run locally

Requires Node.js 20.19+ or 22.12+ and npm. Node 24 is recommended.

```sh
npm install
npm run dev
```

Open the address printed by Vite. The current VS Code task starts it at **http://127.0.0.1:5173/**. Use that exact address: on this machine, `localhost:5173` may reach a different application's IPv6 server. If the IPv4 port is occupied, Vite selects another free port and prints the new address.

The VS Code task **Start Parole locally** also launches the application.

```sh
npm test
npm run build
npm run preview
```

`dist/` contains the production website. Do not open the source HTML directly; use Vite or a static web server.

## Learning flow

1. Choose English or French as the reference language, then a target language and quiz direction.
2. Choose a theme, vocabulary/sentences/dialogues/verbs, and one or more chapters.
3. Change global chapter sizes in Practice settings. Defaults: 10 words, 5 sentences, 1 dialogue, 5 verb drills.
4. Recall an answer, optionally write it, and choose Check answer.
5. Compare the revealed answer and mark I got it or Not yet. Typed answers are not graded automatically.
6. Review the two result lists, expand an item for its answer and context, or retry missed items.

Supported targets: European Portuguese, German, Moroccan Darija, and Japanese. Arabic text is right-to-left with approximate Latin transliteration. Japanese uses modern everyday spelling, hiragana readings, and romaji. Reverse quizzes hide reading hints initially; Show reading is available before checking.

Common verbs in the starter notebook are clickable in supported sentences and examples. Go back closes the reference without changing the underlying quiz or review state.

### Keyboard shortcuts (desktop)

| Action | Key | While editing your answer |
| --- | --- | --- |
| Check answer | Enter | Ctrl+Enter |
| I got it | 1 | Ctrl+1 |
| Not yet | 2 | Ctrl+2 |

Grading keys work only after revealing the answer. Repeated keydown events, active input composition, and open dialogs do not trigger quiz actions. Plain Enter remains available for new lines while typing a dialogue. Buttons expose shortcuts through hover tooltips and accessibility metadata.

### Alphabet themes

Select Japanese to access **Hiragana**, or Moroccan Darija to access **Arabic alphabet**, in the theme list or Change theme picker. Each includes an expandable character chart and chapter quizzes in both directions. Alphabet progress is tracked with the normal review system and stable character IDs.

- Hiragana: 46 basic characters, 25 voiced/semi-voiced forms, 33 small-kana combinations, and 3 special signs. Notes cover particle readings and small tsu. This is a kana reading course, not a handwriting/stroke-order trainer.
- Arabic: 28 letters and 8 vowel marks, with isolated/initial/medial/final shapes and the six letters that do not connect to the next letter. Letter names and approximate sounds are distinguished; notes explain relevant Darija differences. Tanwin is included for script literacy but is not an everyday Darija ending. Additional Moroccan letters, ligatures, handwriting styles, and stroke-order practice are not covered yet.

## Starter content

All ten requested themes are present: rugby, school, family, consultancy, vacation, books, learning languages, secondary-school mathematics teacher training, programming, and emotions.

The thematic collection contains **162 practice items**: 56 vocabulary items, 57 sentences, 10 dialogues, and 39 introductory tense drills. Japanese additionally has **107 hiragana items**; Arabic additionally has **36 script items**. Only matching alphabet items appear in each language's totals and review queue. Rugby has 20 words and 12 sentences, making it the most complete theme for testing multiple chapters. Other themes intentionally begin with four words, five sentences, and one dialogue.

The conjugation notebook initially covers **play** and **learn** in all six languages, not every common verb. Portuguese and German include present, past, and future tables. Darija uses habitual, past, and ghadi-future forms. Japanese has polite non-past, polite past, and introductory plain non-past forms; future practice uses a time expression with non-past forms.

Material is original and generated as a starter curriculum; it has **not been native-speaker reviewed**. Darija spelling is not standardized, transliteration is approximate, and technical loanwords vary. Gender, politeness, and regional variants are not exhaustively covered. In Japanese, sibling terms distinguish older/younger and one's own/another person's family. The first-floor school example uses British floor numbering, hence Japanese second floor. Treat the material as a prototype, particularly for Darija, until reviewed.

The pedagogy draws on active recall, contextual examples, sound/script awareness, and spaced repetition associated with Gabriel Wyner's *Fluent Forever*. It does not reproduce the book or claim to implement its complete method. There are no pronunciation recordings or sound-discrimination exercises yet.

## Progress and review

- Answers are saved after each rating, even if a session is interrupted. Only completed sessions appear in session history.
- Progress is separate for each item, language pair, and direction.
- The dashboard shows coverage, accuracy, practice days, session history, and repeatedly missed items for the selected pair/direction.
- Due review uses the established FSRS algorithm from `ts-fsrs`. Correct maps to Good, incorrect to Again. It reviews studied items that are due, not every unstudied item.
- FSRS dates update during use. Chapter practice can be done even when an item is not due.
- Changing chapter sizes does not change stable item IDs or erase history.

Progress and preferences use browser `localStorage`. They are not uploaded to GitHub and do not sync between devices. Private browsing, clearing site data, changing browser, or changing the site origin can lose access to the saved history. In particular, localhost and GitHub Pages have separate storage.

Use Export progress and Import progress to transfer a JSON backup. Import validates the format and asks before replacing existing history. Export a backup before moving to GitHub Pages.

## Project structure

| File | Responsibility |
| --- | --- |
| `src/content.ts` | Themes, multilingual vocabulary, sentences, dialogues, and contextual examples |
| `src/verbs.ts` | Conjugation notebook and introductory tense drills |
| `src/alphabets.ts` | Target-specific script themes, reference forms, and stable-ID character quizzes |
| `src/shortcuts.ts` | Quiz keyboard action routing and typing/composition protections |
| `src/types.ts` | Language, translation, item, verb, and settings contracts |
| `src/engine.ts` | Grouping, shuffling, direction, scoring, and stable progress keys |
| `src/progress.ts` | FSRS scheduling, storage schema, validation, and answer records |
| `src/text.ts` | Whole-word verb link matching, including Arabic and Japanese |
| `src/App.tsx` | Practice, quiz, review, progress, settings, and reference views |
| `src/styles.css` | Responsive layout and visual styling |
| `public/art/` | Original bundled PNG theme illustrations |

Keep IDs stable when editing existing content. Insert new entries deliberately: IDs are currently assigned from the entry position within each theme. Append new entries; reordering or inserting into the middle would associate existing progress with different material. A future content editor should use explicitly assigned IDs rather than positional ones.

The artwork can be regenerated on Windows with `& '.\scripts\generate-art.ps1'`. It does not require an image service. Google Fonts supplies the preferred typefaces when reachable; local fallback fonts remain usable without it.

## Future audio

Every `Translation` can optionally contain:

```ts
audio: {
  src: string;
  provider?: string;
  verified: boolean;
}
```

Translations are shared by vocabulary, examples, sentences, dialogues, and verb forms, so audio can be attached without changing scoring or storage IDs. An eventual player should appear only after answer reveal, accept normal/slow playback, and distinguish generated versus reviewed material.

Pre-generated MP3 files can be served from GitHub Pages. On-demand paid generation requires a separate backend or local generation script; never expose API credentials in a static browser build. Darija voices must be tested on actual Moroccan phrases, not assumed accurate because their locale says Morocco.

## GitHub Pages later

The project package is named `language-app`; the in-app product name remains Parole. The repository is https://github.com/stefbrame-nawlou/language-app and the GitHub Pages address is https://stefbrame-nawlou.github.io/language-app/ once deployment completes. The workflow at `.github/workflows/pages.yml` has **only a manual trigger**, so uploading or pushing code does not deploy automatically.

1. Create a repository in your own GitHub account and push this project, including the lockfile and bundled artwork. Do not include `node_modules/` or secrets.
2. In the repository's Settings > Pages, select GitHub Actions as the source.
3. In Actions, select Deploy Parole to GitHub Pages and choose Run workflow on the main branch.
4. The workflow runs tests, builds the static site, and deploys `dist/`. GitHub displays the final website URL.

Vite's `base: './'` keeps bundled scripts and images relative, so repository subpaths are supported. UI navigation uses component state rather than server-side routes, avoiding GitHub Pages refresh/404 problems. The Pages website is publicly readable unless your GitHub plan and repository configuration support restricted Pages access; progress itself remains in each visitor's browser.

## Verification

Twenty-five automated tests cover grouping, shuffle integrity, direction-specific keys, empty scores, scheduling, backup validation, script completeness, content IDs, verb links, keyboard shortcuts, hiragana coverage, Arabic joining forms, and target-language filtering.

Browser checks performed during implementation: complete 20-item quiz with mixed ratings (75%, 15 remembered, 5 missed), persistence after reload, conjugation return navigation, Japanese reading visibility, French reverse practice, Darija RTL/phonetics, adjustable chapter sizes, future-tense filtering, and dialogue line breaks. Practice and progress were checked at 320, 390, 768, 1024, and 1440 pixels without horizontal overflow. These browser checks are manual automation, not part of `npm test`.
