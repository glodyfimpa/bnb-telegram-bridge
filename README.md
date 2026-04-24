# bnb-telegram-bridge

Bridge tra GitHub Actions e Telegram Bot API per le routine remote Claude Code (CCR) del progetto BnB Via Braida.

## Perché esiste

Le routine remote Claude Code girano in un sandbox cloud Anthropic con allowlist di rete ristretta. `api.telegram.org` non è raggiungibile direttamente dal sandbox. `github.com` invece è nell'allowlist di default (integrazione nativa).

Questo repo espone due workflow `workflow_dispatch` che fanno da ponte: la routine CCR li lancia via `gh workflow run`, l'Actions runner (che ha internet libero) chiama Telegram.

## Workflows

### `send-telegram.yml`
Invia un messaggio Telegram al chat configurato.

```bash
gh workflow run send-telegram.yml \
  -R glodyfimpa/bnb-telegram-bridge \
  -f message="Ciao da Claude routine" \
  -f parse_mode=HTML
```

### `get-telegram-updates.yml`
Recupera gli ultimi messaggi inviati al bot, filtrati per chat e finestra temporale.

```bash
gh workflow run get-telegram-updates.yml \
  -R glodyfimpa/bnb-telegram-bridge \
  -f since_hours=24
```

## Secrets richiesti

- `TELEGRAM_BOT_TOKEN` — token bot da @BotFather
- `TELEGRAM_CHAT_ID` — ID chat destinatario
