# Plan: Next Version (No Backups, Direct Theme Load, Configurable Themes Dir)

## Goals
- Stop copying/moving Waybar files and remove backup logic entirely.
- Apply themes by symlinking `config.jsonc` and `style.css` into `~/.config/waybar`.
- Add a basic config file to allow users to customize the themes directory path.
- Add single-letter short flags for all options except `--dry-run`.
- Keep usage/help/README in sync with new behavior.

## Assumptions To Confirm
- Missing `config.jsonc` or `style.css` should produce a clear error and abort.
- `omarchy-restart-waybar` is available to reload Waybar after swapping symlinks.

## Proposed Config File
- Path: `~/.wayflipper`
- Format (bash-friendly key/value):
  - `themes_dir=~/.config/waybar/themes`
- Behavior:
  - Create the file with defaults on first run if missing.
  - Allow environment override (e.g., `WAYFLIPPER_THEMES_DIR`) if needed.

## Execution Phases
### Phase 1: Validate Restart Path
- Confirm `omarchy-restart-waybar` is available and sufficient for symlink-based reloads.

### Phase 2: Core Script Refactor
- Remove backup logic (`backup_files`, `count_backup_files`, pointer file).
- Load config file at `~/.wayflipper`; set themes dir from config with a default fallback.
- Replace file-copying with symlink updates for config/style.
- If active files are regular files, move them into a `existing-backup` theme once.
- Update CLI options (add short flags for all remaining options except `--dry-run`).
- Keep `usage()` output in sync.

### Phase 3: Documentation Updates
- Update `README.md` for new behavior, config file, and updated options.
- Update `CHANGELOG.md` with a breaking-change note.
- Document the new short flags.

### Phase 4: Manual Verification
- `shellcheck wayflipper`
- `./wayflipper --list` with custom themes dir
- `./wayflipper <theme> --dry-run` shows the correct symlink operations
- Real run against a disposable theme and verify Waybar reloads

## Additional Dev Checks (Optional)
- Verify `.wayflipper` auto-creation with defaults (simulate in `--dry-run`).
- Confirm `--browse` works with and without `fzf`.
- Confirm nonexistent theme or missing files yields a clear error and nonzero exit.

## Open Questions
- Should `--dry-run` show config file creation and any other filesystem changes?
