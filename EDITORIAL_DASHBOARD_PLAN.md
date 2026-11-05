# Paws & Proud — Editorial Dashboard Plan

Status: Proposed workflow, not implemented. 2026-11-05.

## Product constraints
- Dogs and cats only.
- Operating budget target: $15/month maximum.
- All AI content stays unpublished until explicitly approved by the owner.
- Mobile-friendly, warm and polished, light/dark mode matching the public site.
- Clear distinction between draft generation, source verification, editorial approval and publication.

## Navigation
1. Overview — drafts awaiting review, approved/scheduled, published, failed tasks, monthly estimated spend.
2. Review Queue — filters by Dogs/Cats, draft status, source-check status, newest first; title, short excerpt, date, and actionable review link.
3. Published — searchable articles, status, URL, date; unpublish and edit.
4. Settings — AI generation cadence, spending cap, topics, publishing controls, notifications.

## Review flow
Draft generated → cited claims inspected → pending review → owner edits or rejects or approves → publish or schedule → audit record. No auto-approval, including after timeouts or errors. Approval is per article/version: changing approved text invalidates that approval.

## Draft editor
Title, excerpt, body, heading hierarchy, featured image, image license/source, citations, claims requiring review, metadata, slug, preview on mobile, Save Draft / Reject / Approve & Publish / Approve & Schedule. Publishing confirmation and clear error states. Drafts should never be visible publicly before approval.

## Technical acceptance criteria
- Authentication and server-side authorization for all dashboard and publishing endpoints.
- Persistent storage for drafts, versions, decisions, sources, costs and publication records.
- Idempotent publishing and retry logic; failures never publish without approval.
- Responsive keyboard-accessible interface; contrast at least WCAG 2.2 AA for normal text; readable focus states; accessible controls.
- Spending ledger and hard limit before initiating paid AI jobs; notify when budget near cap.
- Generated content must not invent citations, author credentials, or imply editorial verification it has not received.
- Audit log records who approved which version and when.
- Confirm actual deployment and end-to-end test with an approved test article before launch.

## Evidence and rationale
- W3C WCAG 2.2: contrast, focus visibility, target size, reflow, headings and labels: https://www.w3.org/TR/WCAG22/
- Nielsen Norman Group on recognition over recall and visibility of system status: https://www.nngroup.com/articles/ten-usability-heuristics/
- Google Search Central on helpful people-first content, including AI-assisted content: https://developers.google.com/search/docs/fundamentals/creating-helpful-content
- NIST AI Risk Management Framework for human oversight and documentation: https://www.nist.gov/itl/ai-risk-management-framework

These sources support accessibility, clarity, editorial oversight and responsible content quality; they do not prove that any particular dashboard arrangement maximizes retention. Validate with actual use.

## Saved for Later (approved requirement, 2026-11-05)
- Owner can move any generated article to a dedicated **Saved for Later** section without publishing it.
- Keep full article body, title, image, source references, verification report, category, and revision history.
- Saved items are distinct from drafts awaiting work, published posts, and trash.
- Owner can reopen, edit, return to the approval queue, or explicitly publish after passing required checks.
- Recheck time-sensitive claims, working citations, and image rights before publishing a saved item, especially after substantial delay or editing.
- Saving never implies approval, scheduling, or publication. Only explicit owner approval of the current verified version permits publication.
- Acceptance test: given three generated posts, save one, approve the other two, confirm the saved item remains private and recoverable; later reopen and approve it separately.
