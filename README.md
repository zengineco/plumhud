Plum HUD
Zengine™ | Vincent Gonzalez | v3.0.0 — 2026
Crypto miner fleet monitor — borderless overlay HUD + full command center.

What's new in v3.0.0
Async polling engine — all miners polled concurrently via aiohttp. 20 miners poll in 2.5s flat instead of 50s serial worst case. Sync ThreadPoolExecutor fallback if aiohttp not installed.
SQLite analytics DB — every poll result written to plumhud.db. 30-day retention with auto-prune. Enables history charts and alert delta detection.
Alert engine — configurable rules: miner offline, hash below threshold, temp above, reject spike. Per-rule cooldown. Discord webhook delivery. Telegram bot delivery.
4 HUD display modes — ring, bar, spark, grid. Double-click HUD to cycle.
Full opacity control — slider 0.1 (ghost) to 1.0 (fully opaque). Live-applies.
5 skin presets — Plum Dark, Plum Matte, Zengine Cyan, Carbon, Terminal Green.
Metrics toggles — show/hide hash, temp, shares, IP independently.
5-tab Command Center — Miners / Analytics / Alerts / Skin / Settings.
Parallel subnet scanner — 64 threads, 254 hosts in ~2 seconds, progress bar.
Discord + Telegram test buttons — verify webhooks before going live.
Queue-based thread safety — bg poll results via queue.Queue, drained by after(500). Zero Tk calls from bg threads.
Quick start
Install Python 3.10+: https://www.python.org/downloads/

Minimum install:

pip install requests Pillow matplotlib
Full async + tray:

pip install aiohttp requests Pillow matplotlib pystray
Place logo (optional):

assets/logo.png
Run:

python plumhud.py
CONFIG BLOCK
Constant	Default	Description
CONFIG_FILE	plumhud_config.json	Config path relative to CWD
DB_FILE	plumhud.db	SQLite DB path
LOGO_FILE	./assets/logo.png	Logo PNG
POLL_INTERVAL	5	Seconds between fleet polls
SCAN_TIMEOUT	1.0	TCP ping timeout per host
SCAN_THREADS	64	Parallel threads during subnet scan
HUD_W / HUD_H	400 / 440	HUD overlay size px
CMD_W / CMD_H	780 / 700	Command Center size px
DB_RETENTION_DAYS	30	Auto-prune cutoff
ALERT_COOLDOWN_SEC	120	Min seconds between repeat alerts
DEV_MODE	False	True = verbose console logging
HUD Display Modes
ring — pie-ring, one segment per miner, summary in center donut bar — horizontal hash-rate bars spark — mini sparklines from last 1h DB history grid — compact 4-column status tiles, up to 32 miners

Double-click the HUD to cycle. Or set from Skin tab.

Alert Rules
Types:

miner_offline — fires on transition to Offline
hash_below — fires when hash < threshold MH/s
temp_above — fires when temp > threshold C
reject_spike — fires when rejected shares delta >= threshold in one cycle
Add rules: Alerts tab -> set type + threshold -> tick Discord/Telegram -> Add Rule.

Discord: Server Settings -> Integrations -> Webhooks -> copy URL -> paste in Settings tab -> Test.

Telegram: BotFather -> /newbot -> copy token -> get chat ID from getUpdates -> paste both in Settings tab -> Test.

Miner API
Polls http://{ip}/api/system/info, reads:

initMiner -> alias
hashRate -> MH/s
temp -> temperature C
sharesAccepted / sharesRejected -> share counts
uptime -> uptime string
Missing keys fall back to random demo values for UI testing.

Dependencies
Package	Required	Purpose
tkinter	Yes (stdlib)	All UI
aiohttp	Recommended	Concurrent async polling
requests	Recommended	Webhooks + sync fallback
Pillow	Optional	Logo + tray icon
matplotlib	Optional	Analytics chart
pystray	Optional	System tray icon
Known Issues
Alert toast uses messagebox (blocking). Replace with plyer.notification for non-blocking toasts (flagged in code).
sound_accept.wav / sound_reject.wav listed in ASSET MANIFEST but playback not yet wired.
Spark mode shows flat line on first run before DB accumulates history.
macOS overrideredirect behavior may differ from Windows.
NEXT STEPS
Wire sound events via winsound (Windows) or playsound
Replace messagebox alert with plyer.notification
Per-miner detail popup on click
Profitability estimator (power draw x coin price)
Live coin price from CoinGecko API (no key needed)
Multi-API adapters: Antminer CGMiner port 4028, Whatsminer, Goldshell
PyInstaller single-exe + Inno Setup installer
Web dashboard on localhost:7777
Version History
v3.0.0 — 2026 — Async engine, SQLite, 4 HUD modes, alert engine, Discord/Telegram, skin presets, opacity control, 5-tab Command Center v2.0.0 — 2026 — Zengine standard rebuild, fixed math import, thread-safe UI refresh v1.0.0 — 2025 — ChatGPT-era original
