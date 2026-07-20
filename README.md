<h1 align="center">RecencyPulse</h1>

<p align="center">
  <strong>Search what people actually said this month.</strong><br>
  A free, open-source Chrome extension that jumps straight to recent Reddit and TikTok discussion.
</p>

<p align="center">
  <a href="https://wushu75.github.io/RecencyPulse/">Website</a> ·
  <a href="https://chromewebstore.google.com/detail/recencypulse-social-searc/ojoeceihhidhabomofgddlbhpmioldeb">Install</a> ·
  <a href="#how-it-works">How it works</a> ·
  <a href="#privacy">Privacy</a>
</p>

<p align="center">
  <img alt="Manifest V3" src="https://img.shields.io/badge/Manifest-V3-1877f2">
  <img alt="Permissions: none" src="https://img.shields.io/badge/permissions-none-ff4500">
  <img alt="License: MIT" src="https://img.shields.io/badge/license-MIT-14131a">
  <a href="https://chromewebstore.google.com/detail/recencypulse-social-searc/ojoeceihhidhabomofgddlbhpmioldeb"><img alt="Chrome Web Store" src="https://img.shields.io/badge/Chrome%20Web%20Store-live-34a853"></a>
</p>

---

## What it is

Search results have filled up with pages written to rank well rather than to be read. When you want a real opinion — is this thing worth buying, is this neighbourhood any good, did anyone else hit this bug — you end up appending `reddit` to every search out of habit.

RecencyPulse turns that habit into one click. Click the icon, type a topic, pick a platform. It opens the search you actually wanted, already filtered for recent discussion.

## Why the time limit matters

An old thread can rank at the top for years. If you're asking about a product, a policy, a version, or a place, a four-year-old answer is often just wrong.

Reddit searches are pinned to the **past month** by default — the filter most people want but rarely bother to set.

## Install

**From the Chrome Web Store**

> ### [→ Add RecencyPulse to Chrome](https://chromewebstore.google.com/detail/recencypulse-social-searc/ojoeceihhidhabomofgddlbhpmioldeb)

Or search "RecencyPulse" in the Chrome Web Store.

**From source**

```bash
git clone https://github.com/wushu75/RecencyPulse.git
```

1. Open `chrome://extensions`
2. Turn on **Developer mode** (top right)
3. Click **Load unpacked**
4. Select the `recency-pulse` folder

Works in Chrome, Edge, Brave, and Arc.

## How it works

There's no backend, no API, and no scraping. The extension builds a search URL and opens it in a new tab. That's the entire mechanism.

| Platform | URL it opens |
|---|---|
| Reddit | `reddit.com/search/?q=QUERY&sort=relevance&t=month` |
| TikTok | `tiktok.com/search?q=QUERY` |

Your input is trimmed and passed through `encodeURIComponent`, then handed to `chrome.tabs.create()`.

**Using it**

- Click a platform button to select it, click again to search
- Or press <kbd>Enter</kbd> to search whichever platform is selected
- The selected platform shows a brand-coloured stroke — orange for Reddit, black for TikTok

## Privacy

RecencyPulse **requests no permissions at all**. When you install it, Chrome has nothing to warn you about, because there's nothing to approve.

- No data collected. Search terms go from the box to the search page and nowhere else.
- No page access. It can't read your history, your tabs, or the sites you visit.
- No analytics, no tracking, no ads, no accounts.
- No network requests of its own.

`chrome.tabs.create()` needs no permission when opening a normal URL, which is why the manifest has no `permissions` key. If you ever see a version of this extension asking for permissions, it isn't this one.

## Project structure

```
recency-pulse/
├── manifest.json     Manifest V3 config, no permissions
├── popup.html        300px popup UI and styles
├── popup.js          Query building and tab routing
├── icon16.png
├── icon48.png
└── icon128.png
```

Everything is vanilla HTML, CSS, and JavaScript. No framework, no build step, no dependencies — clone it and it runs.

## Adding a platform

Add one line to the `PLATFORMS` map in `popup.js`:

```js
const PLATFORMS = {
  reddit:   q => `https://www.reddit.com/search/?q=${q}&sort=relevance&t=month`,
  tiktok:   q => `https://www.tiktok.com/search?q=${q}`,
  yourSite: q => `https://example.com/search?q=${q}`,
};
```

Then add a matching button in `popup.html`:

```html
<button class="search-btn yourSite" data-platform="yourSite">Search Example</button>
```

The `data-platform` value must match the key in `PLATFORMS`. Add a background colour and a `.yourSite.selected { outline-color: ... }` rule to the stylesheet and you're done — routing and selection wire up automatically.

## A note on Facebook

Earlier versions included a Facebook button. It was removed: Facebook returns *Not Found* for direct search URLs unless you're logged in, and has been progressively retiring the `/search/posts` path. Rather than ship a button that fails for most people, it's gone. Reddit works logged-out; TikTok mostly does.

## Contributing

Issues and pull requests are welcome. Useful things to work on:

- Additional platforms with stable, public search URLs
- Configurable time ranges for Reddit (week / month / year)
- Remembering the last selected platform between popup sessions
- Keyboard shortcut to open the popup

Please keep it dependency-free and permission-free — that's the point of the project.

## License

MIT. See [LICENSE](LICENSE).

---

<sub>RecencyPulse is an independent project and is not affiliated with, endorsed by, or sponsored by Reddit or TikTok. It opens their public search pages and nothing more.</sub>
