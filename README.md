# Unity MCP   

**Connect your Unity Editor to LLMs using the Model Context Protocol.**

Unity MCP acts as a bridge, allowing AI assistants (like Claude, Cursor) to interact directly with your Unity Editor via a local **MCP (Model Context Protocol) Client**. Give your LLM tools to manage assets, control scenes, edit scripts, and automate tasks within Unity.

---

## Key Features 

*   **-> Natural Language Control:** Instruct your LLM to perform Unity tasks.
*   **-> Powerful Tools:** Manage assets, scenes, materials, scripts, and editor functions.
*   **-> Automation:** Automate repetitive Unity workflows.
*   **-> Extensible:** Designed to work with various MCP Clients.

<details>
  <summary><strong>Expand for Available Tools...</strong></summary>

  Your LLM can use functions like:

  *   `read_console`: Gets messages from or clears the console.
  *   `manage_script`: Manages C# scripts (create, read, update, delete).
  *   `manage_editor`: Controls and queries the editor's state and settings.
  *   `manage_scene`: Manages scenes (load, save, create, get hierarchy, etc.).
  *   `manage_asset`: Performs asset operations (import, create, modify, delete, etc.).
  *   `manage_gameobject`: Manages GameObjects: create, modify, delete, find, and component operations.
  *   `execute_menu_item`: Executes a menu item via its path (e.g., "File/Save Project").
</details>

---

## Working

Unity MCP connects your tools using two components:

1.  **Unity MCP Bridge:** A Unity package running inside the Editor. (Installed via Package Manager).
2.  **Unity MCP Server:** A Python server that runs locally, communicating between the Unity Bridge and your MCP Client. (Installed manually).

**Flow:** `[Your LLM via MCP Client] <-> [Unity MCP Server (Python)] <-> [Unity MCP Bridge (Unity Editor)]`

---

## Installation ⚙️

> **Note:** The setup is constantly improving as we update the package. Check back if you randomly start to run into issues.

### Prerequisites

<details>
  <summary><strong>Click to view required software...</strong></summary>

  *   **Git CLI:** For cloning the server code. [Download Git](https://git-scm.com/downloads)
  *   **Python:** Version 3.12 or newer. [Download Python](https://www.python.org/downloads/)
  *   **Unity Hub & Editor:** Version 2020.3 LTS or newer. [Download Unity](https://unity.com/download)
  *   **uv (Python package manager):**
      ```bash
      pip install uv
      # Or see: https://docs.astral.sh/uv/getting-started/installation/
      ```
  *   **An MCP Client:**
      *   [Claude Desktop](https://claude.ai/download)
      *   [Cursor](https://www.cursor.com/en/downloads)
      *   *(Others may work with manual config)*
</details>


### Step 1: Install Python
Install PYTHON 3.12 or newer version from python.org.
Make sure to add PYTHON to your system's PATH during installation.


### Step 2: Install uv
uv is python package manager that simplifies dependency management. Install it using the command below based on your operating system:

* Mac: brew install uv
* Windows: powershell -c "irm https://astral.sh/uv/install.ps1 | iex"
      Then, add uv to your PATH:
      set Path-%USERPROFILE%\. local\bin;%Path%
* Linux: curl -LsSf https://astral.sh/uv/install.sh | sh

Important: Do not proceed without installing uv.


### Step 3: Install the Unity Package (Bridge)

1.  Open your Unity project.
2.  Go to `Window > Package Manager`.
3.  Click `+` -> `Add package from git URL...`.
4.  Enter:
    ```
    https://github.com/TanushShoor/ThreatSim-Buildverse.git?path=/UnityMcpBridge
    ```
5.  Click `Add`.
6. The MCP Server should automatically be installed onto your machine as a result of this process.

### Step 4: Install Claude AI 
* Go to https://claude.ai/download and download Claude AI according to your system
* After signing in, go to settings>Developer where you will see "unityMCP running" after step 5.

  
### Step 5: Configure Your MCP Client

Connect your MCP Client (Claude, Cursor, etc.) to the Python server you installed in Step 1.

**Option A: Auto-Configure (Recommended for Claude/Cursor)**

1.  In Unity, go to `Window > Unity MCP`.
2.  Click `Auto Configure Claude` or `Auto Configure Cursor`.
3.  Look for a green status indicator 🟢 and "Connected". *(This attempts to modify the MCP Client's config file automatically)*.

**Option B: Manual Configuration**

If Auto-Configure fails or you use a different client:

1.  **Find your MCP Client's configuration file.** (Check client documentation).
    *   *Claude Example (macOS):* `~/Library/Application Support/Claude/claude_desktop_config.json`
    *   *Claude Example (Windows):* `%APPDATA%\Claude\claude_desktop_config.json`
2.  **Edit the file** to add/update the `mcpServers` section, using the *exact* paths from Step 1.

<details>
<summary><strong>Click for OS-Specific JSON Configuration Snippets...</strong></summary>

**Windows:**

  ```json
  {
    "mcpServers": {
      "UnityMCP": {
        "command": "uv",
        "args": [
          "run",
          "--directory",
          "C:\\Users\\YOUR_USERNAME\\AppData\\Local\\Programs\\UnityMCP\\UnityMcpServer\\src",
          "server.py"
        ]
      }
      // ... other servers might be here ...
    }
  }
``` 

(Remember to replace YOUR_USERNAME and use double backslashes \\)

**macOS:**

```json
{
  "mcpServers": {
    "UnityMCP": {
      "command": "uv",
      "args": [
        "run",
        "--directory",
        "/usr/local/bin/UnityMCP/UnityMcpServer/src",
        "server.py"
      ]
    }
    // ... other servers might be here ...
  }
}
```
(Replace YOUR_USERNAME if using ~/bin)

**Linux:**

```json
{
  "mcpServers": {
    "UnityMCP": {
      "command": "uv",
      "args": [
        "run",
        "--directory",
        "/home/YOUR_USERNAME/bin/UnityMCP/UnityMcpServer/src",
        "server.py"
      ]
    }
    // ... other servers might be here ...
  }
}
```

(Replace YOUR_USERNAME)

</details>

---

## How to use? 

1. **Open your Unity Project.** The Unity MCP Bridge (package) should connect automatically. Check status via Window > Unity MCP.
    
2. **Start your MCP Client** (Claude, Cursor, etc.). It should automatically launch the Unity MCP Server (Python) using the configuration from Installation Step 3.
    
3. **Interact!** Unity tools should now be available in your MCP Client.
    
    Example Prompt: `Create a 3D player controller.`
    

