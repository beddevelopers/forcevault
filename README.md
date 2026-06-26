# ForceVault for Salesforce

**Version 1.0.0** · A Chrome side-panel toolkit for Salesforce admins and
developers. Explore your org's objects, fields, profiles, permission sets, users,
and metadata dependencies — using your existing Salesforce login session.

> Privacy-first: ForceVault runs entirely in your browser, connects only to the
> Salesforce org you're already logged into, and never sends your data to anyone.
> No analytics. No tracking. No remote code.

## Features

- **Objects & Fields** — every SObject and field with labels, API names, types.
- **Create Custom Field** — 20+ types with Field-Level Security assignment.
- **Where is this used?** — find every reference to a field/object across Apex,
  Flows, layouts, LWC/Aura, and profile/permission-set access.
- **Profiles & Permission Sets** — object/field permissions, Apex & Visualforce
  access, system & custom permissions.
- **Users** — search, manage permission-set/group assignments, access viewer,
  audit trail, login-as.
- **Compare Access** — diff two profiles or permission sets (system, object,
  per-object field, Apex, Visualforce & custom permissions) with a CSV report.
- **Current Record** — all field values with inline edit on the open record.
- **SOQL Query** — run SOQL/SOSL with grid results and CSV export, plus **Query
  Plan** (Salesforce Explain) to preview cost and table-scan warnings.
- **Execute Anonymous Apex** — editor with autocomplete, saved scripts, and
  debug-log capture that opens straight in the Log Analyzer.
- **Apex Tests & Coverage** — run any test class (or up to 5 in parallel) from a
  Test Classes tab; org-wide and full per-class/trigger coverage.
- **REST / Tooling Explorer** — send any REST/Tooling request and inspect the JSON.
- **Code Search** — "git grep" for your org: full-text search across every Apex
  class and trigger; indexes once, then searches instantly.
- **Background Jobs** — live dashboard of async Apex, scheduled jobs and paused
  flows, with a failed-only view.
- **Log Analyzer** — paste or load an Apex debug log for a full execution timeline,
  method counts, SOQL/DML/callouts, exceptions, and governor limits; scales to
  very large (10 MB+) logs.
- **Audit Trail** — org-wide setup changes with filters and CSV export.
- **Quick Links** — a launchpad of common Setup destinations you can extend.
- **Org Info** — org details, license usage, **governor-limits dashboard**, the
  org's current + next release, maintenance windows, and live API usage.
- **Secure sign-in (OAuth)** — optional per-org OAuth 2.0 + PKCE with the refresh
  token encrypted at rest (falls back to your session automatically).
- **Per-org settings** — choose the Salesforce API version, default landing page,
  and favicon color; a status bar shows the connected user.
- **Sortable tables** and **search filters** throughout, **multi-org aware**, and
  **Open in full tab**.

## Install

See [INSTALL.md](INSTALL.md) for loading the unpacked extension, or install from
the Chrome Web Store (link coming soon).

## What's new

See [CHANGELOG.md](CHANGELOG.md).

## Privacy

See the [Privacy Policy](https://beddevelopers.github.io/forcevault/privacy-policy.html).

## Supported orgs

Developer Edition, Trailhead Playgrounds, Sandboxes, and Production — Classic and
Lightning Experience.

## Contact

forcevaultsfdc@zohomail.in
