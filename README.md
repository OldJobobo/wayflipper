# Wayflipper

Wayflipper is a Bash utility to flip between Waybar themes on Omarchy/Hyprland setups. It updates Waybar by symlinking a theme’s `config.jsonc` and `style.css` into `~/.config/waybar`.

## Directory layout
- Active config: `~/.config/waybar/config.jsonc` (symlink)
- Active style: `~/.config/waybar/style.css` (symlink)
- Themes root (default): `~/.config/waybar/themes/<theme-name>/{config.jsonc,style.css}`
- Config file: `~/.wayflipper`

Create the themes directory if it does not exist:

```bash
mkdir -p ~/.config/waybar/themes
```

## Config file
Wayflipper creates `~/.wayflipper` on first run with a default themes directory:

```
themes_dir=~/.config/waybar/themes
```

You can edit this file to point at a different themes location. If you set
`WAYFLIPPER_THEMES_DIR`, it overrides the config file.

## Existing config handling
If `~/.config/waybar/config.jsonc` or `~/.config/waybar/style.css` are regular files,
Wayflipper moves them into a theme named `existing-backup` under `themes_dir` before
creating symlinks. This is a one-time safety move so you can switch back later.

## Preserve your current setup as a "default" theme
It’s a good idea to save your current Waybar configuration before experimenting:

```bash
mkdir -p ~/.config/waybar/themes/default
cp -p ~/.config/waybar/config.jsonc ~/.config/waybar/themes/default/config.jsonc
cp -p ~/.config/waybar/style.css ~/.config/waybar/themes/default/style.css
```

You can then switch back with `wayflipper default`.

## Installation
Place the `wayflipper` script somewhere on your `PATH` (or run it directly from this repo):

```bash
chmod +x wayflipper
# Optionally move it into a bin directory you own:
mv wayflipper ~/.local/bin/
```

## Usage
```
wayflipper <theme-name> [--dry-run]
wayflipper --list
wayflipper --version
wayflipper --help
wayflipper --browse [--dry-run] [--browse-once]
```

### Options
- `-l`, `--list` List themes found in `themes_dir` (default `~/.config/waybar/themes`)
- `-b`, `--browse` Interactive selector for themes (fzf stays open after applying; otherwise a numbered prompt)
- `--browse-once` Exit after applying a selection in browse mode
- `--dry-run` Show what would run without changing anything
- `-v`, `--version` Show the script version
- `-h`, `--help` Show usage

### Examples
- List available themes: `wayflipper --list`
- Preview a switch: `wayflipper V6.e --dry-run`
- Apply a theme: `wayflipper V6.e`
- Browse and apply interactively: `wayflipper --browse`
- Browse once (no UI reopen): `wayflipper --browse-once`

## Notes on reload/restart
- The script calls `omarchy-restart-waybar` after applying a theme. Ensure it is installed
  and available in `PATH`; otherwise the script exits with an error.
- In `--dry-run`, it only reports the command it would run.
