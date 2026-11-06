# Federal Government Discord Bot

A starter Discord bot with:
- `/ask` — Gemini-powered answers using your server's own information.
- `/positions` — counts members in configured Discord roles and shows open/filled seats.
- `/ping` and `/about`.
- Secret keys loaded from environment variables, never hard-coded.

## 1. Create a Gemini API key
1. Open https://aistudio.google.com/app/apikey
2. Create an API key.
3. Keep it private. Do not put it in a Python file or commit it to GitHub.

Google's official Python SDK is `google-genai`: https://googleapis.github.io/python-genai/

## 2. Prepare the Discord bot
1. Open https://discord.com/developers/applications and select your bot.
2. Under **Bot**, enable **Server Members Intent** (needed by `/positions`).
3. Under **OAuth2 > URL Generator**, select `bot` and `applications.commands`.
4. Give only the permissions needed: View Channels, Send Messages, Embed Links, Read Message History.
5. Invite the bot using the generated URL.
6. Use the NEW token after rotating the token that was accidentally shared.

## 3. Add GitHub repository secrets
In your repository, open **Settings > Secrets and variables > Actions > New repository secret**.
Add:
- `DISCORD_TOKEN` — your newly reset Discord bot token.
- `GEMINI_API_KEY` — your Gemini API key.

Never upload `.env`, tokens, or API keys as files.

## 4. Configure the bot
Edit `data/government_info.md` and replace the template with your actual server information.
Edit the `POSITIONS` list near the top of `bot.py` so role names and seat limits exactly match your Discord server.
Role names must match Discord role names exactly.

## 5. Run locally (optional)
Python 3.10+ is recommended.

```bash
python -m venv .venv
# Windows:
.venv\Scripts\activate
# macOS/Linux:
source .venv/bin/activate

pip install -r requirements.txt
cp .env.example .env
# Edit .env with your keys, then:
python bot.py
```

## 6. Keep it online
This is an interactive bot, so it must run continuously on an always-on host (a VPS or suitable bot hosting service).
GitHub Actions is good for scheduled scripts, but it is not a dependable always-on home for a Discord gateway bot.
Set `DISCORD_TOKEN` and `GEMINI_API_KEY` as environment variables in your host's dashboard, then start with `python bot.py`.

For quick slash-command syncing while testing, set `GUILD_ID` to your Discord server ID. Leave it blank for global commands (which can take longer to appear).

## Notes
- `/ask` is limited to 6 requests per user per minute in this starter.
- `/ask` replies ephemerally by default. Remove `ephemeral=True` if you want public replies.
- The AI only knows the information you put in `data/government_info.md`; it does not automatically know your Discord server's private records.
- The bot does not execute administrative actions. Role changes, application decisions, and moderation should be added later with explicit permission checks and audit logging.
- Gemini API free-tier availability, limits, model names, and data handling can change. Check current details at https://ai.google.dev/gemini-api/docs/pricing before relying on free usage.
