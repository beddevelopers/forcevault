# ForceVault for Salesforce — Installation Guide

## Loading in Chrome

1. Open Chrome and navigate to `chrome://extensions`
2. Enable **Developer mode** (toggle in top-right corner)
3. Click **Load unpacked**
4. Select the `sf-explorer-extension` folder
5. The **ForceVault** icon (FV) appears in your toolbar

## Usage

1. Log in to any Salesforce org in Chrome (`*.salesforce.com` / `*.force.com`)
2. Click the **ForceVault** icon — the side panel opens automatically
3. The extension auto-detects your session from the active Salesforce tab
4. If not connected, click **Detect Salesforce** in the panel
5. ForceVault only shows content while you're on **your connected org's** tab
   (Lightning / `my.salesforce.com` / `my.salesforce-setup.com`)

## Navigation

Open the **☰ menu** (top-left) to switch between sections:

| Group | Item | What it does |
|---|---|---|
| Explore | **Objects & Fields** | All SObjects → drill into fields, create fields, "Where used" |
| Security | **Profiles** | Object/field permissions, Apex/VF access, system perms |
| Security | **Permission Sets** | Same detail view for permission sets |
| Security | **Users** | Search users, manage permission-set / group assignments, license & access info, audit trail, login-as |
| Data | **Current Record** | Auto-detects the open record → all field values, inline edit |
| Data | **SOQL Query** | Run SOQL/SOSL, grid results, CSV export |
| Tools | **Log Analyzer** | Paste an Apex debug log → execution timeline, method counts, governor limits (offline) |
| Tools | **Audit Trail** | Org-wide setup changes with user/date filters + CSV export |
| Org | **Org Info** | Org details, user-license usage, current user, connection |

### Highlights
- **Where used** — find every component (Apex, Flows, layouts, LWC/Aura, profiles/perm sets) that references a field or object
- **Create Custom Field** — 20+ types, with Field-Level Security assignment
- Redirect (↗) icons jump straight to the matching Salesforce Setup page
- Multi-org aware — switches automatically as you change org tabs

## Permissions required

- `cookies` — reads the Salesforce `sid` session cookie
- `storage` — caches the session between panel opens
- `sidePanel` — opens as a side panel
- `tabs` — detects the active Salesforce tab for auto-connect
- Host permissions for `*.salesforce.com`, `*.force.com` — REST/Tooling API calls

No data leaves your browser except API calls to your own Salesforce org. The Log
Analyzer is fully offline.

## Salesforce CORS

API calls go directly from the panel to your org. Chrome grants extensions CORS
bypass for domains in `host_permissions`, so no Salesforce CORS setup is needed.

## Supported Org Types

- Developer Edition / Trailhead Playgrounds
- Sandbox (`*.sandbox.my.salesforce.com`)
- Production orgs (`*.my.salesforce.com`)
- Classic and Lightning Experience
