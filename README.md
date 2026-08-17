# Stake Code Drop — Chrome Extension

> Automate Stake bonus code redemptions the millisecond they drop. No credentials or session cookies required.

[![Requirements](https://img.shields.io/badge/Browser-Chrome%20%2F%20Chromium-brightgreen)](#prerequisites)
[![License](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![Telegram](https://img.shields.io/badge/Telegram-@StakeCodeDrop-blue?logo=telegram)](https://t.me/stakecodedropcom)

---

## Overview

Bonus drops from Stake channels disappear in seconds. **Stake Code Drop** is a lightweight Chromium extension that monitors drop feeds and automatically triggers the Stake bonus redemption modal the moment a code goes live.

### Key Features
* **Zero Credentials Required:** Never asks for your Stake username, password, API key, or session cookies.
* **Instant Execution:** Opens the exact claim URL directly to minimize delay against redemption caps.
* **Customizable Channels:** Filter by **Daily Drops**, **High Roller Codes**, or **Saturday Stream** codes.
* **Safe UI Automation:** Functions strictly as a browser helper without performing dangerous account actions.

---

## How It Works

1. The extension listens for incoming codes detected via the backend feed or Telegram drop monitoring.
2. Once a code is captured, it opens the direct redemption link on Stake:
   `https://stake.com/settings/offers?type=drop&code=YOUR_CODE`
3. You claim the bonus directly through your active Stake session.

---

## Installation Guide (Developer / Unpacked)

Since this extension runs locally to ensure speed and transparency, you can load it directly into any Chromium browser:

1. **Clone or Download** this repository:
   ```bash
   git clone [https://github.com/your-username/stake-code-drop.git](https://github.com/your-username/stake-code-drop.git)
