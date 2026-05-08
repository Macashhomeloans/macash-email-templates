# MacAsh Email Template HTML Hosting

This directory contains standalone HTML files served via GitHub Pages, used as `templateDataUrl` source for GHL email templates.

## How GHL uses these

`POST /emails/builder` with `{type: "html", templateDataUrl: "<this raw URL>"}` — GHL fetches the URL during template creation and uses the HTML as the email body. (Body content cannot be edited via PATCH after creation — confirmed via 11 field-name + 10 endpoint-path probes 2026-05-08.)

## Files

| File | GHL Template Name | When fired |
|---|---|---|
| `01_booking_confirmation.html` | `[DRAFT] Confirmation - Appointment Booked` | Immediately after Sarah books a consult |
| `03_post_consult_thank_you.html` | `[DRAFT] Follow-Up - Post-Consult` | 4 hours after consultation marked complete |

## Tokens

Files contain GHL Liquid-style tokens like `{{contact.first_name}}` and `{{appointment.start_date}}`. These are interpolated by GHL at send time — they are NOT processed by GitHub Pages. The raw HTML lives unchanged on GitHub Pages; GHL substitutes tokens during email send.

## Updating

To update an email body:
1. Edit the HTML file in this repo
2. Commit + push to `main`
3. Wait ~1 minute for GitHub Pages cache to invalidate
4. Either:
   - **(Existing template still works as-is)** GHL caches the HTML at template creation time. If your edit needs to apply to existing templates, re-create the template (archive old, POST new with same name + updated templateDataUrl).
   - GHL may not re-fetch templateDataUrl on subsequent sends (TBD — needs probe). If they do re-fetch, edits propagate automatically.

## Compliance

Each template includes the required disclosures in the footer:
- Kevin Duffy NMLS 1045534
- Company NMLS 2099559
- Physical address
- Equal Housing Lender
- IDFPR (Illinois) licensed
