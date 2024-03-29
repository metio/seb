---
title: Backups with emacs
date: 2021-10-25
categories:
- snippet
tags:
- emacs
- backup
---

[emacs](https://www.gnu.org/software/emacs/) will create backups of your files by default. Those backups are located right next to the original file and are called `<file>~`. Unfortunately, emacs will not clean those up by default, which annoys me from time to time. Therefore, I'm now using the following configuration to keep those backups in a different folder:

```el
(setq version-control t     ;; Use version numbers for backups.
      kept-new-versions 10  ;; Number of newest versions to keep.
      kept-old-versions 0   ;; Number of oldest versions to keep.
      delete-old-versions t ;; Don't ask to delete excess backup versions.
      backup-by-copying t)  ;; Copy all files, don't rename them.

(setq vc-make-backup-files t)

;; Default and per-save backups go here:
(setq backup-directory-alist '(("" . "~/.emacs.d/backup/per-save")))

(defun force-backup-of-buffer ()
  ;; Make a special "per session" backup at the first save of each
  ;; emacs session.
  (when (not buffer-backed-up)
    ;; Override the default parameters for per-session backups.
    (let ((backup-directory-alist '(("" . "~/.emacs.d/backup/per-session")))
          (kept-new-versions 3))
      (backup-buffer)))
  ;; Make a "per save" backup on each save.  The first save results in
  ;; both a per-session and a per-save backup, to keep the numbering
  ;; of per-save backups consistent.
  (let ((buffer-backed-up nil))
    (backup-buffer)))

(add-hook 'before-save-hook  'force-backup-of-buffer)
```

Thanks to that configuration, backups per-save will be created in `~/.emacs.d/backup/per-save` and backups per-session in `~/.emacs.d/backup/per-session`.

