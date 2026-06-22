# turbofit

Hardware-fit checker + multi-launcher installer for Hermes-Agent. Uses `llmfit` to verify if a model fits your VRAM/RAM, generates accurate launch strings for **llama.cpp / Ollama / vllm / SGlang**, launches the server detached, and wires it into Hermes as main or auxiliary. Also bundles a curated NVIDIA NIM API list.

## Install

```bash
hermes skills install SouthpawIN/turbofit
~/.hermes/skills/turbofit/scripts/install.sh
source ~/.bashrc
serve install
```

## Supported backends

| Launcher | Binary | Use case |
|----------|--------|----------|
| `llama-cpp` (default) | `llama-server` | GGUF files, custom builds, TurboQuant |
| `ollama` | `ollama serve` | Standard ollama models |
| `vllm` | `vllm serve` | HuggingFace Safetensors, high throughput |
| `sglang` | `python -m sglang.launch_server` | RadixAttention, low-latency |

Future community projects can be added by extending the launcher registry.

## UI choices (`--ui`)

| UI | Command | Use case |
|----|---------|----------|
| `tui` (default) | `hermes --tui` | Terminal session |
| `dashboard` | `hermes dashboard` | Web UI in browser |
| `gateway` | `hermes gateway run` | Discord/Telegram/etc. bot |
| `desktop` | `hermes desktop` | Native desktop app |
| `herm` | `herm` + `hermes --tui` | Both UIs |

## Commands

```bash
# Install / update
serve install [launcher]              # Install (default: llama-cpp)
serve update [launcher|all]           # Update a launcher or all
serve check                           # Show all version status

# Hardware fit
serve fit <model>                     # Run llmfit fit check (default ctx=65536)

# Catalog
serve register <alias> <path>         # Register model
           [--launcher llama-cpp|ollama|vllm|sglang]
           [--port N]
serve catalog                         # Show registered aliases

# Launch
serve <alias>                         # Launch detached, show backend/port/logs
serve string <alias>                  # Print launch string (no launch)
serve stop <alias>                    # Stop a running server
serve list                            # List running servers

# Hermes routing + UI
serve main <alias> [--ui ...]         # Launch + set main + start UI
serve aux <alias> [--ui ...]          # Launch + set aux + start UI
serve herm <alias>                    # Launch + main + herm + hermes
serve herm aux <alias>                # Launch + aux + herm + hermes

# NVIDIA NIM API
serve api list                        # Show curated NIM list
serve api use <rank|api_id> [main|aux]# Wire NIM model into Hermes config
```

## NVIDIA NIM API (curated)

5 models verified 2026-06-22 via build.nvidia.com. **All 5 are FREE** with a `NVIDIA_API_KEY` from `build.nvidia.com` (free signup):

| Rank | Model | Vision | $/in | $/out |
|------|-------|--------|------|-------|
| 1 | DeepSeek V4 Pro | no | FREE | FREE |
| 2 | GLM 5.1 | no | FREE | FREE |
| 3 | DeepSeek V4 Flash | no | FREE | FREE |
| 4 | MiniMax M3 | 👁 yes | FREE | FREE |
| 5 | Nemotron Ultra | 👁 yes | FREE | FREE |

## Enforces a 64K context floor

Every launch string and server uses `ctx_size: 65536` minimum (Hermes-Agent requirement).

## License

MIT