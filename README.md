# Cyber Portfolio

A dark, terminal-themed portfolio for a cybersecurity researcher, penetration
tester and ethical hacker. Built with Next.js App Router, TypeScript, Tailwind
CSS v4, Framer Motion and Lucide.

---

## Quick start

```bash
npm install
npm run dev          # http://localhost:3000
```

```bash
npm run build        # production build
npm run start        # serve the build
npm run lint         # eslint
npm run typecheck    # tsc --noEmit
```

Requires Node 20.9 or newer.

---

## Make it yours

**Nearly everything you need to change lives in one file: `lib/site.ts`.**
Names, copy, projects, writeups, certifications, roles, tooling and social
links are all data. No component needs editing for a content change. Every
placeholder is marked with a `// TODO` comment.

Checklist before deploying:

| What | Where |
|---|---|
| Name, email, location, availability | `lib/site.ts` → `site` |
| Canonical URL (used by OG tags, sitemap, robots) | `lib/site.ts` → `site.url` |
| Social links | `lib/site.ts` → `socials` |
| Projects, disclosures, education, experience, tooling | `lib/site.ts` (each exported array) |
| Portrait | replace `public/profile.jpg` |
| Social share image | replace `public/opengraph-image.png` (1200×630) |
| Favicon | replace `app/icon.png` and `app/apple-icon.png` |
| CV | drop a PDF at `public/resume.pdf` |

The colour system, typography and spacing are tokens in `app/globals.css`
under `@theme`. Change `--color-matrix` and the entire site re-skins.

---

## `preview.html`

A single-file, zero-dependency static build of the same design, bundled for
convenience. Open it directly in a browser to see the visual system — matrix
rain, preloader, cursor, filtering and the interactive shell all work — with
no install step. It is a preview only; the Next.js app in this repo is the
real thing, and content changes there do not propagate to it.

---

## Structure

```
app/
  layout.tsx          Fonts, metadata, JSON-LD, chrome, skip link
  page.tsx            Section composition
  globals.css         Design tokens, utilities, keyframes, reduced-motion
  not-found.tsx       404
  sitemap.ts robots.ts

components/
  layout/             Preloader, CustomCursor, Navbar, Footer, AmbientFx
  background/         MatrixRain (canvas), CyberGrid
  ui/                 Reveal, SectionShell, TerminalWindow, TypeLines,
                      CyberButton, GlitchText, Meter, SeverityBadge, Tag
  interactive/        InteractiveTerminal
  sections/           Hero, About, Skills, SkillRadar, Research, Projects,
                      Education, Experience, Arsenal, Contact

lib/
  site.ts             All content
  motion.ts           Shared easing and transition tokens
  hooks.ts            Media query, reduced motion, scroll spy, scrolled
  utils.ts            cn(), clamp()
```

---

## Design decisions worth knowing

**Dark only, by intent.** There is no light theme. The palette is a near-black
base, one high-chroma signal colour, and an emerald support tone.

**Red means severity, never decoration.** `SeverityBadge` is the only
component that uses red, and it always writes the level out as text, so
severity survives colour-blindness and greyscale printing.

**Motion is opt-out everywhere.** `prefers-reduced-motion` is honoured in
three layers: a global CSS switch, `useReducedMotion()` in every animated
component, and early returns in the JS loops — so the matrix canvas never
starts rather than animating invisibly. Nothing is conveyed by motion alone.

**The custom cursor is conditional.** It only activates on `(pointer: fine)`,
never hides the native cursor on touch, stops lagging under reduced motion,
and is restored on unmount.

**The preloader is skippable and runs once per session.** It is under two
seconds, dismissible with Escape or a button, stores completion in
`sessionStorage`, and the page content sits in the DOM beneath it the whole
time — so crawlers and no-JS visitors are unaffected.

**Canvas is well-behaved.** The matrix rain caps its frame rate, clamps device
pixel ratio at 2, and pauses entirely when the tab is hidden or the canvas
scrolls out of view.

---

## Accessibility

- Skip link is the first tab stop.
- All body text meets WCAG AA (4.5:1). Measured against the *lightest* surface
  each token sits on, not just the page background: `bone` 14.4:1, `ash`
  6.9:1, `slate` 4.65:1. `matrix-deep` (3.4:1) is a gradient tone only and is
  never used for text — green type uses `matrix-dim` (7.2:1).
- Focus is never removed, only restyled; `:focus-visible` keeps the ring for
  keyboard users without showing it on mouse click.
- Interactive targets are at least 44×44px.
- The mobile menu traps focus, closes on Escape, and restores focus to its
  trigger.
- Meters and tool bars are real `progressbar` roles with `aria-valuenow`.
- The radar chart is decorative reinforcement; the same numbers are available
  as labelled meters beside it.
- Project filtering announces its result count via `aria-live`, and the
  zero-result state offers a way out.
- Zoom is not disabled.

Worth testing yourself before launch: a full keyboard pass, VoiceOver or NVDA
on the interactive terminal, and Lighthouse on the deployed build.

---

## Deploying

Works on any Node host. On Vercel, import the repo and deploy — no
configuration needed. Set `site.url` to the real origin first, or the Open
Graph tags, canonical URL and sitemap will point at `example.com`.

Security headers (`X-Frame-Options`, `X-Content-Type-Options`,
`Referrer-Policy`, `Permissions-Policy`) are set in `next.config.ts`.

---

## Still to fill in

Search the project for `// TODO` — every one is in `lib/site.ts`:

1. **Employment dates** in `experience[]` are estimates. Put your real ones in.
2. **`arsenal[]`** lists your two tools plus a standard toolkit inferred from
   your skills. Delete anything you don't actually use — a tool on this page is
   a tool you can be asked about.
3. **Repo links** in `projects[]` point at your GitHub profile, not the
   individual repositories.
4. **`site.url`** must be your real domain before deploying, or the Open Graph
   tags, canonical URL and sitemap all point at `example.com`.
5. **`about.stats`** — verify each number against your own records.
6. **Decathlon and Stripe** entries in `writeups[]` have no technical detail
   yet. Add the vulnerability type and severity when you're free to.

Replace `public/profile.jpg` with your own portrait, and drop a CV at
`public/resume.pdf` for the Download CV button.
