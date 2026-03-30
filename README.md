# Face Recognition Pipeline

This repository contains a face detection + recognition pipeline with optional Triton inference, PostgreSQL-backed embeddings, and utilities for syncing identities, running live/recorded recognition, and downloading camera snapshots.

## What This Project Does
1. Detects faces with RetinaFace.
2. Aligns faces and generates embeddings with ArcFace.
3. Compares embeddings against a PostgreSQL database.
4. Optionally auto-enrolls unknown faces (in `inference_unknown.py`).
5. Saves annotated snapshots and metadata.

## Repository Layout
- `main.py`: Meraki snapshot downloader (CLI).
- `sim.py`: Recognition from RTSP/video file or images directory.
- `inference_unknown.py`: Recognition with auto-enroll for unknowns.
- `sync_data.py`: Sync employee data/images from API into local folders.
- `db_creation.py`: Creates the faces table.
- `setup_postgres.py`: Initializes and starts a local Postgres instance.
- `constants.py`: Central config loaded from `.env`.
- `onnx_runner/`: RetinaFace ONNX inference wrapper.
- `weights/`: Model weights (ArcFace + RetinaFace).

## Prerequisites
- Python 3.10
- (Optional) CUDA for GPU inference
- PostgreSQL (local or remote)
- Conda or pip

## Quick Start (Recommended)
### 1. Create environment
Option A: Conda
```bash
conda env create -f environment.yml
conda activate face_recog
```

Option B: Pip
```bash
python -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
```

### 2. Configure `.env`
Create a `.env` file in the project root. Example:
```bash
# Database
PG_HOST=localhost
PG_PORT=5433
PG_USER=onstak
PG_PASS=
PG_DB=new_db_1
FACES_TABLE=faces

# App
CAMERA_ID=34
SIMILARITY_THRESHOLD=0.40
FRAME_SKIP=1
OUTPUT_VIDEO_PATH=output_fr_video.mp4
SAVE_DIR=faces_recognized

# Model / metadata
DL_MODEL_ID=5
VIDEO_FRAME_START=120
VIDEO_FRAME_END=480
VIDEO_TIME_START=12345678
VIDEO_TIME_END=12345678
VIDEO_EVENT_TYPE_ID=3
VIDEO_ALERT_TYPE_ID=3

# Triton
USE_TRITON=False
TRITON_URL=10.39.110.26:9000
RETINA_FACE_MODEL_NAME=retinaface
ARC_FACE_MODEL_NAME=arcface

# APIs
AUTH_URL=https://api.onstak.io/services/identity/api/v2/
BASE_URL=https://api.onstak.io/services/va-platform/api/v2/

# Meraki snapshot downloader
API_KEY=your_meraki_api_key
```

### 3. Ensure models exist
Place the ONNX models here:
- `weights/retinaface_640x640.onnx`
- `weights/glintr100.onnx`

If your models are in a different location, set:
```bash
RETINA_FACE_MODEL_PATH=/path/to/retinaface_640x640.onnx
ARC_FACE_MODEL_PATH=/path/to/glintr100.onnx
```

### 4. Start or configure PostgreSQL
Option A: Use the provided helper
```bash
python setup_postgres.py
python db_creation.py
```

Option B: Use your own database
Make sure `PG_*` values in `.env` are correct and run:
```bash
python db_creation.py
```

## Running Recognition
### 1. RTSP or video file
The RTSP/video mapping is in `constants.py` under `RTSP_URLS`. Use the key as the camera id.
```bash
python sim.py 1 --display
```

### 2. Process a directory of images
```bash
python sim.py --images-dir final_download_images --display
```

### 3. Auto-enroll unknowns
```bash
python inference_unknown.py 1 --display
```

## Meraki Snapshot Downloader
Download snapshots in a loop and save them to `final_download_images/`.
```bash
python main.py --camera-id 1 --camera-serial Q4EM-APD9-W7NQ
```

Resize to model size (optional):
```bash
python main.py --camera-id 1 --camera-serial Q4EM-APD9-W7NQ --resize 640
```

Adjust interval (seconds between snapshots):
```bash
python main.py --camera-id 1 --camera-serial Q4EM-APD9-W7NQ --interval 1.5
```

## Syncing Identities (Optional)
`sync_data.py` pulls employee images and metadata from the configured APIs.
Usage depends on a job file with access keys. Example:
```bash
python sync_data.py --job path/to/job.json
```

## Notes and Tips
- `SAVE_DIR` is created automatically.
- `SIMILARITY_THRESHOLD` controls recognition strictness.
- `FRAME_SKIP` controls how many frames are skipped between detections.
- For CPU-only usage, set `USE_TRITON=False` and install `onnxruntime` instead of `onnxruntime-gpu`.

## Troubleshooting
1. `Missing API_KEY`  
Add `API_KEY=...` to `.env`.

2. `Cannot open` RTSP/Video  
Check `RTSP_URLS` in `constants.py` and verify the stream URL or file path.

3. Model errors  
Ensure the ONNX files are present and paths are correct.

4. Database errors  
Confirm `PG_HOST`, `PG_PORT`, `PG_USER`, and `PG_DB` in `.env`.  
Run `python setup_postgres.py` and `python db_creation.py`.

## Common Commands
```bash
# Start recognition (camera id 1)
python sim.py 1 --display

# Auto-enroll unknowns
python inference_unknown.py 1 --display

# Download snapshots from Meraki
python main.py --camera-id 1 --camera-serial Q4EM-APD9-W7NQ --resize 640
```
