# ⚡ High-Productivity Zsh & Workspace Environment

A blazing-fast, production-grade terminal stack built for software engineering workflows. This setup prioritizes minimal runtime latency (<40ms), interactive fuzzy utilities, deep environment awareness, and a clean separation between personal and professional machine configurations.

---

## 🏗️ Architecture Overview

* **Framework Engine:** `Zimfw` (Static-compilation strategy for zero shell-startup overhead).
* **Prompt Engine:** `Starship` (Cross-shell Rust binary).
* **Fuzzy Layer:** `FZF` + `fzf-tab` (Interactive menu replacements for completions and history).
* **Navigation & Environment:** `Zoxide` + `Direnv` + `GHQ` (Automated path mapping, context variable loading, and repository tree sorting).
* **Workspace Multiplexer:** `Zellij` (Rust-powered terminal window tiling and layout engine).
* **Sync & Distribution:** `Chezmoi` (Version-controlled dotfiles state compiler).

---

## 📥 1. Global Homebrew Installation

Install the baseline command-line tools, core binaries, and developer fonts via Homebrew:

```bash
# Core Utilities, Multiplexer, and Sync Engine
brew install chezmoi starship fzf ripgrep fd bat eza lazygit zoxide direnv ghq zellij

# Developer Nerd Font (Required for directory icons and Starship glyphs)
brew install --cask font-jetbrains-mono-nerd-font

⚙️  Font Activation

After installing the font, open your terminal emulator (iTerm2, Terminal.app, or VS Code / Cursor Settings) and update the primary display font to:

Font Family: JetBrainsMono Nerd Font (or JetBrainsMonoNF)

Note: Enable Ligatures for optimal symbol rendering.

## 📄 3. Configuration Layouts

📦 File 1: ~/.zimrc (Plugin Manifest)
This file is used exclusively to list plugins. Replace its contents with this precise module alignment:

```bash
# =====================================================================
# ZIMFW CORE MODULES
# =====================================================================
zmodule environment
zmodule input
zmodule termtitle
zmodule utility

# =====================================================================
# HIGH-PRODUCTIVITY PLUGINS
# =====================================================================
zmodule git
zmodule fzf

# Completion Engine (Must run after core utilities)
zmodule zsh-users/zsh-completions --fpath src
zmodule completion

# Interactivity & Highlighting (Must initialize last)
zmodule Aloxaf/fzf-tab
zmodule zsh-users/zsh-syntax-highlighting
zmodule zsh-users/zsh-history-substring-search
zmodule zsh-users/zsh-autosuggestions
```

🔒 File 3: ~/.zshrc.local (Machine-Specific / Corporate Untracked)
This file handles tasks specific to one machine. It is created only on your work machine and is omitted from Chezmoi sync outputs. It isolates specialized enterprise binaries and access workflows

## 🔄 4. State Management & Machine Synchronization (Chezmoi)
To track configuration versions and sync them across personal and professional hardware, deploy the ecosystem utilizing chezmoi.

Initial Setup (On your main device)

```bash
# Initialize localized tracking folder
chezmoi init

# Register shared configurations to tracking array
chezmoi add ~/.zshrc
chezmoi add ~/.zimrc
chezmoi add ~/.config/starship.toml

# Push to a private GitHub configuration repository
chezmoi cd
git remote add origin git@github.com:YOUR_GITHUB_USERNAME/dotfiles.git
git branch -M main
git add .
git commit -m "Feat: Complete high-productivity Zsh environment setup"
git push -u origin main
exit
```

Deployment Strategy (On your second device)

```bash
# Pull down core specifications
chezmoi init [https://github.com/YOUR_GITHUB_USERNAME/dotfiles.git](https://github.com/YOUR_GITHUB_USERNAME/dotfiles.git)
chezmoi apply

# Force Zimfw to read the freshly synced plugin file and rebuild its cache
zimfw install

# Safe environment reload sequence
exec zsh
```

## 🛑 Standard Sync Hygiene
Never use source ~/.zshrc to refresh settings, as it causes configuration collisions. Use exec zsh.

Keep your ~/.zshrc.local out of the shared repo. It will stay hidden automatically unless you run chezmoi add ~/.zshrc.local.
