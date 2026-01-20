# Changelog

All notable changes to this project will be documented in this file.

## Unreleased

## 0.2.3
- Symlink theme subdirectories (e.g., assets/scripts) into Waybar and remove them on theme switch.
- Quit browse mode with Ctrl-Q so q is available for search.

## 0.2.2
- Keep fzf browse UI open while applying themes; Enter applies without closing.
- Add `--browse-once` for single-apply browse sessions.

## 0.2.0
- Keep fzf selection positioned on the last chosen theme when returning to browse mode.
- Use fzf result event with sync mode to restore cursor position after list load.
- Breaking: drop timestamped backups; apply themes via symlinks in `~/.config/waybar`.
- Move existing Waybar config/style files into a `existing-backup` theme once, then symlink.
- Add `~/.wayflipper` config file for `themes_dir` and short flags for core options.
