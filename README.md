# osubot-telegram

Telegram bot for osu! — profiles, recent plays, top scores, map leaderboards, pp math and replay rendering, across bancho and a handful of private servers.

A fork of [Airkek/osubot-telegram](https://github.com/Airkek/osubot-telegram), which in turn started as a port of OctoDumb's [osubot-old](https://github.com/OctoDumb/osubot-old).

**Bot:** [@osureplaybot](https://t.me/osureplaybot) · **Code:** [`morasooma`](https://github.com/gsslg4/osubot-telegram/tree/morasooma) branch

## What this fork adds

- **Morasooma.** The private osu!lazer server [morasooma.net](https://morasooma.net) runs a g0v0-server, so it speaks osu! API v2 and sits next to the others on the `m` prefix — profiles, top and recent scores, compare, leaderboards, replays, its own linked nickname and its own mode.
- **Redrawn cards.** Every card — profile, score, top scores, leaderboard, beatmap and pp — is rendered with [satori](https://github.com/vercel/satori) instead of canvas, in one visual language.
- **Inline mode.** `@osureplaybot <nickname>` in any chat asks every server at once and offers a card per server that knows the player. A prefix pins it to one: `@osureplaybot m peppy`.

## Servers

| Prefix | Server | | Prefix | Server |
| --- | --- | --- | --- | --- |
| `s` | Bancho | | `a` | Akatsuki |
| `m` | **Morasooma** | | `ax` | Akatsuki!Relax |
| `g` | Gatari | | `ap` | Akatsuki!AutoPilot |
| `r` | Ripple | | `bl` | BeatLeader |
| `rx` | Ripple!Relax | | `ss` | ScoreSaber |

Commands go as `<prefix> <command>`: `user`, `top`, `recent`, `compare`, `leaderboard`, `chat`, `find`, `nick`, `mode`. So `m u innerbloom` is a Morasooma profile, `m t` your own top scores there. `osu help` lists the rest.

## Running it

Docker Compose brings up the bot, the osu! performance server, PostgreSQL and a local Telegram Bot API server. Full setup — Compose file, `.env.example` and the comments in it — is in the [upstream README](https://github.com/Airkek/osubot-telegram#run-with-docker-compose); nothing there changed in this fork.

To build from this branch instead of the published image, point the bot service at the checkout:

```yaml
osubot:
  build:
    context: ../osubot-src
    dockerfile: bot/Dockerfile
  image: osubot-telegram:morasooma
```

### Extra settings

| Variable | Purpose |
| --- | --- |
| `MORA_USERNAME`, `MORA_PASSWORD` | Morasooma account to sign in with. Morasooma issues no OAuth client credentials and score endpoints are gated, so an account is the only way in. Without these the bot stays anonymous and serves profiles only. |
| `MORA_API_BASE` | Morasooma host. Defaults to `https://morasooma.net`. |
| `MORA_CLIENT_ID`, `MORA_CLIENT_SECRET` | OAuth client credentials, if they are ever issued. |
| `INLINE_CARD_BASE_URL` | Where the Bot API server fetches rendered inline cards, e.g. `http://osubot:3000`. Without it inline results degrade to text. |

Inline mode also has to be enabled for the bot in [@BotFather](https://t.me/BotFather) with `/setinline`.

## License

GPL-3.0-only, same as upstream. The files under `bot/assets/` are **not** covered by that grant — see [NOTICE](NOTICE); they include osu!-specific material belonging to ppy Pty Ltd.
