# Publishing safely

This is a public repository. Sanitize **before staging or committing**, never after publishing. Keep working originals and rollback backups outside this checkout.

## Before the first commit of source material

1. Work from a separate sanitized copy. Read the complete file, including comments, includes, generated SAVE_CONFIG blocks, metadata and referenced files.
2. Remove credentials, passwords, keys, tokens, account links, service identifiers and session material. Replace network addresses/hostnames, device identifiers, private filesystem paths, usernames and personal information with descriptive placeholders. Review IPv6, URLs and encoded/embedded content as well as obvious IPv4 addresses.
3. Remove logs, diagnostic dumps, database exports, camera media, slicer job names and metadata that reveal private projects. Do not assume a configuration file is safe because it has no password field.
4. Record source date, sanitization and test status without exposing private paths or job names. A sanitized pin/config example is **draft**, not a backup that can be restored blindly.
5. Inspect `git status --short`, `git diff --cached --name-only` and the entire `git diff --cached`. Check every intended path; ignored files can still be force-added or already tracked.
6. Scan the staged content using a maintained secret scanner such as [Gitleaks](https://github.com/gitleaks/gitleaks), with redaction enabled. Use its documented staged/pre-commit mode for the installed version. Run a full-history scan before the initial push and on later release candidates. Resolve findings; do not add broad allowlists to silence them.
7. Review the scan results and diff manually for privacy. Secret scanners do not reliably catch local IPs, hostnames, personal data or every private path.

## Pre-commit and repository protection

Recommended, not yet installed/enforced: configure the scanner's official pre-commit integration in each developer's checkout; pin and review its version and dependencies, fail commits on detected secrets, and test it with a synthetic fixture outside publication history. A local hook can be bypassed and does not automatically run for other contributors. Document the installed version and setup when introducing executable tooling.

Enable GitHub secret scanning and push protection where available in repository settings; review branch protection/rulesets and required review for `main`. These account/repository settings have not been changed by the documentation bootstrap. Any future CI action should be reviewed and pinned to an immutable commit, with minimal permissions and no printer connectivity.

`.gitignore` is a guardrail, not a security boundary. This contribution intentionally supplies guidance rather than pretending a scanner or hook has been installed. Future enforcement should have its own tested change.

If a secret reaches Git history: stop publication, revoke/rotate the credential, notify the owner, and agree a history-remediation plan. Deleting the visible file is insufficient. History rewriting is a separate, disruptive operation requiring approval.
