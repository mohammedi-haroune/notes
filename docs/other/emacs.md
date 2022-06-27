## Tkharbich
### Gtags
- Spacemacs layer: https://github.com/syl20bnr/spacemacs/tree/master/layers/%2Btags/gtags

helm-gtags and ggtags are clients for GNU Global. GNU Global is a source code tagging system that allows querying symbol locations in source code, such as definitions or references. Adding the gtags layer enables both of these modes.

### Eldoc
- Website: https://www.emacswiki.org/emacs/ElDoc

A very simple but effective thing, eldoc-mode is a Minor Mode which shows you, in the echo area, the argument list of the function call you are currently writing. Very handy. By Noah Friedman. Part of Emacs.

## Useful commands
- `spacemacs/describe-variable`: show documentation of a variable, such as the layer's configurations variables

## FAQ
### 1.14 Should I place my settings in user-init or user-config?
Any variable that layer configuration code will read and act on must be set in user-init, and any variable that Spacemacs explicitly sets but you wish to override must be set in user-config.

Anything that isn't just setting a variable should 99% be in user-config.

Note that at time of writing files supplied as command line arguments to emacs will be read before user-config is executed. (Hence to yield consistent behaviour, mode hooks should be set in user-init.)

### 1.20 Why do I get files starting with .#?
These are lockfiles, created by Emacs to prevent editing conflicts which occur when the same file is edited simultaneously by two different programs. To disable this behaviour:
```emacs-lisp
(setq create-lockfiles nil)
```

### 1.23 Why am I getting a message about environment variables on startup?¶
Spacemacs uses the exec-path-from-shell package to set the executable path when Emacs starts up. This is done by launching a shell and reading the values of variables such as PATH and MANPATH from it. If your shell configuration sets the values of these variables inconsistently, this could be problematic. It is recommended to set such variables in shell configuration files that are sourced unconditionally, such as .profile, .bash_profile or .zshenv, as opposed to files that are sourced only for interactive shells, such as .bashrc or .zshrc. If you are willing to neglect this advice, you may disable the warning, e.g. from dotspacemacs/user-init:
```emacs-lisp
(setq exec-path-from-shell-check-startup-files nil)
```
You can also disable this feature entirely by adding exec-path-from-shell to the list dotspacemacs-excluded-packages if you prefer setting exec-path yourself.
