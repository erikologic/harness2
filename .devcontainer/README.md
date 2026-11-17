# Isolated OpenCode development container

Open this repository in a Dev Container to get Python 3.12, Node.js 22, `uv`,
and the OpenCode CLI. The container mounts this repository as its workspace; it
does not mount your home directory, OpenCode settings, SSH keys, or cloud
credentials. To use a host OpenRouter key, export `OPENROUTER_API_KEY` in the
environment from which you launch VS Code. The Dev Container forwards that
variable to remote processes; no key is written into this repository. If it is
unset, use `/connect` inside OpenCode instead.

The container and VS Code processes run as the non-root `vscode` user. On
Docker Desktop for macOS, setting `containerUser` also makes the workspace mount
writable to that user without changing permissions on the host.

The project OpenCode settings in [`.opencode/opencode.jsonc`](../.opencode/opencode.jsonc)
deny external-directory access, deny reads and edits of `.env` files, and ask
before shell commands. The shell permission is an approval gate, not an OS
security boundary: OpenCode's shell directory detection is best-effort. The
container is the stronger boundary, but the repository itself is mounted
read-write, so code and files in the repository remain accessible to the agent.
The forwarded API key is also available to processes started in the remote
environment, including approved shell commands; use a dedicated, low-limit key
and revoke it when no longer needed.

**Important:** this repository may have a local `.env` file. Although the OpenCode
file-read tool is denied access to it, shell commands run after approval could
still read it. For untrusted models, move secrets out of the workspace before
starting the container, or use a sanitized copy of the repository. The Dev
Container does not expose host files outside this project, but it is not a
network-egress sandbox.

OpenCode data and credentials configured inside the container persist in the
container's home directory, not on the host. Rebuilding or removing the
container may remove them.
