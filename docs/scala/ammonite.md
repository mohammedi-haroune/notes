# Ammonite

## Install
```bash
sudo sh -c '(echo "#!/usr/bin/env sh" && curl -L https://github.com/lihaoyi/Ammonite/releases/download/1.6.4/2.12-1.6.4) > /usr/local/bin/amm && chmod +x /usr/local/bin/amm' && amm
```

## Script Files
Ammonite defines a format that allows you to load external scripts into the REPL; this can be used to save common functionality so it can be used at a later date. In the simplest case, a script file is simply a sequence of Scala statements, e.g.
