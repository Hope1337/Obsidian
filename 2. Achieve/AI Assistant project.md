## Quick CLI
- `n8n`: docker run -it --rm --name n8n -p 5678:5678 -v n8n_data:/home/node/.n8n docker.n8n.io/n8nio/n8n
- `openWebUI`: 

## Phase 1: Giao diện chat + n8n

- **Bước 1: Chuẩn bị image cho Open WebUI**: Giao diện chat được sử dụng là [Open WebUI](https://github.com/open-webui/open-webui), chỉ cần image version bình thường (không gpu) do tí nữa dùng API của OpenAI.

- **Bước 2: Chuẩn bị image cho n8n**

- ==Lưu ý==: Trong Docker, mỗi container là một máy tính riêng biệt, do đó URL kiểu `localhost` trong container `A` sẽ không thể truy cập được từ container `B`. Thay vào đó, nếu cả hai container có cùng network thì có thể gọi bằng tên container, tức là `http://n8n:5678` thay vì `http://localhost:5678`.

- **Bước 3: Tạo docker compose để share network**: Do lý do trên nên cần một shared network cho hai container giao tiếp với nhau:
```yml
version: '3.8'

services:
  open-webui:
    image: ghcr.io/open-webui/open-webui:main
    container_name: open-webui
    ports:
      - "3000:8080"
    volumes:
      - open-webui:/app/backend/data
    environment:
      - OPENAI_API_KEY=dummy-key
      - WEBUI_SECRET_KEY=supersecret123
    depends_on:
      - n8n
    networks:
      - shared-net

  n8n:
    image: n8nio/n8n:latest
    container_name: n8n
    ports:
      - "5678:5678"
    volumes:
      - n8n_data:/home/node/.n8n
    environment:
      - GENERIC_TIMEZONE=Asia/Ho_Chi_Minh
      - TZ=Asia/Ho_Chi_Minh
    networks:
      - shared-net

volumes:
  n8n_data:
    external: true
  open-webui:
    external: true
  
networks:
  shared-net:
    driver: bridge
```

- **Bước 4: Tạo Webhook**: webhook là một URL để đợi trigger, khi được trigger thì chạy một pipeline gì đó. Xong rồi thì tạo luôn cái response cho webhook đó là được. Tất nhiên nếu có nhiều webhook cũng cần lưu ý tí về cách nó được response.

- **Bước 5:** tạo function cho open WebUI, do mặc định nó không tích hợp n8n nên ta tận dụng khả năng custom của nó để có thể tích hợp chung với n8n, [source code](https://www.pondhouse-data.com/blog/integrating-n8n-with-open-webui#implementing-the-n8n-integration-pipe):
``` python
"""
title: n8n Pipe Function
author: Cole Medin / Andreas Nigg (Pondhouse Data GmbH)
version: 0.2.0

This module defines a Pipe class that utilizes an N8N workflow for an Agent
"""

from typing import Optional, Callable, Awaitable
from pydantic import BaseModel, Field
import os
import time
import aiohttp
import asyncio


class Pipe:
    class Valves(BaseModel):
        n8n_url: str = Field(
            default="https://n8n.[your domain].com/webhook/[your webhook URL]"
        )
        n8n_bearer_token: str = Field(default="...")
        input_field: str = Field(default="chatInput")
        response_field: str = Field(default="output")
        emit_interval: float = Field(
            default=2.0, description="Interval in seconds between status emissions"
        )
        enable_status_indicator: bool = Field(
            default=True, description="Enable or disable status indicator emissions"
        )

    def __init__(self):
        self.type = "pipe"
        self.id = "n8n_pipe"
        self.name = "N8N Pipe"
        self.valves = self.Valves()
        self.last_emit_time = 0

    async def emit_status(
        self,
        __event_emitter__: Callable[[dict], Awaitable[None]],
        level: str,
        message: str,
        done: bool,
    ):
        current_time = time.time()
        if (
            __event_emitter__
            and self.valves.enable_status_indicator
            and (
                current_time - self.last_emit_time >= self.valves.emit_interval or done
            )
        ):
            await __event_emitter__(
                {
                    "type": "status",
                    "data": {
                        "status": "complete" if done else "in_progress",
                        "level": level,
                        "description": message,
                        "done": done,
                    },
                }
            )
            self.last_emit_time = current_time

    async def make_n8n_request(self, payload: dict) -> dict:
        """Separate async function to handle the N8N API request"""
        headers = {
            "Authorization": f"Bearer {self.valves.n8n_bearer_token}",
            "Content-Type": "application/json",
        }

        async with aiohttp.ClientSession() as session:
            async with session.post(
                self.valves.n8n_url, json=payload, headers=headers
            ) as response:
                if response.status == 200:
                    response_data = await response.json()
                    return response_data[self.valves.response_field]
                else:
                    error_text = await response.text()
                    raise Exception(f"Error: {response.status} - {error_text}")

    async def pipe(
        self,
        body: dict,
        __user__: Optional[dict] = None,
        __metadata__: Optional[dict] = None,
        __event_emitter__: Callable[[dict], Awaitable[None]] = None,
        __event_call__: Callable[[dict], Awaitable[dict]] = None,
    ) -> Optional[dict]:
        n8n_response = None

        try:
            await self.emit_status(
                __event_emitter__, "info", "Calling N8N Workflow...", False
            )

            messages = body.get("messages", [])

            # Verify a message is available
            if messages:
                question = messages[-1]["content"]
                if "Prompt: " in question:
                    question = question.split("Prompt: ")[-1]

                await self.emit_status(
                    __event_emitter__, "info", "Processing request...", False
                )

                # Prepare payload
                payload = {"sessionId": __metadata__["chat_id"]}
                payload[self.valves.input_field] = question
                payload["user"] = __user__

                # Make the API request
                n8n_response = await self.make_n8n_request(payload)

                # Set assistant message with chain reply
                body["messages"].append({"role": "assistant", "content": n8n_response})

                await self.emit_status(
                    __event_emitter__, "info", "Processing response...", False
                )

            else:
                await self.emit_status(
                    __event_emitter__,
                    "error",
                    "No messages found in the request body",
                    True,
                )
                body["messages"].append(
                    {
                        "role": "assistant",
                        "content": "No messages found in the request body",
                    }
                )
                return "No messages found in the request body"

        except Exception as e:
            error_message = f"Error: {str(e)}"
            await self.emit_status(
                __event_emitter__,
                "error",
                error_message,
                True,
            )
            body["messages"].append({"role": "assistant", "content": error_message})
            return {"error": error_message}

        finally:
            await self.emit_status(__event_emitter__, "info", "Complete", True)

        return n8n_response

```

Để ý phần 
```python
input_field: str = Field(default="chatInput")
response_field: str = Field(default="output")
```
Để setting các trường nhận/trả dữ liệu trong n8n cho đúng.