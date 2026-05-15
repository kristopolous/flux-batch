# flux-batch

Batch image editing with FLUX.2 [klein] — run the same prompt across many images.

```
flux-batch -in "*.jpg" -out new/ 'repaint this as a disney cartoon'
```

## Requirements

- GPU with ~29 GB VRAM (RTX 4090+)
- Accept the [FLUX Non-Commercial License](https://huggingface.co/black-forest-labs/FLUX.2-klein-9B) on Hugging Face
- Python 3.10+

## Install

```
pip install -r requirements.txt
```

## Usage

```
flux-batch -in <glob> -out <dir> <prompt>
```

| Flag | Default | Description |
|------|---------|-------------|
| `-in` | — | Glob pattern for input images (e.g. `"*.jpg"`) |
| `-out` | — | Output directory |
| `--model` | `black-forest-labs/FLUX.2-klein-9B` | HF model ID or local path |
| `--device` | `cuda` | Torch device |
| `--dtype` | `bfloat16` | `bfloat16`, `float16`, or `float32` |
| `--steps` | `4` | Inference steps |
| `--guidance` | `3.0` | Guidance scale |
| `--seed` | random | Seed for reproducibility |
| `--no-offload` | (offload on) | Keep model on GPU |
# flux-batch
