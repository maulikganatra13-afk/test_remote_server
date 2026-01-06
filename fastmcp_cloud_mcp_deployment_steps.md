# Deploying a Custom MCP Server on FastMCP Cloud (Free Tier)

This guide explains how to deploy your custom **FastMCP (Model Context
Protocol) server** to **FastMCP Cloud** using the free tier.

------------------------------------------------------------------------

## 1. Build Your MCP Server Locally

### Install FastMCP

``` bash
pip install fastmcp
```

### Create your server file (example: `my_mcp_server.py`)

``` python
from fastmcp import FastMCP

mcp = FastMCP(name="My Custom MCP Server")

@mcp.tool
def add(a: int, b: int) -> int:
    """Example tool that adds two numbers."""
    return a + b

if __name__ == "__main__":
    mcp.run()
```

Make sure your server runs locally before deploying.

------------------------------------------------------------------------

## 2. Push the Project to GitHub

1.  Create a new GitHub repository.
2.  Add your MCP server file(s).
3.  Add a `requirements.txt` file containing:

```{=html}
<!-- -->
```
    fastmcp

4.  Push everything to the `main` branch.

FastMCP Cloud will automatically detect dependencies.

------------------------------------------------------------------------

## 3. Sign In to FastMCP Cloud

1.  Open **https://fastmcp.cloud/**
2.  Sign in using your GitHub account.
3.  Authorize repository access.

The free tier is sufficient for personal deployments.

------------------------------------------------------------------------

## 4. Create a Deployment Project

Inside FastMCP Cloud:

-   Click **Create Project**
-   Select your GitHub repo
-   Set:
    -   **Project Name** --- any readable name

    -   **Entrypoint** --- reference your MCP instance, e.g.

            my_mcp_server.py:mcp

You may leave authentication disabled for testing.

------------------------------------------------------------------------

## 5. Deploy the Server

FastMCP Cloud will:

-   Clone your repository
-   Install dependencies
-   Build & deploy your MCP server

You will receive a public MCP endpoint such as:

    https://your-project.fastmcp.app/mcp

------------------------------------------------------------------------

## 6. Connect Using an MCP Client

Use the endpoint in a compatible client, for example:

    client.connect("https://your-project.fastmcp.app/mcp")

Your tools are now available remotely.

------------------------------------------------------------------------

## 7. Continuous Deployment

Whenever you push changes to `main`:

-   FastMCP Cloud automatically rebuilds
-   The deployment is updated

No manual redeployment needed.

------------------------------------------------------------------------

## ✅ Quick Checklist

-   [x] MCP server runs locally
-   [x] Repo on GitHub
-   [x] `requirements.txt` includes `fastmcp`
-   [x] Entrypoint set in FastMCP Cloud
-   [x] Deployment completed
-   [x] Endpoint tested in client

------------------------------------------------------------------------

Happy building!
