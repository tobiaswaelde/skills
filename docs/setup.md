# Complete Codex Setup on a New Computer

This guide sets up the personal environment from this repository on Linux or macOS. On Windows, follow the same Linux instructions inside WSL2. Run all commands in a terminal unless a step explicitly refers to a Codex chat.

The setup includes:

| Component | Purpose | Official source |
| --- | --- | --- |
| Codex CLI | Agent, plugin management, and MCP configuration | [OpenAI: Installation](https://learn.chatgpt.com/docs/cli) |
| uv | Install Python and manage Python CLI tools | [Astral: Installation](https://docs.astral.sh/uv/getting-started/installation/) |
| Spec Kit / `specify` | Project specifications, plans, and tasks | [GitHub: Installation](https://github.github.io/spec-kit/installation.html) |
| RTK | Compress shell output for the agent | [rtk-ai/rtk](https://github.com/rtk-ai/rtk#installation) |
| Context7 | Retrieve current library documentation through MCP | [Context7: Codex](https://context7.com/docs/clients/codex) |
| Personal Skills, Matt Pocock, Caveman | Personal and external workflows | [Marketplace directory](../marketplaces.json) |

You need internet access, local administrator privileges for system packages, a GitHub account, and an account with Codex access. Context7 requires a separate sign-in. This guide uses standalone installers and a remote Context7 MCP server, so Node.js, npm, Rust, and a separately installed system Python are not required. Install project-specific runtimes such as Node.js later, according to each project's requirements.

## 1. Prepare the Operating System

Run only the section for your operating system.

### Ubuntu / Debian

```sh
sudo apt update
sudo apt install -y git curl ca-certificates tar unzip ripgrep jq gh
```

If `gh` is unavailable in your distribution's package repositories, follow the [official GitHub CLI installation instructions for Linux](https://github.com/cli/cli/blob/trunk/docs/install_linux.md).

### Fedora

```sh
sudo dnf install -y git curl ca-certificates tar unzip ripgrep jq gh
```

### Arch Linux / CachyOS

```sh
sudo pacman -Syu --needed git curl ca-certificates tar unzip ripgrep jq github-cli
```

### macOS

First, install the Command Line Tools and complete the installation dialog:

```sh
xcode-select --install
```

Then install [Homebrew](https://brew.sh/):

```sh
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

Follow the installer's “Next steps” to add `brew` to your `PATH` permanently. Open a new terminal, then install the tools:

```sh
brew install git gh ripgrep jq
```

### Windows with WSL2

Run PowerShell **as Administrator**:

```powershell
wsl --install -d Ubuntu
```

Restart Windows when prompted, open Ubuntu, and create your Linux username and password. Then follow the **Ubuntu / Debian** section and run all remaining shell commands inside Ubuntu. Store projects under `~/projects` in the Linux filesystem. A native Windows Codex installation has its own user configuration; it does not automatically inherit the WSL configuration.

Source: [Microsoft: Install WSL](https://learn.microsoft.com/en-us/windows/wsl/install).

### Prepare the User Directory for CLI Tools

For Bash or Zsh, add `~/.local/bin` to your `PATH` permanently. Run only the block for your shell.

Bash, for example on Ubuntu or CachyOS:

```bash
mkdir -p "$HOME/.local/bin"
touch "$HOME/.bashrc"
grep -Fqx 'export PATH="$HOME/.local/bin:$PATH"' "$HOME/.bashrc" || \
  printf '\nexport PATH="$HOME/.local/bin:$PATH"\n' >> "$HOME/.bashrc"
export PATH="$HOME/.local/bin:$PATH"
```

Zsh, for example in the default macOS terminal:

```zsh
mkdir -p "$HOME/.local/bin"
touch "$HOME/.zshrc"
grep -Fqx 'export PATH="$HOME/.local/bin:$PATH"' "$HOME/.zshrc" || \
  printf '\nexport PATH="$HOME/.local/bin:$PATH"\n' >> "$HOME/.zshrc"
export PATH="$HOME/.local/bin:$PATH"
```

For another shell, update its startup file accordingly. The following examples assume Bash or Zsh.

## 2. Configure Git and Sign In to GitHub

Replace the placeholders with your name and a GitHub-verified email address or your personal GitHub noreply address:

```sh
git config --global user.name "YOUR NAME"
git config --global user.email "YOUR GITHUB EMAIL"
gh auth login --hostname github.com --git-protocol https --web
gh auth setup-git
gh auth status
```

Complete the browser sign-in. HTTPS is sufficient for this guide; an SSH key is not required. For private GitLab or Forgejo projects, configure their Git credentials and any applicable Codex plugins separately.

Source: [GitHub CLI: Quickstart](https://docs.github.com/en/github-cli/github-cli/quickstart).

## 3. Install Codex and Sign In

Use the official standalone installer:

```sh
curl -fsSL https://chatgpt.com/codex/install.sh | sh
```

Follow the installer's PATH instructions and open a new terminal if needed. Then run:

```sh
codex --version
codex login
codex login status
codex plugin --help
```

Complete the ChatGPT sign-in in your browser. Without a local browser, use `codex login --device-auth` instead. If `codex plugin` is unavailable, update Codex before configuring the marketplaces.

For a graphical interface, also install the [Codex app](https://developers.openai.com/codex/app) using its platform-specific instructions and sign in. Continue the setup through the CLI. The app and CLI share configuration when they use the same user account and `CODEX_HOME`; this guide assumes the default directory, `~/.codex`.

Sources: [OpenAI: Codex CLI](https://learn.chatgpt.com/docs/cli), [CLI commands](https://learn.chatgpt.com/docs/cli/reference).

## 4. Install uv and Python

[uv](https://docs.astral.sh/uv/) manages Python and isolated installations of Python CLI tools. Install uv first:

```sh
curl -LsSf https://astral.sh/uv/install.sh | sh
```

Open a new terminal, then install Python for Spec Kit and configure the tool path:

```sh
uv --version
uv python install 3.12
uv tool update-shell
```

If `uv tool update-shell` changed your shell configuration, open another terminal. Python 3.12 satisfies Spec Kit's requirement for Python 3.11 or later. Selecting Python explicitly in the next step avoids depending on system Python.

Sources: [Install uv](https://docs.astral.sh/uv/getting-started/installation/), [Manage Python](https://docs.astral.sh/uv/guides/install-python/), [Manage CLI tools](https://docs.astral.sh/uv/concepts/tools/).

## 5. Install Spec Kit

[Spec Kit](https://github.com/github/spec-kit) is accessed through the `specify` CLI. Install the officially published package with uv:

```sh
uv tool install --python 3.12 specify-cli
specify version
specify check
```

`specify check` also lists other supported agents. You do not need to install those agents; this setup requires Codex to be available.

For a reproducible installation pinned to a specific version, use a reviewed release tag from the [Spec Kit releases](https://github.com/github/spec-kit/releases) instead. Replace `vX.Y.Z` before running:

```sh
uv tool install --python 3.12 specify-cli --from 'git+https://github.com/github/spec-kit.git@vX.Y.Z'
```

These commands are alternative installation methods. Do not install a second copy with `pip` or `pipx`.

Source: [Spec Kit: Installation](https://github.github.io/spec-kit/installation.html).

### Enable Spec Kit per Project

Installing the CLI globally does not create project files. For a new project:

```sh
mkdir -p "$HOME/projects"
cd "$HOME/projects"
specify init my-project --integration codex --script sh
cd my-project
codex
```

For an existing project, commit or back up local changes first, then run this command from its directory:

```sh
specify init --here --integration codex --script sh
```

Review the prompt for a nonempty directory and the resulting Git diff. In an existing Spec Kit project, use `specify integration install codex` instead if the Codex integration is missing.

The integration creates skills under `.agents/skills/speckit-*/SKILL.md`. Start a new Codex chat in the project afterward. Enter the following prompts **in the Codex chat**, one at a time, reviewing each result before continuing:

```text
$speckit-constitution Define project principles for quality, testing, and maintainability.
$speckit-specify Describe the desired behavior and requirements.
$speckit-plan Plan the technical implementation.
$speckit-tasks Break the implementation into verifiable tasks.
$speckit-implement
```

Spec Kit is configured per project. The personal `coding-workflow` still governs approvals and delivery; Spec Kit does not replace those rules.

Source: [Spec Kit: Codex integration](https://github.github.io/spec-kit/integrations/codex.html).

## 6. Install RTK and Enable It for Codex

[RTK](https://github.com/rtk-ai/rtk) compresses supported shell output before the agent reads it. The correct project is **rtk-ai/rtk**.

```sh
curl -fsSL https://raw.githubusercontent.com/rtk-ai/rtk/refs/heads/master/install.sh | sh
rtk --version
rtk gain
rtk init -g --codex
```

Review the output of `rtk init` and confirm the proposed configuration changes. `-g --codex` configures the global Codex integration with a hook and instructions under `~/.codex`. Omitting `--codex` configures a different agent integration.

Restart Codex completely afterward. To inspect the integration without changing files:

```sh
rtk init -g --codex --show
```

In a Codex chat inside a Git repository, ask the agent to run `git status` in the terminal. Then check `rtk gain` in your own terminal. Running `git status` manually in your own terminal does not test the Codex hook. The integration applies to shell calls; it does not automatically compress other built-in tools.

Sources: [RTK: Installation and Quick Start](https://github.com/rtk-ai/rtk#quick-start), [Installer](https://github.com/rtk-ai/rtk/blob/master/install.sh).

## 7. Connect Context7 through MCP and OAuth

[Context7](https://github.com/upstash/context7) provides current library documentation. Like the existing computer, this setup uses the remote MCP server with OAuth:

```sh
codex mcp add context7 --url https://mcp.context7.com/mcp/oauth
codex mcp login context7
codex mcp get context7
codex mcp list
```

Complete the Context7 sign-in in your browser. This method requires neither a local Context7 installation nor a manually managed API key. The OAuth endpoint ends in **`/mcp/oauth`**, not `/mcp`.

Codex saves the server in `~/.codex/config.toml`. The relevant entry looks like this:

```toml
[mcp_servers.context7]
url = "https://mcp.context7.com/mcp/oauth"
```

Do not add a duplicate entry manually if the CLI has already registered it. For an existing server, inspect `codex mcp get context7` first and adjust its configuration as needed.

In a new Codex chat, ask something like: “Show me the current Vue documentation for `watch`. Use Context7.” Expect calls to resolve the library and retrieve its documentation. An entry in `codex mcp list` alone does not prove that documentation retrieval works.

Sources: [Context7: OAuth](https://context7.com/docs/howto/oauth), [Context7 with Codex](https://context7.com/docs/clients/codex), [OpenAI: MCP](https://learn.chatgpt.com/docs/extend/mcp).

## 8. Install Personal and External Skills

Register all three marketplaces separately. The `marketplaces.json` file lists their sources; it does not install their plugins automatically.

```sh
codex plugin marketplace add https://github.com/tobiaswaelde/skills.git --ref main
codex plugin add personal-skills@tobiaswaelde

codex plugin marketplace add https://github.com/mattpocock/skills.git
codex plugin add mattpocock-skills@mattpocock

codex plugin marketplace add https://github.com/JuliusBrussee/caveman.git
codex plugin add caveman@caveman

codex plugin marketplace list
codex plugin list
```

Sources: [Personal Skills](https://github.com/tobiaswaelde/skills), [Matt Pocock Skills](https://github.com/mattpocock/skills), [Caveman](https://github.com/JuliusBrussee/caveman).

The personal plugin includes the seven skills described in the [README](../README.md#personal-skills). Caveman Ultra does not require a Caveman Cloud account for response formatting. Cloud and Learn features have additional requirements from their provider.

## 9. Configure Global Instructions

Maintain `~/.codex/AGENTS.md` separately on each computer. Plugin installation does not synchronize this file. After configuring RTK, open the file in a text editor and add the following instructions. Preserve existing instructions and the reference to `RTK.md` created by RTK; do not add a second RTK reference.

```markdown
# Global Codex Instructions

## Response style

- Always use `/caveman ultra` (the `ultracave` skill) by default in every session, unless I explicitly request another mode.
- Always keep final responses concise.
- Prefer bullet points over paragraphs.
- Be technical and direct. Avoid long-form explanations unless explicitly requested.
- Do not repeat my request or describe every intermediate step.
- After completing a task, use only the relevant sections: Changed, Tests, Notes. Omit empty sections; aim for 3–8 bullet points.

## Coding workflow

- Use the `coding-workflow` skill by default for coding project tasks in every project, regardless of repository host or issue tracker, unless I explicitly request a different workflow. Read and follow its instructions without requiring me to invoke it by name.
- Prefer native Codex question/selection controls for clarifications and approvals whenever an available tool supports that purpose in the current mode. Offer concise action-specific choices instead of requiring a typed "yes"; wait for an actually submitted answer before dependent work.

## Documentation

- Write all guides and setup instructions in en-US.
- Use Context7 for current library and framework documentation, API references, setup and configuration questions. Match the project's version when available.
```

After `rtk init -g --codex`, the file also contains the RTK reference. With the default directory, an example is:

```text
@/home/YOUR-USERNAME/.codex/RTK.md
```

RTK generates the actual absolute path. On macOS, it is typically under `/Users/YOUR-USERNAME/.codex/RTK.md`. Do not copy the example path unchanged or replace the RTK-managed `RTK.md` with your own content.

## 10. Verify the Setup

Open a new terminal and run:

```sh
git --version
gh auth status
codex --version
codex login status
uv --version
uv python find 3.12
specify version
specify check
rtk --version
rtk init -g --codex --show
codex plugin marketplace list
codex plugin list
codex mcp list
```

Then restart Codex and verify:

- Personal Skills, Matt Pocock, and Caveman are installed and available in the new chat.
- `$coding-workflow` can be invoked; the global instructions enable it by default for coding tasks.
- `/caveman status` reports Ultra mode after the global instruction has been applied.
- A Context7 question retrieves actual documentation without authentication errors.
- A supported shell command executed by the agent appears in the RTK statistics.
- `$speckit-specify` is available in a project initialized with Spec Kit.

For `command not found`, check `command -v codex uv specify rtk` and your `PATH` first. If skills are missing, start a new chat and restart the app if necessary. For Context7 authentication errors, run `codex mcp login context7` again. GitHub, Codex, and Context7 sign-ins are independent.

## 11. Update Each Computer

For the standalone installers and the uv installation from PyPI used above:

```sh
curl -fsSL https://chatgpt.com/codex/install.sh | sh
uv self update
uv tool upgrade specify-cli
curl -fsSL https://raw.githubusercontent.com/rtk-ai/rtk/refs/heads/master/install.sh | sh
rtk init -g --codex

codex plugin marketplace upgrade tobiaswaelde
codex plugin add personal-skills@tobiaswaelde
codex plugin marketplace upgrade mattpocock
codex plugin add mattpocock-skills@mattpocock
codex plugin marketplace upgrade caveman
codex plugin add caveman@caveman
```

For a Spec Kit installation pinned to a Git tag, install the selected new tag with `uv tool install --force --python 3.12 specify-cli --from 'git+https://github.com/github/spec-kit.git@vX.Y.Z'` instead. Update Homebrew installations with `brew upgrade`; do not mix installation methods.

Update the Spec Kit CLI and previously generated project files separately. For existing projects, follow the [Spec Kit upgrade guide](https://github.github.io/spec-kit/upgrade.html) and review changes before accepting them. The remote Context7 server does not require a local package update. After plugin or hook updates, restart Codex and repeat the setup checks.

## 12. Set Up Additional Computers

This repository is the source for the personal skills. On another computer, follow this guide again and then transfer your global instructions. A clone is required only when you want to edit skills:

```sh
mkdir -p "$HOME/projects"
git clone https://github.com/tobiaswaelde/skills.git "$HOME/projects/skills"
```

Transfer `AGENTS.md` and the required nonsecret settings selectively. Sign in to GitHub, Codex, and MCP servers separately on each device. Do not commit credentials or entire credential directories to this public repository. Spec Kit project files are provided through the respective project repositories; configure additional MCP servers and service connections separately as needed.
