# Security Notes — Sheet Manager

This is a **client-side (browser-only) web app** that uses the Google Identity
Services token flow to talk to the Google Sheets and Drive APIs. There is no
backend server.

## Where the app runs

- **Live URL:** https://anshubhaisarehabuild-spec.github.io/sheet-manager/sheet-manager.html
- Hosted on **GitHub Pages** from the `main` branch of this repo.

## The Client ID is public — and that's fine

The Google OAuth **Client ID** in `sheet-manager.html` is *meant* to be public.
Every browser-based Google app ships its Client ID in plain JavaScript. Hiding
or obfuscating it adds no security.

The real protection comes from the **Authorized JavaScript origins** configured
in the Google Cloud Console — these restrict which website the Client ID is
allowed to be used from.

## Google Cloud Console configuration

Credentials page: https://console.cloud.google.com/apis/credentials
(OAuth 2.0 Client ID — "Sheet Manager")

### Authorized JavaScript origins
The only origin that matters for production:

```
https://anshubhaisarehabuild-spec.github.io
```

`http://localhost:<port>` entries may also be present for local development.
These are safe — `localhost` only ever refers to the developer's own machine and
cannot be abused remotely.

> ⚠️ Origins must be the bare scheme + domain — **no path, no trailing slash.**

### OAuth scopes (minimal, intentional)
- `spreadsheets` — read/write the user's sheets (core feature)
- `drive.metadata.readonly` — read-only Drive metadata for import (not full Drive)
- `userinfo.email`, `userinfo.profile` — basic sign-in identity

### Consent screen
- **Testing** mode = only listed test users can sign in (strongest lock).
- **Production** mode = any Google account can sign in; access is then gated by
  the in-app allow/deny team-access control.

## Rules — DO NOT break these

1. **Never** commit a `client_secret` to this repo or paste it into
   `sheet-manager.html`. This app does **not** need one. (A secret exists in the
   Cloud Console but is unused and must stay only there.)
2. **Never** add an API key or any other credential to the HTML.
3. Keep the Authorized JavaScript origins list tight — only real domains you
   control. No wildcards.

## Verified state (as of last review)

- ✅ No `client_secret` in the code
- ✅ No API key in the code
- ✅ Only the public Client ID is present (expected)
- ✅ Production origin correctly whitelisted
