<p align="center">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="https://stakecodedrop.com/assets/logo-light.png">
    <img src="https://stakecodedrop.com/assets/logo-dark.png" alt="Stake Code Drop" width="420">
  </picture>
</p>

<p align="center">
  <b>Opens Stake bonus drop codes the moment they land.</b><br>
  Free, open source, and it never touches the page you're on.
</p>

<p align="center">
  <a href="https://stakecodedrop.com/download.php"><b>Download</b></a> ·
  <a href="https://stakecodedrop.com/settings.php">Settings guide</a> ·
  <a href="https://stakecodedrop.com">Live code feed</a> ·
  <a href="https://t.me/stakecodedropcom">Telegram</a>
</p>

---

## What it does

Stake posts bonus drop codes with no warning and they're gone in minutes. This
Chrome extension watches the drop feed and opens the redeem page the second a
code appears.

It listens on a live push connection rather than checking every few seconds, so
there's no polling interval to lose. Latency is one network hop.

<p align="center">
  <img src="https://stakecodedrop.com/assets/extension-image.png" alt="The Stake Code Drop popup" width="360">
</p>

## Install

Not on the Chrome Web Store yet, so it loads unpacked:

1. [Download the .zip](https://stakecodedrop.com/download.php) and unzip it
2. Go to `chrome://extensions`
3. Turn on **Developer mode** (top right)
4. Click **Load unpacked** and select the unzipped folder

<p align="center">
  <img src="https://stakecodedrop.com/assets/instruct.webp" alt="Chrome extensions page with Developer mode enabled" width="640">
</p>

Works on Chrome, Edge, Brave, Opera and Vivaldi. Desktop only — mobile Chrome
can't run extensions.

## Settings

Everything saves the moment you click it. There's no save button.

| Setting | What it does |
| --- | --- |
| **Which site** | Stake.com or Stake.us. One at a time — a `.com` code won't redeem on `.us` |
| **Domain** | Which Stake mirror to open. Pick from the list or type your own |
| **Currency** | Which wallet the bonus lands in — LTC, BTC, USDT, Sweeps, Gold |
| **Catch these** | Daily Drop, High Roller, Other. Each on or off |
| **Background tab** | Open the code quietly instead of jumping straight to it |
| **Desktop notification** | Alert with the code, its value and the wager |
| **Ignore codes older than** | Stops a reconnect dumping dead codes into your tabs |
| **Bucket rules** | Which dollar amounts count as Daily Drop vs High Roller |

<p align="center">
  <img src="https://stakecodedrop.com/assets/ext-settings.png" alt="The settings page" width="440">
</p>

### Drop types

| | Type | Values |
| :-: | --- | --- |
| <img src="https://stakecodedrop.com/assets/ddicon.png" width="30"> | **Daily Drop** | $1, $3, $5 |
| <img src="https://stakecodedrop.com/assets/hricon.png" width="30"> | **High Roller** | $12.50, $25, $50 |
| <img src="https://stakecodedrop.com/assets/oticon.png" width="30"> | **Other** | Stream codes and anything with no value attached |

The feed sends a dollar amount but no category, so the type is worked out from
the amount. Both lists are editable in settings if the tiers ever change.

## How it works

```
Firebase drop feed ──SSE──▶ background.js ──▶ chrome.tabs.create(redeem URL)
```

The service worker holds an open Server-Sent Events connection to the public
Firebase realtime feed. When a code is pushed the extension already has it — it
builds the redeem URL from your settings and opens a tab.

MV3 service workers get shut down when idle, so two things keep it alive: the
streaming fetch is ongoing network activity that resets the idle timer, and a
30-second watchdog alarm reconnects anything stale. Worst case after a hard
shutdown, it's back within 30 seconds.

On first run it takes a snapshot of what's already in the feed and ignores all
of it, so installing doesn't open forty dead tabs.

## Privacy

Chrome will tell you this extension **doesn't need permission to read or change
the sites you visit**. That's accurate — it has no content scripts and no host
permissions for Stake. It opens a tab, nothing more.

- No login details, cookies or session tokens are read
- No account, no signup, no API key
- The only host it connects to is the public Firebase feed

It does send one anonymous heartbeat every 15 minutes — a random ID and the
version number — so the site can show how many installs are live. There's no
account behind that ID and no way back to a person. It's in
[`background.js`](background.js) if you want to read it or strip it out.

## Repo layout

```
manifest.json      MV3 manifest
background.js      service worker — the SSE connection and tab opening
common.js          shared config, bucket rules, URL builder
popup.html/js      toolbar popup
options.html/js    settings page
theme.css          shared styling
drop_relay.py      server-side relay (see below)
```

### drop_relay.py

Separate from the extension. It watches the same feed and posts new codes to a
webhook, which is what fills the code list on stakecodedrop.com and the Telegram
and Facebook channels. The extension doesn't need it and never talks to it.

```bash
python3 drop_relay.py stakecom
python3 drop_relay.py stakeus
```

Stdlib only, no dependencies. One process per feed, each with its own state file.

## Support

It's free and it stays free. If it's landed you a few codes,
[a coffee](https://buymeacoffee.com/rabbitclard) is always welcome.

## Disclaimer

Not affiliated with, endorsed by, or connected to Stake in any way. Gamble
responsibly.
