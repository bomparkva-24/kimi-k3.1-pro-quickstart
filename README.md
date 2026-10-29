```markdown
# Kimi K3.1 Pro Quickstart

Minimal, copy-paste examples for calling **Kimi K3.1 Pro** from Moonshot AI and its faster **Highspeed** variant through an OpenAI-compatible API.

- **Model page and pricing:** [Kimi K3.1 Pro on CometAPI](https://www.cometapi.com/models/moonshotai/kimi-k3-1-pro/?utm_source=github&utm_medium=distribution&utm_campaign=bom&utm_content=kimi-k3-1-pro)
- **Topics:** `kimi`, `moonshot-ai`, `llm`, `openai-compatible`, `quickstart`

## Why Kimi K3.1 Pro?

- 65.7% on Terminal-Bench 4.0
- Up to 1M tokens of context
- Text, image, and video input
- Reasoning effort options such as `low`, `high`, and `max`
- Function calling, JSON schema output, streaming, and context caching

> Benchmark results are reported by Moonshot AI. Test the model on your own workload before relying on these figures.

## Models and pricing

| Model ID | Input / 1M tokens | Output / 1M tokens | Notes |
|---|---:|---:|---|
| `kimi-k3.1-pro` | $1.60 | $8.00 | Standard pricing; official pricing is listed as $2.00 / $10.00 |
| `kimi-k3.1-pro-highspeed` | $3.20 | $16.00 | Faster output and higher quota, according to CometAPI |

Cached input reads are listed at **$0.16 per 1M tokens**.

Check the [current model pricing](https://www.cometapi.com/models/moonshotai/kimi-k3-1-pro/?utm_source=github&utm_medium=distribution&utm_campaign=bom&utm_content=kimi-k3-1-pro) before estimating costs.

## Setup

Set your CometAPI key as an environment variable:

```bash
export COMETAPI_KEY="your_key_here"
```

Keep your API key private. Do not commit it to your repository.

## cURL

```bash
curl --fail-with-body \
  "https://api.cometapi.com/v1/chat/completions" \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer $COMETAPI_KEY" \
  -d '{
    "model": "kimi-k3.1-pro",
    "messages": [
      {
        "role": "user",
        "content": "Explain what an API is in one sentence."
      }
    ]
  }'
```

## Python

Install the OpenAI SDK:

```bash
python -m pip install openai
```

Save this as `examples/quickstart.py`:

```python
import os

from openai import OpenAI

client = OpenAI(
    base_url="https://api.cometapi.com/v1",
    api_key=os.environ["COMETAPI_KEY"],
)

# Basic call
response = client.chat.completions.create(
    model="kimi-k3.1-pro",
    messages=[
        {"role": "system", "content": "You are a helpful assistant."},
        {"role": "user", "content": "Explain what an API is in one sentence."},
    ],
)

print(response.choices[0].message.content)

# Streaming with the Highspeed variant
stream = client.chat.completions.create(
    model="kimi-k3.1-pro-highspeed",
    messages=[
        {"role": "user", "content": "Give me three tips for writing clean Python."}
    ],
    stream=True,
)

for chunk in stream:
    if chunk.choices and chunk.choices[0].delta.content:
        print(chunk.choices[0].delta.content, end="", flush=True)

print()
```

Run it with:

```bash
python examples/quickstart.py
```

## Node.js 18+

Install the OpenAI SDK:

```bash
npm install openai
```

Save this as `examples/quickstart.mjs`:

```javascript
import OpenAI from "openai";

const apiKey = process.env.COMETAPI_KEY;

if (!apiKey) {
  throw new Error("Set the COMETAPI_KEY environment variable first.");
}

const client = new OpenAI({
  baseURL: "https://api.cometapi.com/v1",
  apiKey,
});

const response = await client.chat.completions.create({
  model: "kimi-k3.1-pro",
  messages: [
    {
      role: "user",
      content: "Explain what an API is in one sentence.",
    },
  ],
});

console.log(response.choices[0].message.content);
```

Run it with:

```bash
node examples/quickstart.mjs
```

## Gotchas

- **Reasoning cannot be disabled.** Choose an effort level instead. Check the API documentation for the exact parameter name and supported values.
- **Multi-turn conversations and tool calls:** resend the complete assistant message, including reasoning and tool-call fields, when required by the API.
- **Sampling controls may be fixed:** parameters such as `temperature`, `top_p`, `n`, and penalties may not be configurable.
- **Images:** use the structured content format and provide image data in the format required by the API.
- **Context limits and supported features:** confirm current limits and availability in the provider documentation.

## Links

- [Kimi K3.1 Pro model page and pricing](https://www.cometapi.com/models/moonshotai/kimi-k3-1-pro/?utm_source=github&utm_medium=distribution&utm_campaign=bom&utm_content=kimi-k3-1-pro)
- [CometAPI API documentation](https://apidoc.cometapi.com)

## License

MIT



