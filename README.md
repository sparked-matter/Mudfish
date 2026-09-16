# Mudfish

**Half sailboat, half survival pod — built with surviving harsh conditions in its DNA.**

**An open-source, LGPL-licensed, fully parametric aluminum sailboat, designed for ocean crossing.**

> **Historical note:** Mudfish is believed to be the first sailboat design released under a formal open-source license (LGPL-3.0) with fully parametric design files, version-controlled documentation, and a public repository — as distinct from "free plans," which lack a legal open-source framework. This repository was made public on GitHub in 2026. All commits are cryptographically timestamped and permanently recorded. If you know of an earlier LGPL-licensed parametric sailboat design, please open a Discussion — we want the record to be accurate.


Twin bilge keels for beachability and easy transport — no centerboard needed. Stitch-and-weld aluminum construction, buildable in a garage with a MIG welder. A hull that bounces off hard objects instead of breaking. Fully parametric design — change one parameter and the hull updates everywhere. Full CNC-ready build files, released free for anyone to build, study, and improve.

Mudfish is a flagship project of [Sparked Matter LLC](https://sparkedmatter.com).

---

## Why Mudfish

Most small-boat design plans are sold, closed, and undocumented — you get a set of drawings and little else. Mudfish takes a different approach:

- **Open.** Full design files, calculations, and reasoning are public, under LGPL-3.0.
- **Accessible.** Designed around amateur-buildable aluminum stitch-and-weld construction, not a professional boatyard.
- **Transparent.** Every major design decision is documented in the [Engineering Log](./engineering-log) — the formulas, the tradeoffs, and yes, the AI-assisted exploration that helped get there. Nothing is a black box.

## Status

🚧 **Early development.** Hull form and stability calculations are the current priority — see the [Engineering Log](./engineering-log) for active work. No finalized build files yet. Watch/star the repo to follow progress.

## Design Philosophy

- **Twin bilge keels** instead of a fin keel or centerboard — dries out upright on a beach or mudflat, simplifies construction, no moving parts below the waterline.
- **Aluminum, not fiberglass or plywood** — dents instead of cracking on impact with reefs, containers, or debris. Variable plate thickness by zone (heavier at the keel/bottom, lighter above the waterline) balances toughness against weight.
- **Aluminum 5086 alloy** — marine-grade, weldable, excellent corrosion resistance in saltwater. The reference design uses MIG welding (accessible to an amateur builder with practice), with serious ongoing consideration for a riveted construction variant — mechanically fastened panels with Sikaflex sealant, no welding equipment required.
- **MIG-first, riveted as a serious alternative** — the welded path requires a quality pulse-MIG welder and practice; the riveted path opens the build to anyone with hand tools and patience.
- **AI-accelerated, human-verified** — FreeCAD and modern engineering tools shorten the iteration cycle dramatically. Every calculation is still checked against real naval architecture principles, not just AI output.
- **Material-agnostic parametric core** — the reference design targets aluminum 5086, but the parametric architecture is designed to accommodate alternative materials: marine plywood, cold-molded wood, fiberglass/foam composite, basalt fiber composite, or other structural materials. Different materials require different plate thickness laws and structural calculations, but the hull geometry engine is not locked to any one material.

## Repository Structure

```
mudfish/
├── README.md              this file
├── LICENSE                LGPL-3.0 full text
├── CONTRIBUTING.md        how to propose changes, ask questions, get involved
├── engineering-log/       dated record of design decisions — the "why," not just the "what"
├── cad/                   FreeCAD source files (coming as work progresses)
└── cutfiles/              CNC-ready DXF/nested layouts (coming once hull form is finalized)
```

## Getting Started

This project is early — there isn't yet a build-ready file set. The best way to follow along right now:

1. **Star/watch this repo** for updates.
2. **Read the [Engineering Log](./engineering-log)** to see current design thinking and open questions.
3. **Open a [Discussion](../../discussions)** if you have naval architecture, welding, or CNC experience and want to weigh in on an open question.

## License

Mudfish is released under the **GNU Lesser General Public License v3.0** — see [LICENSE](./LICENSE). You are free to build, study, modify, and share this design, including forking it in your own direction.

## About Sparked Matter

Sparked Matter LLC works at the intersection of three things: **it matters** (real problems, real purpose), **subject matter** (deep engineering knowledge), and **physical matter** (ideas made into working hardware). Mudfish is that philosophy applied to a hull. Learn more at [sparkedmatter.com](https://sparkedmatter.com).

