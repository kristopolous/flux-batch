# flux-batch

Batch image editing with FLUX.2 [klein] — run the same prompt across many images.

```
flux-batch -in "*.jpg" -out new/ 'repaint this as a disney cartoon'
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
