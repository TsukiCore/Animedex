# 🎌 AnimeDex

> *Infinite series. Zero mercy. Your soul was already ours.*

A fast, modern anime streaming site built with **Next.js 15**. Pulls metadata from AniList's GraphQL API and streams episodes through multiple fallback providers — no single point of failure.

![Next.js](https://img.shields.io/badge/Next.js-15-black?style=flat-square&logo=next.js)
![React](https://img.shields.io/badge/React-19-61DAFB?style=flat-square&logo=react)
![AniList](https://img.shields.io/badge/AniList-GraphQL-02A9FF?style=flat-square)
![Version](https://img.shields.io/badge/version-v4.0-c0394d?style=flat-square)
![License](https://img.shields.io/badge/license-MIT-green?style=flat-square)


## ✨ Features

| Category | Feature |
|---|---|
| **Discovery** | Spotlight banner with TVDB fanart, trending, popular, seasonal browse, genre filter, "Picked For You" AI recommendations |
| **Search** | Instant search with grouped results (Series / Movies / Specials) |
| **Player** | ArtPlayer with Vidstack media icons · HLS via hls.js · ±10s seek · auto-skip OP/ED · progress bar chapter markers |
| **Multi-Audio** | Animelok multi-audio support (JPN/ENG/HIN/TEL/TAM) with audio track switcher |
| **AniSkip** | Intro & outro skip buttons + auto-skip toggle + progress bar markers |
| **Quality** | Auto 720p default · manual quality selector |
| **Subtitles** | VTT/SRT tracks · SRT→VTT conversion · Multi-language subtitle search (OpenSubtitles) |
| **Continue Watching** | Saves position every 5s · resumes on return |
| **Dub/Sub** | Per-source sub/dub switching with episode reload |
| **Sources** | Kiwi (default), AnimePahe, AnimeGG, Animelok (multi-audio), Miruro, VidNest embeds |
| **Download** | One-click episode download via Kiwi/AnimePahe direct MP4 links |
| **Clip & Share** | Create 5-30s clips with watermark · shareable links · canvas-based download with branding |
| **Soundtrack** | Mini music player (🎵) with OP/ED themes from AnimeThemes.moe |
| **Episode Metadata** | TVDB thumbnails, multilingual titles, descriptions, ratings, air dates (via ani.zip) |
| **Color Themes** | Anime-specific accent colors extracted from AniList cover art |
| **Schedule** | Cosmic-inspired date slider + alternating timeline · full month view |
| **News** | ANN RSS feed · anime & manga tabs · tag filter · article modal |
| **Watch Party** | Room system · BroadcastChannel sync · live chat · voice toggle |
| **Discord Alerts** | Automated webhooks: new episodes, premieres, countdowns, daily schedule, trending, news |
| **Online Users** | Live counter on landing page & footer |
| **Auth** | AniList OAuth 2.0 (Authorization Code) · httpOnly cookie |
| **Profile** | 8 tabs: Overview, Watching, Completed, Planned, Manga, Favourites, Activity, History |
| **Watchlist** | localStorage + AniList sync on login |
| **Comments** | TheAnimeCommunity iframe embed (load-on-demand) |
| **Watermark** | AnimeDex branding on player (visible in fullscreen, screenshots, screen recordings) |
| **DevTools Guard** | Blocks F12, Ctrl+Shift+I, right-click (deterrent for casual users) |
| **Cache** | Upstash Redis → Turso SQLite → in-memory (three-layer) |
| **Proxy** | Server-side HLS proxy · M3U8 rewriting · Range header support · multi-CDN referer handling |
| **Community** | Community Trending rankings · "Picked For You" personalized recommendations |


## 🛠 Tech Stack

| Layer | Technology |
|---|---|
| Framework | Next.js 15.5 (App Router) |
| UI | React 19, CSS Modules, Framer Motion |
| Metadata API | AniList GraphQL + ani.zip (TVDB) |
| Streaming | Kiwi/miruro-api-jade (primary) + Crysoline + Animelok + VidNest |
| Player | ArtPlayer + hls.js + Vidstack media icons |
| Auth | AniList OAuth 2.0 (Authorization Code) |
| Cache L1 | Upstash Redis |
| Cache L2 | Turso SQLite (libsql) |
| Episode Data | ani.zip API (TVDB thumbnails, titles, ratings) |
| Soundtrack | AnimeThemes.moe API |
| Subtitles | OpenSubtitles REST API |
| Discord | Webhook-based alerts (no bot needed) |
| Comments | TheAnimeCommunity embed |
| News | Anime News Network RSS |


## 📦 Getting Started

### Prerequisites

- Node.js 20+
- npm

### 1. Clone & install

```bash
git clone https://github.com/skgupta507/animedex-next.git
cd animedex-next
npm install
```

### 2. Environment variables

```bash
cp .env.example .env.local
```

See `.env.example` for all available variables. The app works with minimal config — only `CRYSOLINE_API_KEY` is required for streaming.

### 3. AniList OAuth setup

1. Go to [anilist.co/settings/developer](https://anilist.co/settings/developer)
2. Create a new client (Type: Web)
3. Set **Redirect URI** to: `http://localhost:3000/api/auth/callback`
4. Copy **Client ID** and **Client Secret** into `.env.local`

### 4. Run

```bash
npm run dev        # dev server
npm run build      # production build
npm run start      # production server
```

Open [http://localhost:3000](http://localhost:3000).


## 🔔 Discord Alerts Setup

Automated notifications to your Discord server via webhooks (no bot needed):

1. Create channels: `#new-episodes`, `#anime-news`, `#daily-schedule`, `#trending`, `#new-releases`, `#countdowns`, `#clip-of-the-day`, `#weekly-stats`
2. Create a webhook in each channel (Server Settings → Integrations → Webhooks)
3. Add webhook URLs to `.env`
4. Set up cron jobs on your VPS:

```bash
# Every 15 minutes — episode alerts, countdowns, schedule, trending, stats
*/15 * * * * curl -s "http://localhost:3000/api/cron/discord-alerts?key=YOUR_ADMIN_KEY"

# Every 2 hours — anime news
0 */2 * * * curl -s "http://localhost:3000/api/cron/discord-news?key=YOUR_ADMIN_KEY"
```


## 📁 Project Structure

```
src/
├── app/
│   ├── page.js                  # Home (with landing gate)
│   ├── anime/[id]/              # Anime detail page
│   ├── watch/[id]/[ep]/         # Episode player
│   ├── clip/[id]/               # Clip viewer page
│   ├── browse/                  # Browse by genre/category
│   ├── search/                  # Search results
│   ├── schedule/                # Weekly airing schedule
│   ├── news/                    # Anime & manga news (ANN RSS)
│   ├── manga/                   # Manga reader (Atsumaru)
│   ├── watch-together/          # Watch party rooms
│   ├── profile/                 # User profile (requires login)
│   └── api/
│       ├── anime/               # Info, episodes, search, home, schedule, episodes-meta
│       ├── auth/                # Login, callback, session, logout, me
│       ├── stream/              # Crysoline, Animelok, Miruro, sources, kickass
│       ├── proxy/               # Server-side HLS proxy (M3U8 rewriting)
│       ├── subtitles/           # OpenSubtitles search + download
│       ├── clips/               # Clip creation and retrieval
│       ├── themes/              # AnimeThemes.moe proxy
│       ├── trending/            # Community trending rankings
│       ├── cron/                # Discord webhook alerts
│       └── cache/               # Cache stats and prune
├── components/
│   ├── HlsPlayer.jsx            # ArtPlayer + hls.js + AniSkip + watermark
│   ├── WatchClient.jsx          # Watch page (sources, controls, episodes)
│   ├── AnimeDetailClient.jsx    # Anime detail (TVDB thumbnails, gallery)
│   ├── ClipCreator.jsx          # Clip & Share panel
│   ├── DownloadButton.jsx       # Episode download
│   ├── SoundtrackPlayer.jsx     # Mini OP/ED music player
│   ├── SubtitleSearch.jsx       # Multi-language subtitle search
│   ├── CommunityTrending.jsx    # Trending sidebar card
│   ├── DevToolsGuard.jsx        # DevTools deterrent
│   └── ...
└── lib/
    ├── crysoline.js             # Crysoline API client
    ├── discord.js               # Discord webhook utilities
    ├── recommendations.js       # "Picked For You" engine
    ├── cache.js                 # Three-layer cache
    ├── anilist.js               # AniList GraphQL client
    └── api.js                   # Client-side API wrapper
```


## 🎬 Streaming Sources

| Source | Type | Default |
|--------|------|---------|
| **Kiwi** (miruro-api-jade) | HLS | ✅ Primary |
| **AnimePahe** (Crysoline) | HLS | Fallback |
| **AnimeGG** (Crysoline) | MP4 | Fallback |
| **Animelok** | HLS + Multi-audio | Manual |
| **Miruro** | HLS | Manual |
| **VidNest** | Embed | Manual |
| **MegaPlay** | Embed | Manual |

All HLS streams are proxied server-side through `/api/proxy` — handles CORS, CDN referer requirements, and M3U8 segment URL rewriting.


## 🔒 Environment Variables

| Variable | Required | Description |
|---|---|---|
| `CRYSOLINE_API_KEY` | ✅ | Crysoline streaming API key |
| `ANILIST_CLIENT_ID` | For login | AniList OAuth client ID |
| `ANILIST_CLIENT_SECRET` | For login | AniList OAuth client secret |
| `ANILIST_REDIRECT_URI` | For login | OAuth callback URL |
| `UPSTASH_REDIS_REST_URL` | Recommended | Upstash Redis cache |
| `UPSTASH_REDIS_REST_TOKEN` | Recommended | Upstash Redis token |
| `TURSO_DATABASE_URL` | Recommended | Turso SQLite for mappings |
| `TURSO_AUTH_TOKEN` | Recommended | Turso auth token |
| `NEXT_PUBLIC_SITE_URL` | Production | Your domain (e.g. https://animedex.pp.ua) |
| `NEXT_PUBLIC_ADMIN_KEY` | For admin | Admin key for announcements/ads/cron |
| `DISCORD_WEBHOOK_*` | For alerts | Discord webhook URLs (8 channels) |


## � License

MIT — do whatever you want with it.
