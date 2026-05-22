# ComfyUI Portable — Quick Start

This quick-start guide shows how to get the portable Windows build running, which files to download, where to place models, and the common commands to launch ComfyUI locally.

**What this package is**
- A self-contained, portable Windows build of ComfyUI (embedded Python + PyTorch for the selected GPU variant).
- Provides a web UI + API to run node-graph workflows such as text→image using Stable Diffusion checkpoints.

**Requirements**
- Windows with up-to-date GPU drivers (for GPU builds).
- Enough disk space for model checkpoints (these files are large).

**Important files**
- `run_cpu.bat` — start in CPU-only mode.
- `run_nvidia_gpu.bat` — start using NVIDIA GPU (portable NVIDIA build).
- `run_nvidia_gpu_fast_fp16_accumulation.bat` — NVIDIA optimized FP16 accumulation mode.
- `python_embeded\python.exe` — the embedded Python runtime used by the scripts.
- `ComfyUI\main.py` — main application entrypoint.
- `extra_model_paths.yaml.example` — example configuration to share models with other UIs.
- `update\update_comfyui.bat` — helper to update the ComfyUI code in this package.

**Downloads**
- Official portable builds (GitHub releases):
	- ComfyUI Windows portable (NVIDIA): https://github.com/comfyanonymous/ComfyUI/releases/latest/download/ComfyUI_windows_portable_nvidia.7z
	- ComfyUI Windows portable (AMD): https://github.com/comfyanonymous/ComfyUI/releases/latest/download/ComfyUI_windows_portable_amd.7z
	- ComfyUI Windows portable (Intel): https://github.com/comfyanonymous/ComfyUI/releases/latest/download/ComfyUI_windows_portable_intel.7z
	- Alternate NVIDIA (CUDA 12.6 / Python 3.12): https://github.com/comfyanonymous/ComfyUI/releases/latest/download/ComfyUI_windows_portable_nvidia_cu126.7z

- ComfyUI core & docs:
	- Repo and releases: https://github.com/comfyanonymous/ComfyUI
	- Documentation & templates: https://docs.comfy.org/

- Useful tools / models:
	- 7-Zip (for extracting .7z): https://www.7-zip.org/
	- Stable Diffusion checkpoint (example v1.5): https://huggingface.co/Comfy-Org/stable-diffusion-v1-5-archive/blob/main/v1-5-pruned-emaonly-fp16.safetensors
	- comfy-cli (optional installer): https://docs.comfy.org/comfy-cli/getting-started

Notes:
- If extracting on Windows, right-click the downloaded archive → Properties → Unblock if extraction fails.
- The portable builds include an embedded Python and a PyTorch build compatible with the target GPU variant. Update drivers if the GPU build fails to start.

**After download — extract & run (step-by-step)**
1. Verify and unblock the downloaded archive (if needed):

```powershell
# If Windows blocks the file, unblock it before extracting
Unblock-File -Path .\ComfyUI_windows_portable_nvidia.7z
```

2. Extract the .7z archive using 7-Zip (GUI) or the command line. Example using 7-Zip CLI:

```powershell
# Extract to the current directory (creates a folder named after the archive)
& 'C:\Program Files\7-Zip\7z.exe' x ComfyUI_windows_portable_nvidia.7z -o.
```

3. Open the extracted folder and confirm the structure: you should see `ComfyUI`, `python_embeded`, `run_cpu.bat`, `run_nvidia_gpu.bat`, and `update`.

4. Place model checkpoints (ckpt / safetensors) into `ComfyUI\models\checkpoints`. Create the folder if it doesn't exist.

5. Start the app from a terminal so you can see logs (recommended). Example commands (run from the extracted package root):

```powershell
# CPU mode (slow but useful for testing)
.\run_cpu.bat

# NVIDIA GPU mode (double-click also works but this shows logs)
.\run_nvidia_gpu.bat

# Advanced: run embedded Python directly
.\python_embeded\python.exe -s ComfyUI\main.py --windows-standalone-build
```

