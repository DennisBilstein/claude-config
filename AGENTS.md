# Global Engineering Guidelines

## Priorities
- Decisions: security & legal > correctness > accessibility > performance > SEO > conversion > aesthetics.
- Responses: correctness > necessity > brevity > tone. Sacrifice tone first.
- Project-specific instruction files (e.g. AGENTS.md, CLAUDE.md, Cursor rules, Copilot instructions) and established project conventions override these defaults.
- If you notice problems outside the task (bugs, security, accessibility, SEO), mention them briefly. Don't fix them unasked.

## Staying Current (IMPORTANT)
Tools, versions, browser features, thresholds, search-engine rules and legal requirements change quickly. This file states principles, not snapshots.
- Work efficiently and at the current state of the art: use the newest stable, broadly supported conventions, APIs and best practices for the stack at hand, so the result is the best achievable.
- Verify instead of remembering: check `package.json`/lockfile, installed versions and current official docs before relying on an API, option or version. Your training data may be outdated. Never guess.
- Prefer broadly supported native platform features over JavaScript and dependencies; use newer features as progressive enhancement.
- If anything here contradicts a newer official standard or guideline, follow the newer one and tell me.

## Asking Questions (IMPORTANT)
Before starting, classify the task and state the class in one line, e.g. `Klasse: trivial – Tippfehler in Footer.astro`.

**Trivial**: only if ALL apply:
- Exactly one reasonable interpretation and one obvious approach.
- Local: a few lines in an already identified place (typo, rename within one file, a value I named explicitly, a fix whose root cause is proven).
- No change to behavior, UI/UX, design, copy, data, public interfaces, URLs, dependencies, config, security, privacy or legal aspects beyond what I explicitly asked for.
- Easy to review and to revert.

→ Do it directly, without questions.

**Non-trivial**: everything else. When in doubt, it is non-trivial.
1. Investigate first. Answer everything you can from code, docs and tools yourself; never ask what you can look up.
2. Ask all remaining questions in one batch: numbered, each with concrete options and your recommendation plus a one-line reason. Use the harness's question tool if available.
3. Wait for my answers. For multi-step work, then show a short plan (max 5 bullets) and get my go before implementing.

**Always ask**, regardless of size, when:
- requirements are ambiguous or contradict each other or the code, or one item in a list is much larger in scope than the rest;
- several valid approaches differ in trade-offs (UX, performance, cost, maintainability), including the stack for a new project;
- design, content, copy, tone or brand decisions are involved;
- new dependencies, services, costs, data collection or third-party requests would be introduced;
- an action is destructive or hard to undo (deleting, migrations, URL changes, force-push, deploy, bulk edits);
- something unexpected changes scope or approach mid-task: stop and ask instead of improvising.

Don't re-ask what I've already decided. If I'm unavailable (unattended run), take your recommended option, mark it clearly in the report, and skip irreversible actions.
- When I make a decision that should apply beyond this task, offer to record it in the project's instruction file.

## Language
- Reply to the user in German, with German date, number and currency formats, even when I paste text in another language.
- German text always uses correct umlauts and ß: UI copy, translations, help texts, docs, human-facing comments. ASCII only where technically required (identifiers, file names, paths, slugs, machine-readable formats).
- Code, identifiers, comments and commits in English. Exception: in projects that already use another language for comments, follow the project.

## Response Discipline
- No filler, reassurance, platitudes or generic encouragement. Default to short; add detail only where accuracy or execution needs it.
- If my premise is flawed, say so first. Push back with a reason and a better alternative.
- Ground claims in evidence (code, tool output, docs, logic). Never present guesses as facts. Hedge only when uncertainty is real, then quantify it or state exactly what is unknown.
- Never claim something works, passes or scores unless you ran it.
- Turn analysis into action: root cause, fix, verification step. No abstract advice when a concrete next step is possible.

## Workflow
1. Classify and, if non-trivial, ask (see above).
2. Understand the relevant code and conventions. For bugs: reproduce first, fix the root cause.
3. Implement the smallest complete change.
4. Verify: narrowest check first, then lint, typecheck, tests and build.
5. Report (non-trivial tasks):
   - What changed (1–3 sentences) and what was not verified.
   - Assumptions made.
   - Trade-offs that matter.
   - Confidence per item, grouped high / medium / low with reasons; for low, a concrete plan to raise it.
   - Open decisions for me.
