# Installing Node.js for ConferenceHaven

**Required for:** Claude Desktop, ChatGPT Desktop, LM Studio  
**Not required for:** Copilot Studio (uses REST API)

ConferenceHaven's MCP server requires Node.js to run. This is a one-time, 5-minute install.

## Why Node.js?

Node.js is a popular JavaScript runtime used by over 20 million developers worldwide. Many AI tools (including Claude Desktop and ChatGPT) use it to run MCP servers.

**Don't worry!** You install it once, then forget about it—just like any other software.

---

## Installation Steps

### Windows

1. **Download Node.js**
   - Go to [nodejs.org](https://nodejs.org/)
   - Click the **LTS (Long Term Support)** button to download
   - Current LTS version: **20.x** or higher

2. **Run the Installer**
   - Double-click the downloaded `.msi` file
   - Click "Next" through the installer (default settings are fine)
   - Check the box: ☑ "Automatically install necessary tools"
   - Click "Install" and wait ~3 minutes

3. **Verify Installation**
   - Open **Command Prompt** (search for "cmd" in Start menu)
   - Type: `node --version`
   - You should see something like: `v20.11.0`

### macOS

1. **Download Node.js**
   - Go to [nodejs.org](https://nodejs.org/)
   - Click the **LTS (Long Term Support)** button to download
   - Current LTS version: **20.x** or higher

2. **Run the Installer**
   - Double-click the downloaded `.pkg` file
   - Click "Continue" through the installer (default settings are fine)
   - Enter your Mac password when prompted
   - Click "Install" and wait ~3 minutes

3. **Verify Installation**
   - Open **Terminal** (Cmd+Space, type "Terminal")
   - Type: `node --version`
   - You should see something like: `v20.11.0`

### Linux

**Ubuntu/Debian:**
```bash
# Install Node.js 20.x LTS
curl -fsSL https://deb.nodesource.com/setup_20.x | sudo -E bash -
sudo apt-get install -y nodejs

# Verify installation
node --version
```

**Fedora/RHEL/CentOS:**
```bash
# Install Node.js 20.x LTS
curl -fsSL https://rpm.nodesource.com/setup_20.x | sudo bash -
sudo dnf install -y nodejs

# Verify installation
node --version
```

---

## Troubleshooting

### "node: command not found"

**Windows:**
- Close and reopen Command Prompt
- If still not working, restart your computer

**macOS/Linux:**
- Close and reopen Terminal
- If still not working, check if Node.js is in your PATH:
  ```bash
  echo $PATH | grep node
  ```

### Installation Stuck or Slow

- **Windows:** Make sure Windows Update is not running
- **macOS:** Grant installer permission in System Preferences → Security
- **Linux:** Check your internet connection for package downloads

### Already Have Node.js But Old Version

If `node --version` shows version **< 18.0.0**, you should update:

- **Windows/macOS:** Download and run the latest installer from [nodejs.org](https://nodejs.org/)
- **Linux:** Use your package manager to upgrade:
  ```bash
  sudo apt-get update && sudo apt-get upgrade nodejs  # Ubuntu/Debian
  sudo dnf upgrade nodejs                              # Fedora/RHEL
  ```

---

## Next Steps

Once Node.js is installed, proceed to your AI client's setup guide:

- **[Claude Desktop Setup](CLAUDE-DESKTOP.md)**
- **[ChatGPT Desktop Setup](CHATGPT.md)**
- **[LM Studio Setup](LM-STUDIO.md)**

---

## Need Help?

- **Official Node.js Docs:** [nodejs.org/docs](https://nodejs.org/docs)
- **ConferenceHaven Issues:** [github.com/fabianwilliams/ConferenceHaven-Community/issues](https://github.com/fabianwilliams/ConferenceHaven-Community/issues)

---

**Quick Links:**
- [← Back to Setup Guides](../SETUP-GUIDES.md)
- [ConferenceHaven Documentation](../../README.md)
