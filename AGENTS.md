# Dotdrum — notes for AI coding agents

One HTML file: markup, inline CSS and inline scripts; GSAP with ScrollTrigger, Lenis smooth scroll, all from jsDelivr. No build step, no framework.

## Files
Work from the files: a page read through a link comes back as a summary, and a page rebuilt from it would differ.
```bash
curl -LO https://raw.githubusercontent.com/8bsite/print-studio-website-template/main/index.html
curl -LO https://raw.githubusercontent.com/8bsite/print-studio-website-template/main/AGENTS.md
curl -LO https://raw.githubusercontent.com/8bsite/print-studio-website-template/main/LICENSE
curl -LO https://raw.githubusercontent.com/8bsite/print-studio-website-template/main/CREDITS.md
```
In Windows PowerShell type `curl.exe`. Publish `index.html` with LICENSE and CREDITS.md; this file, README.md and `previews/` are not part of the site.

## Do not break
- Keep it one file unless the user asks otherwise; no framework or bundler for copy, colour or photo changes.
- Keep the ids and the `data-8b-section` / `data-8b-motion` attributes. Each section has its styles in `<style data-8b-part="section:<id>">` and its animation, if any, in `<script data-8b-part="scene:<id>">`.
- The scripts find these classes and attributes; when you change a section's markup, keep them or update the script:
  - `section-script:nav`: `.nav__toggle` `.nav__close`
  - `scene:press-pass`: `.press__art` `.lane` `.press__band` `.press__panel` `.drum` `.drum__disc`
  - `section-script:quote`: `.quote__form` `.quote__ticket` `data-out` `data-quote-mail` `.quote__lines` `data-setup` `data-passes` `data-rate` `data-factor` `data-label` `data-days-small` `data-days-mid` `data-days-large`
  - `section-script:contact`: `.contact__form` `data-sent`
  - `scene:inks-register`: `.inks__stage` `.inks__plate--blue` `.inks__plate--pink`
  - `scene:process-run`: `.process__window` `.process__track` `.sheet__plate--blue` `.sheet__plate--pink` `.sheet__under`
  - `scene:editions-pistons`: `.editions__rack` `.editions__lane--low` `.editions__lane--high` `.editions__trail`
  - `scene:workshops-index`: `.workshops__lane` `.workshops__carriage` `.session`
  - `scene:runs-lens`: `.run__client`
  - `scene:contact-landing`: `.contact__lanes` `.lane` `.drum` `.lane__print`
- Colours, fonts, radii and shadows are CSS variables in the `:root` block (`data-8b-part="tokens"`); change them there and keep body text at 4.5:1 contrast or more.
- Animations run only when the visitor allows motion (`prefers-reduced-motion: no-preference`); the page must stay readable without them.
- A display line with `style="--len:N"` is sized to fill its width: N is its number of characters, spaces included (`Two inks,` → 9, `ten thousand dots.` → 18).
- Keep the header comment and `<meta name="generator">`. Keep the "Design: 8B" footer link unless the site owner asks to remove it.

## Replace before publishing
- Everything marked `data-8b-placeholder` (og-image, favicon, email, form, phone, social); remove the attribute once the real value is in. With no real value: no preview image — delete the `og:image` tag and set `twitter:card` to `summary`; no phone, email or address — remove that link or row.
- Order links under each of the eight editions: link to mailto:shop@dotdrum.example with the edition in the subject now; point them to your shop or checkout URL for each edition (or keep the email with your real address).
- Book a place links under each of the five workshops: link to mailto:hello@dotdrum.example with the workshop in the subject now; point them to your booking or ticketing URL for each session (or keep the email with your real address).
- The Send this ticket button in the quote section: link to a mailto: address (the script adds the job lines) now; point them to your real email address, or your own quote or order form.
- **The forms send nothing.** `section-script:quote` and `section-script:contact` check the fields and show a thank-you message without sending. Pick one:
  - Formspree: set `action="https://formspree.io/f/<id>"` on the form; in the script, before the thank-you message, `await fetch(form.action, { method: 'POST', body: new FormData(form), headers: { Accept: 'application/json' } })` and show it only if the response is ok.
  - Netlify Forms (site on Netlify): add `name="contact" data-netlify="true"` and `<input type="hidden" name="form-name" value="contact">`; send with `fetch('/', { method: 'POST', headers: { 'Content-Type': 'application/x-www-form-urlencoded' }, body: new URLSearchParams(new FormData(form)) })`.
  - No form service: remove the `<form>` together with the script that handles it (it would fail without the form) and keep the email link.
