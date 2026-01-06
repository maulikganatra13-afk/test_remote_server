**ExpenseTracker — FastMCP MCP server**

- **What:** A small FastMCP-based MCP server exposing tools to add, list and summarize expenses and a categories resource. The server code lives in `main.py`, proxy example in `proxy.py`, and categories data in `categories.json`.

**Prerequisites**
- FastMCP Cloud account and an app (create via FastMCP Cloud console).
- Git access to push your repository (FastMCP Cloud typically deploys from a Git remote).
- Python 3.11+ (matches `pyproject.toml` requirement).

**Prepare repository**
- Ensure `pyproject.toml` lists dependencies (`fastmcp`, `aiosqlite`). Commit your code:

```bash
git init                 # if not already a repo
git add .
git commit -m "Prepare ExpenseTracker MCP app"
```

**Deploying to FastMCP Cloud (basic flow)**
1. In the FastMCP Cloud console create a new app and connect your Git repository (or add the FastMCP Git remote they provide).
2. Push your branch to the FastMCP remote:

```bash
# example FastMCP remote name; replace with the URL provided by the console
git remote add fastmcp <FASTMCP_GIT_URL>
git push fastmcp main
```

3. In the FastMCP app settings set the Python runtime to match (>=3.11) and set the start command to run the server, for example:

```text
python main.py
```

4. Deploy and watch logs in the FastMCP console. The app should install dependencies and start.

**Important: Port & Binding behaviour**
- The `main.py` in this repository currently starts the MCP server with a fixed host/port (`0.0.0.0:8000`). Two deployment scenarios follow:

- Option A — Platform maps external traffic to internal port 8000 (no code change required):
	- Configure the FastMCP app to route incoming HTTP traffic to the process listening on port `8000` (some PaaS platforms let you specify an internal port or perform automatic mapping). Set the start command to `python main.py` and deploy.

- Option B — Platform requires you to bind to a platform-assigned `PORT` environment variable (requires adjusting code):
	- If FastMCP Cloud requires binding to the environment-provided `PORT`, update `main.py` so the server reads `PORT` from `os.environ` before calling `mcp.run(...)`. Example minimal change (optional):

```python
import os
port = int(os.environ.get("PORT", "8000"))
mcp.run(transport="http", host="0.0.0.0", port=port)
```

	- Commit that change and push to FastMCP. (If you prefer not to change source, see Option A or set a custom start command in the console that launches the server bound to port 8000 if the platform supports it.)

**Post-deploy: update `proxy.py`**
- After the app is live, update the URL in `proxy.py` to point to your FastMCP app (for example `https://your-app.fastmcp.app/mcp`) and run the proxy as needed.

**Verify the deployment**
- Use the FastMCP Cloud logs to confirm successful startup and to inspect printed messages.
- Test the MCP endpoint using a FastMCP client/proxy or curl against the `/mcp` endpoint if appropriate.

**Data persistence note**
- The app creates an SQLite DB in the system temporary directory by default. On cloud platforms this may be ephemeral across restarts or redeploys. For production use, mount persistent storage or switch to a managed DB.

**Security note**
- Do not expose unprotected MCP tools publicly. Add authentication, network restrictions or IP allowlists before making the endpoint public.

**Troubleshooting**
- Dependency install failures: confirm `pyproject.toml` and Python version in the FastMCP app settings.
- Database write errors: check platform filesystem permissions and consider using persistent storage.

---
