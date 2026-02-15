![zix](https://img.shields.io/badge/zix-gray?style=for-the-badge&logo=GNU%20Bash&logoColor=green) 
![License](https://img.shields.io/badge/License-GPL--3.0-orange?style=for-the-badge) 
![Stars](https://img.shields.io/gitea/stars/erffy/zix?gitea_url=https%3A%2F%2Fcodeberg.org&style=for-the-badge&color=DAA520)
![Issues](https://img.shields.io/github/issues/erffy/zix?style=for-the-badge)
![Last Commit](https://img.shields.io/gitea/last-commit/erffy/zix?gitea_url=https%3A%2F%2Fcodeberg.org&style=for-the-badge&color=F7A41D)
<br>
**A fast, lightweight, and powerful Zig version manager**

### Overview

Manage multiple Zig versions effortlessly with intelligent caching, parallel downloads, automatic version detection, and automatic date-based releases.

**Key Features:**
- **Smart Version Detection** - Auto-detects `.zigversion` files in project directories
- **Multiple Versions** - Install and switch between any Zig version instantly
- **ZLS Support** - Easily install and manage Zig Language Server
- **Shell Completions** - Tab completion for bash, zsh, and fish

### Support

If you find these projects useful and want to support my work, you can do so on [GitHub Sponsors](https://github.com/sponsors/erffy).<br>
Every bit of support helps me continue improving and maintaining these projects — thank you! ❤️

### Community

Join our Discord server for updates, support and discussion:<br>
[![Discord](https://img.shields.io/badge/discord-gray?style=for-the-badge&logo=discord)](https://discord.gg/tb8MvnnSfZ)

### Installation

```bash
curl -fsSL https://codeberg.org/erffy/zix/raw/branch/master/install.sh | bash
```

#### Custom Installation

```bash
# clone zix
git clone https://codeberg.org/erffy/zix.git

# cd into zix
cd zix

# Custom directories
ZIX_HOME=/opt/zix ZIX_BIN_DIR=/usr/local/bin ./install.sh

# Force specific downloader
ZIX_DOWNLOADER=aria2c ./install.sh
```

#### Manual Installation

```bash
# Create directories
mkdir -p ~/.local/zix/zig ~/.local/bin

# Download zix
curl -fsSL https://codeberg.org/erffy/zix/raw/branch/master/zix -o ~/.local/zix/zix
chmod +x ~/.local/zix/zix

# Create symlink
ln -sf ~/.local/zix/zix ~/.local/bin/zix

# Add to PATH (add to your shell config)
export PATH="$HOME/.local/bin:$PATH"
```

### Dependencies

> [!NOTE]
> `aria2` is recommended for downloads

- `bash` 4.0+
- `jq` - JSON parsing
- `tar` - Archive extraction
- `sha256sum` or `shasum` - Checksum verification
- One of: `curl`, `wget`, or `aria2c` - Downloads

<sub>* Core dependencies: `jq`, `tar`, `sha256sum` (typically pre-installed)</sub>

## Quick Start

```bash
# Verify installation
zix doctor

# Install latest stable version
zix install 0.15.2

# Install latest development version
zix install master

# Install with ZLS (Zig Language Server)
zix install 0.15.2 --with-zls

# Switch to a version
zix use 0.15.2

# Verify it's working
zig version
```

### Usage

#### Basic Commands

```bash
# Install a specific version
zix install 0.15.2
zix install 0.14.0
zix install 0.15.2 --with-zls   # Install with ZLS
zix install master -z           # Development version with ZLS

# List installed versions
zix list

# List all available versions
zix list-remote

# Use a specific version
zix use 0.15.2

# Show current version
zix current

# Remove a version
zix remove 0.14.0
```

#### ZLS Management

```bash
# Install ZLS for an existing version
zix zls install 0.15.2

# Remove ZLS from a version
zix zls remove 0.15.2

# When you switch versions, zix automatically links the correct ZLS
zix use 0.15.2
```

#### Project Version Management

Create a `.zigversion` file in your project:

```bash
echo "0.15.2" > .zigversion
```

Then use auto-detection:

```bash
zix auto
```

ZIX will search up the directory tree to find the version file, automatically install if needed, and activate it.

**Supported version files:**
- `.zig-version`
- `.zigversion`
- `.zv`

#### Advanced Usage

```bash
# Check installation and configuration
zix doctor

# Show environment setup
zix env

# Clean download cache
zix clean

# Install shell completions
zix completion-install

# Update zix itself
zix update
```

### Configuration

#### Environment Variables

| Variable | Description | Default |
|----------|-------------|---------|
| `ZIX_HOME` | Zix root directory | `~/.local/zix` |
| `ZIG_HOME` | Zig data directory | `$ZIX_HOME/zig` |
| `ZIX_BIN_DIR` | Binary directory | `~/.local/bin` |
| `ZIX_DOWNLOADER` | Force downloader: `aria2c`, `curl`, or `wget` | Auto-detect |
| `ZIX_UPDATE_CHECK` | Enable automatic daily update checks | `1` (enabled) |
| `ZIG_DOWNLOADS_DIR` | Directory for downloaded archives | `$ZIG_HOME/downloads` |

#### Examples

```bash
# Use custom root directory
export ZIX_HOME="/opt/zix"
zix install 0.15.2

# Force curl for downloads (useful in restricted environments)
ZIX_DOWNLOADER=curl zix install master

# Use custom binary location
export ZIX_BIN_DIR="/usr/local/bin"
```

### Shell Integration

#### Automatic PATH Setup

The installer automatically configures your shell. To manually add:

**Bash/Zsh** (`~/.bashrc` or `~/.zshrc`):
```bash
export PATH="$HOME/.local/bin:$PATH"
```

**Fish** (`~/.config/fish/config.fish`):
```fish
fish_add_path "$HOME/.local/bin"
```

#### Shell Completions

```bash
# Install completions for your shell
zix completion-install

# Restart your shell or reload config
source ~/.bashrc  # or ~/.zshrc or ~/.config/fish/config.fish
```

### Troubleshooting

#### Installation Issues

```bash
# Check what's wrong
zix doctor

# Check installation log (during install)
cat /tmp/zix-install-*.log

# Verify dependencies
command -v jq tar sha256sum curl
```

#### Common Issues

**Problem:** `zix: command not found`
```bash
# Ensure ~/.local/bin is in PATH
echo $PATH | grep -q "$HOME/.local/bin" || echo 'export PATH="$HOME/.local/bin:$PATH"' >> ~/.bashrc
source ~/.bashrc
```

**Problem:** Download fails
```bash
# Try different downloader
ZIX_DOWNLOADER=wget zix install 0.15.2

# Or clear cache and retry
zix clean
zix install 0.15.2
```

**Problem:** Version not found
```bash
# List available versions
zix list-remote

# Install specific version
zix install 0.15.2
```

**Problem:** Permission denied
```bash
# Don't run as root - zix is per-user
# Ensure home directory is writable
ls -ld ~
```

### Uninstallation

```bash
# Using the installer
bash install.sh --uninstall

# To also remove all installed Zig versions
rm -rf ~/.local/zix
```

### Contributing

Contributions are welcome! Please feel free to submit issues or pull requests.

### Acknowledgments

- Built for the [Zig](https://ziglang.org) community
- Inspired by [nvm](https://github.com/nvm-sh/nvm) and other version managers
- Powered by [aria2](https://aria2.github.io) for blazing fast downloads

---

<div align="center">

**Made with ❤️ by Me**

*Star ⭐ this repo if you find it useful!*<br>
*Support the project on [GitHub Sponsors](https://github.com/sponsors/erffy)* 💖<br>
*Join our [Discord](https://discord.gg/tb8MvnnSfZ) for support and updates!*

This project is licensed under the **GNU General Public License v3.0**. See [LICENSE](https://codeberg.org/erffy/zix/src/branch/master/LICENSE) for details.

</div>
