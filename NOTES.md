# Open notes — known issues in the brief

Findings from a data-verification and interaction audit of `index.html`. None of these
are fixed yet; they are recorded here so the work isn't lost. Ordered roughly by how
badly they'd hurt in front of an interviewer.

## Data accuracy

Checked against the live Supabase billing docs (`billing-on-supabase`,
`manage-your-usage`, `org-based-billing`, `billing-faq`).

**Confirmed wrong:**

- **Spend Cap exclusion list is incomplete.** The brief says "compute, IPv4, replicas,
  PITR" bill through the cap. The docs also list Branching Compute, Custom Domain,
  provisioned Disk IOPS/Throughput, Log Drains, and MFA Phone. This phrasing appears in
  roughly six places (§03, the bundle description, the diagram caption, §10, §11).
- **Egress is described as leaking through the Spend Cap.** In the Thesis A risk line.
  This is backwards — egress *is* covered by the cap. This is the most quotable error
  in the brief, since the whole Spend Cap argument rests on which meters it catches.
- **"CU-hours per project" is Neon's unit, not Supabase's.** Supabase invoices
  "Compute Hours." Using a competitor's terminology for Supabase's own meter is exactly
  the detail a pricing interviewer would notice.
- **Navigator cards point at the wrong sections.** The "How to read this" cards cite
  §09 / §10 / §11 / §12 for the Bundle Builder, validation plan, and open questions;
  the actual section numbers are §08 / §09 / §10 / §11.

**Unverified — spot-check these personally before using the brief.** The build
environment had no general web access, so every non-Supabase number rests on the
original drafting research and has not been re-confirmed: the Neon rows
($0.106/$0.222 per CU-hr, Databricks acquisition, "storage cut 80%"), PlanetScale
("killed Hobby, Apr 2024"), Vercel (Active CPU launch, "75% on Fluid", "up to 90%
savings"), Clerk (Feb 2026 changes, 50K MRU, $20 annual), Cloudflare ($5 / 100K reqs
per day), Cursor ($2.3B raise at $29.3B, Nov 2025), the Cara ~$98K Vercel bill, the
~7M developer count, and the $5B Series E / reported $10B talks.

Also worth a second look: the Free tier's 7-day auto-pause and the Pro tier's $10
compute credits, both stated as fact in the recommended-tiers table.

## Interaction and robustness

- **The bill-range chart is not to scale.** Min, Expected, and Worst-case markers are
  pinned at 0% / 50% / 100% regardless of the dollar values behind them, but the axis
  line and gradient imply a proportional scale. $27 between $25 and $200 renders
  identically to $100 between $99 and $101. Either make the positions proportional or
  label the strip "not to scale."
- **The amber threshold contradicts its own caption.** The Worst-case marker turns amber
  above 3× expected, while the caption buckets switch at 5× / 2.5× / 1.4×. Around 2.7×
  the caption warns about surprise bills while the marker stays neutral.
- **The phase timeline breaks on phones.** Hard-coded `grid-cols-7` with no breakpoint,
  so it stays seven columns at 375px (~45px each), and the year labels are
  `whitespace-nowrap overflow-hidden` — they clip silently.
- **The diagram shows six meters; the brief claims nine.** `MetersFlowDiagram` hard-codes
  Compute, DB, Storage, Egress, MAU, Vector, directly under a section headlined "Nine
  meters, one invoice."
- **No accessibility affordances at all.** Zero `aria-*` attributes in the file. The three
  selectors (timeline, persona, bundle) convey selected state through color alone with no
  `aria-pressed`, the Bundle Builder recomputes an entire panel with no `aria-live`
  region to announce it, the SVG diagram has no `role="img"` or label, and there is no
  `focus-visible` styling — keyboard focus on the dark buttons is nearly invisible.
- **No `prefers-reduced-motion` handling.** Two infinite pulse animations (hero status
  dot, "Begin brief" arrow) run regardless of the user's setting.
- **Step 1 pressure meters show bare numbers.** No "/100", inconsistent with every other
  meter on the page. "Usage volatility" is worse: the bar renders `volatility × 16` while
  the note prints `{volatility}x` — two different numbers for one row.
- **The `Soundbites` component is dead code.** It `return null`s, so its lines never
  render. Either delete it or wire it in.

## Maintainability

`index.html` is a build output — inlined React, precompiled JSX, and static Tailwind CSS
in one file, which is what makes it work on GitHub Pages with no CDN. The readable JSX
source is not in the repo, so any future content edit means either editing compiled code
or reconstructing the source. Worth committing a `src/app.jsx` plus a small build script
before the next round of changes.
