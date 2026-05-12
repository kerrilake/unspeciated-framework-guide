# Framework Guide — Developer README

**Repo:** `unspeciated-framework-guide`
**Deployment:** [unspeciated.netlify.app](https://unspeciated.netlify.app) (Netlify, auto-deploy from GitHub)
**Custom domain:** [unspeciated.com](https://unspeciated.com)
**Role in ecosystem:** Public-facing narrative tour of the framework. The first articulation many newcomers encounter.

---

## What the Framework Guide is

The Framework Guide is a **seven-section narrative tour** that introduces the Unspeciated™ Perceive · Relate To · Apply framework to people meeting it for the first time. It's not a tool — it's a guided experience. A visitor opens the page, sees a question that orients them ("What if the species you work with are already showing us what true intelligence looks like?"), and walks through seven sections that introduce the framework, show what 172 species reveal when mapped through it, let them explore featured species, and offer a vision of what the framework makes possible.

The Framework Guide sits at the **public face of the ecosystem**, at the `unspeciated.com` domain. Where PRAcel meets first-time visitors in conversation, the Framework Guide meets them in narrative — a structured introduction that can be experienced in five minutes or lingered in for an hour, depending on how much the visitor wants to engage with each section.

This app has no auth, no API calls, no Firebase, no Cloud Functions. It's a self-contained React application that runs entirely in the browser. The content lives in the code; the experience is the rendering of that content with care.

---

## The seven sections

The guide walks through seven sections in order, with previous/next navigation, nav-dot indicators, swipe support on mobile, and per-section animations:

1. **Welcome** — Opening invitation framing the framework, with subtle fade-in animations and a single "Begin" call-to-action
2. **The Question** — Three lines that emerge sequentially, naming what the visitor already knows about species intelligence and offering a framework that can hold it
3. **P / R / A** — The framework itself, with tappable expansion for each of the three dimensions (Perceive, Relate To, Apply), full descriptions, and species examples
4. **What Life Shows** — The data visualization: 61% Relate To-primary, 18% Perceive-primary, 12% Apply-primary, 9% Balanced. A bar chart that animates in, plus a contextual note that humans are one of the few species that are Apply-primary
5. **Species** — An interactive species explorer with search and filter (All / Relate To / Perceive / Apply), 21 featured species each with tagline, narrative, and optional dimensional scores
6. **Recognition** — Two lines that emerge sequentially, naming homotopy equivalence (elephants and mycelial networks, wolves and dolphins, bears and hyenas) and the framework's invitation to recognition
7. **What's Possible** — Four expandable cards on shared language across disciplines, education that sees species on their own terms, conservation informed by intelligence patterns, and a different foundation for AI

The visitor moves through sections via the "Next" button, the nav dots, or swipe gestures on mobile. The "Back" button appears once they've left Welcome.

---

## The 21 featured species

The species shown in the Species section are a curated subset of the framework's larger corpus (currently 172 species in the full dataset, surfaced as "Full dataset: 172 species across every kingdom" in the explorer's footer note).

The featured species and their primary dimensions:

**Relate To-primary** (12 species): Aspen Trees, Bears, Coral Reefs, Crows, Dolphins, Elephants, Horses, Kelp, Lactobacillus, Mycelial Networks, Whales, Wolves, Cassowary

**Perceive-primary** (5 species): Crystals, Flies, Grasshoppers, Jellyfish, Mantis Shrimp

**Apply-primary** (2 species): Frogs, Humans

**Special** (1, excluded from the explorer but defined in the data): Earth, marked as Perceive but with values that put it in a category of its own

Each species carries:
- A common name and kingdom classification
- An emoji icon
- A tagline (one evocative phrase)
- A narrative (one paragraph of framework-grounded description)
- Three dimensional scores (P, R, A) on a 0-1 scale
- An assigned primary category (which the score values support)

The species data is hardcoded in `index.html` as a JavaScript array. Changes to the featured set require editing the array directly.

---

## What's in this repo

```
unspeciated-framework-guide/
├── index.html              — the entire app: HTML structure + inline React via CDN + species data + section components
└── README.md               — this file
```

That's it. One file plus a README. The HTML loads React 18 and ReactDOM via CDN, loads two Google Fonts (Newsreader serif, JetBrains Mono), and runs the entire application from inline JavaScript inside a single `<script>` block.

This is the smallest-footprint repo in the ecosystem. No build step. No dependencies installed locally. No backend. No deployment configuration beyond Netlify pointing at the repo and the custom domain pointing at Netlify.

---

## How the app is built

### React via CDN, no build step

The Framework Guide uses **React 18 via CDN** (production builds of `react.production.min.js` and `react-dom.production.min.js`). It does not use JSX or Babel — instead, it uses React's `createElement` function (aliased as `h`, then wrapped in a helper called `E`) to build the component tree programmatically.

This pattern is unusual but deliberate. The advantages are:
- **No build step** — push to GitHub, Netlify serves the file as-is
- **No bundler dependencies** — React's CDN handles delivery, no `node_modules` to manage
- **One file** — everything that runs the app is in `index.html`, making the codebase legible at a glance

The disadvantages are:
- Reading the component code requires fluency with `createElement` rather than JSX (the function-call form looks denser than JSX)
- Editing components requires careful attention to nested function-call structure rather than the more visually obvious JSX hierarchy

For Alex's purposes: the pattern works well for what this app is (a small narrative tour with no growing complexity). If the Framework Guide grew significantly — for example, into a multi-page experience with routing, or if the species data started being fetched from Firestore — a build step with proper JSX would become worth introducing. As long as it remains a single-page narrative tour with hardcoded content, the current pattern is fit-for-purpose.

### Component structure

The app's seven sections are seven React components:

- `WelcomeSection` — opening fade-in, "Begin" CTA
- `QuestionSection` — three sequenced text lines with staggered timing
- `FrameworkSection` — three tappable P/R/A dimension cards with expansion
- `DataSection` — the animated bar chart with legend and humans-context note
- `ExplorerSection` — the species explorer with search, filter pills, and per-species expansion (includes the `ScoreBar` sub-component for dimensional scores)
- `RecognitionSection` — two sequenced text blocks on homotopy equivalence
- `VisionSection` — four expandable "what's possible" cards plus a closing reference panel

A `PRAGuide` component wraps all seven and handles navigation, swipe gestures, and section transitions. The navigation appears as a top bar once the visitor has left the Welcome section.

### Visual standards

The Framework Guide is the **clearest visual expression** of the GoH palette and typography across the ecosystem:

- **Background:** `#2a2725` (charcoal)
- **Primary text:** `#f0ebe3` (warm cream)
- **Perceive:** `#d6a86c` (warm gold)
- **Relate To:** `#6a9ab5` (teal, lighter variant) / `#3d6178` (teal, darker variant)
- **Apply:** `#c47a52` (terracotta)
- **Fonts:** Newsreader serif for body and display, JetBrains Mono for labels and metadata
- **Subtle dot texture** at 1.5% opacity in the background, creating warmth without distraction

These conventions match the GoH visual standards documented in `GoH_Build_Standards_and_Workflows_v1_2.md` and reflected across the rest of the ecosystem. If anything looks visually wrong in another app, this is the reference.

---

## Authentication and access

**There is none.** The Framework Guide is fully public. Anyone with the URL can experience the tour. No sign-in, no tier check, no Firebase integration of any kind.

This is intentional. The Framework Guide is the framework's public introduction, and asking visitors to sign in before they meet the framework would be a barrier in the wrong place. PRAcel handles the gentler conversational first-encounter (also public), and Research Explorer / Researchers' Mode / the Map and the rest sit behind various access gates for users whose engagement has matured.

The Framework Guide is the doorway before the doorway — the moment someone might encounter the work via a link from a paper, a social post, or a partner's website, and decide whether they want to know more.

---

## Database collections used

None. The Framework Guide does not read from or write to Firestore.

The 21 featured species are hardcoded JavaScript objects in `index.html`. Statistics like "61% Relate To-primary" and "172 species" are static text reflecting the corpus state at the time of the last content update.

This means the Framework Guide can drift from the live corpus over time — if the corpus grows to 200 species or the primary-dimension percentages shift meaningfully, the Framework Guide won't reflect that automatically. Updates are manual edits to `index.html`.

---

## Deployment

GitHub → Netlify auto-deploy. Push to `main` triggers a Netlify build (no build step required — static site); Netlify serves at `unspeciated.netlify.app`, and the custom domain `unspeciated.com` points at the Netlify deployment via DNS configuration.

```bash
git clone [repo URL]
cd unspeciated-framework-guide
# Open index.html in a browser directly — no local server needed
# Or use a simple server for testing:
python3 -m http.server 8000
# Open http://localhost:8000
```

No build configuration. No environment variables. No Firebase secrets. No Cloud Functions to deploy.

---

## What's working well

The Framework Guide is in stable operation as of May 2026. The narrative flow works on desktop and mobile; the swipe gestures feel natural; the animations are calibrated to introduce sections without dominating them. The featured species content is current to recent framework discipline (affirmative-first language, "primary" not "dominant," appropriate handling of homotopy equivalence).

This is the app that most often gets shared as the framework's introduction in outreach contexts, conference materials, and email signatures. Its job — being a beautiful, accurate, accessible first encounter — is the job it's doing.

---

## What's pending

### Near-horizon

No active build queue items affect the Framework Guide directly. The app is operating as intended.

### Mid-horizon

- **Content currency.** As the species corpus grows (currently 172 species, with active expansion through Tool 5), the Framework Guide's featured species set and statistics should be periodically reviewed and updated. The "61% / 18% / 12% / 9%" distribution reflects a particular moment in the corpus; significant corpus growth could shift those numbers.
- **Featured species selection.** The current 21 featured species are a curated subset chosen to represent dimensional variety, kingdom variety, and intuitive recognizability. As the corpus grows and the framework matures, the featured set could be revisited — perhaps with periodic rotations of which species are featured, perhaps with seasonal or thematic curations.
- **Conversion path to the rest of the ecosystem.** The closing Vision section points to `researchexplorer.live` and `pracel.live`. As the ecosystem evolves (particularly with Research Explorer moving behind Institute Partner access per the In-House Repositioning decision), the closing call-to-action may want to direct visitors specifically toward PRAcel as the public next step.

### Known quirks worth knowing

- **The Earth entry is special-cased.** Earth is defined in the species data with P/R/A scores (0.92, 0.90, 0.80) but is explicitly filtered out of the Species explorer. The data is there for future use; the rendering choice was to keep the explorer focused on more recognizable beings rather than open with a planetary-scale entity. Worth knowing if Earth ever shows up unexpectedly or is asked about.
- **The repo's name is `unspeciated-framework-guide`** but a stale legacy README in the repo refers to it as `unspeciated-portal` (see Historical note below).
- **The `framework-guide.html` in the Intelligence Map repo is a different file.** That file is the Map's inline framework guide modal — accessed via the Map's footer link — and is not the same as this standalone app. Both exist, both are named "framework guide," and they serve different contexts.

---

## Historical note: the `unspeciated-portal` README

The repo currently contains (or did until recently) a legacy README titled **"Unspeciated™ Research Portal"** that describes a planned authentication-and-tier-gateway portal — a single entry point that would have managed access across all the apps. This is **not** what the current Framework Guide is.

The repo was originally created for that planned portal project, then repurposed when the Framework Guide became the priority for the `unspeciated.com` domain. The README was never updated when the repurposing happened, so the description of the codebase has been stale for some time.

The legacy README has value as **provenance** — it captures an earlier architectural vision where access lived centrally rather than per-app. That model isn't what the ecosystem became (auth lives per-app via `core-auth.js`, not via a central portal), but knowing it was once considered is useful context.

The legacy README has been preserved as `archive/legacy_unspeciated_portal_README.md` in the central onboarding folder. It's out of the main reading order, available if someone wants to understand "what was that portal idea?"

---

## Working on the Framework Guide — orientation for Alex

If you're touching the Framework Guide for the first time, a recommended path:

1. **Read this README**
2. **Visit unspeciated.com** and walk through all seven sections at your own pace. Notice the timing of the animations, the texture of the language, the way each section opens onto the next.
3. **Open `index.html` in an editor.** It's a single file. Read it top to bottom. The species data is at the top; the section components are next; the main `PRAGuide` component is at the bottom.
4. **Try a small edit locally.** Change a species tagline, refresh the file in a browser, see the change. The no-build-step pattern means the iteration loop is instant.
5. **Read the GoH visual standards** in `GoH_Build_Standards_and_Workflows_v1_2.md`. The Framework Guide is the cleanest reference for those standards in operation.

This app is the simplest in the ecosystem, and the most directly expressive of the framework's voice. It's a good place to develop a feel for what GoH "looks like" before working on the more complex apps.

---

## Related documents

- **`README_PRAcel.md`** — the conversational counterpart to the Framework Guide; together they form the framework's public-facing surface
- **`README_Intelligence_Map.md`** — note the framework-guide.html distinction (different file, similar name)
- **`Build_Queue_Operational_Tracker.md`** — current state of work
- **`Interspecies_Prompt_Design_Axioms_v5.md`** — the framework the guide introduces
- **`GoH_Build_Standards_and_Workflows_v1_2.md`** — the visual and language standards the guide exemplifies
- **`archive/legacy_unspeciated_portal_README.md`** — historical artifact of an earlier portal vision (see Historical note above)
- **`00_START_HERE.md`** — the ecosystem orientation

---

## Contact

For questions, content changes, or significant edits: Kerri Lake (founder/conductor) or Alex Milewski (technical co-builder, once onboarded).

---

*Generation of Harmony LLC · Intuitive Learning Foundation*
*The Framework Guide is the doorway before the doorway. This README is the developer's orientation to that doorway.*
