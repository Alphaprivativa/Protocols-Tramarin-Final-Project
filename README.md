# Tamarin Devcontainer

A ready-to-use development environment for Tamarin, designed for courses and workshops. Students can use it in two ways: in the browser via GitHub Codespaces, or locally in VS Code with Docker. No manual Tamarin installation required either way.

## For students: getting started

### Option A: GitHub Codespaces (browser, no local install)

1. Click the green **Code** button at the top of this repository.
2. Select the **Codespaces** tab.
3. Click **Create codespace on main**.
4. Wait for the pre-built container image to be pulled (only on first launch; subsequent opens should be quite fast).
5. Open any `.spthy` file in `examples/` and start modeling.

> **Warning: delete your codespace when you are done.**
>
> In the GitHub Codespaces free tier you have 120 hours/month compute allowance and 15GB/month storage allowance. Codespaces auto-suspend after 30 minutes of inactivity (pausing compute), but the stopped container keeps consuming your storage quota for up to 30 days before GitHub auto-deletes it. Each codespace takes about 3-4 GB of your 15 GB/month free allowance. Create a few and forget to delete them and you will exhaust your quota.
>
> To delete: go to [github.com/codespaces](https://github.com/codespaces), click `...` next to your codespace, and select **Delete**.

### Option B: Local VS Code with Docker (recommended if you have VS Code and Docker installed)

**Prerequisites:** [Docker Desktop](https://www.docker.com/products/docker-desktop/) and the [Dev Containers](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-containers) VS Code extension.

1. Clone this repository:
   ```bash
   git clone https://github.com/stefanoberlato/tamarin-devcontainer.git
   cd tamarin-devcontainer
   ```
2. Open the folder in VS Code:
   ```bash
   code .
   ```
3. VS Code will detect the `.devcontainer` configuration and show a prompt — click **Reopen in Container**.
   - If the prompt does not appear: open the Command Palette (`Ctrl+Shift+P` / `Cmd+Shift+P`) and run **Dev Containers: Reopen in Container**.
4. The pre-built image is pulled from GHCR. The Tamarin extension is installed automatically.
5. Open any `.spthy` file in `examples/` and start modeling.

### Using the Tamarin VS Code extension

The [Tamarin VS Code extension](https://marketplace.visualstudio.com/items?itemName=tamarin-prover.tamarin-prover) is pre-installed. It provides syntax highlighting, syntax error detection, wellformedness checks, and Language Server Protocol support for `.spthy` files.

Features:
- Syntax highlighting for Tamarin theory files (`.spthy`, `.splib`)
- Real-time syntax error detection
- Wellformedness checks (variable usage, fact arities, built-in imports, etc.)
- Quick fixes for common issues
- Rename identifiers and search definitions

### Using the Tamarin web interface

To start the interactive web interface, open the integrated terminal and run:

```bash
tamarin-prover interactive examples/<file_name>.spthy
```

The web UI will be available on port 3000 (or 3001). Then, either look at the Ports panel (bottom of VS Code) and click the globe icon to open it in your browser, or follow the link returned in the terminal (usually, `http://127.0.0.1:3000` or `http://127.0.0.1:3001`) with CTRL+click.

## For instructors: customising the environment

### Repository structure

```
.
├── .devcontainer/
│   ├── devcontainer.json   # Codespaces configuration
│   └── Dockerfile          # Container image definition
└── examples/
    └── *.spthy             # Tamaring files
```

### Adding course material

Drop `.spthy` files into `examples/` (or any subdirectory). Students will see them in the file explorer when they open the Codespace.

### Changing the Tamarin version

The Dockerfile builds Tamarin from the `main` branch of the official repository. To pin to a specific commit or branch, change the `git clone` line in `.devcontainer/Dockerfile`:

```dockerfile
# Clone a specific branch or tag:
RUN git clone --branch <branch-or-tag> https://github.com/tamarin-prover/tamarin-prover.git
```

### Dependencies installed

| Dependency | Purpose |
|---|---|
| Maude 3.5.1 | Maude backend for Tamarin |
| Graphviz | Visualization of attack traces |
| Haskell Stack | Build tool for Tamarin |
| Node.js / npm | Required by the VS Code language server |

### Rebuilding the image

The container image is pre-built and hosted on GitHub Container Registry (GHCR). It is rebuilt automatically by the `devcontainer-build` GitHub Actions workflow whenever you push a change to `.devcontainer/Dockerfile` or `.devcontainer/devcontainer.json` on `main`.

To trigger a manual rebuild without a code change, go to **Actions → Build and publish devcontainer image → Run workflow**.

Once the new image is pushed, the next Codespace created by any student will use it automatically.

## Useful resources

- [Tamarin Prover repository](https://github.com/tamarin-prover/tamarin-prover)
- [Tamarin Prover manual](https://tamarin-prover.com/manual/master/)
- [Tamarin VS Code extension source](https://github.com/tamarin-prover/vscode-tamarin)
- [GitHub Codespaces documentation](https://docs.github.com/en/codespaces)
