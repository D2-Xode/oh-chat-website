# Olingo Chat website

The public website for **Olingo Chat**, the messenger that translates every message into each reader's language
(the app itself lives in `~/ai-server/chat-app`).

Plain HTML, CSS and JavaScript. There is no build step and nothing to install: any web server that serves
static files can host it.

## Files

| Path | What it is |
|---|---|
| `index.html` | The whole page: hero with live demo, how it works, translation, features, languages, security, get the app (main call to action), FAQ |
| `404.html` | Shown by GitHub Pages for any address that doesn't exist (with status 404, so search engines don't index it) |
| `assets/css/style.css` | All styles. Colours match the app (`chat-app/app/src/lib/theme.ts`); light and dark follow the visitor's system setting |
| `assets/js/main.js` | The hero demo (one group chat as each of its four members sees it), the menu button on phones, and the fade-in on scroll. The page works without it |
| `assets/fonts/` | Bricolage Grotesque, the heading font (Latin letters only; everything else uses the system font). Free to use, licence in `OFL.txt` |
| `assets/img/` | Logo and icons, copied from `chat-app/branding/logo-concepts/` and `chat-app/app/assets/`, plus `og-image.png` (link previews), `logo-512.png` (search engines) and `android-download-qr.svg` (the QR code to the APK, shown on computers) |
| `favicon.ico` | The 48 px icon, for browsers and tools that ask for `/favicon.ico` directly |
| `robots.txt`, `sitemap.xml` | Tell search engines they may index everything, and which page to index |
| `_config.yml` | Keeps this README off the public website (GitHub Pages publishes every file otherwise) |

## Call-to-action links

| Button | Goes to |
|---|---|
| Join Olingo Chat | https://app.olingo.chat/signup (needs an invite code while Google/Facebook/Apple sign-in is off) |
| Sign in / I already have an account | https://app.olingo.chat/login |
| Download for Android / Download the APK | https://github.com/D2-Xode/oh-chat-website/releases/latest/download/oh-chat.apk |
| iPhone | "Coming soon" (no link) |

The same list is in a comment at the top of `index.html`. To change a link, search `index.html` for the old address.

## Android download

The APK (about 56 MB) is too big for git, so it is hosted as a file on a
[GitHub Release](https://github.com/D2-Xode/oh-chat-website/releases), always named `oh-chat.apk`.
The source APKs are on the server in `~/ai-server/apk/`.

The download buttons use
`https://github.com/D2-Xode/oh-chat-website/releases/latest/download/oh-chat.apk`, which always points to
the newest release, so the links never need changing.

For a new app version:

1. Create a new release (for example tag `1.5.0.apk`) and upload the APK renamed to `oh-chat.apk`.
   Do **not** tick "Set as a pre-release": the `latest` link skips pre-releases.
2. In `index.html`, update "Version 1.4.0" in the "Get Olingo Chat" section.

The QR code (`assets/img/android-download-qr.svg`) holds the same `latest` address, so it never needs changing either.

## Search engines (SEO)

Already in place:

- A descriptive `<title>` and meta description, a canonical address (`https://olingo.chat/`) and
  `robots` meta that allows indexing.
- All text is plain HTML, so crawlers read everything without running JavaScript. The 40 languages are listed
  with their English names too (for searches like "Khmer chat translation").
- Structured data: the site name and logo (JSON-LD in the `<head>`), and the FAQ (schema.org `FAQPage`
  attributes on the questions themselves, so the visible text and the data can't drift apart).
- `robots.txt` and `sitemap.xml`, link previews (`og:` tags and `og-image.png`), and a real 404 page.

Still to do once, outside this repository (needs the Google account that manages `olingo.chat`):

1. In [Google Search Console](https://search.google.com/search-console), add the **domain property**
   `olingo.chat` (or the URL-prefix property `https://olingo.chat/`). Search Console shows a TXT record:
   add it to the domain's DNS, wait for it to work, then click Verify.
2. Under **Sitemaps**, submit `https://olingo.chat/sitemap.xml`.
3. Under **URL inspection**, enter the home page address and click **Request indexing**.
4. Optional: do the same in [Bing Webmaster Tools](https://www.bing.com/webmasters) (it can import from Search Console).

When you change the site:

- After editing `style.css` or `main.js`, raise the `?v=3` after their names in `index.html` and `404.html`
  (`?v=4`, ...), so visitors don't keep an old copy (GitHub Pages lets browsers cache files for 10 minutes).
- When the page's content changes, update `<lastmod>` in `sitemap.xml`.
- To add an FAQ question, copy a whole `<details>` block including its `itemprop`/`itemscope` attributes.
- `og-image.png` must stay 1200 × 630 px. Social sites cache previews: after replacing it, refresh them with
  Facebook's [Sharing Debugger](https://developers.facebook.com/tools/debug/).

## The Security section

Every claim in the Security section and the security answers in the FAQ matches the app's server code
(`chat-app/server/encryption.py`, `auth.py`, `admin.py` and the server README): messages, translations, files and
file names are encrypted at rest with AES-256-GCM, connections use HTTPS, and translation runs on Olingo Chat's own
server. It also says plainly that Olingo Chat is **not** end-to-end encrypted, and what isn't encrypted (names,
profile pictures, group names, who is in which chat, message times).

Keep it true when the app changes. In particular, if push notifications are added with message text in them
(`PUSH_SHOW_TEXT`), that text passes through Expo, Google and Apple, so update the section and the FAQ.

## Preview on your laptop

On the server:

```bash
cd ~/ai-server/oh-chat-website
python3 -m http.server 8090 --bind 127.0.0.1
```

`--bind 127.0.0.1` keeps the preview private to the server, so no firewall change is needed. VS Code
(Remote-SSH) offers to forward port 8090; accept, or add it in the **Ports** tab. Then open
http://localhost:8090 on your laptop. Press Ctrl+C in the terminal to stop the preview.

## Not done yet

- A privacy policy page (the Google Play Store and Facebook Login both require one) and terms of service.
  When they exist, link them in the footer and add them to `sitemap.xml`.
- Google Search Console verification and sitemap submission (see "Search engines" above).
