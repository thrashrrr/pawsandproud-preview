# Paws & Proud — Single-Article Pipeline Test Contract
Status: Implementation-ready test specification; no live AI, database, scheduler, or publishing integration has been enabled.
Date: 2026-11-05

## Workflow
1. Scheduled job proposes one nonduplicate Dogs or Cats topic.
2. Budget gate checks estimated cost against the $15/month hard ceiling BEFORE any paid request. Reserve estimated cost atomically; reconcile actual cost after.
3. Research collects original, attributable sources and publication dates.
4. Draft generator writes specific, naturally worded, nonformulaic prose using the approved EDITORIAL_WRITING_STANDARD.md.
5. Claim verification maps each material claim to supporting evidence, checks URLs and source relevance, and blocks unsupported or conflicting claims. No AI checker may mark its own unsupported assertion as verified.
6. Image checker validates exact image identity, source, commercial license and applicable third-party rights. If clearance is incomplete, block publication.
7. On success, save the draft and verification report in persistent storage and put it in the owner's Review Queue. Failures go to Exceptions, never Review Queue.
8. Owner may Edit, Save for Later, Reject, or Approve. Editing invalidates earlier approval and triggers re-verification.
9. Only an authenticated, explicit approval of the CURRENT verified version may trigger publication.
10. On successful public publication, set published_at server-side to actual publication timestamp; retain created_at and saved_at separately.

## Proposed minimal article record
id, category, title, slug, body, summary, image_url, image_source, image_license, claims[], sources[], checks{claims,links,terminology,style,image}, status, version, approved_version, created_at, saved_at, scheduled_for, published_at, updated_at, estimated_cost_usd, actual_cost_usd, audit_events[].

## Statuses
researching -> drafting -> verifying -> ready_for_review -> saved_for_later OR approved -> published
Errors -> exception; rejection -> rejected; deletion -> trash.
Approval never follows automatically from timeouts, retries, saved status, or successful checks.

## First end-to-end test
- Generate exactly one Dogs draft using real sources.
- Assert every cited URL opens and supports its linked claim.
- Verify the selected photo's exact original source and commercial rights.
- Assert the article is not accessible publicly before approval.
- Save for Later, reopen, and confirm it remains unpublished.
- Edit, then verify any previous approval is invalidated.
- Approve a checked version and confirm exactly one public article and correct published_at.
- Test delete/unpublish, restore, and permanent delete permissions.
- Simulate budget exhaustion and verify no paid job begins.
- Test retries and concurrent jobs to ensure no duplicate publications.

## Cost controls
The $15/month target includes AI API calls and any other paid services. A hard limit needs server-side enforcement; a dashboard display alone is not sufficient. At the ceiling, stop jobs and alert the owner. Prefer reducing output volume over weakening verification.

## Implementation boundary
The public GitHub Pages preview is static HTML and local browser demo data. A secure backend, authentication, persistent database, private credentials, AI provider, scheduler, and publishing service are required for real operation. Never place API keys or privileged write endpoints in public client code.