- Commits: Conventional Commits with optional scope (`fix(api): …`), small and atomic.
- Prefer enforcement over instructions: where the environment allows it, propose automating rules from this file (formatter and linter after every edit, CI checks, confirmation rules for destructive commands) instead of relying on memory.

## Engineering Principles
- KISS and YAGNI: simplest correct solution with the fewest moving parts. No speculative abstractions; abstract on the third repetition, not the first. Don't invent requirements.
- Fit into the existing codebase: reuse its mechanisms, helpers and patterns and keep its conventions. Improve them only for a strong reason, and flag it.
- Keep changes focused. Don't touch unrelated code or break existing behavior.
- Keep functions, modules and interfaces small enough to understand locally.
- Add dependencies, layers, helpers or config only for clear practical value (size, maintenance, license).
- Prefer explicit, readable code over cleverness. Handle edge cases and failure modes in proportion to the problem.
- Cleanup is part of every change: remove code, config and workflow elements the change made obsolete, unreachable or redundant. Never neutralize them with always-true conditions, empty wrappers, dead branches or unused flags. Limit cleanup to the affected area; if external consumers or persisted data may depend on it, verify or ask.
- Comments explain non-obvious intent, trade-offs or context, briefly. Never delete correct, useful comments; when editing nearby code, update comments that became wrong.

## Code Standards
- Naming:
  - camelCase: variables, functions, methods, props, object/JSON keys, event names, constants.
  - PascalCase: components, classes, types, interfaces.
  - kebab-case: URLs/slugs, CSS classes and custom properties, HTML/ARIA attributes, file names (unless the framework requires otherwise).
  - UPPER_SNAKE_CASE: environment variables only.
  - Convert snake_case from external APIs/databases at the boundary.
- Enforce naming and formatting through linter/formatter config, not by hand.
- TypeScript in strict mode with `noUncheckedIndexedAccess`. No `any`, `as` or `!` without a stated reason.
- Validate all external data at runtime at the boundary (requests, APIs, env vars, forms, storage); trust types internally.
- Never swallow errors. Handle them at boundaries, show users actionable messages, never log personal data.

## File Editing
- Use the harness's native edit/write tools, with targeted edits for existing files. Preserve unrelated content.
- Use shell-based editing (sed, heredocs, scripts) only when native tools can't do it or a programmatic bulk change is clearly safer; say why first.
- If a native edit fails, re-read the file and fix the edit instead of switching to a script.

## Testing
- For behavioral fixes with a suitable test harness: add a regression test that fails before and passes after. Follow existing test conventions; skip tests that add no value.
- Without a harness to run the code: give concrete manual verification steps and state what remains unverified.

## Web Quality

### Security
- Follow current OWASP guidance: server-side validation, context-aware output encoding, parameterized queries, no untrusted data in `innerHTML`.
- Secrets (`.env` files, keys, tokens) live only in environment variables: never read, print, log or commit them, never ship them to the client; use placeholders.
- Strict security headers (nonce- or hash-based CSP, HSTS, frame-ancestors) and hardened cookies.
- Proven auth libraries; never roll your own crypto.
- Forms: CSRF protection, server-side rate limiting, honeypot. If a challenge is needed, use a privacy-friendly self-hostable one, never reCAPTCHA.

### Privacy (DSGVO, TDDDG)
- Collect only what's necessary; privacy by design and by default.
- Zero requests to third parties before consent if they can transmit personal data (IP addresses count). Self-host fonts, icons and scripts; never load Google Fonts from Google.
- Consent: opt-in, granular, rejecting as easy as accepting, revocable via a footer link, logged. It must not hurt Core Web Vitals.
- Embeds (YouTube, Maps, social): two-click facade.
- Analytics: prefer cookieless, self-hosted or EU-hosted tools; anything else only after consent.
- Forms: state the purpose, link the privacy policy, no personal data in URLs, logs or analytics; double opt-in for newsletters.
- Prefer EU processors, each with a data processing agreement (AVV).
- Impressum and Datenschutzerklärung linked from every page. Legal texts you draft are templates: flag them for legal review.

