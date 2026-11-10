# Oh Chat website

The public website for **Oh Chat**, the messenger that translates every message into each reader's language
(the app itself lives in `~/ai-server/chat-app`).

Plain HTML, CSS and JavaScript. There is no build step and nothing to install: any web server that serves
static files can host it.

## Files

| Path | What it is |
|---|---|
| `index.html` | The whole page: hero with live demo, how it works, translation, features, languages, privacy, get the app (main call to action), FAQ |
| `assets/css/style.css` | All styles. Colours match the app (`chat-app/app/src/lib/theme.ts`); light and dark follow the visitor's system setting |
| `assets/js/main.js` | The hero demo: one group chat shown as each of its four members sees it (English, Korean, Japanese, Spanish) |
| `assets/img/` | Logo and icons, copied from `chat-app/branding/logo-concepts/` and `chat-app/app/assets/` |

## Call-to-action links

| Button | Goes to |
|---|---|
| Join Oh Chat | https://chat.rocksontech.com/signup (needs an invite code while Google/Facebook/Apple sign-in is off) |
| Sign in / I already have an account | https://chat.rocksontech.com/login |
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
2. In `index.html`, update "Version 1.4.0" in the "Get Oh Chat" section.

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

- Hosting and a domain for the website.
- A privacy policy page (the Google Play Store and Facebook Login both require one) and terms of service.
- A link-preview image (`og:image`, needs the final domain because it must be a full URL).
