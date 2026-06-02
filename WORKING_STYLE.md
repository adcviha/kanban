# Working Style & Collaboration Protocol

Extracted from CLAUDE.md — drop this into another project's CLAUDE.md or use as a standalone reference.

## Code conventions

- **Readable code over clever code.** No minification, no one-liners flexing. Comments explain *why*, not *what*.
- **Hackable.** Someone with intermediate skill in the language should be able to open the source files, find the relevant section, and modify it without setup.
- `const` declarations before any code that uses them, even within a file.
- Functions short and named for what they do.
- Internals exposed on a global for console hacking (e.g. `window.DEBUG`).

## Version incrementing (SemVer)

We use **Major.Minor.Patch** as strings, not decimals. `0.9.10` comes after `0.9.9`.

- **Patch** (0.9.0 → 0.9.1): Bug fixes, polish, small tweaks. No new features.
- **Minor** (0.9.0 → 0.10.0): New feature, tool, or system. This is the only thing that moves the "feature number" forward.
- **Major** (0.9.0 → 1.0.0): Breaking change to the data format, data model, or core workflow.

**CRITICAL: DESIGN.md milestones are Minor targets.** When DESIGN.md says "v0.9 — Selection transform," that means the 0.9.x series delivers selection transform. Ship it as `0.9.0`. Bug fixes after that are `0.9.1`, `0.9.2`, etc. Do NOT bump the Minor digit for bug fixes.

Git commits are tagged with the version. Push to master auto-deploys.

## Working protocol (every task, every time)

Before writing any code, follow these four steps in order:

### 1. Translate the Vibe
The user describes behavior in plain English ("make the camera feel heavier"). Before touching code, translate this into technical terms ("add camera damping — increase the lerp factor so movement has more inertia") and state your approach in one sentence. If the description is ambiguous, ask the one clarifying question that matters most.

### 2. Version bump
State the new version number based on SemVer (see above). The version string goes in every place it's referenced.

### 3. The GO Gate
List every file you will modify or create. State the plan in 2-3 sentences. Then **stop and wait** for the user to say "GO" before writing any code. Do not output code, diffs, or implementations until you hear "GO."

### 4. No reinventing wheels
If a request can be handled by an existing library method, browser API, or a function already in the codebase, say so and use it. Don't write custom math when the library already does it.

## When making changes

- **Always follow the Working Protocol** (Translate → Version → GO Gate → No Reinventing). Never skip the GO Gate.
- Test in-browser (or the project's equivalent). There is no lint or build step to lean on.
- Don't introduce a build step "to make things cleaner." The build step IS the cost.
- If a feature is getting complex, that's a signal to redesign the workflow, not pile on code.
- Match the user's pace. They are still in early prototyping. Don't over-engineer ahead of decisions they haven't made.
- One commit per version increment. Don't batch unrelated changes.

## When to push back

- A user request that would require adding a framework, build chain, or large dependency — flag the tradeoff before doing it.
- A feature that conflicts with the project's stated aesthetic or simplicity targets.
- Anything cascading or recursive (prefabs containing prefabs, nested editors) — the user may explicitly want a flat mental model.

## When to confirm before coding

- **The user describes a behavior change imprecisely** (vague language, contradictory signals, "I'm not sure how to describe this"). Pause, replay what you understood in your own words, and get explicit confirmation before touching any code.
- Examples of signals: "I'm not exactly sure how to describe that," "it's like... but also...," "I can't imagine exactly without trying," or when they describe a fix but also describe behavior that contradicts the fix.
- Once confirmed, implement exactly what was agreed. Don't add "while I was in there" changes.
