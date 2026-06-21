# Secure sign-in (OAuth 2.0 PKCE) — per-org setup

ForceVault can authenticate two ways:

1. **Session cookie (default).** Reads your active Salesforce `sid` cookie and uses
   it as a bearer token. Nothing to set up.
2. **Secure sign-in (OAuth).** You create an OAuth app **in your own org**, paste
   its **Consumer Key** into the extension's Settings, and sign in once. The
   extension then holds a proper, revocable **access + refresh token** instead of
   the session cookie.

The two coexist: if OAuth isn't connected for an org (or a refresh fails), the
extension falls back to the session cookie automatically.

> **Why per-org?** The app lives in the *same* org you sign into, so Salesforce's
> cross-org and "uninstalled connected app" restrictions never apply. Each user
> sets up their own org once.

> **Why it's more secure:** the token is scoped, independently revocable (Setup →
> Connected Apps OAuth Usage → Revoke), and bound to your app + redirect URL. The
> Consumer Key is a *public* client id — PKCE means there's no secret to steal.

---

## Step 1 — Get your Callback URL (from the extension)

1. Open a tab on the Salesforce org you want to secure, so ForceVault connects.
2. Side panel → **Settings → Secure sign-in (OAuth)**.
3. Copy the **Callback URL** shown there (it looks like
   `https://<extension-id>.chromiumapp.org/`). You'll register this in your app.

> This URL is specific to *your* installed extension. The unpacked/dev build and
> the Web Store build have different ones — always copy it from Settings.

---

## Step 2 — Create the OAuth app **in this org**

Either app type works because it's same-org. Pick whichever your org offers.

### Option A — External Client App (newer orgs)

External Client Apps (ECA) are the newer framework. Settings are split across two
places: **OAuth Settings** (at creation) and **Policies** (after creation) — don't
skip the Policies tab.

**A1. (If needed) allow ECA creation.**
If you don't see a **New External Client App** button, an admin must enable it:
**Setup → External Client App Settings → turn on "Allow creation of connected
apps."**

**A2. Create the app.**
1. **Setup** → Quick Find → **External Client App Manager** → **New External
   Client App**.
2. **Basic Information:**
   - External Client App Name: `ForceVault`
   - API Name: (auto-fills)
   - Contact Email: your email
   - **Distribution State: Local**

**A3. OAuth settings.**
3. Expand **API (Enable OAuth Settings)** → check **Enable OAuth**.
4. **Callback URL:** paste the URL from Step 1 (exactly, with the trailing `/`).
5. **Selected OAuth Scopes** — add both:
   - `Manage user data via APIs (api)`
   - `Perform requests at any time (refresh_token, offline_access)`
6. ✅ Check **Require Proof Key for Code Exchange (PKCE) Extension for Supported
   Authorization Flows**.
7. ❌ **Uncheck** **Require Secret for Web Server Flow**.
8. ❌ **Uncheck** **Require Secret for Refresh Token Flow** (if shown).
9. Click **Create**.

**A4. Policies (do not skip).**
10. Open the app → **Policies** tab → **Edit**.
11. Under **OAuth Policies**:
    - **Permitted Users:** *All users may self-authorize* (or leave default if it's
      just you as admin).
    - **Refresh Token Policy:** *Refresh token is valid until revoked* ← so the
      extension's auto-refresh keeps working.
12. **Save.**

> ⚠️ A **Local** External Client App only works for the org it was created in.
> That's exactly what we want here — just make sure you sign in to **that same
> org**. (Signing into a *different* org is what gives the "Cross-org OAuth flows
> are not supported" error.)

### Option B — Classic Connected App

1. If you don't see "New Connected App": **Setup → External Client App Settings →
   enable "Allow creation of connected apps."**
2. **Setup → App Manager → New Connected App.**
3. Enable OAuth, **Callback URL** = the URL from Step 1, same scopes as above.
4. ✅ Require PKCE.  ❌ Uncheck Require Secret for Web Server Flow.
5. **Save** (wait 2–10 min to propagate).
6. **Manage → Edit Policies → Permitted Users = "All users may self-authorize"**,
   Refresh Token Policy = *valid until revoked*. Save.

---

## Step 3 — Copy the Consumer Key

- **External Client App:** app → **Settings → OAuth Settings → Consumer Key and
  Secret** → copy the **Consumer Key**.
- **Classic Connected App:** **App Manager → View → Manage Consumer Details** →
  copy the **Consumer Key**.

You only need the **Key**, never the Secret.

---

## Step 4 — Connect in the extension

1. Back in **Settings → Secure sign-in (OAuth)**, paste the **Consumer Key** into
   the field.
2. Click **Connect securely** → approve in the Salesforce window.
3. The badge flips to **Secure (OAuth)**. Done — the key is saved for this org, so
   you won't re-enter it. The token auto-refreshes; **Disconnect** revokes it and
   returns to session-cookie auth.

Repeat Steps 1–4 in each org you want to secure (each org needs its own app +
key, stored separately).

---

## Troubleshooting

| Symptom | Fix |
| --- | --- |
| `OAUTH_AUTHORIZATION_BLOCKED: Cross-org OAuth flows…` | You signed into a different org than where the app lives. Sign into the **same** org, or use a classic Connected App with self-authorize. |
| `redirect_uri_mismatch` | Callback URL in the app must exactly equal the one from Settings (trailing slash, correct extension ID). |
| `invalid_client_id` / app not found | Wait a few minutes for propagation; re-copy the Consumer Key (no stray spaces). |
| "Authorization page could not be loaded" | Usually the callback isn't registered/propagated, or a secret is still required. Recheck Step 2.6. |
| Token exchange fails / `invalid_grant` | Ensure **Require Secret for Web Server Flow** is unchecked and **PKCE** is checked. |

---

**Default build ships no key**, so Web Store users keep session-cookie auth with
zero change. Secure sign-in activates only when a user pastes their org's key.
