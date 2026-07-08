# CLIR Analyzer

Paste a PBXware / Asterisk AGI call trace (CLIR) and get an instant, structured
call analysis — caller identity, dialed number classification, timing and ring/talk
breakdown, trunk and routing, recording status, feature flags, dial-option decoding,
and concurrency limits.

**Privacy:** the tool is a single static HTML file. Parsing happens entirely in the
browser with plain JavaScript — no backend, no analytics, no network requests. Call
data (names, phone numbers) never leaves the analyst's machine. It also works fully
offline: just open `index.html`.

## What it extracts

The tool auto-detects call **direction**. On outbound calls the caller is shown as an
internal extension; on inbound calls the caller is the external number (never labeled
"Ext"), and the DID reached, presented CNAM name, and RDNIS are surfaced instead.

| Category | Fields |
| --- | --- |
| Direction | Inbound vs outbound detection; caller rendered as external number or extension accordingly |
| Identity | Caller ID name/number, extension or external number, outbound CID rewrite, ANI, presentation flag, RDNIS |
| Call | Dialed number + classification (toll-free / LD / international / emergency), dial & end timestamps, talk time, computed ring time, DIALSTATUS |
| Routing | Route name/ID, primary/secondary/tertiary trunks, LCR, operation times, DID DB match, absolute timeout, PBX loops |
| Recording | Active vs not started, on-demand armed + format, announcement prompts, predicted spool path |
| Features | Call forwarding, CF-on-unavailable, transfer detection (attended/blind + transferer), gloCOM / dialer / email2fax / hot-desking flags, fax detection, audio/video media |
| Network | Source IP with LAN vs public (remote worker) detection, SIP call ID, channel, unique ID, tenant account |
| Capacity | Every concurrency limit line with usage bars; exceeded limits flagged |
| Billing | Bill amount, rating duration |

Fields the trace genuinely does not contain (who hung up, DND, phone UAD type) are
reported honestly as "not in this log" instead of guessed.

## Run it

Open `index.html` in any modern browser. That's it.

## Host it with GitHub Pages

1. Create a repository (e.g. `clir-analyzer`) and push these files:
   ```bash
   git init
   git add index.html README.md
   git commit -m "CLIR analyzer v1"
   git branch -M main
   git remote add origin git@github.com:YOUR-ORG/clir-analyzer.git
   git push -u origin main
   ```
2. In the repo: **Settings → Pages → Source: Deploy from a branch → main / (root) → Save**.
3. The tool goes live at `https://YOUR-ORG.github.io/clir-analyzer/` within a minute or two.

> For a **private** repo, GitHub Pages sites are still publicly reachable by URL on
> Free plans (access-restricted Pages requires GitHub Enterprise Cloud). If your team
> needs the tool truly internal, host `index.html` on an internal web server, a shared
> drive, or just commit it to a private repo and have analysts open the file locally —
> it needs no server.

## Extending

All parsing lives in the `parse()` function in `index.html` — one regex per field.
To support additional trace types (inbound calls, queue legs, transfers from other
PBX platforms), add patterns there and new `row()` entries in `render()`.

## Copy summary

After analyzing, the **Copy summary** button puts a compact plain-text digest on the
clipboard — handy for pasting into tickets or chat.
