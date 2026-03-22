# Editor Configuration to Prevent Backup Files

## Vim
Add to ~/.vimrc or ~/.config/nvim/init.vim:
```vim
set nobackup
set nowritebackup
set noswapfile
```

## Emacs
Add to ~/.emacs or ~/.emacs.d/init.el:
```elisp
(setq make-backup-files nil)
(setq auto-save-default nil)
```

## Nano
Add to ~/.nanorc:
```nano
set backup false
```

## VS Code
Add to settings.json:
```json
{
  "files.backup": false
}
```

## Notes

- The bootstrap script automatically sets AUTOMAKE_BACKUPS=0 to prevent autotools from creating backup files
- It also cleans up any ~ files that might be created
- Configure your editor to prevent backup files from manual edits