6. If prompted by Windows Firewall, allow the app to accept incoming connections (it's the local web UI server on port 8188).

```powershell
# Optional: create a firewall rule to allow local access to port 8188
New-NetFirewallRule -DisplayName "ComfyUI" -Direction Inbound -Action Allow -Protocol TCP -LocalPort 8188
```

7. Open your browser to `http://localhost:8188` to access the ComfyUI web interface.

8. Load an example blueprint from the `ComfyUI\blueprints` folder (or create a simple text→image workflow) and run it. If outputs fail, confirm a checkpoint is present in `ComfyUI\models\checkpoints`.

9. Updating: to update the core files run `update\update_comfyui.bat`. To also update Python dependencies, run `update\update_comfyui_and_python_dependencies.bat` only if needed.

Troubleshooting quick tips:
- If you see "Torch not compiled with CUDA enabled" — your PyTorch and GPU drivers/CUDA version mismatch; try the CPU script to verify the UI starts.
- If extraction fails or files look corrupt, re-download the .7z and make sure antivirus/quarantine didn't block parts of it.
- Use the terminal output to read errors — double-clicking a `.bat` may close the window before you can read logs.

**Where to put model files**
- Place Stable Diffusion checkpoints or safetensors in `ComfyUI\models\checkpoints`.
- Put VAE files in `ComfyUI\models\vae` if required by the model.
- To share models between this package and another UI, copy and edit `extra_model_paths.yaml.example` to `extra_model_paths.yaml` and add paths.

**How to run (quick commands)**
Open a PowerShell or Command Prompt in the package root and run one of the scripts:

```powershell
.\run_cpu.bat
.\run_nvidia_gpu.bat
.\run_nvidia_gpu_fast_fp16_accumulation.bat
```

Or run the embedded Python directly (advanced):

```powershell
.\python_embeded\python.exe -s ComfyUI\main.py --cpu --windows-standalone-build
# or GPU mode
.\python_embeded\python.exe -s ComfyUI\main.py --windows-standalone-build
```

**Web UI / API**
- Default HTTP port: `8188`. Open your browser to `http://localhost:8188` after the server starts.

**Common runtime flags**
- `--cpu` : force CPU-only execution (very slow for image generation).
- `--enable-manager` : enable ComfyUI-Manager for installing custom nodes (requires extra dependencies).
- `--disable-api-nodes` : disable any paid-api nodes.

Check `ComfyUI\README.md` for full list of supported command-line options.

**Troubleshooting**
- If you see a red error in the UI, confirm you have at least one model checkpoint in `ComfyUI\models\checkpoints`.
- If you get a CUDA/Torch error: update your NVIDIA drivers and ensure the PyTorch CUDA build matches your GPU driver; consider using the CPU script to verify the UI starts.
- Run the `.bat` from a terminal to see logs (double-click hides the log output).

**Updating**
- To update the ComfyUI code only, run `update\update_comfyui.bat`.
- To update the code and Python dependencies, use `update\update_comfyui_and_python_dependencies.bat` only if you need package changes.

**Useful locations**
- Blueprints (prebuilt workflows): `ComfyUI\blueprints` — open them from the UI to load example text→image workflows.
- Custom nodes: `ComfyUI\custom_nodes` — add third-party nodes here.

**Next steps**
- Start the package with the appropriate `run_*.bat` for your hardware.
- Open `http://localhost:8188` and load a blueprint in `ComfyUI\blueprints` such as `txt2img` to explore a text→image workflow.

If you want, I can also update the existing `ComfyUI\README.md` with a condensed Quick Start section or show the contents of the `run_nvidia_gpu.bat` next.

**Example workflow screenshot**
Below is a visual example of a text→image workflow (nodes chained left→right). Save your real screenshot to `assets/workflow_screenshot.png`, `assets/workflow_screenshot.jpg`, or `assets/workflow_screenshot.jpeg` and it will display here on GitHub.

<!-- Supported image formats: .png, .jpg, .jpeg -->
![Text→Image workflow](assets/workflow_screenshot.jpeg)

Note: GitHub will display `assets/workflow_screenshot.png`, `assets/workflow_screenshot.jpg`, or `assets/workflow_screenshot.jpeg`. Save your screenshot with any of those names and the file will render here.
