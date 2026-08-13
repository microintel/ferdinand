# Ferdinand

A single-page, browser-only tool for concealing text, images, audio, and video behind a password — using client-side AES-256-GCM encryption. Nothing you type or upload ever leaves your browser.

## Features

- **Conceal / Reveal** — encrypt any supported content with a password, decrypt it later with the same one.
- **Multi-format support** — plain text, images, audio, and video (up to 10MB per file).
- **Import from link** — pull text content in directly from a URL.
- **Share & download** — send concealed content through the OS share sheet or save it as a file.
- **Read aloud** — text-to-speech playback for revealed text; native playback controls for revealed audio/video.
- **Light / dark theme** — persisted across visits, defaults to dark.
- **Shareable links** — opening the app with `?text=...` in the URL pre-fills that content.
- **No backend** — all encryption, decryption, and file handling happens in-browser via the Web Crypto API.

## How it works

Ferdinand uses:

- **AES-256-GCM** for encryption (authenticated — tampering or a wrong password is detected, not silently decrypted into garbage).
- **PBKDF2** (SHA-256, 150,000 iterations) to turn your password into an actual encryption key.
- A fresh random **salt** (16 bytes) and **IV** (12 bytes) on every single encryption, so the same input never produces the same output twice.

A detailed, plain-language writeup of the whole pipeline — including how text, images, audio, and video are each handled — lives in [`cryptography.html`](./cryptography.html). Open it from the app itself via the **?** icon in the toolbar, or directly in a browser.

## Getting started

Ferdinand has no build step and no dependencies to install — it's a static HTML file.

1. Download or clone this repository.
2. Keep `index.html`, `cryptography.html`, `manifest.json`, and the image/GIF assets in the same folder.
3. Open `index.html` in a browser, or serve the folder with any static file server:

   ```bash
   npx serve .
   ```

## Usage

1. **Add content** — type/paste text into the document, or use the file picker to load a text, image, audio, or video file.
2. **Conceal** — press *Conceal* and enter a password. The password is never stored; there is no recovery option.
3. **Save or share** — the document now shows a block of ciphertext. Copy it, download it, or share it.
4. **Reveal** — paste a concealed block back in, press *Reveal*, and enter the same password.

## Project structure

```
.
├── index.html          the app
├── cryptography.html   companion page explaining the cryptography
├── manifest.json        web app manifest
├── ferdinand-favicon.png
├── ferdinand-logo.png   (fetched remotely in index.html)
└── venom-load.gif       loading indicator asset
```

## Tech stack

- Vanilla HTML/CSS/JS — no framework, no build tooling.
- [Web Crypto API](https://developer.mozilla.org/en-US/docs/Web/API/Web_Crypto_API) (`crypto.subtle`) for all encryption.
- [SweetAlert2](https://sweetalert2.github.io/) for dialogs and prompts.
- [particles.js](https://vincentgarreau.com/particles.js/) for the ambient background.
- [Bootstrap Icons](https://icons.getbootstrap.com/), Google Fonts (Source Serif 4, Inter, IBM Plex Mono).

## Security notes

- Encryption and decryption happen entirely client-side; content is never transmitted to a server.
- The password is the only key. If it's lost, concealed content is unrecoverable by design.
- MIME type and content type are stored inside the encrypted payload, so a concealed block reveals nothing about what it contains until the correct password is entered.

## Credits

Developed by **Microintel**.
