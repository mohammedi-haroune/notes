# Split mkv video
mkvtoolnix: https://mkvtoolnix.download/downloads.html#ubuntu

1. PGP key
```
sudo wget -O /usr/share/keyrings/gpg-pub-moritzbunkus.gpg https://mkvtoolnix.download/gpg-pub-moritzbunkus.gpg
```

2. Add apt repo
```
❯ cat /etc/apt/sources.list.d/mkvtoolnix.list
deb [arch=amd64 signed-by=/usr/share/keyrings/gpg-pub-moritzbunkus.gpg] https://mkvtoolnix.download/ubuntu/ impish main
```

3. Install
```
sudo apt update
sudo apt install mkvtoolnix mkvtoolnix-gui
```

4. Use
```
mkvmerge --split parts:00:02:13-00:03:00 input.mkv -o output.mkv
```

# Analyze disk usage
```bash
ncdu -1xo- / | gzip > /tmp/ncdu_export.gz
# ...some time later:
zcat /tmp/ncdu_export.gz | ncdu -f-
```

# Convert a text file to an image
```bash
convert -size 1920x1080 xc:black -font /home/mohammedi/.fonts/SourceCodePro-Medium.otf -pointsize 30 -fill white -annotate +15+20 "@ascii.txt" image.png
```

Default security policy in `/etc/ImageMagick-6/policy.xml` prevents writing a text file to image, you have to disable this behavior by commenting this line:
```xml
<policy domain="path" rights="none" pattern="@*"/>
```

# Convert an HTML to an image
```bash
sudo apt-get install texlive-latex-base texlive-fonts-recommended texlive-fonts-extra texlive-latex-extra
```

```bash
pandoc tasks.html -o tasks.pdf
```

```bash
convert tasks.pdf tasks.png
```

# Set a background (doesn't work for ubuntu/gnome)
```bash
sudo apt install feh
feh --bg-scale image.png
```

```bash
gsettings set org.gnome.desktop.background picture-uri "file:////tmp/tasks.png"
```


# I3

## Rofi
```bash
sudo apt install rofi
```

## polybar
```bash
sudo apt install polybar
sudo apt install mpd
sudo apt install bspwm
sudo apt install xbacklight
```

## Natural scrolling
https://askubuntu.com/questions/1122513/how-to-add-natural-inverted-mouse-scrolling-in-i3-window-manager

Add `Option "NaturalScrolling" "True"` to `/usr/share/X11/xorg.conf.d/40-libinput.conf`

For mouse look for "pointer catch all", for touchpad look for "touchpad catch all"

## Autolock
answer: https://faq.i3wm.org/question/83/how-to-run-i3lock-after-computer-inactivity.1.html

```bash
sudo apt install xautolock
```

`cat ~/bin/fuzzy_lock.sh`:

```bash
#!/bin/sh -e

# Take a screenshot
scrot /tmp/screen_locked.png

# Pixellate it 10x
mogrify -scale 10% -scale 1000% /tmp/screen_locked.png

# Lock screen displaying this image.
i3lock -i /tmp/screen_locked.png

# Turn the screen off after a delay.
# What about if I unlock the screen before this 60 seconds completes ?
# sleep 60; pgrep i3lock && xset dpms force off
```

# KDE on Ubuntu
https://askubuntu.com/questions/135267/whats-the-difference-between-kde-packages

### SDDM insted of GDM
```bash
sudo apt install sddm
```

- Get rid of the virtual keyboard that takes most of the screen when typing password
```
sudo cat /etc/sddm.conf
[General]
InputMethod=
```

### Keyboard shortcuts
- The PATH is not yet loaded, so we can't for example assume the scripts in `~/bin` are accessible by their names, we have to use the full path, `~` is also acceptable and will be, as expected, replaced by `$HOME`
- I couldn't set a keyboard shortcut to a command with parameter like "command arg"
- If we setup a keyboard shortcut to a bash script that has no `#!/bin/bash` this error will pop up when we click that shortcut "execvp: Exec format error"

### Snapd applications are not present in the menu
Which also makes applications that uses "xdg-open" (maybe alo kio-client) to fail to open their links from the browser like when trying to log in to a Slack workspace
```
sudo ln -s /var/lib/snapd/desktop/applications/* /usr/share/applications/
```

## Watch Don't starve folder and commit changes
```
#!/bin/bash
# run inotify command
echo "Start watching for close_write,create,delete and move events with inotify ..."
# we use close_write instead of modify because modify could be emited many times, it depends on how much write() was called
# whereas the close_write is only emited once when the file being edited (opened in write mode) is closed
# see: https://stackoverflow.com/a/32424150/7573221
inotifywait -r -q -m -e close_write -e delete -e move --exclude='.git/*' --format="echo 'Committing: %e on %w%f' && git add . && git commit -m'auto commit: %e on %w%f' && sleep 1 && git push" . | zsh
echo "Stopped watching ..."
```

```
[program:auto-push-dont-starve]
environment=USER=mohammedi,HOME=/home/mohammedi
user=mohammedi
autostart=True
directory=/home/mohammedi/.steam/steam/userdata/1072787539/219740
command=zsh -c "sudo -u mohammedi /home/mohammedi/.steam/steam/userdata/1072787539/219740/watch.sh"
stderr_logfile=/var/log/auto-push-dont-starve/error.log
stdout_logfile=/var/log/auto-push-dont-starve/out.log
```
