# AI Organoid Counter

Segmentation-based organoid counting for brightfield microscopy images.

This repository contains a desktop-oriented analysis tool for counting organoids from `.tif`, `.png`, and `.jpg` images. The workflow tiles each image, runs a fine-tuned SegFormer segmentation model, reconstructs the prediction, and exports count and measurement summaries for individual images or folders.

Web demo:

- [Hugging Face Space](https://huggingface.co/spaces/ReyaLabColumbia/OrganoidCounter)

Model:

- [ReyaLabColumbia/Segformer_Organoid_Counter_GP](https://huggingface.co/ReyaLabColumbia/Segformer_Organoid_Counter_GP)

## Overview

The installable Python version is intended for heavier lab use than the web demo. It can process groups of folders, run locally with a selected model folder, and use CUDA when available.

Core files:

| File | Purpose |
| --- | --- |
| `Organoid_analyzer_gui.py` | Tkinter interface for selecting images, folders, and model settings. |
| `Organoid_analyzer_AI.py` | Segmentation model loading and inference. |
| `Organoid_analyzer_Zstack.py` | Image and z-stack processing utilities. |
| `launch_gui.sh` | Optional Linux launcher script. |
| `launcher.desktop` | Optional desktop launcher template for Linux. |

## Method

The model was fine-tuned from NVIDIA's SegFormer architecture:

- Base model: [`nvidia/segformer-b3-finetuned-cityscapes-1024-1024`](https://huggingface.co/nvidia/segformer-b3-finetuned-cityscapes-1024-1024)
- Architecture repo: [NVlabs/SegFormer](https://github.com/NVlabs/SegFormer)
- Paper: Xie et al., "SegFormer: Simple and efficient design for semantic segmentation with transformers," arXiv:2105.15203.

Training masks were created in FIJI. Large `1536 x 2048` brightfield images were rotated, flipped, and subdivided into `512 x 512` tiles for model training and inference.

## Setup

Install Python 3 and the required libraries:

- NumPy
- Pandas
- Tkinter
- OpenCV
- Transformers
- Pillow
- Matplotlib

CUDA is recommended for faster inference but is not required.

Download the model files from Hugging Face and place the model folder next to the three analyzer Python files. The model folder should include files such as:

- `model.safetensors`
- `config.json`
- `training_args.bin`

Run the GUI:

```bash
python Organoid_analyzer_gui.py
```

On Windows, the GUI can also be launched by opening `Organoid_analyzer_gui.py` from the project folder. On Linux, update `launch_gui.sh` and `launcher.desktop` with the local paths if you want a clickable desktop launcher.

## Outputs

The tool exports spreadsheet-style summaries for group analysis and image-level results. Example output files and screenshots are attached below.

![Group analysis result example](https://github.com/user-attachments/assets/cd5feeca-e5a1-40db-afbc-6f53d5f71f71)

![Group analysis summary](https://github.com/user-attachments/assets/3efed9ee-5fd2-4b1d-a49f-d6c5fc57bb23)

![Working GUI screenshot](https://github.com/user-attachments/assets/e865662b-b9df-4c27-a349-fe0a876fc4e6)

## Current Limitations

- The tool is tuned for images that fit within `1536 x 2048` pixels before tiling.
- Segmentation quality depends on how closely new images match the training data.
- CUDA improves speed but local CPU inference may be slow on large folders.
- The repository does not currently include a pinned `requirements.txt` or automated tests.
- Fine-tuning scripts and training data are not included in this public repo.

## Future Work

- Add a pinned environment file for reproducible installation.
- Include a small sample image and expected output for smoke testing.
- Add a short usage walkthrough for the desktop GUI.
- Document model classes, post-processing assumptions, and known failure cases.
