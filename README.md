# **Qwen-Image-Edit-2509-LoRAs-Fast**

## Overview

> Qwen-Image-Edit-2509-LoRAs-Fast is a high-performance, user-friendly web application built with Gradio that leverages the advanced Qwen/Qwen-Image-Edit-2509 model from Hugging Face for seamless image editing tasks. This app specializes in rapid, specialized edits using lightweight LoRA (Low-Rank Adaptation) adapters, enabling users to transform photos into anime styles, adjust camera angles for multi-view generation, restore lighting by removing harsh shadows, or relight scenes with custom illumination—all in just a few inference steps (as low as 4) for near-instant results. Powered by a custom SteelBlueTheme for an intuitive interface, the app automatically resizes input images to optimal dimensions (multiples of 8 for efficient diffusion processing) while preserving aspect ratios, and supports seed randomization for creative variations. Whether you're a content creator experimenting with artistic styles, a photographer fine-tuning lighting, or a developer prototyping AI edits, this tool democratizes state-of-the-art image manipulation with minimal setup, running efficiently on CUDA-enabled GPUs via Diffusers and PEFT libraries. Explore predefined examples for quick starts, or dive into advanced settings like guidance scale and step counts to fine-tune outputs, all hosted on Hugging Face Spaces for easy deployment and sharing.

<img width="1242" height="788" alt="b-XZNdCXcLSnRltdgPwEd" src="https://github.com/user-attachments/assets/0863eb56-f0e0-479c-b8c4-0449da29bccf" />
<img width="1270" height="782" alt="7Pzyvt2v1IltnqVSjOMQE" src="https://github.com/user-attachments/assets/8c12f1b8-54c4-48aa-b63e-85d3e23f3063" />
<img width="1243" height="783" alt="33RDAh7xG0e8mNeBUVsUy" src="https://github.com/user-attachments/assets/310c0dd4-aa16-45ef-8e87-724395746b40" />

## Features

- **Specialized LoRA Adapters**: Choose from four pre-loaded adapters:
  - **Photo-to-Anime**: Converts real-world photos into vibrant anime artwork.
  - **Multiple-Angles**: Rotates or switches camera perspectives (e.g., 45° left, top-down, wide-angle) for dynamic multi-view edits.
  - **Light-Restoration**: Removes unwanted shadows and artifacts for cleaner, evenly lit images.
  - **Relight**: Applies custom lighting effects, such as soft golden-hour filters or diffused illumination.
  
- **Fast Inference**: Optimized for speed with FlowMatchEulerDiscreteScheduler, bfloat16 precision, and as few as 4 steps—ideal for real-time prototyping.

- **User-Friendly Interface**: 
  - Drag-and-drop image upload with automatic resizing.
  - Text prompt for precise edits (e.g., "transform into anime" or "rotate camera 180° upside down").
  - Advanced accordion for sliders on seed, guidance scale (1.0–10.0), and steps (1–50).
  - Built-in examples showcasing diverse use cases.

- **Technical Optimizations**:
  - Double-stream attention processor (QwenDoubleStreamAttnProcessorFA3) for enhanced efficiency.
  - Negative prompting to avoid common artifacts (e.g., blurriness, extra digits).
  - CUDA device detection and multi-GPU support via `device_map='cuda'`.

- **Deployment-Ready**: Integrates with Hugging Face Spaces for GPU-accelerated hosting, with progress tracking and error handling.

## Installation

To run this app locally or in a custom environment:

1. Clone the repository:
   ```
   git clone https://github.com/PRITHIVSAKTHIUR/Qwen-Image-Edit-2509-LoRAs-Fast.git
   cd Qwen-Image-Edit-2509-LoRAs-Fast
   ```

2. Create a virtual environment (recommended):
   ```
   python -m venv venv
   source venv/bin/activate  # On Windows: venv\Scripts\activate
   ```

3. Install dependencies:
   ```
   pip install -r requirements.txt
   ```

4. Download required models and LoRAs (handled automatically on first run via `from_pretrained`):
   - Base model: `Qwen/Qwen-Image-Edit-2509`
   - Transformer: `linoyts/Qwen-Image-Edit-Rapid-AIO` (subfolder: `transformer`)
   - LoRAs:
     - `autoweeb/Qwen-Image-Edit-2509-Photo-to-Anime` (`Qwen-Image-Edit-2509-Photo-to-Anime_000001000.safetensors`)
     - `dx8152/Qwen-Edit-2509-Multiple-angles` (`镜头转换.safetensors`)
     - `dx8152/Qwen-Image-Edit-2509-Light_restoration` (`移除光影.safetensors`)
     - `dx8152/Qwen-Image-Edit-2509-Relight` (`Qwen-Edit-Relight.safetensors`)

5. Launch the app:
   ```
   python app.py  # Assuming the main script is saved as app.py
   ```
   The Gradio interface will open at `http://127.0.0.1:7860`.

### Requirements

Install the following packages via pip (full `requirements.txt` below):

- `git+https://github.com/huggingface/accelerate.git`
- `git+https://github.com/huggingface/diffusers.git`
- `git+https://github.com/huggingface/peft.git`
- `huggingface_hub`
- `sentencepiece`
- `transformers`
- `torchvision`
- `kernels`  (Note: This may refer to optimized kernels like xformers; install via `pip install xformers` if needed for attention efficiency)
- `spaces`
- `torch`
- `numpy`
- Additional implicit deps: `gradio`, `PIL` (Pillow), `qwenimage` (custom from repo)

**requirements.txt**:
```
git+https://github.com/huggingface/accelerate.git
git+https://github.com/huggingface/diffusers.git
git+https://github.com/huggingface/peft.git
huggingface_hub
sentencepiece
transformers
torchvision
kernels
spaces
torch
numpy
gradio
Pillow
```

**Hardware Notes**:
- GPU recommended (NVIDIA with CUDA 11.8+ for optimal performance).
- ~8GB VRAM minimum for bfloat16 mode.
- On CPU, inference will be slower; set `device='cpu'` in code.

## Usage

1. **Upload an Image**: Drag a photo (JPG/PNG) into the input field. It auto-resizes to 1024px max dimension (aspect-preserved, 8px multiples).

2. **Enter Prompt**: Describe the edit, e.g.:
   - Anime: "Transform into anime style with vibrant colors."
   - Angles: "Switch to top-down view."
   - Lighting: "Apply soft morning light from the left."

3. **Select Adapter**: Dropdown for LoRA style (default: Photo-to-Anime).

4. **Tune Advanced Settings** (optional):
   - Randomize seed for variations.
   - Adjust guidance scale (higher = stricter prompt adherence).
   - Increase steps for higher quality (but slower).

5. **Run**: Click "Run" to generate. Output appears alongside.

## Troubleshooting

- **CUDA Errors**: Ensure `torch.cuda.is_available()` returns True. Check `nvidia-smi` for GPU usage.
- **Model Download Fails**: Verify Hugging Face token if gated models are accessed.
- **Slow Inference**: Reduce steps or use fewer adapters; enable `torch.backends.cudnn.benchmark = True`.
- **Out-of-Memory**: Lower resolution or batch size (single-image mode here).

## Repository

- GitHub: [https://github.com/PRITHIVSAKTHIUR/Qwen-Image-Edit-2509-LoRAs-Fast.git](https://github.com/PRITHIVSAKTHIUR/Qwen-Image-Edit-2509-LoRAs-Fast.git)
- Hugging Face Space🤗: [Qwen-Image-Edit-2509-LoRAs-Fast](https://huggingface.co/spaces/prithivMLmods/Qwen-Image-Edit-2509-LoRAs-Fast)
