# Contributing Guidelines

Thanks for considering a contribution! This list only works if entries stay
accurate, so please read this before opening a pull request.

## Before you submit

- **Search first.** Check the README and open/closed PRs to make sure the
  tool isn't already listed (or already rejected, with a reason).
- **One tool/change per PR.** Makes review and history much easier to
  follow.
- **The tool must be self-hostable.** SaaS-only products, even GDPR-friendly
  ones, don't belong here — this list is specifically about running your own
  infrastructure. If you think a SaaS tool is a better fit for a related
  list, say so in the PR and we'll point you at one.
- **Fits a solopreneur/small-team scale.** Enterprise-only tooling that
  assumes a dedicated platform team is out of scope.

## What every entry needs

Format:

```markdown
- [Name](https://link-to-repo-or-homepage) - One or two sentence, honest description. ⚠️ Any license caveat, telemetry/US-cloud call-out, or maturity warning goes here.
```

- The link should go to the project's source repository (preferred) or
  official homepage if there's no public repo.
- State the actual license if it's anything other than a standard permissive
  open-source license (MIT/Apache-2.0/BSD). Source-available licenses
  (Sustainable Use License, Elastic License, BUSL, HFOIL, etc.) **must** be
  called out with a ⚠️.
- If the tool sends data to a third-party/US-hosted endpoint by default
  (telemetry, crash reporting, a "cloud" tier that's easy to enable by
  accident), say so with a ⚠️. If you're not sure, say what you checked and
  we'll help verify before merging.
- No dead links. No archived/abandoned repositories (check for at least
  some commit activity in the last 12 months) unless you're adding it with
  an explicit "unmaintained, listed for historical/migration reasons" note.

## What won't be merged

- Entries you haven't actually used or at least read the source/docs for.
  "I saw it trending" is not verification.
- Marketing copy lifted from a project's landing page. Write a plain,
  independent description.
- Anything that isn't self-hostable, or that's fully proprietary with no
  open-source or source-available self-hosted option.
- Affiliate links or anything that benefits you financially from inclusion.

## DSGVO / Compliance Basics section

That section summarizes publicly available regulator guidance. Changes to
it need a link to the actual primary source (a DPA publication, the AI Act
text, an official EU institution page) — not a blog post's summary of a
summary. If you're not citing a primary source, open an issue to discuss
first rather than a PR.

## Style

- Keep descriptions to 1–2 sentences plus an optional ⚠️ sentence.
- Alphabetical order isn't enforced within categories; entries are ordered
  by how directly relevant they are to the category's core use case.
- Run `npx awesome-lint` locally before opening a PR — CI runs it too, but
  catching issues locally saves a review round-trip.
