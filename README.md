# flux-batch

Batch image editing with FLUX.2 [klein] Q4_K_M GGUF — run the same prompt across many images.

```
flux-batch -i "*.jpg" -o new/ -p prompt.txt
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
flux-batch -i "*.jpg" -o new/ -p prompt.txt
```

### Skeleton ControlNet Mode

Use `--skeleton` to guide generation with detected human poses or custom skeleton images:

```bash
# Auto-detect pose from input images and apply skeleton control
flux-batch --skeleton -i "*.jpg" -o pose_out/ -p prompt.txt

# Pure skeleton lines (no Canny edges)
flux-batch --skeleton --skeleton-mode skeleton -i "*.jpg" -o pose_out/ -p prompt.txt

# Use pre-generated skeleton image (reused for all inputs)
flux-batch --skeleton --skeleton-image pose.png -i "*.jpg" -o out/ -p prompt.txt

# Use globbed skeleton images paired with input images
flux-batch --skeleton --skeleton-image "poses/*.png" -i "*.jpg" -o out/ -p prompt.txt

# Adjust control strength (0.0-1.0)
flux-batch --skeleton --skeleton-strength 0.8 -i "*.jpg" -o out/ -p prompt.txt
```
flux-batch -in <glob> -out <dir> -p <prompt>
```

### Required Flags

| Flag | Description |
|------|-------------|
| `-p` / `--prompt` | Prompt file |
| `-o` / `--out` | Output directory |

### Optional Flags

| Flag | Default | Description |
|------|---------|-------------|
| `-i` / `--in` | — | Glob pattern for input images (e.g. `"*.jpg"`) |
| `-c` / `--count` | `1` | Number of images to make |
| `-d` / `--device` | `cuda` | Torch device |
| `-s` / `--steps` | `4` | Inference steps |
| `-r` / `--ref` | — | Reference image(s) for style/content (can be specified multiple times) |
| `--skeleton` | — | Enable skeleton ControlNet mode for pose-guided generation |
| `--skeleton-mode` | `auto` | Skeleton extraction mode: `auto`, `skeleton`, `edges`, `canny` |
| `--skeleton-strength` | `0.6` | ControlNet conditioning scale (0.0-1.0) |
| `--skeleton-image` | — | Pre-generated skeleton/control image (supports glob patterns) |
| `--controlnet` | `InstantX/FLUX.1-dev-Controlnet-Canny` | ControlNet model |
| `--exact` | false | Use [exact](https://huggingface.co/dx8152/Flux2-Klein-9B-Consistency) text encoder |
| `--nsfw` | false | Use [uncensored](https://huggingface.co/ponpoke/flux2-klein-9b-uncensored-text-encoder) text encoder |
| `-nc` | false | No Clobber — skip existing outputs |

### Text Encoders

By default, flux-batch uses the censored FLUX text encoder. Alternative text encoders can be enabled:

- `--exact`: Uses [dx8152/Flux2-Klein-9B-Consistency](https://huggingface.co/dx8152/Flux2-Klein-9B-Consistency) for exact/prompt-following mode
- `--nsfw`: Uses [ponpoke/flux2-klein-9b-uncensored-text-encoder](https://huggingface.co/ponpoke/flux2-klein-9b-uncensored-text-encoder) for unrestricted content

```bash
# Use exact encoder
flux-batch --exact -i "*.jpg" -o out/ -p prompt.txt

# Use uncensored encoder
flux-batch --nsfw -i "*.jpg" -o out/ -p prompt.txt
```

### Skeleton ControlNet Mode

Use `--skeleton` to guide generation with detected human poses or custom skeleton images:

```bash
# Auto-detect pose from input images and apply skeleton control
flux-batch --skeleton -in "*.jpg" -out pose_out/ -p prompt.txt

# Pure skeleton lines (no Canny edges)
flux-batch --skeleton --skeleton-mode skeleton -in "*.jpg" -out pose_out/ -p prompt.txt

# Use pre-generated skeleton image (reused for all inputs)
flux-batch --skeleton --skeleton-image pose.png -in "*.jpg" -out out/ -p prompt.txt

# Use globbed skeleton images paired with input images
flux-batch --skeleton --skeleton-image "poses/*.png" -in "*.jpg" -out out/ -p prompt.txt

# Adjust control strength (0.0-1.0)
flux-batch --skeleton --skeleton-strength 0.8 -in "*.jpg" -out out/ -p prompt.txt
```

**Skeleton modes:**
- `auto` (default): Renders skeleton lines, then applies Canny edge detection
- `skeleton`: Pose skeleton lines only
- `edges`: Skeleton lines + Canny edges on skeleton
- `canny`: Canny edge detection on original input image

**Notes:**

- The prompt file is reread at each file so you can do realtime modification of the content as you go through a batch.
- Only the first line of the prompt file is read. This allows you manage your prompts however you please.