# Word Chain

A multiplayer word chain game where each word must start with the last letter of the previous word. All words are verified against the English dictionary in real-time.

![Word Chain](https://img.shields.io/badge/Game-Word%20Chain-f5a623?style=for-the-badge) ![Status](https://img.shields.io/badge/Status-Live-4caf50?style=for-the-badge) ![No Backend](https://img.shields.io/badge/Backend-None-e74c3c?style=for-the-badge)

## Play Now

**[word-chain-brown.vercel.app](https://word-chain-brown.vercel.app)**

## Game Modes

### Solo
Build the longest word chain by yourself. Beat your high score!

### VS Bot
Take turns against an AI bot. The bot sources words from the [Datamuse API](https://www.datamuse.com/api/) based on your selected difficulty level. Three invalid words in a row and you lose!

### Online Multiplayer
Play with friends in real-time! Create a room, share the 6-character code, and compete head-to-head. Supports 2-player and 3-player matches.

- No account needed — just pick a nickname
- Share the room code with your friends
- Host validates all words
- 3 invalid words = eliminated

## CEFR Difficulty Levels

Choose your vocabulary difficulty before each game:

| Level | Name | Description |
|-------|------|-------------|
| **A1** | Beginner | Very common everyday words |
| **A2** | Elementary | Common elementary words |
| **B1** | Intermediate | Moderately common words |
| **B2** | Upper Intermediate | Less common, richer vocabulary |
| **C1** | Advanced | Advanced uncommon words |
| **C2** | Proficiency | Rare and sophisticated words |

Word difficulty is based on frequency data from the Datamuse API, mapped to CEFR levels.

## Scoring

| Word Length | Points |
|-------------|--------|
| 2 letters | +1 |
| 3 letters | +2 |
| 4 letters | +4 |
| 5 letters | +7 |
| 6 letters | +10 |
| 7+ letters | +15 |

## Tech Stack

- **Frontend:** Single HTML file — vanilla JS, CSS
- **Dictionary:** [Free Dictionary API](https://dictionaryapi.dev/)
- **Word Source:** [Datamuse API](https://www.datamuse.com/api/) (bot words + CEFR frequency data)
- **Multiplayer:** [MQTT](https://mqtt.org/) over WebSocket (broker.emqx.io)
- **Hosting:** [Vercel](https://vercel.com/) (static deployment, no backend)

## How It Works

### Word Validation
Every submitted word is checked against the [Free Dictionary API](https://dictionaryapi.dev/). If the word exists, you score points and see a short definition.

### Bot Intelligence
The bot fetches words from the Datamuse API filtered by frequency score. Higher CEFR levels use rarer words, making the game progressively harder.

### Online Architecture
Online multiplayer uses MQTT pub/sub messaging — no WebRTC or backend server needed:

1. Host creates a room → subscribes to an MQTT topic
2. Joiner enters the code → subscribes to the same topic
3. All game messages (joins, word submissions, results) flow through MQTT
4. Heartbeat system detects disconnected players
5. Automatic broker failover (emqx.io → hivemq.com)

## Run Locally

```bash
# Clone the repo
git clone https://github.com/ali-novruz/word-chain.git
cd word-chain

# Serve with any static server
python -m http.server 3456

# Open http://localhost:3456
```

## Deploy

Push to GitHub and import on [Vercel](https://vercel.com/):

1. Connect your GitHub repo
2. Set **Root Directory** to `word-game` (if inside a subdirectory)
3. Deploy — no build step needed

## License

MIT
