# Ollama on Windows

Juan Ortega, Distinguished Solutions Architect | Cyber Ops. R&amp;D

Arete Advisors, LLC

jortega@areteir.com

## Ollama Installation

**Download the Ollama installation file and run through the setup wizard**

**[https://ollama.com/download](https://ollama.com/download)**

[![image.png](https://wiki.loveisblind.app/uploads/images/gallery/2024-07/scaled-1680-/i6Ximage.png)](https://wiki.loveisblind.app/uploads/images/gallery/2024-07/i6Ximage.png)

**Verify Ollama is running by checking the dock**

[![image.png](https://wiki.loveisblind.app/uploads/images/gallery/2024-07/scaled-1680-/Z1Himage.png)](https://wiki.loveisblind.app/uploads/images/gallery/2024-07/Z1Himage.png)

**Verify how much vram your dedicated graphics card has**

[![image.png](https://wiki.loveisblind.app/uploads/images/gallery/2024-07/scaled-1680-/W8Wimage.png)](https://wiki.loveisblind.app/uploads/images/gallery/2024-07/W8Wimage.png)

**Download a model, please note that model performance is determined by your dedicated graphics card (vram) and ram**

- Browse [https://ollama.com/library](https://ollama.com/library) for a compatible model for your system. Larger models are better but most modern systems are not powerful enough to run them.

[![image.png](https://wiki.loveisblind.app/uploads/images/gallery/2024-07/scaled-1680-/w1Jimage.png)](https://wiki.loveisblind.app/uploads/images/gallery/2024-07/w1Jimage.png)

[![image.png](https://wiki.loveisblind.app/uploads/images/gallery/2024-07/scaled-1680-/8ETimage.png)](https://wiki.loveisblind.app/uploads/images/gallery/2024-07/8ETimage.png)

- Download llama3. This model is small and should work on most systems

[![image.png](https://wiki.loveisblind.app/uploads/images/gallery/2024-07/scaled-1680-/BLDimage.png)](https://wiki.loveisblind.app/uploads/images/gallery/2024-07/BLDimage.png)

## Interaction with the ollama API

While ollama works perfectly fine via the command line, there's a local API that's accessible via 'localhost' which can be leveraged to locally develop code. Open up your favorite IDE, the following examples are in python but feel free to use your programming language of choice.

**REST API Example**

```python
import requests
import json
from argparse import ArgumentParser


def generate_answer(prompt):
    url = "http://localhost:11434/api/generate"
    headers = {
        'Content-Type': 'application/json'
    }
    payload = json.dumps({
        "model": "llama3",
        "prompt": prompt,
        "stream": False
    })
    response = requests.post(url, headers=headers, data=payload)
    print(response.text)


def main():
    parser = ArgumentParser()
    parser.add_argument("--prompt", help="The prompt to use for the API request.")
    args = parser.parse_args()
    if not args.prompt:
        generate_answer("Why is the sky blue?")
    else:
        generate_answer(args.prompt)


if __name__ == "__main__":
    main()


```

**Ollama Python Library** ([https://github.com/ollama/ollama-python)](https://github.com/ollama/ollama-python))

The Ollama library allows you to easily implement steaming of responses rather than waiting for the whole generation of the response before returning to the client/user.

```python
from ollama import Client
from argparse import ArgumentParser


def generate_answer(prompt):
    client = Client(host='http://localhost:11434')

    stream = client.chat(
        model='llama3',
        messages=[{'role': 'user', 'content': f'{prompt}'}],
        stream=True,
    )

    for chunk in stream:
        print(chunk['message']['content'], end='', flush=True)


def main():
    parser = ArgumentParser()
    parser.add_argument("--prompt", help="The prompt to use for the API request.")
    args = parser.parse_args()
    if not args.prompt:
        generate_answer(prompt="Why is the sky blue?")
    else:
        generate_answer(prompt=args.prompt)


if __name__ == "__main__":
    main()


```

## OpenWebUI

Many opensource projects have been created to add a face / interface to the ollama API. The best one is Open WebUI. Open WebUI is an extensible, feature-rich, and user-friendly self-hosted WebUI designed to operate entirely offline. It supports various LLM runners, including Ollama and OpenAI-compatible APIs.

- 🚀 **Effortless Setup**: Install seamlessly using Docker or Kubernetes (kubectl, kustomize or helm) for a hassle-free experience with support for both `:ollama` and `:cuda` tagged images.
- 🤝 **Ollama/OpenAI API Integration**: Effortlessly integrate OpenAI-compatible APIs for versatile conversations alongside Ollama models. Customize the OpenAI API URL to link with **LMStudio, GroqCloud, Mistral, OpenRouter, and more**.
- 🧩 **Pipelines, Open WebUI Plugin Support**: Seamlessly integrate custom logic and Python libraries into Open WebUI using [Pipelines Plugin Framework](https://github.com/open-webui/pipelines). Launch your Pipelines instance, set the OpenAI URL to the Pipelines URL, and explore endless possibilities. [Examples](https://github.com/open-webui/pipelines/tree/main/examples) include **Function Calling**, User **Rate Limiting** to control access, **Usage Monitoring** with tools like Langfuse, **Live Translation with LibreTranslate** for multilingual support, **Toxic Message Filtering** and much more.
- 📱 **Responsive Design**: Enjoy a seamless experience across Desktop PC, Laptop, and Mobile devices.
- 📱 **Progressive Web App (PWA) for Mobile**: Enjoy a native app-like experience on your mobile device with our PWA, providing offline access on localhost and a seamless user interface.
- ✒️🔢 **Full Markdown and LaTeX Support**: Elevate your LLM experience with comprehensive Markdown and LaTeX capabilities for enriched interaction.
- 🎤📹 **Hands-Free Voice/Video Call**: Experience seamless communication with integrated hands-free voice and video call features, allowing for a more dynamic and interactive chat environment.
- 🛠️ **Model Builder**: Easily create Ollama models via the Web UI. Create and add custom characters/agents, customize chat elements, and import models effortlessly through [Open WebUI Community](https://openwebui.com/) integration.
- 🐍 **Native Python Function Calling Tool**: Enhance your LLMs with built-in code editor support in the tools workspace. Bring Your Own Function (BYOF) by simply adding your pure Python functions, enabling seamless integration with LLMs.
- 📚 **Local RAG Integration**: Dive into the future of chat interactions with groundbreaking Retrieval Augmented Generation (RAG) support. This feature seamlessly integrates document interactions into your chat experience. You can load documents directly into the chat or add files to your document library, effortlessly accessing them using the `#` command before a query.
- 🔍 **Web Search for RAG**: Perform web searches using providers like `SearXNG`, `Google PSE`, `Brave Search`, `serpstack`, `serper`, `Serply`, `DuckDuckGo` and `TavilySearch` and inject the results directly into your chat experience.
- 🌐 **Web Browsing Capability**: Seamlessly integrate websites into your chat experience using the `#` command followed by a URL. This feature allows you to incorporate web content directly into your conversations, enhancing the richness and depth of your interactions.
- 🎨 **Image Generation Integration**: Seamlessly incorporate image generation capabilities using options such as AUTOMATIC1111 API or ComfyUI (local), and OpenAI's DALL-E (external), enriching your chat experience with dynamic visual content.
- ⚙️ **Many Models Conversations**: Effortlessly engage with various models simultaneously, harnessing their unique strengths for optimal responses. Enhance your experience by leveraging a diverse set of models in parallel.
- 🔐 **Role-Based Access Control (RBAC)**: Ensure secure access with restricted permissions; only authorized individuals can access your Ollama, and exclusive model creation/pulling rights are reserved for administrators.
- 🌐🌍 **Multilingual Support**: Experience Open WebUI in your preferred language with our internationalization (i18n) support. Join us in expanding our supported languages! We're actively seeking contributors!

### **Install Docker**

**Enable WSL Support**

[![image.png](https://wiki.loveisblind.app/uploads/images/gallery/2024-07/scaled-1680-/Ie0image.png)](https://wiki.loveisblind.app/uploads/images/gallery/2024-07/Ie0image.png)

**Reboot and wait for the system to come back online. Install Ubuntu via the Window Store**

[![image.png](https://wiki.loveisblind.app/uploads/images/gallery/2024-07/scaled-1680-/bKJimage.png)](https://wiki.loveisblind.app/uploads/images/gallery/2024-07/bKJimage.png)

**Open Ubuntu Command line and run through initial setup**

[![image.png](https://wiki.loveisblind.app/uploads/images/gallery/2024-07/scaled-1680-/qulimage.png)](https://wiki.loveisblind.app/uploads/images/gallery/2024-07/qulimage.png)

**Run Docker Installer**

[![image.png](https://wiki.loveisblind.app/uploads/images/gallery/2024-07/scaled-1680-/YX8image.png)](https://wiki.loveisblind.app/uploads/images/gallery/2024-07/YX8image.png)

**Open Docker, close any active cmd.exe, powershell, or terminal windows**

[![image.png](https://wiki.loveisblind.app/uploads/images/gallery/2024-07/scaled-1680-/VKoimage.png)](https://wiki.loveisblind.app/uploads/images/gallery/2024-07/VKoimage.png)

**Validate Docker is running via WSL**

[![image.png](https://wiki.loveisblind.app/uploads/images/gallery/2024-07/scaled-1680-/vcRimage.png)](https://wiki.loveisblind.app/uploads/images/gallery/2024-07/vcRimage.png)

**Open terminal and download/run Open WebUI**

```powershell
docker run -d -p 3000:8080 -e WEBUI_NAME=UCB -e WEBUI_SECRET_KEY=makeyourownrandomsecretkeyIMPORTANT -e ENABLE_IMAGE_GENERATION=true -e IMAGE_GENERATION_ENGINE=openai -e IMAGE_SIZE=1024x1024 -e IMAGE_GENERATION_MODEL=dall-e-3 -e ENABLE_ADMIN_EXPORT=false -e ENABLE_SIGNUP=true -e WEBUI_URL=https://ai.example.com/ -e OPENAI_API_KEY=sk-youapikeyforopenai -e OLLAMA_BASE_URL=http://host.docker.internal:11434 -v open-webui:/app/backend/data --name open-webui --restart always ghcr.io/open-webui/open-webui:main
```

[![image.png](https://wiki.loveisblind.app/uploads/images/gallery/2024-07/scaled-1680-/MQiimage.png)](https://wiki.loveisblind.app/uploads/images/gallery/2024-07/MQiimage.png)

[![image.png](https://wiki.loveisblind.app/uploads/images/gallery/2024-07/scaled-1680-/ytAimage.png)](https://wiki.loveisblind.app/uploads/images/gallery/2024-07/ytAimage.png)

***Unix Script to update open WebUI***

```bash
#!/bin/bash

# Delete old images
docker rmi $(sudo docker images -q)

# Download Latest Image
docker pull ghcr.io/open-webui/open-webui:main

# Find the container ID of "/open-webui"
container_id=$(docker ps  -aqf "name=/open-webui")

if [ -n "$container_id" ]; then
    # Container with name "/open-webui" exists, stop and remove it
    echo "Stopping and removing existing /open-webui container"
    docker rm $container_id -f
fi

sudo docker run -d -p 3000:8080 -e WEBUI_NAME=UCB -e WEBUI_SECRET_KEY=makeyourownrandomsecretkeyIMPORTANT -e ENABLE_IMAGE_GENERATION=true -e IMAGE_GENERATION_ENGINE=openai -e IMAGE_SIZE=1024x1024 -e IMAGE_GENERATION_MODEL=dall-e-3 -e ENABLE_ADMIN_EXPORT=false -e ENABLE_SIGNUP=false -e WEBUI_URL=https://ai.example.com/ -e OPENAI_API_KEY=sk-youapikeyforopenai -e OLLAMA_BASE_URL=http://127.0.0.1:11434 -v open-webui:/app/backend/data --name open-webui --restart always ghcr.io/open-webui/open-webui:main
```
