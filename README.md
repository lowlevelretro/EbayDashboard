# eBay Seller Dashboard

A simple, single-page dashboard for viewing your **active** and **sold** eBay
listings at a glance — counts, watchers, views, sold units, and listed value —
all in one self-contained HTML file. No build step, no server, no dependencies:
open the file in a browser and go.

> ⚠️ **Use at your own risk — provided as-is.** This is a personal hobby project
> with no warranty of any kind. It talks to eBay's API using credentials *you*
> supply and routes requests through a CORS proxy (see below). Review the code,
> understand what it does with your keys, and use it at your own discretion.
> Nothing here is affiliated with or endorsed by eBay.

## What it does

- Fetches your active and sold listings via the eBay **Trading API**
  (`GetMyeBaySelling`).
- Shows summary stats: active count, total watchers, views, units sold, and
  total listed value.
- Sortable tables for active and sold items.
- Stores your settings in the browser's `localStorage`, and can import/export
  them as a plain `key = value` `.conf` file.

It's a single file — [`eBayDashboard.html`](eBayDashboard.html). That's the
whole app.

## What you need to set it up

### 1. An eBay Developer account and API keys

Create a free account at [developer.ebay.com](https://developer.ebay.com), then
under **My Account** create a **Production** keyset. You'll need four values:

| Field | Where to find it |
|-------|------------------|
| **App ID** (Client ID) | Application Keys page |
| **Dev ID** | Application Keys page |
| **Cert ID** (Client Secret) | Application Keys page |
| **User Token** | My Account → **User Tokens** → *Get a Token for this Application* |

You'll also pick your **eBay site** (e.g. `2` = Canada, `0` = US, `3` = UK).

### 2. A CORS proxy (required)

eBay's API rejects direct requests from a browser (no CORS headers), so requests
must pass through a proxy that adds them. Two options:

- **Quick start:** [`corsproxy.io`](https://corsproxy.io) works out of the box —
  use `https://corsproxy.io/?` as the proxy URL. Note this sends your request
  (including your token) through a third-party service.
- **Recommended for real use:** run your own proxy so nothing transits a service
  you don't control — for example a small
  [Cloudflare Worker](https://developers.cloudflare.com/workers/) that forwards
  the request to `https://api.ebay.com/ws/api.dll`. Put its URL in the proxy
  field.

> 🔒 **Heads up on credentials:** your API keys and user token are stored in your
> browser's `localStorage` and are transmitted to eBay via whichever proxy you
> configure. Prefer your own proxy if you want end-to-end control, and never
> commit your real credentials anywhere.

## Usage

1. Open `eBayDashboard.html` in your browser.
2. Click **⚙ Settings** and enter your four API values, site, and proxy URL.
3. Click **Save & Close**, then load your listings.

### Saving / loading settings

- **Export to File** writes your settings to `ebay-settings.conf` (a plain,
  human-editable `key = value` text file).
- **Import from File** reads a `.conf` back in and applies it.

An example template is included as
[`ebay-settings.example.conf`](ebay-settings.example.conf) — copy it to
`ebay-settings.conf` and fill in your own values. Your real `ebay-settings.conf`
is git-ignored so it never gets committed.
