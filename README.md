# brrr

Control plane for agent-to-user communication via [brrr](https://brrr.now/).

A shell script that lets agents (or scripts, CI, cron jobs, etc.) send structured notifications directly to a user's phone. One command, instant delivery.

## Setup

```bash
# 1. Clone
git clone https://github.com/Saint0X/brrr.git
cd brrr

# 2. Add your credentials
cp .env.example .env
# Edit .env with your brrr API key and URL

# 3. Install to your shell
brrr install --zsh      # or --bash or --fish
```

## Usage

### Urgency levels

```bash
brrr critical [reason] <message>   # 🚨 Something is broken — user must act now
brrr urgent   [reason] <message>   # 🔴 User's attention needed immediately
brrr alert    [reason] <message>   # 🟠 Important — user should act soon
brrr high     [reason] <message>   # 🟡 Elevated priority
brrr info     [reason] <message>   # 💬 Keeping the user informed
brrr low      [reason] <message>   # 🟢 Non-urgent — user can check later
brrr bg       [reason] <message>   # ⚪ Background context, no action needed
brrr fyi      [reason] <message>   # 📎 For the user's awareness
```

The `[reason]` is optional — it tags why the user is being contacted. If you pass one argument, it's treated as the message.

### Quick actions

```bash
brrr ping [message]       # Quick nudge (default: 👋 ping)
brrr raw  <message>       # Send raw unformatted text
brrr test                 # Confirm the connection works
brrr pipe [urgency]       # Pipe stdout into a notification
brrr help                 # Show reference
```

### Full control

```bash
brrr send -u <urgency> -r <reason> -m <message> [-s <source>]
```

| Flag | Description |
|------|-------------|
| `-u, --urgency` | `critical\|urgent\|alert\|high\|normal\|info\|low\|bg\|fyi` |
| `-r, --reason` | Why the user is being contacted |
| `-m, --message` | What the user needs to know |
| `-s, --source` | What's sending this (default: `terminal`) |

### Examples

```bash
brrr urgent "deploy" "prod is returning 500s on /api/auth"
brrr low "reminder" "PR #31 is ready for your review"
brrr info "Build passed — all 127 tests green"
brrr ping
brrr send -u critical -r "outage" -m "DB pool exhausted"
echo "backup done" | brrr pipe bg
brrr raw "Hello world! 🚀"
```

## Environment

| Variable | Description |
|----------|-------------|
| `BRRR_DIR` | Directory containing `.env` (default: script location) |
| `BRRR_API_URL` | API endpoint (loaded from `.env`) |
| `BRRR_API_KEY` | API key (loaded from `.env`) |

## License

MIT
