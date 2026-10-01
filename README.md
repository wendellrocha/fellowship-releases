<p align="center">
  <img src="logo.png" alt="Fellowship" width="128" height="128">
</p>

<h1 align="center">Fellowship</h1>

<p align="center">
  Run Claude Code, Codex, OpenCode and Cursor Agent side by side, as real terminals,<br>
  and let one of them lead the others.
</p>

<p align="center">
  <b>English</b> · <a href="README.pt-BR.md">Português</a>
</p>

---

## What it is

Fellowship is a desktop app for working with several coding agents at once. Each agent is the **real CLI** running in
its own terminal: you can read it, type into it, and it keeps its own login and its own updates. Fellowship adds what
those CLIs do not have between them: a place to run them together, a way to make one of them an **orchestrator** that
hands work to the others, and the same thing on a **remote machine over SSH**.

Nothing to install besides the agents you want to use. No tmux, no Node: the app brings its own runtime.

> The source code of Fellowship is private. This repository only publishes its installers.

## What it does

- **Real terminals, not a chat wrapper.** Claude Code, Codex, OpenCode and Cursor Agent run exactly as they do in
  your terminal. Fellowship never edits their configuration: its hooks and MCP server are added per session.
- **An orchestrator that delegates.** Any agent can coordinate: it can start workers, message them, wait for their
  results and follow their tasks. Workers report back with a structured result. You can still step in and type into
  any of them.
- **Workspaces.** One per project; each has its own agents, tasks and activity. Other workspaces show how many of
  their agents are waiting for you.
- **Saved teams.** An orchestrator plus its members (role, CLI, name, worktree, first message), started in one click.
- **Remote agents over SSH.** Add a host (`user@host`) and run agents on a VPS or a build machine. Fellowship installs
  its own small service there, in your home folder and without root, and reaches it through an SSH tunnel. Anything
  that changes the host (installing Node, updating the service) waits for your approval.
- **Plain terminals**, on this machine or on a host, for when you just need a shell.
- **Safe by default.** A message is never typed into an agent that is busy or waiting on a permission prompt: it
  queues until the agent is idle. Only you answer approval prompts.
- **Git worktrees.** A worker can get its own checkout and branch, so parallel workers cannot collide. Review what a
  worker produced and remove its checkout from the app.
- **Resume.** Bring an agent back after it exits, in the same folder, with the same name and place in the team.
- **Notifications.** A system notification (and the Dock or taskbar badge) when an agent needs you, and optionally
  when one finishes its work. An idle orchestrator is woken up when one of its workers reports back.
- **Launch settings per workspace.** Set a command, arguments and environment for each CLI, globally or only inside
  one workspace. That is how one machine uses two accounts of the same CLI, for example a different
  `CLAUDE_CONFIG_DIR` in each workspace.
- **Roles and your own CLIs.** Define roles (instructions, CLI, model, worktree) and register any terminal agent
  Fellowship does not know yet.
- **Updates.** The app checks once a day, verifies what it downloads, installs it and restarts.
- **English and Brazilian Portuguese.**

## Install

Download the file for your system from the [latest release](../../releases/latest).

### macOS (Apple silicon)

1. Download `Fellowship-<version>-mac-arm64.dmg`, open it and drag **Fellowship** to **Applications**.
2. The app is signed ad hoc, not with a paid Apple Developer ID, so macOS asks you to approve it the first time:
   - **macOS 15 and later:** open Fellowship once (it will be blocked), then go to **System Settings → Privacy &
     Security**, scroll down and choose **Open Anyway**.
   - **Older macOS:** right-click the app and choose **Open**.
   - Or, from a terminal: `xattr -dr com.apple.quarantine /Applications/Fellowship.app`

### Windows (x64)

1. Download `Fellowship-<version>-win-x64.exe` and run it.
2. The installer is not signed, so SmartScreen warns about it: choose **More info**, then **Run anyway**.

### Linux (x64)

Pick one:

- **AppImage** (updates itself):
  ```sh
  chmod +x Fellowship-<version>-linux-x86_64.AppImage
  ./Fellowship-<version>-linux-x86_64.AppImage
  ```
  If it does not start, your system may be missing FUSE 2 (`sudo apt install libfuse2` on Debian and Ubuntu).
- **Debian and Ubuntu package** (updated through a new download from this page):
  ```sh
  sudo apt install ./Fellowship-<version>-linux-amd64.deb
  ```

### First run

1. Install the agent CLIs you want to use, and sign in to each one as you normally do. Fellowship uses the `claude`,
   `codex`, `opencode` and `cursor-agent` it finds on your `PATH`; **Settings → Agent CLIs** lets you point to
   another command.
2. Open Fellowship, create a **workspace** (a name and a project folder), then **New agent**.
3. Pick a role. An `orchestrator` coordinates the others; any other role works on its own.

To run agents on another machine: **Settings → Hosts → Add a host**, then choose that host when you create the
workspace. The host needs SSH access with a key or an agent, and can be Linux or macOS, x64 or arm64.

## Updates

Fellowship looks for a new version when it starts and then once a day, and tells you when there is one. **Settings →
General → Updates** shows the version you run and has **Check for updates** and **Download and restart**. Every
installer is published with a `.sha256` file, and the app discards a download that does not match it. Your agents keep
running through an update; the app asks before it restarts the background service they live in.

On Linux with the `.deb`, the app only opens this page: install the new package as above.

## Status

Fellowship is young (versions 0.x) and changes quickly.

- Resume is verified with Claude Code; for Codex, OpenCode and Cursor Agent it depends on conversation ids that have
  not yet been tried against real conversations.
- Windows and Linux builds are published but have had much less use than macOS.
- Remote hosts have been used on Linux x64.
