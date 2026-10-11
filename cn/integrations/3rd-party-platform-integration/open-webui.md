# 打开网页界面

[开放 WebUI](https://github.com/open-webui/open-webui) 是一个可扩展、功能丰富且用户友好的_自托管 AI 平台_，旨在完全离线运行。

[函数](https://docs.openwebui.com/features/plugin/functions/) 就像 Open WebUI 的插件。我们可以创建一个自定义 [Pipe Function](https://docs.openwebui.com/features/plugin/functions/pipe)，在将结果返回给用户之前，通过调用 Flowise 预测 API 来处理输入并生成响应。通过此，Flowise 可以在 Open WebUI 中使用。

## 设置

1. 首先，启动并运行 Open WebUI，您可以参考[快速入门](https://docs.openwebui.com/getting-started/quick-start/) 指南。从左下角，单击您的个人资料和**管理面板**

<figure><img src="../../.gitbook/assets/image (4) (1).png" alt="" width="235"><figcaption></figcaption></figure>

2. 打开 **函数** 选项卡，然后添加一个新函数。

<figure><img src="../../.gitbook/assets/image (1) (1) (1) (1) (1) (1).png" alt="" width="423"><figcaption></figcaption></figure>

3. 命名该函数，并添加以下代码：

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
                        "name": str(e),
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
    ):
        try:
            stream_enabled = body.get("stream", True)
            session_id = (__metadata__ or {}).get("chat_id") or "owui-session"
            # model can be "flowise.<id>" or just "<id>"
            model_name = body.get("model", "")
            dot = model_name.find(".")
            model_id = model_name[dot + 1 :] if dot != -1 else model_name

            messages = body.get("messages") or []
            if not messages:
                raise Exception("No messages found in request body")
            question = self._process_message_content(messages[-1])

            data = {
                "question": question,
                "overrideConfig": {"sessionId": session_id},
                "streaming": stream_enabled,
            }

            headers = {
                "Authorization": f"Bearer {self.valves.flowise_api_key}",
                "Content-Type": "application/json",
                "Accept": "text/event-stream" if stream_enabled else "application/json",
            }

            url = f"{self.valves.flowise_url}/api/v1/prediction/{model_id}"
            with requests.post(
                url, json=data, headers=headers, stream=stream_enabled, timeout=60
            ) as r:
                r.raise_for_status()

                if stream_enabled:
                    # Ensure correct decoding for SSE (prevents â€™ etc.)
                    r.encoding = "utf-8"

                    for raw_line in r.iter_lines(decode_unicode=True):
                        if not raw_line:
                            continue
                        line = raw_line.strip()

                        # Skip keep-alives or non-data fields
                        if not line.startswith("data:"):
                            continue

                        payload = line[5:].strip()
                        if payload in ("[DONE]", '"[DONE]"'):
                            break

                        # Flowise usually sends {"event":"token","data":"..."}
                        try:
                            obj = json.loads(payload)
                        except json.JSONDecodeError:
                            # Occasionally plain text arrives—stream it anyway
                            if payload:
                                yield payload
                            continue

                        if isinstance(obj, dict):
                            if obj.get("event") == "token":
                                token = obj.get("data") or ""
                                if token:
                                    yield token
                            else:
                                # Some versions send {"data":{"text":"..."}}
                                data_field = obj.get("data")
                                if isinstance(data_field, dict):
                                    text = data_field.get("text")
                                    if text:
                                        yield text
                    return  # end streaming

                # Non-streaming fallback
                resp = r.json()
                return (
                    resp.get("text") or (resp.get("data") or {}).get("text", "") or ""
                )

        except requests.HTTPError as http_err:
            try:
                detail = http_err.response.text[:500]
            except Exception:
                detail = ""
            return f"HTTP error from Flowise: {http_err.response.status_code} {detail}"
        except Exception as e:
            return f"Error in Flowise pipe: {e}"
```

4. 保存功能后，启用它，然后单击设置按钮，输入您的 Flowise URL 和 Flowise API 密钥：

<figure><img src="../../.gitbook/assets/image (2) (1) (1) (1) (1).png" alt="" width="563"><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (3) (1) (1).png" alt="" width="431"><figcaption></figcaption></figure>

5. 现在，当您刷新并单击“新建聊天”时，您将能够看到流程列表。您可以修改代码以显示：

* 仅 智能体流程 V2：`f"{self.valves.flowise_url}/api/v1/chatflows?type=AGENTFLOW"`
* 仅聊天流：`f"{self.valves.flowise_url}/api/v1/chatflows?type=CHATFLOW"`
* 仅助理：`f"{self.valves.flowise_url}/api/v1/chatflows?type=ASSISTANT"`

<figure><img src="../../.gitbook/assets/image (4) (1) (1).png" alt=""><figcaption></figcaption></figure>

6、测试：

<figure><img src="../../.gitbook/assets/image (5) (1).png" alt=""><figcaption></figcaption></figure>
