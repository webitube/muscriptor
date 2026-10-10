# MuScriptor — Agent Guide

## Project Overview

MuScriptor is a multi-instrument music transcription model that converts audio recordings into MIDI and sheet music. It uses a transformer language model trained on music data to identify instruments, notes, and timing.

**Architecture:**
- Python backend with FastAPI server and CLI (Typer)
- React/Vite web frontend with piano roll visualization
- PyTorch transformer model with HuggingFace model hub integration
- MuseScore integration for sheet music engraving

**Key directories:**
- `muscriptor/` — Python package (server, CLI, model, utilities)
- `web/` — React frontend
- `tests/` — Test suite

## Build Commands

### Python Package
```bash
pip install hatch
hatch build
```

### Web Frontend
```bash
cd web
pnpm install
pnpm build          # Production build → ../muscriptor/web_dist/
pnpm dev            # Dev server with hot-reload
```

### Install for Development
```bash
pip install -e ".[dev]"
```

## Running

### Server
```bash
# Quick start (no install)
uvx muscriptor serve

# Windows with GPU
uvx --torch-backend=cu128 muscriptor serve

# With options
muscriptor serve --host 0.0.0.0 --port 8080 --model small --device cuda
```

### CLI
```bash
muscriptor transcribe audio.wav -o output.mid
muscriptor transcribe audio.wav --format sheets --output score/
```

## Testing

```bash
pytest                    # Run all tests
pytest tests/test_cli.py  # Run specific test file
pytest -k "test_name"     # Run tests matching keyword
```

## Code Style

- **Python**: Follow PEP 8. Use type hints. Docstrings for public functions.
- **TypeScript/React**: Use TypeScript strict mode. Functional components with hooks.
- **Formatting**: Run formatters before committing (pre-commit hooks available).
- **Imports**: Sort imports. Group stdlib, third-party, local.

## Pre-commit Hooks

```bash
pre-commit install
```

## Security Considerations

- Model weights are downloaded from HuggingFace; verify `HF_TOKEN` is secure
- Server binds to `127.0.0.1` by default; use `--host 0.0.0.0` only when needed
- Audio files are processed in memory; large files may consume significant RAM
- Sheet music generation spawns MuseScore subprocess; validate input paths

## Commit Messages

Use conventional commits:
```
feat: add new feature
fix: fix bug
docs: update documentation
chore: maintenance task
test: add or update tests
refactor: code refactoring
```

## Pull Request Guidelines

1. Create a feature branch from `main`
2. Make changes and add/update tests
3. Run `pytest` to ensure all tests pass
4. Update documentation if needed
5. Submit PR with clear description of changes

## Large Datasets

- Audio files can be large; use streaming where possible
- Model weights are ~1-2GB; ensure sufficient disk space
- HuggingFace cache is stored in `~/.cache/huggingface/`

## Deployment

See `DEVOPS.md` for Docker and Swarm deployment instructions.

## Troubleshooting

- **GPU not detected**: Use `--torch-backend=cu128` on Windows
- **Intel Mac issues**: Use `--python 3.12` with uvx
- **Sheet music fails**: Ensure MuseScore 4+ is installed and in PATH
- **Model download fails**: Check HuggingFace authentication (`uvx hf auth login`)
