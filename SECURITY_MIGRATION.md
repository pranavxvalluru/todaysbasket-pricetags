# Price Tags secure migration (non-production)

This branch is a staging workspace. Production `main` and the existing GitHub Pages URL remain unchanged.

## Confirmed source review
- `index.html` includes browser-side credential/hash checks and sessionStorage login state; these do not provide enforceable access control.
- Excel files are parsed in the browser with SheetJS; tag rendering and printing are browser-side.
- User-specific formatting preferences are stored in localStorage.
- Existing modes include 6-up portrait, 4-up landscape, A4 signage and A10. All need regression testing.
- The repository is currently public. Previous copies and commit history cannot be recalled by making it private.

## Migration acceptance criteria
1. Unauthenticated requests cannot retrieve the application assets or invoke protected APIs.
2. Credentials and password hashes are not embedded in browser assets; authenticate at the edge or trusted backend, with role-based authorization.
3. Sensitive tag business rules are executed server-side where feasible. Browser-visible UI and authorized output remain inherently reproducible.
4. Existing spreadsheet import, column mapping, discount logic, editable tags, layout modes, print CSS, print preview, and mobile behavior are preserved.
5. Uploaded store inventory remains private; if uploads are sent to a backend, document retention and deletion. Prefer local parsing for privacy if server processing is unnecessary.
6. Cache-control and CORS are reviewed, and the origin cannot be bypassed using an unprotected *.pages.dev address.
7. Production migration is gated on print-output comparisons and a rollback plan.

## Implementation sequence
- Record baseline screenshots, test Excel fixtures and printed PDFs for every layout.
- Provision a *separate* Cloudflare staging site, protected by Cloudflare Access (or an equivalent server-enforced identity layer).
- Move password validation away from browser code. Keep the existing UI behavior, but require verified server sessions.
- Extract only proprietary generation logic into Workers where it materially protects intellectual property; do not assume a static frontend can be kept secret.
- Validate employee access, file handling, 6-up, 4-up, A4 and A10 output on desktop and mobile.
- After verified staging cutover, retire the public Pages deployment, restrict repository visibility, review collaborators and rotate any exposed/reused credentials.

## Important
Do not publish credentials or secrets in this repository, including this staging branch. Making a repository private does not make a publicly hosted frontend private. No live access controls, production DNS, or repository visibility have been changed by this document.
