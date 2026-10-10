# MuScriptor DevOps Guide

## Building

### Python Package

The Python package is built with Hatchling. To build a wheel and sdist:

```bash
pip install hatch
hatch build
```

This produces artifacts in `dist/`. The wheel includes the built web frontend (see below).

### Web Frontend

The web UI is a React/Vite app in the `web/` directory. It must be built before packaging or serving.

```bash
cd web
pnpm install
pnpm build
```

The build output goes to `../muscriptor/web_dist/`, which is included in the Python wheel via `[tool.hatch.build] artifacts`.

To run the dev server with hot-reload (proxies API calls to `http://127.0.0.1:8222`):

```bash
cd web
pnpm dev
```

## Running

### Quick Start (no install)

Use `uvx` to run without installing:

```bash
# Linux / macOS (Apple Silicon)
uvx muscriptor serve

# Windows (GPU)
uvx --torch-backend=cu128 muscriptor serve

# macOS (Intel)
uvx --python 3.12 muscriptor serve
```

### Install and Run

```bash
pip install muscriptor
muscriptor serve
```

### Server Options

```bash
muscriptor serve --host 0.0.0.0 --port 8080 --model small --device cuda
```

| Option | Description |
|--------|-------------|
| `--host` | Bind address (default: `127.0.0.1`) |
| `--port` | Port (default: `8222`) |
| `--model` / `-m` | Model size (`small`, `medium`, `large`), local path, or URL |
| `--device` / `-d` | Device (`auto`, `cpu`, `cuda`, `cuda:0`, `mps`) |
| `--dtype` | Transformer dtype (`float32`, `float16`, `bfloat16`) |

### CLI Transcription

```bash
muscriptor transcribe audio.wav -o output.mid
muscriptor transcribe audio.wav --format sheets --output score/
```

## Docker

A `Dockerfile` is provided for containerized deployment:

```bash
docker build -t muscriptor .
docker run -p 8222:8222 muscriptor
```

## Swarm Deployment

A `swarm.yml` file is provided for Docker Swarm:

```bash
docker stack deploy -c swarm.yml muscriptor
```

## Testing

```bash
pip install -e ".[dev]"
pytest
```

## Pre-commit Hooks

```bash
pip install -e ".[dev]"
pre-commit install
```

## HuggingFace Authentication

Model weights are downloaded from HuggingFace on first use. Users must:

1. Accept the CC BY-NC 4.0 license on the model page
2. Authenticate with `uvx hf auth login` or set `HF_TOKEN`

## Dependencies

- **Python**: >= 3.10
- **PyTorch**: >= 2.3 (>= 2.0, < 2.3 on Intel Macs)
- **MuseScore 4+**: Required for sheet music generation (optional)
- **Node.js / pnpm**: Required for building the web frontend
