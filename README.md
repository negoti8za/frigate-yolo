# Frigate YOLOv9 ONNX Models

YOLOv9-s pretrained ONNX models exported for [Frigate NVR](https://frigate.video).  
Two variants — one per hardware platform.

---

## Models

### `yolov9_s_640_frigate_cuda.onnx` — NVIDIA GPU

| Setting | Value |
|---|---|
| Model | YOLOv9-s |
| Input size | 640 × 640 px |
| Input tensor | NCHW, float32, RGB |
| Output | `[1, 84, 8400]` (YOLOv8-style: 4 box + 80 COCO classes) |
| Detector type | `onnx` (CUDA execution provider) |
| Frigate image | `ghcr.io/blakeblackshear/frigate:stable` |
| Labelmap | `/labelmap/coco-80.txt` (built-in to Frigate) |
| IR version | 8 (patched for Frigate ORT compatibility) |

Use this on any machine with an Nvidia GPU (RTX, GTX, etc).  
Note: TensorRT support was removed from Frigate amd64; ONNX is the only GPU path.

---

### `yolov9_s_320_frigate_intel.onnx` — Intel iGPU / ARC

| Setting | Value |
|---|---|
| Model | YOLOv9-s |
| Input size | 320 × 320 px |
| Input tensor | NCHW, float32, RGB |
| Output | `[1, 84, 2100]` (YOLOv8-style: 4 box + 80 COCO classes) |
| Detector type | `openvino`, `device: GPU` |
| Frigate image | `ghcr.io/blakeblackshear/frigate:stable` (OpenVINO built-in) |
| Labelmap | `/labelmap/coco-80.txt` (built-in to Frigate) |
| IR version | 8 (patched for Frigate ORT compatibility) |

320×320 is recommended for iGPU — Frigate crops frames before inference so detail loss is minimal.

---

## Usage

1. Copy the `.onnx` file to `/config/model_cache/` on your Frigate host.
2. Merge the matching `.yaml` snippet into your `config.yml`.
3. Restart Frigate.

---

## How these were built

Exported from the official [WongKinYiu/yolov9](https://github.com/WongKinYiu/yolov9) pretrained weights (`yolov9-s-converted.pt`) using `export.py --simplify --include onnx`.  
IR version downgraded from 10 → 8 for Frigate ORT compatibility.  
Verified with `onnxruntime` CPUExecutionProvider before upload.
