
# Installation

Welcome to `agentic-atlas`! We're excited to have you here. Let's get the necessary tools installed so you can start building. 

The best way to install the `agentic-atlas` package is through `pip`, Python's package installer.

## Prerequisites

Make sure you have the following installed:

* [Docker](https://www.docker.com/)
* [Docker Compose](https://docs.docker.com/compose/)
* [`make`](https://www.gnu.org/software/make/) (optional)

  * macOS: `brew install make`
  * Linux: `sudo apt-get install make`

## 🚀 Local Setup

1. Clone the repository:

   ```bash
   git clone https://github.com/genai-works-org/genai-agentos.git
   cd genai-agentos/
   ```

2. Create a `.env` file by copying the example (can be empty and customized later):

   ```bash
   cp .env-example .env
   ```

   * A `.env` file **should be present** for configuration.
   * All variables in `.env-example` are commented.
     You can customize any environment setting by **uncommenting** the relevant line and providing a new value.

3. Start Docker desktop and ensure it is running.

4. Start the infrastructure:

   ```bash
   make up
   # or alternatively
   docker compose up
   ```

5. After startup:

   * Frontend UI: [http://localhost:3000/](http://localhost:3000/)
   * Swagger API Docs: [http://localhost:8000/docs#/](http://localhost:8000/docs#/)

## Agentic Atlas CLI

A command-line tool for interacting with the **GenAI infrastructure**. It allows you to register users, manage agents, and run them in isolated environments.

***

### Installation

### Option 1: Install Pre-Built Binary

#### Linux / macOS

1.  Run the installation script:
    ```bash
    ./install_cli.sh
    ```
2.  Enter your system password when prompted to install the binary to `/usr/local/bin`.
3.  Verify the installation:
    ```bash
    genai --help
    ```

#### Windows

1.  Set the `GITHUB_TOKEN` environment variable in PowerShell:
    ```powershell
    [Environment]::SetEnvironmentVariable("GITHUB_TOKEN", "your_token_here", "User")
    ```
2.  Run the installation script:
    ```powershell
    .\install_cli.ps1
    ```
3.  Verify the installation:
    ```powershell
    .\genai.exe --help
    ```
> **Note**: This process will be simplified once the repository is public.

---

### Option 2: Build From Source

#### Linux / macOS
You can run the build script:
```bash
./build_cli.sh
```

Or build manually:

  1. Ensure `python3.12` and `uv` are installed.
  2. Run:

     ```bash
     uv run pyinstaller --onefile --name genai.bin cli.py
     ```

####  Windows

Build using:

  ```powershell
  ./build_cli.ps1
  ```

This uses **Nuitka** to compile into a Windows-friendly executable that avoids malware flags.

And that's it! You now have the core agentic atlas library installed and are ready to go.



Now that you're all set up, let's move on to the Quick Start and build something!