## Install OpenClaw

```
npm i -g openclaw@latest
```

## Install Ollama

Subscribe to a cloud plan at https://ollama.com/pricing

```
curl -fsSL https://ollama.com/install.sh | sh
ollama signin
ollama pull glm-5.1:cloud
ollama launch openclaw --config
```

Remarks: Do not launch OpenClaw for now.

Read more at https://docs.ollama.com/integrations/openclaw

## Configure and Run OpenClaw

Configure openclaw.json:

Take [openclaw.json](openclaw_sample.json) as a reference and update it with your configuration.

Edit `~.openclaw/openclaw.json`, and run:

```
openclaw onboard --install-daemon
```

Remarks: Skip the model provider option and keep the current value.

## Configure Web Search with SearXNG

```
openclaw configure
```

Enter url for searxng, e.g. http://localhost:4000.

## Configure Discord

Follow the instruction in [Discord Setup](discord.md).

Then run this command to add discord channel:

```
openclaw configure
```

Alternately, install discord plugin manually:

```
openclaw plugins install @openclaw/discord
```

## Configure WhatsApp and Instagram [Optional]

```
openclaw configure
```

## Upgrade OpenClaw

For example, upgrading to a newer nvm version:

```
nvm install 24 # change version number here
nvm alias default 24
npm install -g mcporter@latest
npm install -g clawhub@latest
npm install -g @steipete/summarize@latest
npm install -g @mariozechner/pi-ai@latest
npm install -g openclaw@latest
npm install -g @google/gemini-cli@latest
npm install -g @openai/codex@latest
```