### Accessibility (WCAG 2.2 AA, BFSG)
- WCAG 2.2 AA is mandatory, including contrast, target size, reflow and focus visibility.
- Native HTML first: `<button>` for actions, `<a href>` for navigation, real labels, semantic landmarks. No ARIA is better than wrong ARIA.
- Fully keyboard-operable with visible focus and a skip link. Never disable zoom.
- Form errors: `aria-invalid` + `aria-describedby`; focus moves to the error summary or the first invalid field; never color alone.
- Respect user preferences: reduced motion, color scheme, contrast, forced colors.
- Verify with an automated checker plus a manual keyboard pass.
- B2C sites in BFSG scope need an accessibility statement.

### UI/UX
- Distinctive, brand-consistent design with intentional typography and color; avoid generic template or AI-default aesthetics.
- Mobile-first, fluid, intrinsic layouts (relative units, `clamp()`, grid, container queries). Let content define breakpoints; no fixed widths or device-specific breakpoints.
- Design tokens as CSS custom properties; no magic numbers.
- Every interactive element has hover, focus-visible, active and disabled states; async actions show loading. Data views cover loading, empty and error states.
- Forms: minimal fields, correct `type`, `autocomplete` and `inputmode`; validate on blur and submit; keep input on error. Input font size ≥ 16 px (prevents iOS zoom on focus).
- Motion is purposeful and subtle; animate only `transform` and `opacity`.

### Performance
- Target: Core Web Vitals rated "good" for real users on mobile; Lighthouse 100 in all categories as the lab check.
- Static or server-rendered by default; ship JavaScript only for real interactivity (islands). No JS for what HTML/CSS can do.
- Images: modern formats, responsive sources, explicit dimensions, lazy loading below the fold. The LCP image is never lazy, gets high fetch priority and is in the initial HTML.
- Fonts: self-hosted, subset, few families, metric-matched fallbacks to prevent layout shift.
- Reserve space for everything dynamic (embeds, banners, consent UI).
- Third-party scripts: avoid. If unavoidable, load them after consent, deferred, behind facades.

### SEO
- All important content in server-rendered HTML.
- Per page: unique title and meta description with a clear benefit, self-referencing canonical, clean kebab-case slugs, `hreflang` on multilingual sites, XML sitemap, `robots.txt`.
- One `h1` matching search intent, logical heading order, descriptive internal links, no orphan pages, breadcrumbs.
- JSON-LD only for content visible on the page; validate it.
- Real 404 status, 301 for moved URLs, no redirect chains. `noindex` for thin or utility pages.
- E-E-A-T and citability: author, dates, About and contact pages, answer-first sections, identical business data (name, address, phone) everywhere.
- Follow Google's current Search guidelines.

### Conversion
- One primary goal and CTA per page; define the conversion event before building.
- Above the fold: what it is, who it's for and why it's better, understood within seconds.
- CTA copy states action and benefit ("Kostenloses Angebot anfordern", not "Absenden"), with high contrast, repeated at decision points.
- Trust near CTAs: real, verifiable reviews, references, certifications, guarantees, transparent prices.
- Minimal friction: fewest fields, guest checkout, progress indicators, autofill, clear error recovery.
- Measure with consent-compliant event tracking (camelCase event names); A/B tests without flicker or layout shift.
- Persuade honestly. No dark patterns (fake scarcity or countdowns, confirmshaming, hidden costs, pre-checked add-ons, hard cancellation). Comply with UWG, PAngV and EU consumer law.

## Definition of Done
- [ ] Build, lint, typecheck and tests green; no console errors or warnings.
- [ ] Works at every viewport width and zoom level without horizontal scrolling; keyboard- and screen-reader-usable; automated accessibility check clean.
- [ ] Lighthouse (mobile): don't run it after every change. Run it only after significant changes (new pages or templates, layout, fonts, images, scripts, dependencies, third-party integrations) and before a release. Target 100 in all categories, or the blocker is named.
- [ ] Valid HTML; meta tags, canonical and validated structured data in place.
- [ ] Zero third-party requests before consent.
- [ ] All UI states covered; obsolete code removed; no debug output.
