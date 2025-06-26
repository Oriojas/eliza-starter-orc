export NVM_DIR="$HOME/.nvm" && [ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh" && nvm use 22 && export PATH="$HOME/.local/bin:$PATH" && pnpm start# Eliza

## Character Configuration

### Default Character (Eliza)

The default character is defined in `src/character.ts`. To modify it:

1. **Open the character file:**
```bash
nano src/character.ts  # or use your preferred editor
```

2. **Basic character configuration:**
```typescript
export const character: Character = {
    ...defaultCharacter,
    name: "Eliza",
    plugins: [],
    clients: [],
    modelProvider: ModelProviderName.GROQ,  // or OPENAI, ANTHROPIC, etc.
    // Uncomment and edit these sections:
    // bio: [
    //     "Your character's biography here",
    //     "Multiple lines describe your character"
    // ],
    // system: "System prompt that defines behavior",
    // messageExamples: [
    //     [
    //         { user: "{{user1}}", content: { text: "Hello" } },
    //         { user: "Eliza", content: { text: "Hello there!" } }
    //     ]
    // ]
};
```

### Custom Characters

#### Option 1: Create JSON character files

1. **Create a character JSON file** (e.g., `characters/my-character.json`):
```json
{
    "name": "MyBot",
    "bio": [
        "I'm a helpful assistant specialized in coding",
        "I love to help with programming questions"
    ],
    "system": "You are a coding assistant. Be helpful and concise.",
    "modelProvider": "groq",
    "clients": ["twitter", "discord"],
    "postExamples": [
        "Just solved a tricky algorithm problem!",
        "Code review tip: always comment your complex logic"
    ],
    "messageExamples": [
        [
            {"user": "{{user1}}", "content": {"text": "How do I sort an array?"}},
            {"user": "MyBot", "content": {"text": "You can use array.sort() in JavaScript, or sorted() in Python!"}}
        ]
    ]
}
```

2. **Load custom character:**
```bash
# Single character
export NVM_DIR="$HOME/.nvm" && [ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh" && nvm use 22 && export PATH="$HOME/.local/bin:$PATH" && pnpm start --characters="characters/my-character.json"

# Multiple characters
export NVM_DIR="$HOME/.nvm" && [ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh" && nvm use 22 && export PATH="$HOME/.local/bin:$PATH" && pnpm start --characters="characters/char1.json,characters/char2.json"
```

#### Option 2: Modify the TypeScript character

1. **Edit `src/character.ts` directly:**
```typescript
export const character: Character = {
    ...defaultCharacter,
    name: "CodeBot",
    bio: [
        "Expert programming assistant",
        "Specializes in Python, JavaScript, and system design"
    ],
    system: "You are a senior software engineer. Provide clear, practical coding advice.",
    modelProvider: ModelProviderName.GROQ,
    postExamples: [
        "Clean code is not written by following a set of rules",
        "Debugging is twice as hard as writing the code"
    ],
    topics: [
        "Programming",
        "Software architecture",
        "Code review",
        "Debugging techniques"
    ],
    style: {
        all: [
            "Be technical but approachable",
            "Use code examples when helpful",
            "Focus on best practices"
        ]
    }
};
```

### Model Provider Configuration

Choose your AI model provider in the character configuration:

```typescript
// For Groq (recommended for cost and speed)
modelProvider: ModelProviderName.GROQ,

// For OpenAI
modelProvider: ModelProviderName.OPENAI,

// For Anthropic Claude
modelProvider: ModelProviderName.ANTHROPIC,

// For local Ollama
modelProvider: ModelProviderName.OLLAMA,
```

Make sure to configure the corresponding API keys in your `.env` file.

### Add Social Media Clients

Add social media integration to your character:

```typescript
// In character.ts
clients: [Clients.TWITTER, Clients.DISCORD, Clients.TELEGRAM],

// In character.json
"clients": ["twitter", "discord", "telegram"]
```

**Available clients:**
- `twitter` - X/Twitter integration
- `discord` - Discord bot
- `telegram` - Telegram bot

**Note:** Each client requires additional configuration in your `.env` file with API keys and credentials.

## Duplicate the .env.example template

```bash
cp .env.example .env
```

\* Fill out the .env file with your own values.

### Add login credentials and keys to .env
```
DISCORD_APPLICATION_ID="discord-application-id"
DISCORD_API_TOKEN="discord-api-token"
...
OPENROUTER_API_KEY="sk-xx-xx-xxx"
...
TWITTER_USERNAME="username"
TWITTER_PASSWORD="password"
TWITTER_EMAIL="your@email.com"
```

## Install dependencies and start your agent

### Prerequisites
- Node.js version 22 or higher
- pnpm package manager

### Installation and Setup

1. **Install Node.js 22 (if not already installed):**
```bash
# Install nvm (Node Version Manager)
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.0/install.sh | bash

# Reload your shell or run:
export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"

# Install and use Node.js 22
nvm install 22
nvm use 22
```

2. **Install pnpm (if not already installed):**
```bash
npm install -g pnpm --prefix ~/.local
export PATH="$HOME/.local/bin:$PATH"
```

3. **Install dependencies and start your agent:**
```bash
# Set up Node.js environment and install dependencies
export NVM_DIR="$HOME/.nvm" && [ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh" && nvm use 22 && export PATH="$HOME/.local/bin:$PATH" && pnpm install

# Build the project
export NVM_DIR="$HOME/.nvm" && [ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh" && nvm use 22 && export PATH="$HOME/.local/bin:$PATH" && pnpm run build

# Start the agent
export NVM_DIR="$HOME/.nvm" && [ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh" && nvm use 22 && export PATH="$HOME/.local/bin:$PATH" && pnpm start
```

4. **For future runs (after initial setup):**
```bash
export NVM_DIR="$HOME/.nvm" && [ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh" && nvm use 22 && export PATH="$HOME/.local/bin:$PATH" && pnpm start
```

### Common Issues

**If you encounter build errors with Bun:**
- This project requires Node.js 22 with pnpm, not Bun
- Bun is not compatible with the native dependencies like `better-sqlite3`

**If you get "ERR_USE_AFTER_CLOSE" errors:**
- Make sure you're using Node.js 22
- Try cleaning the data directory: `rm -rf data/*`
- Ensure your API keys are correctly configured in `.env`

## API Endpoints

Once your agent is running, you can interact with it via REST API endpoints.

### Get Available Agents

```bash
curl http://localhost:3001/agents
```

Response:
```json
{
  "agents": [
    {
      "id": "b850bc30-45f8-0041-a00a-83df46d8555d",
      "name": "Eliza",
      "clients": []
    }
  ]
}
```

### Send Message to Agent

```bash
curl -X POST http://localhost:3001/b850bc30-45f8-0041-a00a-83df46d8555d/message \
  -H "Content-Type: application/json" \
  -d '{
    "text": "Hello, how are you?",
    "userId": "user123",
    "userName": "Test User"
  }'
```

Response:
```json
[
  {
    "user": "Eliza",
    "text": "ah, hello there. the night brings interesting conversations, doesn't it?",
    "action": "NONE"
  }
]
```

## Python Examples

### Basic Setup

```python
import requests
import json

# Base URL for your Eliza instance
BASE_URL = "http://localhost:3001"

# Get agent ID
def get_agent_id():
    response = requests.get(f"{BASE_URL}/agents")
    agents = response.json()["agents"]
    return agents[0]["id"] if agents else None

# Send message to Eliza
def send_message(agent_id, message, user_id="user123", user_name="User"):
    url = f"{BASE_URL}/{agent_id}/message"
    payload = {
        "text": message,
        "userId": user_id,
        "userName": user_name
    }

    response = requests.post(url, json=payload)
    if response.status_code == 200:
        return response.json()[0]["text"]
    else:
        return f"Error: {response.status_code}"

# Example usage
if __name__ == "__main__":
    agent_id = get_agent_id()
    if agent_id:
        # Send a message
        response = send_message(agent_id, "¿Puedes ayudarme con programación en Python?")
        print(f"Eliza: {response}")
    else:
        print("No agents found")
```

### Interactive Chat Script

```python
import requests
import json

class ElizaChat:
    def __init__(self, base_url="http://localhost:3001"):
        self.base_url = base_url
        self.agent_id = self._get_agent_id()
        self.user_id = "chat_user"
        self.user_name = "User"

    def _get_agent_id(self):
        try:
            response = requests.get(f"{self.base_url}/agents")
            agents = response.json()["agents"]
            return agents[0]["id"] if agents else None
        except Exception as e:
            print(f"Error getting agent ID: {e}")
            return None

    def send_message(self, message):
        if not self.agent_id:
            return "No agent available"

        url = f"{self.base_url}/{self.agent_id}/message"
        payload = {
            "text": message,
            "userId": self.user_id,
            "userName": self.user_name
        }

        try:
            response = requests.post(url, json=payload)
            if response.status_code == 200:
                return response.json()[0]["text"]
            else:
                return f"Error: {response.status_code}"
        except Exception as e:
            return f"Connection error: {e}"

    def chat(self):
        print("🤖 Eliza Chat - Type 'quit' to exit")
        print("=" * 50)

        while True:
            user_input = input("\nYou: ").strip()

            if user_input.lower() in ['quit', 'exit', 'bye']:
                print("👋 Goodbye!")
                break

            if user_input:
                response = self.send_message(user_input)
                print(f"Eliza: {response}")

# Run the chat
if __name__ == "__main__":
    chat = ElizaChat()
    chat.chat()
```

### Async Example with Multiple Messages

```python
import asyncio
import aiohttp
import json

class AsyncElizaClient:
    def __init__(self, base_url="http://localhost:3001"):
        self.base_url = base_url
        self.agent_id = None

    async def init(self):
        async with aiohttp.ClientSession() as session:
            async with session.get(f"{self.base_url}/agents") as response:
                data = await response.json()
                agents = data.get("agents", [])
                self.agent_id = agents[0]["id"] if agents else None

    async def send_message(self, message, user_id="async_user", user_name="AsyncUser"):
        if not self.agent_id:
            return "No agent available"

        url = f"{self.base_url}/{self.agent_id}/message"
        payload = {
            "text": message,
            "userId": user_id,
            "userName": user_name
        }

        async with aiohttp.ClientSession() as session:
            async with session.post(url, json=payload) as response:
                if response.status == 200:
                    data = await response.json()
                    return data[0]["text"]
                else:
                    return f"Error: {response.status}"

    async def send_multiple_messages(self, messages):
        tasks = [self.send_message(msg) for msg in messages]
        responses = await asyncio.gather(*tasks)
        return responses

# Example usage
async def main():
    client = AsyncElizaClient()
    await client.init()

    messages = [
        "Hello Eliza!",
        "What do you think about AI?",
        "Can you help me with coding?",
        "Tell me a philosophical thought"
    ]

    responses = await client.send_multiple_messages(messages)

    for msg, response in zip(messages, responses):
        print(f"You: {msg}")
        print(f"Eliza: {response}")
        print("-" * 50)

# Run async example
if __name__ == "__main__":
    asyncio.run(main())
```

### Error Handling and Retry Logic

```python
import requests
import time
from typing import Optional

class RobustElizaClient:
    def __init__(self, base_url="http://localhost:3001", max_retries=3):
        self.base_url = base_url
        self.max_retries = max_retries
        self.agent_id = self._get_agent_id_with_retry()

    def _get_agent_id_with_retry(self) -> Optional[str]:
        for attempt in range(self.max_retries):
            try:
                response = requests.get(f"{self.base_url}/agents", timeout=10)
                response.raise_for_status()
                agents = response.json().get("agents", [])
                return agents[0]["id"] if agents else None
            except Exception as e:
                print(f"Attempt {attempt + 1} failed: {e}")
                if attempt < self.max_retries - 1:
                    time.sleep(2 ** attempt)  # Exponential backoff
        return None

    def send_message_with_retry(self, message, user_id="user", user_name="User") -> str:
        if not self.agent_id:
            return "❌ No agent available"

        url = f"{self.base_url}/{self.agent_id}/message"
        payload = {
            "text": message,
            "userId": user_id,
            "userName": user_name
        }

        for attempt in range(self.max_retries):
            try:
                response = requests.post(url, json=payload, timeout=30)
                response.raise_for_status()

                data = response.json()
                return data[0]["text"]

            except requests.exceptions.Timeout:
                print(f"⏰ Timeout on attempt {attempt + 1}")
            except requests.exceptions.ConnectionError:
                print(f"🔌 Connection error on attempt {attempt + 1}")
            except Exception as e:
                print(f"❌ Error on attempt {attempt + 1}: {e}")

            if attempt < self.max_retries - 1:
                time.sleep(2 ** attempt)

        return "❌ Failed to get response after all retries"

# Example usage
if __name__ == "__main__":
    client = RobustElizaClient()

    messages = [
        "Hello, how are you?",
        "What's your favorite programming language?",
        "Can you explain machine learning?",
        "Tell me about philosophy"
    ]

    for msg in messages:
        print(f"\n💬 You: {msg}")
        response = client.send_message_with_retry(msg)
        print(f"🤖 Eliza: {response}")
```

## Run with Docker

### Build and run Docker Compose (For x86_64 architecture)

#### Edit the docker-compose.yaml file with your environment variables

```yaml
services:
    eliza:
        environment:
            - OPENROUTER_API_KEY=blahdeeblahblahblah
```

#### Run the image

```bash
docker compose up
```

### Build the image with Mac M-Series or aarch64

Make sure docker is running.

```bash
# The --load flag ensures the built image is available locally
docker buildx build --platform linux/amd64 -t eliza-starter:v1 --load .
```

#### Edit the docker-compose-image.yaml file with your environment variables

```yaml
services:
    eliza:
        environment:
            - OPENROUTER_API_KEY=blahdeeblahblahblah
```

#### Run the image

```bash
docker compose -f docker-compose-image.yaml up
```

# Deploy with Railway

[![Deploy on Railway](https://railway.com/button.svg)](https://railway.com/template/aW47_j)
