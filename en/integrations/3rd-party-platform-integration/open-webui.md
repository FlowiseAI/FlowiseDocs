# Open WebUI

[Open WebUI](https://github.com/open-webui/open-webui) is an extensible, feature-rich, and user-friendly _self-hosted AI platform_ designed to operate entirely offline.

[Funcitons](https://docs.openwebui.com/features/plugin/functions/) are like plugins for Open WebUI. We can create a custom [Pipe Function](https://docs.openwebui.com/features/plugin/functions/pipe) that process inputs and generate responses by invoking Flowise Prediction API before returning results to the user. Through this, Flowise can be used in Open WebUI.

## Setup

1. First, have Open WebUI up and running, you can refer to the [Quickstart](https://docs.openwebui.com/getting-started/quick-start/) guide. From the left bottom, click your profile and **Admin Panel**

<figure><img src="../../.gitbook/assets/image (4).png" alt="" width="235"><figcaption></figcaption></figure>

2. Open **Functions** tab, and add a new Function.

<figure><img src="../../.gitbook/assets/image (1) (1) (1).png" alt="" width="423"><figcaption></figcaption></figure>

3. Name the Function, and add the following code:

```python
"""
title: Flowise Integration for OpenWebUI
Requirements:
  - Flowise API URL (set via FLOWISE_API_URL)
  - Flowise API Key (set via FLOWISE_API_KEY)
"""

from pydantic import BaseModel, Field
from typing import Optional, Dict, Any, List, Union, Generator, Iterator
import requests
import json
import os


class Pipe:
    class Valves(BaseModel):
        flowise_url: str = Field(
            default=os.getenv("FLOWISE_API_URL", ""),
            description="Flowise URL",
        )
        flowise_api_key: str = Field(
            default=os.getenv("FLOWISE_API_KEY", ""),
            description="Flowise API key for authentication",
        )

    def __init__(self):
        self.type = "manifold"
        self.id = "flowise_chat"
        self.valves = self.Valves()

        # Validate required settings
        if not self.valves.flowise_url:
            print(
                "⚠️ Please set your Flowise URL using the FLOWISE_API_URL environment variable"
            )
        if not self.valves.flowise_api_key:
            print(
                "⚠️ Please set your Flowise API key using the FLOWISE_API_KEY environment variable"
            )

    def pipes(self):
        if self.valves.flowise_api_key and self.valves.flowise_url:
            try:
                headers = {
                    "Authorization": f"Bearer {self.valves.flowise_api_key}",
                    "Content-Type": "application/json",
                }

                r = requests.get(
                    f"{self.valves.flowise_url}/api/v1/chatflows?type=AGENTFLOW",
                    headers=headers,
                )
                models = r.json()
                return [
                    {
                        "id": model["id"],
                        "name": model["name"],
                    }
                    for model in models
                ]

            except Exception as e:
                return [
                    {
                        "id": "error",
                        "name": e,
                    },
                ]
        else:
            return [
                {
                    "id": "error",
                    "name": "API Key not provided.",
                },
            ]

    def _process_message_content(self, message: dict) -> str:
        """Process message content, handling text for now"""
        if isinstance(message.get("content"), list):
            processed_content = []
            for item in message["content"]:
                if item["type"] == "text":
                    processed_content.append(item["text"])
            return " ".join(processed_content)
        return message.get("content", "")

    def pipe(
        self, body: dict, __user__: Optional[dict] = None, __metadata__: dict = None
    ) -> Union[str, Generator, Iterator]:
        """Process chat messages through Flowise"""
        try:
            print("\nProcessing Flowise request:")
            print(f"Request body: {json.dumps(body, indent=2)}")

            stream_enabled = body.get("stream", True)

            session_id = __metadata__['chat_id']

            # Extract model id from the model name
            model_id = body["model"][body["model"].find(".") + 1 :]

            # Extract messages from the body
            messages = body.get("messages", [])
            if not messages:
                raise Exception("No messages found in request body")

            # Get the current message (last message)
            current_message = messages[-1]
            question = self._process_message_content(current_message)

            # Prepare request payload according to Flowise API format
            data = {
                "question": question,  # Current message
                "overrideConfig": {
                    "sessionId": session_id
                },  # Optional configuration,
                "streaming": stream_enabled
            }

            headers = {
                "Authorization": f"Bearer {self.valves.flowise_api_key}",
                "Content-Type": "application/json",
            }

            print("\nMaking Flowise API request:")
            print(f"URL: {self.valves.flowise_url}")
            print(f"Headers: {headers}")
            print(f"Data: {json.dumps(data, indent=2)}")

            # Make the API request
            r = requests.post(
                url=f"{self.valves.flowise_url}/api/v1/prediction/{model_id}",
                json=data,
                headers=headers
            )
            r.raise_for_status()

            # Return response based on streaming preference
            if stream_enabled:
                for line in r.iter_lines(decode_unicode=True):
                    if line and line.startswith('data:'):
                        try:
                            # Remove 'data:' prefix and parse JSON
                            json_data = line[5:]  # Remove 'data:' prefix
                            response = json.loads(json_data)
                            
                            # Only yield content from token events
                            if isinstance(response, dict) and response.get("event") == "token":
                                token_data = response.get("data", "")
                                if token_data:  # Only yield non-empty tokens
                                    yield token_data
                        except json.JSONDecodeError:
                            # Skip malformed JSON lines
                            continue
            else:
                response = r.json()
                # Only return the text field from the response
                if isinstance(response, dict) and "text" in response:
                    return response["text"]
                return ""

        except Exception as e:
            error_msg = f"Error in Flowise pipe: {str(e)}"
            print(error_msg)
            return error_msg

```

4. After Function has been saved, enable it, and click the settings button to put in your Flowise URL and Flowise API Key:

<figure><img src="../../.gitbook/assets/image (2) (1) (1).png" alt="" width="563"><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (3) (1).png" alt="" width="431"><figcaption></figcaption></figure>

5. Now when you refresh and click New Chat, you will be able to see the list of flows. You can modify the code to show:

* Only Agentflows V2: `f"{self.valves.flowise_url}/api/v1/chatflows?type=AGENTFLOW"`
* Only Chatflows: `f"{self.valves.flowise_url}/api/v1/chatflows?type=CHATFLOW"`
* Only Assistants: `f"{self.valves.flowise_url}/api/v1/chatflows?type=ASSISTANT"`

<figure><img src="../../.gitbook/assets/image (4) (1).png" alt=""><figcaption></figcaption></figure>

6. Test:

<figure><img src="../../.gitbook/assets/image (5).png" alt=""><figcaption></figcaption></figure>
