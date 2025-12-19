# Wayflipper

Wayflipper is a Bash utility to flip between Waybar themes on Omarchy/Hyprland setups. It swaps in a theme’s `config.jsonc` and `style.css`, makes timestamped backups of the active files, and keeps a record of the last switch.

## Directory layout
- Active config: `~/.config/waybar/config.jsonc`
- Active style: `~/.config/waybar/style.css`
- Themes root: `~/.config/waybar/themes/<theme-name>/{config.jsonc,style.css}`
- Last switch log: `~/.config/waybar/.last-theme-switch`

Create the themes directory if it does not exist:

```bash
mkdir -p ~/.config/waybar/themes
```

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
wayflipper <theme-name> [--dry-run] [--force]
wayflipper --list
wayflipper --version
wayflipper --help
```

### Options
- `--list` List themes found in `~/.config/waybar/themes`
- `--dry-run` Show what would be backed up and copied without changing anything
- `--force` Overwrite backups if a timestamped filename already exists for this run
- `--version` Show the script version
- `--help` Show usage

### Examples
- List available themes: `wayflipper --list`
- Preview a switch: `wayflipper V6.e --dry-run`
- Apply a theme: `wayflipper V6.e`
- Apply and force overwriting same-timestamp backups: `wayflipper V6.e --force`

## Backup behavior
- If the active files exist, they are copied to:
  - `config.jsonc.bak-YYYYmmdd-HHMMSS`
  - `style.css.bak-YYYYmmdd-HHMMSS`
  in `~/.config/waybar/`.
- Backups are never deleted automatically.
- A pointer file `~/.config/waybar/.last-theme-switch` records the theme name, timestamp, and backup filenames.
- If a backup with the same timestamped name exists and `--force` is not set, the run aborts safely.

## Notes on reload/restart
- The script calls `omarchy-restart-waybar` after applying a theme. Ensure it is installed/available in `PATH`; otherwise the script exits with an error.
- In `--dry-run`, it only reports the command it would run.
