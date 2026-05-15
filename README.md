# flux-batch

Batch image editing with FLUX.2 [klein] Q4_K_M GGUF — run the same prompt across many images.

```
flux-batch -in "*.jpg" -out new/ 'repaint this as a disney cartoon'
```

## Requirements

- GPU (tested on RTX 4090+)
- Accept the [FLUX Non-Commercial License](https://huggingface.co/black-forest-labs/FLUX.2-klein-9B) on Hugging Face
- Python 3.10+

## Install

```
pip install -r requirements.txt
```

## Usage

```
flux-batch -in <glob> -out <dir> -prompt <prompt>
```

| Flag | Default | Description |
|------|---------|-------------|
| `-in` | — | Glob pattern for input images (e.g. `"*.jpg"`) |
| `-out` | — | Output directory |
| `--device` | `cuda` | Torch device |
| `--count` | `1` | Number of images to make |
| `--steps` | `4` | Inference steps |

**Notes:** 

 * The prompt file is reread at each file so you can do realtime modification of the content as you go through a batch. 
 * Only the first line of the prompt file is read. This allows you manage your prompts however you please.
