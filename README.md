# ERPNext v16 on Docker (Windows + VS Code)

Local test setup for ERPNext v16. Not for production.

## Requirements
- Windows 10/11 with WSL2
- Docker Desktop running ("Engine running")
- VS Code with the **Docker** and **WSL** extensions (VS Code will suggest them)

## Run it
1. Unzip this folder (best place: inside WSL, e.g. `\\wsl$\Ubuntu\home\<you>\erpnext-docker`).
2. In VS Code: **File → Open Folder** → select `erpnext-docker`.
3. Press **Ctrl + Shift + B** → runs **ERPNext: Start**.
4. First run only: **Terminal → Run Task → ERPNext: Watch site creation (first run)**.
   Wait until it finishes (3–5 min after images download), then press Ctrl + C.
5. **Terminal → Run Task → ERPNext: Open in browser** (or go to http://localhost:8080).
6. Log in: **Administrator** / **admin** (the `ADMIN_PASSWORD` in `.env`).

Or from the VS Code terminal (Ctrl + `):
```bash
docker compose up -d
docker compose logs -f create-site
```

## Tasks (Terminal → Run Task)
| Task | What it does |
|---|---|
| ERPNext: Start | Start all containers |
| ERPNext: Watch site creation | Follow first-run site setup |
| ERPNext: Status | List containers |
| ERPNext: All logs | Follow all logs |
| ERPNext: Open in browser | Open http://localhost:8080 |
| ERPNext: Stop (keep data) | Stop, data kept |
| ERPNext: Backup site | Backup DB + files into the sites volume |
| ERPNext: Bench shell | Bash inside the backend container |
| ERPNext: RESET | Delete containers **and all data** |

## Settings (`.env`)
- `ERPNEXT_VERSION` – image tag, e.g. `v16.12.0`
- `HTTP_PORT` – change if 8080 is busy (then open http://localhost:<port>)
- `ADMIN_PASSWORD`, `DB_ROOT_PASSWORD` – only used on first creation.
  To change them later, run **RESET** first (this deletes your data).

## Troubleshooting
- **"Cannot connect to the Docker daemon"** → open Docker Desktop and wait.
- **Port already in use** → change `HTTP_PORT` in `.env`, run Start again.
- **Only login page / site not found** → run *Watch site creation*; if it failed, run RESET then Start.
- **Slow / crashes** → give WSL more RAM in `C:\Users\<you>\.wslconfig`:
  ```ini
  [wsl2]
  memory=6GB
  ```
  then `wsl --shutdown` in PowerShell and restart Docker Desktop.
