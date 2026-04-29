# Supabase Pricing & Billing — Strategic Brief

A measured read of Supabase's current pricing surface, the buyer-base shift driving the next decision, and the validation sequence I'd run before any structural change ships.

Prepared by Kelvin Zee ahead of an interview for the Product Manager, Pricing & Billing role at Supabase. April 2026.

**Live site:** [your-github-username.github.io/supabase-pricing-brief](#)

## What this is

A self-contained interactive brief covering twelve sections — context, telecom-evolution analogy used carefully, current Supabase pricing architecture, competitive landscape, AI-builder customer shift, segment map, four pricing theses, an interactive Bundle Builder (persona × bundle structure → bill / trust / margin trade-off visualization), the framework I'd validate, a 90-day validation plan, open questions, and the candidate read.

**Reading time:**
- 3 minutes — Hero plus §01 (the setup) and §09 (framework I'd validate)
- 15 minutes — add §07 (theses), §08 (Bundle Builder), §10 (validation plan)
- Full read — all twelve sections plus §11 (open questions for Supabase)

## Contact

Kelvin Zee · Seattle, WA · kelvinz@gmail.com · [linkedin.com/in/kelvinz](https://linkedin.com/in/kelvinz)

## Notes on this version

- This is v2, recruiter-primary. Every recommendation is presented as a hypothesis to validate against Supabase's internal data, not a prescription.
- Specific tier prices and quotas are illustrative, benchmarked against external comparables. They would require fake-door tests and customer interviews on Supabase data before being defended as final.
- Sources verified April 2026. Pricing in this category moves quickly; figures may have shifted.

## Tech notes

Single-file static site. React via CDN (unpkg), Tailwind via CDN, Babel standalone for in-browser JSX. No build step. Loads in any modern browser. No tracking, no cookies, no analytics. Hosted free on GitHub Pages.

The interactive Bundle Builder uses illustrative coefficients derived from public bill-shock anecdotes and category benchmarks. Calibration in production would replace these with real telemetry — the directional pattern is the point, not the specific numbers.
