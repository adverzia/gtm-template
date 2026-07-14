# Adverzia GTM Community Template

A Google Tag Manager [Community Template Gallery](https://support.google.com/tagmanager/answer/9454109)
template that lets merchants fire Adverzia affiliate/referral tracking events
without editing site HTML — configured entirely as GTM tags + triggers.

It wraps the same `advz()` queue API the [tracker](../../tracker) snippet
exposes: `advz("init", campaignId)`, `advz("event", "pageload")`,
`advz("event", "conversion", {...})`, `advz("event", "customer", {...})`, and
arbitrary custom events.

## Track Types

One tag template, four modes selected via the **Track Type** field:

| Track Type | Fires | When to use |
|---|---|---|
| Page View | `advz("init", campaignId)` + `advz("event", "pageload")` | Once per page, on **All Pages** |
| Conversion | `advz("event", "conversion", ed)` | On a conversion trigger (e.g. Thank You page, purchase event) |
| Customer | `advz("event", "customer", ed)` | When a visitor becomes a registered customer |
| Custom Event | `advz("event", <name>, ed)` | Any other interaction to log against the click |

`ed` (event data) is built from the **Event Data** key/value table on the tag.

## Fields

- **Track Type** — see table above.
- **Campaign / Referral Program ID** — required for Page View. Found in
  Campaign → Integration tab in the Adverzia dashboard.
- **Event Name** — required for Custom Event.
- **Event Data** — key/value table, used by Conversion / Customer / Custom Event.
- **Advanced Settings**
  - **Tracker Script URL** — defaults to `https://advz.it/pixel.js`. Only
    change this for self-hosted or white-labeled tracker domains — and update
    the `inject_script` permission in `template.tpl` to match, or the browser
    will block the request.
  - **Cookie Domain** — optional override so the referral cookie is shared
    across subdomains.
  - **Log to console in Preview mode** — debug toggle, only logs in GTM's
    debug/preview environment (`logging` permission is scoped to `debug`).

## Local development

1. Open Google Tag Manager → any container → **Templates** → **New** → the
   overflow menu (⋮) → **Import**, and pick `template.tpl` to load it into
   the Template Editor for live editing, or paste its sections directly.
2. Edit fields / code in the Template Editor UI, which keeps
   `___TEMPLATE_PARAMETERS___` and `___WEB_PERMISSIONS___` in sync — hand
   edits to the permissions block should always be re-verified there before
   submission, since the Editor generates that JSON internally.
3. Use the Template Editor's **Tests** tab to run the scenarios in
   `___TESTS___`; see [Test and write templates](https://developers.google.com/tag-platform/tag-manager/templates/test-write)
   for the mocking API (`mock`, `runCode`, `assertThat`, `assertApi`).
4. Export back to `template.tpl` (⋮ → Export) after each change so the repo
   stays the source of truth.

## Before submitting to the gallery

- [x] `brand.thumbnail` in `___INFO___` — base64-encoded from
      `website/public/favicon/favicon-96x96.png` (96×96, meets the gallery's
      square/48–96px/50KB icon spec). Swap for a dedicated icon if the
      favicon isn't the intended gallery mark.
- [ ] Confirm `___WEB_PERMISSIONS___` matches what the Template Editor's
      Permissions tab generates (hand-authored here from the public schema —
      not yet round-tripped through the Editor).
- [ ] Run all `___TESTS___` scenarios green in the Editor.
- [ ] Smoke test all four Track Types against a real campaign in GTM Preview
      mode, confirm `Click` / `Conversion` / `Commission` rows land correctly.
- [ ] Push this repo to its own public GitHub repo (root-level `template.tpl`,
      `metadata.yaml`, `LICENSE`, one `template.tpl` per repo, everything on
      `main`, Issues enabled — the gallery links directly to them).
- [ ] Set `versions[0].sha` in `metadata.yaml` to the commit being submitted.
- [ ] Submit via tagmanager.google.com/gallery → ⋮ → **Submit Template** →
      paste the repo URL.

See [Submit a template to the Community Template Gallery](https://developers.google.com/tag-platform/tag-manager/templates/gallery)
for the full process.
