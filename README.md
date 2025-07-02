# SAM 2: A Lightweight Fork for Easy Integration

[![License: Apache 2.0](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](https://opensource.org/licenses/Apache-2.0)
[![Original Repo](https://img.shields.io/badge/Original-Meta%20AI%20SAM%202-brightgreen)](https://github.com/facebookresearch/sam2)

This repository is a lightweight and user-friendly fork of Meta AI's official **SAM 2** project. The goal of this fork is to provide a simplified interface for developers who want to integrate SAM 2's powerful segmentation capabilities into their own projects without the full research codebase.

## Purpose of this Fork

The primary changes in this version are:

* **Reduced Dependencies:** Removed all code related to training, evaluation, and the Gradio demo to minimize dependencies.
* **Focus on Inference:** This fork is designed exclusively for running segmentation on new images and videos.
* **Added Custom Repository:** Added a custom repository for downloading checkpoints for [culicidaelab](https://github.com/iloncka-ds/culicidaelab) library.
* **Added Symlinks:** Added symlinks to the version 2.1 models configs.

For the full research paper, original implementation, and datasets, please see the [official SAM 2 repository](https://github.com/facebookresearch/sam2).

---

## Installation

You can install this package directly from GitHub:

```bash
pip install "sam2@git+https://github.com/iloncka-ds/sam2.git@v1.2-light"

```

### Download CheckpointsMore actions

First, we need to download a model checkpoint. All the model versions of SAM2.1 checkpoints can be downloaded by running:

```bash
cd checkpoints && \
./download_ckpts.sh && \
cd ..
```

or individually from:

* [sam2.1_hiera_tiny.pt](https://dl.fbaipublicfiles.com/segment_anything_2/092824/sam2.1_hiera_tiny.pt)
* [sam2.1_hiera_small.pt](https://dl.fbaipublicfiles.com/segment_anything_2/092824/sam2.1_hiera_small.pt)
* [sam2.1_hiera_base_plus.pt](https://dl.fbaipublicfiles.com/segment_anything_2/092824/sam2._hiera_base_plus.pt)
* [sam2.1_hiera_large.pt](https://dl.fbaipublicfiles.com/segment_anything_2/092824/sam2.1_hiera_large.pt)

Alternatively, models can also be loaded from [Hugging Face](https://huggingface.co/models?search=facebook/sam2) (requires `pip install huggingface_hub`).

## Quickstart

Here is a simple example of how to segment an object in an image using a point prompt.

```python
import torch
from PIL import Image
from sam2.predictor import SAM2Predictor
from sam2.build_sam import build_sam2

# Load an image
image = Image.open("path/to/your/image.jpg").convert("RGB")

# Define a prompt (e.g., a point in the center of the image)
h, w = image.height, image.width
prompt = {
    "points": torch.tensor([[[w // 2, h // 2]]]), # A point prompt
    "labels": torch.tensor([[1]]) # 1 indicates a foreground point
}

# Initialize the predictor
predictor = SAM2Predictor(
    build_sam2(model_cfg="sam2_hiera_base_plus.yaml", ckpt_path="path/to/sam2_checkpoint.pth")
)

# Set the image
predictor.set_image(image)

# Predict the mask
masks, scores, _ = predictor.predict(
    prompt=prompt,
    multimask_output=False
)

# The output 'masks' is a tensor of the predicted segmentation masks.
print(f"Mask shape: {masks.shape}") # Should be (1, 1, H, W)
```

## Original Project Information

This project is a fork of and builds upon the incredible work done by the **[AI at Meta, FAIR team](https://ai.meta.com/research/)**.

### SAM 2: Segment Anything in Images and Videos

[Nikhila Ravi](https://nikhilaravi.com/), [Valentin Gabeur](https://gabeur.github.io/), [Yuan-Ting Hu](https://scholar.google.com/citations?user=E8DVVYQAAAAJ&hl=en), [Ronghang Hu](https://ronghanghu.com/), [Chaitanya Ryali](https://scholar.google.com/citations?user=4LWx24UAAAAJ&hl=en), [Tengyu Ma](https://scholar.google.com/citations?user=VeTSl0wAAAAJ&hl=en), [Haitham Khedr](https://hkhedr.com/), [Roman Rädle](https://scholar.google.de/citations?user=Tpt57v0AAAAJ&hl=en), [Chloe Rolland](https://scholar.google.com/citations?hl=fr&user=n-SnMhoAAAAJ), [Laura Gustafson](https://scholar.google.com/citations?user=c8IpF9gAAAAJ&hl=en), [Eric Mintun](https://ericmintun.github.io/), [Junting Pan](https://junting.github.io/), [Kalyan Vasudev Alwala](https://scholar.google.co.in/citations?user=m34oaWEAAAAJ&hl=en), [Nicolas Carion](https://www.nicolascarion.com/), [Chao-Yuan Wu](https://chaoyuan.org/), [Ross Girshick](https://www.rossgirshick.info/), [Piotr Dollár](https://pdollar.github.io/), [Christoph Feichtenhofer](https://feichtenhofer.github.io/)

[[`Paper`](https://ai.meta.com/research/publications/sam-2-segment-anything-in-images-and-videos/)] [[`Project`](https://ai.meta.com/sam2)] [[`Demo`](https://sam2.metademolab.com/)] [[`Dataset`](https://ai.meta.com/datasets/segment-anything-video)] [[`Blog`](https://ai.meta.com/blog/segment-anything-2)] [[`BibTeX`](#Citing the Original Work)]

### License

The SAM 2 model checkpoints, SAM 2 demo code (front-end and back-end), and SAM 2 training code are licensed under [Apache 2.0](./LICENSE), however the [Inter Font](https://github.com/rsms/inter?tab=OFL-1.1-1-ov-file) and [Noto Color Emoji](https://github.com/googlefonts/noto-emoji) used in the SAM 2 demo code are made available under the [SIL Open Font License, version 1.1](https://openfontlicense.org/open-font-license-official-text/).

### Citing the Original Work

If you use SAM 2 in your research, please cite the original paper:

```bibtex
@article{ravi2024sam2,
  title={SAM 2: Segment Anything in Images and Videos},
  author={Ravi, Nikhila and Gabeur, Valentin and Hu, Yuan-Ting and Hu, Ronghang and Ryali, Chaitanya and Ma, Tengyu and Khedr, Haitham and R{\"a}dle, Roman and Rolland, Chloe and Gustafson, Laura and Mintun, Eric and Pan, Junting and Alwala, Kalyan Vasudev and Carion, Nicolas and Wu, Chao-Yuan and Girshick, Ross and Doll{\'a}r, Piotr and Feichtenhofer, Christoph},
  journal={arXiv preprint arXiv:2408.00714},
  url={https://arxiv.org/abs/2408.00714},
  year={2024}
}
```