- Invented demo facts (ask the user for the real ones; do not make up prices, dates or addresses):
  - Edition prices and formats: Soft Machines £9 (zine, 32 pp, A5), Pink Weather £14 (12 postcards), Quiet Pages £6 (booklet, 24 pp, A6), Overprint Almanac £22 (book, 96 pp, A5), Fold Here £5 (zine, 16 pp, A6), Night Buses £30 (3 × A3 posters), Back Issues £12 (magazine, 64 pp, A4), Spine Index £45 (box of ten zines); "when a run sells out, it stays sold out" (editions).
  - Workshop dates, durations, prices and places: Thu 15 Oct (3 hours, £65, 2 places left), Sat 24 Oct (6 hours, £120, 4 left), Wed 4 Nov (3 hours, £70, 6 left), Sat 14 Nov (4 hours, £85, 3 left), Thu 26 Nov (3 hours, £70, 5 left), with 2026 dates in the datetime attributes; "evening sessions for six people at a time" (workshops; note the Saturday sessions are daytime).
  - Print rates and the claim "These are our real rates; the quote we send back will match" (quote): per-copy rates A6 £0.09, A5 £0.15, A4 £0.24, A3 £0.40, factors for two inks (×1.7), paper (×1.15, ×1.6) and double-sided (×1.85), setup £18 per ink, the default ticket £118 / £0.59 a copy, turnaround 2, 3 or 5 working days by run size; prices in GBP (the form's data attributes and section-script:quote).
  - Run sizes: 30 to 3,000 copies (opening facts, the quote copies field, FAQ "smallest and largest runs") and "runs from 30 copies, workshops every month" (meta description).
  - Turnaround and timings: ready "the next working day" (process, Dry, trim, fold), "two working days after you approve the proof", large runs up to five (FAQ), masters "30 seconds each", "ten minutes to dry" (process) and "dries for a minute" (opening note), "two hundred A3 posters take about forty minutes" (process).
  - Machines and specs: up to 130 sheets a minute, A3 at most, a guillotine cutting a 500-sheet stack within half a millimetre, saddle-stitching up to 64 pages, a drying rack with eighty shelves (studio, process); papers Recycled 90 g, Uncoated 120 g, Card 300 g (quote) and uncoated 70 to 300 g (FAQ).
  - Materials and service: soy-based inks, recycled stock as standard, packed flat in recycled card, every overlap proofed before the run, the ink names Medium Blue and Fluorescent Pink (opening, inks, process, FAQ).
  - People and history: Ines and Tom who run the studio from a former bicycle workshop, "one secondhand duplicator", "six years on" (opening, studio).
  - Clients and jobs in the press log: Low Tide Records, Hollow Oak Poetry Night, Gallery Seven, Northern Soup, The Cycling Kitchen, Small Hours Collective, with copies and inks (runs); "artists, bands, galleries" (opening); "the largest we have printed is 3,000 flyers" and the offset printer referral (FAQ).
  - Place, hours and delivery: the address Unit 4, Inkwell Yard Works, Sheffield S0 0AA and the street "off Inkwell Yard" (studio, contact, footer), Sheffield (opening facts, footer), "collect in Sheffield or post anywhere in the UK" (opening), open Tuesday to Friday 10–6, Saturday 11–4 (contact).
  - Service: "We answer within a working day" (the contact form's data-sent message).
  - Copyright "© 2026 Dotdrum Press · Printed in two inks in Sheffield" (footer).
- `<title>`, the meta description and the og tags; photos (Unsplash, see CREDITS.md) when the user has their own.

## Notes
- The drums, lanes, halftone trails, the colour chart and the sample sheet are HTML and CSS (gradients and masks), not images: recolour them through the tokens.
- The quote calculator's rates are data attributes on the form; change them there, and the currency in section-script:quote.
- Lighthouse lists gsap and ScrollTrigger as render-blocking (about 1 s on mobile); they sit at the end of body and the motion scripts after them need them loaded.
- Without JavaScript the phone menu button cannot open the overlay and the job ticket shows its default job only; the links stay reachable in the hero and the footer.
- On screens shorter than 640 px the first screen grows past the viewport and the print starts later; the process stage needs about 600 px of height on phones, below that the sheet shrinks.

## Common edits
- Rebrand: search the old name (`Dotdrum`) in any letter case and replace every match; change the favicon letter and colours in `<link rel="icon">`.
- Photos: replace `src` and delete `srcset` and `sizes` (they list Unsplash sizes), keep `width`/`height` to the new photo's ratio, update `alt` and CREDITS.md.
- Remove a section: delete its markup, its `section:<id>` and `scene-style:<id>` styles, its `scene:<id>` and `section-script:<id>` scripts, and links to `#<id>`.
- Native scroll instead of smooth scroll: delete the Lenis block in `page-init` and the Lenis `<script>` tag.

## Check and publish
- Open the page at 375 and 1440 px, with reduced motion on, with the console open.
- Search `data-8b-placeholder` and the old name: nothing should be left but class names.
