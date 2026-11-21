# School RRM — fresh start on your laptop

You do **not** need anything from the cloud agent. Copy this project to your PC, install two runtimes, start two terminals — then `http://127.0.0.1` works on **your** machine.

## 1. Get the project onto your laptop

In **Cursor** (this chat’s workspace), in the file tree:

1. Find **`school-rrm-for-laptop.zip`** at the top level of the workspace.
2. **Right‑click → Download** (save to your computer).
3. Unzip → you should have:
   - `school-rrm/`
   - `school-rrm-admin/`
   - `README-FIRST.md` (this file)

Optional: **File → Open Folder** → choose the unzipped folder (or a parent folder that contains both directories).

## 2. Install once

| Tool | Why | Get it |
|------|-----|--------|
| **Node.js 20+** | Newcomer portal | https://nodejs.org (LTS) |
| **Python 3.10+** | Staff admin | https://python.org (check “Add to PATH” on Windows) |

## 3. One shared admin password (local only)

Pick one long random string (or run `openssl rand -hex 24` on Mac/Linux).

Put the **same** value in **both** files:

- `school-rrm/.env` → `ADMIN_API_KEY=...`
- `school-rrm-admin/.env` → `ADMIN_API_KEY=...`

Create them from the examples:

```bash
# macOS / Linux — run from the folder that contains school-rrm and school-rrm-admin
cp school-rrm/.env.example school-rrm/.env
cp school-rrm-admin/.env.example school-rrm-admin/.env
```

**Windows (PowerShell):**

```powershell
copy school-rrm\.env.example school-rrm\.env
copy school-rrm-admin\.env.example school-rrm-admin\.env
```

Edit both `.env` files and set `ADMIN_API_KEY` to your chosen value.

In `school-rrm-admin/.env`, set `SCHOOL_RRM_DATA` to the **full path** of `store.json` on your PC, for example:

- macOS: `/Users/you/Projects/school-rrm/data/store.json`
- Windows: `C:\Users\you\Projects\school-rrm\data\store.json`

Leave `REQUIRE_SCHOOL_OTP=false` unless you want the extra 6‑digit step.

## 4. Install dependencies

**Terminal A — portal:**

```bash
cd school-rrm
npm install
```

**Terminal B — admin (after Python venv):**

```bash
cd school-rrm-admin
python3 -m venv .venv          # Windows: py -m venv .venv
source .venv/bin/activate      # Windows: .\.venv\Scripts\activate
pip install -r requirements.txt
```

## 5. Run (every time you work)

**Terminal A:**

```bash
cd school-rrm
npm run dev
```

**Terminal B:**

```bash
cd school-rrm-admin
source .venv/bin/activate      # Windows: .\.venv\Scripts\activate
python main.py
```

## 6. Open in your browser (on the same laptop)

| URL | What |
|-----|------|
| http://127.0.0.1:8787/admin | Create invite links |
| http://127.0.0.1:8787/health | Portal OK check |
| http://127.0.0.1:8790 | Staff dashboard → login with `ADMIN_API_KEY` |

Until OAuth is configured, newcomers can use **dev sign‑in** on the join page (see `school-rrm/README.md`).

## 7. Quick test

```bash
curl -s -o /dev/null -w "%{http_code}\n" http://127.0.0.1:8790/login
curl -s http://127.0.0.1:8787/health
```

Expect `200` and `{"ok":true,...}`.

## Troubleshooting

| Problem | Fix |
|---------|-----|
| Connection refused on 8790 / 8787 | Start both terminals; don’t use the cloud agent URL on your laptop. |
| Admin login fails | `ADMIN_API_KEY` must match in **both** `.env` files; restart both apps. |
| Admin dashboard empty | Wrong `SCHOOL_RRM_DATA` path — must point at **your** `school-rrm/data/store.json`. |

More detail: `school-rrm-admin/LAPTOP.md` and `school-rrm/README.md`.