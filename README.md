## Dream Data Management

A monorepo of small tools and deployment configs to manage experiment data end‑to‑end:
- MongoDB (Sacred) for experiment metadata
- MinIO (S3-compatible) for raw/heavy files
- Omniboard for visualization
- Desktop utilities for TIFF viewing, ROI selection, and experiment sending

### Repository structure

```
DreamDataManagement/
├─ DataManagementToolDocker/     # Docker Compose: MongoDB, MinIO, Omniboard + docs
├─ DreamROItoSacred/             # ROI selection on videos and send data to Sacred/MinIO
├─ DreamTIFFReader/              # Simple Tkinter GUI to view multi-frame TIFFs
└─ ExperimentSenderSacred/       # GUI to send experiments to Sacred (+ MinIO artifacts/raw data)
```

### Subprojects

- **DataManagementToolDocker**: Local stack for MongoDB + MinIO + Omniboard
  - What it provides: a reproducible local/dev environment to store and visualize experiments.
  - Start here if you need a database and storage running locally.
  - Docs:
    - Deployment: `DataManagementToolDocker/DEPLOY.md`
    - User management: `DataManagementToolDocker/MANAGE_USERS.md`
    - Compose file: `DataManagementToolDocker/docker-compose.yml`

- **ExperimentSenderSacred**: GUI to send experiments to Sacred (and MinIO)
  - What it does: select per-experiment folders, map files (config/results/metrics/artifacts/raw data), then send to MongoDB Sacred and optionally MinIO or a filesystem path.
  - Get started: create/activate a virtual env, `pip install -r requirements.txt`, run `python app.py`.
  - Docs: `ExperimentSenderSacred/README.md`

- **DreamROItoSacred**: ROI selection workflow + sending results to Sacred/MinIO
  - What it does: interactively select ROIs on a video, generate outputs, and send to MongoDB Sacred with optional MinIO upload.
  - Get started: adjust `config/params.py`, then `python graphic_tool.py`.
  - Docs: `DreamROItoSacred/README.md`

- **DreamTIFFReader**: Multi-frame TIFF viewer (Tkinter)
  - What it does: open a TIFF, navigate frames, basic ROI interactions; includes a Windows executable built with PyInstaller.
  - Get started: `pip install tkinterdnd2 tifffile numpy matplotlib`, then `python DreamTIFFReader/main.py`.
  - Docs: `DreamTIFFReader/README.md`

### Quick start

- If you need a local environment (MongoDB + Omniboard + MinIO):
  - See `DataManagementToolDocker/DEPLOY.md` (edit `.env`, review `docker-compose.yml`, then `docker compose up -d`).
  - Optional: create an app-specific MongoDB user and policies for MinIO (see docs).

- If you want to send existing experiments to Sacred/MinIO:
  - Use `ExperimentSenderSacred` → `python app.py` and configure sources (config/results/metrics/artifacts/raw data).

- If you want to select ROIs and produce/send derived data:
  - Use `DreamROItoSacred` → configure `config/params.py`, then `python graphic_tool.py`.

- If you just need to browse a TIFF:
  - Use `DreamTIFFReader` → `python main.py` or the provided executable.

### Requirements (high level)

- Python 3.x (recommended: use a virtual environment)
- pip for Python dependencies
- Optional: Docker Desktop (Windows/macOS) or Docker Engine + Compose (Linux) to run the local data stack

### Useful links (internal)

- Deployment: `DataManagementToolDocker/DEPLOY.md`
- Manage users and access: `DataManagementToolDocker/MANAGE_USERS.md`
- Experiment sender docs: `ExperimentSenderSacred/README.md`
- ROI workflow docs: `DreamROItoSacred/README.md`
- TIFF viewer docs: `DreamTIFFReader/README.md`

### Notes

- Credentials and endpoints for MongoDB/MinIO are configured in the Docker `.env` and per-app configs (e.g., `config/params.py` in `DreamROItoSacred` and UI fields in `ExperimentSenderSacred`).
- Omniboard connects to your MongoDB to visualize runs and artifacts; ensure its connection string matches your database/auth setup.

### License

GNU General Public License v3.0. See `LICENSE` at the repository root.


