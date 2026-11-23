<div align="center">

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="https://raw.githubusercontent.com/lakshyapocketlab/.github/main/assets/hello-logo-on-dark.svg">
  <source media="(prefers-color-scheme: light)" srcset="https://raw.githubusercontent.com/lakshyapocketlab/.github/main/assets/hello-logo.svg">
  <img alt="Hello" src="https://raw.githubusercontent.com/lakshyapocketlab/.github/main/assets/hello-logo.svg" height="64">
</picture>

# The official WhatsApp Business API for businesses, SaaS platforms and AI agents.

**Connect your number to Meta's official API. Send through one endpoint, receive replies on your own server, and follow every message to *read*.**

[![Website](https://img.shields.io/badge/Website-lakshyapocketlab.com-03A84E?style=for-the-badge)](https://lakshyapocketlab.com)
[![Docs](https://img.shields.io/badge/Docs-read_the_guide-2563EB?style=for-the-badge)](https://lakshyapocketlab.com/docs/)
[![Coexistence](https://img.shields.io/badge/Coexistence-app_%2B_API-25D366?style=for-the-badge)](https://lakshyapocketlab.com/whatsapp-coexistence/)
[![Partners](https://img.shields.io/badge/Partner_Program-SaaS_%26_B2B-7C3AED?style=for-the-badge)](https://lakshyapocketlab.com/partners/)

*Official Meta Tech Provider &nbsp;·&nbsp; One month on us, no card required.*

</div>

<br>

## Why Hello

Hello gives your software a voice on WhatsApp. It is infrastructure, not an inbox and not a broadcast tool.

| Your number, on the official platform | One endpoint, every message | Delivery you can see |
|---|---|---|
| Onboard through Meta's Embedded Signup in minutes. With [WhatsApp coexistence](https://lakshyapocketlab.com/whatsapp-coexistence/), your WhatsApp Business app keeps working on the same number. | Templates, text, media, documents, location and contacts, through one consistent API. | Every message returns an ID and reports its journey: queued, sent, delivered, read. When one fails, you see why. |

**Replies, straight to your server.** With Direct webhook, Meta delivers incoming messages and status updates to your own endpoint.

**Meta's rates, unmarked.** WhatsApp message charges are billed by Meta directly, at Meta's published rates. Hello adds no markup.

## WhatsApp coexistence: keep the app, add the API

Already using the WhatsApp Business app? Keep it. With coexistence, the same number works in the app **and** on the API at the same time:

- Your team keeps chatting from the WhatsApp Business app on their phones.
- Your software sends order updates, reminders and passcodes through Hello.
- Chat history and contacts stay on the number. No new number, no migration.

[How coexistence works →](https://lakshyapocketlab.com/whatsapp-coexistence/)

## Hello Partner Program: WhatsApp for every one of your customers

For SaaS platforms, agencies and B2B companies that want to offer WhatsApp inside their own product:

- **One WhatsApp Business account per customer**, each on its own number, so one customer's quality rating never affects another's.
- **Onboard customers in minutes** through Meta's Embedded Signup, and manage them all from one dashboard.
- **Build once** on the same API, SDKs and MCP server you already use.

[Become a partner →](https://lakshyapocketlab.com/partners/)

## Built for the moments that matter

| When this happens in your system | Your customer receives |
|---|---|
| An order ships | A tracking update |
| An appointment is tomorrow | A reminder with Confirm and Reschedule |
| An invoice is issued | The invoice, as a PDF |
| A payment is due | A reminder with a payment link |
| A user signs in | A one-time passcode |

## Official SDKs

| Language | Repository | Install |
|---|---|---|
| ![Go](https://img.shields.io/badge/Go-00ADD8?logo=go&logoColor=white) | [`hello-go`](https://github.com/lakshyapocketlab/hello-go) | `go get github.com/lakshyapocketlab/hello-go` |
| ![Node.js](https://img.shields.io/badge/Node.js-339933?logo=nodedotjs&logoColor=white) | [`hello-node`](https://github.com/lakshyapocketlab/hello-node) | `npm i @lakshyapocketlab/hello` |
| ![Python](https://img.shields.io/badge/Python-3776AB?logo=python&logoColor=white) | [`hello-python`](https://github.com/lakshyapocketlab/hello-python) | `pip install lakshyapocketlab-hello` |

Create an API key (`hello_live_...`) in the [Hello dashboard](https://hello.lakshyapocketlab.com) and set `HELLO_API_KEY`. Then send your first message:

<details>
<summary><b>Go</b></summary>

```go
package main

import (
	"context"
	"fmt"
	"log"

	hello "github.com/lakshyapocketlab/hello-go"
)

func main() {
	// Reads HELLO_API_KEY (hello_live_...) unless hello.WithAPIKey is given.
	client, err := hello.NewClient()
	if err != nil {
		log.Fatal(err)
	}
	ctx := context.Background()

	numbers, err := client.PhoneNumbers.List(ctx)
	if err != nil {
		log.Fatal(err)
	}

	resp, err := client.Messages.SendText(ctx, numbers[0].PhoneNumberID, "+16505551234", "Your order has shipped.")
	if err != nil {
		log.Fatal(err)
	}
	fmt.Println("Hello message ID:", resp.Messages[0].HelloID)
}
```

</details>

<details>
<summary><b>Node.js</b></summary>

```ts
import { Hello } from "@lakshyapocketlab/hello";

const hello = new Hello({ apiKey: process.env.HELLO_API_KEY });

const { data: numbers } = await hello.phoneNumbers.list();
const from = numbers[0]!.phone_number_id;

// Templates can start a conversation; free-form messages need an open 24-hour window.
const res = await hello.messages.sendTemplate(from, "+919999999999", {
  name: "order_shipped",
  language: "en",
  components: [{ type: "body", parameters: [{ type: "text", text: "Asha" }] }],
});
console.log(res.messages[0]?.hello_id);
```

</details>

<details>
<summary><b>Python</b></summary>

```python
from lakshyapocketlab_hello import Hello

with Hello() as client:  # reads HELLO_API_KEY
    number = client.phone_numbers.list()["data"][0]
    sent = client.messages.send_text(
        number["phone_number_id"], "+919999999999", "Hello from Python!"
    )
    print(sent["messages"][0]["hello_id"])
```

</details>

## Connect AI agents (MCP)

Hello runs a Model Context Protocol server in front of the API. Connect Claude, Cursor or Codex, then send messages, check delivery and manage templates by asking. The server uses Streamable HTTP and authenticates with your API key as a Bearer token.

```bash
claude mcp add --transport http hello "https://helloapi.lakshyapocketlab.com/mcp" \
  --header "Authorization: Bearer $HELLO_API_KEY"
```

Setup for Claude Code, Claude Desktop, Cursor, VS Code, Windsurf and Codex is in the [MCP guide](https://lakshyapocketlab.com/docs/ai/integrate-with-ai/).

## Links

| | |
|---|---|
| **Website** | [lakshyapocketlab.com](https://lakshyapocketlab.com) |
| **Dashboard and API keys** | [hello.lakshyapocketlab.com](https://hello.lakshyapocketlab.com) |
| **Docs** | [lakshyapocketlab.com/docs](https://lakshyapocketlab.com/docs/) |
| **Coexistence** | [lakshyapocketlab.com/whatsapp-coexistence](https://lakshyapocketlab.com/whatsapp-coexistence/) |
| **Partner Program** | [lakshyapocketlab.com/partners](https://lakshyapocketlab.com/partners/) |
| **Quickstart** | [lakshyapocketlab.com/docs/getting-started/quickstart](https://lakshyapocketlab.com/docs/getting-started/quickstart/) |
| **Support** | [support@lakshyapocketlab.com](mailto:support@lakshyapocketlab.com) |

<br>

<div align="center">

Open source under the MIT license. Contributions are welcome: see [CONTRIBUTING](https://github.com/lakshyapocketlab/.github/blob/main/CONTRIBUTING.md).

<sub>&copy; Lakshya Pocket Lab</sub>

</div>
