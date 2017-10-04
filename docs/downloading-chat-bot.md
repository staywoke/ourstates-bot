**[↤ Developer Overview](../README.md)**

Downloading Chat Bot
===

You can download this Chat Bot using the code below ( this assumes you have [SSH integrated with Github](https://help.github.com/articles/adding-a-new-ssh-key-to-your-github-account/) ):

```bash
cd /path/to/bot
git clone git@github.com:staywoke/ourstates-bot.git .
```

Setup Environment
---

```bash
cp .env.example .env
```

Use your favorite text editor and set the following values in `.env` to whatever you need:

```
APP_ENV=local
APP_DEBUG=true
APP_LOG_LEVEL=debug
APP_URL=http://localhost
BUGSNAG_API_KEY=
SEGMENT_API_KEY=
```

Setup Folder Permissions
---

```bash
chmod -R o+w bootstrap/cache
chmod -R o+w storage
```
